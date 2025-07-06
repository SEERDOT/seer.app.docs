# Configurações e Preferências - Documentação Frontend

## Tabela de Conteúdo

1. [Propósito & Escopo](#1-propósito--escopo)
2. [Componentes Visuais](#2-componentes-visuais)
3. [Fluxo de Interação](#3-fluxo-de-interação)
4. [Validações e Regras](#4-validações-e-regras)
5. [Endpoints Django](#5-endpoints-django)
6. [Estilos CSS Principais](#6-estilos-css-principais)
7. [Exemplos de Código](#7-exemplos-de-código)

---

## 1. Propósito & Escopo

**Settings Modal** é um *overlay* leve que concentra atalhos de configuração da conta. Atualmente, apenas **Preferências da Organização** está habilitada; **Preferências Pessoais** e **Relatar Problema** aparecem bloqueadas para indicar futuras funcionalidades.

A opção **Sair** executa `document.getElementById('logout-form').submit()` e encerra a sessão.

Ao selecionar *Preferências da Organização*, o usuário é redirecionado para uma página full‑screen composta por **Sidebar** e **Container de Conteúdo**. A página oferece três abas principais — **Geral**, **Planos & Pagamentos** e **Privacidade** — implementadas como rotas distintas em Django.

---

## 2. Componentes Visuais

### 2.1 Settings Modal

| Elemento                | Classe / ID                     | Estado                  | Papel                                                               |
| ----------------------- | ------------------------------- | ----------------------- | ------------------------------------------------------------------- |
| Modal                   | `#settingsModal.settings-modal` | `.active` quando aberto | Container principal                    |
| Opção Preferências Org. | `.settings-option` (1ª)         | Ativa                   | Linka para `/client/preferencias/geral` |
| Opções futuras          | `.settings-option.locked`       | Inativa                 | Indicadores de roadmap               |
| Sair                    | `.settings-option`              | Ativa                   | Submete `logout-form`                  |

### 2.2 Página de Preferências
- **Sidebar** (`.preferences-sidebar`) com itens list (`li`) que recebem `.active` conforme rota. - **Container** (`.preferences-container`) responsável por renderizar cada aba. 
- **Menu local** nas abas **Planos** e **Privacidade** usa `.menu .item` (hover/selected). 

### 2.3 Aba **Geral**
| Elemento     | Classe / ID           | Papel/Conteúdo |
| ------------ | --------------------- | -------------- |
| Painel Geral | `.org-info`           | Caixa branca com 2 colunas — **label** (à esquerda) e **content** (à direita). Lista: Organização, ID, URL, Plano.  |
| Botão Ver Planos | `.label .button`   | Redireciona para rota **Planos & Pagamentos** (`/client/preferencias/planos-e-pagamentos`).  |

### 2.4 Aba **Privacidade**
| Elemento | Classe / ID | Papel |
| -------- | ----------- | ----- |
| Título   | `.title` | Texto “Privacidade” + data de atualização.  |
| Menu     | `.menu .item` | Alterna entre **Política de Privacidade** (`#privacy-policy`) e **Termos de Uso** (`#terms-of-use`). Cada item recebe `.active` quando selecionado.  |
| Seções   | `#privacy-policy`, `#terms-of-use` | Conteúdo extenso com `<h2>`/`<h3>` para LGPD, direitos do usuário, etc.  |

---

## 3. Fluxo de Interação

1. **Usuário clica** no ícone de engrenagem → `#settingsModal` aparece com animação *fade‑in* (`opacity 0.3s`). 
2. Seleção de **Preferências da Organização** redireciona para `/client/preferencias/geral` (rota nomeada `client_general_preferences`).
3. Dentro da página:
   - **Sidebar** mantém navegação persistente; clique em **Planos e Pagamentos** ou **Privacidade** carrega nova rota, preservando layout.
   - Abas internas (ex.: `btnPlans` ↔ `btnPayments`) alternam blocos `#plansContainer` e `#paymentsContainer` via JS simples. 4. O botão **Sair** (no modal) fecha a sessão e redireciona para `/login`.

---

## 4. Validações e Regras

### 4.1 Planos & Pagamentos

- **Comutador de Compromisso** (`.plan-switch-item`) troca estados `.active-left` / `.active-right`. Somente um lado pode ficar ativo; a troca ajusta preço e selo **MELHOR VALOR** dinamicamente. 
- Botão **Comprar agora** (`.plan-button.buy-now`) fica cinza quando plano é *Plano Atual*; torna‑se verde (`#23ce6b`) e clicável quando plano disponível. 
- **Popover** de valores usa `data-bs-toggle="popover"` do Bootstrap para detalhar impostos. 

### 4.2 Privacidade & Termos

- Tabs internas controladas por JS: clique em `btnPolicy`/`btnTerms` adiciona `.active` e alterna blocos `#privacy-policy` / `#terms-of-use`.

### 4.3 Geral (somente leitura)
- Todos os campos exibem dados da organização; **não** há inputs editáveis.  
- Único elemento interativo é **Ver Planos**, que faz `window.location.href='/client/preferencias/planos-e-pagamentos'`.
---

## 5. Endpoints Django

| Ação                                 | URL Name                                    | Método        | CSRF                                               |
| ------------------------------------ | ------------------------------------------- | ------------- | -------------------------------------------------- |
| Atualizar preferência de notificação | `client_app:update_notification_preference` | `POST` (JSON) | Header `X-CSRFToken`  |
| Planos / Pagamentos                  | `client_plans_preferences`                  | `GET`         | automático                                         |
| Privacidade                          | `client_privacy_preferences`                | `GET`         | automático                                         |
| Geral                                | `client_general_preferences`                | `GET`         | automático                                         |

---

## 6. Estilos CSS Principais

- **Modal**: `position: fixed`, `width: 260px`, `border-radius: 5px`, sombra leve. 
- **Sidebar**: largura fixa `326px`, `background-color: #dfdfdf`, hover com `#cdf8e8`. 
- **Tokens de cor**: Verde Primário `#23ce6b`, Verde Escuro `#188c48`, Cinza Médio `#969696`.

---


## 7. Exemplos de Código

### 7.1 HTML – Item de Menu na Sidebar

```html
<li class="active" id="geral" onclick="window.location.href='/client/preferencias/geral'">
  <span>Geral</span>
</li>
```

### 7.2 CSS – Mostrar Modal

```css
.settings-modal.active {
  display: flex;
  opacity: 1;
  transition: opacity 0.3s ease;
}
```

### 7.3 JS – Alternar Abas de Planos

```javascript
btnPlans.addEventListener('click', () => {
  btnPlans.classList.add('active');
  btnPayments.classList.remove('active');
  plansContainer.style.display = '';
  paymentsContainer.style.display = 'none';
});
```

---

> **Última atualização:** 06 jul 2025
