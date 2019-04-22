---
title: Memorizzare nella cache in memoria in ASP.NET Core
author: rick-anderson
description: Informazioni su come memorizzare i dati nella cache in memoria in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 6433df36023b79bc679186bee8b0a92371661dbe
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59425049"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Memorizzare nella cache in memoria in ASP.NET Core

Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), e [Steve Smith](https://ardalis.com/)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Nozioni di base di memorizzazione nella cache

La memorizzazione nella cache può migliorare significativamente le prestazioni e scalabilità di un'app, riducendo il lavoro necessario per generare il contenuto. La memorizzazione nella cache funziona meglio con i dati che vengono modificati raramente. La memorizzazione nella cache esegue una copia di dati che possono essere restituiti molto più veloce dall'origine dati originale. Le app devono essere scritte e testate per **mai** dipendono dai dati memorizzati nella cache.

ASP.NET Core supporta diverse cache diverse. La cache più semplice si basa sulla [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), che rappresenta una cache archiviata nella memoria del server web. Le app eseguite in una server farm di più server è necessario assicurarsi che le sessioni sono permanenti quando si usa la cache in memoria. Sessioni permanenti assicurarsi che le successive richieste da un client tutte andare allo stesso server. Ad esempio, uso di App Web di Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) per indirizzare tutte le richieste successive nello stesso server.

Richiedono sessioni permanenti con non in una web farm un' [cache distribuita](distributed.md) per evitare problemi di coerenza della cache. Per alcune App, una cache distribuita può supportare l'aumento maggiore rispetto a un'istanza di cache in memoria. Usando una cache distribuita ripartisce la memoria cache a un processo esterno.

::: moniker range="< aspnetcore-2.0"

Il `IMemoryCache` cache rimuoverà le voci della cache eccessivo della memoria, a meno che il [memorizza nella cache priorità](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) è impostata su `CacheItemPriority.NeverRemove`. È possibile impostare il `CacheItemPriority` per modificare la priorità con cui la cache rimuove gli elementi sotto pressione della memoria.

::: moniker-end

