# spec-extrema

Plugin do Claude Code com um método de **engenharia de especificação para projetos grandes e
complexos**: muitos módulos, muita codificação, múltiplas APIs externas e exigência de
minúcia acima do padrão.

Projetos grandes não morrem por falta de código — morrem pela lacuna de especificação: o
detalhe que ninguém viu vira bug de integração seis semanas depois. Este plugin define como
fatiar um projeto grande demais para uma spec única, como amarrar os contratos entre as
fatias e como atacar a própria spec para encontrar o que ninguém viu, antes de escrever
qualquer código.

**Posicionamento:** opera acima da skill `spec-driven-development` (do plugin
[agent-skills](../../README.md)) e não a substitui — a SPEC-MASTER decompõe o sistema, e
cada spec de módulo derivada continua seguindo o fluxo gated e as seis áreas dela.

## Componentes

```
spec-extrema/
├── skills/
│   ├── spec-extrema/            Skill principal: processo em 6 etapas
│   │   ├── references/          Movimentos, checklist de 25 perguntas, proporcionalidade
│   │   └── templates/           SPEC-MASTER e SPEC-MODULO
│   ├── contratos-de-interface/  Sub-skill: contratos entre módulos e APIs (invocável isolada)
│   └── congelamento/            Sub-skill: fechar a spec (invocável isolada)
└── agents/
    ├── spec-adversario.md       Ataca a SPEC-MASTER em contexto limpo (mínimo 3 lacunas)
    └── auditor-de-contratos.md  Varredura mecânica de fronteiras, par a par
```

### O processo em 6 etapas

1. **Levantamento** — inventário de módulos, APIs externas e fluxos de dados.
2. **Decomposição** — critério de corte explícito por módulo; matriz de dependências com
   ordem de construção.
3. **Contratos entre Módulos** — dados, erros, versionamento; modos de falha por API externa.
4. **Etapa Adversarial** — os agentes atacam a spec antes de qualquer código.
5. **Congelamento** — imutável vs. aberto-com-dono-e-prazo; log de decisões.
6. **Derivação** — índice de specs por módulo, cada uma via `spec-driven-development`.

## Instalação

Via marketplace deste repositório:

```
/plugin marketplace add berafaelli/agent-skills
/plugin install spec-extrema@addy-agent-skills
```

Modo desenvolvimento (sem instalar), a partir da raiz do repositório:

```
claude --plugin-dir ./plugins/spec-extrema
```

Para recarregar mudanças durante o desenvolvimento, use `/reload-plugins` na sessão.

## Uso

As skills registram sob o namespace do plugin (a depender da versão do Claude Code, também
disparam automaticamente pela descrição):

- `spec-extrema:spec-extrema` — o método completo. Frases que disparam: "quero a spec do
  sistema inteiro", "é um projeto grande, com vários módulos e integrações".
- `spec-extrema:contratos-de-interface` — isolada, para auditar fronteiras de uma spec
  existente: "audite os contratos de X".
- `spec-extrema:congelamento` — isolada, para fechar uma spec: "congelar o escopo".

Os agentes `spec-adversario` e `auditor-de-contratos` são invocados pela etapa adversarial
da skill principal, ou diretamente contra qualquer spec multi-módulo.

**Contrato de saída:** a skill principal produz uma SPEC-MASTER (decomposição, contratos,
matriz de dependências, riscos, log de decisões) e o índice de specs por módulo. Nunca
produz código; nunca pula para spec de módulo sem a master aprovada.

## Fase 2 (planejado, não incluído)

- Hooks de enforcement (ex.: bloquear escrita de código quando não existe SPEC-MASTER no
  repositório). Adiado deliberadamente até o fluxo rodar algumas vezes.

## Licença

MIT
