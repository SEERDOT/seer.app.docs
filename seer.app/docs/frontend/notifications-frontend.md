# Página **Notificações** – Documentação Front-End  


## 1. Visão geral

A página **Notificações** centraliza avisos de sistema, novidades de produto e alertas importantes para o usuário.  
Ela oferece dois estados principais:

1. **Sem notificações** – interface de _empty state_ com ícone 🛎️.  
2. **Com notificações** – lista cronológica com linha do tempo lateral, checkbox de leitura e filtros.

---

## 2. Estrutura de alto nível

| Seção | Propósito | Arquivo (fonte) |
|-------|-----------|-----------------|
| **Top Bar** | Caminho (breadcrumb), título “Notificações” e botão **“Marcar tudo como lido”**. | `notifications.css` § 1 Top Bar Content :contentReference[oaicite:0]{index=0} |
| **Tabs “Novidades / Alertas”** | Filtro visual para separar _release notes_ de avisos críticos. | `.novidades`, `.alertas` :contentReference[oaicite:1]{index=1} |
| **Lista de notificações** | Renderiza cada item com checkbox, texto e _timeline_ de datas. | `.single-notification`, `.info-box-*` :contentReference[oaicite:2]{index=2} |
| **Linha do tempo** | Coluna direita com círculos verde / cinza e data relativa. | `.timeline-circle-*`, `.date-text` :contentReference[oaicite:3]{index=3} |

> **Responsividade**  
> A UI usa unidades **`vh` / `vw`** para tipografia e espaçamento, garantindo adaptação a diferentes resoluções.

---

## 3. Componentes e funcionalidades

### 3.1 Breadcrumb & Título

* **Classes:** `.path`, `.green-path`, `.grey-small`, `.topico`.
* Exibe link de retorno _“Início / Notificações”_.  
* **Boas práticas:** usa contraste de cores — texto ativo `#188C48`, texto passivo `#272D2D`.

### 3.2 Botão **Marcar tudo como lido**

| Estado | Classe | Estilo / feedback |
|--------|--------|-------------------|
| Ativo | `.read-button-notification` | Borda `#969696`; _hover_ deixa fundo `#D6F5E5` e borda/ícone `#188C48`. |
| Desabilitado (sem itens) | `.read-button-empty` | Fundo cinza `#C9C9C9`, cursor `default`. |

### 3.3 Tabs de categoria

Botões **Novidades** e **Alertas** reutilizam componentes de tab:

```css
.active-btn-left  { background:#D6F5E5; color:#188C48; }
.non-active-btn-right { background:#D9D9D9; color:#272D2D; }
```

---

## 4. Interações JS Previstas

| Ação | Seletor | Resultado |
|------|---------|-----------|
| Marcar tudo | `.read-button-notification` | Marca checkboxes + POST `/notifications/read-all/` |
| Marcar um | `.info-checkbox input` | POST `/notifications/read/<id>` |
| Trocar tab | `.tab` | Filtra lista (`data-category`) |
| Hover botões | CSS `:hover` | Fundo `#D6F5E5`, ícone verde |

```javascript
document.querySelectorAll('.tab').forEach(tab => {
  tab.onclick = e => {
    const cat = e.target.dataset.category;
    document.querySelectorAll('.single-notification')
      .forEach(card => card.style.display =
        card.dataset.category === cat ? 'flex' : 'none');
  };
});
```

---

## 5. CSS‑chave (trechos)

```css
/* Tabs */
.active-btn-left  { background:#D6F5E5; color:#188C48; }
.non-active-btn-right { background:#D9D9D9; color:#272D2D; }

/* Card base */
.single-notification { display:flex; justify-content:space-between; padding:1.6vh 1vw; }
.info-box-unviewed   { background:#fff; }
.info-box-viewed     { background:#EDEDED; }

/* Timeline */
.timeline-circle-green { background:#23CE6B; }
.timeline-circle-grey  { background:#969696; }
```

---


> **Última atualização:** 06 jul 2025

