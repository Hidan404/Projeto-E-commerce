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

