# Tutorial básico Postgres

## Convenções
- Nomes de bancos: plural. Exemplo: clientes
- Nomes de tabelas: singular. Exemplo: cliente
- Utiliza-se por convenção as palavras chaves do SQL em maiúsculas e os identificadores dosobjetos que criamos em minúsculas.
- Brackets ([ and ]) indicate optional parts.
- Braces ({ and }) and vertical lines (|) indicate that you must choose one alternative.
- Dots (...) mean that the preceding element can be repeated.
- Two dashes (“--”) introduce comments

### Acessar o prompt do psql.
`sudo -u postgres psql`


### Criar um novo cluster
- Indicado para grupos detabelas com muito acesso. Para criar um tablespace utilize o seguinte comando:

`CREATE TABLESPACE nome_tablespace [ OWNER usuário ] LOCATION 'diretório'`

Exemplo: `CREATE TABLESPACE ncluster OWNER usuário LOCATION '/usr/local/pgsql/nc'`

- Criando um banco no novo cluster:
`CREATE DATABASE bdcluster TABLESPACE = ncluster;`

### Comandos de DB
- Ajuda: \h
- Listar todos os tablespaces: \db
- Listar bancos, donos e codificação: \l
- Conectar a um BD: \c nomebanco
- Listar todas as tabelas: \l (requer comando anterior)
- Visualizar esquemas: \dn
- Descreve tabela, índice, seqüência ou view (visão): \d
- Listar usuários e permissões: \du
- Lista grupos: \dg
- lista privilégios de acesso à tabelas, views (visões) e sequências: \dp

## Gerenciar um BD

### Criar um novo BD:
`CREATE DATABASE nomebanco;`

### Excluir um BD:
`DROP DATABASE nomebanco;`

### Fazer bkp de um BD
`pg_dumpall > bancos.sql   .Para preservar os OIDs use a opção -o no pg_dumpall`

### Criar tabelas
Utilize a seguinte estrutura para criar uma tabela
```
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);
```

### Inserir dados nas tabelas
The INSERT statement is used to populate a table with rows:
```
INSERT INTO weather VALUES (
    'San Francisco', 46, 50, 0.25, '1994-11-27'
);
```
An **alternative syntax** allows you to list the columns explicitly:
```
INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
    VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');
```

### Querying a Table
To retrieve data from a table, the table is queried. An SQL SELECT statement is used to do this. Where * is a shorthand for “all columns”
```
SELECT * FROM weather;
```
The same result would be had with:
```
SELECT city, temp_lo, temp_hi, prcp, date FROM weather;
```

#### WHERE, AND, OR, NOT
```
SELECT column1, column2, ...
    FROM table_name
    WHERE condition1 AND condition2 AND condition3 ...;

SELECT column1, column2, ...
    FROM table_name
    WHERE condition1 OR condition2 OR condition3 ...; 

SELECT column1, column2, ...
    FROM table_name
    WHERE NOT condition; 

SELECT * FROM Customers
    WHERE Country='Germany' AND (City='Berlin' OR City='München');
```

#### AVG, COUNT e SUM


#### AS
```
SELECT fqdn AS teste FROM website_domain WHERE id < 10;
```
#### ORDER BY
```
SELECT * FROM weather
    ORDER BY city;

SELECT * FROM weather
    ORDER BY city, temp_lo;
```
#### DISTINCT
You can request that duplicate rows be removed from the result of a query:
```
SELECT DISTINCT city
    FROM weather;
```

Here again, the result row ordering might vary. You can ensure consistent results by using DISTINCT and ORDER BY together:
```
SELECT DISTINCT city
    FROM weather
    ORDER BY city;
```

### Excluir tabelas
`DROP TABLE primeira_tabela`

#### DEFAULT
Ao definir um valor default para um campo, ao ser cadastrado o registro e este campo não forinformado, o valor default é assumido. Caso não seja declarado explicitamente um valordefault, o valor nulo (NULL) será o valor default.
```
CREATE TABLE produtos (
    produto_no integer,
    descricao text,
    preco numeric DEFAULT 9.99
);
```

