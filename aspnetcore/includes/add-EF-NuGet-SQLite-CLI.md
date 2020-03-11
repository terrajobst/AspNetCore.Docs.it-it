<span data-ttu-id="3eb08-101">Eseguire i seguenti comandi dell'interfaccia della riga di comando di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="3eb08-101">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="3eb08-102">I comandi precedenti aggiungono:</span><span class="sxs-lookup"><span data-stu-id="3eb08-102">The preceding commands add:</span></span>

* <span data-ttu-id="3eb08-103">Lo [strumento di ponteggi ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="3eb08-103">The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
* <span data-ttu-id="3eb08-104">Strumenti di Entity Framework Core per il interfaccia della riga di comando di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3eb08-104">The Entity Framework Core Tools for the .NET Core CLI.</span></span>
* <span data-ttu-id="3eb08-105">Il provider EF Core SQLite, che installa il pacchetto di EF Core come dipendenza.</span><span class="sxs-lookup"><span data-stu-id="3eb08-105">The EF Core SQLite provider, which installs the EF Core package as a dependency.</span></span>
* <span data-ttu-id="3eb08-106">I pacchetti necessari per lo scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` e `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="3eb08-106">Packages needed for scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` and `Microsoft.EntityFrameworkCore.SqlServer`.</span></span>

<span data-ttu-id="3eb08-107">Per informazioni aggiuntive sulla configurazione di più ambienti che consente a un'app di configurare i contesti di database per ambiente, vedere <xref:fundamentals/environments#environment-based-startup-class-and-methods>.</span><span class="sxs-lookup"><span data-stu-id="3eb08-107">For guidance on multiple environment configuration that permits an app to configure its database contexts by environment, see <xref:fundamentals/environments#environment-based-startup-class-and-methods>.</span></span>

[!INCLUDE[](~/includes/scaffoldTFM.md)]