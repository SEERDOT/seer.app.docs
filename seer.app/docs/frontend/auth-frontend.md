# Página de Autenticação

> Essa é a documentação completa dos **templates** relacionados a login, registro, recuperação de senha e verificação de e‑mail da aplicação **seer.app**.



## 1. Visão geral

O módulo de autenticação do front‑end é composto por **cinco fluxos principais**:

1. **Login**  
2. **Cadastro de conta**  
3. **Recuperação / redefinição de senha**  
4. **Verificação de e‑mail (2FA)**  
5. **Exibição de Termos de Uso e Política de Privacidade**  

Todos os templates herdam de um layout base comum (`base_auth.html`), garantindo visual consistente e reutilização de código.

---

## 2. Hierarquia de templates

```
public_app/
│
├── base_auth.html          ← layout genérico (head, CSS, JS)
├── login.html              ← form de login
├── register.html           ← form de cadastro + modais de termos
├── verify_email.html       ← inserção de código 2FA
├── reset_password.html     ← solicitar link de reset
├── set_new_password.html   ← definir nova senha
├── reset_password_email.html (HTML de e‑mail)
├── terms.html              ← modal Termos de Uso
└── privacy-policy.html     ← modal Política de Privacidade
```

---

### 2.1 `base_auth.html`
Arquivo que serve como base para a construção das páginas de login e registro. Possui as configurações básicas de um arquivo HTML e importa elementos necessários à construção dessa parte de autenticação.

* **Herança de layout –** todos os demais templates estendem este arquivo com
`{% extends 'public_app/base_auth.html' %}`
* Define **head** com Bootstrap 5, Font Awesome, Google Fonts e folhas de estilo do projeto  
* `{% block title %}` - título da aba/página, usado em `<title>`  
* `{% block content %}` - onde o HTML específico de cada página será injetado

---


### 2.2 Templates filhos

| Página | Objetivo | URL (reverse) | Destaques |
|--------|----------|---------------|-----------|
| `login.html` | Autenticar usuário existente | `public_app:login` | Campo único “identification” aceita **email ou username**; exibe mensagens do Django messages.|
| `register.html` | Criar nova conta | `public_app:register` | Validação JS de email, username, empresa e senha; exibe **modais** de termos/privacidade; inclui botão de exibir senha.|
| `verify_email.html` | 2FA por código enviado por e‑mail | `public_app:verify_email` | Cinco inputs 1‑character com navegação automática; link para reenviar código.|
| `reset_password.html` | Solicitar reset | `public_app:password_reset` | Validação de email em tempo real antes de habilitar envio.|
| `set_new_password.html` | Definir nova senha via token | `public_app:password_reset_confirm` | Dupla checagem de força + igualdade de senha.|
| `reset_password_email.html` | Corpo do e‑mail com link | gerado pelo backend | Layout responsivo em tabela HTML puro (compatível com clientes de e‑mail).|
| `terms.html` & `privacy-policy.html` | Conteúdo jurídico exibido em modal | n/a | Abertos a partir de links na tela de cadastro; fecham via ícone ou clique fora.  |

---

## 3. Fluxo de Login

1. **GET `/login/`**  
   Renderiza o template `login.html`.

2. **POST `/login/`**  
   Envia credenciais para a view `public_app:login`.

3. **Autenticação**  
   - O backend tenta autenticar usando **username** ou **email** + **senha**.  
   - Em caso de sucesso, redireciona para a área logada.  
   - Em caso de erro, adiciona mensagem em `django.contrib.messages`.

4. **Exibição de feedback**  
   - Mensagens de erro/sucesso exibidas no topo com classes Bootstrap (`alert-danger`, `alert-success`).  
   - O botão **Entrar** permanece estático, sem spinner.

> **Regras de UX**  
> - Link “Criar uma conta” → `/register/`  
> - Link “Esqueci minha senha” → `/password-reset/`

---

## 4. Fluxo de Registro

### Front-end (JS + HTML5):

### Validações em Tempo Real nos campos
- **Email**: 
    - Validação via atributo `pattern` do HTML5.
- **Username**:  
    - 5–150 caracteres  
    - Caracteres permitidos: letras, números, `_`, `@`, `+`, `.`, `-`.
- **Empresa**:  
    - Campo não pode estar vazio.
- **Senha**:  
Requisitos mínimos:
    - 8+ caracteres  
    - 1 letra maiúscula  
    - 1 letra minúscula  
    - 1 dígito numérico  
    - 1 caractere especial  

### Comportamento do Botão
- Botão **"Criar"** permanece desabilitado até que:
  1. Todos os campos atendam aos requisitos de validação.
  2. Checkbox de aceite (Termos de Uso + Política de Privacidade) esteja marcado.

### Modais
- **Termos de Uso**:  
  Abre `terms.html` ao clicar no link correspondente.
- **Política de Privacidade**:  
  Abre `privacy-policy.html` ao clicar no link correspondente.

---

## 5. Fluxo de Recuperação de Senha

1. **GET `/password-reset/`**  
   Renderiza `reset_password.html`.

2. **POST `/password-reset/`**  
   - Validação frontend: email via regex `^[^\s@]+@[^\s@]+\.[^\s@]+$`  
   - Botão desabilitado até email válido  
   - Envia e-mail com link usando template `reset_password_email.html`

3. **Redefinição de senha**  
   - Link direciona para `/password-reset-confirm/<uidb64>/<token>/`  
   - Renderiza `set_new_password.html`  
   - Validações:  
     * Força da senha (mesmas regras do registro)  
     * Confirmação deve ser idêntica  
   - Ícone de visibilidade para ambas as senhas

## 6. Fluxo de Verificação de Email (2FA)

1. **GET `/verify-email/`**  
   Renderiza `verify_email.html` após registro

2. **Lógica de inputs**:
   - 5 campos de 1 caractere com navegação automática
   - Preenchimento automático ao digitar
   - Backspace volta ao campo anterior

3. **Links de fallback**:
   - "Reenviar código": `/resend-verification-email/`
   - "Editar email": retorna ao registro

> **Última atualização:** 02 jul 2025