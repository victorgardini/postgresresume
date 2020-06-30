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