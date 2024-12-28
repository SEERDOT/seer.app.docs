# Introdução

Esta documentação fornece uma visão geral da arquitetura do _seer.app_, diretrizes e processos de desenvolvimento e tem como **propósito central** auxiliar na construção de uma aplicação organizada.

---

## Guia de Início Rápido

Clone o repositório:

```bash
https://github.com/SEERDOT/seer.app.git
```

Acesse o ambiente virtual (venv):

```bash
source seer_env/bin/activate
```

Execute o servidor de desenvolvimento:

```bash
python3 manage.py runserver
```

Para mais informações, contate alguém para suporte.

---

## Estrutura do Projeto

```
├── client_app/        # Aplicação Privada [Tenant-Specific] (App)
├── public_app/        # Diretório da Aplicação Pública (Login/Registro)
├── seer_app/          # Diretório do Projeto Geral
├── seer_env/          # Ambiente Virtual com Bibliotecas
├── README.md/         # Guia básico de inicialização
├── manage.py          # Ponto de entrada do CLI do Django
└── requirements.txt   # Dependências do Python
```

---

## Tecnologias Principais

- **Frontend**: Bootstrap, ApexCharts
- **Backend**: Django
- **Banco de Dados**: PostgreSQL
- **Deploy**: -

---

## Contatos e Recursos

- **Responsável**: [Raphael Suarez](mailto:raphael.suarez@seerdot.com)
- **Repositório**: [GitHub Link](https://github.com/SEERDOT/seer.app)