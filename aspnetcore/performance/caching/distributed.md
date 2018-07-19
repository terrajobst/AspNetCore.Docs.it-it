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
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="fa99f-103">Usare una cache distribuita in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa99f-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="fa99f-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="fa99f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fa99f-105">Le cache distribuite possono migliorare le prestazioni e scalabilità delle App ASP.NET Core, soprattutto quando ospitati nel cloud o una server farm.</span><span class="sxs-lookup"><span data-stu-id="fa99f-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="fa99f-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fa99f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="fa99f-107">Che cos'è una cache distribuita</span><span class="sxs-lookup"><span data-stu-id="fa99f-107">What is a distributed cache</span></span>

<span data-ttu-id="fa99f-108">Una cache distribuita è condiviso da più server di app (vedere [nozioni di base Cache](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="fa99f-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="fa99f-109">Le informazioni nella cache non viene archiviate nella memoria del server web singoli e i dati memorizzati nella cache sono disponibili per tutti i server dell'app.</span><span class="sxs-lookup"><span data-stu-id="fa99f-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="fa99f-110">Ciò offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="fa99f-110">This provides several advantages:</span></span>

1. <span data-ttu-id="fa99f-111">Dati memorizzati nella cache sono coerenti in tutti i server web.</span><span class="sxs-lookup"><span data-stu-id="fa99f-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="fa99f-112">Gli utenti non vengono visualizzati risultati diversi a seconda che web server gestisce la richiesta</span><span class="sxs-lookup"><span data-stu-id="fa99f-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="fa99f-113">Dati memorizzati nella cache può essere ripristinato dopo il riavvio del server web e le distribuzioni.</span><span class="sxs-lookup"><span data-stu-id="fa99f-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="fa99f-114">Server web singoli possono essere rimossi o aggiunti senza conseguenze per la cache.</span><span class="sxs-lookup"><span data-stu-id="fa99f-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="fa99f-115">Archivio dati di origine ha meno le richieste effettuate a esso (non più in memoria cache o no cache affatto).</span><span class="sxs-lookup"><span data-stu-id="fa99f-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="fa99f-116">Se si usa una Cache distribuita di SQL Server, alcuni di questi vantaggi sono solo true se un'istanza separata del database viene usata per la cache più i dati di origine dell'app.</span><span class="sxs-lookup"><span data-stu-id="fa99f-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="fa99f-117">Ad esempio una cache qualsiasi, una cache distribuita Puoi migliorare notevolmente la velocità di risposta dell'app, poiché in genere i dati possono essere recuperati dalla cache molto più veloce da un database relazionale (o un servizio web).</span><span class="sxs-lookup"><span data-stu-id="fa99f-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="fa99f-118">Configurazione della cache è specifico dell'implementazione.</span><span class="sxs-lookup"><span data-stu-id="fa99f-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="fa99f-119">Questo articolo descrive come configurare entrambi Redis e distribuite di SQL Server memorizza nella cache.</span><span class="sxs-lookup"><span data-stu-id="fa99f-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="fa99f-120">Indipendentemente dalla scelta dell'implementazione è selezionata, l'app interagisce con la cache usando un comune `IDistributedCache` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="fa99f-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="fa99f-121">L'interfaccia IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="fa99f-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="fa99f-122">Il `IDistributedCache` interfaccia include metodi sincroni e asincroni.</span><span class="sxs-lookup"><span data-stu-id="fa99f-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="fa99f-123">L'interfaccia consente gli elementi di essere aggiunti, recuperati e rimossi dall'implementazione di cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="fa99f-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="fa99f-124">Il `IDistributedCache` interfaccia include i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fa99f-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="fa99f-125">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="fa99f-125">**Get, GetAsync**</span></span>

<span data-ttu-id="fa99f-126">Accetta una chiave di stringa e recupera un elemento memorizzato nella cache come un `byte[]` se trovato nella cache.</span><span class="sxs-lookup"><span data-stu-id="fa99f-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="fa99f-127">**Set, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="fa99f-127">**Set, SetAsync**</span></span>

<span data-ttu-id="fa99f-128">Aggiunge un elemento (come `byte[]`) alla cache usando una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="fa99f-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="fa99f-129">**Aggiornamento, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="fa99f-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="fa99f-130">Aggiorna un elemento nella cache in base alla chiave, reimpostare il timeout di scadenza scorrevole (se presente).</span><span class="sxs-lookup"><span data-stu-id="fa99f-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="fa99f-131">**Rimuovere, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="fa99f-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="fa99f-132">Rimuove una voce della cache basata sulla relativa chiave.</span><span class="sxs-lookup"><span data-stu-id="fa99f-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="fa99f-133">Usare il `IDistributedCache` interfaccia:</span><span class="sxs-lookup"><span data-stu-id="fa99f-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="fa99f-134">Aggiungere i pacchetti NuGet al file di progetto.</span><span class="sxs-lookup"><span data-stu-id="fa99f-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="fa99f-135">Configurare l'implementazione specifica della `IDistributedCache` nella `Startup` della classe `ConfigureServices` (metodo) e aggiungerlo al contenitore non esiste.</span><span class="sxs-lookup"><span data-stu-id="fa99f-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="fa99f-136">L'app [Middleware](xref:fundamentals/middleware/index) o le classi controller MVC, richiedere un'istanza di `IDistributedCache` dal costruttore.</span><span class="sxs-lookup"><span data-stu-id="fa99f-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="fa99f-137">L'istanza verrà fornita da [inserimento delle dipendenze](../../fundamentals/dependency-injection.md) (inserimento delle dipendenze).</span><span class="sxs-lookup"><span data-stu-id="fa99f-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="fa99f-138">Non è necessario usare una durata Singleton o con ambito per `IDistributedCache` istanze (almeno per le implementazioni predefinite).</span><span class="sxs-lookup"><span data-stu-id="fa99f-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="fa99f-139">È anche possibile creare un'istanza ogni volta che si potrebbe essere necessario uno (invece di usare [inserimento delle dipendenze](../../fundamentals/dependency-injection.md)), ma ciò può rendere più difficile da testare, il codice e viola il [principio delle dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="fa99f-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="fa99f-140">Nell'esempio seguente viene illustrato come utilizzare un'istanza di `IDistributedCache` in un componente del middleware semplice:</span><span class="sxs-lookup"><span data-stu-id="fa99f-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="fa99f-141">Nel codice precedente, il valore memorizzato nella cache è di lettura, ma mai scritto.</span><span class="sxs-lookup"><span data-stu-id="fa99f-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="fa99f-142">In questo esempio, il valore viene impostato solo quando si avvia un server e non cambia.</span><span class="sxs-lookup"><span data-stu-id="fa99f-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="fa99f-143">In uno scenario con più server, il server più recente per iniziare sovrascriveranno eventuali valori precedenti che sono stati impostati da altri server.</span><span class="sxs-lookup"><span data-stu-id="fa99f-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="fa99f-144">Il `Get` e `Set` metodi usano il `byte[]` tipo.</span><span class="sxs-lookup"><span data-stu-id="fa99f-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="fa99f-145">Pertanto, il valore della stringa deve essere convertito utilizzando `Encoding.UTF8.GetString` (per `Get`) e `Encoding.UTF8.GetBytes` (per `Set`).</span><span class="sxs-lookup"><span data-stu-id="fa99f-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="fa99f-146">Nell'esempio di codice dal *Startup.cs* Mostra il valore da impostare:</span><span class="sxs-lookup"><span data-stu-id="fa99f-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="fa99f-147">Poiché `IDistributedCache` configurato nel `ConfigureServices` metodo, è disponibile per il `Configure` metodo come parametro.</span><span class="sxs-lookup"><span data-stu-id="fa99f-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="fa99f-148">Aggiungendolo come parametro consentirà l'istanza configurata essere fornito mediante l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="fa99f-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="fa99f-149">Uso di una cache distribuita di Redis</span><span class="sxs-lookup"><span data-stu-id="fa99f-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="fa99f-150">[Redis](https://redis.io/) è un archivio dati in memoria open source, che viene spesso usato come una cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="fa99f-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="fa99f-151">È possibile usarlo in locale ed è possibile configurare un [Cache Redis di Azure](https://azure.microsoft.com/services/cache/) per le app ASP.NET Core ospitate in Azure.</span><span class="sxs-lookup"><span data-stu-id="fa99f-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="fa99f-152">L'app ASP.NET Core consente di configurare l'implementazione della cache mediante un `RedisDistributedCache` istanza.</span><span class="sxs-lookup"><span data-stu-id="fa99f-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="fa99f-153">La cache Redis richiede [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span><span class="sxs-lookup"><span data-stu-id="fa99f-153">The Redis cache requires [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span></span>

<span data-ttu-id="fa99f-154">Si configura l'implementazione di Redis nel `ConfigureServices` e accedervi nel codice dell'app richiedendo un'istanza di `IDistributedCache` (vedere il codice riportato sopra).</span><span class="sxs-lookup"><span data-stu-id="fa99f-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="fa99f-155">Nell'esempio di codice, un `RedisCache` implementazione viene utilizzata quando il server è configurato per un `Staging` ambiente.</span><span class="sxs-lookup"><span data-stu-id="fa99f-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="fa99f-156">In questo modo il `ConfigureStagingServices` metodo consente di configurare il `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="fa99f-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

<span data-ttu-id="fa99f-157">Per installare Redis in locale, installare il pacchetto chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) ed eseguire `redis-server` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="fa99f-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="fa99f-158">Uso di un SQL Server di cache distribuita</span><span class="sxs-lookup"><span data-stu-id="fa99f-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="fa99f-159">L'implementazione SqlServerCache consente alla cache distribuita di utilizzare un database di SQL Server come archivio di backup.</span><span class="sxs-lookup"><span data-stu-id="fa99f-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="fa99f-160">Per creare SQL Server nella tabella è possibile usare lo strumento della cache sql, lo strumento crea una tabella con il nome e lo schema specificati.</span><span class="sxs-lookup"><span data-stu-id="fa99f-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="fa99f-161">Aggiungere `SqlConfig.Tools` per il `<ItemGroup>` elemento del file di progetto ed eseguire `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="fa99f-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="fa99f-162">Testare SqlConfig.Tools eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="fa99f-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="fa99f-163">Consente di visualizzare SqlConfig.Tools utilizzo, opzioni e Guida dei comandi.</span><span class="sxs-lookup"><span data-stu-id="fa99f-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="fa99f-164">Creare una tabella in SQL Server eseguendo il `sql-cache create` comando:</span><span class="sxs-lookup"><span data-stu-id="fa99f-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="fa99f-165">La tabella creata con lo schema seguente:</span><span class="sxs-lookup"><span data-stu-id="fa99f-165">The created table has the following schema:</span></span>

![Tabella della Cache di SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="fa99f-167">Ad esempio tutte le implementazioni della cache, l'app deve ottenere e impostare i valori della cache usando un'istanza di `IDistributedCache`, non un `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="fa99f-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="fa99f-168">Nell'esempio viene implementato `SqlServerCache` nell'ambiente di produzione (in modo che sia configurata in `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="fa99f-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="fa99f-169">Il `ConnectionString` (ed eventualmente `SchemaName` e `TableName`) in genere devono essere archiviate di fuori di controllo del codice sorgente (ad esempio segreti utente), poiché potrebbero contenere credenziali.</span><span class="sxs-lookup"><span data-stu-id="fa99f-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="fa99f-170">Suggerimenti</span><span class="sxs-lookup"><span data-stu-id="fa99f-170">Recommendations</span></span>

<span data-ttu-id="fa99f-171">Quando si decide quale implementazione di `IDistributedCache` è adatto alla tua app, scegliere tra Redis e SQL Server basato su esperienza del team e ambiente, i requisiti di prestazioni e l'infrastruttura esistente.</span><span class="sxs-lookup"><span data-stu-id="fa99f-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="fa99f-172">Se il team ha più abituato a lavorare con Redis, è un'ottima scelta.</span><span class="sxs-lookup"><span data-stu-id="fa99f-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="fa99f-173">Se preferita dal team SQL Server, è possibile essere ritenuto affidabile che anche l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="fa99f-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="fa99f-174">Si noti che una tradizionale soluzione di memorizzazione nella cache vengono archiviati i dati in memoria che consente di recuperare rapidamente i dati.</span><span class="sxs-lookup"><span data-stu-id="fa99f-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="fa99f-175">È necessario archiviare i dati di uso comune in una cache e archiviare tutti i dati in un archivio persistente di back-end, ad esempio archiviazione di Azure o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fa99f-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="fa99f-176">Cache redis è una soluzione di memorizzazione nella cache che offre bassa latenza e velocità effettiva elevata rispetto alle Cache SQL.</span><span class="sxs-lookup"><span data-stu-id="fa99f-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fa99f-177">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fa99f-177">Additional resources</span></span>

* [<span data-ttu-id="fa99f-178">Redis Cache in Azure</span><span class="sxs-lookup"><span data-stu-id="fa99f-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="fa99f-179">Database SQL in Azure</span><span class="sxs-lookup"><span data-stu-id="fa99f-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
