---
title: 'Sistema de Gestão de Ponto em Tempo Real'
description: 'Plataforma de controle de jornada para 80 mil colaboradores em 40 hospitais, com 800 terminais integrados via arquitetura escalável na AWS.'
coverImage: '../../assets/blog-placeholder-1.jpg'
github: 'https://github.com/lucas-lima-developer'
tech: ['NestJS', 'Next.js', 'React Native', 'Redis', 'MySQL', 'ElasticSearch', 'Docker', 'AWS']
pubDate: '2024-08-01'
status: 'active'
---

Sistema desenvolvido na Acesso Ponto Tech após a empresa ganhar uma licitação de grande porte com o governo estadual. O maior desafio foi escalar uma plataforma que precisava suportar 80 mil colaboradores distribuídos em 40 unidades hospitalares, com 800 relógios de ponto integrados em tempo real.

## Desafios técnicos

O sistema original não foi projetado para essa escala. Com o crescimento repentino, apareceram gargalos críticos de CPU e memória RAM na sincronização com os terminais, além de custos de banco de dados fora do controle.

## O que foi construído

- **API principal em NestJS** com arquitetura modular, filas Redis para absorver picos de requisições e cache distribuído que reduziu 90% dos custos do RDS MySQL.
- **Integração em tempo real** com 800 terminais via protocolo proprietário, com reconexão automática e sync de dados offline.
- **App mobile em React Native** para supervisores acompanharem presença e jornada em tempo real.
- **Busca com Elasticsearch** para consultas em dados governamentais de colaboradores, reduzindo tempo de resposta para milissegundos.
- **Observabilidade completa** com Grafana + Prometheus, dashboards de saúde da infraestrutura em tempo real.
- **Pipeline CI/CD** com GitHub Actions em servidores VPS, com zero downtime deploy.

## Infraestrutura AWS

EC2 para os serviços principais, RDS MySQL para dados transacionais, DynamoDB para dados de alta frequência, S3 para arquivos e relatórios, SES para notificações, Secrets Manager para gerenciamento de credenciais.
