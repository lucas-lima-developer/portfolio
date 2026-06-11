---
title: 'Como reduzi custos de banco de dados em 90% com cache distribuído no Redis'
description: 'Um problema real de escalabilidade, como eu identifiquei o gargalo e a solução que projetei usando Redis na AWS.'
pubDate: '2026-06-01'
---

Esse post é sobre um problema real que resolvi na Acesso Ponto Tech. A empresa tinha acabado de ganhar uma licitação de grande porte — passamos de um sistema que servia alguns hospitais para um que precisava suportar 80 mil colaboradores em 40 unidades, com 800 terminais integrados em tempo real.

O problema apareceu rápido.

## O gargalo

Com o crescimento repentino da base, os custos do banco de dados foram às alturas. Cada requisição dos terminais de ponto — registros de entrada, saída, pausas — batia diretamente no banco. Com 800 dispositivos sincronizando em tempo real, o volume de reads era absurdo.

O padrão que encontrei ao analisar os logs era claro: a maior parte das queries buscavam dados que raramente mudavam. Configurações de jornada, dados de colaboradores, regras de turno. Informações que eram consultadas centenas de vezes por minuto mas atualizadas raramente.

## A solução

A decisão foi implementar uma camada de cache distribuído com **Redis** na frente do banco principal (MySQL no RDS da AWS).

A estratégia foi simples:

1. **Cache-aside**: a aplicação primeiro consulta o Redis. Se o dado existe, retorna direto. Se não, busca no banco e armazena no cache com TTL configurado por tipo de dado.
2. **TTLs estratégicos**: dados de configuração de jornada com TTL longo (horas). Dados de presença em tempo real com TTL curto (segundos).
3. **Invalidação seletiva**: ao atualizar um colaborador ou regra de turno, invalidamos apenas as chaves afetadas — sem flush geral que derruba tudo.

O código ficou mais ou menos assim no NestJS:

```typescript
async getEmployeeConfig(employeeId: string) {
  const cacheKey = `employee:config:${employeeId}`;
  
  const cached = await this.redis.get(cacheKey);
  if (cached) return JSON.parse(cached);

  const config = await this.employeeRepository.findConfig(employeeId);
  await this.redis.setex(cacheKey, 3600, JSON.stringify(config));
  
  return config;
}
```

## O resultado

A queda nos custos foi imediata. Em uma semana de rollout gradual, o volume de queries chegando ao RDS caiu em ~90%. O banco passou a ser consultado principalmente para escritas e para cache misses, que viraram uma fração pequena do tráfego total.

Além da economia, o tempo de resposta das APIs caiu bastante — Redis em memória é ordens de magnitude mais rápido que qualquer banco relacional para leitura.

## O que aprendi

Cache resolve muita coisa, mas tem um custo cognitivo: você agora tem dois lugares onde os dados vivem. A invalidação precisa ser pensada com cuidado. No início errei algumas chaves de invalidação e tive dados desatualizados aparecendo nos terminais — foi rápido de identificar pelo Grafana, mas serviu de lição.

A regra que ficou: para cada dado que entra no cache, eu defino explicitamente *quando* ele sai. Seja por TTL, seja por evento de atualização. Nunca cache sem estratégia de saída.
