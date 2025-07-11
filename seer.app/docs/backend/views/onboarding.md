# Documentação das Views de Onboarding

Este documento reúne a documentação completa das views relacionadas ao processo de onboarding da plataforma multi-tenant. As views estão organizadas por etapa de execução e seguem o padrão adotado no projeto.

---

## Etapa 1: Identificação e Perfil da Empresa

**getNameRole**

Responsável por capturar o nome e o papel do representante da empresa.

- Modelos: `TenantProfile`
- Template: `client_app/app/onboarding/step1/getNameRole.html`
- Redireciona para: `getCompanyInfos`

---

**getCompanyInfos**

Coleta informações organizacionais como segmento, área de atuação, porte e maturidade em dados.

- Modelos: `TenantProfile`, `HeadCountChoices`, `DataMaturityLevelChoices`
- Template: `client_app/app/onboarding/step1/getCompanyInfos.html`
- Redireciona para: `letsProceedOnboarding`

---

**letsProceedOnboarding**

Tela de confirmação que resume os dados fornecidos anteriormente antes da transição para a etapa 2.

- Modelos: `TenantProfile`
- Template: `client_app/app/onboarding/step1/letsProceedOnboarding.html`

---

## Etapa 2: Objetivos e Desafios

**getGoal**

Define o objetivo principal da empresa usando slugs convertidos em texto legível.

- Modelos: `TenantProfile`
- Template: `client_app/app/onboarding/step2/getGoal.html`
- Redireciona para: `dataChallenges`

---

**dataChallenges**

Permite ao usuário selecionar os desafios enfrentados pela empresa.

- Modelos: `TenantProfile`, `TenantChallenge`
- Template: `client_app/app/onboarding/step2/dataChallenges.html`
- Redireciona para: `letsIntegrate`

---

**chooseYourChallenges**

Alternativa visual para seleção de desafios, usando SLUG_LABELS.

- Modelos: `TenantChallenge`
- Template: `client_app/app/onboarding/step2/chooseYourChallenges.html`
- Redireciona para: `letsIntegrate`

---

## Etapa 3: Integração

**letsIntegrate**

Resumo da meta principal e dos desafios selecionados. Prepara para a integração com a plataforma.

- Modelos: `TenantProfile`, `TenantChallenge`
- Template: `client_app/app/onboarding/step3/letsIntegrate.html`

---

**letsIntegrate2**

Versão alternativa da tela anterior sem exibição da meta principal.

- Modelos: `TenantChallenge`
- Template: `client_app/app/onboarding/step3/letsIntegrate2.html`

---

**ferramentasGestao**

Exibe uma lista de ferramentas de gestão por categoria, com nome, imagem, id e tags.

- Categorias e ferramentas são definidas localmente via listas de dicionários
- Exibe dados como: nome, imagem estática, id e tags por ferramenta
- Template: `client_app/app/onboarding/step3/ferramentasGestao.html`

---

## Etapa 4: Planos

**planos**

Analisa os dados coletados e recomenda um plano adequado (Básico ou Pro).

- Modelos: `TenantProfile`, `TenantChallenge`
- Template: `client_app/app/onboarding/step4/planos.html`

---

**recomendacao_plano**

Renderiza uma tela visual com os dados e desafios selecionados da empresa.

- Modelos: `TenantProfile`, `TenantChallenge`
- Template: `client_app/app/onboarding/step4/recomendacao_plano.html`

---

## Considerações Finais

O conjunto de views documentado aqui forma um fluxo coeso e completo para onboarding de empresas em um sistema multi-tenant. Cada etapa foi planejada para coletar dados de forma segmentada e contextualizada, facilitando a personalização de soluções com base no perfil, dores e objetivos de cada cliente. O uso consistente do `schema_context` garante o isolamento seguro de dados, e a estrutura modular permite evolução futura com facilidade.