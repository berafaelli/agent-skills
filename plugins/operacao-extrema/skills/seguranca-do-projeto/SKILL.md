---
name: seguranca-do-projeto
description: 'Audita a segurança de um projeto inteiro — documentos, specs, código, configs, CI, dependências e dados — e termina em correções priorizadas por severidade. Use quando o pedido for "auditar a segurança do projeto", "revisar a segurança de documentos e código" ou "varredura de segurança completa". NÃO use para endurecer código novo de uma feature (para isso, security-and-hardening), nem como pentest real ou parecer de compliance.'
---

# Segurança do Projeto

## Visão Geral

A maioria das revisões de segurança olha só o código. Mas o vazamento mais barato de
explorar costuma estar fora dele: a chave de API esquecida num documento de arquitetura, a
senha "de exemplo" que é real no `.env.example`, o README que expõe a URL do admin com a
credencial padrão. Esta skill audita o projeto **inteiro** — documentos primeiro, depois
configs, dependências e código — e só termina quando cada achado tem severidade, evidência e
correção proposta. Lista de observações não é auditoria.

**Posicionamento:** a skill `security-and-hardening` (do plugin agent-skills) endurece
código que você está escrevendo, no nível de feature. Esta skill audita um projeto
**existente e completo**, incluindo o que não é código. Para a profundidade técnica de cada
vulnerabilidade de código, delegue a ela e ao `references/security-checklist.md` — não
duplique o checklist aqui.

## Quando Usar

- "Audite a segurança do projeto" / "varredura completa de segurança".
- Antes de um release, de abrir o repositório, ou de entregar o projeto a terceiros.
- Depois de incidente ou de troca grande de dependências.

**Quando NÃO usar:**
- Endurecer uma feature nova em desenvolvimento — use `security-and-hardening`.
- Pentest real com exploração ativa — isso exige especialista e autorização formal.
- Parecer de compliance/jurídico — a etapa de dados é cuidado técnico, não parecer.

## Processo em 5 Etapas

Documentos vêm antes do código de propósito: são o ponto cego habitual e o diferencial
desta auditoria. Cada etapa tem entrada, saída e critério de conclusão.

### 1. Inventário e superfície

- **Entrada:** acesso ao repositório/projeto.
- **Saída:** mapa do que existe — documentos, código, configs, pipelines de CI, lista de
  dependências, pontos de entrada externos (endpoints, formulários, webhooks).
- **Critério:** nada relevante fora do mapa; escopo excluído declarado com motivo.

### 2. Documentos e specs

- **Entrada:** todos os arquivos não-código: READMEs, docs/, specs, wikis exportadas, diagramas.
- **Saída:** achados de exposição em texto: segredos e chaves colados em exemplos, URLs
  internas e credenciais padrão documentadas, detalhes de infraestrutura que facilitam
  ataque, e a pergunta inversa — as specs **exigem** requisitos de segurança ou os ignoram?
- **Critério:** todo documento do inventário varrido; ausência de requisito de segurança em
  spec registrada como achado (severidade Média), não como "ok".

### 3. Código

- **Entrada:** código-fonte do inventário.
- **Saída:** achados orientados pelo OWASP Top 10 — injeção, autenticação quebrada,
  exposição de dados, controle de acesso, configuração insegura. Para o detalhe de cada
  classe, use `security-and-hardening` e o `references/security-checklist.md` como fonte;
  esta etapa localiza e prioriza, não reensina.
- **Critério:** pontos de entrada externos do inventário 100% cobertos; o resto por
  amostragem declarada no relatório.

### 4. Configs, CI e cadeia de suprimentos

- **Entrada:** arquivos de configuração, variáveis de ambiente, pipelines, lockfiles.
- **Saída:** achados de segredo em config commitada, permissão excessiva em pipeline, token
  com escopo largo, dependência desatualizada/abandonada/com vulnerabilidade conhecida.
- **Critério:** toda dependência direta checada; `.env*` e equivalentes lidos um a um.

### 5. Dados e privacidade

- **Entrada:** modelo de dados e fluxos que tocam dados pessoais.
- **Saída:** achados sobre PII em logs, retenção sem prazo, dado pessoal trafegando sem
  necessidade, acesso sem controle. Cuidado técnico adjacente à LGPD — **não é parecer
  jurídico** e o relatório deve dizer isso.
- **Critério:** todo campo de dado pessoal do modelo rastreado até onde ele aparece
  (logs, exports, integrações).

## Saída Obrigatória

Relatório no formato de `templates/RELATORIO-AUDITORIA.template.md`:

- Achados com **Achado | Onde | Evidência | Severidade | Correção** — as 5 colunas sempre.
- Severidades: **Crítica** (segredo exposto, dado pessoal vazando, acesso indevido possível
  agora) / **Alta** / **Média** / **Baixa**.
- Correções priorizadas com dono e prazo.
- Seção "O que foi verificado e está limpo" — obrigatória, para distinguir "seguro" de
  "não olhado".

Para varredura em contexto limpo (quem construiu o projeto não deveria auditá-lo), delegue
ao agente `auditor-de-seguranca`.

## Racionalizações Comuns

| Racionalização | Realidade |
|---|---|
| "Os documentos são internos, não precisam de auditoria" | Documento interno vaza com um clone do repositório. É onde as chaves esquecidas moram. |
| "Isso é dívida conhecida, todo mundo sabe" | Dívida conhecida sem severidade e prazo é dívida aceita para sempre. Entra no relatório com os demais. |
| "A dependência é popular, deve ser segura" | Popularidade mede adoção, não segurança. Checa-se a versão e o histórico, não a fama. |
| "Não achamos nada crítico, auditoria encerrada" | Sem a seção "verificado e limpo", 'nada crítico' é indistinguível de 'não olhamos'. |

## Sinais de Alerta

- Relatório com achados sem severidade ou sem correção proposta.
- Auditoria que começou pelo código e "não deu tempo" de olhar os documentos.
- "Tudo ok" sem lista do que foi verificado.
- Segredo encontrado e corrigido no arquivo, mas não rotacionado (o valor vazado continua válido).
- Achado crítico rebaixado para caber no prazo do release.

## Verificação

- [ ] As 5 etapas executadas, cada uma com saída registrada no relatório.
- [ ] Documentos varridos **antes** do código.
- [ ] Todo achado com as 5 colunas preenchidas (achado, onde, evidência, severidade, correção).
- [ ] Todo segredo encontrado tem correção que inclui **rotação**, não só remoção do arquivo.
- [ ] Seção "O que foi verificado e está limpo" preenchida.
- [ ] Correções priorizadas com dono e prazo.
- [ ] Escopo excluído declarado com motivo.
