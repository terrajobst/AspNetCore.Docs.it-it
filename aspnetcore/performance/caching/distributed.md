---
title: Memorizzazione nella cache in ASP.NET Core distribuita
author: guardrex
description: Informazioni su come usare un'istanza di cache distribuita di ASP.NET Core per migliorare le prestazioni delle app e la scalabilità, soprattutto in un ambiente di farm di server o cloud.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: performance/caching/distributed
ms.openlocfilehash: 871e27c068de3a465cedb517418a35a87f992300
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085831"
---
# <a name="distributed-caching-in-aspnet-core"></a>Memorizzazione nella cache in ASP.NET Core distribuita

Dal [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com/)

Una cache distribuita è una cache condivisa da più server di app, in genere gestito come un servizio esterno per i server di app che vi accedono. Una cache distribuita può migliorare le prestazioni e scalabilità di un'app ASP.NET Core, soprattutto quando l'app è ospitata da un servizio cloud o una server farm.

Una cache distribuita presenta diversi vantaggi rispetto altri scenari di memorizzazione nella cache in cui sono archiviati dati memorizzati nella cache nei server delle singole app.

Quando i dati memorizzati nella cache sono distribuiti, i dati:

* Viene *coerente* (coerente) in tutte le richieste a più server.
* Può essere ripristinato dopo il riavvio del server e le distribuzioni di app.
* Non utilizza memoria locale.

Configurazione di cache distribuita è specifico dell'implementazione. Questo articolo descrive come configurare SQL Server e le cache distribuite Redis. Le implementazioni di terze parti sono anche disponibili, ad esempio [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache su GitHub](https://github.com/Alachisoft/NCache)). Indipendentemente dalla scelta dell'implementazione è selezionata, l'app interagisce con la cache usando il <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfaccia.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

::: moniker range=">= aspnetcore-2.2"

Per usare un Server SQL distribuito cache, riferimento il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o aggiungere un riferimento al pacchetto le [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) pacchetto.

Per usare un Redis cache, riferimento distribuita il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) e aggiungere un riferimento al pacchetto le [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) pacchetto. Il pacchetto di Redis non è incluso nel `Microsoft.AspNetCore.App` creare un pacchetto, pertanto è necessario fare riferimento a pacchetto Redis separatamente nel file di progetto.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Per usare un Server SQL distribuito cache, riferimento il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o aggiungere un riferimento al pacchetto le [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) pacchetto.

Per usare un Redis cache, riferimento distribuita il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) e aggiungere un riferimento al pacchetto le [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) pacchetto. Il pacchetto di Redis non è incluso nel `Microsoft.AspNetCore.App` creare un pacchetto, pertanto è necessario fare riferimento a pacchetto Redis separatamente nel file di progetto.

::: moniker-end

## <a name="idistributedcache-interface"></a>Interfaccia IDistributedCache

Il <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfaccia fornisce i metodi seguenti per modificare gli elementi nell'implementazione di cache distribuita:

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accetta una chiave di stringa e recupera un elemento memorizzato nella cache come un `byte[]` matrice se trovato nella cache.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Aggiunge un elemento (come `byte[]` matrice) alla cache usando una chiave di stringa.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Viene aggiornato un elemento nella cache in base alla chiave, reimpostare il timeout di scadenza scorrevole (se presente).
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Rimuove un elemento della cache basato sulla relativa chiave di stringa.

## <a name="establish-distributed-caching-services"></a>Stabilire i servizi di memorizzazione nella cache distribuiti

Registrare un'implementazione di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`. Implementazioni forniti dal Framework, descritte in questo argomento includono:

