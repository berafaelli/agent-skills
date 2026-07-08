---
name: contratos-de-interface
description: Especifica contratos entre módulos e com APIs externas — formato de dados, erros, versionamento e modos de falha, verificados dos dois lados da fronteira. Use quando for fechar as fronteiras de uma spec nova, ou para auditar os contratos de uma spec ou sistema existente sem rodar o método inteiro.
---

# Contratos de Interface

## Overview

Um contrato é a especificação completa de uma fronteira: o que atravessa (dados), o que dá errado (erros), como evolui (versionamento) e o que acontece quando um lado some (modos de falha). Contratos são a área de minúcia obrigatória número um do método — são o que permite implementar módulos em paralelo e o que mais custa mudar depois. A regra de verificação: **os dois lados de cada fronteira concordam por verificação, não por suposição.**

## When to Use

- Etapa 4 do método `spec-extrema`, depois da decomposição em módulos
- Auditoria isolada: "audita os contratos do [sistema X]" numa spec já existente
- Antes de paralelizar implementação entre pessoas ou agentes
- Uma integração externa nova vai entrar no sistema

**Quando NÃO usar:** detalhes internos de um módulo que nenhum outro módulo vê — isso é conteúdo, não contrato, e pode ficar como dívida com dono.

## Process

### 1. Inventário de fronteiras

Liste todo par de módulos que se tocam (da tabela de decomposição da SPEC-MASTER) e toda API externa consumida ou exposta. Cada item do inventário vira um bloco de contrato. Fronteira fora do inventário é lacuna — o `spec-adversario` vai encontrá-la.

### 2. Dados

Para cada fronteira, o formato exato do que atravessa:

- Tipos de cada campo, nulabilidade explícita ("pode ser null quando…", nunca implícita)
- Limites: tamanho máximo, faixa numérica, cardinalidade de listas
- Unidades e formatos: moeda, fuso horário, encoding, precisão decimal
- Identidade: qual campo identifica o registro, e se é estável entre chamadas

### 3. Erros

Todo erro que cruza a fronteira, em tabela: código, quando ocorre, formato do corpo, **quem trata** (o lado que recebe precisa saber se re-tenta, propaga ou degrada). Erro sem responsável definido é o modo de falha mais comum de sistemas modulares.

### 4. Versionamento

Por contrato: o que é mudança compatível (campo novo opcional?), o que quebra (remoção, mudança de tipo, mudança de semântica), como a versão é sinalizada e qual a política de convivência de versões. Para APIs externas de terceiros: qual versão está sendo assumida e onde isso está pinado.

### 5. Modos de falha

A pergunta padrão, para cada fronteira e em cada direção: *o que este lado vê quando o outro falha?* Especifique timeout (valor), retry (política e limite), fallback (comportamento degradado) e o que chega até o usuário. "Trata o erro" não é resposta.

### 6. Verificação par-a-par

Rode o agente `auditor-de-contratos`: para cada par do inventário, ele compara o que a SPEC-MASTER declara com o que cada SPEC-MODULO expõe na seção Interface, e devolve uma tabela de concordância. Divergência entre lados é correção obrigatória antes de seguir — é exatamente o defeito que este processo existe para impedir.

## Common Rationalizations

| Racionalização | Realidade |
|---|---|
| "Os dois módulos são meus, não preciso formalizar a fronteira" | Daqui a três meses (ou num agente paralelo) 'você' são duas pessoas com memórias diferentes. |
| "Versionamento é prematuro, ainda nem lançamos" | A política custa três linhas agora. Depois do primeiro consumidor externo, custa uma migração. |
| "O JSON de exemplo já documenta o formato" | Exemplo mostra um caso; contrato define todos — nulabilidade, limites e erros não aparecem em exemplo. |
| "Timeout e retry são detalhes de implementação" | São comportamento observável do sistema sob falha. Quem depende da fronteira precisa saber. |

## Red Flags

- Fronteira descrita só do lado de quem expõe (ou só de quem consome)
- Campo sem nulabilidade explícita
- Erro genérico único ("retorna erro 500") para situações diferentes
- Integração externa sem timeout numérico especificado
- Tabela de concordância do auditor com divergências "para resolver depois"

## Verification

- [ ] Inventário cobre todo par de módulos que se tocam e toda API externa
- [ ] Todo campo tem tipo, nulabilidade, limites e unidade
- [ ] Toda linha da tabela de erros tem responsável por tratar
- [ ] Política de versionamento escrita por contrato
- [ ] Timeout, retry e fallback com valores, não com intenções
- [ ] `auditor-de-contratos` rodou e a tabela de concordância está sem divergências
