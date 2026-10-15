# Aula 08 - Remoção de Dados (DELETE) - Nível Sudoers 🛡️

No nível **Sudoers**, o `DELETE` vai além de remover registros.  
Ele passa a ser parte de um **ecossistema de governança, auditoria, compliance e automação DataOps/MLOps**, garantindo que os dados sejam geridos com responsabilidade em ambientes críticos e distribuídos.

---

## 🔍 Auditoria e rastreabilidade

Cada remoção deve ser registrada para compliance (LGPD, GDPR, PCI-DSS).  

```sql
CREATE TABLE clientes_audit (
    id SERIAL,
    cliente_id INT,
    acao VARCHAR(10),
    removido_em TIMESTAMP DEFAULT NOW(),
    origem VARCHAR(50)
);

CREATE OR REPLACE FUNCTION log_deletes()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO clientes_audit (cliente_id, acao, origem)
    VALUES (OLD.id, 'DELETE', 'API-SUDOERS');
    RETURN OLD;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER audit_delete
AFTER DELETE ON clientes
FOR EACH ROW
EXECUTE FUNCTION log_deletes();
```

👉 Cada remoção agora é rastreada automaticamente.

---

## 🌍 Deletes em ambientes distribuídos

Em arquiteturas **Big Data** ou **Multicloud**, o `DELETE` precisa ser pensado em escala:  

- **Delta Lake** → garante consistência ACID mesmo em Data Lakehouse.  
- **BigQuery** → permite deletes eficientes em tabelas particionadas.  
- **Sharding** → distribui deletes entre múltiplos nós.  

Exemplo em Delta Lake:

```sql
DELETE FROM delta.`/mnt/datalake/clientes`
WHERE ultima_compra < '2022-01-01';
```

---

## 🤖 DataOps e automação

No nível Sudoers, deletes fazem parte de pipelines automatizados:  

- **Orquestração** com Airflow/Dagster/dbt.  
- **CI/CD de dados**: scripts de delete versionados e revisados.  
- **Alertas**: remoções críticas geram eventos no Grafana/Prometheus.  

Exemplo com Airflow:

```python
delete_task = PostgresOperator(
    task_id="delete_inativos",
    postgres_conn_id="postgres_sudoers",
    sql="sql/delete_clientes_inativos.sql"
)
```

---

## 🧬 Deletes em MLOps

- **Feature Store** → remoções de features inválidas.  
- **Treinamento de modelos** → exclusão de dados corrompidos ou fora de compliance.  
- **Time Travel** → possibilita reverter deletes e restaurar datasets históricos.  

Exemplo em Delta Lake com Time Travel:

```sql
DELETE FROM features_churn
WHERE score_risco IS NULL;
```

👉 Depois, é possível consultar a versão anterior com `VERSION AS OF`.

---

## ⚔️ Boas práticas Sudoers

- Sempre **auditar** deletes em ambientes críticos.  
- Usar **linhagem de dados** para rastrear impacto.  
- Considerar **soft delete** em sistemas regulados (nunca perder histórico).  
- Avaliar impacto de remoções em **replicações e sincronizações multicloud**.  
- Integrar deletes a **estratégias de arquivamento** em Data Lake ou DW.  

---

📺 **Vídeo complementar:**  
[Aula 08 – Removendo dados no SQL](https://www.youtube.com/watch?v=qQ4PyIbYOnk&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=11) 
