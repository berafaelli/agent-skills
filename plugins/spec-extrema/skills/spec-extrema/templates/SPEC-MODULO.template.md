# SPEC-MODULO: [Nome do Módulo]

> Formato: MODULE-STANDARD v1
> SPEC-MASTER: [link]
> Status: RASCUNHO | EM ATAQUE | CONGELADA

## 1. Responsabilidade

[Uma frase, sem "e". Se precisar de "e", são dois módulos.]

## 2. Interface

*(O que este módulo expõe. Deve bater, caractere a caractere, com os contratos da SPEC-MASTER que o citam — o `auditor-de-contratos` verifica.)*

### Entradas
| Nome | Tipo | Nulável | Limites/unidade | Origem |
|---|---|---|---|---|

### Saídas
| Nome | Tipo | Nulável | Limites/unidade | Consumidor |
|---|---|---|---|---|

### Erros expostos
| Código | Quando ocorre | Formato | Quem trata |
|---|---|---|---|

## 3. Dados

- **Persistidos:** [entidades, esquema, restrições, retenção]
- **Migrações previstas:** [ou "nenhuma"]
- **Classe de proporcionalidade:** minúcia obrigatória se persiste qualquer coisa

## 4. UX

*(Produzida com `spec-extrema:ux`. Só se aplica a módulos com superfície de usuário; caso contrário, escreva "sem superfície de usuário".)*

### Fluxos
[Cada fluxo como passos numerados verificáveis, não descrição de tela.]

### Matriz de estados
| Tela/passo | Vazio | Carregando | Erro | Sucesso |
|---|---|---|---|---|

### Copy de erro
| Situação | Mensagem | Próxima ação do usuário |
|---|---|---|

### Ações destrutivas
| Ação | Confirmação/undo | Auditoria |
|---|---|---|

## 5. Modos de falha

*(O que acontece quando ESTE módulo falha, do ponto de vista de quem depende dele.)*

| Falha | O que os dependentes veem | O que o usuário vê | Recuperação |
|---|---|---|---|

## 6. Critérios de aceite

*(Todos falseáveis — cada um aponta para um teste no plano da SPEC-MASTER.)*

- [ ] [critério] → teste: [id/nome]
- [ ] …

## 7. Aberto-com-dono

| Item | Dono | Decide até |
|---|---|---|

*(Nada de "TBD" sem dono. Itens de classe "minúcia obrigatória" não podem estar aqui.)*
