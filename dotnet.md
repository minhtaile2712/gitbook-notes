# dotnet

Create a project

```powershell
dotnet new console -o MyApp -f net6.0
```

To work with database, install these dependencies:

```powershell
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

dotnet ef migrations add Name\_Of\_Migration

dotnet ef database update
