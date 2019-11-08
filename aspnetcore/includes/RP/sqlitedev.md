### <a name="use-sqlite-for-development-sql-server-for-production"></a>Usare SQLite per lo sviluppo, SQL Server per la produzione

Quando si seleziona SQLite, il codice generato dal modello è pronto per lo sviluppo. Il codice seguente illustra come inserire <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> all'avvio. `IWebHostEnvironment` viene inserito in modo che `ConfigureServices` possa usare SQLite per lo sviluppo e la SQL Server nell'ambiente di produzione.

[!code-csharp[](~/includes/RP/code/StartupDevProd.cs?name=snippet&highlight=5,10,14)]
