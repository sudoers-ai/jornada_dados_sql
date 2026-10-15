# Aula 07 - Atualização de Dados (UPDATE) - Nível Especialista

Nos níveis anteriores aprendemos como usar `UPDATE` de forma básica, intermediária e avançada.  
No **nível Especialista**, o foco está em **performance, controle transacional e boas práticas em cenários de grande escala**.

---

## 🔒 Atualizações em transações

Em cenários críticos, é importante garantir que várias atualizações aconteçam de forma **atômica** (ou tudo é aplicado, ou nada é aplicado).

```sql
BEGIN;

UPDATE pedidos
SET status = 'PROCESSADO'
WHERE data < '2025-01-01';

UPDATE clientes
SET ativo = FALSE
WHERE id NOT IN (SELECT DISTINCT cliente_id FROM pedidos);

COMMIT;
```

👉 Se algo falhar no meio, você pode usar `ROLLBACK` para desfazer tudo.

---

## ⚡ Estratégias de performance

1. **Evite atualizar milhões de linhas de uma só vez** → prefira rodar em lotes (ex.: 10.000 registros por execução).  
2. **Desabilite índices e constraints temporariamente** se for uma carga massiva.  
3. **Use particionamento de tabelas** para limitar o escopo das atualizações.  
4. **Execute VACUUM/ANALYZE** em PostgreSQL após grandes operações para liberar espaço e otimizar planos de execução.  

---

## 📊 Atualização com CTE em massa

```sql
WITH novos_valores AS (
    SELECT id, valor * 1.05 AS valor_corrigido
    FROM pedidos
    WHERE status = 'PENDENTE'
)
UPDATE pedidos p
SET valor = nv.valor_corrigido
FROM novos_valores nv
WHERE p.id = nv.id;
```

👉 Atualização massiva, mas de forma organizada e performática.

---

## 🔗 Atualizações complexas com múltiplas tabelas

```sql
UPDATE estoque e
SET quantidade = e.quantidade - v.qtde_vendida
FROM vendas v
WHERE e.produto_id = v.produto_id
AND v.data_venda = CURRENT_DATE;
```

👉 Aqui o estoque é atualizado automaticamente após registrar vendas.

---

## 🧩 Atualizações agendadas

Em pipelines ETL/ELT é comum rodar atualizações periódicas:

```sql
UPDATE clientes
SET ativo = FALSE
WHERE ultima_compra < CURRENT_DATE - INTERVAL '2 years';
```

👉 Marca como inativos clientes que não compram há mais de 2 anos.

---

## ⚠️ Boas práticas de especialista

- Sempre rode grandes atualizações em **transações**.  
- Se possível, faça **backup ou snapshot** antes da execução.  
- Use **monitoramento de locks** para evitar gargalos.  
- Avalie a possibilidade de **MERGE/UPSERT** quando envolver múltiplas tabelas.  
- Teste a query com `EXPLAIN` antes de executar em massa.  

---


📺 **Vídeo complementar:**  
[Aula 07 – Atualizando dados no SQL](https://www.youtube.com/watch?v=u8C4WDZ1y5s&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=10) 
