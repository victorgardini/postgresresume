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

### Excluir tabelas
```
DROP TABLE primeira_tabela
```

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


####  Alterações nos objetos dos bancos
##### Adicionar Um Campo
```
ALTER TABLE tabela ADD COLUMN campo tipo;
ALTER TABLE produtos ADD COLUMN descricao text;
```

##### Remover Campo
```
ALTER TABLE tabela DROP COLUMN campo;
ALTER TABLE produtos DROP COLUMN descricao;
ALTER TABLE produtos DROP COLUMN descricao CASCADE; -- Cuidado com CASCADE
```

##### Adicionar Constraint
```ALTER TABLE tabela ADD CONSTRAINT nome;
ALTER TABLE produtos ADD COLUMN descricao text CHECK (descricao <> '');
ALTER TABLE produtos ADD CHECK (nome <> '');
ALTER TABLE produtos ADD CONSTRAINT unique_cod_prod UNIQUE (cod_prod);
ALTER TABLE produtos ADD FOREIGN KEY (cod_produtos) REFERENCES grupo_produtos;
ALTER TABLE produtos ADD CONSTRAINT vendas_fk FOREIGN KEY (cod_produtos)REFERENCES produtos (codigo);
```

##### Remover Constraint
```
ALTER TABLE tabela DROP CONSTRAINT nome;
ALTER TABLE produtos DROP CONSTRAINT produtos_pk;
ALTERAR VALOR DEFAULT DE CAMPO:
```

##### Mudar Tipo de Dados de Campo
```
ALTER TABLE tabela ALTER COLUMN campo TYPE tipo;
ALTER TABLE produtos ALTER COLUMN preco TYPE numeric(10,2);
ALTER TABLE produtos ALTER COLUMN data TYPE DATE USING CAST (data AS DATE);
```

##### Mudar Nome De Campo
```
ALTER TABLE tabela RENAME COLUMN campo_atual TO campo_novo;
ALTER TABLE produtos RENAME COLUMN cod_prod TO cod_produto;
```

##### Setar/Remover Valor Default de Campo
```
ALTER TABLE tabela ALTER COLUMN campo SET DEFAULT valor;
ALTER TABLE produtos ALTER COLUMN cod_prod SET DEFAULT 0;
ALTER TABLE produtos ALTER COLUMN preco SET DEFAULT 7.77;
ALTER TABLE tabela ALTER COLUMN campo DROP DEFAULT;
ALTER TABLE produtos ALTER COLUMN preco DROP DEFAULT;
```

##### Adicionar/Remover NOT NULL
```
ALTER TABLE produtos ALTER COLUMN cod_prod SET NOT NULL;
ALTER TABLE produtos ALTER COLUMN cod_prod DROP NOT NULL;
```

##### Renomear Tabela
```
ALTER TABLE tabela RENAME TO nomenovo;
ALTER TABLE produtos RENAME TO equipamentos;
```

##### Adicionar Constraint (Restrição)
```
ALTER TABLE produtos ADD CONSTRAINT produtos_pk PRIMARY KEY (codigo);
ALTER TABLE vendas ADD CONSTRAINT vendas_fk FOREIGN KEY (codigo) REFERENCES produtos(codigo_produto);
ALTER TABLE vendas ADD CONSTRAINT vendas_fk FOREIGN KEY (codigo) REFERENCES produtos; -- Neste caso usa a chave primária da tabela produtos
```

##### Remover Constraint (Restrição)
```
ALTER TABLE produtos DROP CONSTRAINT produtos_pk;
ALTER TABLE vendas DROP CONSTRAINT vendas_fk;
```