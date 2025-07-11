# Documentação de Componentes - Front-End

> Esta seção documenta os **componentes reutilizáveis** da interface da aplicação **seer.app**, explicando sua estrutura, comportamento, estilização e função no sistema.

---

## O que são componentes?

Componentes são **combinações de elementos base**, formando padrões de interface com um propósito específico.
### Exemplos:

* **Navbar**: barra de navegação que reúne logo, links, ícones de ação e perfil.
* **Sidebar**: menu lateral com links de navegação e seções.
* **Cards**: blocos visuais com título, conteúdo e botões.
* **Modais**: sobreposição com cabeçalho, corpo e rodapé.

### Função

Entregar **padrões reutilizáveis** com estrutura HTML, comportamento JavaScript e estilo CSS prontos para uso.

---

## 1. Navbar

### 1.1 Visão geral

A **navbar** é um componente fixo no topo da aplicação, presente em todas as views principais. Sua função é:

* Fornecer acesso rápido a funcionalidades globais (busca, notificacões, ajuda, configurações).
* Controlar a abertura/fechamento da sidebar lateral.

### 1.2 Estrutura HTML

* A navbar é definida usando a classe Bootstrap `navbar navbar-expand-lg fixed-top`.
* É dividida em três áreas principais:

  * **Hamburger menu** (esquerda): abre/fecha a sidebar.
  * **Campo de busca** (centro): formulário com input e ícone de lupa.
  * **Ícones à direita**: botões para notificações, ajuda, configurações e logout.

### 1.3 Estilização (CSS)

* A navbar possui altura de `65px`, cor de fundo `#fcfcfc`, sombra sutil e z-index alto.
* Ícones e botões são estilizados com `material-icons-outlined`.
* O hover aplica:

  * `background-color: #d6f5e5`
  * `border: 0.7px solid #188c48`
  * `color: #188c48` nos ícones
* A responsividade oculta a busca em telas pequenas (via `d-none d-lg-block`).

### 1.4 JavaScript

* A navbar possui um script que salva o **estado da sidebar** (aberta ou fechada) no `localStorage`, usando o ID do botão hamburguer:

```js
localStorage.setItem('sidebarState', 'closed');
```

* Esse comportamento é importante para manter a preferência do usuário mesmo após recarregar a página.

### 1.5 Bibliotecas utilizadas

* **Material Icons Outlined**: icones com `<span class="material-icons-outlined">`.
* **Bootstrap 5**: estrutura e responsividade.
* **Django Static**: uso de `{% load static %}` para caminhos de assets.

### 1.6 Funções específicas dos botões

| Botão            | Função                              | Comportamento                        |
| ---------------- | ----------------------------------- | ------------------------------------ |
| Menu (hamburger) | Abre/fecha sidebar                  | Salva estado no `localStorage`       |
| Busca            | Formulário GET para `global_search` | Permite perguntas e consultas        |
| Notificações     | Abre modal ou painel de alertas     | Ajax para `client_app:notifications` |
| Ajuda            | Abre modal de suporte               | Chama `toggleSupportModal()`         |
| Configurações    | Abre modal de perfil/configuração   | Chama `toggleSettingsModal()`        |
| Logout           | Envia POST oculto                   | View `public_app:logout`             |

---

## 2. Sidebar

### 2.1 Visão geral

A **sidebar** é o componente de navegação lateral da aplicação. Ela permite o acesso rápido às principais seções do sistema e está presente em todas as views internas do usuário autenticado.

Sua função principal é:

- Apresentar links organizados por hierarquia (ex: Início, Painel, Relatórios, Insights)
- Reforçar o contexto da navegação com ícones e destaques visuais
- Recolher automaticamente em telas menores ou conforme preferência do usuário

---

### 2.2 Estrutura HTML

- Estrutura HTML definida com uma `<div id="sidebar" class="sidebar">`.
- Os itens são renderizados como `<li>` dentro de uma `<ul>`, e cada item contém:

  - Um ícone da **Material Icons Outlined**
  - Um texto (visível apenas quando expandido)
  - Links configurados com `{% url %}` e controle de `active` via `request.resolver_match.url_name`

