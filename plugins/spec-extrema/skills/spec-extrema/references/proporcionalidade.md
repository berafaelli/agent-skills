# Proporcionalidade de Detalhe

Minúcia uniforme é uma armadilha: a spec rasa deixa lacunas, mas a spec de 3 mil linhas que
ninguém relê é pior — dá a sensação de cobertura sem dar cobertura. Esta regra define onde a
minúcia extrema é obrigatória e onde detalhe demais é dívida.

## Regra de decisão

> **Atravessa uma fronteira (entre módulos ou com terceiro) ou toca dado irreversível
> (dinheiro, dado do cliente, efeito externo)? → detalhe extremo na SPEC-MASTER.
> Caso contrário → delegue à spec do módulo, ou nem especifique.**

## Detalhe extremo obrigatório

| Área | Por quê |
|---|---|
| Contratos entre módulos | É onde dois entendimentos corretos e incompatíveis se encontram; ambiguidade aqui vira bug de integração garantido. |
| Integrações com APIs externas | Você não controla o outro lado; cada comportamento não especificado é uma decisão que o terceiro toma por você. |
| Modelo de dados compartilhado | Dado cruzando fronteira sem formato canônico gera cópias divergentes e bugs de conversão. |
| Segurança e autenticação | Falha aqui é irreversível e silenciosa; "a implementação decide" não é aceitável. |
| Dinheiro e cálculo financeiro | Arredondamento, moeda e unidade erradas produzem prejuízo real e perda de confiança — e auditoria depois não conserta. |

## Detalhe excessivo é dívida

| Área | O que fazer em vez de detalhar na master |
|---|---|
| UI interna de um módulo | Uma frase de intenção na spec do módulo; o detalhe nasce na implementação com `frontend-ui-engineering`. |
| Nomes de variáveis, funções, arquivos internos | Convenção de estilo na seção "Estilo de Código" da spec do módulo — nunca listas de nomes. |
| Otimização prematura | Registre o requisito de performance como critério mensurável e pare; a técnica é decisão de implementação. |
| Layout interno de diretórios de cada módulo | Template padrão na spec do módulo; desvios são decisão do dono do módulo. |
| Fluxos que não cruzam fronteira | Delegue inteiro à spec do módulo. Se a master os descreve, a master está na altitude errada. |

## Teste rápido de altitude

Ao escrever qualquer trecho da SPEC-MASTER, pergunte: **"se este parágrafo estiver errado,
quantos módulos quebram?"**

- Dois ou mais → está na altitude certa, detalhe ao máximo.
- Um → pertence à spec daquele módulo.
- Zero → corte.
