---
name: consumo-de-apis
description: 'Diagnostica e ajusta o consumo de APIs externas em projetos de médio porte — robustez (timeout, retry, rate limit, fallback) e economia (cache para cortar chamadas pagas, orçamento de quota e custo). Use quando o pedido for "nossa integração está frágil/lenta/cara", "reduzir custo de chamadas de API" ou "revisar o consumo de APIs externas". NÃO use para desenhar o contrato da integração (spec-extrema:contratos-de-interface) nem para performance geral da própria aplicação (performance-optimization).'
---

# Consumo de APIs

## Visão Geral

Projetos de médio porte vivem de consultar APIs de terceiros — e morrem de duas formas:
fragilidade (a integração cai junto com a API, ou pior, derruba o resto) e custo silencioso
(cada chamada evitável vira fatura). Esta skill percorre cada integração existente com um
diagnóstico de 9 dimensões e devolve uma lista de ajustes priorizados por impacto × esforço.

**Posicionamento em três fronteiras:**
- `spec-extrema:contratos-de-interface` é **design-time**: o que o contrato promete e como
  falha, na spec. Esta skill é **runtime**: como o seu cliente se comporta de verdade.
- `performance-optimization` cuida da performance da **sua** aplicação — cache lá existe
  para servir suas respostas mais rápido; **cache aqui existe para cortar chamadas pagas ao
  terceiro**. A frase muda tudo: o motivo do cache é custo, não latência.
- `api-and-interface-design` cuida de APIs que você **serve**; aqui você é o cliente.

## Quando Usar

- Integração frágil: quebra quando a API degrada, ou nunca foi testada sob falha.
- Fatura de API crescendo sem explicação, ou medo de crescer.
- Projeto de médio porte com 2+ integrações de consulta e nenhuma revisão de consumo.

**Quando NÃO usar:**
- Especificar uma integração nova — `spec-extrema:contratos-de-interface`.
- App lento por causa própria (banco, bundle, render) — `performance-optimization`.

## Fase 1 — Diagnóstico por Integração

Inventarie **toda** integração externa e avalie cada uma nas 9 dimensões. Para cada
dimensão: qual o estado atual (com evidência no código/config) e qual o risco ou custo de
ficar como está.

1. **Timeout** — existe, é específico por integração (não um global único), e o chamador
   sabe o que fazer quando estoura?
2. **Retry com backoff e jitter** — há teto de tentativas? Backoff exponencial? Jitter para
   não sincronizar clientes? Retry sem teto amplifica o outage do terceiro.
3. **Rate limit** — o cliente trata 429? Respeita `Retry-After`? Conhece o limite contratado
   e se mantém abaixo dele por design, não por sorte?
4. **Cache para cortar chamadas** — o que é cacheável (TTL), qual a chave, qual staleness o
   negócio tolera? Toda chamada repetida com a mesma pergunta é dinheiro devolvido ao terceiro.
5. **Paginação como cliente** — a travessia pega todas as páginas? Para quando deve? Assume
   "página 1 basta" em algum lugar?
6. **Validação de resposta** — dado de terceiro é dado não confiável: o shape é validado
   antes de usar? Campo ausente/novo derruba o quê?
7. **Orçamento de quota e custo** — existe número de chamadas/mês esperado? Alguém é
   avisado quando o consumo foge dele, ou o aviso é a fatura?
8. **Fallback e degradação** — API fora do ar: o sistema espera, serve dado velho do cache,
   ou degrada uma funcionalidade declaradamente? "Trava tudo" não é resposta.
9. **Idempotência de efeitos** — se a consulta dispara efeito (gravação, notificação,
   cobrança), o retry duplica o efeito?

**Saída da fase:**

| Integração | Dimensão | Estado atual (evidência) | Risco / Custo |
|---|---|---|---|

## Fase 2 — Ajustes Priorizados

Converta cada risco/custo em ajuste concreto e ordene por **impacto × esforço** — impacto
alto e esforço baixo primeiro.

**Proporcionalidade:** integração paga e quente (muitas chamadas, dinheiro envolvido) exige
as 9 dimensões resolvidas. Consulta esporádica e gratuita pode dispensar dimensões — mas a
dispensa é declarada na tabela, não silenciosa.

**Saída da fase:**

| Ajuste | Integração | Impacto | Esforço | Ordem |
|---|---|---|---|---|

## Racionalizações Comuns

| Racionalização | Realidade |
|---|---|
| "O SDK já cuida do retry" | Cuida como? Verificou teto de tentativas e jitter, ou está confiando no default que ninguém leu? |
| "A API é rápida, cache complica à toa" | O motivo do cache aqui é custo, não latência. Rápida e paga continua paga. |
| "Nunca vimos 429 em desenvolvimento" | Dev não tem o volume de produção. O 429 aparece exatamente no pico, quando mais dói. |
| "Validar resposta de API grande é paranoia" | Grande também depreca campo, muda formato e devolve erro em HTML. O custo da validação é uma função; o da confiança cega é um incidente. |

## Sinais de Alerta

- Retry sem teto ou sem backoff — amplificador de outage.
- Um único timeout global para integrações com naturezas diferentes.
- Chamada idêntica repetida em sequência no log (cache ausente onde mais dói).
- Custo de API descoberto na fatura, não num alerta.
- `catch` genérico em volta da chamada, sem distinguir timeout de 429 de 500.

## Verificação

- [ ] Toda integração externa do projeto inventariada.
- [ ] As 9 dimensões avaliadas por integração — ou dispensadas com justificativa na tabela.
- [ ] Todo estado atual tem evidência (arquivo/config), não suposição.
- [ ] Ajustes ordenados por impacto × esforço.
- [ ] Nenhuma recomendação sem dimensão de origem no diagnóstico.
- [ ] Integrações pagas têm orçamento de custo e alerta definidos.
