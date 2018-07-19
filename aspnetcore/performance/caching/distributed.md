---
title: Usare una cache distribuita in ASP.NET Core
author: ardalis
description: Informazioni su come usare ASP.NET Core distribuita la memorizzazione nella cache per migliorare le prestazioni delle app e la scalabilità, soprattutto in un ambiente di farm di server o cloud.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 9c41a6e008045231bd2e1c1f53a9161e11daafa9
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123840"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Usare una cache distribuita in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Le cache distribuite possono migliorare le prestazioni e scalabilità delle App ASP.NET Core, soprattutto quando ospitati nel cloud o una server farm.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Che cos'è una cache distribuita

Una cache distribuita è condiviso da più server di app (vedere [nozioni di base Cache](memory.md#caching-basics)). Le informazioni nella cache non viene archiviate nella memoria del server web singoli e i dati memorizzati nella cache sono disponibili per tutti i server dell'app. Ciò offre diversi vantaggi:

1. Dati memorizzati nella cache sono coerenti in tutti i server web. Gli utenti non vengono visualizzati risultati diversi a seconda che web server gestisce la richiesta

2. Dati memorizzati nella cache può essere ripristinato dopo il riavvio del server web e le distribuzioni. Server web singoli possono essere rimossi o aggiunti senza conseguenze per la cache.

3. Archivio dati di origine ha meno le richieste effettuate a esso (non più in memoria cache o no cache affatto).

> [!NOTE]
> Se si usa una Cache distribuita di SQL Server, alcuni di questi vantaggi sono solo true se un'istanza separata del database viene usata per la cache più i dati di origine dell'app.

Ad esempio una cache qualsiasi, una cache distribuita Puoi migliorare notevolmente la velocità di risposta dell'app, poiché in genere i dati possono essere recuperati dalla cache molto più veloce da un database relazionale (o un servizio web).

Configurazione della cache è specifico dell'implementazione. Questo articolo descrive come configurare entrambi Redis e distribuite di SQL Server memorizza nella cache. Indipendentemente dalla scelta dell'implementazione è selezionata, l'app interagisce con la cache usando un comune `IDistributedCache` interfaccia.

## <a name="the-idistributedcache-interface"></a>L'interfaccia IDistributedCache

Il `IDistributedCache` interfaccia include metodi sincroni e asincroni. L'interfaccia consente gli elementi di essere aggiunti, recuperati e rimossi dall'implementazione di cache distribuita. Il `IDistributedCache` interfaccia include i metodi seguenti:

**Get, GetAsync**

Accetta una chiave di stringa e recupera un elemento memorizzato nella cache come un `byte[]` se trovato nella cache.

**Set, SetAsync**

Aggiunge un elemento (come `byte[]`) alla cache usando una chiave di stringa.

**Aggiornamento, RefreshAsync**

Aggiorna un elemento nella cache in base alla chiave, reimpostare il timeout di scadenza scorrevole (se presente).

**Rimuovere, RemoveAsync**

Rimuove una voce della cache basata sulla relativa chiave.

Usare il `IDistributedCache` interfaccia:

   1. Aggiungere i pacchetti NuGet al file di progetto.

   2. Configurare l'implementazione specifica della `IDistributedCache` nella `Startup` della classe `ConfigureServices` (metodo) e aggiungerlo al contenitore non esiste.

   3. L'app [Middleware](xref:fundamentals/middleware/index) o le classi controller MVC, richiedere un'istanza di `IDistributedCache` dal costruttore. L'istanza verrà fornita da [inserimento delle dipendenze](../../fundamentals/dependency-injection.md) (inserimento delle dipendenze).

> [!NOTE]
> Non è necessario usare una durata Singleton o con ambito per `IDistributedCache` istanze (almeno per le implementazioni predefinite). È anche possibile creare un'istanza ogni volta che si potrebbe essere necessario uno (invece di usare [inserimento delle dipendenze](../../fundamentals/dependency-injection.md)), ma ciò può rendere più difficile da testare, il codice e viola il [principio delle dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).

Nell'esempio seguente viene illustrato come utilizzare un'istanza di `IDistributedCache` in un componente del middleware semplice:

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

Nel codice precedente, il valore memorizzato nella cache è di lettura, ma mai scritto. In questo esempio, il valore viene impostato solo quando si avvia un server e non cambia. In uno scenario con più server, il server più recente per iniziare sovrascriveranno eventuali valori precedenti che sono stati impostati da altri server. Il `Get` e `Set` metodi usano il `byte[]` tipo. Pertanto, il valore della stringa deve essere convertito utilizzando `Encoding.UTF8.GetString` (per `Get`) e `Encoding.UTF8.GetBytes` (per `Set`).

Nell'esempio di codice dal *Startup.cs* Mostra il valore da impostare:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

Poiché `IDistributedCache` configurato nel `ConfigureServices` metodo, è disponibile per il `Configure` metodo come parametro. Aggiungendolo come parametro consentirà l'istanza configurata essere fornito mediante l'inserimento delle dipendenze.

## <a name="using-a-redis-distributed-cache"></a>Uso di una cache distribuita di Redis

[Redis](https://redis.io/) è un archivio dati in memoria open source, che viene spesso usato come una cache distribuita. È possibile usarlo in locale ed è possibile configurare un [Cache Redis di Azure](https://azure.microsoft.com/services/cache/) per le app ASP.NET Core ospitate in Azure. L'app ASP.NET Core consente di configurare l'implementazione della cache mediante un `RedisDistributedCache` istanza.

La cache Redis richiede [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)

Si configura l'implementazione di Redis nel `ConfigureServices` e accedervi nel codice dell'app richiedendo un'istanza di `IDistributedCache` (vedere il codice riportato sopra).

Nell'esempio di codice, un `RedisCache` implementazione viene utilizzata quando il server è configurato per un `Staging` ambiente. In questo modo il `ConfigureStagingServices` metodo consente di configurare il `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

Per installare Redis in locale, installare il pacchetto chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) ed eseguire `redis-server` da un prompt dei comandi.

## <a name="using-a-sql-server-distributed-cache"></a>Uso di un SQL Server di cache distribuita

L'implementazione SqlServerCache consente alla cache distribuita di utilizzare un database di SQL Server come archivio di backup. Per creare SQL Server nella tabella è possibile usare lo strumento della cache sql, lo strumento crea una tabella con il nome e lo schema specificati.

::: moniker range="< aspnetcore-2.1"

Aggiungere `SqlConfig.Tools` per il `<ItemGroup>` elemento del file di progetto ed eseguire `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Testare SqlConfig.Tools eseguendo il comando seguente:

```console
dotnet sql-cache create --help
```

Consente di visualizzare SqlConfig.Tools utilizzo, opzioni e Guida dei comandi.

Creare una tabella in SQL Server eseguendo il `sql-cache create` comando:

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

La tabella creata con lo schema seguente:

![Tabella della Cache di SQL Server](distributed/_static/SqlServerCacheTable.png)

Ad esempio tutte le implementazioni della cache, l'app deve ottenere e impostare i valori della cache usando un'istanza di `IDistributedCache`, non un `SqlServerCache`. Nell'esempio viene implementato `SqlServerCache` nell'ambiente di produzione (in modo che sia configurata in `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> Il `ConnectionString` (ed eventualmente `SchemaName` e `TableName`) in genere devono essere archiviate di fuori di controllo del codice sorgente (ad esempio segreti utente), poiché potrebbero contenere credenziali.

## <a name="recommendations"></a>Suggerimenti

Quando si decide quale implementazione di `IDistributedCache` è adatto alla tua app, scegliere tra Redis e SQL Server basato su esperienza del team e ambiente, i requisiti di prestazioni e l'infrastruttura esistente. Se il team ha più abituato a lavorare con Redis, è un'ottima scelta. Se preferita dal team SQL Server, è possibile essere ritenuto affidabile che anche l'implementazione. Si noti che una tradizionale soluzione di memorizzazione nella cache vengono archiviati i dati in memoria che consente di recuperare rapidamente i dati. È necessario archiviare i dati di uso comune in una cache e archiviare tutti i dati in un archivio persistente di back-end, ad esempio archiviazione di Azure o SQL Server. Cache redis è una soluzione di memorizzazione nella cache che offre bassa latenza e velocità effettiva elevata rispetto alle Cache SQL.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Redis Cache in Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Database SQL in Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
