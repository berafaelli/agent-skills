---
name: spec-extrema
description: 'Engenharia de especificação para projetos grandes demais para uma spec única — decompõe o sistema em módulos, amarra contratos entre fronteiras e caça lacunas via passe adversarial antes de qualquer código. Use quando o projeto tiver 3+ módulos ou 2+ APIs externas, ou quando o pedido soar como "quero a spec do sistema inteiro" ou "é um projeto grande, com vários módulos e integrações". NÃO use para spec de módulo único ou feature isolada (ex.: "spec de uma feature de login") — nesses casos use spec-driven-development diretamente.'
---

# Spec Extrema

## Visão Geral

Projetos grandes não morrem por falta de código — morrem pela lacuna de especificação: o
detalhe que ninguém viu vira bug de integração seis semanas depois. Esta skill captura um
método de engenharia de spec para sistemas com muitos módulos e múltiplas APIs externas:
como fatiar, como amarrar os contratos entre as fatias e como atacar a própria spec para
encontrar o que ninguém viu, antes de escrever qualquer código.

**Posicionamento:** esta skill opera ACIMA de `spec-driven-development` e não o substitui.
Ela entra quando o projeto é grande demais para uma spec única. A SPEC-MASTER que ela produz
decompõe o sistema; cada spec de módulo derivada continua seguindo o fluxo gated e as seis
áreas de `spec-driven-development` (Objetivo, Comandos, Estrutura, Estilo, Testes,
Fronteiras). A elicitação inicial de requisitos continua sendo território de `interview-me`
e `idea-refine`.

## Quando Usar

- Projeto com 3 ou mais módulos, ou 2 ou mais APIs externas.
- O pedido menciona "sistema inteiro", "projeto grande", "vários módulos", "plataforma".
- Uma spec existente cresceu tanto que ninguém consegue relê-la inteira.

**Quando NÃO usar:**
- Spec de módulo único ou feature isolada — vá direto para `spec-driven-development`.
- Auditoria pontual de fronteiras em spec já existente — use a sub-skill
  `contratos-de-interface` isoladamente.
- Fechar uma spec quase pronta — use a sub-skill `congelamento` isoladamente.

## O Processo em 6 Etapas

Cada etapa tem entrada, saída, critério de avanço e artefato. Não avance sem cumprir o
critério — pular etapa é a forma mais cara de ir rápido.

### 1. Levantamento

- **Entrada:** descrição do sistema, requisitos elicitados (via `interview-me` se necessário).
- **Saída:** inventário de módulos candidatos, APIs externas e fluxos de dados principais.
- **Critério de avanço:** nenhuma API externa ou fluxo de dados conhecido fora do inventário.
- **Artefato:** inventário inicial (rascunho das seções 2 e 5 da SPEC-MASTER).

### 2. Decomposição

- **Entrada:** inventário da etapa 1.
- **Saída:** lista final de módulos com critério de corte explícito por módulo, e matriz de
  dependências com ordem de construção.
- **Critério de corte de módulo** (todo módulo precisa de pelo menos um): dado que muda
  junto fica junto; dono/equipe distinta; fronteira de API externa; ciclo de deploy próprio.
- **Critério de avanço:** matriz sem ciclos não resolvidos; cada módulo tem responsabilidade
  de uma frase.
- **Artefato:** SPEC-MASTER §2 e §3.

### 3. Contratos entre Módulos

- **Entrada:** matriz de dependências.
- **Saída:** contrato por par de módulos que se tocam (dados, erros, versionamento) e mapa
  de modos de falha por API externa. Processo detalhado na sub-skill
  `contratos-de-interface`.
- **Critério de avanço:** nenhum par da matriz sem contrato; nenhuma API externa sem mapa de
  falha; dicionário de dados cobre dinheiro, datas e identificadores.
- **Artefato:** SPEC-MASTER §4, §5 e §6.

### 4. Etapa Adversarial

- **Entrada:** SPEC-MASTER com §1–§7 preenchidas.
- **Saída:** relatório de lacunas e spec corrigida. Execute o agente `spec-adversario` em
  contexto limpo (quem escreveu a spec não deve ser quem a ataca) e o agente
  `auditor-de-contratos` para a varredura mecânica de fronteiras. Perguntas de ataque: o que
  quebra primeiro sob carga? Onde dois módulos entendem a mesma coisa de forma diferente?
  Qual API externa vai mentir para nós?
- **Critério de avanço:** o passe encontrou no mínimo 3 lacunas e todas foram corrigidas ou
  registradas como risco — ou há justificativa explícita de por que menos de 3.
