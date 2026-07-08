---
name: estrategia
description: Escreve a camada estratégica de uma spec — problema, por que agora, alternativas descartadas, fora de escopo, métrica única de sucesso e riscos. Use quando for iniciar uma SPEC-MASTER, ou quando uma spec existente não sabe responder por que o sistema existe e o que declararia sucesso.
---

# Estratégia

## Overview

Nenhuma decomposição antes de a spec saber por que existe. Esta skill produz a seção "Estratégia" da SPEC-MASTER: cinco respostas curtas e um bloco de riscos. O objetivo não é um documento de negócio — é impedir que estrutura seja desenhada em cima de uma aposta não examinada. Estrutura em cima de estratégia errada otimiza para o problema errado com precisão.

## When to Use

- Etapa 1 do método `spec-extrema` (sempre)
- Uma spec existente será retomada e ninguém lembra por que as decisões foram tomadas
- Duas propostas de sistema competem e falta uma base para compará-las
- Alguém pede "só uma feature" que na verdade é uma aposta estratégica disfarçada

**Quando NÃO usar:** mudanças dentro de escopo já validado — a estratégia já existe; não a reescreva a cada tarefa.

## Process

### 1. Problema em uma frase

Escreva o problema como um usuário real o descreveria — sem nome de solução dentro. Teste: mostre a frase a alguém do público-alvo (ou simule); se a reação não for "sim, isso me acontece", reescreva.

### 2. Por que agora

O custo de **não** construir, por escrito: o que continua quebrado, quanto custa por mês, o que a concorrência faz nesse meio-tempo. Se não houver custo de esperar, a resposta honesta pode ser "não é agora" — e a spec para aqui, barata.

### 3. Alternativas descartadas

No mínimo três, avaliadas de verdade: **comprar** (existe SaaS/ferramenta pronta?), **adaptar** (algo interno já cobre 80%?), **não fazer** (viver com o problema). Para cada uma: por que foi descartada, em uma frase falseável ("descartada porque X" onde X pode ser verificado). Este bloco alimenta o registro de decisões no congelamento.

### 4. Fora de escopo

Lista explícita do que a v1 **não** faz. Regra prática: se durante a escrita alguém disse "seria bom também…", esse item entra aqui com nome. Fora de escopo sem lista vira escopo por acreção.

### 5. Métrica única de sucesso

Uma métrica, um número-alvo, um horizonte de tempo. "Melhorar a visibilidade competitiva" não é métrica; "80% dos lançamentos de concorrentes detectados em até 48h, medido no primeiro trimestre" é. Se a métrica não pode ser medida com o que existe, a instrumentação dela entra na spec como minúcia obrigatória.

### 6. Riscos estratégicos

O que, se for verdade, invalida a aposta inteira — não riscos de execução ("pode atrasar"), riscos de tese ("usuários não confiam em dados coletados automaticamente"). Cada risco com um sinal observável que o confirmaria cedo.

### 7. Gate humano

Apresente as seis respostas ao humano em bloco único e peça validação explícita antes de a SPEC-MASTER continuar. Este é o gate mais barato do método inteiro.

## Common Rationalizations

| Racionalização | Realidade |
|---|---|
| "A estratégia é óbvia, todo mundo sabe por que estamos fazendo isso" | "Todo mundo sabe" nunca sobrevive a escrever. As divergências aparecem na primeira frase. |
| "Isso é trabalho de produto, não de spec" | A spec vai congelar decisões derivadas da estratégia. Sem ela escrita, o congelamento não tem base. |
| "Analisar comprar/adaptar atrasa o projeto" | Uma hora avaliando alternativas é mais barata que um trimestre construindo o que existia pronto. |
| "Métrica a gente define depois do lançamento" | Depois do lançamento a métrica vira justificativa do que foi feito, não critério do que fazer. |

## Red Flags

- Problema formulado com o nome da solução dentro ("precisamos de um dashboard de X")
- "Por que agora" respondido com "porque foi pedido"
- Alternativas descartadas sem motivo falseável ("não serve para nós")
- Métrica de sucesso plural, vaga ou sem prazo
- Decomposição em módulos começando antes do gate humano desta etapa

## Verification

- [ ] Problema em uma frase, sem nome de solução, reconhecível pelo usuário
- [ ] Custo de não construir escrito
- [ ] Três ou mais alternativas descartadas com motivo falseável
- [ ] Lista explícita de fora de escopo da v1
- [ ] Uma métrica, um alvo, um horizonte — e instrumentação viável
- [ ] Riscos de tese com sinal observável de confirmação
- [ ] Validação explícita do humano registrada antes da etapa 2