#### CHECK
Ao criar uma tabela podemos prever que o banco exija que o valor de um campo satisfaçauma expressão.
```
CREATE TABLE produtos (
    produto_no integer,
    descricao text,
    preco numeric CHECK (preco > 0)
);
```

#### NOT NULL
Obrigar o preenchimento de um campo. Ideal para campos importantes que não devem ficarsem preenchimento. Mas devemos ter em mente que até um espaço em branco atende aesta restrição.
```
CREATE TABLE produtos (
    cod_prod integer NOT NULL CHECK (cod_prod > 0),
    nome text NOT NULL,
    preco numeric
);
```
**Obs importante: nulos não são checados. UNIQUE não aceita valores repetidos, mas aceitavários nulos (já que estes não são checados). Cuidado com NULLs.**

#### Unique Constraint
Obrigar valores exclusivos para cada campo em todos os registros.
```
CREATE TABLE produtos (
    cod_prod integer UNIQUE,
    nome text,
    preco numeric
);

CREATE TABLE exemplo (
    a integer,
    b integer,
    c integer,
    UNIQUE (a, c)
);
```

#### Chaves Primárias (Primary Key)
A chave primária de uma tabela é formada internamente pela combinação das constraintsUNIQUE e NOT NULL. Uma tabela pode ter no máximo uma chave primária. A teoria debancos de dados relacional dita que toda tabela deve ter uma chave primária. O PostgreSQLnão obriga que uma tabela tenha chave primária, mas é recomendável seguir, a não ser queesteja criando uma tabela para importar de outra que contenha registros duplicados paratratamento futuro ou algo parecido.
```
CREATE TABLE produtos (
    cod_prod integer PRIMARY KEY,
    nome text,
    preco numeric
);

CREATE TABLE exemplo (
    a integer,
    b integer,
    c integer,
    PRIMARY KEY (a, c)
);
```

#### Chave Estrangeira (Foreign Key)
Criadas com o objetivo de relacionar duas tabelas, mantendo a integridade referencial entreambas.  Especifica que o valor da coluna (ou grupo de colunas) deve corresponder a algumvalor existente em um registro da outra tabela. Normalmente queremos que na tabelaestrangeira existam somente registros que tenham um registro relacionado na tabelaprincipal. Como também garantir que não se remova um registro na tabela principal quetenha registros relacionados na estrangeira.
```
CREATE TABLE produtos (
    cod_prod integer PRIMARY KEY,
    nome text,
    preco numeric
);

CREATE TABLE pedidos (
    cod_pedido integer PRIMARY KEY,
    cod_prod integer,
    quantidade integer,
    CONSTRAINT pedidos_fk FOREIGN KEY (cod_prod) REFERENCES produtos (cod_prod)
);
```

OBS.: Preferir sempre criar FK, utilizando a palavra reservada FOREIGN KEY e não somentecom REFERENCES.
Obviamente, o número de colunas e tipo na restrição devem ser semelhantes ao número etipo das colunas referenciadas.

#### Herança
Podemos criar uma tabela que herda todos os campos de outra tabela existente.
```
CREATE TABLE cidades (
    nome text,
    populacao float,
    altitude int -- (em pés)
);

CREATE TABLE capitais (
    estado char(2)
) INHERITS (cidades);
```
capitais assim passa a ter também todos os campos da tabela cidades.

**Segundo uma entrevista (vide DBFree Magazine No. 2) com a equipe dedesenvolvimento do PostgreSQL, evite utilizar herança de tabelas.**


#### Esquemas (Schema)
Um banco de dados pode conter vários esquemas e dentro de cada um desses podemoscriar várias tabelas. Ao invés de criar vários bancos de dados, criamos um e criamosesquemas dentro desse. Isso permite uma maior flexibilidade, pois uma única conexão aobanco permite acessar todos os esquemas e suas tabelas. Portanto devemos planejar bempara saber quantos bancos precisaremos, quantos esquemas em cada banco e quantastabelas em cada esquema.

Cada banco ao ser criado traz um esquema public, que é onde ficam todas as tabelas, casonão seja criado outro esquema. Este esquema public não é padrão ANSI. Caso se pretendaao portável devemos excluir este esquema public e criar outros. Por default todos os usuárioscriados tem privilégio CREATE e USAGE para o esquema public.

