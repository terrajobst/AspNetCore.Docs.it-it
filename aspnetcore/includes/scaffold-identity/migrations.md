<span data-ttu-id="9fef8-101">Il codice di database di identità generato richiede [Entity Framework Core migrazioni](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="9fef8-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="9fef8-102">Creare una migrazione e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="9fef8-102">Create a migration and update the database.</span></span> <span data-ttu-id="9fef8-103">Ad esempio, eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9fef8-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9fef8-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9fef8-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9fef8-105">In Visual Studio **Console di gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="9fef8-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9fef8-106">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="9fef8-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="9fef8-107">Il parametro di nome "CreateIdentitySchema" per il `Add-Migration` comando è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="9fef8-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="9fef8-108">`"CreateIdentitySchema"` descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="9fef8-108">`"CreateIdentitySchema"` describes the migration.</span></span>