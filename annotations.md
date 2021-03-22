# ✍️ Anotações gerais do curso


## Introdução ao banco de dados PostgreSQL

### Fundamentos de banco de dados
#### Dados
- Valores brutos, fatos brutos, observações documentadas, registros soltos, que são recolhidos e armazenados sem sofrer qualquer tratamento

#### Informações
- Estruturação de dados, organização de dados
- Conjunto de dados relacionados entre si que geram valor, que criam sentidos aos dados
- Material do conhecimento

#### Tabela
- Conjunto de dados dispostos em colunas e linhas referentes a um objetivo comum
- As colunas são consideradas como "campos da tabela", como atributos da tabela
- As linhas de uma tabela são chamadas também de tuplas, e é onde estão contidos os valores, os dados

#### O que pode ser definido como tabelas?
- Coisas tangíveis
   - Elementos físicos (carro, produto, animal)
- Funções
   - Perfis de usuário, status de compra
- Eventos ou ocorrências
   - Produtos de um pedido, histórico de dados

#### Colunas importantes
- Chave Primária / Primary Key / PK
   - Conjunto de um ou mais campos que nunca se repetem
   - Identidade da tabela
   - São utilizados como índice de referência na criação de relacionamentos entre tabelas
- Chave Estrangeira / Foreign Key / FK
   - Valor de referência a uma PK de outra tabela ou da mesma tabela para criar um relacionamento

#### Sistemas de gerenciamento de banco de dados (SGBD)
- Ou Sistemas de gestão de base de dados
- Chamamos pela sigla SGBD
- Conjunto de programas ou softwares responsáveis pelo gerenciamento de um banco de dados
- Programas que facilitam a administração de um banco de dados

#### O que é o PostgreSQL?
- Sistema de gerenciamenot de banco de dados objeto relacional
- Teve início no Departamento de Ciência da Computação na Universidade da Califórnia em Berkeley em 1986
- SGBD Opensource

#### Principais características
- OpenSource
- Point in time recovery
- Linguagem procedural com suporte a várias linguages de programação (perl, python, etc)
- Views, functions, procedures, triggers
- Consultas complexas e Common table expressions (CTE)
- Suporte a dados geográficos (PostGIS)
- Controle de concorrência multi-versão

#### Instalação e documentação do PostgreSQL
- Site oficial:
   - https://postgresql.org/
- Download com instruções passo a passo:
   - https://postgresql.org/download/
- Documentação completa:
   - https://postgresql.org/docs/manuals/

---


### Instalação do PostgreSQL no Ubuntu
<!-- #### Instalação no Linux (Ubuntu 18)
- Como root, criar o arquivo:
   - /etc/apt/sources.list.d/pgdg.list
- Adicionar o conteúdo:
   - deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main
- Importar o chave do repositório oficial:
   - wget --quiet -O -https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
- Atualizar os repositórios:
   - sudo apt-get update
- Instalar o PostgreSQL:
   - apt-get install postgresql-11

### pgAdmin
- Ferramenta gráfica para interagir com o banco de dados
- https://www.pgadmin.org/
- Seguir as instruções para download
- Usaremos a versão 4 -->

- Verificar passo a passo no site oficial https://www.postgresql.org/download/

---


### Instalação do PostgreSQL no CentOS/Red Hat

- Verificar passo a passo no site oficial https://www.postgresql.org/download/

---


### Instalação do PostgreSQL no Windows

- Baixar executável (`.exe`) em https://www.enterprisedb.com/downloads/postgres-postgresql-downloads
- Executar e seguir orientações do instalador

---


## Objetos e tipos de dados do PostgreSQL

### O que é o arquivo postgresql.conf
##### Definição
- Arquivo onde estão definidas e armazenadas todas as configurações do servidor PostgreSQL
- Alguns parâmetros só podem ser alterados com uma reinicialização do banco de dados
- A view pg_settings, acessada por dentro do banco de dados, guarda todas as configurações atuais

