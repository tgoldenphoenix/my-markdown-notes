# Config SQL Database

## PostgeSQL configurations

The password for the postgres user is the one you entered during the PostgreSQL installation.  
PostgesSQL password khác với pgadmin password. Nhưng có thể đặt trùng cũng được.

username: root
password: password (postgres password & pgadmin password)

**psql** is a terminal-based front-end to PostgreSQL. It enables you to type in queries interactively, issue them to PostgreSQL, and see the query results. Alternatively, input can be from a file or from command line arguments.

### command-line tool

`psql` là command-line tool của postgres

`\?` Coi bảng command in psql mode
cái nữa là `help` hiện ra bảng nhỏ hơn or `psql --help` (cái này dùng ở ngoài psql mode nhé)

`\l` show list of database

You can either use lowercase or uppercase. You are recommend to use UPPERCASE for SQL commands. That way, you can tell what is syntax and what is not. Remember to have semi-colon ở cuối command nhé.

`psql -h localhost -p 5432 -U <username> <DB name>` -> connect to a DB  
`-h` là host, nếu connecting to a remote server thì phải type ip ra.  
`-p` là port, default là 5432 như hình 1  
`-U` là username

The other way to connect to a psql DB
`psql` > `\l` > `\c <DB name>`
\c for “connect”. You can switch between DBs using the same command. Ko cần phải exit ra rồi vô cái mới.

## MySQL commands

`C:\Program Files\MySQL\MySQL Server 8.0\bin`

Use powershell instead of git bash.

```bash
PS C:\Users\anhao> mysql -h 127.0.0.1 -P 3306 -u root -p
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 667
Server version: 8.0.46 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
PS C:\Users\anhao>
```

`-p` (lowercase) tells the MySQL client to **prompt you interactively for your password** (`Enter password:`) rather than connecting without a password.

---

some commands

```bash
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| etl_adrepo         |
| etl_harbest        |
| hb_test            |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
7 rows in set (0.01 sec)

mysql> source C:\Users\anhao\Downloads\mysqlsampledatabase.sql
```

## Adrepo

mysql: root:root (password) or 123 or password
