---
title: Caching distribuito in ASP.NET Core
author: guardrex
description: Informazioni su come usare una cache distribuita ASP.NET Core per migliorare le prestazioni e la scalabilità delle app, soprattutto in un ambiente cloud o server farm.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/caching/distributed
ms.openlocfilehash: 3e039a26505aed1bcc0299880039760fef19fd67
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727238"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="179bf-103">Caching distribuito in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="179bf-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="179bf-104">Di [Luke Latham](https://github.com/guardrex), di la [Nasir Nasir](https://github.com/mohsinnasir)e di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="179bf-104">By [Luke Latham](https://github.com/guardrex), [Mohsin Nasir](https://github.com/mohsinnasir), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="179bf-105">Una cache distribuita è una cache condivisa da più server app, in genere gestita come servizio esterno ai server app che vi accedono.</span><span class="sxs-lookup"><span data-stu-id="179bf-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="179bf-106">Una cache distribuita può migliorare le prestazioni e la scalabilità di un'app ASP.NET Core, soprattutto quando l'app è ospitata da un servizio cloud o da un server farm.</span><span class="sxs-lookup"><span data-stu-id="179bf-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="179bf-107">Una cache distribuita presenta diversi vantaggi rispetto ad altri scenari di memorizzazione nella cache in cui i dati memorizzati nella cache vengono archiviati nei singoli server dell'app.</span><span class="sxs-lookup"><span data-stu-id="179bf-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="179bf-108">Quando vengono distribuiti i dati memorizzati nella cache, i dati:</span><span class="sxs-lookup"><span data-stu-id="179bf-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="179bf-109">È *coerente* (coerente) tra le richieste a più server.</span><span class="sxs-lookup"><span data-stu-id="179bf-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="179bf-110">Sopravvive ai riavvii del server e alle distribuzioni di app.</span><span class="sxs-lookup"><span data-stu-id="179bf-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="179bf-111">Non usa la memoria locale.</span><span class="sxs-lookup"><span data-stu-id="179bf-111">Doesn't use local memory.</span></span>

<span data-ttu-id="179bf-112">La configurazione della cache distribuita è specifica dell'implementazione.</span><span class="sxs-lookup"><span data-stu-id="179bf-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="179bf-113">Questo articolo descrive come configurare le cache distribuite SQL Server e Redis.</span><span class="sxs-lookup"><span data-stu-id="179bf-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="179bf-114">Sono disponibili anche implementazioni di terze parti, ad esempio [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache su GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="179bf-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="179bf-115">Indipendentemente dall'implementazione selezionata, l'app interagisce con la cache usando l'interfaccia <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>.</span><span class="sxs-lookup"><span data-stu-id="179bf-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="179bf-116">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="179bf-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="179bf-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="179bf-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="179bf-118">Per utilizzare una SQL Server cache distribuita, aggiungere un riferimento al pacchetto [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="179bf-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="179bf-119">Per usare una cache distribuita Redis, aggiungere un riferimento al pacchetto [Microsoft. Extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="179bf-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="179bf-120">Per usare la cache distribuita NCache, aggiungere un riferimento al pacchetto [NCache. Microsoft. Extensions. Caching. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="179bf-120">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="179bf-121">Per usare una cache distribuita SQL Server, fare riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) o aggiungere un riferimento al pacchetto [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="179bf-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="179bf-122">Per usare una cache distribuita Redis, fare riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) e aggiungere un riferimento al pacchetto [Microsoft. Extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="179bf-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="179bf-123">Il pacchetto Redis non è incluso nel pacchetto di `Microsoft.AspNetCore.App`, quindi è necessario fare riferimento al pacchetto Redis separatamente nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="179bf-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="179bf-124">Per usare la cache distribuita NCache, fare riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) e aggiungere un riferimento al pacchetto [NCache. Microsoft. Extensions. Caching. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="179bf-124">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="179bf-125">Il pacchetto NCache non è incluso nel pacchetto di `Microsoft.AspNetCore.App`, quindi è necessario fare riferimento al pacchetto NCache separatamente nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="179bf-125">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="179bf-126">Per usare una cache distribuita SQL Server, fare riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) o aggiungere un riferimento al pacchetto [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="179bf-126">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="179bf-127">Per usare una cache distribuita Redis, fare riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) e aggiungere un riferimento al pacchetto [Microsoft. Extensions. Caching. Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) .</span><span class="sxs-lookup"><span data-stu-id="179bf-127">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="179bf-128">Il pacchetto Redis non è incluso nel pacchetto di `Microsoft.AspNetCore.App`, quindi è necessario fare riferimento al pacchetto Redis separatamente nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="179bf-128">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="179bf-129">Per usare la cache distribuita NCache, fare riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) e aggiungere un riferimento al pacchetto [NCache. Microsoft. Extensions. Caching. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="179bf-129">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="179bf-130">Il pacchetto NCache non è incluso nel pacchetto di `Microsoft.AspNetCore.App`, quindi è necessario fare riferimento al pacchetto NCache separatamente nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="179bf-130">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="179bf-131">Interfaccia IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="179bf-131">IDistributedCache interface</span></span>

<span data-ttu-id="179bf-132">L'interfaccia <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> fornisce i metodi seguenti per modificare gli elementi nell'implementazione della cache distribuita:</span><span class="sxs-lookup"><span data-stu-id="179bf-132">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="179bf-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; accetta una chiave di stringa e recupera un elemento memorizzato nella cache come matrice `byte[]` se viene trovato nella cache.</span><span class="sxs-lookup"><span data-stu-id="179bf-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="179bf-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; aggiunge un elemento, come `byte[]` matrice, alla cache usando una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="179bf-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="179bf-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; aggiorna un elemento nella cache in base alla relativa chiave, reimpostando il timeout di scadenza variabile (se presente).</span><span class="sxs-lookup"><span data-stu-id="179bf-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="179bf-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; rimuove un elemento della cache in base alla relativa chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="179bf-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="179bf-137">Stabilire servizi di memorizzazione nella cache distribuiti</span><span class="sxs-lookup"><span data-stu-id="179bf-137">Establish distributed caching services</span></span>

<span data-ttu-id="179bf-138">Registrare un'implementazione di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="179bf-138">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="179bf-139">Le implementazioni fornite dal Framework descritte in questo argomento includono:</span><span class="sxs-lookup"><span data-stu-id="179bf-139">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="179bf-140">Cache di memoria distribuita</span><span class="sxs-lookup"><span data-stu-id="179bf-140">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="179bf-141">Cache SQL Server distribuita</span><span class="sxs-lookup"><span data-stu-id="179bf-141">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="179bf-142">Cache Redis distribuita</span><span class="sxs-lookup"><span data-stu-id="179bf-142">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="179bf-143">Cache NCache distribuita</span><span class="sxs-lookup"><span data-stu-id="179bf-143">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="179bf-144">Cache di memoria distribuita</span><span class="sxs-lookup"><span data-stu-id="179bf-144">Distributed Memory Cache</span></span>

<span data-ttu-id="179bf-145">La cache di memoria distribuita (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) è un'implementazione fornita dal framework di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> che archivia gli elementi in memoria.</span><span class="sxs-lookup"><span data-stu-id="179bf-145">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="179bf-146">La cache di memoria distribuita non è una cache distribuita effettiva.</span><span class="sxs-lookup"><span data-stu-id="179bf-146">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="179bf-147">Gli elementi memorizzati nella cache vengono archiviati dall'istanza dell'app nel server in cui è in esecuzione l'app.</span><span class="sxs-lookup"><span data-stu-id="179bf-147">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="179bf-148">La cache di memoria distribuita è un'implementazione utile:</span><span class="sxs-lookup"><span data-stu-id="179bf-148">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="179bf-149">Negli scenari di sviluppo e test.</span><span class="sxs-lookup"><span data-stu-id="179bf-149">In development and testing scenarios.</span></span>
* <span data-ttu-id="179bf-150">Quando un singolo server viene utilizzato nell'ambiente di produzione e l'utilizzo della memoria non costituisce un problema.</span><span class="sxs-lookup"><span data-stu-id="179bf-150">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="179bf-151">L'implementazione della cache di memoria distribuita astrae l'archiviazione dei dati memorizzati nella cache.</span><span class="sxs-lookup"><span data-stu-id="179bf-151">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="179bf-152">Consente di implementare in futuro una soluzione di memorizzazione nella cache distribuita se più nodi o tolleranza di errore diventano necessari.</span><span class="sxs-lookup"><span data-stu-id="179bf-152">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="179bf-153">L'app di esempio usa la cache di memoria distribuita quando l'app viene eseguita nell'ambiente di sviluppo in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="179bf-153">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="179bf-154">Cache SQL Server distribuita</span><span class="sxs-lookup"><span data-stu-id="179bf-154">Distributed SQL Server Cache</span></span>

<span data-ttu-id="179bf-155">L'implementazione della cache SQL Server distribuita (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) consente alla cache distribuita di utilizzare un database SQL Server come archivio di backup.</span><span class="sxs-lookup"><span data-stu-id="179bf-155">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="179bf-156">Per creare una tabella SQL Server elemento memorizzato nella cache in un'istanza di SQL Server, è possibile utilizzare lo strumento `sql-cache`.</span><span class="sxs-lookup"><span data-stu-id="179bf-156">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="179bf-157">Lo strumento crea una tabella con il nome e lo schema specificati.</span><span class="sxs-lookup"><span data-stu-id="179bf-157">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="179bf-158">Creare una tabella in SQL Server eseguendo il comando `sql-cache create`.</span><span class="sxs-lookup"><span data-stu-id="179bf-158">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="179bf-159">Fornire l'istanza di SQL Server (`Data Source`), il database (`Initial Catalog`), lo schema (ad esempio, `dbo`) e il nome della tabella (ad esempio, `TestCache`):</span><span class="sxs-lookup"><span data-stu-id="179bf-159">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="179bf-160">Viene registrato un messaggio per indicare che lo strumento è stato completato correttamente:</span><span class="sxs-lookup"><span data-stu-id="179bf-160">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="179bf-161">La tabella creata dallo strumento `sql-cache` dispone dello schema seguente:</span><span class="sxs-lookup"><span data-stu-id="179bf-161">The table created by the `sql-cache` tool has the following schema:</span></span>

![Tabella cache SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="179bf-163">Un'app deve modificare i valori della cache usando un'istanza di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, non una <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span><span class="sxs-lookup"><span data-stu-id="179bf-163">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="179bf-164">L'app di esempio implementa <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in un ambiente non di sviluppo in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="179bf-164">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="179bf-165">Un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (e, facoltativamente, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> e <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) vengono in genere archiviati all'esterno del controllo del codice sorgente (ad esempio, archiviati dal [gestore dei segreti](xref:security/app-secrets) o in *appsettings. JSON*/*appSettings. { ENVIRONMENT} file JSON* ).</span><span class="sxs-lookup"><span data-stu-id="179bf-165">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="179bf-166">La stringa di connessione può contenere credenziali che devono essere mantenute fuori dai sistemi di controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="179bf-166">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="179bf-167">Cache Redis distribuita</span><span class="sxs-lookup"><span data-stu-id="179bf-167">Distributed Redis Cache</span></span>

<span data-ttu-id="179bf-168">[Redis](https://redis.io/) è un archivio dati in memoria open source, che viene spesso usato come cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="179bf-168">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="179bf-169">È possibile usare Redis localmente ed è possibile configurare una [cache Redis di Azure](https://azure.microsoft.com/services/cache/) per un'app ASP.NET Core ospitata in Azure.</span><span class="sxs-lookup"><span data-stu-id="179bf-169">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="179bf-170">Un'app configura l'implementazione della cache usando un'istanza di <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in un ambiente non di sviluppo in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="179bf-170">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="179bf-171">Un'app configura l'implementazione della cache usando un'istanza di <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in un ambiente non di sviluppo in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="179bf-171">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="179bf-172">Un'app configura l'implementazione della cache usando un'istanza di <xref:Microsoft.Extensions.Caching.Redis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span><span class="sxs-lookup"><span data-stu-id="179bf-172">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="179bf-173">Per installare Redis nel computer locale:</span><span class="sxs-lookup"><span data-stu-id="179bf-173">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="179bf-174">Installare il [pacchetto di cioccolaty Redis](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="179bf-174">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="179bf-175">Eseguire `redis-server` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="179bf-175">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="179bf-176">Cache NCache distribuita</span><span class="sxs-lookup"><span data-stu-id="179bf-176">Distributed NCache Cache</span></span>

<span data-ttu-id="179bf-177">[NCache](https://github.com/Alachisoft/NCache) è una cache distribuita in memoria open source sviluppata in modo nativo in .NET e .NET Core.</span><span class="sxs-lookup"><span data-stu-id="179bf-177">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="179bf-178">NCache funziona sia localmente che configurato come cluster di cache distribuita per un'app ASP.NET Core in esecuzione in Azure o in altre piattaforme di hosting.</span><span class="sxs-lookup"><span data-stu-id="179bf-178">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="179bf-179">Per installare e configurare NCache nel computer locale, vedere [NCache Introduzione Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span><span class="sxs-lookup"><span data-stu-id="179bf-179">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="179bf-180">Per configurare NCache:</span><span class="sxs-lookup"><span data-stu-id="179bf-180">To configure NCache:</span></span>

1. <span data-ttu-id="179bf-181">Installare [NCache Open Source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span><span class="sxs-lookup"><span data-stu-id="179bf-181">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="179bf-182">Configurare il cluster di cache in [client. ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span><span class="sxs-lookup"><span data-stu-id="179bf-182">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="179bf-183">Aggiungi il seguente codice a `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="179bf-183">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="179bf-184">Usare la cache distribuita</span><span class="sxs-lookup"><span data-stu-id="179bf-184">Use the distributed cache</span></span>

<span data-ttu-id="179bf-185">Per usare l'interfaccia <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, richiedere un'istanza di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> da qualsiasi costruttore nell'app.</span><span class="sxs-lookup"><span data-stu-id="179bf-185">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="179bf-186">L'istanza viene fornita da [inserimento delle dipendenze (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="179bf-186">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="179bf-187">Quando viene avviata l'app di esempio, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> viene inserita nel `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="179bf-187">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="179bf-188">L'ora corrente viene memorizzata nella cache usando <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (per altre informazioni, vedere [host generico: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span><span class="sxs-lookup"><span data-stu-id="179bf-188">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="179bf-189">Quando viene avviata l'app di esempio, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> viene inserita nel `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="179bf-189">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="179bf-190">L'ora corrente viene memorizzata nella cache usando <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (per altre informazioni, vedere [Web host: interfaccia IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span><span class="sxs-lookup"><span data-stu-id="179bf-190">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="179bf-191">L'app di esempio inserisce <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> nel `IndexModel` per l'uso da parte della pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="179bf-191">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="179bf-192">Ogni volta che viene caricata la pagina di indice, nella cache viene verificata l'ora memorizzata nella cache `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="179bf-192">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="179bf-193">Se il tempo di memorizzazione nella cache non è scaduto, viene visualizzata l'ora.</span><span class="sxs-lookup"><span data-stu-id="179bf-193">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="179bf-194">Se sono trascorsi 20 secondi dall'ultima volta in cui è stato eseguito l'accesso all'ora memorizzata nella cache (l'ultima volta che questa pagina è stata caricata), la pagina Visualizza l' *ora della cache scaduta*.</span><span class="sxs-lookup"><span data-stu-id="179bf-194">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="179bf-195">Aggiornare immediatamente l'ora memorizzata nella cache all'ora corrente selezionando il pulsante **Reimposta tempo memorizzato nella cache** .</span><span class="sxs-lookup"><span data-stu-id="179bf-195">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="179bf-196">Il pulsante attiva il metodo del gestore `OnPostResetCachedTime`.</span><span class="sxs-lookup"><span data-stu-id="179bf-196">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="179bf-197">Non è necessario usare un singleton o una durata con ambito per le istanze di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (almeno per le implementazioni predefinite).</span><span class="sxs-lookup"><span data-stu-id="179bf-197">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="179bf-198">È anche possibile creare un'istanza di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ovunque sia necessario, anziché usare DI, ma la creazione di un'istanza nel codice può rendere il codice più difficile da testare e violare il [principio delle dipendenze esplicite](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="179bf-198">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="179bf-199">Suggerimenti</span><span class="sxs-lookup"><span data-stu-id="179bf-199">Recommendations</span></span>

<span data-ttu-id="179bf-200">Quando si decide quale implementazione di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> è migliore per l'app, tenere presente quanto segue:</span><span class="sxs-lookup"><span data-stu-id="179bf-200">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="179bf-201">Infrastruttura esistente</span><span class="sxs-lookup"><span data-stu-id="179bf-201">Existing infrastructure</span></span>
* <span data-ttu-id="179bf-202">Requisiti relativi alle prestazioni</span><span class="sxs-lookup"><span data-stu-id="179bf-202">Performance requirements</span></span>
* <span data-ttu-id="179bf-203">Cost</span><span class="sxs-lookup"><span data-stu-id="179bf-203">Cost</span></span>
* <span data-ttu-id="179bf-204">Esperienza del team</span><span class="sxs-lookup"><span data-stu-id="179bf-204">Team experience</span></span>

<span data-ttu-id="179bf-205">Le soluzioni per la memorizzazione nella cache si basano in genere sull'archiviazione in memoria per consentire un rapido recupero dei dati memorizzati nella cache, ma la memoria è una risorsa limitata ed è dispendiosa per espanderla.</span><span class="sxs-lookup"><span data-stu-id="179bf-205">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="179bf-206">Archiviare solo i dati utilizzati di frequente in una cache.</span><span class="sxs-lookup"><span data-stu-id="179bf-206">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="179bf-207">In genere, una cache Redis offre una velocità effettiva più elevata e una latenza inferiore rispetto a una cache SQL Server.</span><span class="sxs-lookup"><span data-stu-id="179bf-207">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="179bf-208">Tuttavia, il benchmarking è in genere necessario per determinare le caratteristiche di prestazioni delle strategie di memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="179bf-208">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="179bf-209">Quando SQL Server viene usato come archivio di backup della cache distribuita, l'uso dello stesso database per la cache e l'archiviazione e il recupero dei dati ordinari dell'app possono influire negativamente sulle prestazioni di entrambi.</span><span class="sxs-lookup"><span data-stu-id="179bf-209">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="179bf-210">Si consiglia di usare un'istanza di SQL Server dedicata per l'archivio di backup della cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="179bf-210">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="179bf-211">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="179bf-211">Additional resources</span></span>

* [<span data-ttu-id="179bf-212">Cache Redis in Azure</span><span class="sxs-lookup"><span data-stu-id="179bf-212">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="179bf-213">Database SQL in Azure</span><span class="sxs-lookup"><span data-stu-id="179bf-213">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="179bf-214">[ASP.NET Core provider IDistributedCache per NCache in Web farm](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache su GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="179bf-214">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
