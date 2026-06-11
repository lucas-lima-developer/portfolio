---
title: 'API de Chatbot para Crédito Estudantil'
description: 'API backend que servia o chatbot de IA de uma das maiores plataformas de crédito estudantil do Brasil, atendendo centenas de milhares de estudantes em 500+ instituições.'
coverImage: '../../assets/blog-placeholder-2.jpg'
tech: ['Node.js', 'Express', 'MongoDB', 'RabbitMQ', 'Jest', 'Grafana', 'Docker', 'AWS']
pubDate: '2022-04-01'
status: 'archived'
---

Desenvolvido durante minha passagem pela Mundiale, esse projeto foi a API central de um produto de IA conversacional para crédito estudantil — um dos maiores do Brasil na época. O chatbot tirava dúvidas, gerava documentos, entregava boletos e agendava serviços para estudantes em mais de 500 instituições de ensino.

## Responsabilidades

Fui o desenvolvedor responsável pela API Node.js que servia como backend do chatbot. O sistema precisava ser estável, rápido e escalável — qualquer downtime impactava diretamente estudantes no meio de processos burocráticos.

## O que foi construído

- **API REST em Node.js + Express** com arquitetura orientada a domínio, separação clara de responsabilidades.
- **Filas com RabbitMQ** para processamento assíncrono de geração de documentos e envio de boletos, desacoplando operações pesadas do fluxo principal.
- **Banco de dados MongoDB** com modelagem otimizada para as consultas mais frequentes do chatbot.
- **Cobertura de testes de 98%** com Jest, Husky para hooks de qualidade e SonarQube para análise estática — atingimos nível A+ de qualidade de código.
- **Observabilidade com Grafana** monitorando latência, taxa de erros e saúde geral do sistema em produção.

## Qualidade de código

Esse projeto foi onde desenvolvi minha cultura de testes. A meta de 98% de cobertura não era burocracia — era a garantia de que eu podia fazer deploys com confiança. Cada funcionalidade nova vinha com testes unitários e de integração que validavam o comportamento real do sistema.
