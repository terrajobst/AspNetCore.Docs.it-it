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
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="c2883-103">Utilizzare una cache distribuita in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2883-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="c2883-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c2883-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c2883-105">Le cache distribuite possono migliorare le prestazioni e scalabilità delle app di ASP.NET Core, in particolare quando sono ospitati in un ambiente di farm di server o cloud.</span><span class="sxs-lookup"><span data-stu-id="c2883-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="c2883-106">Questo articolo viene illustrato come utilizzare le implementazioni e astrazioni di cache distribuita incorporata del ASP.NET di base.</span><span class="sxs-lookup"><span data-stu-id="c2883-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="c2883-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c2883-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="c2883-108">Che cos'è una cache distribuita</span><span class="sxs-lookup"><span data-stu-id="c2883-108">What is a distributed cache</span></span>

<span data-ttu-id="c2883-109">Una cache distribuita è condiviso da più server di app (vedere [nozioni di base Cache](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="c2883-109">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="c2883-110">Le informazioni nella cache non vengono memorizzate nella memoria del server web singole e i dati memorizzati nella cache sono disponibili a tutti i server dell'app.</span><span class="sxs-lookup"><span data-stu-id="c2883-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="c2883-111">Ciò offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="c2883-111">This provides several advantages:</span></span>

1. <span data-ttu-id="c2883-112">Dati memorizzati nella cache sono coerenti in tutti i server web.</span><span class="sxs-lookup"><span data-stu-id="c2883-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="c2883-113">Gli utenti non visualizzati risultati diversi a seconda di quale web server gestisce la richiesta</span><span class="sxs-lookup"><span data-stu-id="c2883-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="c2883-114">Dati memorizzati nella cache viene conservata riavvio del server web e le distribuzioni.</span><span class="sxs-lookup"><span data-stu-id="c2883-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="c2883-115">Server web singoli possono essere rimossi o aggiunti senza conseguenze per la cache.</span><span class="sxs-lookup"><span data-stu-id="c2883-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="c2883-116">L'archivio dati di origine ha meno le richieste effettuate a esso (che con più cache in memoria o su no cache affatto).</span><span class="sxs-lookup"><span data-stu-id="c2883-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="c2883-117">Se si usa una Cache distribuita di SQL Server, alcuni di questi vantaggi sono solo true se viene utilizzata per la cache per i dati di origine dell'app rispetto a un'istanza separata del database.</span><span class="sxs-lookup"><span data-stu-id="c2883-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="c2883-118">Come qualsiasi cache, una cache distribuita può migliorare notevolmente la velocità di risposta dell'app, poiché in genere i dati possono essere recuperati dalla cache molto più veloce da un database relazionale (o un servizio web).</span><span class="sxs-lookup"><span data-stu-id="c2883-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="c2883-119">Configurazione della cache è specifica dell'implementazione.</span><span class="sxs-lookup"><span data-stu-id="c2883-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="c2883-120">In questo articolo viene descritto come configurare entrambi Redis e distribuite di SQL Server memorizza nella cache.</span><span class="sxs-lookup"><span data-stu-id="c2883-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="c2883-121">Indipendentemente da quale implementazione è selezionata, l'applicazione interagisce con la cache utilizzando una comune `IDistributedCache` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="c2883-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="c2883-122">L'interfaccia IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="c2883-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="c2883-123">Il `IDistributedCache` interfaccia include metodi sincroni e asincroni.</span><span class="sxs-lookup"><span data-stu-id="c2883-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="c2883-124">L'interfaccia consente gli elementi aggiunti, recuperati, e la rimozione rispetto all'implementazione di cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="c2883-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="c2883-125">Il `IDistributedCache` interfaccia include i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c2883-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="c2883-126">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="c2883-126">**Get, GetAsync**</span></span>

<span data-ttu-id="c2883-127">Accetta una chiave di stringa e recupera un elemento memorizzato nella cache come una `byte[]` se trovato nella cache.</span><span class="sxs-lookup"><span data-stu-id="c2883-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="c2883-128">**Set, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="c2883-128">**Set, SetAsync**</span></span>

<span data-ttu-id="c2883-129">Aggiunge un elemento (come `byte[]`) alla cache utilizzando una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="c2883-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="c2883-130">**Aggiornamento, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="c2883-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="c2883-131">Aggiorna un elemento nella cache in base alla chiave, reimpostare il timeout di scadenza scorrevole (se presente).</span><span class="sxs-lookup"><span data-stu-id="c2883-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="c2883-132">**Rimuovere, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="c2883-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="c2883-133">Rimuove una voce della cache basata sulla relativa chiave.</span><span class="sxs-lookup"><span data-stu-id="c2883-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="c2883-134">Utilizzare il `IDistributedCache` interfaccia:</span><span class="sxs-lookup"><span data-stu-id="c2883-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="c2883-135">Aggiungere i pacchetti NuGet necessari al file di progetto.</span><span class="sxs-lookup"><span data-stu-id="c2883-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="c2883-136">Configurare l'implementazione specifica di `IDistributedCache` nella `Startup` della classe `ConfigureServices` (metodo) e aggiungerlo al contenitore di.</span><span class="sxs-lookup"><span data-stu-id="c2883-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="c2883-137">Dell'app [Middleware](xref:fundamentals/middleware/index) o le classi controller MVC, è richiesta un'istanza di `IDistributedCache` dal costruttore.</span><span class="sxs-lookup"><span data-stu-id="c2883-137">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="c2883-138">L'istanza verrà fornita da [inserimento di dipendenze](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="c2883-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="c2883-139">Non è necessario utilizzare una durata Singleton o nell'ambito per `IDistributedCache` istanze (almeno per le implementazioni predefinite).</span><span class="sxs-lookup"><span data-stu-id="c2883-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="c2883-140">È inoltre possibile creare un'istanza ogni volta che potrebbe essere necessario uno (invece di usare [inserimento di dipendenze](../../fundamentals/dependency-injection.md)), ma ciò può rendere più difficile da testare, il codice e viola il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="c2883-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="c2883-141">Nell'esempio seguente viene illustrato come utilizzare un'istanza di `IDistributedCache` in un componente del middleware semplice:</span><span class="sxs-lookup"><span data-stu-id="c2883-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="c2883-142">Nel codice precedente, il valore memorizzato nella cache viene letto ma mai scritto.</span><span class="sxs-lookup"><span data-stu-id="c2883-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="c2883-143">In questo esempio, il valore viene impostato solo quando un server di avvio e non cambia.</span><span class="sxs-lookup"><span data-stu-id="c2883-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="c2883-144">In uno scenario con più server, il server più recente per l'avvio sovrascriverà i valori precedenti che sono stati impostati da altri server.</span><span class="sxs-lookup"><span data-stu-id="c2883-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="c2883-145">Il `Get` e `Set` metodi utilizzano il `byte[]` tipo.</span><span class="sxs-lookup"><span data-stu-id="c2883-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="c2883-146">Pertanto, il valore di stringa deve essere convertito utilizzando `Encoding.UTF8.GetString` (per `Get`) e `Encoding.UTF8.GetBytes` (per `Set`).</span><span class="sxs-lookup"><span data-stu-id="c2883-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="c2883-147">Nell'esempio di codice da *Startup.cs* Mostra il valore da impostare:</span><span class="sxs-lookup"><span data-stu-id="c2883-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="c2883-148">Poiché `IDistributedCache` è configurato nel `ConfigureServices` metodo, è disponibile per il `Configure` metodo come parametro.</span><span class="sxs-lookup"><span data-stu-id="c2883-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="c2883-149">Aggiunta come un parametro consentirà l'istanza configurata essere fornito mediante (DISCONNECTED Inbound).</span><span class="sxs-lookup"><span data-stu-id="c2883-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="c2883-150">Uso di una cache Redis distribuiti</span><span class="sxs-lookup"><span data-stu-id="c2883-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="c2883-151">[Redis](https://redis.io/) è un archivio Apri origine dati in memoria, che viene spesso usato come una cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="c2883-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="c2883-152">È possibile usare in locale ed è possibile configurare un [Cache Redis di Azure](https://azure.microsoft.com/services/cache/) per le app ASP.NET Core ospitato di Azure.</span><span class="sxs-lookup"><span data-stu-id="c2883-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="c2883-153">L'app ASP.NET Core consente di configurare l'implementazione di cache con un `RedisDistributedCache` istanza.</span><span class="sxs-lookup"><span data-stu-id="c2883-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="c2883-154">Configurare l'implementazione di Redis in `ConfigureServices` e diritti di accesso nel codice dell'app richiedendo un'istanza di `IDistributedCache` (vedere il codice precedente).</span><span class="sxs-lookup"><span data-stu-id="c2883-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="c2883-155">Nell'esempio di codice, un `RedisCache` implementazione viene utilizzata quando il server è configurato per un `Staging` ambiente.</span><span class="sxs-lookup"><span data-stu-id="c2883-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="c2883-156">In questo modo il `ConfigureStagingServices` metodo consente di configurare il `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="c2883-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="c2883-157">Per installare Redis sul computer locale, installare il pacchetto chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) ed eseguire `redis-server` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c2883-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="c2883-158">Utilizzo di un Server SQL cache distribuita</span><span class="sxs-lookup"><span data-stu-id="c2883-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="c2883-159">L'implementazione SqlServerCache consente la cache distribuita utilizzare un database di SQL Server come archivio di backup.</span><span class="sxs-lookup"><span data-stu-id="c2883-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="c2883-160">Per creare il Server SQL table è possibile utilizzare lo strumento della cache sql, lo strumento crea una tabella con il nome e lo schema specificato.</span><span class="sxs-lookup"><span data-stu-id="c2883-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="c2883-161">Aggiungere `SqlConfig.Tools` per il `<ItemGroup>` elemento del file di progetto e run `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="c2883-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="c2883-162">Testare SqlConfig.Tools eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c2883-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="c2883-163">SqlConfig.Tools Visualizza utilizzo, le opzioni e la Guida di comando.</span><span class="sxs-lookup"><span data-stu-id="c2883-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="c2883-164">Creare una tabella in SQL Server eseguendo il `sql-cache create` comando:</span><span class="sxs-lookup"><span data-stu-id="c2883-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="c2883-165">La tabella creata con lo schema seguente:</span><span class="sxs-lookup"><span data-stu-id="c2883-165">The created table has the following schema:</span></span>

![Tabella della Cache di SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="c2883-167">Come tutte le implementazioni di cache, l'app deve ottenere e impostare i valori della cache usando un'istanza di `IDistributedCache`, non un `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="c2883-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="c2883-168">Nell'esempio viene implementato `SqlServerCache` nell'ambiente di produzione (in modo che sia configurato `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="c2883-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="c2883-169">Il `ConnectionString` (ed eventualmente `SchemaName` e `TableName`) in genere devono essere archiviate di fuori di controllo del codice sorgente (ad esempio segreti utente), poiché potrebbero contenere le credenziali.</span><span class="sxs-lookup"><span data-stu-id="c2883-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="c2883-170">Suggerimenti</span><span class="sxs-lookup"><span data-stu-id="c2883-170">Recommendations</span></span>

<span data-ttu-id="c2883-171">Quando si decide quale implementazione di `IDistributedCache` è a destra per l'app, scegliere tra Redis e SQL Server in base l'infrastruttura esistente e ambiente, i requisiti di prestazioni ed esperienza del team.</span><span class="sxs-lookup"><span data-stu-id="c2883-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="c2883-172">Se il team è più abituato a lavorare con Redis, è un'ottima scelta.</span><span class="sxs-lookup"><span data-stu-id="c2883-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="c2883-173">Se il team si preferisce SQL Server, è possibile avere la certezza in che anche l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="c2883-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="c2883-174">Si noti che una tradizionale soluzione di memorizzazione nella cache vengono archiviati i dati in memoria che consente il recupero veloce dei dati.</span><span class="sxs-lookup"><span data-stu-id="c2883-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="c2883-175">È consigliabile archiviare i dati utilizzati in una cache e archiviare tutti i dati in un archivio permanente back-end, ad esempio SQL Server o di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="c2883-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="c2883-176">Cache redis è una soluzione di memorizzazione nella cache che consente una velocità effettiva elevata e latenza bassa rispetto alle Cache SQL.</span><span class="sxs-lookup"><span data-stu-id="c2883-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2883-177">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c2883-177">Additional resources</span></span>

* [<span data-ttu-id="c2883-178">Redis Cache in Azure</span><span class="sxs-lookup"><span data-stu-id="c2883-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="c2883-179">Database SQL in Azure</span><span class="sxs-lookup"><span data-stu-id="c2883-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="c2883-180">Cache in memoria</span><span class="sxs-lookup"><span data-stu-id="c2883-180">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="c2883-181">Rilevare le modifiche apportate con i token di modifica</span><span class="sxs-lookup"><span data-stu-id="c2883-181">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="c2883-182">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="c2883-182">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="c2883-183">Middleware di memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="c2883-183">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="c2883-184">Helper per tag di cache</span><span class="sxs-lookup"><span data-stu-id="c2883-184">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="c2883-185">Helper per tag di cache distribuita</span><span class="sxs-lookup"><span data-stu-id="c2883-185">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
