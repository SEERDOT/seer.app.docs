# Documentação - Views de Notificações

## *Visão Geral*

O módulo de notificações é responsável por exibir avisos ao usuário dentro da plataforma, bem como permitir a atualização da preferência de recebimento por e-mail. Contém duas views principais:

---

## *1. notifications_page(request)*

**Descrição:**  

Renderiza a página de notificações com possibilidade de filtragem (lidas/não lidas) e marcação de leitura individual ou em massa.

**Detalhes:**

- Requer login do usuário (`@login_required`)
- Utiliza a lista estática `NOTIFICATIONS` (em memória)
- Adiciona dinamicamente a chave `formatted_date` para exibição
- Suporta filtro via `?unread=true` (mostra apenas não lidas)
- Permite marcação como lida via POST:
    - Notificações selecionadas (`notification_ids`)
    - Todas da lista renderizada, se nenhuma for selecionada

**Template:**  

- `client_app/app/notifications.html`

---

## *2. update_notification_preference(request)*

**Descrição:**  

Endpoint POST para atualizar a preferência de recebimento de notificações por e-mail do usuário autenticado.

**Detalhes:**

- Requer login do usuário (`@login_required`)
- Espera JSON com a chave `"enabled"` (valor booleano)
- Atualiza o campo `mail_notifications` do modelo `User`
- Retorna JSON de status: `success` ou `error`

---

## *Considerações Finais*

Essas views integram tanto a interface de notificações quanto o gerenciamento das preferências do usuário. Apesar de simples, fornecem uma base funcional para futuras integrações com bancos de dados ou sistemas de mensageria mais robustos.
