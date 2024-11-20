# 🛠️ Projeto de Banco de Dados - Esquema Lógico

Bem-vindo ao meu repositório do  projeto de banco de dados! Este projeto apresenta um esquema lógico que modela as operações de um sistema comercial, focado em organização, rastreabilidade e eficiência no gerenciamento de informações.

---

## 📋 Descrição Geral

O projeto foi desenvolvido para representar as principais entidades e suas relações em um ambiente comercial. Ele abrange desde o cadastro de clientes e produtos até a gestão de fornecedores, pedidos, estoques, pagamentos e entregas.

O objetivo é criar um banco de dados funcional, que suporte consultas para responder a perguntas importantes do negócio e que seja extensível para futuras implementações.

---

## 🎯 Objetivos

- **Gerenciar Clientes:** Pessoas físicas e jurídicas com atributos específicos.  
- **Rastrear Entregas:** Acompanhamento dos pedidos até sua conclusão.  
- **Controlar Estoques:** Quantidades e localizações dos produtos.  
- **Registrar Fornecedores:** Empresas responsáveis pelo fornecimento de produtos.  
- **Processar Pagamentos:** Registro de valores, formas de pagamento e vínculos com os pedidos.  

---

## 🗂️ Estrutura do Esquema Lógico

### **Tabelas e Relações**

| **Tabela**           | **Descrição**                                                                                     |
|-----------------------|-------------------------------------------------------------------------------------------------|
| `PessoaFisica`       | Cadastro de clientes pessoa física com CPF e data de nascimento.                                |
| `PessoaJuridica`     | Cadastro de clientes pessoa jurídica com CNPJ, razão social e nome fantasia.                    |
| `Cliente`            | Dados gerais de clientes, vinculados a `PessoaFisica` ou `PessoaJuridica`.                      |
| `Produto`            | Produtos disponíveis para pedidos, categorizados e com valores definidos.                       |
| `Fornecedor`         | Registro de empresas fornecedoras de produtos.                                                 |
| `ProdutoFornecedor`  | Relação entre produtos e fornecedores.                                                          |
| `Pedido`             | Registro de pedidos realizados por clientes, com frete e vínculo com entrega.                  |
| `Entrega`            | Controle de entregas realizadas, com status e rastreabilidade.                                  |
| `Estoque`            | Localizações e controle de quantidades de produtos.                                             |
| `EstoqueProduto`     | Relação entre estoques e produtos, com a quantidade disponível de cada item.                    |
| `Pagamento`          | Detalhes de pagamentos realizados, vinculados aos pedidos.                                      |
| `FormaPagamento`     | Formas de pagamento utilizadas pelos clientes, com vínculo aos pagamentos registrados.          |

---

## 📊 Consultas SQL (Queries)

Abaixo, algumas queries criadas para demonstrar o uso do banco de dados.

### 🔎 Recuperações simples com `SELECT`
**Pergunta:** Quais são os clientes cadastrados, com seu tipo (Pessoa Física ou Jurídica)?  
```sql
SELECT 
    c.Nome, 
    c.TipoCliente, 
    pf.CPF, 
    pj.CNPJ 
FROM Cliente c
LEFT JOIN PessoaFisica pf ON c.PessoaFisicaId = pf.idPessoaFisica
LEFT JOIN PessoaJuridica pj ON c.PessoaJuridicaId = pj.idPessoaJuridica;
```

### 🔎 Filtros com `WHERE`

**Pergunta:** Quais pedidos têm valor de frete maior que R$50,00 e qual é o status da entrega associada?

```sql
SELECT 
    p.idPedido,
    p.Frete,
    e.StatusEntrega
FROM Pedido p
JOIN Entrega e ON p.EntregaId = e.idEntrega
WHERE p.Frete > 50.00;
```

### 🔎 Contagem e agrupamento com `GROUP BY`

**Pergunta:** Quantos pedidos cada cliente fez, agrupados por cliente?

```sql
SELECT 
    c.Nome AS NomeCliente,
    COUNT(p.idPedido) AS TotalPedidos
FROM Cliente c
LEFT JOIN Pedido p ON c.idCliente = p.ClienteId
GROUP BY c.idCliente, c.Nome
ORDER BY TotalPedidos DESC;
```

### 🔎 Expressões para gerar atributos derivados

**Pergunta:** Qual é o valor total pago por pedido?

```sql
SELECT 
    ped.idPedido,
    SUM(pag.Valor) AS ValorTotalPago
FROM Pedido ped
JOIN Pagamento pag ON ped.idPedido = pag.PedidoId
GROUP BY ped.idPedido
HAVING SUM(pag.Valor) > 0;
```

### 🔎 Ordenação com `ORDER BY`

**Pergunta:** Quais produtos estão disponíveis no estoque e em quais localizações?

```sql
SELECT 
    p.Descricao AS NomeProduto,
    e.Localizacao,
    ep.Quantidade
FROM EstoqueProduto ep
JOIN Produto p ON ep.ProdutoId = p.idProduto
JOIN Estoque e ON ep.EstoqueId = e.idEstoque
WHERE ep.Quantidade > 0
ORDER BY ep.Quantidade DESC;
```

### 🔎 Condições de filtros aos grupos com `HAVING`

**Pergunta:** Quais são os fornecedores de cada produto e quantos produtos cada fornecedor fornece?

```sql
SELECT 
    f.RazaoSocial AS NomeFornecedor,
    COUNT(pf.ProdutoId) AS TotalProdutos
FROM Fornecedor f
JOIN ProdutoFornecedor pf ON f.idFornecedor = pf.FornecedorId
GROUP BY f.idFornecedor, f.RazaoSocial
HAVING COUNT(pf.ProdutoId) > 0
ORDER BY TotalProdutos DESC;
```

### 🔎 Junções entre tabelas com `JOIN`

**Pergunta:** Qual a média do valor dos produtos por categoria?

```sql
SELECT 
    p.Categoria,
    AVG(p.Valor) AS MediaValor
FROM Produto p
GROUP BY p.Categoria
HAVING AVG(p.Valor) > 0;
```

### 🔎 Junções entre tabelas com `JOIN` (Continuação)

**Pergunta:** Quais pedidos têm status "Aprovado" e já possuem pagamentos associados, ordenados pela data de pagamento?

```sql
SELECT 
    ped.idPedido,
    ped.StatusPedido,
    pag.DataPagamento,
    pag.Valor AS ValorPago
FROM Pedido ped
JOIN Pagamento pag ON ped.idPedido = pag.PedidoId
WHERE ped.StatusPedido = 'Aprovado'
ORDER BY pag.DataPagamento ASC;
```

### 🔎 Contagem de registros

**Pergunta:** Quantos clientes cadastrados são pessoa física e quantos são pessoa jurídica?

```sql
SELECT 
    CASE 
        WHEN c.PessoaFisicaId IS NOT NULL THEN 'Pessoa Física'
        WHEN c.PessoaJuridicaId IS NOT NULL THEN 'Pessoa Jurídica'
    END AS TipoCliente,
    COUNT(*) AS TotalClientes
FROM Cliente c
GROUP BY TipoCliente;
```

### 🔎 Clientes sem pedidos

**Pergunta:** Quais clientes ainda não realizaram nenhum pedido?

```sql
SELECT 
    c.Nome AS NomeCliente,
    c.Identificacao AS IdentificacaoCliente
FROM Cliente c
LEFT JOIN Pedido p ON c.idCliente = p.ClienteId
WHERE p.idPedido IS NULL;
```





