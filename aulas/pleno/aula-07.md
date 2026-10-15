# Aula 07 - Atualização de Dados (UPDATE) - Nível Intermediário

No nível iniciante vimos como atualizar registros simples.  
Agora vamos aprender a aplicar o `UPDATE` em situações mais poderosas, usando **condições, expressões, operadores e múltiplas colunas**.

---

## 🔄 Atualizando várias colunas

É possível alterar mais de uma coluna de uma vez:

```sql
UPDATE clientes
SET nome = 'Maria Souza',
    email = 'maria.souza@email.com',
    idade = 29
WHERE id = 2;
```

---

## 🎯 Atualizações condicionais

Podemos usar operadores lógicos (`AND`, `OR`, `NOT`) no `WHERE`:

```sql
UPDATE clientes
SET ativo = FALSE
WHERE idade > 40 AND ativo = TRUE;
```

👉 Aqui, apenas clientes **com mais de 40 anos** e **ainda ativos** serão desativados.

---

## 🧮 Atualizando com expressões

Você pode calcular novos valores no próprio `UPDATE`:

```sql
UPDATE clientes
SET idade = idade + 1
WHERE EXTRACT(MONTH FROM CURRENT_DATE) = 1;
```

👉 Todos os clientes que fazem aniversário em janeiro terão a idade atualizada automaticamente.  

---

## 🔧 Usando operadores no UPDATE

```sql
UPDATE produtos
SET preco = preco * 1.1
WHERE categoria = 'Eletrônicos';
```

👉 Todos os produtos da categoria **Eletrônicos** tiveram o preço aumentado em **10%**.  

---

## 🧩 Usando funções

```sql
UPDATE clientes
SET nome = UPPER(nome)
WHERE ativo = TRUE;
```

👉 Converte o nome de todos os clientes ativos para **maiúsculo**.  

---

## ⚠️ Boas práticas intermediárias

- Teste sua condição `WHERE` primeiro com um `SELECT`.  
- Evite atualizar grandes volumes sem índices (isso pode travar a tabela).  
- Para cálculos, prefira atualizar em lote com expressões ao invés de linha a linha.  

---


📺 **Vídeo complementar:**  
[Aula 07 – Atualizando dados no SQL](https://www.youtube.com/watch?v=u8C4WDZ1y5s&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=10) 
