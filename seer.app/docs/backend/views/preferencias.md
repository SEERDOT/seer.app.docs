# Documentação das Views de Preferências do Cliente

As funções de preferência da aplicação permitem que o usuário visualize e configure informações relacionadas à sua conta, plano, perfil, privacidade e notificações. Também contemplam envio de sugestões, pedidos de suporte e integrações.

---

## *1. client_general_preferences(request)*

**Descrição:**  

Renderiza a página de preferências gerais do cliente.

**Detalhes:**

- Requer autenticação (`@login_required`)
- Passa o objeto `tenant` para o template

**Template:** 

`client_app/app/general-preferences.html`

---

## *2. client_plans_preferences(request)*

**Descrição:**  

Renderiza a página com preferências de plano do cliente, com base no subdomínio.

**Detalhes:**

- Requer autenticação (`@login_required`)
- Busca `tenant` via subdomínio (`request.get_host()`)
- Redireciona com erro se o cliente não for encontrado

**Template:**  

`client_app/app/plans-preferences.html`

---

## *3. client_privacy_preferences(request)*

**Descrição:**  

Renderiza a página com políticas de privacidade e configurações relacionadas.

**Template:**  

`client_app/app/privacy_preferences.html`

---

## *4. client_profile_preferences(request)*

**Descrição:**  

Renderiza a página de preferências de perfil do cliente, incluindo informações do usuário e do perfil da empresa.

**Detalhes:**

- Requer autenticação (`@login_required`)
- Usa `schema_context` para carregar/criar `TenantProfile`

**Template:**  

`client_app/app/profile_preferences.html`

---

## *5. update_notification_preference(request)*

**Descrição:**  

Atualiza a preferência do usuário para receber notificações por e-mail.

**Detalhes:**

- Método: `POST`
- Requer autenticação
- Espera um JSON com `"enabled"` (booleano)
- Atualiza o campo `mail_notifications` no modelo do usuário
- Retorna JSON com status da operação

---

## *Considerações Finais*

As views de preferências são essenciais para garantir uma experiência personalizada e controlada para cada cliente da plataforma. Elas oferecem pontos de entrada simples, porém funcionais, para futuras extensões e integrações com bases de dados e serviços externos.