- Alguns elementos possuem **tooltips (popovers)** com `data-bs-toggle="popover"`, exibindo o nome da seção ao passar o mouse (quando recolhida).

- Há também um card comentado para **upgrade de plano**, com funcionalidades bloqueadas e botão para visualização de planos.

---

### 2.3 Estilização (CSS)

A estilização está dividida em três blocos principais:

#### 1. Estrutura da Sidebar

- **Altura total da tela** (menos a navbar superior): `height: calc(100vh - 65px)`
- **Largura padrão**: `240px`, recolhida: `70px`
- **Cor de fundo**: `#dfdfdf`
- **Scroll vertical** ativado para longos conteúdos
- **Transição suave** ao recolher ou expandir (`transition: width 0.3s ease`)

#### 2. Itens da Sidebar

- Cada `<li>` tem:

  - `display: flex`, `gap: 10px`, `padding: 10px`
  - `border-radius: 5px`, `font-size: 14px`, `font-weight: 500`
  - Cor padrão: `#272d2d`
  - Efeito hover: `background-color: #b8e4c8` e `color: #188c48`
  - Ícones com `font-size: 18px`

- O nome da seção é escondido quando a sidebar está recolhida: `display: none`

#### 3. Estilo dos Popovers

- Tooltips (popovers) são estilizados com:

  - `background-color: #b8e4c8`
  - Texto com `font-family: "Exo 2"` e `font-size: 13px`
  - Arrow da bolha colorido conforme direção

#### 4. Card de Upgrade (opcional)

- Card visível apenas quando a sidebar está expandida
- Mostra o plano atual do usuário, recursos bloqueados e botão “Ver Planos”
- Design limpo com cores verdes, ícones pequenos e fontes ajustadas para layout compacto

---

### 2.4 JavaScript

A sidebar depende indiretamente do script da **navbar**, que controla sua abertura/fechamento:

- O botão “hamburger” da navbar adiciona ou remove a classe `.open` na `<div id="sidebar">`
- O estado da sidebar é salvo no `localStorage`, com valor `"open"` ou `"closed"`
- Quando recolhida:

  - Os textos dos links desaparecem
  - Apenas os ícones são exibidos
  - Tooltips aparecem ao passar o mouse (via Bootstrap popover)

---

### 2.5 Bibliotecas utilizadas

- **Material Icons Outlined**: ícones visuais dos itens de menu
- **Bootstrap 5**: sistema de popovers e responsividade
- **Django Template Language**: `request.resolver_match` para destacar links ativos
- **LocalStorage + JS simples**: controle de persistência da sidebar
- **CSS Transition**: animação de recolhimento

---

### 2.6 Itens Navegáveis

| Seção         | Ícone (`material-icons-outlined`) | URL Django (`{% url %}`)              | Detalhes                                               |
|---------------|----------------------------|----------------------------------------|--------------------------------------------------------|
| Início        | `home`                     | `/client/`                             | Link direto no `<li>`, sem `<a>`                      |
| Painel        | `dashboard`                | `client_app:panel_default`             | Usa `request.resolver_match.url_name` para ativar      |
| Relatórios    | `analytics`                | `client_app:relatorios`                | Ativo em várias subviews relacionadas                 |
| Insights      | `tips_and_updates`         | `client_app:insights`                  | Inclui detalhe `insight_detail`                       |
| Recomendações | `insights`                 | (sem link ainda)                       | Apenas ícone + label estático                         |
| (Outros)      | `thermostat`, `auto_awesome`| Comentados no HTML                     | Futuras funcionalidades: Termômetro, Assistente AI     |

---

### 2.7 Responsividade

- Em telas menores que 992px:

  - A sidebar é automaticamente recolhida
  - O conteúdo (`#content`) se ajusta com `margin-left: 70px`