##### postgresql.conf
- Ao acessar a view pg_settings, é possível visualizar todas as configurações atuais:
```sql
SELECT name, seetings
FROM pg_settings
```
- Ou é possível usar o comando:
```sql
SHOW [parâmetro]
```

##### Localização do arquivo postgresql.conf
- Por padrão, encontra-se dentro do diretório PGDATA definido no momento da inicialização do cluster de banco de dados
- No sistema operacional Ubuntu, se o PostgreSQL foi instalado a partir do repositório oficial, o local do arquivo postgresql.conf será diferente do diretório de dados
```shell
/etc/postgresql/[versão]/[nome do cluster]/postgresql.conf
```

##### Configurações de conexão
- LISTEN_ADDRESS
   - Endereço(s) TCP/IP das interfaces que o servidor PostgreSQL vai escutar/liberar conexões
- PORT
   - A porta TCP que o servidor PostgreSQL vai ouvir
   - O padrão é 5432
- MAX_CONNECTIONS
   - Número máximo de conexões simultâneas no servidor PostgreSQL
- SUPERUSER_RESERVED_CONNECTIONS
   - Número de conexões (slots) reservadas para conexões ao banco de dados de super usuários

##### Configurações de autenticação
- AUTHENTICATION_TIMEOUT
   - Tempo máximo em segundos para o cliente conseguir uma conexão com o servidor
- PASSWORD_ENCRYPTION
   - Algoritmo de criptografia das senhas dos novos usuários criados no banco de dados
- SSL
   - Habilita a conexão criptografada por SSL (somente se o PostgreSQL foi compilado com suporte SSL)

##### Configurações de memória
- SHARED_BUFFERS
   - Tamanho da memória compartilhada do servidor PostgreSQL para cache/buffer da tabelas, índices e demais relações
- WORK_MEM
   - Tamanho da memória para operações de agrupamento e ordenação (ORDER BY, DISTINCT, MERGE JOINS)
- MAINTENANCE_WORK_MEM
   - Tamnho da memória para operações como VACUUM, INDEX, ALTER TABLE


#### O arquivo pg_hba.conf

##### Definição
- Arquivo responsável pelo controle de autenticação dos usuários no servidor PostgreSQL
- O formato do arquivo pode ser:
```
   local 	database   user   auth-method 	[auth-options]
   host 	database   user   addres 	auth-method 	[auth-options]
   hostssl 	database   user   addres 	auth-method 	[auth-options]
   hostnosssl 	database   user   addres 	auth-method 	[auth-options]
   host 	database   user   IP-addres 	IP-mask 	auth-method 	[auth-options]
   hostssl 	database   user   IP-addres 	IP-mask 	auth-method 	[auth-options]
   hostnossl 	database   user   IP-addres 	IP-mask 	auth-method 	[auth-options]
```

##### Métodos de autenticação
- TRUST (conexão sem requisição de senha)
- REJECT (rejeitar conexões)
- MD5 (criptografia md5)
- PASSWORD (senha sem criptografia)
- GSS (generic security service provider application program interface)
- SSPI (secutiry support provider interface - somente para Windows)
- KRB5 (kerberos V5)
- IDENT (utiliza o usuário do sistema operacional do cliente via ident server)
- PEER (utiliza o usuário do sistema operacional do cliente)
- LDAP (ladp server)
- RADIUS (radius server)
- CERT (autenticação via certificado ssl do cliente)
- PAM (pluggable authentication modules. O usuário precisa estar no b)


#### O arquivo pg_ident.conf

##### Definição
- Arquivo responsável por mapear os usuários do sistema operacional com os usuários do banco de dados
- Localizado no diretório de dados PGDATA de sua instalação
- A opção ident deve ser utilizada no arquivo pg_hba.conf


#### Comandos administrativos

##### Ubuntu
- pg_lsclusters
   - Lista todos os clusters PostgreSQL
- pg_createcluster <version> <cluster name>
   - Cria um novo cluster PostgreSQL
- pg_dropcluster <version> <cluster name>
   - Apaga um cluster PostgreSQL
