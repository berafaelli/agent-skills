# Movimentos de Especificação

Sete movimentos onde esta skill especifica diferente do padrão preguiçoso. Cada um é um
movimento concreto — nomeado, com gatilho e com a classe de bug que evita. Adjetivo sem
movimento por trás não entra nesta lista.

## 1. Contratos antes de conteúdo

Defina a interface de cada par de módulos (dados, erros, versionamento) antes de detalhar o
interior de qualquer módulo.

- **Quando usar:** sempre que dois módulos trocam qualquer coisa; primeiro ato da etapa 3.
- **Que classe de bug evita:** integração que compila dos dois lados e falha em runtime,
  porque cada lado assumiu um formato diferente.

## 2. Mapa de modos de falha por API externa

Para cada API de terceiro, especifique o comportamento do sistema sob rate limit, timeout,
mudança de schema e deprecação — antes de especificar o caminho feliz da integração.

- **Quando usar:** toda API que você não controla, sem exceção para as "confiáveis".
- **Que classe de bug evita:** outage em cascata quando o terceiro degrada e ninguém decidiu
  se o sistema espera, degrada junto ou serve dado velho.

## 3. Inventário de estados impossíveis

Liste as combinações de estado que nunca podem ocorrer (pedido pago e cancelado; estoque
negativo; usuário ativo sem credencial) e o mecanismo que impede cada uma.

- **Quando usar:** etapa 3, depois do dicionário de dados; revisitar no passe adversarial.
- **Que classe de bug evita:** estado inconsistente que nenhum módulo criou sozinho, mas que
  a combinação de dois módulos permite.

## 4. Matriz de dependências com ordem de construção

Grafo explícito de quem depende de quem, com a ordem de construção derivada dele — não da
preferência da equipe.

- **Quando usar:** etapa 2, antes de qualquer estimativa ou alocação.
- **Que classe de bug evita:** retrabalho por dependência circular descoberta com módulo já
  construído, e módulo pronto esperando semanas por dependência não iniciada.

## 5. Caminho infeliz com o mesmo peso do feliz

Para cada fluxo especificado, o comportamento sob erro (quem detecta, quem loga, quem faz
retry, o que o usuário vê) recebe o mesmo espaço de spec que o comportamento de sucesso.

- **Quando usar:** ao escrever qualquer contrato ou fluxo; verificar na etapa adversarial.
- **Que classe de bug evita:** tratamento de erro improvisado na implementação — o `catch`
  vazio, o retry infinito, a mensagem genérica que esconde a causa.

## 6. Decisões com alternativa rejeitada e motivo

Toda escolha estrutural entra no log com a alternativa que perdeu e por quê.

- **Quando usar:** no momento da decisão, não depois; consolidar na etapa de congelamento.
- **Que classe de bug evita:** relitigação da mesma decisão a cada troca de contexto, e
  reversão silenciosa por alguém que não sabia o motivo original.

## 7. Dicionário de dados compartilhado

Formato canônico, unidade e fonte de verdade para todo conceito que cruza fronteira:
dinheiro, datas e fusos, identificadores, medidas.

- **Quando usar:** etapa 3, junto dos contratos; todo conceito citado por 2+ módulos entra.
- **Que classe de bug evita:** bug de conversão entre módulos — centavos contra decimal,
  UTC contra local, ID interno contra ID externo.