- A sidebar usa `transition` para suavizar a mudança de largura
- O conteúdo geral também responde com `transition: margin-left`

---


## 3. Sidebar de Relatórios

### 3.1 Visão geral

A **sidebar da página de criação de relatórios** é um componente lateral **contextual**, que aparece **apenas nas páginas de criação ou edição de relatórios**. Diferente da sidebar global do sistema, ela **não é usada para navegação geral**, mas sim como **indicador de progresso** em um processo passo a passo.

Essa sidebar:
- Mostra ao usuário **em qual etapa do processo** de criação ele está
- Permite uma leitura visual rápida de **etapas concluídas, atuais e futuras**
- Aparece fixa no lado esquerdo da interface da criação de relatórios

---

### 3.2 Estrutura HTML

- O HTML está encapsulado em um `<div class="left-column">`, posicionado como a coluna lateral esquerda.
- Possui três áreas principais:
  1. **Cabeçalho (`.header`)**
     - Título: “Criar Relatório”
     - Ícone de `info` com tooltip (via Bootstrap)
  2. **Descrição (`.description`)**
     - Texto explicativo do propósito do processo
  3. **Lista de etapas (`.steps-list`)**
     - Renderizada dinamicamente com `{% for label in steps %}`
     - Usa controle de classe condicional para marcar o passo atual, os passos concluídos e os futuros

- Ícones:
  - Etapa concluída: `check_circle` (verde)
  - Etapa atual: `cached` (azul)
  - Etapas futuras: `circle` (cinza)

---

### 3.3 Estilização (CSS)

A estilização é feita exclusivamente no escopo da sidebar e suas etapas:

#### 1. Layout `.left-column`
- **Largura:** `20%`
- **Fixo à esquerda** na página de criação
- **Sem fundo colorido**, visual leve
- **Borda direita** com linha sólida `#969696`
- **Alinhamento vertical com `flex-column`**

#### 2. Cabeçalho
- `display: flex` com espaçamento entre título e ícone
- Fonte do título: peso `600`, tamanho `1.2rem`, cor `#000`
- Ícone `info`: cor `#626b6b`, tamanho `1.2rem`

#### 3. Texto Descritivo
- Fonte leve (`400`), tamanho `0.95rem`
- Cor preta com `line-height` de `1.4` para boa legibilidade

#### 4. Linha pontilhada
- Usada como separador visual antes da lista de etapas

#### 5. Lista de etapas (`.steps-list`)
- Apresenta visual tipo **checklist**
- Cada item:
  - É flexível, com ícone e texto alinhados
  - Ícones mudam de cor conforme o status:
    - Verde (`#188C48`) → completado
    - Azul (`#2384CE`) → etapa atual
    - Cinza (`#626B6B`) → futura
  - Texto da etapa:
    - Completado: `line-through` cinza claro
    - Atual: `font-weight: 500`, destaque visual
    - Futuro: texto simples

- O item atual tem **fundo destacado**, borda arredondada e sombra sutil.

---

### 3.4 Lógica de Estado (Django Template)

- A estrutura utiliza o `forloop.counter` do Django para comparar cada passo com `current_step`:
  - Se `< current_step`: etapa já concluída
  - Se `== current_step`: etapa atual
  - Se `> current_step`: etapa futura

- Cada tipo de etapa recebe classes e ícones diferentes, sem necessidade de JavaScript adicional.

---

### 3.5 Bibliotecas utilizadas

- **Material Icons Outlined**: para os ícones de etapas (`check_circle`, `cached`, `circle`)
- **Bootstrap 5**: para tooltips (`data-bs-toggle="tooltip"`)
- **Django Templating**: para renderização condicional dos passos e estado visual

---

### 3.6 Função no sistema

- Ajuda o usuário a entender em que ponto está na criação do relatório
- Indica visualmente o que já foi feito e o que falta concluir
- Reflete o estado do servidor em tempo real por meio da variável `current_step`
- Contribui para **transparência no fluxo** e **confiança do usuário** em processos longos

---


