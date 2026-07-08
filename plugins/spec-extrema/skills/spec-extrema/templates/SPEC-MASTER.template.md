# SPEC-MASTER — [Nome do Sistema]

> Gerada pela skill `spec-extrema`. Esta spec governa o sistema inteiro; specs de módulo
> derivam dela e nunca a contradizem. Preencha todas as seções — uma seção vazia é uma
> decisão não tomada, não uma seção opcional.

## 1. Visão e Objetivo do Sistema

- **Problema que o sistema resolve:**
- **Resultado observável quando estiver pronto:**
- **O que este sistema explicitamente NÃO faz:**

## 2. Mapa de Módulos

| Módulo | Responsabilidade (1 frase) | Dono | Motivo do corte |
|---|---|---|---|
| | | | |

> "Motivo do corte" registra qual critério de decomposição justificou este módulo existir
> separado (dado que muda junto, dono distinto, fronteira de API externa, ciclo de deploy).

## 3. Matriz de Dependências e Ordem de Construção

| Módulo | Depende de | Tipo de dependência (dados / chamada / evento) |
|---|---|---|
| | | |

**Ordem de construção:** 1. … 2. … 3. …

**Ciclos detectados e como foram quebrados:**

## 4. Contratos entre Módulos

> Um bloco por par de módulos que se tocam. Sem exceção: se dois módulos trocam qualquer
> coisa, o par aparece aqui.

### [Módulo A] ⇄ [Módulo B]

- **Dados:** formato, schema, unidades, nulabilidade, encoding
- **Erros:** códigos, semântica, quem faz retry, idempotência
- **Versionamento:** como o contrato evolui sem quebrar o outro lado

## 5. APIs Externas e Modos de Falha

| API | Usada por | Rate limit | Timeout | Mudança de schema | Deprecação | Comportamento quando falha |
|---|---|---|---|---|---|---|
| | | | | | | |

## 6. Dicionário de Dados Compartilhado

| Conceito | Formato canônico | Unidade | Quem é a fonte de verdade |
|---|---|---|---|
| | | | |

> Dinheiro, datas/fusos, identificadores e medidas SEMPRE aparecem aqui.

## 7. Inventário de Estados Impossíveis

> Combinações de estado que nunca podem ocorrer, e qual mecanismo as impede.

| Estado impossível | O que o impede |
|---|---|
| | |

## 8. Riscos

| Risco | Probabilidade | Impacto | Mitigação | Gatilho de reavaliação |
|---|---|---|---|---|
| | | | | |

## 9. Log de Decisões

| Decisão | Alternativa rejeitada | Motivo | Data |
|---|---|---|---|
| | | | |

## 10. Congelados e Abertos

**Imutáveis (mudar exige reabrir a SPEC-MASTER):**
- …

**Abertos:**

| Item aberto | Dono | Prazo de decisão | Impacto do atraso |
|---|---|---|---|
| | | | |

## 11. Índice de Specs de Módulo

| Módulo | Arquivo da spec | Status |
|---|---|---|
| | `specs/[modulo]/SPEC.md` | pendente / rascunho / aprovada |

## 12. Relatório Adversarial

- **Data do último passe:**
- **Lacunas encontradas:** (mínimo 3, ou justificativa explícita de por que menos)
- **Lacunas corrigidas nesta versão:**