- pg_ctlcluster <version> <cluster> <action>
   - Start, Stop, Status, Restart de clusters PostgreSQL

##### CentOS
- systemctl <action> <cluster>
   - systemctl start postgresql-11
      - Inicia o cluster PostgreSQL
   - systemctl status postgresql-11
      - Mostra o status do cluster PostgreSQL
   - systemctl stop postgresql-11
      - Para o cluster PostgreSQL
   - systemctl restart postgresql-11
      - Restarta o cluster PostgreSQL

##### Windows
- Usando services.msc

##### Binários do PostgreSQL
- createdb
- createuser
- dropdb
- dropuser
- initdb
- pg_ctl
- pg_basebackup
- pg_dump / pg_dumpall
- pg_restore
- psql
- reindexdb
- vacuumdb

##### Arquitetura / Hierarquia

- Cluster
   - Coleção de bancos de dados que compartilham as mesmas configurações (arquivos de configuração) do PostgreSQL e do sistema operacional (porta, listen_address, etc)
- Banco de dados (database)
   - Conjunto de schemas com seus objetos/relações (tabelas, funções, views, etc)
- Schema
   - Conjunto de objetos/relações (tabelas, funções, views, etc)

---


### Conheça a ferramenta PGAdmin

#### PGAdmin
##### Importante para a conexão:

1. Liberar acesso ao cluster em postgresql.conf
2. Liberar acesso ao cluster para o usuário do banco de dados em pg_hba.conf
3. Criar/editar usuários

---


### Como administrar usuários no banco de dados

#### Conceitos users/roles/groups

##### Definição
- Roles (papéis ou funções), users (usuários) e grupos de usuários são "contas", perfis de atuação em um banco de dados, que possuem permissões em comum ou específicas
- Nas versões anteriores do PostgreSQL 8.1, usuários e roles tinham comportamentos diferentes
- Atualmente, roles e users são alias
- É possível que roles pertençam a outros roles

#### Comandos Terminal/Prompt/CMD

- Conexão acessando 127.0.0.1 na porta 5432 usando usuário postgres
```cmd
   psql -h 127.0.0.1 -p 5432 -U postgres
```

- Fechar conexão
```cmd
   \q
```

- Exibir as roles criadas
```cmd
   \du
```

- Query que retorna todas as roles disponíveis no banco de dados
```sql
   SELECT * FROM pg_roles;
```

#### Administrando users/roles/groups

```sql
   CREATE ROLE name [ [ WITH ] option [...] ]
```

   where option can be:

```sql
   SUPERUSER | NOSUPERUSER
   | CREATEDB | NOCREATEDB
   | CREATEROLE | NOCREATEROLE
   | INHERIT | NOINHERIT
   | LOGIN | NOLOGIN
   | REPLICATION | NOREPLICATION
   | BYPASSRLS | NOBYPASSRLS
   | CONNECTION LIMIT connlimit
   | [ENCRYPTED] PASSWORD 'password' | PASSWORD NULL
   | VALID UNTIL 'timestamp'
   | IN ROLE role_name [, ...]
   | IN GROUP role_name [, ...]
   | ROLE role_name [, ...]
   | ADMIN role_name [, ...]
   | USER role_name [, ...]
   | SYSID uid
```


##### Exemplos de criação

```sql
   CREATE ROLE administradores
      CREATEDB
	  CREATEROLE
	  INHERIT
	  NOLOGIN
	  REPLICATION
	  BYPASSRLS
	  CONNECTION LIMIT -1;

   CREATE ROLE professores
      NOCREATEDB
	  NOCREATEROLE
	  INHERIT
	  NOLOGIN
	  NOBYPASSRLS
	  CONNECTION LIMIT 10;

   CREATE ROLE alunos
      NOCREATEDB
	  NOCREATEROLE
	  INHERIT
	  NOLOGIN
	  NOBYPASSRLS
	  CONNECTION LIMIT 90;
```


