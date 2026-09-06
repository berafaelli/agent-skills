---
name: congelamento
description: Fecha uma especificação particionando tudo em imutável vs. aberto-com-dono-e-prazo, com log de decisões e critério de reabertura. Use quando o pedido for "fechar a spec", "congelar o escopo" ou ao executar a etapa 5 da skill spec-extrema. NÃO use em rascunhos iniciais que ainda estão sendo explorados.
---

# Congelamento

## Visão Geral

Uma spec sem congelamento nunca termina: cada leitor trata cada seção como negociável, e as
decisões são relitigadas até na implementação. Congelar é particionar explicitamente tudo o
que a spec afirma em duas categorias — **imutável** (mudar exige reabrir a spec formalmente)
e **aberto** (ainda sem decisão, mas com dono e prazo) — e registrar cada decisão com a
alternativa que perdeu. O congelamento transforma a spec de documento vivo demais em base
estável para derivar trabalho.

## Quando Usar

- Executando a etapa 5 da skill `spec-extrema`, após o passe adversarial.
- Pedido isolado: "fechar a spec", "congelar o escopo", "essa spec não para de mudar".
- Antes de derivar specs de módulo ou iniciar implementação de qualquer parte.

**Quando NÃO usar:**
- Em rascunho inicial ainda em exploração — congelar cedo demais mata alternativas boas.
- Como arma de escopo ("está congelado" para evitar discussão legítima) — o critério de
  reabertura existe exatamente para isso.

## Processo

### 1. Varrer decisões implícitas

Percorra a spec inteira procurando afirmações que são decisões disfarçadas de descrição
("os dados ficam no banco X", "a autenticação usa Y"). Cada uma vira entrada explícita na
partição — decisão implícita é a que ninguém defende e todos revertem.

### 2. Classificar: imutável vs. aberto

Para cada decisão e cada seção:

- **Imutável:** já decidida, sustentada por motivo registrado, e cara de reverter depois
  que o trabalho derivar dela.
- **Aberto:** ainda sem decisão final. Aberto não é vago — é uma pergunta específica
  aguardando resposta.

### 3. Todo aberto ganha dono, prazo e impacto

| Item aberto | Dono | Prazo de decisão | Impacto do atraso |
|---|---|---|---|

O impacto do atraso responde: o que fica bloqueado se o prazo estourar? Item aberto sem
bloqueio declarado provavelmente nem precisava estar na spec.

### 4. Log de decisões com alternativa rejeitada

Para cada imutável:

| Decisão | Alternativa rejeitada | Motivo | Data |
|---|---|---|---|

A alternativa rejeitada é obrigatória: é ela que impede a relitigação ("já consideramos X,
perdeu por este motivo") e a reversão silenciosa.

### 5. Critério de reabertura

Escreva o que justifica descongelar um imutável — por exemplo: requisito novo que invalida o
motivo registrado, falha do passe adversarial em produção, mudança externa (API deprecada).
Sem critério escrito, "imutável" derrete na primeira pressão de prazo; com critério, a
exceção tem processo em vez de ser precedente.

## Saída

As seções §9 (Log de Decisões) e §10 (Congelados e Abertos) da SPEC-MASTER preenchidas — ou,
em spec avulsa, um bloco "Congelamento" no final do documento com as mesmas tabelas.

## Racionalizações Comuns

| Racionalização | Realidade |
|---|---|
| "Deixa aberto, decidimos quando chegar lá" | Sem dono e prazo, "quando chegar lá" significa "no meio da implementação, pela pessoa com menos contexto". |
| "Não precisa registrar o motivo, todo mundo sabe" | Todo mundo de hoje. O motivo não registrado morre na primeira troca de equipe. |
| "Registrar a alternativa rejeitada é burocracia" | É o único mecanismo barato contra relitigar a mesma decisão a cada reunião. |
| "Congelar engessa o projeto" | O critério de reabertura existe para mudanças legítimas. O que o congelamento engessa é a mudança sem motivo. |

## Sinais de Alerta

- Item aberto sem dono ou sem prazo.
- Decisão no log sem alternativa rejeitada ("decidimos usar X" — contra o quê?).
- Spec "congelada" sendo alterada sem passar pelo critério de reabertura.
- Mais itens abertos que imutáveis às vésperas da implementação — a spec não está pronta
  para congelar.
- Prazo de decisão de item aberto posterior à data em que o item bloqueia trabalho.

## Verificação

- [ ] Toda decisão implícita da spec foi explicitada e classificada.
- [ ] Nenhum item aberto sem dono, prazo e impacto do atraso.
- [ ] Todo imutável tem entrada no log com alternativa rejeitada, motivo e data.
- [ ] Critério de reabertura escrito na própria spec.
- [ ] Nenhum prazo de decisão posterior ao momento em que o item bloqueia trabalho derivado.