* [Cache distribuita in memoria](#distributed-memory-cache)
* [Cache distribuita di SQL Server](#distributed-sql-server-cache)
* [Cache distribuita di Redis](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a>Cache distribuita in memoria

La Cache di memoria distribuita (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) è un'implementazione fornita dal framework di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> che archivia gli elementi in memoria. La Cache di memoria distribuita non è un'istanza di cache distribuita effettivo. Gli elementi memorizzati nella cache vengono archiviati per l'istanza dell'app nel server in cui viene eseguita l'app.

La Cache distribuita di memoria è un'implementazione utile:

* Negli scenari di test e sviluppo.
* Quando si usa un singolo server in termini di consumo di memoria e di produzione non è un problema. Implementazione della Cache in memoria distribuita riassunti memorizzati nella cache di archiviazione dei dati. Consente di implementare che un vero e proprio distribuite soluzioni di memorizzazione nella cache nel futuro se più nodi o la tolleranza di errore diventano necessari.

L'app di esempio vengono utilizzate le Cache in memoria distribuita quando l'app viene eseguita nell'ambiente di sviluppo in `Startup.ConfigureServices`:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a>Cache del Server SQL distribuito

L'implementazione di Cache distribuita di Server SQL (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) consente alla cache distribuita di utilizzare un database di SQL Server come archivio di backup. Per creare una tabella di elemento memorizzato nella cache di SQL Server in un'istanza di SQL Server, è possibile usare il `sql-cache` dello strumento. Lo strumento crea una tabella con il nome e allo schema specificato.

Creare una tabella in SQL Server eseguendo il `sql-cache create` comando. Specificare l'istanza di SQL Server (`Data Source`), database (`Initial Catalog`), lo schema (ad esempio, `dbo`) e il nome di tabella (ad esempio, `TestCache`):

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

Un messaggio viene registrato per indicare che lo strumento ha esito negativo:

```console
Table and index were created successfully.
```

La tabella creata dal `sql-cache` strumento presenta lo schema seguente:

![Tabella della Cache di SQL Server](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> Un'app deve modificare i valori della cache usando un'istanza di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, non un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.

L'app di esempio implementa <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in un ambiente non di sviluppo in `Startup.ConfigureServices`:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> Oggetto <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (e, facoltativamente, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> e <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) vengono in genere archiviate all'esterno di controllo del codice sorgente (ad esempio, archiviati per il [Secret Manager](xref:security/app-secrets) o nella *appSettings. JSON* / *appsettings. {ENVIRONMENT}. JSON* file). La stringa di connessione può contenere le credenziali che devono essere mantenute all'esterno di sistemi di controllo di origine.

### <a name="distributed-redis-cache"></a>Cache distribuite Redis

[Redis](https://redis.io/) è un archivio dati in memoria open source, che viene spesso usato come una cache distribuita. È possibile usare Redis in locale, ed è possibile configurare un [Cache Redis di Azure](https://azure.microsoft.com/services/cache/) per un'app ASP.NET Core ospitate in Azure.

::: moniker range=">= aspnetcore-2.2"

Consente di configurare un'app per l'implementazione della cache mediante un <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> istanza (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in un ambiente non di sviluppo in `Startup.ConfigureServices`:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Consente di configurare un'app per l'implementazione della cache mediante un <xref:Microsoft.Extensions.Caching.Redis.RedisCache> istanza (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

Per installare Redis in locale:

* Installare il [pacchetto Chocolatey Redis](https://chocolatey.org/packages/redis-64/).
* Eseguire `redis-server` da un prompt dei comandi.

## <a name="use-the-distributed-cache"></a>Usare la cache distribuita

Usare la <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> l'interfaccia, richiedere un'istanza di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> da qualsiasi altro costruttore nell'app. L'istanza viene fornita dalla [inserimento delle dipendenze (dipendenze)](xref:fundamentals/dependency-injection).

All'avvio dell'app, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> viene inserito nelle `Startup.Configure`. L'ora corrente viene memorizzato nella cache usando <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (per altre informazioni, vedere [Host Web: Interfaccia IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

L'app di esempio inserisce <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> nella `IndexModel` da utilizzare per la pagina di indice.

Ogni volta che viene caricata la pagina di indice, la cache è selezionata per la volta memorizzati nella cache in `OnGetAsync`. Se l'ora memorizzati nella cache non è ancora scaduto, viene visualizzata l'ora. Se sono trascorsi 20 secondi dall'ultima volta l'ora memorizzati nella cache è stato eseguito (l'ultima è stata caricata in questa pagina), la pagina viene visualizzata *tempo scaduto memorizzato nella cache*.

Aggiornare immediatamente il tempo memorizzato nella cache all'ora corrente selezionando il **reimpostare memorizzato nella cache ora** pulsante. Le attivazioni di pulsanti di `OnPostResetCachedTime` metodo del gestore.

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> Non è necessario usare una durata Singleton o con ambito per <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> istanze (almeno per le implementazioni predefinite).
>
> È anche possibile creare un <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ogni volta che si potrebbe essere necessario uno invece di usare l'inserimento delle dipendenze, ma la creazione di un'istanza nel codice possono rendere più difficile da testare il codice dell'istanza e viola il [principio delle dipendenze esplicite](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

## <a name="recommendations"></a>Suggerimenti

Quando si decide quale implementazione di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> è ideale per l'app, tenere presente quanto segue:

* Infrastruttura esistente
* Requisiti relativi alle prestazioni
* Costi
* Esperienza del team

Soluzioni di memorizzazione nella cache di solito si basano sull'archiviazione in memoria per fornire il recupero veloce dei dati memorizzati nella cache, ma la memoria è una risorsa limitata e costosi da espandere. Unico archivio dati comunemente utilizzati in una cache.

In generale, una cache Redis offre una maggiore velocità effettiva e latenza più bassa rispetto a una cache di SQL Server. Tuttavia, il benchmarking è in genere obbligatorie per determinare le caratteristiche delle prestazioni delle strategie di memorizzazione nella cache.

Quando SQL Server viene usato come archivio di backup di cache distribuita, utilizzare dello stesso database per la cache e l'archiviazione di dati normale dell'app e il recupero può influire negativamente sulle prestazioni di entrambi. È consigliabile usare un'istanza di SQL Server dedicata per la cache distribuita di archivio di backup.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Redis Cache in Azure](/azure/azure-cache-for-redis/)
* [Database SQL in Azure](/azure/sql-database/)
* [ASP.NET Core Provider IDistributedCache per NCache nella Web farm](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache su GitHub](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
