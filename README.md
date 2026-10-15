# Jornada Engenharia de Dados
Bem-vindo ao **Mapa do Engenheiro de Dados**, um programa prático e desafiador que recria situações reais enfrentadas por profissionais da área de dados.  
Neste módulo, você será guiado no aprendizado do **SQL**.

Ao longo do curso, você aprenderá os fundamentos e aplicará técnicas avançadas da linguagem, desenvolvendo consultas transacionais e analíticas até alcançar um nível de maestria.

---

## 🗂️ Sobre o ANSI-SQL
O **ANSI-SQL** é o padrão definido para a **Structured Query Language (SQL)**, a linguagem declarativa utilizada nos principais **Sistemas Gerenciadores de Banco de Dados Relacionais (SGBDs)**.  

Embora o SQL tenha sido inicialmente criado pela IBM, rapidamente surgiram diversos “dialetos” desenvolvidos por outros fabricantes. Para garantir compatibilidade e consistência, o **American National Standards Institute (ANSI)** e a **ISO** definiram um padrão oficial para a linguagem.

As versões mais importantes incluem:
- **SQL-92 (1992)**: consolidou a padronização inicial.  
- **SQL:1999 (SQL3)**: introduziu queries recursivas, triggers, expressões regulares de emparelhamento e recursos de orientação a objetos.  
- **SQL:2003**: adicionou suporte ao XML, sequências padronizadas e colunas de auto-incremento (identity).  

Apesar da padronização, cada fornecedor ainda adiciona extensões próprias. Em geral, a linguagem pode ser migrada entre plataformas com poucas mudanças estruturais.

---

## 🔹 Subconjuntos da SQL

A linguagem SQL é dividida em **subconjuntos**, de acordo com as operações que queremos efetuar sobre um banco de dados.  

### 📊 Tabela comparativa

| Subconjunto | Nome completo | Finalidade | Principais comandos |
|-------------|---------------|------------|---------------------|
| **DDL** | Data Definition Language | Define e altera estruturas do banco de dados (tabelas, índices, etc.) | `CREATE`, `DROP`, `ALTER` |
| **DML** | Data Manipulation Language | Manipula registros dentro das tabelas | `INSERT`, `UPDATE`, `DELETE`, `SELECT INTO` |
| **DCL** | Data Control Language | Gerencia permissões e segurança de acesso | `GRANT`, `REVOKE` |
| **DTL** | Data Transaction Language | Controla transações (início, confirmação e desfazer) | `BEGIN WORK`/`START TRANSACTION`, `COMMIT`, `ROLLBACK` |
| **DQL** | Data Query Language | Consulta e recuperação de dados | `SELECT` |

> 📌 O comando **SELECT** é tecnicamente classificado como **DQL**, mas também é considerado parte essencial do **DML** por muitos fabricantes.


## 🧮 Operadores em SQL

Dentro dos **SGBDs**, existe um grande conjunto de **operadores embutidos**, que permitem realizar cálculos, manipulação de texto, comparações e operações lógicas diretamente nas queries.

## 🧮 Operadores em SQL

Dentro dos **SGBDs**, existe um grande conjunto de **operadores embutidos**, que permitem realizar cálculos, manipulação de texto, comparações e operações lógicas diretamente nas queries.

### 🔢 Operadores Numéricos
- `+` → Soma  
- `-` → Subtração  
- `*` → Multiplicação  
- `/` → Divisão  
- `%` → Resto da divisão  
- `^` → Exponencial  
- `|/` → Raiz quadrada  
- `||/` → Raiz cúbica  
- `!` → Fatorial  

### 🔐 Operadores Lógicos
- `AND` → Retorna verdadeiro se ambas as condições forem verdadeiras.  
- `OR` → Retorna verdadeiro se pelo menos uma condição for verdadeira.  
- `NOT` → Inverte o resultado lógico.  

