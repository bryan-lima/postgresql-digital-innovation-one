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
   local 	    database   user   auth-method 	[auth-options]
   host 	    database   user   addres 	auth-method 	[auth-options]
   hostssl 	    database   user   addres 	auth-method 	[auth-options]
   hostnosssl 	database   user   addres 	auth-method 	[auth-options]
   host 	    database   user   IP-addres 	IP-mask 	auth-method 	[auth-options]
   hostssl 	    database   user   IP-addres 	IP-mask 	auth-method 	[auth-options]
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

---


### Como administrar usuários no banco de dados

---


### Objetos e comandos do banco de dados

---


## Fundamentos da Structured Query Language (SQL)

### Conheça o DML e o Truncate

---


### Funções agregadas em PostgrSQL

---


### Trabalhando com JOINs

---


### Otimizando o código com CTE

---


## Comandos avançados da Structured Query Language (SQL)

### Como as views auxiliam no acesso ao banco de dados

---


### Conheça um dos principais conceitos de banco de dados: transações

---


### Conheça as funções que podem ser criadas pelo desenvolvedor

