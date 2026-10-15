# Aula 06 - Inserção de Dados (INSERT)

Nesta aula vamos aprender como **inserir registros em tabelas** usando o comando `INSERT`.  
Esse é um dos comandos mais usados do subconjunto **DML (Data Manipulation Language)**.

---

## 📝 Estrutura básica do INSERT

O comando `INSERT` adiciona novos registros em uma tabela.  
Sua sintaxe geral é:

```sql
INSERT INTO nome_tabela (coluna1, coluna2, coluna3, ...)
VALUES (valor1, valor2, valor3, ...);
```

### 📌 Pontos importantes:
1. O número de colunas deve **bater** com o número de valores.  
2. Os tipos de dados precisam ser **compatíveis** (não tente inserir texto em um campo numérico).  
3. Strings sempre ficam entre aspas simples (`'texto'`).  

---

## 🚀 Exemplo prático

Suponha que temos a tabela `clientes`:

```sql
CREATE TABLE clientes (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100),
    email VARCHAR(100),
    idade INT
);
```

### Inserindo um cliente:
```sql
INSERT INTO clientes (nome, email, idade)
VALUES ('Maria Silva', 'maria@email.com', 28);
```

👉 Aqui, o campo `id` não foi informado porque ele é `SERIAL` (auto-incremento).

---

## 🔄 Inserindo múltiplos registros

Você pode inserir vários registros de uma vez:

```sql
INSERT INTO clientes (nome, email, idade)
VALUES 
    ('João Souza', 'joao@email.com', 32),
    ('Ana Costa', 'ana@email.com', 25),
    ('Pedro Lima', 'pedro@email.com', 40);
```

---

## ⚡ Inserindo em todas as colunas

Se você for preencher **todas as colunas**, pode omitir a lista de colunas:

```sql
INSERT INTO clientes
VALUES (DEFAULT, 'Lucas Rocha', 'lucas@email.com', 35);
```

📌 Aqui usamos `DEFAULT` para a coluna `id` (auto-incremento).  

---

## 🎯 Inserindo a partir de SELECT

É possível inserir dados de outra tabela, sem precisar escrever `VALUES` manualmente:

```sql
INSERT INTO clientes_backup (id, nome, email, idade)
SELECT id, nome, email, idade
FROM clientes
WHERE idade > 30;
```

👉 Isso é útil para **migrar ou copiar dados**.

---

## ⚠️ Boas práticas

- Sempre indique os nomes das colunas (isso evita erros se a tabela mudar no futuro).  
- Verifique se os dados correspondem ao **tipo de dado** da coluna.  
- Prefira `DEFAULT` em colunas automáticas (como `SERIAL`).  
- Use `INSERT ... SELECT` para cargas de dados maiores.  

---

📺 **Vídeo complementar:**  
[Aula 06 – Inserindo dados no SQL](https://www.youtube.com/watch?v=Hj2rkoiW8WY&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=9)

---