##### Associação entre roles
- Quando uma role assume as permissões de outra role
   - Necessário a opção INHERIT
- No momento de criação da role:
   - IN ROLE (passa a pertencer a role informada)
   - ROLE (a role informada passa a pertencer a nova role)
- Ou após a criação da role:
   - GRANT [role a ser concedida] TO [role a assumir as permissões]


##### Exemplos de associação

```sql
   CREATE ROLE professores
      NOCREATEDB
	  NOCREATEROLE
	  INHERIT
	  NOLOGIN
	  NOBYPASSRLS
	  CONNECTION LIMIT -1;
   
   CREATE ROLE daniel LOGIN CONNECTION LIMIT 1 PASSWORD '123' IN ROLE professores;
      -- A role daniel  passa a assumir as permissões da role professores
	  
   CREATE ROLE daniel LOGIN CONNECTION LIMIT 1 PASSWORD '123' ROLE professores;
      -- A role professores passa a fazer parte da role daniel assumindo suas permissões
	  
   CREATE ROLE daniel LOGIN CONNECTION LIMIT 1 PASSWORD '123';
   GRANT professores TO daniel;
```


##### Desassociar membros entre roles

```sql
   REVOKE [role que será revogada] FROM [role que terá suas permissões revogadas]
   
   REVOKE professores FROM daniel;
```


##### Alterando uma role

```sql
   ALTER ROLE role_specification [ WITH ] option [...]
```

   where option can be:

```sql
   SUPERUSER | NOSUPERUSER
   | CREATEDB | NOCREATEDB
   | CREATEROLE | NOCREATEROLE
   | CREATEUSER | NOCREATEUSER
   | INHERIT | NOINHERIT
   | LOGIN | NOLOGIN
   | REPLICATION | NOREPLICATION
   | BYPASSRLS | NOBYPASSRLS
   | CONNECTION LIMIT connlimit
   | [ENCRYPTED | UNENCRYPTED] PASSWORD 'password'
   | VALID UNTIL 'timestamp'
```


##### Excluindo uma role

```sql
   DROP ROLE role_specification
```


#### Administrando acessos (GRANT)

##### Definição

- São privilégios de acesso aos objetos do banco de dados

##### Privilégios

<pre>
   -- tabela
   -- coluna
   -- sequence
   -- database
   -- domain
   -- foreign data wrapper
   -- foreign server
   -- function
   -- language
   -- large object
   -- schema
   -- tablespace
   -- type
</pre>


##### Database

```sql
   GRANT {{CREATE | CONNECT | TEMPORARY | TEMP}[, ...] | ALL [PRIVILEGES]}
      ON DATABASE database_name [, ...]
	  TO role_specification [, ...] [WITH GRANT OPTION]
```


##### Schema	  

```sql
   GRANT {{CREATE | USAGE}[, ...] | ALL [PRIVILEGES]}
      ON SCHEMA schema_name [, ...]
	  TO role_specification [, ...] [WITH GRANT OPTION]
```


##### Table

```sql
   GRANT {{SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER}
      [, ...] | ALL [PRIVILEGES]}
	  ON {[TABLE] table_name [, ...]
	     | ALL TABLES IN SCHEMA schema_name [, ...]}
	  TO role_specification [, ...] [WITH GRANT OPTION]
```


##### Revoke

- Retira as permissões da role


###### Database

```sql
   REVOKE [GRANT OPTION FOR]
      {{CREATE | CONNECT TEMPORARY | TEMP} [, ...] | ALL [PRIVILEGES]}
	  ON DATABASE database_name [, ...]
	  FROM {[GROUP] role_name | PUBLIC} [, ...]
	  [CASCADE | RESTRICT]
```


###### Schema

```sql
   REVOKE [GRANT OPTION FOR]
      {{CREATE | USAGE} [, ...] | ALL [PRIVILEGES]}
	  ON SCHEMA schema_name [, ...]
	  FROM {[GROUP] role_name | PUBLIC} [, ...]
	  [CASCADE | RESTRICT]
```


