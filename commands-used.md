# ⌨️ Comandos usados


## Módulo: Objetos e tipos de dados do PostgreSQL

### Aula: Conheça a ferramenta PGAdmin

- Acessar pgAdmin4 via browser
   - Normalmente é aberto automaticamente, quando pgAdmin4 é iniciado
- Na barra de ferramentas, clicar em `Object`
- Selecionar a opção `Create` e após `Server Group...`

![Create server group on pgAdmin4](./.github/pgAdmin-create-server-group-option.png)


- Definir nome do `Server Group`

![Define server group name in pgAdmin4](./.github/pgAdmin-create-server-group-name.png)


- Na barra de ferramentas, novamente acessar a opção `Object`
- Clicar em `Create Server`

![Create server on pgAdmin4](./.github/pgAdmin-create-server-option.png)


- Definir nome do servidor, o grupo de servidor e comentários (opcional)

![Set server name](./.github/pgAdmin-set-server-name.png)


- Configurar URL e porta do servidor e definir usuário e senha

![Initial server configurations](./.github/pgAdmin-initial-server-configurations.png)


- Para abrir Query Tool, selecione o database `postgres`, dentro do servidor `aula`
- Na barra de ferramentas, acesse a opção `Tools`, em seguida `Query Tool`

![Open Query Tool](./.github/pgAdmin-open-query-tool.png)


- No `Query Editor` aberto, digitar o comando:

```sql
CREATE DATABASE financas;

```

- Executar o comando pressionando `F5` (atalho de teclado)


### Aula: Objetos e comandos do banco de dados

- Criar as tabelas `banco`, `agencia`, `cliente`, `conta_corrente`, `tipo_transacao` e `cliente_transacoes`

```sql
CREATE TABLE IF NOT EXISTS banco (
	numero INTEGER NOT NULL,
	nome VARCHAR(50) NOT NULL,
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (numero)
);

CREATE TABLE IF NOT EXISTS agencia (
	banco_numero INTEGER NOT NULL,
	numero INTEGER NOT NULL,
	nome VARCHAR(80) NOT NULL,
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (banco_numero, numero),
	FOREIGN KEY (banco_numero) REFERENCES banco (numero)
);

CREATE TABLE IF NOT EXISTS cliente (
	numero BIGSERIAL PRIMARY KEY,
	nome VARCHAR(120) NOT NULL,
	email VARCHAR(250) NOT NULL,
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS conta_corrente (
	banco_numero INTEGER NOT NULL,
	agencia_numero INTEGER NOT NULL,
	numero BIGINT NOT NULL,
	digito SMALLINT NOT NULL,
	cliente_numero BIGINT NOT NULL,
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (banco_numero, agencia_numero, numero, digito, cliente_numero),
	FOREIGN KEY (banco_numero, agencia_numero) REFERENCES agencia (banco_numero, numero),
	FOREIGN KEY (cliente_numero) REFERENCES cliente (numero)
);

CREATE TABLE IF NOT EXISTS tipo_transacao (
	id SMALLSERIAL PRIMARY KEY,
	nome VARCHAR(50) NOT NULL,
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS cliente_transacoes (
	id BIGSERIAL PRIMARY KEY,
	banco_numero INTEGER NOT NULL,
	agencia_numero INTEGER NOT NULL,
	conta_corrente_numero BIGINT NOT NULL,
	conta_corrente_digito SMALLINT NOT NULL,
	cliente_numero BIGINT NOT NULL,
	tipo_transacao_id SMALLINT NOT NULL,
	valor NUMERIC(15, 2) NOT NULL,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	FOREIGN KEY (banco_numero, agencia_numero, conta_corrente_numero, conta_corrente_digito, cliente_numero) REFERENCES conta_corrente (banco_numero, agencia_numero, numero, digito, cliente_numero)
);
```


## Módulo: Fundamentos da Structured Query Language (SQL)

### Aula: Conheça o DML e o Truncate

