# Aula 08 - Remoção de Dados (DELETE) - Nível Avançado

Nos níveis iniciante e intermediário aprendemos a remover registros simples e condicionais.  
Agora, no nível **Avançado**, vamos explorar técnicas que envolvem **remoções em massa, uso de CTEs e joins entre tabelas**.

---

## 🔥 Remoção em massa

Para excluir grandes quantidades de dados de uma só vez:

```sql
DELETE FROM logs
WHERE data_registro < '2024-01-01';
```

👉 Remove todos os registros de log com mais de um ano.  
⚠️ Operações assim podem ser pesadas — sempre teste antes.

---

## 🧩 Usando CTE (Common Table Expression)

As CTEs ajudam a organizar remoções complexas:

```sql
WITH clientes_inativos AS (
    SELECT id
    FROM clientes
    WHERE ultima_compra < CURRENT_DATE - INTERVAL '2 years'
)
DELETE FROM clientes
WHERE id IN (SELECT id FROM clientes_inativos);
```

👉 Remove todos os clientes que não compram há mais de 2 anos.

---

## 🔗 DELETE com JOIN (dependendo do SGBD)

Alguns bancos permitem `DELETE` diretamente com `JOIN`:

```sql
DELETE FROM pedidos p
USING entregas e
WHERE p.id = e.id_pedido
AND e.status = 'CANCELADO';
```

👉 Remove pedidos relacionados a entregas canceladas.

No PostgreSQL usamos `USING`, enquanto no MySQL é aceito `DELETE ... JOIN`.

---

## 📊 Estratégias de performance

- **Rodar em lotes:** evite apagar milhões de registros de uma vez.  
  ```sql
  DELETE FROM logs WHERE data_registro < '2024-01-01' LIMIT 10000;
  ```
- **Desabilitar índices temporariamente** em remoções massivas (quando possível).  
- **Vacuum/Analyze (PostgreSQL):** libera espaço em disco após grandes exclusões.  
- **Particionamento:** facilita remoção de blocos inteiros de dados antigos.  

---

## ⚠️ Boas práticas avançadas

- Sempre rode operações grandes dentro de **transações (`BEGIN/COMMIT`)** para garantir rollback.  
- Prefira **CTEs e subconsultas** em vez de listas manuais de IDs.  
- Em ambientes críticos, avalie o uso de **soft delete** (marcar como inativo) em vez de apagar fisicamente.  

---

📺 **Vídeo complementar:**  
[Aula 08 – Removendo dados no SQL](https://www.youtube.com/watch?v=qQ4PyIbYOnk&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=11) 
