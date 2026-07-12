---
name: dados-para-acao
description: 'Transforma números, métricas e relatórios em planos de ação com passo-a-passo, responsável, prazo e meta mensurável, mais acionadores que disparam ações quando métricas cruzam limiares. Use quando o pedido for "transformar esses números em ação", "analisar as métricas e propor um plano" ou quando um relatório de dados chegar sem desdobramento. NÃO use para construir dashboards ou pipelines de dados, nem para análise exploratória sem intenção de agir.'
---

# Dados para Ação

## Visão Geral

> **Jamais fique satisfeito com um número — ele precisa ser insumo para desdobramentos de
> ações.**

Um número sem desdobramento é custo de coleta desperdiçado. Esta skill opera com cabeça de
líder e estrategista, direta ao ponto: recebe dados e devolve plano — o que fazer, quem faz,
até quando, com que meta e o que dispara a próxima ação. O ciclo não termina na análise;
termina quando o resultado medido vira o próximo insumo. Crescimento é isso: números virando
ações, ações virando números melhores, em loop.

## Quando Usar

- Um relatório, dashboard ou métrica chegou e a pergunta é "e agora, o que fazemos?".
- Reunião de resultados que historicamente termina em observações, não em tarefas.
- Meta batida ou perdida — os dois casos geram trabalho (escalar ou corrigir).

**Quando NÃO usar:**
- Construir o dashboard/pipeline que coleta os dados — isso é engenharia de dados.
- Análise exploratória sem intenção de agir — sem decisão em jogo, não há plano a gerar.

## O Ciclo em 6 Elos

Cada elo tem entrada, saída e critério. Pular elo produz ou achismo (ação sem diagnóstico)
ou paralisia (diagnóstico sem ação).

### 1. Número

- **Entrada:** a métrica bruta.
- **Saída:** métrica com fonte e confiança declaradas — de onde veio, período, se é
  comparável com a medição anterior.
- **Critério:** número sem fonte não avança; primeiro conserta-se a medição.

### 2. Leitura

- **Entrada:** número qualificado.
- **Saída:** o que ele diz — contra a meta, contra o histórico, contra o par de mercado.
  Um número isolado não diz nada; a leitura é sempre comparativa.
- **Critério:** pelo menos duas comparações; "está baixo/alto" sem referência não é leitura.

### 3. Diagnóstico

- **Entrada:** leitura.
- **Saída:** por que o número está assim — hipóteses ranqueadas, cada uma com evidência e
  grau de confiança. Achismo declarado como achismo.
- **Critério:** hipótese principal tem evidência ou tem uma ação de verificação no plano.

### 4. Desdobramento

- **Entrada:** diagnóstico.
- **Saída:** plano de ação. **Cada ação obrigatoriamente com: responsável, prazo,
  passo-a-passo e meta mensurável.** Ação sem os 4 campos é intenção, não ação.
- **Critério:** todo número do elo 1 aponta para ≥1 ação — ou para uma dispensa
  justificada ("sem ação porque...", com o gatilho que mudaria isso).

### 5. Acionadores

- **Entrada:** plano.
- **Saída:** gatilhos métrica→ação: "se X cruzar Y, então Z" — decisões pré-tomadas que não
  dependem de nova reunião. Todo gatilho tem limiar numérico, quem verifica e quando.
- **Critério:** cada ação do plano tem gatilho de reavaliação (o que faria mudar de rumo).

### 6. Medição

- **Entrada:** ações em execução.
- **Saída:** data marcada, métricas a coletar (incluindo as metas do elo 4) e responsável
  pela coleta. O resultado medido é o novo elo 1 — **o ciclo reabre sozinho**.
- **Critério:** próxima medição agendada antes de encerrar o plano.

## Saída Obrigatória

Plano no formato de `templates/PLANO-DE-ACAO.template.md` — números de partida, diagnóstico,
ações, acionadores, números sem ação (justificados) e próxima medição.

Regra dura do documento: **número órfão invalida o plano.** Todo número citado aparece como
origem de ação ou na tabela de dispensas justificadas.

Para gerar o plano em contexto limpo a partir de um relatório bruto, delegue ao agente
`estrategista-de-dados`.

## Racionalizações Comuns

| Racionalização | Realidade |
|---|---|
| "O dashboard já é o entregável" | Dashboard é insumo. Entregável é a ação com dono, prazo e meta que sai dele. |
| "O número está bom, nada a fazer" | Número bom é insumo igual: a ação é escalar e replicar o que o produziu — antes que a janela feche. |
| "A análise está feita, alguém vai agir" | Análise sem dono não é plano. 'Alguém' é ninguém com prazo. |
| "Definimos a meta depois que a ação rodar" | Meta depois do resultado é auto-elogio garantido. A meta se fixa antes, ou não mede nada. |

## Sinais de Alerta

- Plano com verbos sem responsável ("melhorar", "revisar", "acompanhar" — quem?).
- "Melhorar X" sem número-alvo e sem prazo.
- Relatório que termina em observações e a reunião seguinte recomeça do zero.
- Acionador sem limiar numérico ("se piorar muito, a gente age").
- Métrica estável ignorada em silêncio — estabilidade também se vigia, com gatilho.

## Verificação

- [ ] Os 6 elos presentes, cada um com saída registrada.
- [ ] Toda ação com responsável, prazo, passo-a-passo e meta mensurável.
- [ ] Todo número de partida mapeado a ação ou a dispensa justificada — zero números órfãos.
- [ ] Todo acionador com limiar numérico, verificador e frequência.
- [ ] Número bom do relatório gerou ação de escala (não foi celebrado e esquecido).
- [ ] Próxima medição agendada, com as metas do plano entre as métricas a coletar.
