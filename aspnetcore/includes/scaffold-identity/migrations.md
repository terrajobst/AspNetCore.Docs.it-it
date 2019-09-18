Il codice del database di identità generato richiede [Entity Framework Core migrazioni](/ef/core/managing-schemas/migrations/). Creare una migrazione e aggiornare il database. Ad esempio, eseguire i comandi seguenti:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

In Visual Studio **Console di gestione pacchetti**:

```powershell
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

Il parametro del nome "CreateIdentitySchema" per `Add-Migration` il comando è arbitrario. `"CreateIdentitySchema"`viene descritta la migrazione.
