---
title: Memorizzare nella cache in memoria in ASP.NET Core
author: rick-anderson
description: Imparare a memorizzare nella cache i dati in memoria in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/14/2016
uid: performance/caching/memory
ms.openlocfilehash: 5a7085269e255ae233a0e7eeb860a04b2c6bede3
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077586"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="2602f-103">Memorizzare nella cache in memoria in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2602f-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="2602f-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2602f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2602f-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2602f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="2602f-106">Nozioni fondamentali sulla memorizzazione nella cache</span><span class="sxs-lookup"><span data-stu-id="2602f-106">Caching basics</span></span>

<span data-ttu-id="2602f-107">La memorizzazione nella cache può migliorare significativamente le prestazioni e scalabilità di un'app, riducendo il lavoro necessario per generare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="2602f-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="2602f-108">Memorizzazione nella cache funziona meglio con i dati che vengono modificati raramente.</span><span class="sxs-lookup"><span data-stu-id="2602f-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="2602f-109">La memorizzazione nella cache crea una copia di dati che possono essere restituite molto più veloce rispetto dalla fonte originale.</span><span class="sxs-lookup"><span data-stu-id="2602f-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="2602f-110">È necessario scrivere e testare l'app per mai dipendono dai dati memorizzati nella cache.</span><span class="sxs-lookup"><span data-stu-id="2602f-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="2602f-111">ASP.NET Core supporta diverse cache diverse.</span><span class="sxs-lookup"><span data-stu-id="2602f-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="2602f-112">Si basa la cache più semplice la [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), che rappresenta una cache archiviata nella memoria del server web.</span><span class="sxs-lookup"><span data-stu-id="2602f-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="2602f-113">Le app che vengono eseguiti in una server farm di più server è necessario assicurarsi che le sessioni sono permanenti quando si utilizza la cache in memoria.</span><span class="sxs-lookup"><span data-stu-id="2602f-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="2602f-114">Le sessioni permanenti garantire che le successive richieste da un client tutti nello stesso server.</span><span class="sxs-lookup"><span data-stu-id="2602f-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="2602f-115">Ad esempio, uso di App Web di Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) per il routing di tutte le richieste successive nello stesso server.</span><span class="sxs-lookup"><span data-stu-id="2602f-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="2602f-116">Le sessioni permanenti non in una web farm richiedono un [cache distribuita](distributed.md) per evitare problemi di coerenza della cache.</span><span class="sxs-lookup"><span data-stu-id="2602f-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="2602f-117">Per alcune App, una cache distribuita può supportare una maggiore scalabilità orizzontale rispetto a una cache in memoria.</span><span class="sxs-lookup"><span data-stu-id="2602f-117">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="2602f-118">Uso di una cache distribuita trasferisce il carico di memoria cache per un processo esterno.</span><span class="sxs-lookup"><span data-stu-id="2602f-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="2602f-119">Il `IMemoryCache` cache rimuove le voci della cache eccessivo della memoria, a meno che il [memorizza nella cache priorità](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) è impostata su `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="2602f-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="2602f-120">È possibile impostare il `CacheItemPriority` per regolare la priorità con cui la cache rimuove gli elementi eccessivo della memoria.</span><span class="sxs-lookup"><span data-stu-id="2602f-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="2602f-121">La cache in memoria può memorizzare qualsiasi oggetto. l'interfaccia cache distribuita è limitato a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="2602f-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="2602f-122">Utilizzando IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="2602f-122">Using IMemoryCache</span></span>

<span data-ttu-id="2602f-123">La memorizzazione nella cache in memoria è un *servizio* che viene fatto riferimento dall'app utilizzando [inserimento di dipendenze](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="2602f-123">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="2602f-124">Chiamare `AddMemoryCache` in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2602f-124">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="2602f-125">Richiedere il `IMemoryCache` istanza nel costruttore:</span><span class="sxs-lookup"><span data-stu-id="2602f-125">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2602f-126">`IMemoryCache` richiede il pacchetto NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="2602f-126">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2602f-127">`IMemoryCache` richiede il pacchetto NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), che è disponibile nel [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="2602f-127">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="2602f-128">`IMemoryCache` richiede il pacchetto NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), che è disponibile nel [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2602f-128">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="2602f-129">Il codice seguente usa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) per verificare se un'ora è nella cache.</span><span class="sxs-lookup"><span data-stu-id="2602f-129">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="2602f-130">Se non viene memorizzato nella cache di un'ora, una nuova voce viene creata e aggiunto alla cache con [impostare](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="2602f-130">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="2602f-131">L'ora corrente e l'ora memorizzati nella cache vengono visualizzati:</span><span class="sxs-lookup"><span data-stu-id="2602f-131">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="2602f-132">Memorizzato nella cache `DateTime` valore rimane nella cache mentre vi sono richieste entro il periodo di timeout (e nessuna rimozione dovuta a pressione della memoria).</span><span class="sxs-lookup"><span data-stu-id="2602f-132">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="2602f-133">La figura seguente mostra l'ora corrente e un'ora precedente recuperato dalla cache:</span><span class="sxs-lookup"><span data-stu-id="2602f-133">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Visualizzazione dell'indice con due diverse volte visualizzato](memory/_static/time.png)

<span data-ttu-id="2602f-135">Il codice seguente usa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) per memorizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="2602f-135">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="2602f-136">Il codice seguente chiama [ottenere](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) per recuperare l'ora memorizzati nella cache:</span><span class="sxs-lookup"><span data-stu-id="2602f-136">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="2602f-137">Vedere [metodi IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) e [CacheExtensions metodi](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) per una descrizione dei metodi della cache.</span><span class="sxs-lookup"><span data-stu-id="2602f-137">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="2602f-138">Utilizzo MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="2602f-138">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="2602f-139">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="2602f-139">The following sample:</span></span>

- <span data-ttu-id="2602f-140">Imposta l'ora di scadenza assoluta.</span><span class="sxs-lookup"><span data-stu-id="2602f-140">Sets the absolute expiration time.</span></span> <span data-ttu-id="2602f-141">Questo è il tempo massimo che può essere memorizzati nella cache la voce e impedisce che l'elemento obsolescenza troppo quando la scadenza variabile viene rinnovata in modo continuo.</span><span class="sxs-lookup"><span data-stu-id="2602f-141">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="2602f-142">Imposta una scadenza variabile.</span><span class="sxs-lookup"><span data-stu-id="2602f-142">Sets a sliding expiration time.</span></span> <span data-ttu-id="2602f-143">Le richieste che accedono a questo elemento memorizzato nella cache verranno reimpostato il clock di scadenza scorrevole.</span><span class="sxs-lookup"><span data-stu-id="2602f-143">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="2602f-144">Imposta la priorità di cache su `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="2602f-144">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="2602f-145">Imposta una [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) che verrà chiamato dopo la voce viene rimossa dalla cache.</span><span class="sxs-lookup"><span data-stu-id="2602f-145">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="2602f-146">Il callback viene eseguito su un thread diverso dal codice che rimuove l'elemento dalla cache.</span><span class="sxs-lookup"><span data-stu-id="2602f-146">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a><span data-ttu-id="2602f-147">Dipendenze della cache</span><span class="sxs-lookup"><span data-stu-id="2602f-147">Cache dependencies</span></span>