##### Revogando todas as permissões (simplificado)

```sql
   REVOKE ALL ON ALL TABLES IN SCHEMA [schema] FROM [role];
   REVOKE ALL ON SCHEMA [schema] FROM [role];
   REVOKE ALL ON DATABASE [database] FROM [role];
```

---


### Objetos e comandos do banco de dados

#### Database

- É o banco de dados
- Grupo de schemas e seus objetos, como tabelas, types, views, funções, entre outros
- Seus schemas e objetos não podem ser compartilhadas entre si
- Cada database é separado um do outro compartilhando apenas usuários/roles e configurações do cluster PostgreSQL


#### Schema

- É um grupo de objetos, como tabelas, types, views, funções, entre outros
- É possível relacionar objetos entre diversos schemas
- Por exemplo: schema public e schema curso podem ter tabelas com o mesmo nome (teste por exemplo) relacionando-se entre si


#### Objetos

- São as tabelas, views, funções, types, sequences, entre outros, pertencentes aos schemas


#### Manipulação de database

```sql
   CREATE DATABASE name
      [[WITH] [OWNER [=] user_name]
	     [TEMPLATE [=] template]
		 [ENCODING [=] encoding]
		 [LC_COLLATE [=] lc_collate]
		 [LC_CTYPE [=] lc_ctype]
		 [TABLESPACE [=] tablespace_name]
		 [ALLOW_CONNECTIONS [=] allowconn]
		 [CONNECTION LIMIT [=] connlimit]
		 [ISTEMPLATE [=] istemplate]]
	
	
	ALTER DATABASE name RENAME new_name
	ALTER DATABASE name OWNER TO {new_owner | CURRENT_USER | SESSION_USER}
	ALTER DATABASE name SET TABLESPACE new_tablespace
	
	
	DROP DATABASE [nome]
```


#### Manipulação de schema

```sql
   CREATE SCHEMA schema_name [AUTHORIZATION role_specification]
   
   
   ALTER SCHEMA name RENAME TO new_name
   ALTER SCHEMA name OWNER TO {new_owner | CURRENT_USER | SESSION_USER}
   
   
   DROP SCHEMA [nome]
```


##### Melhores práticas

```sql
   CREATE SCHEMA IF NOT EXISTS schema_name [AUTHORIZATION role_specification]
   DROP SCHEMA IF EXISTS [nome];
```


#### Tabelas, Colunas e Tipos de dados

##### Definição

- Conjuntos de dados dispostos em colunas e linhas referentes a um objetivo comum
- As colunas são consideradas como "campos da tabela", como atributos da tabela
- As linhas de uma tabela são chamadas também de tuplas, e é onde estão contidos os valores, os dados


##### Tipos de dados

<pre>
- Numeric Types
- Monetary Types
- Character Types
- Binary Data Types
- Date/Time Types
- Boolean Types
- Enumerated Types
- Geometric Types
- Network Addess Types
- Bit String Types
- Text Search Types
- UUID Type
- XML Type
- JSON Types
- Arrays
- Composite Types
- Range Types
- Domain Types
- Object Identifier Types
- pg_lsn Type
- Pseudo-Types
</pre>


#### DML e DDL

- DML
   - Data Manipulation Language
   - Linguagem de Manipulação de Dados
   - INSERT, UPDATE, DELET, SELECT
      * O SELECT, alguns consideram como DML, outros como DQL, que significa Data Query Language, ou Linguagem de Consulta de Dados
- DDL
   - Data Definition Language
   - Linguage de Definição de Dados
   - CREATE, ALTER, DROP


##### CREATE / ALTER / DROP

```sql
   CREATE [objeto] [nome do objeto] [opções];
   ALTER [objeto] [nome do objeto] [opções];
   DROP [objeto] [nome do objeto] [opções];
```


##### Exemplos

```sql
   CREATE DATABASE dadosbancarios;
   ALTER DATABASE dadosbancarios OWNER TO diretoria;
   DROP DATABASE dadosbancarios;
   
   CREATE SCHEMA IF NOT EXISTS bancos;
   ALTER SCHEMA bancos OWNER TO diretoria;
   DROP SCHEMA IF EXISTS bancos;
```