- Acessar o repositório [@drobcosta/digital_innovation_one](https://github.com/drobcosta/digital_innovation_one)
- Baixar o arquivo `dml.sql`
   - O arquivo contém dados para inserção nas tabelas criadas na aula anterior
- No pgAdmin4, em Query Tool, abrir o arquivo `dml.sql`
- Executar todo os comandos pressionando `F5`
- Algumas consultas para analisar os dados inseridos

```sql
SELECT numero, nome, ativo FROM banco;
SELECT banco_numero, numero, nome FROM agencia;
SELECT numero, nome, email FROM cliente;
SELECT id, nome FROM tipo_transacao;
SELECT banco_numero, agencia_numero, numero, cliente_numero FROM conta_corrente;
SELECT banco_numero, agencia_numero, cliente_numero FROM cliente_transacoes;

SELECT * FROM cliente_transacoes;
```

- Testando alguns comandos:

```sql
CREATE TABLE IF NOT EXISTS teste (
	id SERIAL PRIMARY KEY,
	nome VARCHAR(50) NOT NULL,
	created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

DROP TABLE IF EXISTS teste;

CREATE TABLE IF NOT EXISTS teste (
	cpf VARCHAR(11) NOT NULL,
	nome VARCHAR(50) NOT NULL,
	created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (cpf)
);

-- Após a execução do comando abaixo, tentar executar novamente e analisar comportamento
INSERT INTO teste (cpf, nome, created_at) 
VALUES ('22344566712', 'José Colméia', '2019-07-01 12:00:00');

INSERT INTO teste (cpf, nome, created_at) 
VALUES ('22344566712', 'José Colméia', '2019-07-01 12:00:00')
ON CONFLICT (cpf) DO NOTHING;

UPDATE teste SET nome = 'Pedro Alvares' WHERE cpf = '22344566712';

SELECT * FROM teste;
```


### Aula: Funções agregadas em PostgreSQL

- Exemplos de uso de funções agregadas

```sql
SELECT numero, nome FROM banco;
SELECT banco_numero, numero, nome FROM agencia;
SELECT numero, nome, email FROM cliente;
SELECT banco_numero, agencia_numero, cliente_numero FROM cliente_transacoes;

SELECT * FROM conta_corrente;

SELECT * FROM information_schema.columns;
-- Exibe todas as colunas de todas as tabelas

SELECT * FROM information_schema.columns WHERE table_name = 'banco';

SELECT column_name, data_type FROM information_schema.columns WHERE table_name = 'banco';


-- AVG
SELECT AVG(valor) FROM cliente_transacoes;


-- COUNT (HAVING)
SELECT COUNT(numero) FROM cliente;

SELECT COUNT(numero), email FROM cliente
WHERE email ILIKE '%gmail.com'
GROUP BY email;


-- MAX
SELECT MAX(numero) FROM cliente;

SELECT MAX(valor) FROM cliente_transacoes;

SELECT MAX(valor), tipo_transacao_id FROM cliente_transacoes
GROUP BY tipo_transacao_id;


-- MIN
SELECT MIN(numero) FROM cliente;

SELECT MIN(valor) FROM cliente_transacoes;

SELECT MIN(valor), tipo_transacao_id FROM cliente_transacoes
GROUP BY tipo_transacao_id;


-- SUM
SELECT COUNT(id), tipo_transacao_id FROM cliente_transacoes
GROUP BY tipo_transacao_id
HAVING COUNT(id) > 150;

SELECT SUM(valor) FROM cliente_transacoes;

SELECT SUM(valor), tipo_transacao_id FROM cliente_transacoes
GROUP BY tipo_transacao_id;

SELECT SUM(valor), tipo_transacao_id FROM cliente_transacoes
GROUP BY tipo_transacao_id
ORDER BY tipo_transacao_id;

SELECT SUM(valor), tipo_transacao_id FROM cliente_transacoes
GROUP BY tipo_transacao_id
ORDER BY tipo_transacao_id DESC;
```

