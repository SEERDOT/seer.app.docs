# Documentação do Backend do Fluxo de Relatórios (`report_app`)

## Visão Geral

Este documento detalha as views, endpoints e a lógica de processamento do backend responsável pelo ciclo de vida dos relatórios customizados.

---

## Principais Views e Endpoints

1. **Criação de ReportDraft (Escolha do Modelo)**
2. **Configuração de Metadados**
3. **Seleção de Indicadores**
4. **Seleção de Visualizações**
5. **Definição de Destinatários e Canais**
6. **Configuração de Agendamento**
7. **Revisão, Confirmação e Agendamento Final**
8. **Listagem e Detalhamento de Relatórios**

---

## Detalhamento das Views

### 1. **Criação de ReportDraft (Escolha do Modelo) — `modelos`**
- **Endpoint:** `/relatorios/modelos/`
- **Métodos:** `GET`, `POST`
- **Lógica:**
  - `GET`: Exibe os modelos disponíveis para o usuário escolher (ou iniciar em branco).
  - `POST`: 
    - Se o usuário escolher "rascunho em branco", cria um novo `ReportDraft` e redireciona para a etapa de metadados.
    - Se escolher um modelo, cria um `ReportDraft` já pré-preenchido com os dados do template, indicadores e visualizações, e redireciona para a etapa de destinatários.
- **Contexto enviado ao template:** modelos, categorias, passo atual.

---

### 2. **Configuração de Metadados — `metadados`**
- **Endpoint:** `/relatorios/metadados/<draft_id>/`
- **Métodos:** `GET`, `POST`
- **Lógica:**
  - `GET`: Exibe formulário para preenchimento de título, categoria e descrição.
  - `POST`: Valida e salva os metadados no `draft_config` do `ReportDraft`, atualiza status e redireciona para seleção de indicadores.
- **Contexto enviado ao template:** draft, formulário, passo atual.

---

### 3. **Seleção de Indicadores — `indicadores`**
- **Endpoint:** `/relatorios/indicadores/<draft_id>/`
- **Métodos:** `GET`, `POST`
- **Lógica:**
  - `GET`: Lista indicadores disponíveis conforme a categoria do relatório.
  - `POST`: Salva os indicadores selecionados, atualiza status e redireciona para seleção de visualizações.
- **Contexto enviado ao template:** draft, indicadores, indicadores já selecionados, passo atual.

---

### 4. **Seleção de Visualizações — `visualizacoes`**
- **Endpoint:** `/relatorios/visualizacoes/<draft_id>/`
- **Métodos:** `GET`, `POST`
- **Lógica:**
  - `GET`: Lista visualizações permitidas para a categoria do relatório.
  - `POST`: Salva as visualizações selecionadas, atualiza status e redireciona para definição de destinatários.
- **Contexto enviado ao template:** draft, visualizações, visualizações já selecionadas, passo atual.

---

### 5. **Definição de Destinatários e Canais — `destinatarios_canais`**
- **Endpoint:** `/relatorios/destinatarios_canais/<draft_id>/`
- **Métodos:** `GET`, `POST`
- **Lógica:**
  - `GET`: Exibe formulário para adicionar e-mails dos destinatários.
  - `POST`: Salva os destinatários no `draft_config`, atualiza status e redireciona para agendamento.
- **Contexto enviado ao template:** draft, passo atual.

---

### 6. **Configuração de Agendamento — `agendamentos`**
- **Endpoint:** `/relatorios/agendamentos/<draft_id>/`
- **Métodos:** `GET`, `POST`
- **Lógica:**
  - `GET`: Exibe formulário para configurar frequência, dias e horários de envio.
  - `POST`: Salva as configurações de agendamento no `draft_config`, atualiza status e redireciona para revisão.
- **Contexto enviado ao template:** draft, agendamento atual, labels de dias/meses/horas, passo atual.

---