## 4. Sidebar de Preferências da Organização

### 4.1 Visão geral

A **sidebar da página de preferências** é um componente **navegacional fixo**, utilizado **exclusivamente na área de configurações da organização**. Ela permite ao usuário alternar entre diferentes seções da página de preferências sem perder o contexto da navegação.

Sua função principal é:

- Fornecer uma estrutura clara de navegação entre abas de configurações
- Manter o componente visível e acessível independentemente da seção ativa
- Reforçar visualmente qual seção está selecionada

---

### 4.2 Estrutura HTML

- O componente é encapsulado em `<div class="preferences-sidebar">`.
- A navegação é composta por uma `<ul>` com `<li>`s que:
  - Apresentam o nome da seção
  - Usam `onclick` para fazer navegação direta via `window.location.href`
  - Destacam a seção ativa via Django:  
    ```django
    class="{% if request.resolver_match.url_name == '...' %}active{% endif %}"
    ```

- O conteúdo é dividido em dois blocos com `<span class="title">`:
  1. **Preferências da Organização**
  2. **Preferências Pessoais**

- Há um `<hr />` separando visualmente essas duas seções.

---

### 4.3 Estilização (CSS)

#### 1. Estrutura geral
- Largura fixa: `326px`
- Cor de fundo: `#dfdfdf`
- Sem flexibilidade de redimensionamento (`flex-shrink: 0`)
- Borda direita com `1px solid #969696`
- Transição suave aplicada à largura

#### 2. Títulos de seção (`.title`)
- Fonte maior: `20px`
- Peso: `500`
- Visualmente destacados para separação hierárquica

#### 3. Itens de navegação (`li`)
- Padding interno: `5px 14px`
- Cursor `pointer`
- Item com `.active` ou `:hover`:
  - `background-color: #cdf8e8`
  - `color: #188c48`

#### 4. Organização com flexbox
- A lista (`ul`) é uma coluna vertical com `gap: 10px`

---

### 4.4 Comportamento e Navegação

- Cada item da lista redireciona o usuário para a rota da respectiva preferência:
  | Item                 | Rota                                 | Observação                     |
  |----------------------|--------------------------------------|--------------------------------|
  | Geral                | `/client/preferencias/geral`         | Preferência organizacional     |
  | Planos e Pagamentos  | `/client/preferencias/planos-e-pagamentos` | Relacionado a billing          |
  | Privacidade          | `/client/preferencias/privacidade`   | Políticas e controle de dados  |
  | Perfil               | `client_app:client_profile_preferences` | Preferência do usuário logado  |
  | Notificações         | `client_app:notifications`           | Notificações e alertas         |

- O destaque visual do item ativo é controlado pelo Django com `request.resolver_match.url_name`.

---

### 4.5 Bibliotecas e Tecnologias utilizadas

- **Bootstrap 5 (opcional):** para tooltips e responsividade (não usado diretamente nesta sidebar, mas compatível)
- **Django Template Language:** para identificação e estilização dinâmica do item ativo
- **Material Icons (potencial):** não estão incluídos neste arquivo, mas a arquitetura da sidebar é compatível

---

### 4.6 Função no sistema

- Garante uma navegação clara e segmentada entre diferentes **preferências do sistema**
- Mantém consistência visual com a sidebar principal do sistema, porém com escopo restrito à área de **configuração**
- Evita o uso de abas ou dropdowns, oferecendo uma **experiência mais estável e visível**
- Pode ser expandida para incluir mais seções no futuro (ex: “Membros”, que já está comentado no código)

---


# 5. Componentes: Modais

## 5.1 O que são Modais?

Modais são **componentes de sobreposição** que aparecem no topo do conteúdo da aplicação para apresentar informações adicionais ou permitir interações rápidas, sem que o usuário precise sair da tela atual.

### Características dos modais:

- **Fixos e flutuantes:** aparecem sobre o restante da interface.
- **Foco isolado:** podem desativar temporariamente o conteúdo de fundo (não necessariamente).
- **Fechamento explícito ou por clique fora.**
- **Acionamento via botão, ícone ou evento.**

