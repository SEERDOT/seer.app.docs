# Views de Autenticação e Início da Sessão

## Visão Geral

Este conjunto de views trata do fluxo de login e início da sessão do usuário na aplicação, com suporte a múltiplos tenants por subdomínio. As views gerenciam a autenticação, redirecionamento para a home do cliente e apresentação de boas-vindas.

---

## *complete_login*

Finaliza o processo de login do usuário após o preenchimento da senha.

- **Método:** `POST`
- **URL de template:** `client_app/auth/complete_login.html`
- **Parâmetros esperados:** `username` (via URL) e `password` (via POST)
- **Processo:**
    - Autentica o usuário usando `authenticate`.
    - Descobre o tenant com base no subdomínio (`schema_name`).
    - Valida a existência do tenant e do usuário.
    - Em caso de sucesso, faz login com `login(request, user)` e redireciona para `client_home`.
    - Em caso de falha, exibe mensagem de erro e redireciona para o login.

---

## *client_home*

Renderiza a **página inicial da aplicação** para o cliente logado.

- **Método:** `GET`
- **Requer autenticação:** obrigatória
- **Template:** `client_app/app/home.html`
- **Contexto enviado ao template:**
    - `name`: nome capitalizado do subdomínio
    - `username`: nome de usuário autenticado
- **Validação de tenant:** tenta carregar via subdomínio. Se inválido, redireciona para o login com mensagem de erro.

---

## *welcomeView*

Exibe uma tela simples de **boas-vindas**.

- **Método:** `GET`
- **Template:** `client_app/app/welcome.html`
- **Autenticação:** não obrigatória
- **Finalidade:** apresentar uma interface amigável inicial ao usuário recém-ingresso.

---

## Considerações Finais

Essas três views compõem juntas o **fluxo de entrada do usuário**, do login até o acesso à aplicação. A validação por subdomínio garante o isolamento por tenant. O uso da função `schema_context()` em `complete_login` é essencial para autenticação multitenant segura.

