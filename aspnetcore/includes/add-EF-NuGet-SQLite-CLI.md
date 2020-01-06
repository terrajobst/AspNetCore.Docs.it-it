Eseguire i seguenti comandi dell'interfaccia della riga di comando di .NET Core:

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

I comandi precedenti aggiungono:

* Lo [strumento di ponteggi ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).
* Strumenti di Entity Framework Core per il interfaccia della riga di comando di .NET Core.
* Il provider EF Core SQLite, che installa il pacchetto di EF Core come dipendenza.
* I pacchetti necessari per lo scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` e `Microsoft.EntityFrameworkCore.SqlServer`.

Per informazioni aggiuntive sulla configurazione di più ambienti che consente a un'app di configurare i contesti di database per ambiente, vedere <xref:fundamentals/environments#environment-based-startup-class-and-methods>.

[!INCLUDE[](~/includes/scaffoldTFM.md)]