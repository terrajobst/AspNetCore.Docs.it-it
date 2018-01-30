---
title: Memorizzazione nella cache in ASP.NET Core
author: rick-anderson
description: Imparare a memorizzare nella cache i dati in memoria in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: 4219cae4e3d3f9d15afe6725b21cc8966979d95c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="in-memory-caching-in-aspnet-core"></a>Memorizzazione nella cache in ASP.NET Core

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), e [Steve Smith](https://ardalis.com/)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Nozioni fondamentali sulla memorizzazione nella cache

La memorizzazione nella cache può migliorare significativamente le prestazioni e la scalabilità di un'app, riducendo il lavoro necessario per generare il contenuto. Memorizzazione nella cache funziona meglio con i dati vengono modificati raramente. La memorizzazione nella cache crea una copia di dati che possono essere restituite molto più veloce dall'origine dati originale. È necessario scrivere e testare l'app per mai dipendono dai dati memorizzati nella cache.

ASP.NET Core supporta diverse cache diverse. Dipende dalla cache più semplice la [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), che rappresenta una cache archiviata nella memoria del server web. Le app che vengono eseguiti in una server farm di più server è necessario assicurarsi che le sessioni sono permanenti quando si utilizza la cache in memoria. Le sessioni permanenti garantire che le successive richieste da un client tutti nello stesso server. Ad esempio, uso di App Web di Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) per il routing di tutte le richieste successive nello stesso server.

Le sessioni permanenti con non in una web farm richiedono un [cache distribuita](distributed.md) per evitare problemi di coerenza della cache. Per alcune App, una cache distribuita può supportare maggiore scalabilità rispetto a una cache in memoria. Una cache distribuita consente di scaricare la memoria cache per un processo esterno. 

Il `IMemoryCache` cache rimuove le voci della cache in memoria, a meno che il [cache priorità](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) è impostato su `CacheItemPriority.NeverRemove`. È possibile impostare il `CacheItemPriority` per regolare la priorità della cache rimuove gli elementi eccessivo della memoria.

La cache in memoria è possibile archiviare qualsiasi oggetto. l'interfaccia cache distribuita è limitato a `byte[]`.

## <a name="using-imemorycache"></a>Utilizzo delle interfacce IMemoryCache

Memorizzazione nella cache in memoria è un *servizio* a cui fa riferimento da cui l'app utilizzando [Dependency Injection](../../fundamentals/dependency-injection.md). Chiamare `AddMemoryCache` in `ConfigureServices`:

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

Richiesta di `IMemoryCache` istanza nel costruttore:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-)] 

`IMemoryCache`richiede il pacchetto NuGet "Microsoft.Extensions.Caching.Memory".

Il codice seguente usa [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) per verificare se l'ora corrente è nella cache. Se l'elemento non è memorizzato nella cache, una nuova voce viene creata e aggiunto alla cache con [impostare](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Viene visualizzata l'ora corrente e quella memorizzata nella cache:

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Memorizzato nella cache `DateTime` valore rimane nella cache mentre vi sono richieste entro il periodo di timeout (e nessuna rimozione a causa di memoria). L'immagine seguente mostra l'ora corrente e un'ora precedente recuperato dalla cache:

![Visualizzazione dell'indice con due diverse volte visualizzato](memory/_static/time.png)

Il codice seguente usa [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) per memorizzare i dati. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Il codice seguente chiama [ottenere](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) per recuperare l'ora memorizzati nella cache:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Vedere [metodi IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) e [metodi CacheExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) per una descrizione dei metodi della cache.

## <a name="using-memorycacheentryoptions"></a>Utilizzando MemoryCacheEntryOptions

L'esempio seguente:

- Imposta l'ora di scadenza assoluta. Questo è il tempo massimo che può essere memorizzato nella cache la voce e impedisce l'elemento di diventare obsoleti quando la scadenza variabile è costantemente rinnovata.
- Imposta una scadenza variabile. Le richieste che accedono a questo elemento memorizzato nella cache verranno reimpostato la scadenza variabile.
- Imposta la priorità di cache su `CacheItemPriority.NeverRemove`. 
- Imposta un [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) che verrà chiamato dopo la voce viene rimossa dalla cache. Il callback viene eseguito su un thread diverso dal codice che rimuove l'elemento dalla cache.

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>Dipendenze della cache

L'esempio seguente viene illustrato come la scadenza di una voce della cache se la scadenza di una voce di dipendenti. Oggetto `CancellationChangeToken` viene aggiunto all'elemento memorizzato nella cache. Quando `Cancel` viene chiamato sul `CancellationTokenSource`, entrambe le voci della cache vengono rimossi. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Utilizzando un `CancellationTokenSource` consente più voci nella cache da rimuovere come gruppo. Con il `using` modello nel codice precedente, le voci della cache creato all'interno di `using` blocco erediterà le impostazioni di scadenza e trigger.

## <a name="additional-notes"></a>Note aggiuntive

- Quando si usa un callback di ricompilare un elemento della cache:

  - Più richieste possono trovare vuoto il valore della chiave memorizzata nella cache in quanto il callback non è completata. 
  - Questo può comportare vari thread ripopolare l'elemento memorizzato nella cache.

- Quando una voce di cache viene utilizzata per creare un altro, l'elemento figlio copia i token di scadenza della voce padre e le impostazioni di scadenza basati sul tempo. L'elemento figlio non è scaduto per la rimozione manuale o l'aggiornamento della voce padre.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Uso di una cache distribuita](xref:performance/caching/distributed)
* [Rilevare le modifiche apportate con i token di modifica](xref:fundamentals/primitives/change-tokens)
* [Memorizzazione nella cache delle risposte](xref:performance/caching/response)
* [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
* [Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
