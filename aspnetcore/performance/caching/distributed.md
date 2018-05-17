---
title: Utilizzare una cache distribuita in ASP.NET Core
author: ardalis
description: Imparare a usare ASP.NET Core distribuita la memorizzazione nella cache per migliorare le prestazioni dell'applicazione e la scalabilità, in particolare in un ambiente di farm di server o cloud.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: c40209e3b3f2b5bf28450bb2a88cbe40e9e23230
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Utilizzare una cache distribuita in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Le cache distribuite possono migliorare le prestazioni e scalabilità delle applicazioni ASP.NET di base, soprattutto quando è ospitato in un ambiente di farm di server o cloud. In questo articolo viene illustrato come utilizzare le implementazioni e astrazioni di cache distribuita incorporata del ASP.NET di base.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Che cos'è una cache distribuita

Una cache distribuita è condiviso da più server di app (vedere [nozioni di base Cache](memory.md#caching-basics)). Le informazioni nella cache non vengono memorizzate nella memoria del server web singole e i dati memorizzati nella cache sono disponibili a tutti i server dell'app. Ciò offre diversi vantaggi:

1. Dati memorizzati nella cache sono coerenti in tutti i server web. Gli utenti non visualizzati risultati diversi a seconda di quale web server gestisce la richiesta

2. Dati memorizzati nella cache viene conservata riavvio del server web e le distribuzioni. Server web singoli possono essere rimossi o aggiunti senza conseguenze per la cache.

3. L'archivio di dati di origine ha meno le richieste effettuate a esso (non più in memoria cache o no cache affatto).

> [!NOTE]
> Se si usa una Cache distribuita di SQL Server, solo alcuni di questi vantaggi sono true se un'istanza separata del database viene utilizzata per la cache di per i dati di origine dell'app.

Ad esempio una cache, una cache distribuita può migliorare notevolmente la velocità di risposta di un'app, poiché in genere i dati possono essere recuperati dalla cache più veloce da un database relazionale (o un servizio web).

Configurazione della cache è specifica dell'implementazione. In questo articolo viene descritto come configurare entrambi Redis e distribuite di SQL Server memorizza nella cache. Indipendentemente dall'implementazione è selezionata, l'applicazione interagisce con la cache utilizzando una comune `IDistributedCache` interfaccia.

## <a name="the-idistributedcache-interface"></a>L'interfaccia IDistributedCache

Il `IDistributedCache` interfaccia include metodi sincroni e asincroni. L'interfaccia consente gli elementi da aggiungere, recuperare e rimossi dall'implementazione della cache distribuita. Il `IDistributedCache` interfaccia include i metodi seguenti:

**Get, GetAsync**

Accetta una chiave di stringa e recupera un elemento memorizzato nella cache come una `byte[]` se trovato nella cache.

**Set, SetAsync**

Aggiunge un elemento (come `byte[]`) alla cache utilizzando una chiave di stringa.

**Aggiornamento, RefreshAsync**

Aggiorna un elemento nella cache in base alla relativa chiave, reimpostare il timeout di scadenza scorrevole (se presente).

**Rimuovere, RemoveAsync**

Rimuove una voce della cache in base alla relativa chiave.

Utilizzare il `IDistributedCache` interfaccia:

   1. Aggiungere i pacchetti NuGet necessari al file di progetto.

   2. Configurare l'implementazione specifica di `IDistributedCache` nel `Startup` della classe `ConfigureServices` (metodo) e aggiungerlo al contenitore di.

   3. L'app [Middleware](xref:fundamentals/middleware/index) o classi controller MVC, è richiesta un'istanza di `IDistributedCache` dal costruttore. L'istanza verrà fornito da [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Non è necessario utilizzare una durata nell'ambito o Singleton per `IDistributedCache` istanze (almeno per le implementazioni predefinite). È inoltre possibile creare un'istanza ogni volta che potrebbe essere necessario uno (anziché [Dependency Injection](../../fundamentals/dependency-injection.md)), ma ciò può rendere più difficile da testare, il codice e viola il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).

Nell'esempio seguente viene illustrato come utilizzare un'istanza di `IDistributedCache` in un componente del middleware semplice:

[!code-csharp[](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

Nel codice precedente, il valore memorizzato nella cache viene letto ma non è stato scritto. In questo esempio, il valore viene impostato solo quando un server viene avviato e non cambia. In uno scenario con più server, il server più recente per l'avvio sovrascriverà i valori precedenti che sono stati impostati da altri server. Il `Get` e `Set` metodi utilizzano il `byte[]` tipo. Pertanto, il valore di stringa deve essere convertito utilizzando `Encoding.UTF8.GetString` (per `Get`) e `Encoding.UTF8.GetBytes` (per `Set`).

Nell'esempio di codice da *Startup.cs* Mostra il valore da impostare:

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> Poiché `IDistributedCache` è configurato nel `ConfigureServices` (metodo), è disponibile per il `Configure` metodo come parametro. Aggiungerlo come parametro consentirà l'istanza configurata essere fornito mediante DI.

## <a name="using-a-redis-distributed-cache"></a>Uso di una cache Redis distribuita

[Redis](https://redis.io/) è un archivio Apri origine dati in memoria, che viene spesso utilizzato come una cache distribuita. È possibile usare in locale e, è possibile configurare un [Cache Redis di Azure](https://azure.microsoft.com/services/cache/) per le app ASP.NET Core ospitato in Azure. L'app ASP.NET Core consente di configurare l'implementazione di cache con un `RedisDistributedCache` istanza.

Configurare l'implementazione di Redis in `ConfigureServices` e diritti di accesso nel codice dell'app richiedendo un'istanza di `IDistributedCache` (vedere il codice precedente).

Nell'esempio di codice, un `RedisCache` implementazione viene utilizzata quando il server è configurato per un `Staging` ambiente. In questo modo il `ConfigureStagingServices` metodo consente di configurare il `RedisCache`:

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> Per installare Redis sul computer locale, installare il pacchetto chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) ed eseguire `redis-server` da un prompt dei comandi.

## <a name="using-a-sql-server-distributed-cache"></a>Utilizzo di un Server SQL cache distribuita

L'implementazione di SqlServerCache consente la cache distribuita utilizzare un database di SQL Server come archivio di backup. Per creare il Server SQL table è possibile utilizzare lo strumento della cache sql, lo strumento crea una tabella con il nome e lo schema specificato.

Per utilizzare lo strumento della cache sql, aggiungere `SqlConfig.Tools` per il `<ItemGroup>` elemento del *csproj* file ed eseguire il ripristino dotnet.

[!code-xml[](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

Test SqlConfig.Tools eseguendo il comando seguente

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

strumento della cache SQL visualizzano la Guida di utilizzo e le opzioni di comando, ora è possibile creare tabelle in sql server, eseguire "Creazione di cache di sql" comando:

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

La tabella creata con lo schema seguente:

![Tabella della Cache di SQL Server](distributed/_static/SqlServerCacheTable.png)

Come tutte le implementazioni di cache, l'app deve ottenere e impostare i valori della cache utilizzando un'istanza di `IDistributedCache`, non un `SqlServerCache`. L'esempio implementa `SqlServerCache` nel `Production` ambiente (in modo che è configurato `ConfigureProductionServices`).

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> Il `ConnectionString` (e, facoltativamente, `SchemaName` e `TableName`) in genere devono essere archiviate di fuori di controllo del codice sorgente (ad esempio segreti utente), poiché potrebbero contenere le credenziali.

## <a name="recommendations"></a>Suggerimenti

Quando si decide quale implementazione di `IDistributedCache` diritto per l'app, scegliere tra Redis e SQL Server in base all'infrastruttura esistente e ambiente, i requisiti di prestazioni ed esperienza del team. Se il team è più in grado di utilizzare con Redis, è un'ottima scelta. Se SQL Server preferita dal team, è possibile la certezza che anche l'implementazione. Si noti che una tradizionale soluzione di memorizzazione nella cache vengono archiviati i dati in memoria che consente il recupero veloce dei dati. È necessario archiviare i dati utilizzati in una cache e archiviare tutti i dati in un archivio permanente di back-end, ad esempio SQL Server o di archiviazione di Azure. Cache redis è una soluzione di memorizzazione nella cache che consente una velocità effettiva elevata e latenza bassa rispetto alle Cache SQL.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Redis Cache in Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Database SQL in Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [Cache in memoria](xref:performance/caching/memory)
* [Rilevare le modifiche apportate con i token di modifica](xref:fundamentals/primitives/change-tokens)
* [Memorizzazione nella cache delle risposte](xref:performance/caching/response)
* [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
* [Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