### ⚡ Operadores Binários
- `&` → Executa um **AND** a nível de bit.  
- `|` → Executa um **OR** a nível de bit.  
- `<<` → Desloca bits para a esquerda.  
- `>>` → Desloca bits para a direita.  

### 📏 Operadores Relacionais
- `<` → Menor que  
- `>` → Maior que  
- `!=` ou `<>` → Diferente de  
- `=` → Igual  
- `>=` → Maior ou igual a  
- `<=` → Menor ou igual a  

### 🔤 Operadores de String
- `||` → Concatenação de strings (PostgreSQL).  
- `CONCAT(str1, str2)` → Concatenação de strings (MySQL).  
- `LIKE` → Busca padrões dentro de uma string.  
- `SIMILAR TO` → Busca padrões com expressões regulares.  

> 🎯 **Dica prática:** operadores são a base de consultas inteligentes. Dominar seu uso garante filtros mais precisos e análises mais poderosas.

---

## 🗃️ Tipos de Dados em SQL

O **ANSI-SQL** define um conjunto de tipos de dados fundamentais, utilizados em quase todos os SGBDs. Cada tipo foi projetado para armazenar informações específicas, garantindo consistência e eficiência nas consultas.

### 🔢 Tipos Numéricos

**Inteiros**  
- `SMALLINT`, `INTEGER`, `BIGINT`: armazenam números inteiros, sem parte fracionária.  
  - `INTEGER` é o mais usado por equilibrar desempenho e espaço.  
  - `SMALLINT` só é útil em cenários com recursos de disco muito limitados.  
  - `BIGINT` deve ser usado apenas quando `INTEGER` não for suficiente.  

**Precisão Arbitrária**  
- `NUMERIC(p, s)` e `DECIMAL(p, s)`: permitem até 1.000 dígitos com precisão exata.  
  - `p` = precisão total.  
  - `s` = escala (quantidade de casas decimais).  
  - Exemplo: `NUMERIC(5,2)` armazena valores até **999,99**.  
  - Ideal para valores monetários.  

**Ponto Flutuante**  
- `REAL`, `FLOAT`, `DOUBLE PRECISION`: tipos não exatos (IEEE 754).  
  - `REAL` → ~6 dígitos de precisão.  
  - `DOUBLE PRECISION` → ~15 dígitos de precisão.  
  - Possuem valores especiais: `Infinity`, `-Infinity`, `NaN`.  
  - ⚠️ Não devem ser usados para cálculos financeiros.  

**Auto-incremento (sequenciais)**  
- `SERIAL`, `BIGSERIAL`: criam colunas com valores únicos automaticamente (similar ao `AUTO_INCREMENT`).  
  - Muito usado para chaves primárias.  

---

### 🔤 Tipos de Caracteres

- `CHAR(n)` → comprimento fixo, preenchido com espaços.  
- `VARCHAR(n)` → comprimento variável com limite.  
- `TEXT` → comprimento variável ilimitado.  

> 📌 Observação: `CHAR` geralmente é pouco usado. `VARCHAR` e `TEXT` são preferidos na prática.

---

### ✅ Tipo Lógico

- `BOOLEAN` → valores `TRUE`, `FALSE` ou `NULL` (desconhecido).  

---

### 🕒 Tipos de Data e Hora

- `DATE` → somente data (AAAA-MM-DD).  
- `TIME[(p)] [WITHOUT TIME ZONE]` → hora.  
- `TIME[(p)] WITH TIME ZONE` → hora com fuso horário.  
- `TIMESTAMP[(p)] [WITHOUT TIME ZONE]` → data e hora.  
- `TIMESTAMP[(p)] WITH TIME ZONE` → data e hora com fuso horário.  
- `INTERVAL [(p)]` → intervalo de tempo.  

Exemplo:
```sql
SELECT CURRENT_DATE, CURRENT_TIME, NOW();
```

---

### 🌍 Outros Tipos de Dados

