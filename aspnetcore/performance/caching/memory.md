---
title: Memorizzare nella cache in memoria ASP.NET Core
author: rick-anderson
description: Informazioni su come memorizzare i dati nella cache in memoria in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 8/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 474f71225371ea89b023ee077d4ecc03e9751681
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030321"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Memorizzare nella cache in memoria ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo)e [Steve Smith](https://ardalis.com/)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Nozioni fondamentali sulla memorizzazione nella cache

La memorizzazione nella cache può migliorare significativamente le prestazioni e la scalabilità di un'app riducendo il lavoro necessario per generare il contenuto. La memorizzazione nella cache funziona meglio con i dati che cambiano raramente. La memorizzazione nella cache crea una copia dei dati che possono essere restituiti molto più velocemente rispetto a quelli dell'origine originale. Le app devono essere scritte e testate in modo da non dipendere **mai** dai dati memorizzati nella cache.

ASP.NET Core supporta diverse cache. La cache più semplice si basa su [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), che rappresenta una cache archiviata nella memoria del server Web. Le app eseguite in un server farm di più server devono garantire che le sessioni siano permanenti quando si usa la cache in memoria. Le sessioni permanenti garantiscono che le richieste successive provenienti da un client vadano tutte allo stesso server. Ad esempio, le app Web di Azure usano [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (arr) per indirizzare tutte le richieste successive allo stesso server.

Le sessioni non permanenti in una Web farm richiedono una [cache distribuita](distributed.md) per evitare problemi di coerenza della cache. Per alcune app, una cache distribuita può supportare una maggiore scalabilità orizzontale rispetto a una cache in memoria. L'uso di una cache distribuita trasferisce la memoria cache a un processo esterno.

::: moniker range="< aspnetcore-2.0"

La `IMemoryCache` cache eliminerà le voci della cache in condizioni di memoria insufficiente, a meno `CacheItemPriority.NeverRemove`che la priorità della [cache](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) non sia impostata su. È possibile impostare `CacheItemPriority` per regolare la priorità con cui la cache Elimina gli elementi in condizioni di memoria insufficiente.

::: moniker-end

La cache in memoria può archiviare qualsiasi oggetto; l'interfaccia della cache distribuita è `byte[]`limitata a. Gli elementi memorizzati nella cache in memoria e nella cache distribuita vengono archiviati come coppie chiave-valore.

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([Pacchetto NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) può essere usato con:

* .NET Standard 2,0 o versione successiva.
* Qualsiasi [implementazione di .NET](/dotnet/standard/net-standard#net-implementation-support) destinata a .NET standard 2,0 o versione successiva. Ad esempio, ASP.NET Core 2,0 o versione successiva.
* .NET Framework 4,5 o versione successiva.

È consigliabile usare [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (descritto in questo articolo), anziché `System.Runtime.Caching`/`MemoryCache`, perché è più integrato in ASP.NET Core. Ad esempio, `IMemoryCache` funziona in modo nativo con ASP.NET Core [inserimento](xref:fundamentals/dependency-injection)delle dipendenze.

Usare `System.Runtime.Caching` comeBridgedi`MemoryCache` compatibilità quando si porta il codice da ASP.NET 4. x a ASP.NET Core. /

## <a name="cache-guidelines"></a>Linee guida per la cache

* Il codice deve sempre avere un'opzione di fallback per recuperare i dati e **non** dipendere da un valore memorizzato nella cache disponibile.
* La cache usa una risorsa scarsa, ovvero la memoria. Limita aumento dimensioni cache:
  * Non usare input esterno come chiavi di cache.
  * Usare le scadenze per limitare la crescita della cache.
  * [Per limitare le dimensioni della cache, usare le dimensioni, le dimensioni e il valore di SizeLimit](#use-setsize-size-and-sizelimit-to-limit-cache-size). Il runtime di ASP.NET Core non limita le dimensioni della cache in base all'utilizzo eccessivo della memoria. Spetta allo sviluppatore limitare le dimensioni della cache.

## <a name="using-imemorycache"></a>Uso di IMemoryCache

> [!WARNING]
> L'uso di una cache *Shared* Memory dall'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) e della chiamata di `SetSize`, `Size` o `SizeLimit` per limitare le dimensioni della cache può causare un errore dell'app. Quando si imposta un limite di dimensioni in una cache, è necessario che tutte le voci specifichino una dimensione al momento dell'aggiunta. Questo può causare problemi perché gli sviluppatori potrebbero non avere il controllo completo su ciò che usa la cache condivisa. Ad esempio, Entity Framework Core utilizza la cache condivisa e non specifica una dimensione. Se un'app imposta un limite per le dimensioni della cache e USA EF Core, l' `InvalidOperationException`app genera un'eccezione.
> Quando si `SetSize`USA `Size`, o `SizeLimit` per limitare la cache, creare un singleton della cache per la memorizzazione nella cache. Per altre informazioni e per un esempio, vedere [use sesize, Size e SizeLimit per limitare le dimensioni della cache](#use-setsize-size-and-sizelimit-to-limit-cache-size).

La memorizzazione nella cache in memoria è un *servizio* a cui viene fatto riferimento dall'app mediante l' [inserimento](xref:fundamentals/dependency-injection)di dipendenze. Chiama `AddMemoryCache` in `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Richiedere l' `IMemoryCache` istanza nel costruttore:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache`richiede il pacchetto NuGet [Microsoft. Extensions. Caching. memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache`richiede il pacchetto NuGet [Microsoft. Extensions. Caching. memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), disponibile nel metapacchetto [Microsoft. AspNetCore. All](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache`richiede il pacchetto NuGet [Microsoft. Extensions. Caching. memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), disponibile nel metapacchetto [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

::: moniker-end

Il codice seguente usa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) per verificare se un'ora è nella cache. Se l'ora non viene memorizzata nella cache, viene creata una nuova voce che viene aggiunta alla cache con [set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Vengono visualizzati l'ora corrente e l'ora memorizzata nella cache:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Il valore `DateTime` memorizzato nella cache rimane nella cache mentre sono presenti richieste entro il periodo di timeout. La figura seguente mostra l'ora corrente e un tempo precedente recuperato dalla cache:

![Visualizzazione indice con due ore diverse visualizzate](memory/_static/time.png)

Il codice seguente usa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) per memorizzare i dati nella cache.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Il codice seguente chiama [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) per recuperare l'ora memorizzata nella cache:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>e [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) sono metodi di estensione parte della classe [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) che estende la funzionalità di <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Vedere [Metodi IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) e [Metodi CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) per una descrizione di altri metodi della cache.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

L'esempio seguente:

* Imposta l'ora di scadenza assoluta. Questo è il tempo massimo in cui la voce può essere memorizzata nella cache e impedisce che l'elemento diventi troppo obsoleto quando la scadenza variabile viene rinnovata continuamente.
* Imposta una data di scadenza variabile. Le richieste che accedono a questo elemento memorizzato nella cache reimpostano il clock di scadenza variabile.
* Imposta la priorità della cache `CacheItemPriority.NeverRemove`su.
* Imposta un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) che verrà chiamato dopo la rimozione della voce dalla cache. Il callback viene eseguito su un thread diverso dal codice che rimuove l'elemento dalla cache.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Usare le dimensioni di ridimensionamento, dimensioni e SizeLimit per limitare le dimensioni della cache

Un' `MemoryCache` istanza può facoltativamente specificare e applicare un limite di dimensioni. Il limite per le dimensioni della memoria non dispone di un'unità di misura definita perché la cache non ha alcun meccanismo per misurare la dimensione delle voci. Se il limite delle dimensioni della memoria cache è impostato, tutte le voci devono specificare le dimensioni. Il runtime di ASP.NET Core non limita le dimensioni della cache in base all'utilizzo eccessivo della memoria. Spetta allo sviluppatore limitare le dimensioni della cache. Le dimensioni specificate sono in unità scelte dallo sviluppatore.

Ad esempio:

* Se l'app Web è stata utilizzata principalmente per la memorizzazione nella cache delle stringhe, le dimensioni della voce della cache potrebbero essere la lunghezza della stringa.
* L'app può specificare le dimensioni di tutte le voci come 1 e il limite di dimensioni è il numero di voci.

Il codice seguente crea un elemento [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) a dimensione fissa a dimensione fissa accessibile dall' [inserimento](xref:fundamentals/dependency-injection)delle dipendenze:

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) non dispone di unità. Le voci memorizzate nella cache devono specificare le dimensioni in base alle unità ritenute più appropriate se le dimensioni della memoria della cache sono state impostate. Tutti gli utenti di un'istanza della cache devono usare lo stesso sistema di unità. Una voce non verrà memorizzata nella cache se la somma delle dimensioni della voce memorizzata nella cache `SizeLimit`supera il valore specificato da. Se non è impostato alcun limite per le dimensioni della cache, le dimensioni della cache impostate per la voce verranno ignorate.

Il codice seguente esegue `MyMemoryCache` la registrazione con il contenitore di [inserimento](xref:fundamentals/dependency-injection) delle dipendenze.

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache`viene creato come cache di memoria indipendente per i componenti che sono consapevoli di questa dimensione limitata della cache e sanno come impostare la dimensione della voce della cache in modo appropriato.

Il codice seguente usa `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

La dimensione della voce della cache può essere impostata in base a [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) o al metodo di estensione [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_)

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>Dipendenze della cache

Nell'esempio seguente viene illustrato come impostare come scaduti una voce della cache in caso di scadenza di una voce dipendente. Viene `CancellationChangeToken` aggiunto un oggetto all'elemento memorizzato nella cache. Quando `Cancel` viene chiamato su, `CancellationTokenSource`entrambe le voci della cache vengono eliminate.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

L'uso `CancellationTokenSource` di un oggetto consente di rimuovere più voci della cache come gruppo. Con il `using` modello nel codice precedente, le voci della cache create all' `using` interno del blocco erediteranno i trigger e le impostazioni di scadenza.

## <a name="additional-notes"></a>Note aggiuntive

* Quando si usa un callback per ripopolare un elemento della cache:

  * Più richieste possono trovare il valore della chiave memorizzato nella cache perché il callback non è stato completato.
  * Questo può comportare la ricompilazione dell'elemento memorizzato nella cache da parte di più thread.

* Quando viene utilizzata una voce della cache per crearne un'altra, l'elemento figlio copia i token di scadenza della voce padre e le impostazioni di scadenza basate sul tempo. L'elemento figlio non è scaduto dalla rimozione manuale o dall'aggiornamento della voce padre.

* Utilizzare [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) per impostare i callback che verranno generati dopo la rimozione della voce della cache dalla cache.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
