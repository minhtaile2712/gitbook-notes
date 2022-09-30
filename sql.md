# SQL

Run SQL Server 2019 in Docker:

```bash
docker run \
-e "ACCEPT_EULA=Y" \
-e "MSSQL_PID=Express" \
-e "MSSQL_SA_PASSWORD=P@ssw0rd" \
-dp 1433:1433 \
mcr.microsoft.com/mssql/server:2019-latest
```

Connection string:

```powershell
// .DbMigrator default
var Default = "Server=(LocalDb)\\MSSQLLocalDB;Database=BookStore;Trusted_Connection=True";
const string connectionString = @"Server=localhost,1433;Database=taidb;User Id=SA;Password=P@ssw0rd";
```
