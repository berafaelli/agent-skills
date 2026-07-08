---
name: ux
description: Especifica a experiência de usuário de um módulo — fluxos como passos numerados, matriz de estados (vazio, carregando, erro, sucesso), copy de erro com próxima ação e ações destrutivas com confirmação ou undo. Use na etapa de UX do método spec-extrema, ou isoladamente para especificar ou auditar a UX de um módulo ou tela existente.
---

# UX

## Overview

UX na spec extrema não é design visual — é comportamento observável pelo usuário, especificado antes do código. As decisões que saem inconsistentes quando tomadas "durante a implementação" são sempre as mesmas: o que aparece antes de existir dado, o que aparece enquanto carrega, o que aparece quando falha, e o que protege o usuário de si mesmo em ações destrutivas. Esta skill preenche a seção UX da SPEC-MODULO (formato MODULE-STANDARD) e os padrões transversais da SPEC-MASTER.

## When to Use

- Etapa 5 do método `spec-extrema`, para cada módulo com superfície de usuário
- Isoladamente: "especifica a UX do [fluxo X]" em sistema novo ou existente
- Auditoria: uma tela existente tem estados faltando ou copy de erro que não ajuda
- Antes de entregar implementação de UI a outra pessoa ou agente

**Quando NÃO usar:** módulos sem superfície de usuário (escreva "sem superfície de usuário" na SPEC-MODULO e siga); escolhas puramente estéticas (cor, tipografia) — isso é design system, não spec.

## Process

### 1. Fluxos como passos numerados

Cada fluxo é uma sequência verificável: `1. usuário faz X → 2. sistema responde Y → 3. …`. Não descreva telas ("a tela de configurações tem…"); descreva travessias. Teste de qualidade: outra pessoa consegue executar o fluxo como roteiro de teste manual sem perguntar nada — este é o formato que `testes-de-ferramenta` consome direto.

### 2. Matriz de estados

Para cada tela ou passo de fluxo, os quatro estados em tabela:

| Estado | Pergunta que responde |
|---|---|
| **Vazio** | O que o usuário vê antes de existir qualquer dado? Primeira visita conta como vazio. |
| **Carregando** | O que aparece durante a espera? A partir de quanto tempo muda (skeleton → mensagem)? |
| **Erro** | O que aparece quando falha? (Ligado aos modos de falha dos contratos do módulo.) |
| **Sucesso** | O estado normal, com dados — inclusive nos extremos: 1 item e 10.000 itens. |

Célula vazia na matriz é lacuna, não liberdade. O estado vazio é o mais esquecido e o primeiro que todo usuário novo vê.

### 3. Copy de erro com próxima ação

Toda mensagem de erro em tabela: situação, mensagem, **próxima ação do usuário**. A regra: a mensagem diz o que o usuário faz a seguir, não o que o sistema sentiu ("erro inesperado" reprova; "não conseguimos conectar ao Radar — tente de novo em instantes ou verifique sua conexão" passa). Copy de erro é minúcia obrigatória; copy do caminho feliz pode ser dívida com dono.

### 4. Ações destrutivas

Inventário de tudo que apaga, sobrescreve, publica ou notifica terceiros. Para cada uma: confirmação (proporcional ao dano — apagar 1 item ≠ apagar tudo) ou undo (prazo e alcance), e rastro de auditoria. Ação destrutiva sem proteção especificada reprova no checklist (pergunta 18).

### 5. Padrões transversais

O que se repete entre módulos sobe para a seção "UX transversal" da SPEC-MASTER: o padrão dos quatro estados, o padrão de copy de erro, o padrão de confirmação. Módulos herdam o padrão e só especificam o desvio — é assim que a consistência sobrevive à implementação paralela.

## Common Rationalizations

| Racionalização | Realidade |
|---|---|
| "Estados de erro e vazio a gente resolve na implementação" | Resolvidos um a um no código, saem diferentes em cada módulo. É o defeito de consistência mais visível ao usuário. |
| "O designer decide isso depois" | Estados e copy de erro são comportamento, não estética. Se ficam para depois, ficam para nunca. |
| "É só um CRUD, não precisa de spec de UX" | Todo CRUD tem vazio, carregando, erro, e delete. São exatamente as quatro coisas que esta skill especifica. |
| "Especificar 10.000 itens é paranoia" | O extremo de volume decide paginação, busca e performance percebida. Descobrir na produção custa redesign. |

## Red Flags

- Fluxo descrito como lista de telas em vez de passos numerados
- Matriz de estados com células em branco sem dono
- Mensagem de erro que descreve o sistema em vez de orientar o usuário
- Ação destrutiva sem confirmação, undo nem auditoria especificados
- O mesmo padrão (ex.: estado de carregando) especificado diferente em dois módulos sem justificativa

## Verification

- [ ] Todo fluxo executável como roteiro de teste manual por alguém que não participou da spec
- [ ] Matriz de estados completa (vazio, carregando, erro, sucesso) para cada tela/passo
- [ ] Estado vazio da primeira visita especificado
- [ ] Toda copy de erro tem próxima ação do usuário
- [ ] Todo item do inventário de ações destrutivas tem confirmação ou undo
- [ ] Padrões repetidos promovidos à SPEC-MASTER; módulos especificam só desvios
