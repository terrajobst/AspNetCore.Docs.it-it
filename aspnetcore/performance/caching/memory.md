---
title: Memorizzare nella cache in memoria in ASP.NET Core
author: rick-anderson
description: Informazioni su come memorizzare i dati nella cache in memoria in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2018
uid: performance/caching/memory
ms.openlocfilehash: f0d5bb74985b6ce0da7d4c5b69e31b8d2bbb5105
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090046"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="6391d-103">Memorizzare nella cache in memoria in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6391d-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="6391d-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6391d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6391d-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6391d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="6391d-106">Nozioni di base di memorizzazione nella cache</span><span class="sxs-lookup"><span data-stu-id="6391d-106">Caching basics</span></span>

<span data-ttu-id="6391d-107">La memorizzazione nella cache può migliorare significativamente le prestazioni e scalabilità di un'app, riducendo il lavoro necessario per generare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="6391d-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="6391d-108">La memorizzazione nella cache funziona meglio con i dati che vengono modificati raramente.</span><span class="sxs-lookup"><span data-stu-id="6391d-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="6391d-109">La memorizzazione nella cache esegue una copia di dati che possono essere restituiti molto più veloce dall'origine dati originale.</span><span class="sxs-lookup"><span data-stu-id="6391d-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="6391d-110">È necessario scrivere e testare l'app per non dipendere mai dati memorizzati nella cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="6391d-111">ASP.NET Core supporta diverse cache diverse.</span><span class="sxs-lookup"><span data-stu-id="6391d-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="6391d-112">La cache più semplice si basa sulla [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), che rappresenta una cache archiviata nella memoria del server web.</span><span class="sxs-lookup"><span data-stu-id="6391d-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="6391d-113">Le app eseguite in una server farm di più server è necessario assicurarsi che le sessioni sono permanenti quando si usa la cache in memoria.</span><span class="sxs-lookup"><span data-stu-id="6391d-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="6391d-114">Sessioni permanenti assicurarsi che le successive richieste da un client tutte andare allo stesso server.</span><span class="sxs-lookup"><span data-stu-id="6391d-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="6391d-115">Ad esempio, uso di App Web di Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) per indirizzare tutte le richieste successive nello stesso server.</span><span class="sxs-lookup"><span data-stu-id="6391d-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="6391d-116">Richiedono sessioni permanenti con non in una web farm un' [cache distribuita](distributed.md) per evitare problemi di coerenza della cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="6391d-117">Per alcune App, una cache distribuita può supportare l'aumento maggiore rispetto a un'istanza di cache in memoria.</span><span class="sxs-lookup"><span data-stu-id="6391d-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="6391d-118">Usando una cache distribuita ripartisce la memoria cache a un processo esterno.</span><span class="sxs-lookup"><span data-stu-id="6391d-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6391d-119">Il `IMemoryCache` cache rimuoverà le voci della cache eccessivo della memoria, a meno che il [memorizza nella cache priorità](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) è impostata su `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="6391d-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="6391d-120">È possibile impostare il `CacheItemPriority` per modificare la priorità con cui la cache rimuove gli elementi sotto pressione della memoria.</span><span class="sxs-lookup"><span data-stu-id="6391d-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

::: moniker-end

<span data-ttu-id="6391d-121">La cache in memoria può archiviare qualsiasi oggetto. l'interfaccia di cache distribuita è limitata a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="6391d-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="6391d-122">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="6391d-122">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="6391d-123"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([Mobileengagement](https://www.nuget.org/packages/System.Runtime.Caching/)) può essere usato con:</span><span class="sxs-lookup"><span data-stu-id="6391d-123"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="6391d-124">.NET standard 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="6391d-124">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="6391d-125">Eventuali [implementazione di .NET](/dotnet/standard/net-standard#net-implementation-support) che ha come destinazione .NET Standard 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="6391d-125">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="6391d-126">Ad esempio, ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="6391d-126">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="6391d-127">.NET framework 4.5 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="6391d-127">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="6391d-128">[Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (descritte in questo argomento) è consigliato rispetto `System.Runtime.Caching` / `MemoryCache` perché è meglio integrato in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6391d-128">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this topic) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="6391d-129">Ad esempio, `IMemoryCache` funziona in modo nativo con ASP.NET Core [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6391d-129">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="6391d-130">Uso `System.Runtime.Caching` / `MemoryCache` come un bridge di compatibilità durante il porting del codice da ASP.NET 4.x ad ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6391d-130">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="6391d-131">Linee guida per la cache</span><span class="sxs-lookup"><span data-stu-id="6391d-131">Cache guidelines</span></span>

