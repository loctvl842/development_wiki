# SQL Server

## Install SQL Server

### Using Docker

```bash
docker run \
-e "ACCEPT_EULA=Y" \
-e "MSSQL_SA_PASSWORD=<MSSQL_SA_PASSWORD>" \
-p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2022-latest
```

**NOTE:** The `password` must be at least 8 characters long and contain characters from three of the following four sets: Uppercase letters, Lowercase letters, Base 10 digits, and Symbols..
