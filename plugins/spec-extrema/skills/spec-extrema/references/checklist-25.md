# Checklist das 25 Perguntas

O gate final antes de declarar a spec pronta. Percorra na ordem; cada pergunta exige resposta objetiva. **"Não sei" reprova** — significa lacuna. "Não se aplica" só vale com justificativa de uma linha.

## A. Estratégia e escopo (etapa 1–2)

1. O problema está formulado em uma frase que um usuário real reconheceria como seu?
2. Por que agora? O custo de *não* construir está escrito?
3. Quais alternativas (comprar, adaptar existente, não fazer) foram consideradas e por que foram descartadas?
4. O que está explicitamente **fora** do escopo da primeira versão?
5. Qual métrica única declara sucesso, e em qual horizonte de tempo?

## B. Decomposição e módulos (etapa 3)

6. Todo módulo tem SPEC-MODULO própria no formato MODULE-STANDARD?
7. Cada módulo tem responsabilidade descrita em uma frase sem "e"?
8. As dependências entre módulos formam um grafo sem ciclos?
9. Existe algum comportamento do sistema que não pertence a nenhum módulo? (Se sim: lacuna de decomposição.)
10. Cada módulo declara o que acontece com o resto do sistema quando ele está fora do ar?

## C. Contratos e fronteiras (etapa 4)

11. Toda fronteira entre módulos tem formato de dados exato — tipos, nulabilidade, limites, unidades?
12. Todo erro que cruza uma fronteira tem código, formato e responsável definido por tratá-lo?
13. A política de versionamento de cada contrato está escrita (o que é mudança compatível, o que quebra)?
14. Os dois lados de cada fronteira concordam — verificado pelo `auditor-de-contratos`, não assumido?
15. Toda integração externa tem timeout, política de retry e fallback especificados?

## D. UX e estados (etapa 5)

16. Todo fluxo tem os quatro estados especificados: vazio, carregando, erro, sucesso?
17. Está escrito o que o usuário vê na primeira vez, antes de existir qualquer dado?
18. Toda ação destrutiva tem confirmação ou undo especificado?
19. Toda mensagem de erro diz ao usuário o que fazer a seguir?
20. Os fluxos estão escritos como passos numerados verificáveis, não como descrições de telas?

## E. Testes, ataque e congelamento (etapas 6–8)

21. Todo critério de aceite é falseável — dá para escrever um teste que falharia hoje?
22. O plano de testes cobre cada contrato de fronteira e cada modo de falha, não só o caminho feliz?
23. O último ataque do `spec-adversario` rodou em contexto limpo e devolveu menos de 3 lacunas novas relevantes?
24. Cada seção está marcada como imutável ou aberta-com-dono (nenhum "TBD" órfão)?
25. O registro de decisões explica, para cada decisão relevante, quais alternativas foram descartadas e por quê?

---

**Como usar:** responda as 25 por escrito, no fim da SPEC-MASTER ou em comentário de PR da spec. As reprovadas viram tarefas de correção na etapa correspondente — não notas de rodapé.