---


## Fundamentos da Structured Query Language (SQL)

### Conheça o DML e o Truncate

- Idempotência
   - Propriedade que algumas ações/operações possuem possibilitando-as de serem executadas diversas vezes sem alterar o resultado após a aplicação inicial
   - IF EXISTS
   - Comandos pertinentes ao DDL e DML


#### DML - CRUD

##### INSERT - Idempotência

```sql
    INSERT INTO agencia (banco_numero, numero, nome) VALUES (341, 1, 'Centro da cidade');
    
    INSERT INTO agencia (banco_numero, numero, nome)
    SELECT 341, 1, 'Centro da cidade'
    WHERE NOT EXISTS (SELECT banco_numero, numero, nome, FROM agencia 
       WHERE banco_numero = 341 AND numero = 1 AND nome = 'Centro da cidade');
```

##### ON CONFLICT

```sql
    INSERT INTO agencia (banco_numero, numero, nome) VALUES (341, 1, 'Centro da cidade')
    ON CONFLICT (banco_numero, numero) DO NOTHING;
```


#### Truncate

#### Definição

- "Esvaziar" a tabela


```sql
    TRUNCATE [TABLE] [ONLY] name [*] [, ...]
       [RESTART IDENTITY] | [CONTINUE IDENTITY] [CASCADE | RESTRICT]
```


---


### Funções agregadas em PostgrSQL

- AVG
- COUNT (opção: HAVING)
- MAX
- MIN
- SUM

---


### Trabalhando com JOINs

#### Definição

- JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN
- CROSS JOIN


#### JOIN (INNER JOIN)

```sql
   SELECT tabela_1.campos, tabela_2.campos
   FROM tabela_1
   JOIN tabela_2
      ON tabela_2.campo = tabela_1.campo
```


#### LEFT JOIN (LEFT OUTER JOIN)

```sql
   SELECT tabela_1.campos, tabela_2.campos
   FROM tabela_1
   LEFT JOIN tabela_2
      ON tabela_2.campo = tabela_1.campo
```


#### RIGHT JOIN (RIGHT OUTER JOIN)

```sql
   SELECT tabela_1.campos, tabela_2.campos
   FROM tabela_1
   RIGHT JOIN tabela_2
      ON tabela_2.campo = tabela_1.campo
```


#### FULL JOIN (FULL OUTER JOIN)

```sql
   SELECT tabela_1.campos, tabela_2.campos
   FROM tabela_1
   FULL JOIN tabela_2
      ON tabela_2.campo = tabela_1.campo
```


#### CROSS JOIN

- Todos os dados de uma tabela serão cruzados com todos os dados da tabela referenciada no CROSS JOIN criando uma matriz

```sql
   SELECT tabela_1.campos, tabela_2.campos
   FROM tabela_1
   CROSS JOIN tabela_2
```

---


### Otimizando o código com CTE

- Commom Table Expressions - CTE


#### Definição

- Forma auxiliar de organizar "statements", ou seja, blocos de códigos, para consultas muito grandes, gerando tabelas temporárias e criando relacionamentos entre elas
- Dentro das statements podem ter SELECTs, INSERTs, UPDATEs ou DELETEs


#### WITH STATEMENTS
   
```sql
   WITH [nome1] AS (
      SELECT (campos,)
	  FROM tabela_A
	  [WHERE]
   ), [nome2 AS (
      SELECT (campos,)
	  FROM tabela_B
	  [WHERE]
   )
   SELECT [nome1].(campos), [nome2].(campos,)
   FROM [nome1]
   JOIN [nome2] ....
```

---


## Comandos avançados da Structured Query Language (SQL)

### Como as views auxiliam no acesso ao banco de dados

---


### Conheça um dos principais conceitos de banco de dados: transações

---


### Conheça as funções que podem ser criadas pelo desenvolvedor

