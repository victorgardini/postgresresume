## Gerenciar um BD

### Criar um novo BD:
`CREATE DATABASE nomebanco;`

### Excluir um BD:
`DROP DATABASE nomebanco;`

### Fazer bkp de um BD
`pg_dumpall > bancos.sql   .Para preservar os OIDs use a opção -o no pg_dumpall`

### Restaurar dados de um BD
`/usr/local/pgsql/bin/psql -d postgres -f bancos.sql`