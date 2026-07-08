# SPEC-MASTER: [Nome do Sistema]

> Status: RASCUNHO | EM ATAQUE | CONGELADA
> Última decisão registrada: [#N — data]

## 1. Estratégia

*(Produzida com `spec-extrema:estrategia`, validada pelo humano antes de qualquer decomposição.)*

- **Problema:** [uma frase que um usuário real reconheceria como sua]
- **Por que agora:** [o custo de não construir]
- **Alternativas descartadas:** [comprar X porque…, adaptar Y porque…, não fazer porque…]
- **Fora de escopo (v1):** [lista explícita]
- **Métrica de sucesso:** [uma métrica, um horizonte de tempo]
- **Riscos estratégicos:** [o que invalidaria esta aposta]

## 2. Suposições declaradas

[Tudo que está sendo assumido sem confirmação. Cada item: a suposição e o que muda se ela for falsa.]

1. …
2. …

## 3. Decomposição em módulos

| Módulo | Responsabilidade (uma frase, sem "e") | Spec | Depende de |
|---|---|---|---|
| [nome] | [frase] | [link SPEC-MODULO] | [módulos] |

*(O grafo de dependências não pode ter ciclos. Todo comportamento do sistema pertence a exatamente um módulo.)*

## 4. Contratos de interface

*(Um bloco por fronteira, produzido com `spec-extrema:contratos-de-interface` e auditado par-a-par pelo `auditor-de-contratos`.)*

### Contrato: [Módulo A] ⇄ [Módulo B]

- **Dados:** [formato exato — tipos, nulabilidade, limites, unidades]
- **Erros:** [código, formato, quem trata cada um]
- **Versionamento:** [o que é mudança compatível; o que quebra; como versiona]
- **Modos de falha:** [timeout, indisponibilidade, dado inválido — o que o outro lado vê]

## 5. UX transversal

*(O que vale para o sistema inteiro; o detalhe por módulo vive na SPEC-MODULO, seção UX.)*

- Padrão dos quatro estados (vazio, carregando, erro, sucesso): …
- Padrão de copy de erro (sempre com próxima ação): …
- Padrão de confirmação/undo para ações destrutivas: …

## 6. Plano de testes

*(Produzido com `spec-extrema:testes-de-ferramenta`. Índice rastreável: cada critério de aceite → teste.)*

| Critério de aceite | Teste | Tipo |
|---|---|---|
| … | … | contrato / falha injetada / fluxo / smoke manual |

## 7. Proporcionalidade

| Seção/área | Classe | Se aberta: dono e prazo |
|---|---|---|
| … | minúcia obrigatória / ABERTO | dono: … — decide até: … |

## 8. Registro de decisões

*(Só cresce; nunca se edita entrada antiga. Mudança pós-congelamento = nova entrada.)*

| # | Data | Decisão | Alternativas descartadas | Motivo | Dono |
|---|---|---|---|---|---|
| 1 | … | … | … | … | … |

## 9. Relatórios de ataque adversarial

| Rodada | Data | Lacunas encontradas | Corrigidas em |
|---|---|---|---|
| 1 | … | [n] — [resumo de cada] | [#decisão / seção] |

*(A spec só congela quando uma rodada devolve menos de 3 lacunas novas relevantes.)*

## 10. Checklist das 25

[Respostas por escrito às 25 perguntas de checklist-25.md. Nenhuma pode ser "não sei".]
