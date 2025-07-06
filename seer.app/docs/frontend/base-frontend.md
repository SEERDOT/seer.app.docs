
# 📘 `base.md` — Documentação de Elementos Base do Sistema

## 🧩 O que é uma Base no Sistema?

A base do sistema representa o **alicerce visual e estrutural** da aplicação. São os elementos que **se repetem em todas as telas**, formando a identidade da plataforma. A base define **padrões universais de estilo, layout, comportamento e interação**, como:

- Paleta de cores
- Estilo de botões
- Tipografia
- Espaçamentos
- Ícones e tooltips
- Cartões e hovers
- Layout de colunas
- Modais e feedbacks

Ela não carrega conteúdo funcional, mas garante **consistência, coesão e escalabilidade**.

---


## 🎨 1. Tipografia Padrão

A tipografia principal do sistema é:

```css
font-family: 'Exo 2', sans-serif;
```

Essa fonte moderna e legível é utilizada em **todos os textos da interface**, incluindo títulos, parágrafos, botões, inputs e tooltips. Ela transmite uma estética tecnológica, limpa e coerente com o propósito da plataforma.

---

## 🌈 2. Paleta de Cores

| Cor                | Código     | Função                                         |
|--------------------|------------|------------------------------------------------|
| Verde Primário     | `#23CE6B`  | Destaques, linhas ativas, feedback positivo    |
| Verde Escuro       | `#188C48`  | Hovers, seleções, textos de confirmação        |                    |
| Branco             | `#FFFFFF`  | Fundos de cartões, modais e inputs             |
| Cinza Claro        | `#FCFCFC`  | Fundos secundários                             |
| Cinza Médio        | `#969696`  | Bordas, placeholders, divisores                |
| Cinza Escuro       | `#272D2D`  | Títulos, textos principais                     |
| Vermelho Erro      | `#B3261E`  | Feedbacks de erro e alertas                    |
| Verde Claro Hover  | `#CDF8E8`  | Hovers em botões e cartões                     |

---

🖼️ 3. Layout e Estrutura Global
--------------------------------

*   **Cor de fundo global:** `#DFDFDF`
*   **Estrutura vertical flexível:** usando `flex-direction: column`
*   **Conteúdo centralizado:** com `.page-wrapper { max-width: 1400px; margin: 0 auto; }`
*   **Sidebar fixa à esquerda:** com margem de `270px` no conteúdo principal
*   **Padding padrão entre seções:** `20px a 30px`

---

🧱 4. Cartões (Cards) Reutilizáveis
-----------------------------------

Cards são usados em todas as páginas (Ex: home, painel, relatórios, onboarding). Padrões:

*   Fundo: `#fff`
*   Borda: `1px solid #969696` ou transparente
*   Radius: `8px`
*   Sombra leve: `box-shadow: 0 2px 4px rgba(0,0,0,0.1)`
*   Padding interno: `15px a 20px`

Cartões podem conter:

*   Títulos em negrito (font-size: 1rem ou 20px)
*   Ícones da Material Icons
*   Tooltips explicativos
*   Linhas para dividir conteúdo

---

## 🎛️ 5. Botões e Hovers

### Estilo Base de Botões

Todos os botões seguem um estilo minimalista com:

- Borda arredondada (`border-radius: 5px`)  
- Cor de fundo: geralmente transparente ou branca  
- Ícone + texto  
- Fonte: `Exo 2`, 15px, peso 600  

### Hover Padrão

Ao passar o mouse sobre os botões interativos:

- Fundo muda para `#CDF8E8` (verde claro)  
- Texto e ícone mudam para `#188C48` (verde escuro)  

---

💬 6. Tooltips e Ícones
-----------------------

**Tooltips** são mensagens explicativas que aparecem quando o usuário passa o mouse sobre um ícone de ajuda. Elas ajudam o usuário a entender a função de botões e seções sem poluir a interface.

Os **ícones** usados na aplicação seguem o padrão do Material Icons Outlined. Eles são visuais e comunicam a função de forma clara e intuitiva.

*   Tooltips flutuantes (`.tooltip`, `.custom-tooltip`) são leves e verdes
*   Ícones seguem padrão `material-icons-outlined`
*   Ícones de ajuda possuem `cursor: pointer` e cor `#626B6B`

---


🔧 7. Interações JavaScript Padrão
-----------------------------------

Scripts recorrentes controlam:

*   Abertura/fechamento de modais com overlay
*   Validações de formulário
*   Tooltips flutuantes ao passar o mouse
*   Submissão de formulários com botões "Confirmar" e "Cancelar"
*   Feedback de carregamento com barras de progresso
*   Alteração dinâmica de filtros, selects e tabs

---

✅ Conclusão
-----------

A base do sistema garante que todas as páginas e seções sigam uma **linguagem visual unificada**. Seguir esses padrões permite:

*   Redução de inconsistências
*   Aceleração do desenvolvimento
*   Experiência de uso mais intuitiva
*   Facilidade de manutenção e evolução futura
