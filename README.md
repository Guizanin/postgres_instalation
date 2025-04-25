#### CREDITS BY [SEBASTIÃO FIDÊNCIO](https://github.com/sfidencio)

# 🐘 PostgreSQL Installation & Configuration on Ubuntu

## 📋 Summary

- [Install PostgreSQL on Ubuntu](#-install-postgresql-on-ubuntu--instale-o-postgresql-no-ubuntu)
- [Change postgres User Password](#-change-postgres-user-password--altere-a-senha-do-usuário-postgres)
- [Allow Remote Connections](#️-allow-remote-connections--permitir-conexões-remotas)
- [Connect Using DBeaver or External CLI](#-connect-using-dbeaver-or-external-cli--conecte-usando-dbeaver-ou-cli-externo)
- [Basic postgresql.conf Tuning](#️-basic-postgresqlconf-tuning--tunagem-básica-do-postgresqlconf)
- [Security Tips](#-security-tips--dicas-de-segurança)
- [Useful Commands](#-useful-commands--comandos-úteis)
- [Example Usage with psql](#-example-usage-with-psql--exemplo-de-uso-com-psql)
- [References](#-references--referências)

---

## 🚀 Install PostgreSQL on Ubuntu | Instale o PostgreSQL no Ubuntu

**ENG | PT-BR:**  
Install PostgreSQL and check the service status.  
Instale o PostgreSQL e verifique o status do serviço.

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl status postgresql
```

## 🔑 Change postgres User Password | Altere a senha do usuário postgres

**ENG | PT-BR:**  
Change the default password for the `postgres` user.  
Altere a senha padrão do usuário `postgres`.

```bash
sudo -i -u postgres
psql
\password postgres
# Enter new password | Digite a nova senha
\q
exit
```

## 🛠️ Allow Remote Connections | Permitir Conexões Remotas

**ENG | PT-BR:**  
Edit configuration files to allow remote access.  
Edite os arquivos de configuração para permitir acesso remoto.

1. **Edit `postgresql.conf`**  
   ```bash
   sudo nano /etc/postgresql/<version>/main/postgresql.conf
   ```
   - Set: `listen_addresses = '*'`  
     Defina: `listen_addresses = '*'`

2. **Edit `pg_hba.conf`**  
   ```bash
   sudo nano /etc/postgresql/<version>/main/pg_hba.conf
   ```
   - Add for remote access | Adicione para acesso remoto:
     ```
     host    all    all    0.0.0.0/0    md5
     ```
   - For local | Para local:
     ```
     local   all    all               md5
     ```

3. **Restart PostgreSQL | Reinicie o PostgreSQL**
   ```bash
   sudo systemctl restart postgresql
   ```

## 🧑‍💻 Connect Using DBeaver or External CLI | Conecte usando DBeaver ou CLI Externo

**ENG | PT-BR:**  
- **DBeaver:**  
  Download and install from [dbeaver.io](https://dbeaver.io/). Create a new PostgreSQL connection and fill in host, port, user, and password.  
  Baixe e instale em [dbeaver.io](https://dbeaver.io/). Crie uma nova conexão PostgreSQL e preencha host, porta, usuário e senha.
- **psql CLI (from another machine) | psql CLI (de outra máquina):**
  ```bash
  psql -h <server_ip> -U postgres -d <database>
  ```

## ⚙️ Basic postgresql.conf Tuning | Tunagem Básica do postgresql.conf

**ENG | PT-BR:**  
Adjust memory, connections, and logging settings according to your server.  
Ajuste configurações de memória, conexões e logs conforme seu servidor.

- **Memory | Memória:**
  ```
  shared_buffers = 512MB
  work_mem = 16MB
  maintenance_work_mem = 128MB
  ```
- **Connections | Conexões:**
  ```
  max_connections = 100
  ```
- **Logging | Log:**
  ```
  logging_collector = on
  log_directory = 'log'
  log_filename = 'postgresql-%a.log'
  log_statement = 'ddl'
  ```

> ⚠️ Always tune according to your server specs and workload!  
> ⚠️ Sempre ajuste conforme os recursos do seu servidor e carga de trabalho!

## 🔒 Security Tips | Dicas de Segurança

**ENG | PT-BR:**  
- Change all default passwords.  
  Altere todas as senhas padrão.
- Restrict `listen_addresses` and `pg_hba.conf` to trusted networks.  
  Restrinja `listen_addresses` e `pg_hba.conf` para redes confiáveis.
- Keep PostgreSQL updated.  
  Mantenha o PostgreSQL atualizado.

## 🧹 Useful Commands | Comandos Úteis

```bash
sudo systemctl start postgresql
sudo systemctl stop postgresql
sudo systemctl restart postgresql
sudo systemctl status postgresql
sudo -u postgres psql -c "\l"   # List databases | Listar bancos
sudo -u postgres psql -c "\du"  # List users | Listar usuários
```

---

## 📝 Example Usage with psql | Exemplo de Uso com psql

**ENG | PT-BR:**  
Below are some basic commands to create a database, table, insert data, and run queries using the `psql` CLI.  
Abaixo alguns comandos básicos para criar banco, tabela, inserir dados e consultar usando o `psql`.

### 1. Access psql

```bash
sudo -u postgres psql
```

### 2. Create a Database | Criar um Banco

```sql
CREATE DATABASE testdb;
\c testdb
```

### 3. Create a Table | Criar uma Tabela

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100)
);
```

### 4. Insert Data | Inserir Dados

```sql
INSERT INTO users (name, email) VALUES ('Alice', 'alice@email.com');
INSERT INTO users (name, email) VALUES ('Bob', 'bob@email.com');
```

### 5. Query Data | Consultar Dados

```sql
SELECT * FROM users;
```

### 6. List Tables | Listar Tabelas

```sql
\dt
```

### 7. Describe Table | Descrever Tabela

```sql
\d users
```

### 8. Exit psql | Sair do psql

```sql
\q
```

---

### 🖥️ Main psql CLI Commands | Principais Comandos do psql CLI

**ENG | PT-BR:**  
Some of the most useful psql commands for daily work.  
Alguns dos comandos mais úteis do psql para o dia a dia.

| Command         | Description (EN)                | Descrição (PT-BR)                   |
|-----------------|---------------------------------|-------------------------------------|
| `\l`            | List databases                  | Listar bancos de dados              |
| `\c dbname`     | Connect to database             | Conectar ao banco                   |
| `\dt`           | List tables                     | Listar tabelas                      |
| `\d tablename`  | Describe table structure        | Descrever estrutura da tabela       |
| `\du`           | List roles/users                | Listar usuários/roles               |
| `\df`           | List functions                  | Listar funções                      |
| `\dn`           | List schemas                    | Listar schemas                      |
| `\x`            | Toggle expanded display         | Alternar visualização expandida     |
| `\timing`       | Toggle query execution timing   | Alternar tempo de execução          |
| `\h`            | Help on SQL commands            | Ajuda sobre comandos SQL            |
| `\?`            | Help on psql commands           | Ajuda sobre comandos do psql        |
| `\q`            | Quit psql                       | Sair do psql                        |

---

## 📚 References | Referências

- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [DBeaver](https://dbeaver.io/)
- [PostgreSQL Tuning Guide](https://pgtune.leopard.in.ua/)

---

[⬅️ Back ](../README.md)
