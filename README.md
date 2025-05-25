Descrição do projeto para Modelagem ER como conteúdo do curso Suzano - Análise de Dados com Power BI

*** ENTIDADES E ATRIBUTOS ***

1. Produto
id_produto (PK)
nome
descrição
categoria
preço_unitário
código_SKU

2. Fornecedor
id_fornecedor (PK)
razão_social
CNPJ
contato

3. Cliente (generalização/especialização)
id_cliente (PK)
nome
email
telefone
tipo_cliente (PF/PJ)
endereço (relacionado)
CPF (se PF) — atributo exclusivo da subclasse PF
CNPJ (se PJ) — atributo exclusivo da subclasse PJ

(Restrição: um cliente só pode ter um CPF ou um CNPJ; nunca os dois)

4. Estoque
id_estoque (PK)
local (se necessário)
quantidade_disponível
fk_produto (FK)

5. Pedido
id_pedido (PK)
data_pedido
valor_total
desconto_total
tributos
status_pedido (ex: Em andamento, Enviado, Entregue, Cancelado)
prazo_devolucao
código_rastreamento (nullable)
fk_cliente (FK)
fk_endereco_entrega (FK)
fk_forma_pagamento (FK)

6. FormaPagamento
id_forma_pagamento (PK)
tipo (à vista, crédito, débito, boleto, pix)

7. Endereço
id_endereco (PK)
logradouro
número
complemento
cidade
estado
CEP
valor_frete

8. ItemPedido
(entidade associativa entre Pedido e Produto)
id_item_pedido (PK)
fk_pedido (FK)
fk_produto (FK)
quantidade
valor_unitário

*** RELACIONAMENTOS ***
Produto - Fornecedor
N:N — Um produto pode ter vários fornecedores e um fornecedor pode fornecer vários produtos
Entidade associativa: ProdutoFornecedor (com fk_produto, fk_fornecedor)

Produto - Estoque
1:1 ou 1:N (caso exista mais de um depósito)
Um produto possui controle de estoque

Pedido - Produto
N:N via ItemPedido

Cliente - Pedido
1:N — Um cliente pode ter vários pedidos

Pedido - FormaPagamento
1:1 — Cada pedido tem uma única forma de pagamento

Pedido - Endereço
1:1 — O pedido possui um endereço de entrega, com valor de frete associado

*** ESPECIALIZAÇÃO ***
Cliente é generalização com especialização disjunta e total:

Pessoa Física (PF)
CPF
Pessoa Jurídica (PJ)
CNPJ

(Regra: um cliente só pode ser PF ou PJ, nunca ambos)

*** REGRAS DE NEGÓCIO ***
Um produto só é vendido por uma única plataforma, não existindo marketplace com vendedores distintos.
Um pedido pode conter vários produtos.
Só é permitida uma forma de pagamento por pedido.
A entrega pode ter um status (ex: Enviado, Entregue, Cancelado) e código de rastreio.
Produtos podem ser devolvidos, respeitando o prazo de carência definido no pedido.
Não pode haver clientes duplicados (mesmo nome com CPF e CNPJ diferentes).