<span data-ttu-id="2602f-148">L'esempio seguente viene illustrato come la scadenza di una voce della cache se scade una voce di dipendenti.</span><span class="sxs-lookup"><span data-stu-id="2602f-148">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="2602f-149">Oggetto `CancellationChangeToken` viene aggiunto all'elemento memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="2602f-149">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="2602f-150">Quando si `Cancel` viene chiamato sul `CancellationTokenSource`, entrambe le voci della cache vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="2602f-150">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="2602f-151">Utilizzando un `CancellationTokenSource` consente a più voci nella cache da rimuovere come gruppo.</span><span class="sxs-lookup"><span data-stu-id="2602f-151">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="2602f-152">Con il `using` motivo nel codice precedente, le voci della cache creato all'interno di `using` blocco erediterà i trigger e le impostazioni di scadenza.</span><span class="sxs-lookup"><span data-stu-id="2602f-152">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="2602f-153">Note aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2602f-153">Additional notes</span></span>

- <span data-ttu-id="2602f-154">Quando si usa un callback di ricompilare un elemento della cache:</span><span class="sxs-lookup"><span data-stu-id="2602f-154">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="2602f-155">Più richieste possono trovare vuoto il valore della chiave memorizzata nella cache in quanto il callback non è completata.</span><span class="sxs-lookup"><span data-stu-id="2602f-155">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="2602f-156">Ciò può comportare vari thread ripopolare l'elemento memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="2602f-156">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="2602f-157">Quando una voce di cache viene utilizzata per creare un altro, l'elemento figlio copia la voce padre i token di scadenza e le impostazioni di scadenza basati sul tempo.</span><span class="sxs-lookup"><span data-stu-id="2602f-157">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="2602f-158">L'elemento figlio non scadute dalla rimozione manuale o l'aggiornamento della voce padre.</span><span class="sxs-lookup"><span data-stu-id="2602f-158">The child isn't expired by manual removal or updating of the parent entry.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2602f-159">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2602f-159">Additional resources</span></span>

* [<span data-ttu-id="2602f-160">Usare una cache distribuita</span><span class="sxs-lookup"><span data-stu-id="2602f-160">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="2602f-161">Rilevare le modifiche apportate con i token di modifica</span><span class="sxs-lookup"><span data-stu-id="2602f-161">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="2602f-162">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="2602f-162">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="2602f-163">Middleware di memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="2602f-163">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="2602f-164">Helper per tag di cache</span><span class="sxs-lookup"><span data-stu-id="2602f-164">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="2602f-165">Helper per tag di cache distribuita</span><span class="sxs-lookup"><span data-stu-id="2602f-165">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
