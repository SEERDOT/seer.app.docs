# Documentação das Views de Relatórios

Este documento descreve todas as views relacionadas à criação, edição, gerenciamento e envio de relatórios agendados na aplicação multi-tenant.

---

## `relatorios`

Lista todos os relatórios agendados (ativos ou inativos).Permite busca por nome, filtro por categoria e alternância entre abas (ativos/inativos).

- Template: `client_app/app/relatorios/relatorios.html`

---

## `modelos`

Exibe modelos prontos de relatórios para seleção e inicializa um novo rascunho com base em um template.

- Template: `client_app/app/relatorios/modelos.html`

---

##  `metadados`

Etapa 2: coleta metadados do relatório, como título, categoria e descrição.

- Template: `client_app/app/relatorios/metadados.html`

---

## `indicadores`

Etapa 3: permite a seleção dos indicadores que serão incluídos no relatório.

- Template: client_app/app/relatorios/indicadores.html

---

## `visualizacoes`

Etapa 4: define os tipos de visualização que serão gerados (ex: gráficos).

- Template: `client_app/app/relatorios/visualizacoes.html`

---

## `destinatarios_canais`

Etapa 5: define os e-mails de destino para recebimento dos relatórios.

- Template: `client_app/app/relatorios/destinatarios_canais.html`

---

## `agendamentos`

Etapa 6: define a frequência de envio do relatório.

Template: `client_app/app/relatorios/agendamentos.html`

---

## `revisao`

Etapa 7: revisão final e criação do objeto `ReportSchedule` persistente.

- Template: `client_app/app/relatorios/revisao.html``

---

## `relatorio_detail`

Visualização detalhada de um relatório agendado.

- Template: client_app/app/relatorios/detail.html

---

## Ações de Gerenciamento

- `cancelar_draft`: remove um rascunho de relatório não finalizado
- `disable_schedule`: desativa um agendamento
- `delete_schedule`: deleta um agendamento
- `disable_schedules`: desativa vários agendamentos
- `delete_schedules`: deleta vários agendamentos
- `reactivate_schedules`: reativa agendamentos inativos

## Observação Final

As views de relatório seguem uma sequência de etapas bem definida (`STEPS`) que guia o usuário desde a escolha do modelo até a revisão e agendamento. Toda a lógica respeita o isolamento de dados por tenant com uso de `schema_context`, e está preparada para personalizações futuras.