### Exemplo de componentes modais:

- **Modal de configurações**
- **Modal de ajuda**
- **Modais de relatórios (ex: cencelar processo, sair da operação)**

### Função no sistema

Os modais proporcionam uma **experiência de usuário fluida**, permitindo ajustes, ações ou visualizações rápidas sem recarregar a página ou navegar para outras views.

### Estrutura e Componentização

Modais são **componentizáveis** da seguinte maneira:

| Camada              | Composição                                                                 |
|---------------------|----------------------------------------------------------------------------|
| HTML                | Estrutura do modal: div principal, botões, ícones, conteúdo interno        |
| CSS                 | Estilização: posição fixa, z-index, sombra, transições, estados `.active`  |
| JavaScript          | Abertura e fechamento (toggle), interações internas, animações             |
| Integração (Django) | Inclusão via `{% include %}`, interação com views ou templates específicos  |

---

## 5.2 Modal de Configurações

### 5.2.1 Visão Geral

O **Modal de Configurações** é exibido ao clicar no ícone de engrenagem da navbar. Sua função principal é **oferecer atalhos rápidos** para:

- Preferências da organização
- Logout
- Itens bloqueados (como "Preferências Pessoais" e "Relatar Problema", ainda não habilitados)

### 5.2.2 Estrutura HTML

- Envolvido por `<div id="settingsModal" class="settings-modal">`
- Cada opção dentro do modal é representada por um bloco `.settings-option`
- Ícones `material-icons-outlined` representam as ações
- Algumas opções possuem `class="locked"` com ícone de cadeado (`lock`) para indicar indisponibilidade
- A opção de "Sair" executa `document.getElementById('logout-form').submit();`

### 5.2.3 Estilização (CSS)

| Elemento             | Estilo principal                                 |
|----------------------|--------------------------------------------------|
| `.settings-modal`    | `position: fixed`, `top: 70px`, `right: 10px`    |
|                      | Largura: `260px`, cor: `#fcfcfc`, `border-radius` |
| `.active`            | Aplica `display: flex` e `opacity: 1`            |
| `.settings-option`   | `display: flex`, `gap: 10px`, `hover` com cinza claro |
| `.lock-icon`         | Utilizado nos itens indisponíveis (`locked`)     |

### 5.2.4 Interações e JavaScript

- O modal é exibido ou ocultado via **classe `active`**, que pode ser controlada por JS.
- Os botões acionam diretamente redirecionamentos (`window.location.href`) ou ações (`submit()`).
- Pode ser facilmente acoplado a eventos como `onclick="toggleSettingsModal()"`.

### 5.2.5 Função no Sistema

Este modal **centraliza ações globais de configuração e sessão**, funcionando como um menu de ações rápidas, especialmente útil em telas com muita informação ou profundidade de navegação.

### 5.2.6 Comportamentos especiais

- O modal não interfere no scroll da página.
- Pode ser facilmente estendido com novos itens.
- Itens bloqueados são visíveis mas não interativos (sem `onclick` e com ícone de `lock`).

---

## 5.3 Modal de Suporte

### 5.3.1 Visão Geral

O **Modal de Suporte** é um componente robusto e multifuncional da aplicação, desenvolvido para **centralizar o atendimento ao usuário**, **receber sugestões de melhorias**, e **acompanhar conversas já iniciadas**. Ele é acionado a partir do ícone de "Ajuda" na navbar e possui múltiplas camadas de interação.

Este modal compreende dois níveis:

- Um **mini modal inicial** com as opções "Atendimento" e "Centro de Ajuda"
- Um **modal expandido** com abas, formulários, histórico de conversas e áreas contextuais

---

### 5.3.2 Funcionalidades principais

