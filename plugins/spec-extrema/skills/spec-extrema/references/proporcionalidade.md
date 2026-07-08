# Proporcionalidade: onde a minúcia é obrigatória e onde é dívida

Especificação extrema não significa detalhar tudo igualmente — significa saber **onde** o detalhe é inegociável e declarar, com dono, tudo o que ficou aberto. A régua abaixo classifica cada área da spec.

## Minúcia obrigatória

Nessas áreas, lacuna na spec vira defeito caro ou irreversível. Detalhe até o nível de tipos, limites e casos de borda:

| Área | Por quê |
|---|---|
| Contratos entre módulos e APIs externas | É o que trava implementação paralela; mudar depois quebra os dois lados. |
| Dados persistidos e migrações | Dado gravado errado é o defeito mais caro de corrigir; migração não tem undo barato. |
| Autenticação, permissões e limites de acesso | Decisão improvisada aqui é incidente, não bug. |
| Dinheiro: cobrança, créditos, limites de plano | Arredondamento, moeda, idempotência de cobrança — tudo explícito. |
| Ações destrutivas e irreversíveis | Apagar, sobrescrever, publicar, notificar terceiros: confirmação, undo e auditoria especificados. |
| Modos de falha de integrações externas | Timeout, retry, fallback e o que o usuário vê — o fornecedor VAI falhar. |

## Dívida aceitável — com dono e prazo

Nessas áreas, deixar aberto é legítimo, desde que registrado no formato `ABERTO — dono: <nome> — decide até: <etapa ou data>`:

| Área | Condição |
|---|---|
| Layout e organização interna de um módulo | Desde que a interface do módulo esteja congelada. |
| Copy final de textos não críticos | Desde que a copy de erro e de ações destrutivas já esteja especificada. |
| Otimizações de performance | Desde que os limites de aceite (latência máxima, volume) estejam na spec. |
| Escolha de biblioteca/ferramenta interna | Desde que o contrato que ela atende esteja fechado. |
| Telemetria além do mínimo | Desde que a métrica de sucesso da estratégia tenha instrumentação especificada. |

## As duas regras

1. **Dívida sem dono é lacuna.** "TBD", "a definir", "ver depois" reprovam no checklist. `ABERTO — dono: ana — decide até etapa 6` passa.
2. **A classificação é da área, não da preguiça.** Se algo listado como minúcia obrigatória está aberto, a spec não congela — não existe "dívida" em contrato, dado persistido ou dinheiro.

## Teste rápido

Para decidir a classe de uma seção, pergunte: *se quem implementar decidir isso sozinho e errar, o custo é de refazer código ou de refazer o mundo?* Refazer código (um módulo, uma tela) → dívida aceitável. Refazer o mundo (migrar dados, quebrar clientes do contrato, estornar cobranças, pedir desculpas a usuários) → minúcia obrigatória.
