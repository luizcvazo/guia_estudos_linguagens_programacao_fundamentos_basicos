# Guia de Sintaxe da Linguagem SQL

> Guia objetivo e prático para aprender a sintaxe de SQL e consultar e manipular dados em bancos relacionais. Feito para quem já entende lógica de programação, mas quer destravar a sintaxe. Todos os exemplos usam o mesmo banco de dados de exemplo (seção 2), então você pode copiar, colar e executar tudo.

---

## 1. O que é SQL e por que ele importa

SQL (*Structured Query Language*, "linguagem de consulta estruturada") é a linguagem usada para **conversar com bancos de dados relacionais**. Ela nasceu nos anos **1970** na IBM, foi padronizada nos anos 1980 e continua sendo, meio século depois, uma das habilidades mais pedidas no mercado de tecnologia.

- Praticamente todo sistema que guarda dados usa SQL por baixo: bancos de aplicativos, e-commerces, bancos, hospitais, sistemas de gestão.
- É a base de **análise de dados** e **BI** (Power BI, Tableau, Metabase).
- Toda pessoa desenvolvedora back-end, analista de dados ou engenheira de dados usa SQL.
- Linguagens como Python, Java e C#, quando "falam com o banco", enviam comandos SQL.

### Características principais

| Característica | O que significa na prática |
|---|---|
| **Declarativa** | Você diz **o que** quer, não **como** obter. Não há laços nem `if` para percorrer linhas: você descreve o resultado e o banco decide o caminho. |
| **Baseada em conjuntos** | Um comando trabalha com **tabelas inteiras** (conjuntos de linhas) de uma vez, e não linha por linha. |
| **Não é uma linguagem de propósito geral** | Serve para dados. Não se faz interface gráfica, jogo ou site só com SQL. |
| **Tipagem estática (por coluna)** | Cada coluna tem um tipo (`INTEGER`, `VARCHAR`, `DATE`...) definido na criação da tabela. |
| **Padronizada, mas com "dialetos"** | Existe um padrão (ANSI SQL), mas cada banco (PostgreSQL, MySQL, SQLite, SQL Server, Oracle) tem pequenas diferenças. O básico é igual em todos. |
| **Não diferencia maiúsculas de minúsculas nos comandos** | `select` e `SELECT` são a mesma coisa. Por convenção, palavras-chave em MAIÚSCULAS. |
| **Persistente** | Diferente de variáveis de um programa, os dados ficam **gravados em disco** e sobrevivem ao desligamento. |

### Categorias de comandos

| Categoria | Sigla | Para que serve | Comandos |
|---|---|---|---|
| Definição de dados | **DDL** | Criar e alterar a **estrutura** | `CREATE`, `ALTER`, `DROP` |
| Manipulação de dados | **DML** | Inserir, alterar e apagar **dados** | `INSERT`, `UPDATE`, `DELETE` |
| Consulta de dados | **DQL** | Ler dados | `SELECT` |
| Controle de transação | **TCL** | Confirmar ou desfazer mudanças | `BEGIN`, `COMMIT`, `ROLLBACK` |
| Controle de acesso | **DCL** | Permissões | `GRANT`, `REVOKE` |

Neste guia o foco é **DQL, DML e o básico de DDL**, que é o que se usa no dia a dia.

### SQL vs linguagens de programação (Python, C)

| C / Python | SQL |
|---|---|
| Imperativa: você escreve o **passo a passo** | Declarativa: você descreve o **resultado** |
| Percorre dados com `for` / `while` | Filtra e agrupa com `WHERE` / `GROUP BY` |
| Variáveis vivem na memória e somem ao fechar | Dados vivem em tabelas, gravados em disco |
| `if` / `else` para decidir | `CASE WHEN` dentro da consulta |
| Estruturas: listas, dicionários, `struct` | Estrutura única: **tabelas** (linhas e colunas) |
| Funções e laços para juntar dados | `JOIN` para combinar tabelas |
| Um erro geralmente para o programa | Um erro cancela o comando e o banco continua de pé |

**Resumo prático:** em Python você escreveria um `for` percorrendo uma lista, somando valores e guardando em um dicionário. Em SQL, isso é uma linha: `SELECT cidade, COUNT(*) FROM alunos GROUP BY cidade;`. A mudança de mentalidade, de "como percorrer" para "o que eu quero", é a parte mais difícil de aprender.

### Quando SQL é usado hoje

- **Aplicações**: guardar usuários, pedidos, produtos, mensagens.
- **Análise de dados**: relatórios, dashboards, métricas de negócio.
- **Engenharia de dados**: pipelines, data warehouses (BigQuery, Snowflake, Redshift).
- **Ciência de dados**: extrair e preparar dados antes de usar pandas ou modelos.
- **Qualquer profissão com planilhas grandes demais**: SQL é o "Excel sem limite de linhas".

---

## 2. Como praticar e o banco de exemplo

### Onde rodar SQL sem instalar nada complicado

