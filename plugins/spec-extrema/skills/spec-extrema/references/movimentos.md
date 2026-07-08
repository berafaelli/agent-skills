# Os Movimentos da Spec Extrema

Os oito diferenciais nomeados do método. Cada um existe para eliminar uma classe específica de retrabalho. Se você está cortando um movimento, saiba qual classe de defeito está aceitando de volta.

## 1. O que não está escrito não existe

O ônus da prova é da spec, não do leitor. Se um comportamento não está escrito, quem implementa não tem permissão para inventá-lo — tem obrigação de reportar a lacuna. Isso inverte a dinâmica usual, em que a spec "dá uma direção" e o implementador preenche os vazios em silêncio. Vazio preenchido em silêncio é a origem da maioria das divergências entre intenção e produto.

**Na prática:** toda vez que uma pergunta de implementação não tem resposta na spec, a correção acontece na spec primeiro, e só depois no código.

## 2. Estratégia antes de estrutura

Nenhuma decomposição em módulos antes de a spec saber responder: qual problema, por que agora, quais alternativas foram descartadas, o que está fora de escopo, qual métrica declara sucesso. Estrutura desenhada sem estratégia validada otimiza para o problema errado com precisão.

**Na prática:** a skill `spec-extrema:estrategia` roda primeiro e o humano valida antes da etapa 2.

## 3. Duas camadas: SPEC-MASTER + MODULE-STANDARD

Uma spec única de sistema grande vira um documento que ninguém lê inteiro e ninguém mantém. O método separa: a SPEC-MASTER carrega o que é do sistema (estratégia, decomposição, contratos, decisões); cada módulo carrega o que é seu, num formato padronizado (MODULE-STANDARD) — o que permite comparar módulos, auditar fronteiras mecanicamente e implementar módulos em paralelo.

**Na prática:** se um detalhe interessa a dois módulos, ele é contrato e sobe para a SPEC-MASTER; se interessa a um, desce para a SPEC-MODULO.

## 4. Contrato antes de conteúdo

As fronteiras entre módulos são especificadas antes do interior de cada módulo. Formato de dados, erros, versionamento e modos de falha de cada fronteira são fechados primeiro — porque são o que trava implementação paralela e o que mais custa mudar depois. O interior de um módulo pode ser refeito sem afetar ninguém; a fronteira, não.

**Na prática:** a etapa de contratos (com auditoria par-a-par) fecha antes de qualquer detalhamento interno profundo.

## 5. Enumeração de modos de falha

Toda fronteira, API externa e ação de usuário lista o que acontece quando dá errado: timeout, indisponibilidade, dado inválido, permissão negada, estado concorrente. "Tratamento de erro" genérico não é especificação — é adiamento. A pergunta padrão em cada ponto: *o que o outro lado vê quando este lado falha?*

**Na prática:** cada contrato tem uma tabela de modos de falha; cada fluxo de UX tem o estado de erro especificado com a próxima ação do usuário.

## 6. Ataque adversarial em contexto limpo

Quem escreveu a spec não consegue encontrar as lacunas dela — o raciocínio que gerou o texto preenche os vazios automaticamente na leitura. Por isso o ataque roda num subagente com contexto limpo: ele recebe só a spec e um mandato ("encontre no mínimo 3 lacunas"), sem o histórico da sessão que a escreveu. O mínimo obrigatório de 3 impede o relatório complacente.

**Na prática:** o agente `spec-adversario` roda a cada iteração; a spec só avança quando um ataque devolve menos de 3 lacunas novas relevantes. Esse é o teste de aceite do método, e é falseável.

## 7. Proporcionalidade declarada

Nem toda seção merece o mesmo nível de minúcia — e fingir que sim é o que torna specs extremas insustentáveis. O método exige que cada área declare seu nível: **minúcia obrigatória** (contratos, dados persistidos, dinheiro, permissões, ações destrutivas) ou **dívida assumida** — que só é válida com dono e prazo. A régua completa está em [proporcionalidade.md](proporcionalidade.md).

**Na prática:** "TBD" sem dono é lacuna e reprova no checklist; "aberto, dono: fulano, decide até a etapa X" é dívida legítima.

## 8. Congelamento com registro de decisões

Uma spec que muda silenciosamente não é fonte de verdade. No congelamento, cada seção é marcada **imutável** ou **aberta-com-dono**, e toda decisão relevante entra no registro com as alternativas descartadas e o motivo. Depois disso, mudar é criar uma nova entrada no registro — o histórico de por que o sistema é como é nunca se perde.

**Na prática:** a skill `spec-extrema:congelamento` executa este movimento; o registro de decisões vive dentro da SPEC-MASTER.