La cache in memoria può archiviare qualsiasi oggetto. l'interfaccia di cache distribuita è limitata a `byte[]`. Gli elementi di cache di archivio della cache in memoria e distribuita come coppie chiave-valore.

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([Mobileengagement](https://www.nuget.org/packages/System.Runtime.Caching/)) può essere usato con:

* .NET standard 2.0 o versione successiva.
* Eventuali [implementazione di .NET](/dotnet/standard/net-standard#net-implementation-support) che ha come destinazione .NET Standard 2.0 o versione successiva. Ad esempio, ASP.NET Core 2.0 o versione successiva.
* .NET framework 4.5 o versione successiva.

[Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (descritto in questo articolo) è consigliato rispetto `System.Runtime.Caching` / `MemoryCache` perché è meglio integrato in ASP.NET Core. Ad esempio, `IMemoryCache` funziona in modo nativo con ASP.NET Core [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

Uso `System.Runtime.Caching` / `MemoryCache` come un bridge di compatibilità durante il porting del codice da ASP.NET 4.x ad ASP.NET Core.

## <a name="cache-guidelines"></a>Linee guida per la cache

* Codice deve essere associato sempre un'opzione di fallback per recuperare i dati e **non** dipendono dal valore memorizzato nella cache siano disponibili.
* La cache utilizza una risorsa limitata, della memoria. Limitare l'aumento delle dimensioni della cache:
  * Effettuare **non** usare input esterno come chiavi della cache.
  * Usare le scadenze per limitare l'aumento delle dimensioni della cache.
  * [Usare SetSize, dimensioni e SizeLimit per limitare le dimensioni della cache](#use-setsize-size-and-sizelimit-to-limit-cache-size). Il runtime di ASP.NET Core non limita le dimensioni di cache basata su richieste di memoria. Spetta allo sviluppatore per limitare le dimensioni della cache.

## <a name="using-imemorycache"></a>Usando IMemoryCache

Memorizzazione nella cache in memoria è una *assistenza* cui viene fatto riferimento dalla tua app usando [inserimento delle dipendenze](../../fundamentals/dependency-injection.md). Chiamare `AddMemoryCache` in `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Richiedere il `IMemoryCache` istanza nel costruttore:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` richiede il pacchetto NuGet [Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` richiede il pacchetto NuGet [Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), che è disponibile nel [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` richiede il pacchetto NuGet [Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), che è disponibile nel [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app).

::: moniker-end

Il codice seguente usa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) per verificare se è presente una volta nella cache. Se non viene memorizzato nella cache una volta, una nuova voce viene creata e aggiunto alla cache con [impostare](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Vengono visualizzati l'ora corrente e l'ora memorizzati nella cache:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Memorizzato nella cache `DateTime` valore rimane nella cache anche se esistono richieste nel periodo di timeout. L'immagine seguente mostra l'ora corrente e un'ora precedente recuperato dalla cache:

![Vista Index con due diverse volte visualizzato](memory/_static/time.png)

Il codice seguente usa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) per memorizzare i dati.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Il codice seguente chiama [ottenere](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) per recuperare l'ora memorizzati nella cache:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, e [ottenere](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) fanno parte di metodi di estensione del [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) classe che estende le funzionalità di <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Visualizzare [metodi IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) e [CacheExtensions metodi](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) per una descrizione degli altri metodi della cache.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

L'esempio seguente:

* Imposta l'ora di scadenza assoluta. Questo è il tempo massimo che può essere memorizzato nella cache la voce e impedisce la voce diventi obsoleta quando la scadenza variabile in modo continuo viene rinnovata.
* Imposta una scadenza variabile. Le richieste che accedono a questo elemento memorizzato nella cache verranno reimpostato il clock di scadenza scorrevole.
* Imposta la priorità della cache su `CacheItemPriority.NeverRemove`.
* Imposta una [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) che verrà chiamato dopo la voce viene rimosso dalla cache. Il callback viene eseguito su un thread diverso dal codice che rimuove l'elemento dalla cache.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Usare SetSize, dimensioni e SizeLimit per limitare le dimensioni della cache

Oggetto `MemoryCache` istanza può facoltativamente specificare e applicare un limite di dimensione. Il limite delle dimensioni di memoria non è un'unità di misura definita perché la cache è disponibile alcun meccanismo per misurare le dimensioni delle voci. Se la dimensione massima di memoria cache è impostata, tutte le voci devono specificare dimensioni. Il runtime di ASP.NET Core non limita le dimensioni di cache basata su richieste di memoria. Spetta allo sviluppatore per limitare le dimensioni della cache. La dimensione specificata è lo sviluppatore decide di unità.

Ad esempio:

* Se l'app web è stata principalmente la memorizzazione nella cache le stringhe, ciascuna dimensione della voce della cache potrebbe essere la lunghezza della stringa.
* L'app è stato possibile specificare la dimensione di tutte le voci come 1 e la dimensione massima è il numero di voci.

Il codice seguente crea un valore a dimensione fissa [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessibile dal [inserimento delle dipendenze](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) non dispone di unità. Le voci memorizzate nella cache è necessario specificare le dimensioni in qualsiasi unità che ritengono più appropriati se le dimensioni della memoria cache sono stata impostata. Tutti gli utenti di un'istanza di cache devono usare lo stesso sistema di unità. Una voce non verrà memorizzata se la somma delle dimensioni della voce memorizzata nella cache supera il valore specificato da `SizeLimit`. Se non è impostato alcun limite di dimensione della cache, le dimensioni della cache impostato sulla voce verranno ignorata.

Nell'esempio di codice registri `MyMemoryCache` con il [inserimento delle dipendenze](xref:fundamentals/dependency-injection) contenitore.

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache` viene creata come una cache di memoria indipendenti per i componenti che sono a conoscenza di questa cache di dimensioni limitate e sapere come impostare le dimensioni di voce della cache in modo appropriato.

Il codice seguente usa `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Le dimensioni della voce della cache possono essere impostate tramite [dimensioni](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) o nella [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) metodo di estensione:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>Dipendenze della cache

L'esempio seguente viene illustrato come la scadenza di una voce della cache se una voce di dipendente scade. Oggetto `CancellationChangeToken` viene aggiunto all'elemento memorizzato nella cache. Quando `Cancel` viene chiamato sul `CancellationTokenSource`, entrambe le voci della cache vengono rimossi.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Usando un `CancellationTokenSource` consente a più voci della cache essere eliminata come un gruppo. Con il `using` motivo nel codice precedente, le voci della cache creata all'interno di `using` blocco erediterà i trigger e le impostazioni di scadenza.

## <a name="additional-notes"></a>Note aggiuntive

* Quando si usa un callback per ripopolare un elemento della cache:

  * Più richieste possono trovare vuoto il valore della chiave memorizzata nella cache perché non è stato completato il callback.
  * Ciò può comportare diversi thread ripopolamento l'elemento memorizzato nella cache.

* Quando una voce della cache viene usata per creare un altro, l'elemento figlio copia token scadenza della voce padre e le impostazioni di scadenza basati sul tempo. L'elemento figlio non è scaduto per la rimozione manuale o l'aggiornamento della voce padre.

* Uso [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) per impostare i callback che verranno generati dopo che la voce della cache viene rimosso dalla cache.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
