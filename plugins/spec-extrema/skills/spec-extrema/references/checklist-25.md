# Checklist das 25 Perguntas

Agrupadas pelas 6 etapas do processo. Cada pergunta indica a lacuna de spec que fecha.
Foram escritas para funcionar a frio — impressas numa reunião de revisão, sem IA presente.
Nove perguntas são sobre integração e fronteiras entre módulos, porque é onde projetos
grandes morrem.

## Etapa 1 — Levantamento

1. **Existe alguma API externa, job agendado ou integração que algum módulo usa mas que não
   está no inventário?**
   Fecha a lacuna de: dependência externa descoberta durante a implementação.
2. **Todo fluxo de dados tem origem e destino nomeados, ou há dado que "aparece" em algum
   módulo sem ninguém saber de onde?**
   Fecha a lacuna de: fluxo fantasma sem dono, impossível de depurar depois.
3. **Quem vai operar este sistema em produção participou do levantamento?**
   Fecha a lacuna de: requisito operacional (backup, monitoramento, acesso) descoberto no deploy.

## Etapa 2 — Decomposição

4. **Cada módulo tem exatamente um motivo de corte declarado, ou algum existe "porque ficou
   natural"?**
   Fecha a lacuna de: módulo sem razão de existir, que vira depósito de código órfão.
5. **Se dois módulos precisam mudar juntos em toda alteração, por que são dois?**
   Fecha a lacuna de: decomposição falsa — fronteira que só adiciona custo de coordenação.
6. **A matriz de dependências tem algum ciclo, mesmo indireto (A→B→C→A)?**
   Fecha a lacuna de: dependência circular descoberta com módulos já construídos.
7. **A ordem de construção permite testar algo de ponta a ponta cedo, ou só integra tudo
   no final?**
   Fecha a lacuna de: big bang de integração no último mês do projeto.

## Etapa 3 — Contratos e Integração

8. **Para cada par de módulos que se tocam: os dois lados leem o mesmo schema, com as
   mesmas unidades e a mesma nulabilidade?**
   Fecha a lacuna de: divergência de formato que compila dos dois lados e quebra em runtime.
9. **Quando o módulo A recebe um erro do módulo B, está escrito o que A faz — retenta,
   propaga, degrada?**
   Fecha a lacuna de: comportamento de erro decidido por improviso no `catch`.
10. **As operações entre módulos são idempotentes, ou um retry duplica efeito (cobrança,
    e-mail, registro)?**
    Fecha a lacuna de: efeito duplicado sob retry — o clássico cliente cobrado duas vezes.
11. **Como cada contrato evolui sem quebrar o outro lado — há regra de versionamento ou
    "avisamos no chat"?**
    Fecha a lacuna de: quebra silenciosa de contrato numa mudança "pequena".
12. **Para cada API externa: o que o sistema faz sob rate limit, timeout, mudança de schema
    e deprecação?**
    Fecha a lacuna de: outage em cascata quando o terceiro degrada.
13. **Dinheiro, datas/fusos e identificadores têm formato canônico único no dicionário de
    dados, com fonte de verdade nomeada?**
    Fecha a lacuna de: bug de conversão entre módulos (centavos vs. decimal, UTC vs. local).
14. **Quem é a fonte de verdade de cada dado compartilhado — e os outros módulos leem dela
    ou mantêm cópia própria?**
    Fecha a lacuna de: duas cópias do mesmo dado divergindo sem ninguém perceber.
15. **Há limite de tamanho/volume declarado em cada fronteira (payload máximo, itens por
    página, taxa de eventos)?**
    Fecha a lacuna de: fronteira que funciona no teste e estoura no primeiro cliente grande.
16. **Se uma mensagem/evento chega duas vezes ou fora de ordem, o consumidor está
    especificado para lidar com isso?**
    Fecha a lacuna de: corrupção de estado por reentrega — inevitável em qualquer fila real.

## Etapa 4 — Adversarial

17. **O que quebra primeiro se o volume for 10x o previsto — e a spec diz o que acontece
    nesse ponto?**
    Fecha a lacuna de: gargalo conhecido só no incidente.
18. **Existe algum termo que dois módulos usam com significados diferentes ("pedido",
    "usuário", "ativo")?**
    Fecha a lacuna de: dois módulos implementando corretamente entendimentos incompatíveis.
19. **Qual API externa tem maior chance de mentir (dado velho, sucesso falso, schema
    divergente da doc) — e o sistema detectaria?**
    Fecha a lacuna de: confiança cega em resposta de terceiro.
20. **Que estado impossível do inventário se torna possível se dois módulos falharem na
    ordem certa?**
    Fecha a lacuna de: invariante garantida por sorte, não por mecanismo.

## Etapa 5 — Congelamento

21. **Todo item aberto tem dono e prazo — ou há algum "decidimos depois" sem responsável?**
    Fecha a lacuna de: decisão adiada que ressurge como bloqueio na implementação.
22. **Cada decisão estrutural tem a alternativa rejeitada e o motivo registrados?**
    Fecha a lacuna de: relitigação e reversão silenciosa por quem não conhece o motivo.
23. **O que exatamente justifica descongelar um item imutável — está escrito?**
    Fecha a lacuna de: "imutável" que derrete na primeira pressão de prazo.

## Etapa 6 — Derivação

24. **Toda spec de módulo referencia os contratos da master em vez de redefini-los?**
    Fecha a lacuna de: contrato bifurcado — a master diz uma coisa, o módulo outra.
25. **Alguém consegue começar a spec de um módulo lendo só a master e o template — ou
    depende de contexto que está na cabeça de alguém?**
    Fecha a lacuna de: conhecimento oral como dependência de projeto.
