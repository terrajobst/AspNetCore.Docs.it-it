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
# <a name="cache-in-memory-in-aspnet-core"></a>Memorizzare nella cache in memoria in ASP.NET Core

Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), e [Steve Smith](https://ardalis.com/)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Nozioni fondamentali sulla memorizzazione nella cache

La memorizzazione nella cache può migliorare significativamente le prestazioni e scalabilità di un'app, riducendo il lavoro necessario per generare il contenuto. Memorizzazione nella cache funziona meglio con i dati che vengono modificati raramente. La memorizzazione nella cache crea una copia di dati che possono essere restituite molto più veloce rispetto dalla fonte originale. È necessario scrivere e testare l'app per mai dipendono dai dati memorizzati nella cache.

ASP.NET Core supporta diverse cache diverse. Si basa la cache più semplice la [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), che rappresenta una cache archiviata nella memoria del server web. Le app che vengono eseguiti in una server farm di più server è necessario assicurarsi che le sessioni sono permanenti quando si utilizza la cache in memoria. Le sessioni permanenti garantire che le successive richieste da un client tutti nello stesso server. Ad esempio, uso di App Web di Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) per il routing di tutte le richieste successive nello stesso server.

Le sessioni permanenti non in una web farm richiedono un [cache distribuita](distributed.md) per evitare problemi di coerenza della cache. Per alcune App, una cache distribuita può supportare una maggiore scalabilità orizzontale rispetto a una cache in memoria. Uso di una cache distribuita trasferisce il carico di memoria cache per un processo esterno.

Il `IMemoryCache` cache rimuove le voci della cache eccessivo della memoria, a meno che il [memorizza nella cache priorità](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) è impostata su `CacheItemPriority.NeverRemove`. È possibile impostare il `CacheItemPriority` per regolare la priorità con cui la cache rimuove gli elementi eccessivo della memoria.

La cache in memoria può memorizzare qualsiasi oggetto. l'interfaccia cache distribuita è limitato a `byte[]`.

## <a name="using-imemorycache"></a>Utilizzando IMemoryCache

La memorizzazione nella cache in memoria è un *servizio* che viene fatto riferimento dall'app utilizzando [inserimento di dipendenze](../../fundamentals/dependency-injection.md). Chiamare `AddMemoryCache` in `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Richiedere il `IMemoryCache` istanza nel costruttore:

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

Il codice seguente usa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) per verificare se un'ora è nella cache. Se non viene memorizzato nella cache di un'ora, una nuova voce viene creata e aggiunto alla cache con [impostare](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

L'ora corrente e l'ora memorizzati nella cache vengono visualizzati:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Memorizzato nella cache `DateTime` valore rimane nella cache mentre vi sono richieste entro il periodo di timeout (e nessuna rimozione dovuta a pressione della memoria). La figura seguente mostra l'ora corrente e un'ora precedente recuperato dalla cache:

![Visualizzazione dell'indice con due diverse volte visualizzato](memory/_static/time.png)

Il codice seguente usa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) per memorizzare i dati. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Il codice seguente chiama [ottenere](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) per recuperare l'ora memorizzati nella cache:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Vedere [metodi IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) e [CacheExtensions metodi](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) per una descrizione dei metodi della cache.

## <a name="using-memorycacheentryoptions"></a>Utilizzo MemoryCacheEntryOptions

L'esempio seguente:

- Imposta l'ora di scadenza assoluta. Questo è il tempo massimo che può essere memorizzati nella cache la voce e impedisce che l'elemento obsolescenza troppo quando la scadenza variabile viene rinnovata in modo continuo.
- Imposta una scadenza variabile. Le richieste che accedono a questo elemento memorizzato nella cache verranno reimpostato il clock di scadenza scorrevole.
- Imposta la priorità di cache su `CacheItemPriority.NeverRemove`.
- Imposta una [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) che verrà chiamato dopo la voce viene rimossa dalla cache. Il callback viene eseguito su un thread diverso dal codice che rimuove l'elemento dalla cache.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a>Dipendenze della cache

L'esempio seguente viene illustrato come la scadenza di una voce della cache se scade una voce di dipendenti. Oggetto `CancellationChangeToken` viene aggiunto all'elemento memorizzato nella cache. Quando si `Cancel` viene chiamato sul `CancellationTokenSource`, entrambe le voci della cache vengono rimossi.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Utilizzando un `CancellationTokenSource` consente a più voci nella cache da rimuovere come gruppo. Con il `using` motivo nel codice precedente, le voci della cache creato all'interno di `using` blocco erediterà i trigger e le impostazioni di scadenza.

## <a name="additional-notes"></a>Note aggiuntive

- Quando si usa un callback di ricompilare un elemento della cache:

  - Più richieste possono trovare vuoto il valore della chiave memorizzata nella cache in quanto il callback non è completata.
  - Ciò può comportare vari thread ripopolare l'elemento memorizzato nella cache.

- Quando una voce di cache viene utilizzata per creare un altro, l'elemento figlio copia la voce padre i token di scadenza e le impostazioni di scadenza basati sul tempo. L'elemento figlio non scadute dalla rimozione manuale o l'aggiornamento della voce padre.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Usare una cache distribuita](xref:performance/caching/distributed)
* [Rilevare le modifiche apportate con i token di modifica](xref:fundamentals/primitives/change-tokens)
* [Memorizzazione nella cache delle risposte](xref:performance/caching/response)
* [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
* [Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
