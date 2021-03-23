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


### Aula: Trabalhando com JOINs

- Exemplos de uso de `INNER JOIN`, `LEFT OUTER JOIN` e `RIGHT OUTER JOIN`

```sql
SELECT numero, nome FROM banco;
SELECT banco_numero, numero, nome FROM agencia;
SELECT numero, nome FROM cliente;
SELECT banco_numero, agencia_numero, numero, digito, cliente_numero FROM conta_corrente;
SELECT id, nome FROM tipo_transacao;
SELECT banco_numero, agencia_numero, conta_corrente_numero, conta_corrente_digito, cliente_numero, valor FROM cliente_transacoes;

SELECT count(1) FROM banco; -- 151
SELECT count(1) FROM agencia; -- 296

-- 296 registros
SELECT banco.numero, banco.nome, agencia.numero, agencia.nome
FROM banco
JOIN agencia ON agencia.banco_numero = banco.numero;

SELECT COUNT(banco.numero)
FROM banco
JOIN agencia ON agencia.banco_numero = banco.numero;

SELECT banco.numero
FROM banco
JOIN agencia ON agencia.banco_numero = banco.numero
GROUP BY banco.numero;

SELECT COUNT(DISTINCT banco.numero)
FROM banco
JOIN agencia ON agencia.banco_numero = banco.numero;

-- LEFT JOIN
SELECT banco.numero, banco.nome, agencia.numero, agencia.nome
FROM banco
LEFT JOIN agencia ON agencia.banco_numero = banco.numero; -- 438

-- RIGHT JOIN
SELECT agencia.numero, agencia.nome, banco.numero, banco.nome
FROM agencia
RIGHT JOIN banco ON banco.numero = agencia.banco_numero; -- 438

-- FULL JOIN
SELECT banco.numero, banco.nome, agencia.numero, agencia.nome
FROM banco
FULL JOIN agencia ON agencia.banco_numero = banco.numero; -- 438
```


- Criar tabelas para exemplificar uso de `CROSS JOIN`

```sql
CREATE TABLE IF NOT EXISTS teste_a (
	id SERIAL PRIMARY KEY,
	valor VARCHAR(10)
);

CREATE TABLE IF NOT EXISTS teste_b (
	id SERIAL PRIMARY KEY,
	valor VARCHAR(10)
);

INSERT INTO teste_a (valor) VALUES ('teste1');
INSERT INTO teste_a (valor) VALUES ('teste2');
INSERT INTO teste_a (valor) VALUES ('teste3');
INSERT INTO teste_a (valor) VALUES ('teste4');

INSERT INTO teste_b (valor) VALUES ('teste_a');
INSERT INTO teste_b (valor) VALUES ('teste_b');
INSERT INTO teste_b (valor) VALUES ('teste_c');
INSERT INTO teste_b (valor) VALUES ('teste_d');

-- CROSS JOIN
SELECT tbla.valor, tblb.valor
FROM teste_a tbla
CROSS JOIN teste_b tblb;

DROP TABLE IF EXISTS teste_a;
DROP TABLE IF EXISTS teste_b;
```


- Exemplo de uso de vários `JOINs`

```sql
SELECT banco.nome,
	   agencia.nome,
	   conta_corrente.numero,
	   conta_corrente.digito,
	   cliente.nome
FROM banco
JOIN agencia ON agencia.banco_numero = banco.numero
JOIN conta_corrente
	-- ON conta_corrente.banco_numero = agencia.banco_numero
	ON conta_corrente.banco_numero = banco.numero
	AND conta_corrente.agencia_numero = agencia.numero
JOIN cliente
	ON cliente.numero = conta_corrente.cliente_numero;
```


### Aula: Otimizando o código com CTE

- Exemplos de uso de `WITH STATEMENT`

```sql
SELECT numero, nome FROM banco;
SELECT banco_numero, numero, nome FROM agencia;

-- With Statements
WITH tbl_tmp_banco AS (
	SELECT numero, nome
	FROM banco
)
SELECT numero, nome
FROM tbl_tmp_banco;


WITH params AS (
	SELECT 213 AS banco_numero
), tbl_tmp_banco AS (
	SELECT numero, nome
	FROM banco
	JOIN params ON params.banco_numero = banco.numero
)
SELECT numero, nome
FROM tbl_tmp_banco;


WITH clientes_e_transacoes AS (
	SELECT cliente.nome AS cliente_nome,
	       tipo_transacao.nome AS tipo_transacao_nome,
	       cliente_transacoes.valor AS tipo_transacao_valor
	FROM cliente_transacoes
	JOIN cliente ON cliente.numero = cliente_transacoes.cliente_numero
	JOIN tipo_transacao ON tipo_transacao.id = cliente_transacoes.tipo_transacao_id
)
SELECT cliente_nome, tipo_transacao_nome, tipo_transacao_valor
FROM clientes_e_transacoes;


WITH clientes_e_transacoes AS (
	SELECT cliente.nome AS cliente_nome,
	       tipo_transacao.nome AS tipo_transacao_nome,
	       cliente_transacoes.valor AS tipo_transacao_valor
	FROM cliente_transacoes
	JOIN cliente ON cliente.numero = cliente_transacoes.cliente_numero
	JOIN tipo_transacao ON tipo_transacao.id = cliente_transacoes.tipo_transacao_id
	JOIN banco ON banco.numero = cliente_transacoes.banco_numero AND banco.nome ILIKE '%Itaú%'
)
SELECT cliente_nome, tipo_transacao_nome, tipo_transacao_valor
FROM clientes_e_transacoes;


-- Subselect
SELECT banco.numero, banco.nome
FROM banco
JOIN (
	SELECT 213 AS banco_numero
) params ON params.banco_numero = banco.numero;
```


