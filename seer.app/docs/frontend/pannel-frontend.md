
# Documentação Página *Painel* 

> **Contexto**: a página *Painel* oferece uma visão consolidada dos setores **Financeiro** e **Vendas** da empresa. São exibidos KPIs, gráficos interativos e filtros de período, além de exportação para PDF.

---

## 1. Visão Geral

- **Templates**: `financeiro_panel.html`, `vendas_panel.html`
- **Estilos**: `panel.css` (global), `panelVendas.css` (vendas)
- **Bibliotecas**: Highcharts, **jsPDF 2.5.1**, **html2canvas 1.4.1**, Material Icons Outlined
- **Objetivo UX**: leitura rápida de métricas, comparação temporal e compartilhamento de relatórios.

---



## 2. Anatomia do DOM

```text
.panel-container
 ├─ .breadcrumb
 ├─ .title-select-row
 │   └─ .global-filter-container
 ├─ .sectors-nav
 ├─ .kpi-row (x2)
 ├─ .graph-container           (linha/área)
 ├─ .blank-containers
 │   ├─ .blank-container (bar)
 │   └─ .blank-container (pie/bar)
 └─ #tooltips-container
```

Principais IDs para scripts/estilos:

| ID | Descrição |
|----|-----------|
| `global-filter-btn` | Abre dropdown de filtros globais |
| `global-filter-dropdown` | Container das `.filter-option` |
| `profit-chart` | Gráfico de faturamento |
| `bar-chart` | Lucro bruto × líquido |
| `bar-chart-right` | Despesas por categoria |

---

## 3. Componentes

Cada componente abaixo cumpre um papel específico de **usabilidade** dentro do sistema.

### 3.1 Breadcrumb

**Função:** Orientar o usuário, mostrando em que parte da plataforma ele se encontra e permitindo voltar facilmente a níveis anteriores.

Exibe caminho da aplicação; ícone inicial verde (`#23CE6B`).

### 3.2 Navegação de Setores

**Função:** Permitir alternar rapidamente entre as visões Financeiro e Vendas sem recarregar toda a aplicação.

Botões `.sector-item` redirecionam para `/panel/financeiro` ou `/panel/vendas`.  
Classe `.selected` colore o botão ativo (fundo `#CDF8E8`, texto `#188C38`).

#### Como isso é feito?

1. **URL pattern** — `/client/panel/<sector>` mapeia para a _view_ `panel(sector)`.

2. A view carrega o contexto adequado (KPIs, datasets) e decide qual _partial_ incluir:

   * `financeiro_panel.html` ou `vendas_panel.html`.

3. Ambos os parciais **estendem o mesmo `base.html`** e compartilham o CSS global, portanto apenas a seção interna muda; os assets não são baixados de novo graças ao cache do navegador.

4. Resultado: há, tecnicamente, um _full-page reload_, mas com latência mínima e sem “flash” de layout, o que faz a navegação parecer instantânea.


### 3.3 Filtros Globais

**Função:** Restringir o período de análise de **todos** os KPIs e gráficos simultaneamente, garantindo leituras consistentes.

Dropdown controlado por `global-filter-btn`. Cada `<li class="filter-option">` possui `data-months` (1, 3, 6, 12).  
Evento `click` dispara atualização de KPIs e gráficos.

```html
<li class="filter-option" data-months="6">Últimos 6 meses</li>
```

### 3.4 Indicadores (KPIs)

**Função:** Apresentar métricas‑chave em formato reduzido, permitindo uma varredura rápida de performance sem abrir relatórios completos.

Estrutura típica:

```html
<div class="kpi-container">
  <div class="kpi-header">
    <span>Faturamento Total</span>
    <i class="material-icons-outlined help-icon">help_outline</i>
  </div>
  <div class="kpi-value">R$ 2.300.000</div>
  <div class="kpi-change up">
    <i class="material-icons-outlined">trending_up</i><span>+5%</span>
    <span class="compareText">vs. mês passado</span>
  </div>
</div>
```

* Classes `.up` / `.down` definem cor verde ou vermelha (panel.css).
* Tooltip agregado ao ícone `help-icon` (ver § 4.6).

### 3.5 Gráficos

**Função:** Visualizar tendências e comparações ao longo do tempo ou entre categorias, transformando números em insights visuais.

Cada gráfico fica dentro de `.graph-container` ou `.blank-container` com header:

```html
<div class="graph-header">
  <h2>Receita Mensal</h2>
  <div class="graph-controls">
    <select class="period-select">Mensal</select>
    <button class="share-btn"><i class="material-icons-outlined">share</i></button>
  </div>
</div>
<div id="profit-chart"></div>
```

Scripts Highcharts leem dados de `window.profitChartData` etc.

### 3.6 Tooltips KPI

**Função:** Fornecer explicações rápidas sobre o cálculo ou significado dos KPIs, sem desviar o usuário para outra página.

Tooltips estão em `#tooltips-container .tooltip`. JS vincula cada `.help-icon` ao tooltip correspondente:
```css
.tooltip { opacity:0; transition:opacity .2s; }
.tooltip.visible { opacity:1; }
```

---

## 4. Fluxo de Interações

| Interação | Elemento | Lógica |
|-----------|----------|--------|
| **Filtro global** | `.filter-option` | Atualiza `currentFilter`; fetch no backend; re-render KPIs/gráficos |
| **Filtro local** | `.period-select` ou `.time-option` | Atualiza apenas o gráfico dentro do contêiner |
| **Exportar PDF** | `.share-btn` | `generateChartPDF(id,title)` → html2canvas → jsPDF.save |
| **Tooltip** | `.help-icon` mouseenter | Adiciona classe `.visible` ao tooltip correspondente |

Trecho da função de exportação:

```js
function generateChartPDF(containerId, title){
  html2canvas(document.getElementById(containerId)).then(canvas=>{
    const pdf = new jsPDF('p','mm','a4');
    pdf.addImage(canvas,'PNG',10,10,190,90);
    pdf.save(`${title}-${new Date().toISOString().slice(0,10)}.pdf`);
  });
}
```

---

## 5. Estilos Relevantes

`panel.css` (GLOBAL):
```css
.kpi-container     { background:#fff;border-radius:8px;padding:20px; }
.kpi-change.up     { color:#23CE6B; }
.kpi-change.down   { color:#B3261E; }
.sector-item       { cursor:pointer;padding:8px 16px;border-radius:5px; }
.sector-item:hover { background:#CDF8E8; }
.share-btn:hover   { background:#CDF8E8;color:#188C48; }
```

`panelVendas.css` (Vendas):
```css
.sector-item.vendas.selected { background:#A8C9FF;color:#2384CE; }
```

---

## 6. Extensibilidade

* **Novos KPIs**: adicionar entrada no dicionário `kpis` (context) e clonar `.kpi-container`.  
* **Novos gráficos**: criar `<div id="new-chart"></div>` + script Highcharts + botão share.  
* **Novos filtros**: inserir novo `<li class="filter-option">` e tratar no backend.

---

## 7. Dependências

| Biblioteca | Finalidade |
|------------|-----------|
| **Highcharts** | Renderização de linhas, barras, pizza |
| **jsPDF** 2.5.1 | Criação de PDF |
| **html2canvas** 1.4.1 | Screenshot DOM |
| **Material Icons** | Ícones de interface |

---

> **Última atualização:** 06 jul 2025
