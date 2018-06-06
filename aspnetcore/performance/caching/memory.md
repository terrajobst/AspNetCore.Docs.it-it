---
title: Memorizzare nella cache in memoria in ASP.NET Core
author: rick-anderson
description: Imparare a memorizzare nella cache i dati in memoria in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: eca6610caf4e0a654c9a31f89a42e2ac82e94d23
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734484"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Memorizzare nella cache in memoria in ASP.NET Core

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), e [Steve Smith](https://ardalis.com/)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Nozioni fondamentali sulla memorizzazione nella cache

La memorizzazione nella cache può migliorare significativamente le prestazioni e la scalabilità di un'app, riducendo il lavoro necessario per generare il contenuto. Memorizzazione nella cache funziona meglio con i dati vengono modificati raramente. La memorizzazione nella cache crea una copia di dati che possono essere restituite molto più veloce dall'origine dati originale. È necessario scrivere e testare l'app per mai dipendono dai dati memorizzati nella cache.

ASP.NET Core supporta diverse cache diverse. Dipende dalla cache più semplice la [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), che rappresenta una cache archiviata nella memoria del server web. Le app che vengono eseguiti in una server farm di più server è necessario assicurarsi che le sessioni sono permanenti quando si utilizza la cache in memoria. Le sessioni permanenti garantire che le successive richieste da un client tutti nello stesso server. Ad esempio, uso di App Web di Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) per il routing di tutte le richieste successive nello stesso server.

Le sessioni permanenti con non in una web farm richiedono un [cache distribuita](distributed.md) per evitare problemi di coerenza della cache. Per alcune App, una cache distribuita può supportare maggiore scalabilità rispetto a una cache in memoria. Una cache distribuita consente di scaricare la memoria cache per un processo esterno.

Il `IMemoryCache` cache rimuove le voci della cache in memoria, a meno che il [cache priorità](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) è impostato su `CacheItemPriority.NeverRemove`. È possibile impostare il `CacheItemPriority` per regolare la priorità con cui la cache rimuove gli elementi in memoria.

La cache in memoria è possibile archiviare qualsiasi oggetto. l'interfaccia cache distribuita è limitato a `byte[]`.

## <a name="using-imemorycache"></a>Utilizzo delle interfacce IMemoryCache

Memorizzazione nella cache in memoria è un *servizio* a cui fa riferimento da cui l'app utilizzando [Dependency Injection](../../fundamentals/dependency-injection.md). Chiamare `AddMemoryCache` in `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Richiesta di `IMemoryCache` istanza nel costruttore:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` richiede il pacchetto NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` richiede il pacchetto NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), che è disponibile nel [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` richiede il pacchetto NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), che è disponibile nel [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

Il codice seguente usa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) per verificare se un'ora è nella cache. Se non è memorizzato nella cache di un'ora, una nuova voce viene creata e aggiunto alla cache con [impostare](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Vengono visualizzati l'ora corrente e quella memorizzata nella cache:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Memorizzato nella cache `DateTime` valore rimane nella cache mentre vi sono richieste entro il periodo di timeout (e nessuna rimozione a causa di memoria). La figura seguente mostra l'ora corrente e un'ora precedente recuperato dalla cache:

![Visualizzazione dell'indice con due diverse volte visualizzato](memory/_static/time.png)

Il codice seguente usa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) per memorizzare i dati. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Il codice seguente chiama [ottenere](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) per recuperare l'ora memorizzati nella cache:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Vedere [metodi IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) e [metodi CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) per una descrizione dei metodi della cache.

## <a name="using-memorycacheentryoptions"></a>Utilizzando MemoryCacheEntryOptions

L'esempio seguente:

- Imposta l'ora di scadenza assoluta. Questo è il tempo massimo che può essere memorizzato nella cache la voce e impedisce l'elemento di diventare obsoleti quando la scadenza variabile è costantemente rinnovata.
- Imposta una scadenza variabile. Le richieste che accedono a questo elemento memorizzato nella cache verranno reimpostato la scadenza variabile.
- Imposta la priorità di cache su `CacheItemPriority.NeverRemove`.
- Imposta un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) che verrà chiamato dopo la voce viene rimossa dalla cache. Il callback viene eseguito su un thread diverso dal codice che rimuove l'elemento dalla cache.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a>Dipendenze della cache

L'esempio seguente viene illustrato come la scadenza di una voce della cache se la scadenza di una voce di dipendenti. Oggetto `CancellationChangeToken` viene aggiunto all'elemento memorizzato nella cache. Quando `Cancel` viene chiamato sul `CancellationTokenSource`, entrambe le voci della cache vengono rimossi.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Utilizzando un `CancellationTokenSource` consente più voci nella cache da rimuovere come gruppo. Con il `using` modello nel codice precedente, le voci della cache creato all'interno di `using` blocco erediterà le impostazioni di scadenza e trigger.

## <a name="additional-notes"></a>Note aggiuntive

- Quando si usa un callback di ricompilare un elemento della cache:

  - Più richieste possono trovare vuoto il valore della chiave memorizzata nella cache in quanto il callback non è completata.
  - Questo può comportare vari thread ripopolare l'elemento memorizzato nella cache.

- Quando una voce di cache viene utilizzata per creare un altro, l'elemento figlio copia i token di scadenza della voce padre e le impostazioni di scadenza basati sul tempo. L'elemento figlio non è scaduto per la rimozione manuale o l'aggiornamento della voce padre.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Usare una cache distribuita](xref:performance/caching/distributed)
* [Rilevare le modifiche apportate con i token di modifica](xref:fundamentals/primitives/change-tokens)
* [Memorizzazione nella cache delle risposte](xref:performance/caching/response)
* [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
* [Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