## Módulo: Comandos avançados da Structured Query Language (SQL)

### Aula: Como as views auxiliam no acesso ao banco de dados

- Exemplos de uso de `VIEWs`

```sql
SELECT numero, nome, ativo
FROM banco;


CREATE OR REPLACE VIEW vw_bancos AS (
	SELECT numero, nome, ativo
	FROM banco
);

SELECT numero, nome, ativo
FROM vw_bancos;

----------

CREATE OR REPLACE VIEW vw_bancos_2 (banco_numero, banco_nome, banco_ativo) AS (
	SELECT numero, nome, ativo
	FROM banco
);

SELECT banco_numero, banco_nome, banco_ativo
FROM vw_bancos_2;

INSERT INTO vw_bancos_2 (banco_numero, banco_nome, banco_ativo)
VALUES (51, 'Banco Boa Ideia', TRUE);

-- Consulta usando a VIEW vw_bancos_2
SELECT banco_numero, banco_nome, banco_ativo
FROM vw_bancos_2
WHERE banco_numero = 51;

-- Consulta diretamente na tabela banco
SELECT numero, nome, ativo FROM banco WHERE numero = 51;

UPDATE vw_bancos_2 SET banco_ativo = FALSE WHERE banco_numero = 51;

DELETE FROM vw_bancos_2 WHERE banco_numero = 51;
```


- Exemplo de uso da opção `TEMPORARY`
   - Ao sair e entrar novamente, ou ao abrir um novo `Query Editor`, a VIEW não existirá mais

```sql
CREATE OR REPLACE TEMPORARY VIEW vw_agencia AS (
	SELECT nome FROM agencia
)

SELECT nome FROM vw_agencia;
```


- `WITH OPTIONS`

```sql
CREATE OR REPLACE VIEW vw_bancos_ativos AS (
	SELECT numero, nome, ativo
	FROM banco
	WHERE ativo IS TRUE
) WITH LOCAL CHECK OPTION;

INSERT INTO vw_bancos_ativos (numero, nome, ativo) VALUES (51, 'Banco Boa Ideia', FALSE);
-- ERRO, pois a VIEW faz validação do valor do campo "ativo" aceitando somente TRUE



CREATE OR REPLACE VIEW vw_bancos_com_a AS (
	SELECT numero, nome, ativo
	FROM vw_bancos_ativos
	WHERE nome ILIKE 'a%'
) WITH LOCAL CHECK OPTION;

SELECT numero, nome, ativo FROM vw_bancos_com_a;

INSERT INTO vw_bancos_com_a (numero, nome, ativo) VALUES (333, 'Beta Omega', TRUE);
-- ERRO, pois na validação da coluna nome a VIEW vw_bancos_com_a só aceita valores que comecem com A ou a

INSERT INTO vw_bancos_com_a (numero, nome, ativo) VALUES (333, 'Alfa Omega', TRUE);
INSERT INTO vw_bancos_com_a (numero, nome, ativo) VALUES (331, 'Alfa Gama', FALSE);
```


- RECURSIVE

```sql
CREATE TABLE IF NOT EXISTS funcionarios (
	id SERIAL,
	nome VARCHAR(50),
	gerente INTEGER,
	PRIMARY KEY (id),
	FOREIGN KEY (gerente) REFERENCES funcionarios (id)
);

INSERT INTO funcionarios (nome, gerente) VALUES ('Ancelmo', null);
INSERT INTO funcionarios (nome, gerente) VALUES ('Beatriz', 1);
INSERT INTO funcionarios (nome, gerente) VALUES ('Magno', 1);
INSERT INTO funcionarios (nome, gerente) VALUES ('Cremilda', 2);
INSERT INTO funcionarios (nome, gerente) VALUES ('Wagner', 4);

SELECT id, nome, gerente FROM funcionarios WHERE gerente IS NULL
UNION ALL
SELECT id, nome, gerente FROM funcionarios WHERE id = 999;


CREATE OR REPLACE RECURSIVE VIEW vw_func(id, gerente, funcionario) AS (
	SELECT id, gerente, nome
	FROM funcionarios
	WHERE gerente IS NULL
	
	UNION ALL
	
	SELECT funcionarios.id, funcionarios.gerente, funcionarios.nome
	FROM funcionarios
	JOIN vw_func ON vw_func.id = funcionarios.gerente
);

SELECT id, gerente, funcionario
FROM vw_func;
```


### Aula: Conheça um dos principais conceitos de banco de dados: transações

- Exemplos de transações

```sql
SELECT numero, nome, ativo FROM banco
ORDER BY numero;

UPDATE banco SET ativo = false WHERE numero = 0;

BEGIN;
UPDATE banco SET ativo = true WHERE numero = 0;
SELECT numero, nome, ativo FROM banco WHERE numero = 0 ;
ROLLBACK;

BEGIN;
UPDATE banco SET ativo = true WHERE numero = 0;
COMMIT;



SELECT id, gerente, nome
FROM funcionarios;

BEGIN;
UPDATE funcionarios SET gerente = 2 WHERE id = 3;
SAVEPOINT sf_func;
UPDATE funcionarios SET gerente = null;
ROLLBACK TO sf_func;
UPDATE funcionarios SET gerente = 3 WHERE id = 5;
COMMIT;
```