* <span data-ttu-id="6391d-132">Codice deve essere associato sempre un'opzione di fallback per recuperare i dati e **non** dipendono dal valore memorizzato nella cache siano disponibili.</span><span class="sxs-lookup"><span data-stu-id="6391d-132">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="6391d-133">La cache utilizza una risorsa limitata, della memoria.</span><span class="sxs-lookup"><span data-stu-id="6391d-133">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="6391d-134">Limitare l'aumento delle dimensioni della cache:</span><span class="sxs-lookup"><span data-stu-id="6391d-134">Limit cache growth:</span></span>
  * <span data-ttu-id="6391d-135">Effettuare **non** usare input esterno come chiavi della cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-135">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="6391d-136">Usare le scadenze per limitare l'aumento delle dimensioni della cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-136">Use expirations to limit cache growth.</span></span>
  * [<span data-ttu-id="6391d-137">Usare SetSize, dimensioni e SizeLimit per limitare le dimensioni della cache</span><span class="sxs-lookup"><span data-stu-id="6391d-137">Use SetSize, Size, and SizeLimit to limit cache size</span></span>](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a><span data-ttu-id="6391d-138">Usando IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="6391d-138">Using IMemoryCache</span></span>

<span data-ttu-id="6391d-139">Memorizzazione nella cache in memoria è una *assistenza* cui viene fatto riferimento dalla tua app usando [inserimento delle dipendenze](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="6391d-139">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="6391d-140">Chiamare `AddMemoryCache` in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6391d-140">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="6391d-141">Richiedere il `IMemoryCache` istanza nel costruttore:</span><span class="sxs-lookup"><span data-stu-id="6391d-141">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6391d-142">`IMemoryCache` richiede il pacchetto NuGet [Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="6391d-142">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6391d-143">`IMemoryCache` richiede il pacchetto NuGet [Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), che è disponibile nel [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="6391d-143">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="6391d-144">`IMemoryCache` richiede il pacchetto NuGet [Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), che è disponibile nel [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6391d-144">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="6391d-145">Il codice seguente usa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) per verificare se è presente una volta nella cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-145">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="6391d-146">Se non viene memorizzato nella cache una volta, una nuova voce viene creata e aggiunto alla cache con [impostare](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="6391d-146">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="6391d-147">Vengono visualizzati l'ora corrente e l'ora memorizzati nella cache:</span><span class="sxs-lookup"><span data-stu-id="6391d-147">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="6391d-148">Memorizzato nella cache `DateTime` valore rimane nella cache anche se esistono richieste entro il periodo di timeout (e nessuna rimozione dovuta a pressione della memoria).</span><span class="sxs-lookup"><span data-stu-id="6391d-148">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="6391d-149">L'immagine seguente mostra l'ora corrente e un'ora precedente recuperato dalla cache:</span><span class="sxs-lookup"><span data-stu-id="6391d-149">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Vista Index con due diverse volte visualizzato](memory/_static/time.png)

<span data-ttu-id="6391d-151">Il codice seguente usa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) per memorizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="6391d-151">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="6391d-152">Il codice seguente chiama [ottenere](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) per recuperare l'ora memorizzati nella cache:</span><span class="sxs-lookup"><span data-stu-id="6391d-152">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="6391d-153">Visualizzare [metodi IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) e [CacheExtensions metodi](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) per una descrizione dei metodi della cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-153">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="6391d-154">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="6391d-154">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="6391d-155">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="6391d-155">The following sample:</span></span>

- <span data-ttu-id="6391d-156">Imposta l'ora di scadenza assoluta.</span><span class="sxs-lookup"><span data-stu-id="6391d-156">Sets the absolute expiration time.</span></span> <span data-ttu-id="6391d-157">Questo è il tempo massimo che può essere memorizzato nella cache la voce e impedisce la voce diventi obsoleta quando la scadenza variabile in modo continuo viene rinnovata.</span><span class="sxs-lookup"><span data-stu-id="6391d-157">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="6391d-158">Imposta una scadenza variabile.</span><span class="sxs-lookup"><span data-stu-id="6391d-158">Sets a sliding expiration time.</span></span> <span data-ttu-id="6391d-159">Le richieste che accedono a questo elemento memorizzato nella cache verranno reimpostato il clock di scadenza scorrevole.</span><span class="sxs-lookup"><span data-stu-id="6391d-159">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="6391d-160">Imposta la priorità della cache su `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="6391d-160">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="6391d-161">Imposta una [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) che verrà chiamato dopo la voce viene rimosso dalla cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-161">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="6391d-162">Il callback viene eseguito su un thread diverso dal codice che rimuove l'elemento dalla cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-162">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="6391d-163">Usare SetSize, dimensioni e SizeLimit per limitare le dimensioni della cache</span><span class="sxs-lookup"><span data-stu-id="6391d-163">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="6391d-164">Oggetto `MemoryCache` istanza può facoltativamente specificare e applicare un limite di dimensione.</span><span class="sxs-lookup"><span data-stu-id="6391d-164">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="6391d-165">Il limite delle dimensioni di memoria non è un'unità di misura definita perché la cache è disponibile alcun meccanismo per misurare le dimensioni delle voci.</span><span class="sxs-lookup"><span data-stu-id="6391d-165">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="6391d-166">Se la dimensione massima di memoria cache è impostata, tutte le voci devono specificare dimensioni.</span><span class="sxs-lookup"><span data-stu-id="6391d-166">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="6391d-167">La dimensione specificata è lo sviluppatore decide di unità.</span><span class="sxs-lookup"><span data-stu-id="6391d-167">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="6391d-168">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6391d-168">For example:</span></span>

* <span data-ttu-id="6391d-169">Se l'app web è stata principalmente la memorizzazione nella cache le stringhe, ciascuna dimensione della voce della cache potrebbe essere la lunghezza della stringa.</span><span class="sxs-lookup"><span data-stu-id="6391d-169">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="6391d-170">L'app è stato possibile specificare la dimensione di tutte le voci come 1 e la dimensione massima è il numero di voci.</span><span class="sxs-lookup"><span data-stu-id="6391d-170">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="6391d-171">Il codice seguente crea un valore a dimensione fissa [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessibile dal [inserimento delle dipendenze](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="6391d-171">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="6391d-172">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) non dispone di unità.</span><span class="sxs-lookup"><span data-stu-id="6391d-172">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="6391d-173">Le voci memorizzate nella cache è necessario specificare le dimensioni in qualsiasi unità che ritengono più appropriati se le dimensioni della memoria cache sono stata impostata.</span><span class="sxs-lookup"><span data-stu-id="6391d-173">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="6391d-174">Tutti gli utenti di un'istanza di cache devono usare lo stesso sistema di unità.</span><span class="sxs-lookup"><span data-stu-id="6391d-174">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="6391d-175">Una voce non verrà memorizzata se la somma delle dimensioni della voce memorizzata nella cache supera il valore specificato da `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="6391d-175">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="6391d-176">Se non è impostato alcun limite di dimensione della cache, le dimensioni della cache impostato sulla voce verranno ignorata.</span><span class="sxs-lookup"><span data-stu-id="6391d-176">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="6391d-177">Nell'esempio di codice registri `MyMemoryCache` con il [inserimento delle dipendenze](xref:fundamentals/dependency-injection) contenitore.</span><span class="sxs-lookup"><span data-stu-id="6391d-177">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="6391d-178">`MyMemoryCache` viene creata come una cache di memoria indipendenti per i componenti che sono a conoscenza di questa cache di dimensioni limitate e sapere come impostare le dimensioni di voce della cache in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="6391d-178">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="6391d-179">Il codice seguente usa `MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="6391d-179">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="6391d-180">Le dimensioni della voce della cache possono essere impostate tramite [dimensioni](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) o nella [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="6391d-180">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="6391d-181">Dipendenze della cache</span><span class="sxs-lookup"><span data-stu-id="6391d-181">Cache dependencies</span></span>

<span data-ttu-id="6391d-182">L'esempio seguente viene illustrato come la scadenza di una voce della cache se una voce di dipendente scade.</span><span class="sxs-lookup"><span data-stu-id="6391d-182">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="6391d-183">Oggetto `CancellationChangeToken` viene aggiunto all'elemento memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-183">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="6391d-184">Quando `Cancel` viene chiamato sul `CancellationTokenSource`, entrambe le voci della cache vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="6391d-184">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="6391d-185">Usando un `CancellationTokenSource` consente a più voci della cache essere eliminata come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="6391d-185">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="6391d-186">Con il `using` motivo nel codice precedente, le voci della cache creata all'interno di `using` blocco erediterà i trigger e le impostazioni di scadenza.</span><span class="sxs-lookup"><span data-stu-id="6391d-186">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="6391d-187">Note aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6391d-187">Additional notes</span></span>

- <span data-ttu-id="6391d-188">Quando si usa un callback per ripopolare un elemento della cache:</span><span class="sxs-lookup"><span data-stu-id="6391d-188">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="6391d-189">Più richieste possono trovare vuoto il valore della chiave memorizzata nella cache perché non è stato completato il callback.</span><span class="sxs-lookup"><span data-stu-id="6391d-189">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="6391d-190">Ciò può comportare diversi thread ripopolamento l'elemento memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-190">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="6391d-191">Quando una voce della cache viene usata per creare un altro, l'elemento figlio copia token scadenza della voce padre e le impostazioni di scadenza basati sul tempo.</span><span class="sxs-lookup"><span data-stu-id="6391d-191">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="6391d-192">L'elemento figlio non è scaduto per la rimozione manuale o l'aggiornamento della voce padre.</span><span class="sxs-lookup"><span data-stu-id="6391d-192">The child isn't expired by manual removal or updating of the parent entry.</span></span>

- <span data-ttu-id="6391d-193">Uso [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) per impostare i callback che verranno generati dopo che la voce della cache viene rimosso dalla cache.</span><span class="sxs-lookup"><span data-stu-id="6391d-193">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6391d-194">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6391d-194">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