- **Artefato:** SPEC-MASTER §12 e correções nas demais seções.

### 5. Congelamento

- **Entrada:** SPEC-MASTER pós-adversarial.
- **Saída:** partição de tudo em imutável vs. aberto-com-dono-e-prazo, e log de decisões
  com alternativa rejeitada e motivo. Processo detalhado na sub-skill `congelamento`.
- **Critério de avanço:** nenhum item aberto sem dono e prazo; log de decisões cobre toda
  escolha estrutural.
- **Artefato:** SPEC-MASTER §9 e §10.

### 6. Derivação

- **Entrada:** SPEC-MASTER congelada e aprovada por humano.
- **Saída:** índice de specs por módulo, cada uma iniciada a partir de
  `templates/SPEC-MODULO.template.md` e desenvolvida via `spec-driven-development`.
- **Critério de avanço:** todo módulo da §2 tem entrada no índice; a ordem de derivação
  segue a ordem de construção da §3.
- **Artefato:** SPEC-MASTER §11 e os arquivos de spec de módulo.

## Contrato de Saída

Quando invocada, esta skill produz:

1. **SPEC-MASTER** — a partir de `templates/SPEC-MASTER.template.md`: decomposição,
   contratos, matriz de dependências, riscos e log de decisões.
2. **Índice de specs por módulo** — cada uma a partir de `templates/SPEC-MODULO.template.md`.

Nunca produz código. Nunca pula para spec de módulo sem a SPEC-MASTER aprovada.

## Material de Apoio

Carregue sob demanda, conforme a etapa:

- `references/movimentos.md` — os 7 movimentos que diferenciam esta spec do padrão preguiçoso.
- `references/checklist-25.md` — 25 perguntas agrupadas por etapa, para revisão a frio.
- `references/proporcionalidade.md` — onde a minúcia extrema é obrigatória e onde detalhe
  demais é dívida.

## Racionalizações Comuns

| Racionalização | Realidade |
|---|---|
| "O projeto é grande, mas eu entendo tudo de cabeça" | Entender de cabeça não sobrevive a férias, troca de equipe nem a seis semanas de implementação. A fronteira que só existe na sua cabeça é a que vai divergir. |
| "Os contratos podem esperar; definimos na implementação" | Contrato definido na implementação é definido duas vezes — uma em cada lado da fronteira, de forma diferente. |
| "Vamos detalhar tudo com o mesmo rigor" | Minúcia uniforme gera spec de 3 mil linhas que ninguém relê. Use `references/proporcionalidade.md`. |
| "A etapa adversarial é pessimismo; a spec está boa" | Se um ataque estruturado não encontra 3 lacunas num sistema multi-módulo, quase sempre o ataque foi fraco, não a spec forte. |
| "Começamos pelo módulo mais interessante" | A ordem de construção sai da matriz de dependências, não do entusiasmo. Módulo construído antes das dependências é retrabalho garantido. |
| "Uma spec só, bem organizada, resolve" | Acima de certo tamanho, spec única mistura altitudes: decisão de sistema vira parágrafo perdido entre detalhes de módulo. |

## Sinais de Alerta

Pare e volte ao processo se notar:

- Código sendo escrito sem SPEC-MASTER aprovada.
- Par de módulos trocando dados sem contrato na §4.
- API externa citada em algum módulo mas ausente da §5.
- Passe adversarial que "não encontrou nada" aceito sem questionamento.
- Item aberto sem dono ou sem prazo.
- Spec de módulo redefinindo um contrato em vez de referenciar a master.
- Dinheiro, data ou identificador fora do dicionário de dados.

## Verificação

Antes de dar a SPEC-MASTER por pronta:

- [ ] Todo módulo da §2 tem motivo de corte explícito e dono.
- [ ] Matriz de dependências sem ciclos não resolvidos; ordem de construção definida.
- [ ] Todo par de módulos que se tocam tem contrato (dados, erros, versionamento).
- [ ] Toda API externa tem mapa de modos de falha (rate limit, timeout, schema, deprecação).
- [ ] Passe adversarial executado com ≥3 lacunas encontradas, ou justificativa explícita.
- [ ] Todas as lacunas corrigidas ou registradas como risco com mitigação.
- [ ] Log de decisões com alternativa rejeitada e motivo para cada escolha estrutural.
- [ ] Itens abertos com dono e prazo; imutáveis declarados.
- [ ] Índice de specs de módulo completo e na ordem de construção.
- [ ] SPEC-MASTER aprovada por humano antes de derivar qualquer spec de módulo.