| Função                     | Descrição                                                                 |
|---------------------------|---------------------------------------------------------------------------|
| Atendimento               | Abertura de chamados diretamente com o suporte técnico                   |
| Sugestão de funcionalidades | Envio direto de ideias para evolução da plataforma                      |
| Visualização de conversas | Histórico de mensagens já enviadas com opção de visualização/remover     |
| Navegação entre seções    | Rodapé interativo que alterna entre "Início" e "Conversas"               |

---

### 5.3.3 Estrutura HTML

#### 1. `#supportModal`
Mini modal inicial com opções:
- **Atendimento**: aciona o modal principal
- **Centro de Ajuda**: atualmente bloqueado (`locked` com ícone `lock`)

#### 2. `#atendimentoModal`
Modal expandido com múltiplas seções:

| ID do container     | Conteúdo                                                                            |
|---------------------|-------------------------------------------------------------------------------------|
| `#headerInicio`     | Cabeçalho padrão com título e ícone de fechar                                       |
| `#headerConversas`  | Cabeçalho alternativo ao acessar o histórico de mensagens                           |
| `#bodyInicio`       | Tela inicial com prompt "Como podemos ajudar você?"                                 |
| `#bodyFormulario`   | Formulário de suporte (bug, cobrança, lentidão etc.) com upload de arquivos         |
| `#bodySugestao`     | Formulário para envio de sugestões de funcionalidades                               |
| `#bodyConversas`    | Estado vazio do histórico de conversas com botão "Fale Conosco"                     |
| `#bodyHistoricoConversas` | Espaço dinâmico para renderizar conversas anteriores via JS template         |
| `#conversation-template`  | Template JavaScript para exibir interações anteriores                         |
| `.modal-bottom-bar` | Rodapé com botões fixos para navegação ("Início" e "Conversas")                     |

---

### 5.3.4 Estilização (CSS)

A aparência do modal é cuidadosamente desenhada para transmitir clareza e acessibilidade:

| Elemento                      | Destaques visuais                                               |
|-------------------------------|-----------------------------------------------------------------|
| `.support-modal`              | Flutuante, canto superior direito, sombra leve                 |
| `.atendimento-modal`          | Fundo degradê verde/branco, bordas arredondadas, largura fixa  |
| `.atendimento-body`           | Layout flexível, com `scroll` interno, diferentes modos de exibição |
| `.fale-conosco`, `.sugestao-box` | Ações de destaque para interação com o usuário               |
| `.modal-bottom-bar`           | Rodapé fixo com alternância de seções, ícones grandes e responsivos |
| `.form-group`, `.file-upload` | Campos estilizados com foco na acessibilidade e usabilidade    |

---

### 5.3.5 Interações e JavaScript

O modal se comporta de maneira dinâmica através de interações controladas por **JavaScript** (não incluído explicitamente no trecho acima, mas referenciado).

Principais interações JS:

| Evento                           | Ação esperada                                                                 |
|----------------------------------|------------------------------------------------------------------------------|
| `openAtendimentoModal(event)`    | Abre o modal expandido de atendimento                                        |
| `closeAtendimentoModal(event)`   | Fecha o modal                                                                 |
| Alternância de seções            | Mudança de visibilidade entre `#bodyInicio`, `#bodyFormulario`, etc.         |
| Validação e envio de formulários | Ativação de botão "Enviar" apenas quando todos os campos obrigatórios estão válidos |
| Template `#conversation-template`| Renderiza dinamicamente histórico de conversas com ações via `data-action`   |
| Atualização de footer            | O `#optionInicio` e `#optionConversas` mudam cor com base na seção ativa     |

---

### 5.3.6 Fluxo do usuário

1. **Clique no botão de ajuda (ícone na navbar)**
2. O `#supportModal` aparece com opções de atendimento.
3. Ao clicar em "Atendimento":
   - Exibe `#atendimentoModal`, com a tela `bodyInicio` como padrão.
4. O usuário pode:
   - Preencher o formulário de suporte
   - Enviar uma sugestão
   - Alternar entre conversas anteriores e novos envios
5. A navegação ocorre sem reloads de página, garantindo **fluidez e retenção de contexto**.

