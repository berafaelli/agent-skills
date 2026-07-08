---
name: spec-extrema
description: Aplica o método de especificação extrema — estratégia, SPEC-MASTER, specs de módulo no formato MODULE-STANDARD, contratos de interface, UX, plano de testes falseável, ataque adversarial em contexto limpo e congelamento com registro de decisões. Use quando for iniciar um sistema ou feature grande que exige spec completa antes de qualquer código, ou quando uma spec existente precisa ser elevada ao padrão extremo.
---

# Spec Extrema

## Overview

Especificação extrema é escrever a spec até o ponto em que outra pessoa (ou outro agente) consegue implementar o sistema sem tomar nenhuma decisão de produto — apenas decisões de implementação. O princípio central: **o que não está escrito não existe**. Uma lacuna na spec não é liberdade criativa para quem implementa; é um defeito da spec.

O método produz dois níveis de artefato:

- **SPEC-MASTER** — a spec do sistema: estratégia, escopo, decomposição em módulos, contratos entre eles, decisões congeladas.
- **SPEC-MODULO** (uma por módulo, formato MODULE-STANDARD) — responsabilidade, interface, dados, UX, modos de falha e critérios de aceite de cada módulo.

Os diferenciais do método estão nomeados e detalhados em [references/movimentos.md](references/movimentos.md). A régua de "quanto detalhe é suficiente" está em [references/proporcionalidade.md](references/proporcionalidade.md). O gate final usa as 25 perguntas de [references/checklist-25.md](references/checklist-25.md).

## When to Use

- Início de um sistema novo ou de uma feature que toca 3+ módulos
- Uma spec existente precisa de auditoria antes de virar base de implementação
- Trabalho que será implementado por outra pessoa ou por agentes em paralelo (contratos precisam estar fechados antes)
- Retomada de projeto parado, onde a spec é a única memória confiável

**Quando NÃO usar:** correções pontuais, mudanças de escopo autocontido, protótipos descartáveis. Para specs leves, use uma skill de spec convencional — este método cobra o custo da minúcia e só se paga quando o custo do retrabalho é maior.

## Process

Oito etapas, com gate entre elas — não avance com a etapa anterior aberta.

### 1. Estratégia

Invoque `spec-extrema:estrategia`. Antes de decompor qualquer coisa, a spec precisa saber por que existe: problema, por que agora, alternativas descartadas, fora de escopo, métrica única de sucesso. O resultado vira a primeira seção da SPEC-MASTER.

**Gate:** o humano confirma a estratégia. Estrutura em cima de estratégia errada é retrabalho garantido.

### 2. SPEC-MASTER

Preencha [templates/SPEC-MASTER.template.md](templates/SPEC-MASTER.template.md). Liste as suposições explicitamente antes de escrever ("estou assumindo X, Y, Z — corrija agora ou sigo com elas").

### 3. Decomposição em módulos

Para cada módulo identificado na SPEC-MASTER, crie uma spec no formato [templates/SPEC-MODULO.template.md](templates/SPEC-MODULO.template.md). Cada módulo tem uma responsabilidade descrita em uma frase sem "e". Dependências entre módulos formam um grafo sem ciclos.

### 4. Contratos de interface

Invoque `spec-extrema:contratos-de-interface` para cada fronteira entre módulos e cada API externa: formato de dados, erros, versionamento, modos de falha. Depois rode o agente `auditor-de-contratos` para a varredura par-a-par — os dois lados de cada fronteira precisam concordar por verificação, não por suposição.

### 5. UX

Invoque `spec-extrema:ux` para cada módulo com superfície de usuário: fluxos como passos numerados, matriz de estados (vazio, carregando, erro, sucesso), copy de erro com próxima ação, ações destrutivas com confirmação ou undo.

### 6. Plano de testes

Invoque `spec-extrema:testes-de-ferramenta`. Cada critério de aceite vira um teste falseável; cada contrato vira um teste de fronteira; cada modo de falha vira um teste de falha injetada. Uma spec cujos critérios não geram testes tem critérios mal escritos — volte e reescreva-os.

### 7. Ataque adversarial

Rode o agente `spec-adversario` em contexto limpo (subagente, sem o histórico de quem escreveu a spec). Mandato: encontrar no mínimo 3 lacunas, atacando fronteiras, modos de falha e suposições não escritas. Corrija as lacunas e rode de novo. **Teste de aceite:** a spec só avança quando um ataque em contexto limpo devolve menos de 3 lacunas novas relevantes.

### 8. Congelamento

Invoque `spec-extrema:congelamento`. Cada seção é marcada como **imutável** ou **aberta-com-dono**; toda decisão relevante entra no registro de decisões com as alternativas descartadas e o motivo. Depois do congelamento, mudança é uma nova entrada no registro — nunca edição silenciosa.

Antes de declarar a spec pronta, percorra as 25 perguntas de [references/checklist-25.md](references/checklist-25.md).

## Common Rationalizations

| Racionalização | Realidade |
|---|---|
| "Esse detalhe é óbvio, não precisa escrever" | Óbvio para quem escreveu. Quem implementa em outro contexto vai decidir diferente — e a spec não terá como arbitrar. |
| "O ataque adversarial é exagero, eu já revisei" | Quem escreveu a spec não consegue vê-la de fora. O viés de confirmação é o motivo de o ataque rodar em contexto limpo. |
| "Especificar modos de falha atrasa; tratamos quando acontecer" | Modo de falha não especificado vira decisão improvisada em produção. O custo só muda de lugar — e cresce. |
| "Congelar a spec engessa o projeto" | Congelamento não proíbe mudança; exige que mudança seja decisão registrada, com dono, em vez de deriva silenciosa. |
| "UX a gente resolve na implementação" | Estado vazio, erro e carregamento decididos no meio do código saem inconsistentes entre módulos. É spec, não estilo. |
| "25 perguntas é burocracia" | O checklist leva minutos e cada pergunta corresponde a uma classe real de retrabalho. Pular o checklist é aceitar essas classes de volta. |

## Red Flags

- Escrever código (ou esqueleto de código) antes da SPEC-MASTER congelada
- Fronteira entre módulos descrita só de um lado
- Critério de aceite que não dá para transformar em teste que falha
- Seção com "TBD" sem dono e sem prazo — isso é lacuna, não dívida
- Ataque adversarial encontrando lacunas e a spec sendo defendida em vez de corrigida
- Mudança pós-congelamento aplicada por edição direta, sem entrada no registro de decisões

## Verification

Antes de declarar a spec pronta para implementação:

- [ ] Seção de estratégia validada pelo humano (etapa 1)
- [ ] Todo módulo tem SPEC-MODULO no formato MODULE-STANDARD
- [ ] `auditor-de-contratos` rodou e todas as fronteiras concordam dos dois lados
- [ ] Matriz de estados de UX completa para todo módulo com superfície de usuário
- [ ] Plano de testes rastreável: cada critério de aceite aponta para um teste
- [ ] Último ataque do `spec-adversario` devolveu menos de 3 lacunas novas relevantes
- [ ] Toda seção marcada como imutável ou aberta-com-dono; registro de decisões preenchido
- [ ] As 25 perguntas do checklist respondidas — nenhuma resposta é "não sei"
