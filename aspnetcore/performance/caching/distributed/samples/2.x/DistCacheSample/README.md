# <a name="aspnet-core-distributed-cache-sample"></a>Esempio di Cache distribuita di ASP.NET Core

In questo esempio viene illustrato l'utilizzo di una cache distribuita. Questo esempio viene illustrato lo scenario descritto nel [usare una cache distribuita in ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/caching/distributed) argomento.

Nell'ambiente di produzione, l'app di esempio è configurata per usare una cache distribuita di SQL Server. Per riconfigurare l'app per usare una cache distribuita di Redis, modificare la direttiva del preprocessore in cima il *Startup.cs* file usare Redis (`#define Redis // SQLServer`). Per altre informazioni, vedere [direttive del preprocessore nel codice di esempio](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code).
