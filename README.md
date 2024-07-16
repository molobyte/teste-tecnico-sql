
# Teste Técnico de Conhecimentos em SQL

Respostas do teste técnico de conhecimentos em SQL. 

## Respostas

1. **Consulta de Funcionários**: 

```

SELECT id_vendedor as id, nome, salario
 FROM vendedores
 WHERE inativo  = false
 ORDER BY nome;

```

2. **Funcionários com Salário Acima da Média**: 

```

SELECT id_vendedor as id, nome, salario
 FROM vendedores
 WHERE salario > (SELECT AVG(salario) FROM VENDEDORES)
 ORDER BY salario DESC;

```

3. **Resumo por cliente**: 

```

SELECT
 c.id_cliente AS id,
 c.razao_social,
 COALESCE(SUM(p.valor_total), 0) AS total
FROM
 clientes c
LEFT JOIN
 pedido p ON c.id_cliente = p.id_cliente
GROUP BY
 c.id_cliente
ORDER BY
 total DESC;

```

4. **Situação por pedido**: 

```

SELECT
 id_pedido as id,
 valor_total as valor,
 data_emissao as data,
 CASE
  WHEN data_cancelamento IS NOT NULL THEN 'CANCELADO'
  WHEN data_faturamento IS NOT NULL THEN 'FATURADO'
  ELSE 'PENDENTE'
 END AS situacao
FROM pedido;

```

5. **Produtos mais vendidos**: 

```

SELECT
 i.id_produto,
 SUM(i.quantidade) AS quantidade_vendida,
 SUM(i.preco_praticado * i.quantidade) AS total_vendido,
 COUNT(i.id_pedido) AS pedidos,
 COUNT(DISTINCT p.id_cliente) AS clientes
FROM
 itens_pedido i
JOIN
 pedido p ON i.id_pedido = p.id_pedido
GROUP BY
 i.id_produto
ORDER BY
 quantidade_vendida DESC,
 total_vendido DESC
LIMIT 1;

```
