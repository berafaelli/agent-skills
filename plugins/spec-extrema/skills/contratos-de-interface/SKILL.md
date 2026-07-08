---
name: contratos-de-interface
description: Especifica e audita contratos entre módulos e com APIs externas — formato de dados, semântica de erros, versionamento e modos de falha. Use quando o pedido for "audite os contratos de X", "revisar as fronteiras entre módulos" ou ao executar a etapa 3 da skill spec-extrema. NÃO use para escrever o conteúdo interno dos módulos.
---

# Contratos de Interface

## Visão Geral

Um contrato de interface é o acordo completo entre dois lados de uma fronteira: que dados
cruzam, o que acontece quando algo dá errado e como o acordo evolui sem quebrar ninguém.
Esta skill percorre cada fronteira de um sistema — módulo a módulo e com cada API externa —
e produz uma tabela de contratos auditável. Funciona tanto para especificar contratos novos
(etapa 3 da `spec-extrema`) quanto para auditar contratos de uma spec já existente.

## Quando Usar

- Executando a etapa 3 (Contratos entre Módulos) da skill `spec-extrema`.
- Pedido isolado: "audite os contratos de X", "revise as fronteiras entre os módulos".
- Antes de integrar uma nova API externa a um sistema existente.

**Quando NÃO usar:**
- Para especificar o interior de um módulo — isso é `spec-driven-development`.
- Para decidir onde cortar os módulos — isso é a etapa 2 da `spec-extrema`.

## Processo

Para **cada fronteira** (par de módulos que se tocam, ou módulo ⇄ API externa), percorra as
quatro camadas na ordem. Uma fronteira só está coberta com as quatro respondidas.

### 1. Formato de dados

- Schema completo do que cruza a fronteira (campos, tipos, obrigatoriedade).
- Unidades e representações: dinheiro (centavos ou decimal? qual moeda?), datas (fuso?
  formato?), identificadores (de quem é o ID?).
- Nulabilidade: o que significa campo ausente vs. campo nulo vs. string vazia?
- Encoding e limites: tamanho máximo de payload, paginação, taxa de eventos.

### 2. Semântica de erros

- Catálogo de erros que este lado pode devolver, com código e significado.
- Para cada erro: quem faz retry, quantas vezes, com que backoff?
- Idempotência: se a mesma operação chegar duas vezes, o efeito duplica?
- Erro parcial: numa operação em lote, o que acontece quando metade falha?

### 3. Versionamento

- Como o contrato evolui: campo novo é quebra? Campo removido segue qual processo?
- Como as versões coexistem durante uma migração e por quanto tempo.
- Onde a versão é declarada (header, URL, campo) e quem verifica.

### 4. Modos de falha (obrigatório para toda API externa)

- **Rate limit:** o que o sistema faz ao ser limitado — fila, degrada, descarta?
- **Timeout:** quanto espera, o que responde ao chamador enquanto isso?
- **Mudança de schema:** como detecta que o terceiro mudou o formato sem avisar?
- **Deprecação:** qual o plano quando o terceiro anunciar o fim da versão usada?

## Saída

Uma tabela de contratos por fronteira, no formato da SPEC-MASTER §4 e §5 da `spec-extrema`:

| Fronteira | Dados | Erros | Versionamento | Modos de falha | Pendências |
|---|---|---|---|---|---|

Em modo auditoria, a coluna **Pendências** lista cada camada sem resposta — esse é o
resultado da auditoria, não um detalhe.

## Racionalizações Comuns

| Racionalização | Realidade |
|---|---|
| "É JSON, os dois lados se entendem" | JSON define sintaxe, não semântica. Centavos vs. decimal são ambos JSON válidos — e um prejuízo real. |
| "A doc da API externa já cobre isso" | A doc cobre o que a API promete, não o que o SEU sistema faz quando ela não cumpre. |
| "Erro a gente trata com try/catch genérico" | Catch genérico é a decisão de não decidir — cada erro novo vira comportamento indefinido em produção. |
| "Versionamento é para depois que estabilizar" | O contrato muda mais justamente antes de estabilizar. Sem regra de evolução, cada mudança é uma quebra em potencial. |

## Sinais de Alerta

- Fronteira descrita por exemplo ("manda algo assim: {...}") em vez de schema.
- Dinheiro ou data cruzando fronteira sem unidade/fuso declarado.
- API externa com caminho feliz especificado e nenhuma linha sobre falha.
- "Retry" citado sem número de tentativas nem backoff.
- Operação de escrita entre módulos sem resposta sobre idempotência.

## Verificação

- [ ] Toda fronteira da matriz de dependências tem as 4 camadas respondidas.
- [ ] Toda API externa tem os 4 modos de falha especificados.
- [ ] Dinheiro, datas e IDs que cruzam fronteiras têm formato canônico e fonte de verdade.
- [ ] Toda operação com efeito (cobrança, envio, gravação) tem idempotência declarada.
- [ ] Em modo auditoria: a lista de pendências foi entregue, mesmo que vazia.
