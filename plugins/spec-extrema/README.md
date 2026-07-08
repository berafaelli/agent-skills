# spec-extrema

Plugin de especificação extrema para Claude Code: o método completo para escrever specs até o ponto em que outra pessoa (ou outro agente) implementa o sistema sem tomar nenhuma decisão de produto. Princípio central: **o que não está escrito não existe**.

## O que vem no plugin

```
spec-extrema/
├── .claude-plugin/plugin.json
├── skills/
│   ├── spec-extrema/            # skill principal: o método em 8 etapas
│   │   ├── SKILL.md
│   │   ├── references/
│   │   │   ├── movimentos.md            # os 8 diferenciais nomeados do método
│   │   │   ├── checklist-25.md          # as 25 perguntas do gate final
│   │   │   └── proporcionalidade.md     # onde minúcia é obrigatória vs. dívida
│   │   └── templates/
│   │       ├── SPEC-MASTER.template.md
│   │       └── SPEC-MODULO.template.md  # formato MODULE-STANDARD
│   ├── estrategia/              # camada de "por quê" antes da decomposição
│   ├── contratos-de-interface/  # fronteiras: dados, erros, versionamento, falhas
│   ├── ux/                      # fluxos, matriz de estados, copy de erro, destrutivas
│   ├── testes-de-ferramenta/    # plano de testes falseável derivado da spec
│   └── congelamento/            # imutável vs. aberto-com-dono, registro de decisões
├── agents/
│   ├── spec-adversario.md       # ataca a spec em contexto limpo (mínimo 3 lacunas)
│   └── auditor-de-contratos.md  # varredura par-a-par das fronteiras
└── README.md
```

## O método em uma linha por etapa

1. **Estratégia** (`spec-extrema:estrategia`) — problema, por que agora, alternativas descartadas, fora de escopo, métrica única. Gate humano.
2. **SPEC-MASTER** — template preenchido, suposições declaradas.
3. **Decomposição** — uma SPEC-MODULO por módulo, responsabilidade em uma frase sem "e".
4. **Contratos** (`spec-extrema:contratos-de-interface` + agente `auditor-de-contratos`) — fronteiras fechadas e verificadas dos dois lados.
5. **UX** (`spec-extrema:ux`) — fluxos numerados, quatro estados, copy de erro com próxima ação.
6. **Testes** (`spec-extrema:testes-de-ferramenta`) — plano falseável e rastreável; critério sem teste é critério mal escrito.
7. **Ataque adversarial** (agente `spec-adversario`) — contexto limpo, mínimo 3 lacunas; a spec só passa quando um ataque devolve menos de 3 novas.
8. **Congelamento** (`spec-extrema:congelamento`) — imutável vs. aberto-com-dono, registro de decisões, protocolo de mudança.

As sub-skills também funcionam isoladamente em specs já existentes ("audita os contratos do Radar", "especifica a UX do onboarding") — sem rodar o método inteiro.

## Instalação e desenvolvimento

Testar sem instalar, direto do checkout:

```bash
claude --plugin-dir ./plugins/spec-extrema
```

Dentro da sessão, `/reload-plugins` recarrega mudanças sem reiniciar. As skills ficam namespaceadas como `spec-extrema:<skill>` (ex.: `spec-extrema:estrategia`), sem colisão com skills homônimas de outros plugins.

## Teste de aceite do plugin

O agente `spec-adversario` é validado contra uma spec real: rodado numa spec considerada "pronta", deve encontrar **no mínimo 3 lacunas relevantes** na primeira rodada. Se não encontra, o problema é do agente (mandato fraco), não da spec — itere o prompt do agente até passar.

## Fase 2 (planejada, não incluída)

- **Hooks de enforcement** — ex.: `PreToolUse` bloqueando escrita de código quando não existe SPEC-MASTER congelada no repositório. Adiado deliberadamente: enforcement prescritivo só depois de o fluxo rodar completo duas ou três vezes com confiança.

## Segurança

O plugin é 100% markdown e raciocínio: **sem MCP, sem hooks, sem scripts executáveis**. A revisão de segurança se resume a ler os arquivos deste diretório.
