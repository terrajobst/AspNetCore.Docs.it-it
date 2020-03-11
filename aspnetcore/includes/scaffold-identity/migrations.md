<span data-ttu-id="0eb56-101">Il codice del database di identità generato richiede [Entity Framework Core migrazioni](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="0eb56-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="0eb56-102">Creare una migrazione e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="0eb56-102">Create a migration and update the database.</span></span> <span data-ttu-id="0eb56-103">Ad esempio, eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="0eb56-103">For example, run the following commands:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0eb56-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0eb56-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0eb56-105">Nella **console di gestione pacchetti**di Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0eb56-105">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="0eb56-106">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="0eb56-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="0eb56-107">Il parametro del nome "CreateIdentitySchema" per il comando `Add-Migration` è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="0eb56-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="0eb56-108">`"CreateIdentitySchema"` descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="0eb56-108">`"CreateIdentitySchema"` describes the migration.</span></span>