### 7. **Revisão, Confirmação e Agendamento Final — `revisao`**
- **Endpoint:** `/relatorios/revisao/<draft_id>/`
- **Métodos:** `GET`, `POST`
- **Lógica:**
  - `GET`: Exibe resumo de todas as escolhas feitas nas etapas anteriores.
  - `POST`: Valida e cria um novo `ReportSchedule` consolidando todas as configurações, deleta o `ReportDraft` e redireciona para a listagem de relatórios.
  - Retorna JSON com status de sucesso ou erro de validação.
- **Contexto enviado ao template:** draft, agendamento, texto de frequência, passo atual.

---

### 8. **Listagem e Detalhamento de Relatórios — `relatorios`, `relatorio_detail`**
- **Endpoint:** `/relatorios/` e `/relatorios/<schedule_id>/`
- **Métodos:** `GET`
- **Lógica:**
  - `relatorios`: Lista todos os relatórios agendados, com filtros por categoria, busca e status (ativo/inativo).
  - `relatorio_detail`: Exibe detalhes completos de um relatório agendado, incluindo metadados, indicadores, visualizações e histórico de envios.
- **Contexto enviado ao template:** lista de relatórios, filtros, dados detalhados do relatório.

---

### 9. **Edição de Relatório — `editar_relatorio`**
- **Endpoint:** `/relatorios/editar/<schedule_id>/`
- **Método:** `POST` (requisição AJAX, espera JSON)
- **Lógica:**
  - Recebe dados atualizados de metadados, indicadores, visualizações, agendamento e destinatários.
  - Atualiza o objeto `ReportSchedule` correspondente, salvando as alterações em `layout_config` e `schedule_config`.
  - Retorna JSON com status de sucesso ou erro.
- **Uso:** Permite ao usuário editar relatórios já agendados sem precisar criar um novo do zero.

---

### 10. **Views Auxiliares**

#### **Cancelar Rascunho — `cancelar_draft`**
- **Endpoint:** `/relatorios/cancelar_draft/<draft_id>/`
- **Método:** `POST`
- **Lógica:** Exclui o `ReportDraft` correspondente e redireciona para a listagem de relatórios.
- **Uso:** Permite ao usuário abandonar o fluxo de criação de relatório e descartar o rascunho.

---

#### **Desativar Relatório — `disable_schedule`**
- **Endpoint:** `/relatorios/disable_schedule/<schedule_id>/`
- **Método:** `POST`
- **Lógica:** Marca o `ReportSchedule` como inativo (`is_active = False`) e salva.
- **Uso:** Permite ao usuário desativar relatórios agendados sem excluí-los.

---

#### **Excluir Relatório — `delete_schedule`**
- **Endpoint:** `/relatorios/delete_schedule/<schedule_id>/`
- **Método:** `POST`
- **Lógica:** Exclui o `ReportSchedule` do banco de dados e redireciona para a aba de inativos.
- **Uso:** Permite ao usuário remover permanentemente um relatório agendado.

---

#### **Desativar Múltiplos Relatórios — `disable_schedules`**
- **Endpoint:** `/relatorios/disable_schedules/`
- **Método:** `POST`
- **Lógica:** Recebe uma lista de IDs e marca todos os `ReportSchedule` correspondentes como inativos.
- **Uso:** Ação em massa para desativar vários relatórios de uma vez.

---

#### **Excluir Múltiplos Relatórios — `delete_schedules`**
- **Endpoint:** `/relatorios/delete_schedules/`
- **Método:** `POST`
- **Lógica:** Recebe uma lista de IDs e exclui todos os `ReportSchedule` correspondentes.
- **Uso:** Ação em massa para excluir vários relatórios de uma vez.

---

#### **Reativar Relatórios — `reactivate_schedules`**
- **Endpoint:** `/relatorios/reactivate_schedules/`
- **Método:** `POST`
- **Lógica:** Recebe uma lista de IDs e marca todos os `ReportSchedule` correspondentes como ativos (`is_active = True`).
- **Uso:** Permite reativar relatórios previamente desativados. 

---

## Observações

- Algumas funcionalidades, como edição e envio automático, ainda estão em desenvolvimento. 