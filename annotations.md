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

