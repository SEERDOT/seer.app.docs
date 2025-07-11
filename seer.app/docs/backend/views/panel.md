# Documentação da View panel

A view panel é responsável por renderizar o painel principal da aplicação, seja ele financeiro ou de vendas, com base no parâmetro `sector`. Ela opera em um contexto multi-tenant, utilizando o subdomínio da requisição para identificar o `Client` correspondente. O painel apresenta diversos indicadores (KPIs), gráficos e dashboards interativos baseados em dados reais ou simulados.

---

## Autenticação e Tenant

- Requisição protegida por `@login_required`
- Subdomínio extraído de `request.get_host()` é utilizado para localizar o `Client` (tenant).
- Uso extensivo de `schema_context()` para garantir o isolamento de dados por tenant.

---

## Templates Renderizados

- `client_app/app/financeiro_panel.html`
- `client_app/app/vendas_panel.html`

O template é escolhido com base no valor de `sector` ("financeiro" ou "vendas").

---

## KPIs e Métricas

**Mapeamentos**

- `METRIC_MAP`: mapeia KPIs financeiros (ex: `lucro_bruto`, `fluxo_caixa_livre`, etc.)
- `SALES_METRIC_MAP`: mapeia KPIs de vendas (ex: `ticket_medio`, `ciclo_vendas`, etc.)

Todos são combinados em `ALL_METRICS`.

**Cálculo de KPIs**

- KPIs são buscados via modelo `Snapshot`, ordenados pelos dois últimos valores (período atual e anterior).
- Alguns KPIs (ex: taxa de sucesso, churn, conversão) possuem funções específicas com lógicas customizadas baseadas em `Deal`.
- Valores simulados são usados quando dados reais estão indisponíveis.

**Formatação**

- KPIs são formatados como valores monetários, percentuais ou dias.
- Mudanças são apresentadas em comparação ao período anterior com sinal visual (setas e cores CSS).

---

## Dados Gerados para Gráficos

**Financeiro**

- Faturamento Semanal (`income.js`)
- Lucro Bruto x Líquido Mensal (`liquid_gross_profit.js`)
- Despesas por Categoria (`expenses.js`)
- Histórico de KPIs Financeiros

---

**Vendas**

- Funil de Vendas Mensal (`funnel.js`) com dados do modelo `Deal`
- Vendas por Canal (`sellsPerChannel.js`), agregando valores por `lead_source`
- Desempenho por Vendedor (`sellers.js`)
- Eficiência dos Canais (`efficiency.js`)

---

## Considerações Finais

A view panel é um componente central de visualização estratégica da plataforma. Sua capacidade de consolidar e contextualizar múltiplas métricas em uma interface responsiva permite aos clientes obter insights imediatos sobre sua performance financeira ou comercial. A arquitetura multi-tenant garante segurança e isolamento de dados, enquanto a combinação de dados reais com simulações assegura uma experiência rica mesmo em ambientes com dados ainda escassos.