| Opção | Observação |
|---|---|
| **SQLite** (via [DB Browser for SQLite](https://sqlitebrowser.org/)) | Sem servidor, um único arquivo. Ótimo para começar. |
| **Ferramentas online** (SQLite Online, DB Fiddle, OneCompiler) | Abrem no navegador, zero instalação. |
| **PostgreSQL** ou **MySQL** | Bancos "de verdade" usados em produção. Mais trabalho para instalar. |
| **Módulo `sqlite3` do Python** | Já vem com Python, se você já estuda a linguagem. |

Os exemplos deste guia funcionam em **SQLite** e **PostgreSQL** sem alteração. Quando houver diferença relevante entre bancos, ela está indicada.

### Estrutura de um comando SQL

```sql
SELECT nome, idade      -- o que quero ver (colunas)
FROM alunos             -- de onde (tabela)
WHERE idade >= 30       -- com qual condição (filtro)
ORDER BY nome;          -- em que ordem
```

- Cada comando termina com **ponto e vírgula** `;`.
- Quebras de linha e espaços não importam: use-os para deixar legível.
- Comandos e nomes não diferenciam maiúsculas/minúsculas, mas **os textos dentro de aspas simples** (`'São Paulo'`) dependem do banco e da configuração.

### O banco de exemplo (use em todo o guia)

Vamos usar uma escola fictícia com **alunos**, **cursos** e **matrículas**. Copie e execute este script uma vez:

```sql
CREATE TABLE alunos (
    id      INTEGER PRIMARY KEY,
    nome    VARCHAR(100) NOT NULL,
    email   VARCHAR(100),
    cidade  VARCHAR(50),
    idade   INTEGER
);

CREATE TABLE cursos (
    id             INTEGER PRIMARY KEY,
    nome           VARCHAR(100) NOT NULL,
    carga_horaria  INTEGER,
    preco          DECIMAL(10,2)
);

CREATE TABLE matriculas (
    id              INTEGER PRIMARY KEY,
    aluno_id        INTEGER NOT NULL,
    curso_id        INTEGER NOT NULL,
    data_matricula  DATE,
    nota            DECIMAL(4,2),
    FOREIGN KEY (aluno_id) REFERENCES alunos(id),
    FOREIGN KEY (curso_id) REFERENCES cursos(id)
);

INSERT INTO alunos (id, nome, email, cidade, idade) VALUES
(1, 'Ana Souza',    'ana@email.com',   'São Paulo', 28),
(2, 'Bruno Lima',   'bruno@email.com', 'Campinas',  35),
(3, 'Carla Dias',   'carla@email.com', 'São Paulo', 22),
(4, 'Diego Rocha',  NULL,              'Santos',    41),
(5, 'Eva Martins',  'eva@email.com',   'Campinas',  30);

INSERT INTO cursos (id, nome, carga_horaria, preco) VALUES
(1, 'Python', 40, 300.00),
(2, 'SQL',    30, 250.00),
(3, 'Java',   60, 450.00),
(4, 'Excel',  20, 150.00);

INSERT INTO matriculas (id, aluno_id, curso_id, data_matricula, nota) VALUES
(1, 1, 1, '2026-01-10', 9.5),
(2, 1, 2, '2026-02-15', 8.0),
(3, 2, 1, '2026-01-12', 7.0),
(4, 2, 3, '2026-03-01', NULL),
(5, 3, 2, '2026-02-20', 6.5),
(6, 4, 3, '2026-03-05', 8.5),
(7, 3, 1, '2026-04-01', NULL);
```

Detalhes propositais do banco, para exemplos futuros:

- **Diego** não tem e-mail (`NULL`).
- **Eva** não tem nenhuma matrícula.
- O curso **Excel** não tem nenhum aluno.
- Duas matrículas ainda não têm nota (`NULL`, curso em andamento).

> **No SQLite:** para as chaves estrangeiras (`FOREIGN KEY`) serem realmente verificadas, rode antes `PRAGMA foreign_keys = ON;`.

---

## 3. Comentários

```sql
-- comentário de uma linha (padrão em todos os bancos)

/* comentário
   de várias
   linhas */

SELECT nome  -- comentário no fim da linha
FROM alunos;
```

No MySQL também existe `#` para comentários de linha, mas `--` funciona em todos.

---

## 4. Tipos de dados

Cada coluna de uma tabela tem um tipo. Os nomes variam um pouco entre bancos, mas os principais são estes:

| Categoria | Tipo | O que guarda | Exemplo |
|---|---|---|---|
| Inteiro | `INTEGER` / `INT` | números inteiros | `28` |
| Inteiro | `SMALLINT`, `BIGINT` | inteiros menores / maiores | |
| Decimal exato | `DECIMAL(p,s)` / `NUMERIC(p,s)` | decimais com precisão fixa (ideal para dinheiro). `p` = total de dígitos, `s` = casas decimais | `DECIMAL(10,2)` → `12345678.90` |
| Decimal aproximado | `FLOAT`, `REAL`, `DOUBLE` | decimais de ponto flutuante (mesma imprecisão de `float` em C) | `3.14` |
| Texto | `VARCHAR(n)` | texto de até `n` caracteres | `'Ana Souza'` |
| Texto | `CHAR(n)` | texto de tamanho fixo | `'SP'` |
| Texto | `TEXT` | texto longo, sem limite prático | |
| Data | `DATE` | data | `'2026-01-10'` |
| Data e hora | `TIMESTAMP` / `DATETIME` | data e hora | `'2026-01-10 14:30:00'` |
| Hora | `TIME` | hora | `'14:30:00'` |
| Lógico | `BOOLEAN` | verdadeiro/falso (no SQL Server: `BIT`) | `TRUE` |
| Ausência de valor | `NULL` | "não sei / não existe" (não é um tipo, é um estado) | |

**Regras de escrita de valores:**

```sql
'texto'        -- textos e datas SEMPRE entre aspas SIMPLES
"texto"        -- aspas duplas são para NOMES (tabelas/colunas), não para textos!
42             -- números sem aspas
3.14           -- decimal com PONTO
'2026-01-10'   -- datas no formato ISO: ano-mês-dia
NULL           -- ausência de valor (sem aspas)
```

**Cuidado com dinheiro:** use `DECIMAL`, nunca `FLOAT`. Em `FLOAT`, `0.1 + 0.2` pode dar `0.30000000000000004`, o mesmo problema de C e Python.

**Nota sobre o SQLite:** ele é mais "flexível" que os outros bancos com tipos: aceita quase qualquer valor em qualquer coluna. Em PostgreSQL, MySQL e SQL Server os tipos são rigorosos.

---

## 5. Criando tabelas (`CREATE TABLE`)

Uma **tabela** é como uma planilha: **colunas** definem a estrutura e **linhas** (ou registros) guardam os dados. É o equivalente ao `struct` de C ou à classe de Python, só que guardando muitos registros.

```sql
CREATE TABLE produtos (
    id         INTEGER PRIMARY KEY,
    nome       VARCHAR(100) NOT NULL,
    preco      DECIMAL(10,2) DEFAULT 0,
    estoque    INTEGER DEFAULT 0,
    ativo      BOOLEAN DEFAULT TRUE
);
```

### Restrições (constraints)

Restrições são **regras** que o próprio banco garante. Se um dado violar a regra, o banco recusa.

| Restrição | O que garante |
|---|---|
| `PRIMARY KEY` | Identifica cada linha de forma **única** (não repete, não é nula). Toda tabela deve ter uma. |
| `NOT NULL` | A coluna **não pode ficar vazia**. |
| `UNIQUE` | Não permite **valores repetidos** na coluna (ex.: e-mail, CPF). |
| `DEFAULT valor` | Valor usado quando nada é informado no `INSERT`. |
| `CHECK (condição)` | Só aceita valores que satisfaçam a condição. |
| `FOREIGN KEY` | Só aceita valores que **existam em outra tabela** (integridade referencial). |

```sql
CREATE TABLE usuarios (
    id      INTEGER PRIMARY KEY,
    email   VARCHAR(100) NOT NULL UNIQUE,
    idade   INTEGER CHECK (idade >= 0),
    perfil  VARCHAR(20) DEFAULT 'comum'
);
```

### Chave primária e chave estrangeira

- **Chave primária (PK):** o "RG" da linha. Ex.: `alunos.id`.
- **Chave estrangeira (FK):** uma coluna que **aponta para a PK de outra tabela**. Ex.: `matriculas.aluno_id` aponta para `alunos.id`.

É assim que tabelas se relacionam, o equivalente a um "ponteiro" entre tabelas (só que seguro: o banco impede apontar para algo que não existe).

```
alunos            matriculas              cursos
------            ----------              ------
id (PK)  <------  aluno_id (FK)
nome              curso_id (FK)  ------>  id (PK)
cidade            nota                    nome
```

### Auto incremento (o `id` gerado sozinho)

A sintaxe **varia por banco**:

| Banco | Como declarar |
|---|---|
| PostgreSQL | `id SERIAL PRIMARY KEY` ou `id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY` |
| MySQL | `id INT AUTO_INCREMENT PRIMARY KEY` |
| SQLite | `id INTEGER PRIMARY KEY` (já incrementa sozinho se você omitir o valor) |
| SQL Server | `id INT IDENTITY(1,1) PRIMARY KEY` |

---

## 6. Inserindo dados (`INSERT`)

```sql
-- informando as colunas (recomendado)
INSERT INTO alunos (id, nome, email, cidade, idade)
VALUES (6, 'Fábio Alves', 'fabio@email.com', 'Santos', 27);

-- várias linhas de uma vez
INSERT INTO cursos (id, nome, carga_horaria, preco) VALUES
(5, 'Power BI', 25, 280.00),
(6, 'Git',      10, 100.00);

-- deixando colunas de fora (ficam NULL ou DEFAULT)
INSERT INTO alunos (id, nome) VALUES (7, 'Gustavo Reis');
```

**Boas práticas:**

- **Sempre liste as colunas** depois do nome da tabela. Sem isso, o comando depende da ordem das colunas e quebra se a tabela mudar.
- A quantidade de valores precisa ser igual à quantidade de colunas.
- Se violar uma restrição (por exemplo, `id` repetido ou `NOT NULL` vazio), o comando **falha por inteiro** e nada é inserido.

---

## 7. Consultando dados (`SELECT`)

O `SELECT` é o comando mais importante da linguagem: é com ele que você **lê** dados.

```sql
SELECT * FROM alunos;                     -- todas as colunas, todas as linhas
SELECT nome, cidade FROM alunos;          -- só algumas colunas
```

| nome | cidade |
|---|---|
| Ana Souza | São Paulo |
| Bruno Lima | Campinas |
| Carla Dias | São Paulo |
| Diego Rocha | Santos |
| Eva Martins | Campinas |

> `SELECT *` é ótimo para explorar, mas evite em código de produção: traz colunas desnecessárias e quebra se a tabela mudar.

### Apelidos (`AS`)

```sql
SELECT nome AS aluno, idade AS anos FROM alunos;

SELECT nome, preco * 1.10 AS preco_com_reajuste FROM cursos;   -- cálculo na consulta

SELECT a.nome FROM alunos AS a;     -- apelido para a TABELA (muito usado em JOINs)
SELECT a.nome FROM alunos a;        -- o AS é opcional
```

### `DISTINCT` — removendo repetidos

```sql
SELECT DISTINCT cidade FROM alunos;
```

| cidade |
|---|
| São Paulo |
| Campinas |
| Santos |

### Ordem de escrita vs ordem de execução (essencial para entender erros)

Você **escreve** os comandos nesta ordem:

```
SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY → LIMIT
```

Mas o banco **executa** assim:

```
1. FROM / JOIN   (de onde vêm os dados)
2. WHERE         (filtra linhas)
3. GROUP BY      (agrupa)
4. HAVING        (filtra grupos)
5. SELECT        (escolhe colunas e calcula)
6. ORDER BY      (ordena)
7. LIMIT         (corta)
```

Por isso um apelido criado no `SELECT` (`AS total`) **não funciona no `WHERE`** (o `WHERE` roda antes), mas funciona no `ORDER BY`.

---

## 8. Filtrando com `WHERE`

`WHERE` escolhe **quais linhas** entram no resultado, o "if" do SQL.

```sql
SELECT nome, cidade FROM alunos WHERE cidade = 'São Paulo';
```

| nome | cidade |
|---|---|
| Ana Souza | São Paulo |
| Carla Dias | São Paulo |

### Operadores de comparação

```sql
=            -- igual (UM sinal só, diferente de C e Python)
<>  ou  !=   -- diferente
>   <   >=   <=
```

```sql
SELECT nome, idade FROM alunos WHERE idade >= 30;   -- Bruno, Diego, Eva
SELECT nome FROM alunos WHERE cidade <> 'Santos';
```

**Atenção:** em SQL, igualdade é `=` (e não `==`). O `=` também serve para atribuir em `UPDATE`, mas o contexto deixa claro qual é qual.

### Operadores lógicos: `AND`, `OR`, `NOT`

```sql
SELECT nome FROM alunos WHERE cidade = 'Campinas' AND idade > 30;   -- Bruno

SELECT nome FROM alunos WHERE cidade = 'Santos' OR idade < 25;       -- Carla, Diego

SELECT nome FROM alunos WHERE NOT cidade = 'São Paulo';              -- Bruno, Diego, Eva

-- use parênteses para deixar a precedência clara (AND vem antes de OR)
SELECT nome FROM alunos
WHERE (cidade = 'Campinas' OR cidade = 'Santos') AND idade > 30;     -- Bruno, Diego
```

### `IN` — "está numa lista"

```sql
SELECT nome FROM alunos WHERE cidade IN ('Campinas', 'Santos');   -- Bruno, Diego, Eva
SELECT nome FROM alunos WHERE cidade NOT IN ('Campinas', 'Santos');
```

### `BETWEEN` — "está numa faixa" (inclui as pontas)

```sql
SELECT nome, idade FROM alunos WHERE idade BETWEEN 25 AND 35;   -- Ana, Bruno, Eva
SELECT * FROM matriculas WHERE data_matricula BETWEEN '2026-01-01' AND '2026-02-28';
```

### `LIKE` — busca por padrão em texto

| Curinga | Significado |
|---|---|
| `%` | qualquer quantidade de caracteres (inclusive nenhum) |
| `_` | exatamente **um** caractere |

```sql
SELECT nome FROM alunos WHERE nome LIKE 'A%';       -- começa com A: Ana
SELECT nome FROM alunos WHERE nome LIKE '%a';       -- termina com a: Ana, Carla
SELECT nome FROM alunos WHERE nome LIKE '%ar%';     -- contém "ar": Carla, Martins
SELECT nome FROM alunos WHERE nome LIKE '_va%';     -- segunda letra v... : Eva
```

`LIKE` diferencia maiúsculas de minúsculas em alguns bancos (PostgreSQL) e não em outros (SQLite, MySQL). Para busca sem diferenciar no PostgreSQL, use `ILIKE`.

### `NULL` — o valor mais confuso do SQL

`NULL` significa **"valor desconhecido ou ausente"**. Ele **não é** zero, nem texto vazio, e **não é igual a nada, nem a outro NULL**.

```sql
SELECT nome FROM alunos WHERE email = NULL;      -- ERRADO: nunca retorna nada!
SELECT nome FROM alunos WHERE email IS NULL;     -- CERTO: Diego
SELECT nome FROM alunos WHERE email IS NOT NULL; -- todos menos Diego
```

Qualquer comparação com `NULL` (`=`, `<>`, `>`) resulta em "desconhecido", e o `WHERE` descarta linhas cujo resultado não seja verdadeiro. Por isso é obrigatório usar `IS NULL` / `IS NOT NULL`.

```sql
SELECT 5 + NULL;      -- NULL (qualquer conta com NULL vira NULL)
```

---

## 9. Ordenando e limitando resultados

### `ORDER BY`

```sql
SELECT nome, idade FROM alunos ORDER BY idade;             -- crescente (ASC é o padrão)
SELECT nome, idade FROM alunos ORDER BY idade DESC;        -- decrescente
SELECT nome, cidade, idade FROM alunos
ORDER BY cidade ASC, idade DESC;                            -- por cidade; empate: mais velho primeiro
```

| nome | idade |
|---|---|
| Diego Rocha | 41 |
| Bruno Lima | 35 |
| Eva Martins | 30 |
| Ana Souza | 28 |
| Carla Dias | 22 |

> **Sem `ORDER BY`, a ordem das linhas não é garantida.** Mesmo que hoje pareça vir na ordem de inserção, o banco pode devolver em outra ordem. Se a ordem importa, sempre peça.

### `LIMIT` — só as primeiras linhas

```sql
SELECT nome, idade FROM alunos ORDER BY idade DESC LIMIT 3;          -- os 3 mais velhos
SELECT nome, idade FROM alunos ORDER BY idade DESC LIMIT 3 OFFSET 3; -- pula 3, mostra os próximos
```

**Diferenças entre bancos:**

| Banco | Como limitar |
|---|---|
| PostgreSQL, MySQL, SQLite | `LIMIT 3` |
| SQL Server | `SELECT TOP 3 ...` |
| Oracle / padrão ANSI | `FETCH FIRST 3 ROWS ONLY` |

---

## 10. Funções, cálculos e `CASE`

### Operadores aritméticos

```sql
SELECT nome, preco, preco * 2 AS dobro, preco + 50 AS mais_cinquenta FROM cursos;
```

`+`, `-`, `*`, `/` e `%` (resto, no SQL Server, MySQL, SQLite e PostgreSQL).

**Armadilha clássica (igual à divisão inteira de C):** em PostgreSQL, SQLite e SQL Server, dividir dois inteiros dá inteiro:

```sql
SELECT 5 / 2;        -- 2   (MySQL retorna 2.5000)
SELECT 5.0 / 2;      -- 2.5 (basta um lado ser decimal)
```

### Funções de texto

```sql
SELECT UPPER(nome) FROM alunos;              -- MAIÚSCULAS
SELECT LOWER(nome) FROM alunos;              -- minúsculas
SELECT LENGTH(nome) FROM alunos;             -- tamanho (SQL Server: LEN)
SELECT TRIM('  oi  ');                       -- 'oi'
SELECT SUBSTR(nome, 1, 3) FROM alunos;       -- 3 primeiras letras (SQL Server: SUBSTRING)
SELECT REPLACE(nome, 'a', '@') FROM alunos;

-- concatenar textos
SELECT nome || ' - ' || cidade FROM alunos;          -- SQLite, PostgreSQL, Oracle
SELECT CONCAT(nome, ' - ', cidade) FROM alunos;      -- MySQL, PostgreSQL, SQL Server
```

### Funções numéricas

```sql
SELECT ROUND(3.14159, 2);     -- 3.14
SELECT ABS(-10);              -- 10
SELECT CEIL(4.1);             -- 5  (SQLite antigo pode não ter; SQL Server: CEILING)
SELECT FLOOR(4.9);            -- 4
```

### Funções de data

As funções de data são **as que mais mudam** entre bancos. Exemplos por dialeto:

| Tarefa | PostgreSQL | MySQL | SQLite |
|---|---|---|---|
| Data de hoje | `CURRENT_DATE` | `CURDATE()` | `DATE('now')` |
| Data e hora agora | `NOW()` | `NOW()` | `DATETIME('now')` |
| Extrair o ano | `EXTRACT(YEAR FROM d)` | `YEAR(d)` | `STRFTIME('%Y', d)` |

```sql
-- PostgreSQL
SELECT EXTRACT(YEAR FROM data_matricula) AS ano FROM matriculas;

-- SQLite
SELECT STRFTIME('%Y', data_matricula) AS ano FROM matriculas;
```

Guardando datas no formato ISO (`'2026-01-10'`), comparações com `<`, `>` e `BETWEEN` funcionam direto em qualquer banco.

### `COALESCE` — trocando `NULL` por um valor

```sql
SELECT nome, COALESCE(email, 'sem e-mail') AS contato FROM alunos;
```

| nome | contato |
|---|---|
| Ana Souza | ana@email.com |
| ... | ... |
| Diego Rocha | sem e-mail |

`COALESCE(a, b, c)` devolve o **primeiro valor que não for NULL**.

### `CASE WHEN` — o "if / else" do SQL

```sql
SELECT
    aluno_id,
    curso_id,
    nota,
    CASE
        WHEN nota IS NULL THEN 'Em andamento'
        WHEN nota >= 7    THEN 'Aprovado'
        ELSE 'Reprovado'
    END AS situacao
FROM matriculas;
```

| aluno_id | curso_id | nota | situacao |
|---|---|---|---|
| 1 | 1 | 9.50 | Aprovado |
| 1 | 2 | 8.00 | Aprovado |
| 2 | 1 | 7.00 | Aprovado |
| 2 | 3 | NULL | Em andamento |
| 3 | 2 | 6.50 | Reprovado |
| 4 | 3 | 8.50 | Aprovado |
| 3 | 1 | NULL | Em andamento |

O `CASE` testa as condições **de cima para baixo** e para na primeira verdadeira (como `if / elif / else`). A ordem importa: por isso o teste de `NULL` vem primeiro.

---

## 11. Funções de agregação, `GROUP BY` e `HAVING`

**Funções de agregação** pegam **várias linhas** e devolvem **um único valor**:

| Função | O que faz |
|---|---|
| `COUNT(*)` | conta linhas |
| `COUNT(coluna)` | conta linhas onde a coluna **não é NULL** |
| `SUM(coluna)` | soma |
| `AVG(coluna)` | média |
| `MIN(coluna)` / `MAX(coluna)` | menor / maior |

```sql
SELECT COUNT(*)       AS total_alunos,      -- 5
       AVG(idade)     AS media_idade,       -- 31.2
       MIN(idade)     AS mais_novo,         -- 22
       MAX(idade)     AS mais_velho,        -- 41
       SUM(idade)     AS soma_idades        -- 156
FROM alunos;
```

### `COUNT(*)` vs `COUNT(coluna)`

```sql
SELECT COUNT(*) FROM alunos;       -- 5  (conta linhas)
SELECT COUNT(email) FROM alunos;   -- 4  (ignora o e-mail NULL do Diego)
```

Todas as funções de agregação (exceto `COUNT(*)`) **ignoram `NULL`**. Por exemplo, `AVG(nota)` calcula a média só das notas preenchidas.

### `GROUP BY` — agrupando

`GROUP BY` divide as linhas em **grupos** e aplica a função de agregação **em cada grupo**.

```sql
SELECT cidade, COUNT(*) AS total
FROM alunos
GROUP BY cidade;
```

| cidade | total |
|---|---|
| São Paulo | 2 |
| Campinas | 2 |
| Santos | 1 |

```sql
-- média de idade por cidade
SELECT cidade, ROUND(AVG(idade), 1) AS media_idade
FROM alunos
GROUP BY cidade;

-- quantidade de matrículas e nota média por curso
SELECT curso_id, COUNT(*) AS matriculas, AVG(nota) AS nota_media
FROM matriculas
GROUP BY curso_id;
```

| curso_id | matriculas | nota_media |
|---|---|---|
| 1 | 3 | 8.25 |
| 2 | 2 | 7.25 |
| 3 | 2 | 8.50 |

**Regra de ouro:** toda coluna no `SELECT` que **não** está dentro de uma função de agregação **precisa** estar no `GROUP BY`. Caso contrário, dá erro (ou resultado sem sentido, em alguns bancos).

```sql
SELECT cidade, nome, COUNT(*) FROM alunos GROUP BY cidade;   -- ERRO: "nome" não está agrupado
```

### `HAVING` — filtrando grupos

- `WHERE` filtra **linhas**, **antes** de agrupar.
- `HAVING` filtra **grupos**, **depois** de agrupar (é o único que aceita funções de agregação).

```sql
-- cursos com MAIS de 2 matrículas
SELECT curso_id, COUNT(*) AS matriculas
FROM matriculas
GROUP BY curso_id
HAVING COUNT(*) > 2;
```

| curso_id | matriculas |
|---|---|
| 1 | 3 |

```sql
-- combinando os dois: só matrículas com nota, agrupadas, e só cursos com média >= 8
SELECT curso_id, AVG(nota) AS media
FROM matriculas
WHERE nota IS NOT NULL
GROUP BY curso_id
HAVING AVG(nota) >= 8;
```

```sql
SELECT cidade, COUNT(*) FROM alunos WHERE COUNT(*) > 1 GROUP BY cidade;   -- ERRO: use HAVING
```

---

## 12. `JOIN` — combinando tabelas

Como os dados ficam **separados em tabelas** (alunos, cursos, matrículas), usamos `JOIN` para juntá-los em uma consulta. O `ON` diz **como** as tabelas se conectam (normalmente `chave estrangeira = chave primária`).

### `INNER JOIN` — só o que combina nos dois lados

```sql
SELECT a.nome AS aluno, c.nome AS curso, m.nota
FROM matriculas m
INNER JOIN alunos a ON a.id = m.aluno_id
INNER JOIN cursos c ON c.id = m.curso_id;
```

| aluno | curso | nota |
|---|---|---|
| Ana Souza | Python | 9.50 |
| Ana Souza | SQL | 8.00 |
| Bruno Lima | Python | 7.00 |
| Bruno Lima | Java | NULL |
| Carla Dias | SQL | 6.50 |
| Diego Rocha | Java | 8.50 |
| Carla Dias | Python | NULL |

Repare que **Eva** (sem matrícula) e o curso **Excel** (sem aluno) **não aparecem**: o `INNER JOIN` só traz quem tem correspondência dos dois lados. Escrever apenas `JOIN` é o mesmo que `INNER JOIN`.

### `LEFT JOIN` — tudo da esquerda, mesmo sem correspondência

```sql
SELECT a.nome, m.curso_id
FROM alunos a
LEFT JOIN matriculas m ON m.aluno_id = a.id;
```

Todos os alunos aparecem. Quem não tem matrícula (Eva) vem com `NULL` nas colunas da direita.

**Truque muito usado: achar quem NÃO tem correspondência**

```sql
-- alunos sem nenhuma matrícula
SELECT a.nome
FROM alunos a
LEFT JOIN matriculas m ON m.aluno_id = a.id
WHERE m.id IS NULL;                     -- Eva Martins

-- cursos sem nenhum aluno
SELECT c.nome
FROM cursos c
LEFT JOIN matriculas m ON m.curso_id = c.id
WHERE m.id IS NULL;                     -- Excel
```

### `JOIN` + `GROUP BY` (combinação muito comum)

```sql
-- quantas matrículas cada aluno tem (incluindo quem tem zero)
SELECT a.nome, COUNT(m.id) AS total_matriculas
FROM alunos a
LEFT JOIN matriculas m ON m.aluno_id = a.id
GROUP BY a.id, a.nome
ORDER BY total_matriculas DESC;
```

| nome | total_matriculas |
|---|---|
| Ana Souza | 2 |
| Bruno Lima | 2 |
| Carla Dias | 2 |
| Diego Rocha | 1 |
| Eva Martins | 0 |

Repare no `COUNT(m.id)` (e não `COUNT(*)`): assim, Eva conta **0** em vez de 1, pois `m.id` é `NULL` na linha dela.

```sql
-- faturamento por curso (soma dos preços das matrículas)
SELECT c.nome, COUNT(*) AS matriculas, SUM(c.preco) AS faturamento
FROM matriculas m
JOIN cursos c ON c.id = m.curso_id
GROUP BY c.nome
ORDER BY faturamento DESC;
```

### Resumo visual dos tipos de JOIN

| Tipo | Retorna |
|---|---|
| `INNER JOIN` | Só linhas com correspondência **nas duas** tabelas |
| `LEFT JOIN` | **Todas** da esquerda + correspondentes da direita (`NULL` onde não há) |
| `RIGHT JOIN` | **Todas** da direita + correspondentes da esquerda |
| `FULL JOIN` | **Todas** dos dois lados (não existe no MySQL nem no SQLite antigo) |
| `CROSS JOIN` | Todas as combinações possíveis (produto cartesiano) |

Na prática, `INNER JOIN` e `LEFT JOIN` cobrem quase tudo. Se precisar de um `RIGHT JOIN`, geralmente dá para inverter a ordem das tabelas e usar `LEFT JOIN`.

### Auto-relacionamento e alias obrigatório

Quando a mesma tabela aparece duas vezes (ou colunas têm o mesmo nome), use apelidos (`a`, `m`, `c`) e prefixe a coluna: `a.id`, `m.id`. Sem isso, o banco reclama de coluna **ambígua**.

---

## 13. Subconsultas e CTEs

### Subconsulta — um `SELECT` dentro de outro

```sql
-- alunos mais velhos que a média
SELECT nome, idade
FROM alunos
WHERE idade > (SELECT AVG(idade) FROM alunos);     -- média = 31.2 -> Bruno, Diego
```

```sql
-- alunos matriculados no curso de SQL
SELECT nome
FROM alunos
WHERE id IN (
    SELECT m.aluno_id
    FROM matriculas m
    JOIN cursos c ON c.id = m.curso_id
    WHERE c.nome = 'SQL'
);                                                  -- Ana, Carla
```

```sql
-- EXISTS: "existe pelo menos uma linha?"
SELECT nome
FROM alunos a
WHERE EXISTS (SELECT 1 FROM matriculas m WHERE m.aluno_id = a.id);
```

**Cuidado com `NOT IN` e `NULL`:** se a subconsulta devolver algum `NULL`, `NOT IN` não retorna **nenhuma** linha. Prefira `NOT EXISTS` ou `LEFT JOIN ... IS NULL`.

### CTE (`WITH`) — dando nome a uma subconsulta

Deixa consultas complexas **legíveis**, como criar uma variável temporária:

```sql
WITH total_por_aluno AS (
    SELECT aluno_id, COUNT(*) AS total
    FROM matriculas
    GROUP BY aluno_id
)
SELECT a.nome, t.total
FROM alunos a
JOIN total_por_aluno t ON t.aluno_id = a.id
WHERE t.total >= 2;                 -- Ana, Bruno, Carla
```

---

## 14. Alterando e apagando dados (`UPDATE` e `DELETE`)

### `UPDATE`

```sql
UPDATE alunos
SET cidade = 'Santos', idade = 29
WHERE id = 1;

UPDATE cursos
SET preco = preco * 1.10        -- reajuste de 10%
WHERE nome = 'SQL';
```

### `DELETE`

```sql
DELETE FROM alunos WHERE id = 7;

DELETE FROM matriculas WHERE nota IS NULL;
```

### O perigo do `WHERE` esquecido

```sql
UPDATE alunos SET cidade = 'Santos';    -- ATUALIZA TODAS AS LINHAS!
DELETE FROM alunos;                     -- APAGA TODAS AS LINHAS!
```

Sem `WHERE`, o comando atinge **a tabela inteira**, e o banco não pergunta nada. Hábitos que salvam vidas:

1. Escreva o `WHERE` **antes** de escrever o `SET`/`DELETE`.
2. **Teste primeiro com um `SELECT`** usando o mesmo `WHERE`, para ver quais linhas seriam afetadas:

```sql
SELECT * FROM alunos WHERE id = 7;      -- confere
DELETE FROM alunos WHERE id = 7;        -- só depois apaga
```

3. Em bancos reais, faça as mudanças dentro de uma **transação** (seção 17) e confira antes do `COMMIT`.

### Integridade referencial em ação

```sql
DELETE FROM alunos WHERE id = 1;   -- ERRO: o aluno 1 tem matrículas (chave estrangeira)
```

O banco **impede** apagar um aluno que ainda é referenciado. Para resolver: apague antes as matrículas dele, ou configure `ON DELETE CASCADE` na criação da tabela para que sejam apagadas automaticamente.

---

## 15. Alterando a estrutura (`ALTER` e `DROP`)

```sql
-- adicionar coluna
ALTER TABLE alunos ADD COLUMN telefone VARCHAR(20);

-- renomear coluna
ALTER TABLE alunos RENAME COLUMN telefone TO celular;

-- remover coluna (SQLite: só a partir da versão 3.35)
ALTER TABLE alunos DROP COLUMN celular;

-- renomear tabela
ALTER TABLE alunos RENAME TO estudantes;

-- apagar a tabela inteira (estrutura + dados)
DROP TABLE produtos;

-- só apaga se existir / só cria se não existir (evita erro ao rodar de novo)
DROP TABLE IF EXISTS produtos;
CREATE TABLE IF NOT EXISTS produtos (id INTEGER PRIMARY KEY, nome VARCHAR(100));
```

`DROP TABLE` é **irreversível** (a menos que exista backup). Diferença importante:

| Comando | O que faz |
|---|---|
| `DELETE FROM tabela` | apaga as **linhas**, a tabela continua existindo |
| `TRUNCATE TABLE tabela` | apaga **todas** as linhas de forma rápida (não existe no SQLite) |
| `DROP TABLE tabela` | apaga a tabela **inteira**, estrutura incluída |

---

## 16. Views e índices

### View — uma consulta salva com nome

Uma **view** é uma "tabela virtual": guarda uma consulta pronta que você consulta como se fosse uma tabela.

```sql
CREATE VIEW vw_matriculas_detalhadas AS
SELECT a.nome AS aluno, c.nome AS curso, m.nota, m.data_matricula
FROM matriculas m
JOIN alunos a ON a.id = m.aluno_id
JOIN cursos c ON c.id = m.curso_id;

SELECT * FROM vw_matriculas_detalhadas WHERE curso = 'Python';

DROP VIEW vw_matriculas_detalhadas;
```

Útil para não repetir `JOIN`s grandes toda hora e para expor apenas certas colunas.

### Índice — deixando as buscas rápidas

Um **índice** funciona como o índice de um livro: em vez de ler a tabela inteira, o banco "vai direto" ao ponto.

```sql
CREATE INDEX idx_alunos_cidade ON alunos (cidade);
DROP INDEX idx_alunos_cidade;
```

- Colunas `PRIMARY KEY` e `UNIQUE` já ganham índice automático.
- Vale indexar colunas muito usadas em `WHERE`, `JOIN` e `ORDER BY` em tabelas grandes.
- Contrapartida: índices ocupam espaço e deixam `INSERT`/`UPDATE` um pouco mais lentos. Não indexe tudo.

---

## 17. Transações

Uma **transação** agrupa vários comandos em um "tudo ou nada". Se algo falhar no meio, você desfaz tudo e o banco volta ao estado anterior.

```sql
BEGIN;                                   -- inicia (SQL Server: BEGIN TRANSACTION)

UPDATE contas SET saldo = saldo - 100 WHERE id = 1;
UPDATE contas SET saldo = saldo + 100 WHERE id = 2;

COMMIT;                                  -- confirma: agora é definitivo
-- ou
ROLLBACK;                                -- desfaz tudo desde o BEGIN
```

O exemplo clássico é a transferência bancária: se o dinheiro sai de uma conta, **precisa** entrar na outra. Ou as duas atualizações acontecem, ou nenhuma.

Essa garantia faz parte das propriedades **ACID** dos bancos relacionais: **A**tomicidade (tudo ou nada), **C**onsistência (regras sempre respeitadas), **I**solamento (transações simultâneas não se atrapalham) e **D**urabilidade (o que foi confirmado não se perde).

---

## 18. Modelagem básica de dados

Antes de criar tabelas, pense em **quais entidades existem e como se relacionam**.

| Relacionamento | Exemplo | Como implementar |
|---|---|---|
| **1 para N** | Um aluno tem várias matrículas | Chave estrangeira do lado "N" (`matriculas.aluno_id`) |
| **N para N** | Alunos fazem vários cursos e cursos têm vários alunos | **Tabela intermediária** com duas FKs (`matriculas`) |
| **1 para 1** | Usuário e seu perfil detalhado | FK com `UNIQUE`, ou a mesma PK nas duas tabelas |

Repare que `matriculas` é exatamente uma **tabela intermediária** de um relacionamento N para N entre `alunos` e `cursos`, e ainda guarda dados próprios da relação (`nota`, `data_matricula`).

### Regras de ouro da normalização (versão simples)

1. **Um dado, um lugar.** Não repita o nome do curso em cada matrícula; guarde só o `curso_id`.
2. **Uma célula, um valor.** Nada de guardar `'Python, SQL, Java'` numa coluna. Se há várias, crie outra tabela.
3. **Toda tabela tem chave primária.**
4. **Dados de uma coisa ficam na tabela dessa coisa.** O preço do curso fica em `cursos`, não em `matriculas`.

Assim, se o preço do curso mudar, você altera **uma linha**, e não milhares.

---

## 19. Erros comuns de quem está começando em SQL

| Erro | Por que acontece | Como evitar |
|---|---|---|
| `WHERE coluna = NULL` | `NULL` não é igual a nada | Use `IS NULL` / `IS NOT NULL` |
| Aspas duplas em textos (`"Ana"`) | Aspas duplas são para **nomes** de colunas/tabelas | Textos e datas: aspas **simples** (`'Ana'`) |
| `UPDATE` / `DELETE` sem `WHERE` | O comando atinge a tabela inteira | Teste com `SELECT` antes; use transação |
| Função de agregação no `WHERE` | `WHERE` roda antes do agrupamento | Use `HAVING` |
| Coluna no `SELECT` fora do `GROUP BY` | O banco não sabe qual valor mostrar do grupo | Agrupe por ela ou use uma função de agregação |
| `JOIN` sem `ON` (ou `ON` errado) | Gera o produto cartesiano: cada linha com **todas** da outra tabela (explosão de linhas) | Sempre conecte FK = PK; confira a contagem de linhas |
| `INNER JOIN` "sumindo" linhas | Quem não tem correspondência é descartado | Use `LEFT JOIN` quando quiser manter todos |
| `COUNT(*)` no lugar de `COUNT(coluna)` (ou o contrário) | `COUNT(*)` conta linhas; `COUNT(col)` ignora `NULL` | Escolha conforme o que quer contar |
| `NOT IN` com subconsulta contendo `NULL` | Resultado vira vazio | Use `NOT EXISTS` ou `LEFT JOIN ... IS NULL` |
| Divisão inteira inesperada | `5 / 2` dá `2` em vários bancos | Torne um operando decimal: `5.0 / 2` |
| Confiar na ordem sem `ORDER BY` | Sem `ORDER BY` a ordem não é garantida | Sempre ordene quando a ordem importa |
| `SELECT *` em tudo | Traz dados demais, quebra se a tabela mudar | Liste as colunas necessárias |
| Apelido do `SELECT` no `WHERE` | O `WHERE` executa antes do `SELECT` | Repita a expressão ou use subconsulta/CTE |
| Comparar datas como texto em formato errado | `'10/01/2026'` não ordena/compara corretamente | Use o formato ISO: `'2026-01-10'` |
| Esquecer o `;` entre comandos | O banco não sabe onde um termina | Termine cada comando com `;` |
| Guardar dinheiro em `FLOAT` | Imprecisão de ponto flutuante | Use `DECIMAL(p,s)` |

---

## 20. Tabela-resumo rápida (consulta relâmpago)

```sql
-- ---------- criar e alterar estrutura ----------
CREATE TABLE t (
    id     INTEGER PRIMARY KEY,
    nome   VARCHAR(100) NOT NULL,
    idade  INTEGER DEFAULT 0
);
ALTER TABLE t ADD COLUMN email VARCHAR(100);
DROP TABLE IF EXISTS t;

-- ---------- inserir ----------
INSERT INTO t (id, nome) VALUES (1, 'Ana');

-- ---------- consultar ----------
SELECT DISTINCT coluna1, coluna2 AS apelido
FROM tabela
WHERE idade >= 18 AND cidade IN ('SP', 'RJ') AND nome LIKE 'A%'
ORDER BY idade DESC
LIMIT 10;

-- ---------- agregar ----------
SELECT cidade, COUNT(*), AVG(idade)
FROM alunos
GROUP BY cidade
HAVING COUNT(*) > 1;

-- ---------- juntar tabelas ----------
SELECT a.nome, c.nome
FROM matriculas m
JOIN alunos a ON a.id = m.aluno_id
LEFT JOIN cursos c ON c.id = m.curso_id;

-- ---------- condicional e nulos ----------
SELECT CASE WHEN nota >= 7 THEN 'Aprovado' ELSE 'Reprovado' END,
       COALESCE(email, 'sem e-mail')
FROM alunos;

-- ---------- alterar / apagar dados ----------
UPDATE alunos SET idade = 30 WHERE id = 1;
DELETE FROM alunos WHERE id = 7;

-- ---------- transação ----------
BEGIN;
-- ... comandos ...
COMMIT;   -- ou ROLLBACK;
```

### Cola de sintaxe: Python → SQL

| Tarefa | Python | SQL |
|---|---|---|
| Percorrer todos os itens | `for a in alunos:` | `SELECT * FROM alunos;` |
| Filtrar | `[a for a in alunos if a.idade >= 30]` | `WHERE idade >= 30` |
| Ordenar | `sorted(alunos, key=...)` | `ORDER BY idade DESC` |
| Quantidade | `len(alunos)` | `COUNT(*)` |
| Soma / média / máx | `sum()`, `sum()/len()`, `max()` | `SUM()`, `AVG()`, `MAX()` |
| Agrupar e contar | dicionário com `get(chave, 0) + 1` | `GROUP BY` + `COUNT(*)` |
| Primeiros N | `lista[:3]` | `LIMIT 3` |
| Elementos únicos | `set(lista)` | `DISTINCT` |
| Está em uma lista? | `x in [1, 2, 3]` | `x IN (1, 2, 3)` |
| Se / senão | `if ... elif ... else` | `CASE WHEN ... ELSE ... END` |
| Valor padrão se vazio | `x or "padrão"` / `x if x is not None` | `COALESCE(x, 'padrão')` |
| Juntar duas listas por uma chave | laço aninhado / dicionário | `JOIN ... ON` |
| Alterar um item | `aluno.idade = 30` | `UPDATE ... SET ... WHERE` |
| Remover um item | `lista.remove(x)` | `DELETE FROM ... WHERE` |
| Comparar igualdade | `==` | `=` |
| Diferente | `!=` | `<>` (ou `!=`) |
| E / OU / NÃO | `and` `or` `not` | `AND` `OR` `NOT` |
| Vazio / nulo | `None` | `NULL` (com `IS NULL`) |
| Comentário | `#` | `--` |

---

## 21. Próximos passos sugeridos de estudo

1. **Refazer os exemplos do guia** no banco de exemplo até conseguir escrever cada consulta **sem olhar**. Depois, invente perguntas novas ("qual a cidade com mais alunos?", "qual curso tem a maior nota média?") e responda com SQL.
2. **Dominar `JOIN` e `GROUP BY`**: são o coração do SQL no dia a dia. Pratique combinar 3 tabelas e agregar.
3. **Entender `NULL` de verdade**: é a maior fonte de bugs silenciosos. Teste `COUNT`, `AVG` e `NOT IN` com e sem `NULL`.
4. **Criar seu próprio banco do zero**: escolha um tema (biblioteca, loja, agenda de contatos), desenhe as tabelas no papel, defina PKs e FKs, e só então escreva o `CREATE TABLE`.
5. **Aprender funções de janela (window functions)**: `ROW_NUMBER()`, `RANK()`, `SUM() OVER (...)`. São o próximo nível de análise de dados.
6. **Conectar SQL a uma linguagem**: use o módulo `sqlite3` do Python para rodar consultas e trazer os resultados para dentro de um programa.
7. **Estudar desempenho**: `EXPLAIN`, índices e o custo de uma consulta.

### Onde praticar e consultar

- Praticar consultas com exercícios guiados: SQLBolt, SQLZoo, Mode SQL Tutorial, HackerRank (trilha SQL), LeetCode (Database)
- Visualizar o que um `JOIN` faz: pesquise por "SQL joins visualizer"
- Documentação oficial: [PostgreSQL](https://www.postgresql.org/docs/), [SQLite](https://www.sqlite.org/docs.html), [MySQL](https://dev.mysql.com/doc/)
- Testar comandos no navegador: [DB Fiddle](https://www.db-fiddle.com/) ou [SQLite Online](https://sqliteonline.com/)
- Para desenhar as tabelas antes de criar: dbdiagram.io ou draw.io
