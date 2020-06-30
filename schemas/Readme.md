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