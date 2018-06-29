---
title: Utilizzare una cache distribuita in ASP.NET Core
author: ardalis
description: Imparare a usare ASP.NET Core distribuita la memorizzazione nella cache per migliorare le prestazioni dell'applicazione e la scalabilità, in particolare in un ambiente di farm di server o cloud.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 5ddc3a6927652f773ab38f93db1e222c5a1900b3
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077699"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Utilizzare una cache distribuita in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Le cache distribuite possono migliorare le prestazioni e scalabilità delle app di ASP.NET Core, in particolare quando sono ospitati in un ambiente di farm di server o cloud. Questo articolo viene illustrato come utilizzare le implementazioni e astrazioni di cache distribuita incorporata del ASP.NET di base.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Che cos'è una cache distribuita

Una cache distribuita è condiviso da più server di app (vedere [nozioni di base Cache](memory.md#caching-basics)). Le informazioni nella cache non vengono memorizzate nella memoria del server web singole e i dati memorizzati nella cache sono disponibili a tutti i server dell'app. Ciò offre diversi vantaggi:

1. Dati memorizzati nella cache sono coerenti in tutti i server web. Gli utenti non visualizzati risultati diversi a seconda di quale web server gestisce la richiesta

2. Dati memorizzati nella cache viene conservata riavvio del server web e le distribuzioni. Server web singoli possono essere rimossi o aggiunti senza conseguenze per la cache.

3. L'archivio dati di origine ha meno le richieste effettuate a esso (che con più cache in memoria o su no cache affatto).

> [!NOTE]
> Se si usa una Cache distribuita di SQL Server, alcuni di questi vantaggi sono solo true se viene utilizzata per la cache per i dati di origine dell'app rispetto a un'istanza separata del database.

Come qualsiasi cache, una cache distribuita può migliorare notevolmente la velocità di risposta dell'app, poiché in genere i dati possono essere recuperati dalla cache molto più veloce da un database relazionale (o un servizio web).

Configurazione della cache è specifica dell'implementazione. In questo articolo viene descritto come configurare entrambi Redis e distribuite di SQL Server memorizza nella cache. Indipendentemente da quale implementazione è selezionata, l'applicazione interagisce con la cache utilizzando una comune `IDistributedCache` interfaccia.

## <a name="the-idistributedcache-interface"></a>L'interfaccia IDistributedCache

Il `IDistributedCache` interfaccia include metodi sincroni e asincroni. L'interfaccia consente gli elementi aggiunti, recuperati, e la rimozione rispetto all'implementazione di cache distribuita. Il `IDistributedCache` interfaccia include i metodi seguenti:

**Get, GetAsync**

Accetta una chiave di stringa e recupera un elemento memorizzato nella cache come una `byte[]` se trovato nella cache.

**Set, SetAsync**

Aggiunge un elemento (come `byte[]`) alla cache utilizzando una chiave di stringa.

**Aggiornamento, RefreshAsync**

Aggiorna un elemento nella cache in base alla chiave, reimpostare il timeout di scadenza scorrevole (se presente).

**Rimuovere, RemoveAsync**

Rimuove una voce della cache basata sulla relativa chiave.

Utilizzare il `IDistributedCache` interfaccia:

   1. Aggiungere i pacchetti NuGet necessari al file di progetto.

   2. Configurare l'implementazione specifica di `IDistributedCache` nella `Startup` della classe `ConfigureServices` (metodo) e aggiungerlo al contenitore di.

   3. Dell'app [Middleware](xref:fundamentals/middleware/index) o le classi controller MVC, è richiesta un'istanza di `IDistributedCache` dal costruttore. L'istanza verrà fornita da [inserimento di dipendenze](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Non è necessario utilizzare una durata Singleton o nell'ambito per `IDistributedCache` istanze (almeno per le implementazioni predefinite). È inoltre possibile creare un'istanza ogni volta che potrebbe essere necessario uno (invece di usare [inserimento di dipendenze](../../fundamentals/dependency-injection.md)), ma ciò può rendere più difficile da testare, il codice e viola il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).

Nell'esempio seguente viene illustrato come utilizzare un'istanza di `IDistributedCache` in un componente del middleware semplice:

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

Nel codice precedente, il valore memorizzato nella cache viene letto ma mai scritto. In questo esempio, il valore viene impostato solo quando un server di avvio e non cambia. In uno scenario con più server, il server più recente per l'avvio sovrascriverà i valori precedenti che sono stati impostati da altri server. Il `Get` e `Set` metodi utilizzano il `byte[]` tipo. Pertanto, il valore di stringa deve essere convertito utilizzando `Encoding.UTF8.GetString` (per `Get`) e `Encoding.UTF8.GetBytes` (per `Set`).

Nell'esempio di codice da *Startup.cs* Mostra il valore da impostare:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> Poiché `IDistributedCache` è configurato nel `ConfigureServices` metodo, è disponibile per il `Configure` metodo come parametro. Aggiunta come un parametro consentirà l'istanza configurata essere fornito mediante (DISCONNECTED Inbound).

## <a name="using-a-redis-distributed-cache"></a>Uso di una cache Redis distribuiti

[Redis](https://redis.io/) è un archivio Apri origine dati in memoria, che viene spesso usato come una cache distribuita. È possibile usare in locale ed è possibile configurare un [Cache Redis di Azure](https://azure.microsoft.com/services/cache/) per le app ASP.NET Core ospitato di Azure. L'app ASP.NET Core consente di configurare l'implementazione di cache con un `RedisDistributedCache` istanza.

Configurare l'implementazione di Redis in `ConfigureServices` e diritti di accesso nel codice dell'app richiedendo un'istanza di `IDistributedCache` (vedere il codice precedente).

Nell'esempio di codice, un `RedisCache` implementazione viene utilizzata quando il server è configurato per un `Staging` ambiente. In questo modo il `ConfigureStagingServices` metodo consente di configurare il `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> Per installare Redis sul computer locale, installare il pacchetto chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) ed eseguire `redis-server` da un prompt dei comandi.

## <a name="using-a-sql-server-distributed-cache"></a>Utilizzo di un Server SQL cache distribuita

L'implementazione SqlServerCache consente la cache distribuita utilizzare un database di SQL Server come archivio di backup. Per creare il Server SQL table è possibile utilizzare lo strumento della cache sql, lo strumento crea una tabella con il nome e lo schema specificato.

::: moniker range="< aspnetcore-2.1"

Aggiungere `SqlConfig.Tools` per il `<ItemGroup>` elemento del file di progetto e run `dotnet restore`.

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

SqlConfig.Tools Visualizza utilizzo, le opzioni e la Guida di comando.

Creare una tabella in SQL Server eseguendo il `sql-cache create` comando:

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

La tabella creata con lo schema seguente:

![Tabella della Cache di SQL Server](distributed/_static/SqlServerCacheTable.png)

Come tutte le implementazioni di cache, l'app deve ottenere e impostare i valori della cache usando un'istanza di `IDistributedCache`, non un `SqlServerCache`. Nell'esempio viene implementato `SqlServerCache` nell'ambiente di produzione (in modo che sia configurato `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> Il `ConnectionString` (ed eventualmente `SchemaName` e `TableName`) in genere devono essere archiviate di fuori di controllo del codice sorgente (ad esempio segreti utente), poiché potrebbero contenere le credenziali.

## <a name="recommendations"></a>Suggerimenti

Quando si decide quale implementazione di `IDistributedCache` è a destra per l'app, scegliere tra Redis e SQL Server in base l'infrastruttura esistente e ambiente, i requisiti di prestazioni ed esperienza del team. Se il team è più abituato a lavorare con Redis, è un'ottima scelta. Se il team si preferisce SQL Server, è possibile avere la certezza in che anche l'implementazione. Si noti che una tradizionale soluzione di memorizzazione nella cache vengono archiviati i dati in memoria che consente il recupero veloce dei dati. È consigliabile archiviare i dati utilizzati in una cache e archiviare tutti i dati in un archivio permanente back-end, ad esempio SQL Server o di archiviazione di Azure. Cache redis è una soluzione di memorizzazione nella cache che consente una velocità effettiva elevata e latenza bassa rispetto alle Cache SQL.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Redis Cache in Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Database SQL in Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [Cache in memoria](xref:performance/caching/memory)
* [Rilevare le modifiche apportate con i token di modifica](xref:fundamentals/primitives/change-tokens)
* [Memorizzazione nella cache delle risposte](xref:performance/caching/response)
* [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
* [Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
