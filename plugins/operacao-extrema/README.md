# operacao-extrema

Plugin do Claude Code para a **operação de projetos vivos**: segurança do projeto inteiro,
consumo robusto e econômico de APIs externas, e dados transformados em planos de ação.

É o irmão de operação do [spec-extrema](../spec-extrema/README.md): **spec-extrema opera
antes do código** (engenharia de especificação); **operacao-extrema opera no projeto que já
existe e roda** — protegendo-o, ajustando suas integrações e convertendo seus números em
crescimento.

## Componentes

```
operacao-extrema/
├── skills/
│   ├── seguranca-do-projeto/    Auditoria do projeto INTEIRO — documentos primeiro
│   │   └── templates/           RELATORIO-AUDITORIA
│   ├── consumo-de-apis/         Diagnóstico de 9 dimensões + ajustes por impacto × esforço
│   └── dados-para-acao/         Ciclo número → leitura → diagnóstico → ação → acionador → medição
│       └── templates/           PLANO-DE-ACAO
└── agents/
    ├── auditor-de-seguranca.md  Varredura em contexto limpo, com regra anti-inflação
    └── estrategista-de-dados.md Números → plano completo; nunca termina em observação
```

### As três skills

| Skill | Dispara quando | Saída |
|---|---|---|
| `seguranca-do-projeto` | "auditar a segurança do projeto", "varredura completa" | Relatório com achado, evidência, severidade e correção — mais "o que está limpo" |
| `consumo-de-apis` | "integração frágil/cara", "reduzir custo de chamadas de API" | Diagnóstico por integração (9 dimensões) + ajustes priorizados |
| `dados-para-acao` | "transformar esses números em ação", relatório sem desdobramento | Plano de ação: responsável, prazo, passo-a-passo, meta mensurável, acionadores |

## Fronteiras com o conteúdo existente

| Este plugin | Não confundir com | Diferença |
|---|---|---|
| `seguranca-do-projeto` | `security-and-hardening` (agent-skills, EN) | Lá: endurecer código novo de uma feature. Aqui: auditar um projeto existente inteiro, documentos incluídos. |
| `consumo-de-apis` | `spec-extrema:contratos-de-interface` | Lá: design-time — o que o contrato promete, na spec. Aqui: runtime — como o cliente se comporta de verdade. |
| `consumo-de-apis` | `performance-optimization` (agent-skills, EN) | Lá: cache para servir suas respostas mais rápido. Aqui: cache para cortar chamadas pagas ao terceiro. |
| `dados-para-acao` | `observability-and-instrumentation` (agent-skills, EN) | Lá: telemetria técnica (latência, erros). Aqui: métricas de negócio viram planos de ação. |

## Instalação

Via marketplace deste repositório:

```
/plugin marketplace add berafaelli/agent-skills
/plugin install operacao-extrema@addy-agent-skills
```

Modo desenvolvimento (sem instalar), a partir da raiz do repositório:

```
claude --plugin-dir ./plugins/operacao-extrema
```

## Uso

- `operacao-extrema:seguranca-do-projeto` — "audite a segurança do projeto inteiro,
  documentos e código".
- `operacao-extrema:consumo-de-apis` — "nossa integração com a API X está frágil e a fatura
  subiu; revise o consumo".
- `operacao-extrema:dados-para-acao` — "aqui está o relatório do mês; transforme em plano
  de ação".

Os agentes `auditor-de-seguranca` e `estrategista-de-dados` são acionados pelas skills, ou
diretamente: o auditor contra qualquer projeto existente, o estrategista sobre qualquer
relatório de dados.

**Princípio do plugin:** nenhuma saída termina em observação. Auditoria termina em correções
priorizadas; diagnóstico de APIs termina em ajustes ordenados; análise de dados termina em
plano com dono, prazo e meta — e o resultado medido reabre o ciclo.

## Licença

MIT
