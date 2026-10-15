No **INSERT Avançado**, a ideia é mostrar técnicas para **performance**, **cargas massivas**, **transações** e **boas práticas em ambientes reais de produção**.

Aqui vai a versão:

---

# Aula 06 - Inserção de Dados (INSERT) - Avançado

Nos níveis iniciante e intermediário aprendemos a inserir registros básicos, múltiplos e com tratamento de conflitos.  
Agora vamos avançar para técnicas usadas em **cargas de dados maiores**, **otimização de performance** e **boas práticas em sistemas críticos**.

---

## 🚀 Inserção em massa (Bulk Insert)

Para grandes volumes de dados, o método tradicional (`INSERT ... VALUES`) pode ser lento.  
No PostgreSQL, podemos acelerar com:

### 1. Múltiplos registros de uma vez
```sql
INSERT INTO clientes (nome, email, idade)
VALUES
  ('João Almeida', 'joao@email.com', 32),
  ('Carla Dias', 'carla@email.com', 27),
  ('Paulo Nunes', 'paulo@email.com', 41);
```

👉 Evite `INSERT` linha a linha — sempre que possível use **lotes**.

---

### 2. Comando COPY (PostgreSQL)
O `COPY` importa dados diretamente de arquivos para a tabela, muito mais rápido que `INSERT`.  

```sql
COPY clientes (nome, email, idade)
FROM '/tmp/clientes.csv'
DELIMITER ','
CSV HEADER;
```

📌 **Quando usar**: para **cargas iniciais** ou grandes importações.  

---

### 3. Inserindo via ferramenta externa (psql)
Outra forma é usar `\copy` no cliente `psql`, que lê o arquivo do lado do cliente e envia ao servidor:

```bash
\copy clientes (nome, email, idade) FROM 'clientes.csv' DELIMITER ',' CSV HEADER;
```

---

## 🔒 Inserções dentro de transações

Ao inserir muitos dados críticos, é boa prática usar transações:

```sql
BEGIN;

INSERT INTO pedidos (cliente_id, valor) VALUES (1, 100.50);
INSERT INTO pedidos (cliente_id, valor) VALUES (2, 200.75);

COMMIT;
```

👉 Se algo der errado no meio, você pode usar `ROLLBACK` para desfazer.  

---

## 🎯 Inserindo com retorno de valores

O PostgreSQL permite retornar dados gerados automaticamente (como IDs):

```sql
INSERT INTO clientes (nome, email, idade)
VALUES ('Fernanda Gomes', 'fernanda@email.com', 29)
RETURNING id;
```

👉 Muito usado em **sistemas transacionais**, para capturar a chave primária recém-criada.  

---

## ⚡ Otimizando cargas massivas

- **Desative índices e constraints temporariamente** (se possível).  
- **Use COPY ao invés de múltiplos INSERTs**.  
- **Particione os dados** em lotes menores em vez de um único arquivo enorme.  
- **Mantenha transações curtas** para evitar bloqueios.  

---

## 🧩 Exemplo prático de carga massiva

```sql
-- Cria tabela staging (temporária)
CREATE TABLE clientes_staging (
  nome TEXT,
  email TEXT,
  idade INT
);

-- Importa CSV para staging
COPY clientes_staging FROM '/tmp/clientes.csv' DELIMITER ',' CSV HEADER;

-- Insere em lote na tabela final, tratando duplicados
INSERT INTO clientes (nome, email, idade)
SELECT nome, email, idade
FROM clientes_staging
ON CONFLICT (email) DO NOTHING;
```

👉 Essa abordagem evita duplicatas e ainda garante **performance e consistência**.  

---

## ⚠️ Boas práticas avançadas

- Prefira `COPY` para grandes volumes.  
- Use `RETURNING` para capturar IDs em aplicações.  
- Sempre trate conflitos (`ON CONFLICT` / `UPSERT`).  
- Planeje o **rollback** com transações em operações críticas.  
- Em pipelines de dados, utilize **tabelas staging** para limpeza antes de inserir no destino final.  

---

📺 **Vídeo complementar:**  
[Aula 06 – Inserindo dados no SQL](https://www.youtube.com/watch?v=Hj2rkoiW8WY&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=9)

