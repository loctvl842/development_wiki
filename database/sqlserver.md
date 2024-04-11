# SQL Server

## Install SQL Server

### Using Docker

```bash
docker run \
--name sqlserver \
-e "ACCEPT_EULA=Y" \
-e "MSSQL_SA_PASSWORD=<MSSQL_SA_PASSWORD>" \
-p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2022-latest
```

**NOTE:** The `password` must be at least 8 characters long and contain characters from three of the following four sets: Uppercase letters, Lowercase letters, Base 10 digits, and Symbols..

## Commands

### Connect to SQL Server

```bash
/opt/mssql-tools/bin/sqlcmd -S <server_name> -U <username> -P <password> -d <database_name> -Q "<query>"
```

- `S`: Specifies the SQL Server instance or network address. Replace `<server_name>` with the name or IP address of the SQL Server.
- `U`: Specifies the username to use for authentication. Replace `<username>` with your SQL Server username. (Default is `sa`)
- `P`: Specifies the password to use for authentication. Replace `<password>` with your SQL Server password.
- `d`: Specifies the name of the database to connect to. Replace `<database_name>` with the name of your SQL Server database.
- `Q`: Specifies the SQL query to execute. Replace "`<query>`" with the SQL query you want to run. Ensure that the query is enclosed in double quotes.
  Here's an example command to connect to a SQL Server database, execute a simple SELECT query, and print the results:

### SQL Server Commands

#### Create a Database

```sql
CREATE DATABASE [database_name]
```

#### List all Databases

```sql
SELECT name FROM sys.databases
```

#### Use a Database

```sql
USE [database_name]
```
