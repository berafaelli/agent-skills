---
name: testes-de-ferramenta
description: Deriva da spec um plano de testes falseável e rastreável — cada critério de aceite vira teste, cada contrato vira teste de fronteira dos dois lados, cada modo de falha vira teste de falha injetada, e os fluxos de UX viram roteiro de smoke manual. Use na etapa de testes do método spec-extrema, ou isoladamente quando uma ferramenta existente não tem plano de testes rastreável à spec.
---

# Testes de Ferramenta

## Overview

O plano de testes é a prova de que a spec é falseável. Esta skill não escreve os testes — deriva da spec o plano completo e rastreável: qual teste cobre qual critério, qual contrato, qual modo de falha. O efeito colateral mais valioso é diagnóstico: **um critério de aceite que não gera teste é um critério mal escrito**, e a correção acontece na spec, não no plano. O plano preenche a seção 6 da SPEC-MASTER.

## When to Use

- Etapa 6 do método `spec-extrema`, com contratos e UX já fechados
- Isoladamente: uma ferramenta existente tem testes, mas ninguém sabe o que eles provam
- Antes de paralelizar implementação — o plano define o que cada módulo deve provar ao entregar
- Uma spec vai ser congelada e falta a verificação de falseabilidade (pergunta 21 do checklist)

**Quando NÃO usar:** para escrever a implementação dos testes — isso é trabalho da fase de build (TDD), guiado por este plano.

## Process

### 1. Critérios de aceite → testes

Percorra todo critério de aceite (SPEC-MASTER e cada SPEC-MODULO). Para cada um, escreva a linha do plano: o teste que **falharia hoje** e passará quando o critério for verdade. Se não conseguir escrever essa linha, o critério não é falseável — volte à spec e reescreva o critério ("busca rápida" → "busca retorna em < 500ms com 10.000 registros"). Nenhum critério fica sem linha.

### 2. Contratos → testes de fronteira, dos dois lados

Para cada contrato do inventário: um teste do lado que expõe (produz exatamente o formato declarado — tipos, nulabilidade, limites) e um do lado que consome (aceita o formato declarado e rejeita violações). Testar só um lado é como o drift de contrato passa despercebido — os dois lados verdes contra o **mesmo texto do contrato** é a versão executável da tabela de concordância do `auditor-de-contratos`.

### 3. Modos de falha → testes de falha injetada

Para cada modo de falha especificado (timeout, indisponibilidade, dado inválido, permissão negada): um teste que injeta a falha e verifica o comportamento especificado — o que os dependentes veem, o que o usuário vê, se o retry/fallback acontece como escrito. Se a spec diz "timeout de 5s com 2 retries", existe um teste que prova isso. Modo de falha sem teste é modo de falha decorativo.

### 4. Fluxos de UX → roteiro de smoke manual

Os fluxos numerados da seção UX viram, quase literalmente, o roteiro de smoke da ferramenta real: passos, resultado esperado por passo, e os quatro estados da matriz verificados onde o fluxo passa por eles. Este roteiro é executado contra a ferramenta de verdade (CLI, UI, API) antes de cada release — automatize o que der, mas o roteiro existe mesmo enquanto for manual.

### 5. Tabela de rastreabilidade

Monte a seção 6 da SPEC-MASTER: cada linha liga critério → teste → tipo (contrato / falha injetada / fluxo / smoke manual). Depois inverta a leitura: existe algum teste planejado que não rastreia a nada na spec? Ou ele prova algo que deveria estar escrito (lacuna na spec) ou não prova nada (corte-o).

### 6. Proporcionalidade do plano

O plano herda a régua de `proporcionalidade.md`: contratos, dados persistidos, dinheiro e ações destrutivas têm cobertura obrigatória, incluindo casos de borda. Áreas de dívida assumida podem ter só o smoke — desde que isso esteja declarado no plano com dono.

## Common Rationalizations

| Racionalização | Realidade |
|---|---|
| "Plano de testes antes do código é papelada; escrevo os testes direto" | Sem o plano, os testes cobrem o que foi fácil testar, não o que a spec promete. A rastreabilidade é o produto. |
| "Testar os dois lados do contrato é duplicação" | É triangulação. Um lado verde contra suposição errada é exatamente o bug que fronteiras produzem. |
| "Falha injetada é sofisticação prematura" | Se a spec especificou o modo de falha (e especificou), ou ele é testável ou é ficção. |
| "Smoke manual não é teste de verdade" | É o único que exercita a ferramenta real de ponta a ponta. Roteirizado e rastreável à spec, vale mais que cobertura de unidade cega. |

## Red Flags

- Critério de aceite sem linha correspondente no plano
- Contrato testado só do lado de quem expõe
- Plano cobrindo apenas caminho feliz — nenhuma linha do tipo "falha injetada"
- Teste planejado que não rastreia a nenhuma seção da spec
- "Cobertura de X%" usada como critério no lugar de rastreabilidade

## Verification

- [ ] Todo critério de aceite (master e módulos) tem teste que falharia hoje
- [ ] Todo contrato tem teste dos dois lados contra o mesmo texto
- [ ] Todo modo de falha especificado tem teste de falha injetada
- [ ] Roteiro de smoke manual cobre todos os fluxos de UX numerados
- [ ] Tabela de rastreabilidade sem órfãos nas duas direções
- [ ] Critérios não-falseáveis encontrados foram corrigidos na spec, não contornados no plano
