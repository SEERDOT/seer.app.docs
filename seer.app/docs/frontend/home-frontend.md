# Documentação da Página Home

> Essa é a documentação completa dos **templates** relacionados à página inicial (Home) da aplicação **seer.app**.

---

## 1. Visão geral

O módulo de **Front-End** da página Home exibe:

- Saudação personalizada ao usuário  
- Status de sincronização de dados  
- Cartão de integrações ativas  
- Resumo de uso com botões de ação  
- Carrossel de notícias  
- Ações rápidas com drag-and-drop  
- Histórico de atividades  
- (Opcional) Nota de uso e métricas adicionais  

Utiliza o layout base (`base.html`) para navbar, sidebar e scripts globais, e o CSS específico (`home.css`) para estilos desta página. Além disso, a biblioteca de ícones usada é a **Material Icons Outlined**.

---

## 2. Hierarquia de templates

```
client_app/app/templates/
|
├── base.html                  ← layout genérico (head, navbar, sidebar, scripts)
├── partials/
│   ├── navbar.html            ← barra de navegação superior
│   └── sidebar.html           ← menu lateral
└── home.html                  ← template da página Home :contentReference
```

## 3. Elementos do `home.html`

### 3.1 Cabeçalho de boas-vindas
- **Função:** Exibir saudação personalizada (“Bom dia, {{ username }}!”) e data/hora da última sincronização.  
- **Objetivo no sistema:** Criar conexão imediata com o usuário e informar que os dados estão atualizados.

### 3.2 Cartão de Integrações Ativas
- **Função:** Mostrar status geral das integrações (círculo verde para ativo).  
- **Objetivo no sistema:** Permitir ao usuário identificar rapidamente se todas as conexões de API/serviços estão funcionando e agir em caso de erro.

### 3.3 Card de Resumo de Uso
- **Função:** Apresentar três botões de ação rápida (por exemplo, pendências, relatórios, guias).  
- **Objetivo no sistema:** Servir como atalho para funcionalidades críticas do dia a dia, agilizando o fluxo de trabalho.

### 3.4 Seção de Notícias
- **Função:** Exibir slides com as últimas novidades e atualizações do sistema.  
- **Objetivo no sistema:** Manter o usuário informado sobre novas features, alertas de manutenção e comunicados importantes.

### 3.5 Ações Rápidas (Drag-and-Drop)
- **Função:** Fornecer atalhos reorganizáveis por meio de drag-and-drop.  
- **Objetivo no sistema:** Dar flexibilidade para o usuário ordenar suas ações preferidas e otimizar acesso constante.

### 3.6 Histórico de Atividades
- **Função:** Listar eventos recentes (sincronizações, alterações, acessos).  
- **Objetivo no sistema:** Permitir auditoria rápida e recuperação de contexto sobre o que aconteceu na conta.

---

## 4. Funções de JavaScript

### 4.1 Carrossel de Notícias
- **showSlide / prevSlide / nextSlide**  
- **Função no sistema:** Controlar a rotação de slides, exibindo uma notícia de cada vez e atualizando indicadores visuais, garantindo que o usuário não perca nenhuma novidade.

### 4.2 Drag-and-Drop de Ações Rápidas
- **Eventos principais:** `dragstart`, `dragover`, `drop`, `dragend`  
- **Função no sistema:** Permitir que o usuário arraste os botões de atalho para cima ou para baixo, reorganizando-os dinamicamente conforme sua preferência.

### 4.3 Inicialização dos Scripts
- **DOMContentLoaded**  
- **Função no sistema:** Adiar a execução de carrossel e drag-and-drop até que todo o HTML seja carregado, evitando erros de elementos não existentes e garantindo um comportamento estável.

> **Última atualização:** 02 jul 2025