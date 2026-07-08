---
name: congelamento
description: Fecha uma spec — classifica cada seção como imutável ou aberta-com-dono, preenche o registro de decisões com alternativas descartadas e estabelece o processo de mudança pós-congelamento. Use como etapa final do método spec-extrema, ou isoladamente para congelar uma spec existente que ficou madura.
---

# Congelamento

## Overview

Uma spec que muda silenciosamente não é fonte de verdade — é rascunho permanente. O congelamento transforma a spec em base confiável de implementação: cada seção ganha um status explícito (**imutável** ou **aberta-com-dono**), cada decisão relevante ganha uma entrada no registro com as alternativas descartadas, e mudança passa a ser evento registrado em vez de deriva. Congelar não proíbe mudar; proíbe mudar sem dono, sem motivo escrito e sem que os afetados saibam.

## When to Use

- Etapa 8 do método `spec-extrema`, depois de o ataque adversarial passar no teste das 3 lacunas
- Isoladamente: uma spec existente estabilizou e vai virar base de implementação paralela
- Um projeto sofre de "spec drift" — o documento diz uma coisa, o time decide outra em conversas

**Quando NÃO usar:** specs ainda em ataque adversarial ativo (congelar com lacunas conhecidas só as esconde), ou protótipos onde a spec é descartável por definição.

## Process

### 1. Pré-condições

Verifique antes de começar: último ataque do `spec-adversario` devolveu menos de 3 lacunas novas relevantes; tabela de concordância do `auditor-de-contratos` sem divergências; checklist das 25 sem "não sei". Congelamento com pré-condição pendente é carimbo, não fechamento.

### 2. Classificação seção a seção

Percorra toda seção da SPEC-MASTER e de cada SPEC-MODULO e marque:

- **IMUTÁVEL** — mudar isso exige nova entrada no registro de decisões e aviso aos afetados. Obrigatório para tudo que a régua de proporcionalidade classifica como minúcia obrigatória: contratos, dados persistidos, permissões, dinheiro, ações destrutivas.
- **ABERTO — dono: [nome] — decide até: [etapa/data]** — legítimo apenas para áreas de dívida aceitável. O dono é uma pessoa, não um time; o prazo é uma etapa ou data, não "depois".

Seção sem marca é lacuna. "TBD" órfão reprova (pergunta 24 do checklist).

### 3. Registro de decisões

Para cada decisão estrutural tomada durante a spec (decomposição escolhida, alternativas de estratégia descartadas, formato de contrato, política de versionamento), uma entrada: número, data, decisão, **alternativas descartadas**, motivo, dono. As alternativas descartadas são a parte mais valiosa — são o que impede o time de re-litigar a mesma decisão a cada trimestre.

### 4. Processo de mudança pós-congelamento

Escreva na SPEC-MASTER o protocolo, curto:

1. Mudança em seção IMUTÁVEL → nova entrada no registro (nunca edição da entrada antiga), com motivo e o que invalidou a decisão anterior
2. Quem é afetado pelo contrato/seção é avisado antes de a mudança valer
3. Se a mudança toca contrato, o `auditor-de-contratos` roda de novo nos pares afetados
4. Item ABERTO decidido → vira IMUTÁVEL com entrada no registro; prazo estourado sem decisão → escala para o dono da spec

### 5. Carimbo

Atualize o status da SPEC-MASTER para CONGELADA, com data e a rodada de ataque que passou. A partir daqui, implementação pode começar — e qualquer pergunta sem resposta na spec volta como correção de spec, não como decisão local.

## Common Rationalizations

| Racionalização | Realidade |
|---|---|
| "Congelar engessa; requisitos mudam" | O protocolo de mudança existe exatamente para isso. O que o congelamento elimina é a mudança que ninguém decidiu. |
| "Registrar alternativas descartadas é retrabalho" | É o único registro que impede re-litigar decisões. A alternativa é rediscutir o mesmo tema a cada pessoa nova. |
| "Deixa essa seção aberta, decidimos na implementação" | Aberta com dono e prazo, sim — se for área de dívida aceitável. Contrato e dado persistido, nunca. |
| "O time é pequeno, todo mundo sabe o que foi decidido" | O registro não é para o time de hoje; é para o agente da próxima sessão e a pessoa do próximo trimestre. |

## Red Flags

- Congelamento com ataque adversarial pendente ou reprovado
- Seção de contrato marcada como ABERTO
- Entrada antiga do registro de decisões editada em vez de nova entrada criada
- Item ABERTO com prazo estourado e sem escalonamento
- Spec CONGELADA divergindo do que está sendo implementado — e a implementação ganhando

## Verification

- [ ] Pré-condições verificadas: ataque < 3 lacunas, contratos concordantes, checklist sem "não sei"
- [ ] Toda seção de master e módulos marcada IMUTÁVEL ou ABERTO-com-dono-e-prazo
- [ ] Nenhuma área de minúcia obrigatória marcada como ABERTO
- [ ] Registro de decisões com alternativas descartadas e motivo em toda entrada
- [ ] Protocolo de mudança pós-congelamento escrito na SPEC-MASTER
- [ ] Status CONGELADA com data e rodada de ataque registradas