##### Criando Um Esquema
`CREATE SCHEMA nomeesquema;`


##### Excluindo Um Esquema
`DROP SCHEMA nomeesquema;`
Apaga o esquema e todas as suas tabelas, portanto muito cuidado.

Obs.: O padrão SQL exige que se especifique RESTRICT (default no PostgreSQL) OUCASCADE, mas nenhum SGBD segue esta recomendação.

##### Acessando Tabelas Em Esquemas
`SELECT * FROM nomeesquema.nometabela;`


####  Alterações nos objetos dos bancos
##### Adicionar Um Campo
ALTER TABLE tabela ADD COLUMN campo tipo;
ALTER TABLE produtos ADD COLUMN descricao text;

##### Remover Campo
ALTER TABLE tabela DROP COLUMN campo;
ALTER TABLE produtos DROP COLUMN descricao;
ALTER TABLE produtos DROP COLUMN descricao CASCADE; -- Cuidado com CASCADE

##### Adicionar Constraint
ALTER TABLE tabela ADD CONSTRAINT nome;
ALTER TABLE produtos ADD COLUMN descricao text CHECK (descricao <> '');
ALTER TABLE produtos ADD CHECK (nome <> '');
ALTER TABLE produtos ADD CONSTRAINT unique_cod_prod UNIQUE (cod_prod);
ALTER TABLE produtos ADD FOREIGN KEY (cod_produtos) REFERENCES grupo_produtos;
ALTER TABLE produtos ADD CONSTRAINT vendas_fk FOREIGN KEY (cod_produtos)REFERENCES produtos (codigo);

##### Remover Constraint
ALTER TABLE tabela DROP CONSTRAINT nome;
ALTER TABLE produtos DROP CONSTRAINT produtos_pk;
ALTERAR VALOR DEFAULT DE CAMPO:

##### Mudar Tipo de Dados de Campo
ALTER TABLE tabela ALTER COLUMN campo TYPE tipo;
ALTER TABLE produtos ALTER COLUMN preco TYPE numeric(10,2);
ALTER TABLE produtos ALTER COLUMN data TYPE DATE USING CAST (data AS DATE);

##### Mudar Nome De Campo
ALTER TABLE tabela RENAME COLUMN campo_atual TO campo_novo;
ALTER TABLE produtos RENAME COLUMN cod_prod TO cod_produto;

##### Setar/Remover Valor Default de Campo
ALTER TABLE tabela ALTER COLUMN campo SET DEFAULT valor;
ALTER TABLE produtos ALTER COLUMN cod_prod SET DEFAULT 0;
ALTER TABLE produtos ALTER COLUMN preco SET DEFAULT 7.77;
ALTER TABLE tabela ALTER COLUMN campo DROP DEFAULT;
ALTER TABLE produtos ALTER COLUMN preco DROP DEFAULT;

##### Adicionar/Remover NOT NULL
ALTER TABLE produtos ALTER COLUMN cod_prod SET NOT NULL;
ALTER TABLE produtos ALTER COLUMN cod_prod DROP NOT NULL;

##### Renomear Tabela
ALTER TABLE tabela RENAME TO nomenovo;
ALTER TABLE produtos RENAME TO equipamentos;

##### Adicionar Constraint (Restrição)
ALTER TABLE produtos ADD CONSTRAINT produtos_pk PRIMARY KEY (codigo);
ALTER TABLE vendas ADD CONSTRAINT vendas_fk FOREIGN KEY (codigo) REFERENCES produtos(codigo_produto);
ALTER TABLE vendas ADD CONSTRAINT vendas_fk FOREIGN KEY (codigo) REFERENCES produtos; -- Neste caso usa a chave primária da tabela produtos

##### Remover Constraint (Restrição)
ALTER TABLE produtos DROP CONSTRAINT produtos_pk;
ALTER TABLE vendas DROP CONSTRAINT vendas_fk;

### Tipos de Dados


### Restaurar dados de um BD
`/usr/local/pgsql/bin/psql -d postgres -f bancos.sql`