---

### 5.3.7 Considerações

- O modal é altamente **modular**, com múltiplas seções independentes e fáceis de estender.
- Utiliza boas práticas de UI como placeholders, botões desabilitados até preenchimento válido e mensagens de erro contextuais.
- A separação de conteúdo por IDs permite controle total via JS e CSS com baixo acoplamento.
- Ideal para crescimento do sistema com novos canais de suporte, FAQs ou integração com chatbots.

> Este é um dos componentes mais completos da aplicação, representando uma central de relacionamento entre o usuário e o time de produto/suporte.

---


## 5.4 Modal de Saída do Relatório

### 5.4.1 Visão Geral

O **Modal de Saída do Relatório** é um componente de confirmação utilizado para **evitar que o usuário perca seu progresso ao tentar sair do fluxo de criação de um relatório**. Sua função principal é exibir uma advertência clara e permitir que o usuário escolha entre retornar ao fluxo ou confirmar a saída.

Este modal é acionado manualmente (por exemplo, ao clicar em um botão com ícone de fechar) e só é exibido sob demanda.

---

### 5.4.2 Finalidade e Benefícios

- **Evitar perda acidental de dados** durante a criação de um relatório.
- **Permitir tomada de decisão consciente**, apresentando de forma clara as consequências de sair do processo.
- **Fortalecer a experiência de usuário (UX)** com feedback visual direto e interruptivo.

---

### 5.4.3 Estrutura do Modal

| Elemento              | Função                                                                 |
|------------------------|------------------------------------------------------------------------|
| `#exit-modal`         | Container principal do modal, inicialmente oculto (`display: none`)    |
| `h2`, `p`             | Mensagem de alerta clara ao usuário sobre a perda de progresso         |
| `#cancel-form`        | Formulário `POST` para acionar a view de cancelamento do draft         |
| `#btn-back`           | Fecha o modal e mantém o usuário no processo atual                     |
| `#btn-continue`       | Submete o formulário e executa a saída do fluxo de relatório           |
| `#overlay`            | Escurece o fundo da página enquanto o modal está aberto                |

---

### 5.4.4 Interação JavaScript

O comportamento do modal é inteiramente controlado via **JavaScript**, oferecendo uma interação fluida e direta:

| Ação do usuário          | Efeito                                                              |
|--------------------------|---------------------------------------------------------------------|
| Clique no botão de fechar (`.red-close-icon`) | Exibe o modal e a sobreposição escura (`#overlay`)       |
| Clique em "Voltar" (`#btn-back`) | Fecha o modal e restaura a visualização normal da página        |
| Clique em "Sim, continuar" (`#btn-continue`) | Submete o formulário `#cancel-form` com `POST`            |
| Clique fora do modal (overlay) | Fecha o modal (mesmo comportamento do botão "Voltar")             |

O código JavaScript também utiliza `bind` para garantir que o botão "Voltar" seja corretamente acionado quando o fundo escuro for clicado, proporcionando um comportamento **intuitivo e acessível**.

---

### 5.4.5 Comportamento Padrão

- O modal **não está visível** até que seja explicitamente ativado.
- Após confirmação, a requisição é enviada para `{% url 'client_app:cancelar_draft' draft.id %}`, onde o sistema cancela o rascunho de relatório.
- A estrutura permite fácil reutilização em outras partes do sistema com pequena adaptação de IDs e ações.

---

### 5.4.6 Considerações

- Este modal prioriza a **segurança de dados temporários**, reduzindo atritos na experiência do usuário.
- Está isolado em sua estrutura e lida com **operações críticas**, portanto é um candidato forte a ser componentizado e reutilizado em qualquer contexto que exija confirmação de ações destrutivas.
- Recomendável adicionar **animações leves** de transição para suavizar a experiência de entrada e saída.

> Esse modal atua como um **guardião de fluxo**, protegendo o usuário contra perdas acidentais e promovendo uma tomada de decisão segura.

> **Última atualização:** 05 jul 2025