Além dos básicos, muitos SGBDs adicionam tipos extras:  
- **JSON** → armazenamento de dados em formato JSON válido.  
- **GEOMETRIC** → pontos, linhas, polígonos.  
- **TEXT SEARCH** → tipos especiais para busca textual (`tsvector`, `tsquery`).  
- **XML** → dados estruturados em XML.  

---
---

# Liga Sudoers - Treinamento SQL

Este repositório fornece uma estrutura prática para aprendizado de SQL, incluindo:  
- 1 ambiente PostgreSQL pré-configurado  

📺 Recursos complementares:
- [Curso SQL](https://www.youtube.com/watch?v=aQd5_vcn18U&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ)  
- [Aula 01 - Leitura Otimizada com SELECT](https://www.youtube.com/watch?v=Y6LnIujlbhc&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=3)  
- [Aula 02 - Acessando tabelas com FROM](https://www.youtube.com/watch?v=Kf0NUbhKmjI&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=4)  
- [Aula 03 - Teoria de Conjuntos](https://www.youtube.com/watch?v=YEskDoibdBQ&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=5)  
- [Aula 04 - Usando Operadores](https://www.youtube.com/watch?v=L2yPSbdaODw&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=6)  
- [Aula 05 - WHERE e ORDER BY](https://www.youtube.com/watch?v=ZNzAN_MbaNw&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=7)  
- [Aula 05 - ORDER BY (parte 2)](https://www.youtube.com/watch?v=Fc2p-uj7W4c&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=8)  
- [Aula 06 - Inserção de Dados](https://www.youtube.com/watch?v=Hj2rkoiW8WY&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=9)  
- [Aula 07 - Atualização de Dados](https://www.youtube.com/watch?v=u8C4WDZ1y5s&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=10)  
- [Aula 08 - Remoção de Dados](https://www.youtube.com/watch?v=qQ4PyIbYOnk&list=PLD3-a_5KsN3nuXukrq8kCYtxnZR4FD2nJ&index=11)  


Deixe seu like e comentário nos vídeos do YouTube, isso ajuda e incentiva nosso trabalho. 
---

## 👤 Perfil e Responsabilidades

Perfis de profissionais e suas responsabilidades no dia a dia dos processos mostrados:

### Data Analyst ou Business Intelligence (BI) Analyst
**Responsabilidades:**
- Criar visualizações e relatórios interativos para tomada de decisão.  
- Utilizar ferramentas de BI como Power BI, Tableau, Looker ou Metabase.  
- Definir métricas e KPIs com base nos requisitos de negócios.  
- Trabalhar com stakeholders para transformar dados em insights acionáveis.  

---

## 🎯 Objetivo
Em **2 passos**, você terá o banco **PostgreSQL** rodando localmente com tabelas e dados de exemplo prontos para praticar.  
👉 Não é necessário instalar PostgreSQL diretamente nem configurar manualmente.

---

## 📦 Pré-requisitos
- [Docker Desktop](https://www.docker.com/products/docker-desktop) (Windows/Mac) ou Docker Engine (Linux)  
- Docker Compose (já incluso no Docker Desktop)  
- ~2 GB livres em disco  

---

## 🚀 Como começar

```bash
# 1. Clone este repositório
git clone https://github.com/aquinovale/jornada_dados_sql
cd jornada_dados_sql

# 2. Configure variáveis de ambiente (usuário/senha/banco)
cp .env.example .env

# 3. Suba o container (executa o DDL automaticamente na 1ª vez)
docker compose up -d --build

# 4. Pare o container quando não quiser mais usar
docker compose stop
````

---

## 🛠️ Acessar PostgreSQL

```bash
docker exec -it postgres_liga_sudoers_sql bash 
psql -U sudoers -d liga_sudoers -h postgres_liga_sudoers
```

🔑 **Senha:** `password`

---

## 🤝 Contribuição

Contribuições são bem-vindas!
Abra uma *issue* ou envie um *pull request*.

---

## 📜 Licença

Este projeto está licenciado sob a **Licença SUDOERS**.
