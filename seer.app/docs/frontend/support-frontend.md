# Modal de Ajuda - Documentação Front-End

## Tabela de Conteúdo

1. [Propósito e Comportamento](#1-propósito-e-comportamento)
2. [Componentes Visuais](#2-componentes-visuais)
3. [Fluxo de Interação](#3-fluxo-de-interação)
4. [Validações de Formulário](#4-validações-de-formulário)
5. [Endpoints Django e Integração](#5-endpoints-django-e-integração)
6. [Estilos CSS Principais](#6-estilos-css-principais)

---

## 1. Propósito e Comportamento

O **Help Modal** é um *overlay* que fornece canais rápidos de suporte dentro da plataforma **seerdot**. Ele substitui uma página dedicada, mantendo o usuário no contexto atual enquanto oferece opções de atendimento e envio de feedback. Quando ativado, o modal:

- Exibe opções de **Atendimento** (chat/formulário).
- Pode alternar entre cinco "corpos" internos: **Início**, **Conversas**, **Formulário de Suporte**, **Sugestão** e **Histórico de Conversas**.
- Usa transições *fade‑in/out* para garantir feedback visual suave.

## 2. Componentes Visuais

### 2.1 Estrutura Geral

- contêiner flutuante com duas opções principais.
- janela principal exibida após selecionar **Atendimento**.
- **Header** – contém logotipo, título dinâmico e botão **close**.
- **Body** – área mutável onde cada *view* é carregada.
- **Footer (**``**)** – navegação entre **Início** e **Conversas**.

### 2.2 Opções e Ícones

| Elemento               | Classe / ID              | Papel                                 |
| ---------------------- | ------------------------ | ------------------------------------- |
| Atendimento            | `.support-option`        | Abre o modal de atendimento           |
| Centro de Ajuda (lock) | `.support-option.locked` | Futura feature, visual bloqueado      |
| Fale Conosco           | `.btnFaleConosco`        | Atalho para formulário de suporte     |
| Sugestão               | `#sugestaoBox`           | Caixa expansível para enviar sugestão |
| Navegação Footer       | `.bottom-option`         | Alterna entre "Início" e "Conversas"  |

> **Nota:** Os ícones são carregados via **Material Icons** (`<span class="material-icons‑outlined">`).

## 3. Fluxo de Interação

1. **Usuário clica** no botão de ajuda (fora do escopo deste documento).
2. aparece; o usuário escolhe **Atendimento**.
3. Modal é renderizado com **Body Início** e **Footer**.
4. Ações possíveis:
   - **Fale Conosco** ⇒ mostra **Formulário de Suporte** (`#bodyFormulario`).
   - **Enviar Sugestão** ⇒ expande textarea em `#sugestaoBox` e habilita envio.
   - **Footer → Conversas** ⇒ exibe **Body Conversas** ou **Histórico**.
5. **Envio bem‑sucedido** exibe mensagem de confirmação (implementação JS).

## 4. Validações de Formulário

### 4.1 Formulário de Suporte

| Campo | Regras                                      |
| ----- | ------------------------------------------- |
| `categoria`    | `select` obrigatório (placeholder inválido) |
| `assunto`    | `minlength="5"`, obrigatório                |
| `descrição`    | `minlength="10"`, obrigatório               |
| `anexo`    | Opcional; aceita `.pdf`, `.png`, `.jpeg`    |

O botão **Enviar** (`#submitSupport`) é ativado adicionando a classe `.active` via JS somente quando todos os campos obrigatórios estão válidos.

### 4.2 Formulário de Sugestão

| Campo | Regras                        |
| ----- | ----------------------------- |
| `sugAssunto`    | `minlength="5"`, obrigatório  |
| `sugDescrição`    | `minlength="10"`, obrigatório |

## 5. Endpoints Django e Integração

- **Suporte:** `client_app:support_send` → `SUPPORT_URL` (POST + `multipart/form-data`).
- **Sugestão:** `client_app:suggest_send` → `SUGGEST_URL` (POST).
- **Proteção CSRF:** token injetado em `CSRF_TOKEN` (template context) e via `{% csrf_token %}` nos formulários.

### 5.1 Resposta Esperada

| Código HTTP       | Comportamento                                           |
| ----------------- | ------------------------------------------------------- |
| `201 Created`     | Fecha modal e exibe *toast* de sucesso                  |
| `400 Bad Request` | Mantém modal aberto, mostra `.error-message` específica |

## 6. Estilos CSS Principais

- ``display: none → .active`` troca `opacity` de `0` para `1` em 300 ms (`transition`).
- Cores base: **Verde sucesso** `#23CE6B`, **Cinza neutro** `#969696`, **Preto** `#000`.
- Modal usa `position: absolute` + `z-index: 10000` para sobrepor UI.
- **Footer** indica estado ativo via `.bottom-option.active` com cor verde.


> **Última atualização:** 06 jul 2025