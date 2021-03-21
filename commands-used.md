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
