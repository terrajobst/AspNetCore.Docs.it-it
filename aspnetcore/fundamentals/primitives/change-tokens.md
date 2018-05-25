---
title: Rilevare le modifiche apportate con i token di modifica in ASP.NET Core
author: guardrex
description: Informazioni su come usare i token di modifica per rilevare le modifiche.
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 06751e713fbd579a944333cc3c3b2c0c0ad51eba
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Rilevare le modifiche apportate con i token di modifica in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Un *token di modifica* è un blocco predefinito di uso generico e di basso livello che viene usato per il rilevamento delle modifiche.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Interfaccia IChangeToken

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaga le notifiche relative a una modifica apportata. `IChangeToken` risiede nello spazio dei nomi [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives). Per le app che non usano il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), fare riferimento al pacchetto NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) nel file di progetto.

`IChangeToken` ha due proprietà:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indica se il token genera callback in modo proattivo. Se `ActiveChangedCallbacks` è impostato su `false`, non viene mai chiamato alcun callback e l'app deve eseguire il polling di `HasChanged` per ottenere le modifiche. È anche possibile che un token non venga mai annullato se non vengono apportate modifiche o se il listener di modifica sottostante viene eliminato o disabilitato.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) ottiene un valore che indica se è stata apportata una modifica.

L'interfaccia include un metodo, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), che registra un callback che viene richiamato quando il token è stato modificato. Prima che il callback venga richiamato è necessario impostare `HasChanged`.

## <a name="changetoken-class"></a>Classe ChangeToken

`ChangeToken` è una classe statica usata per propagare le notifiche relative a una modifica che è stata apportata. `ChangeToken` risiede nello spazio dei nomi [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives). Per le app che non usano il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), fare riferimento al pacchetto NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) nel file di progetto.

Il metodo `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) registra un elemento `Action` da chiamare quando il token viene modificato:
* `Func<IChangeToken>` produce il token.
* `Action` viene chiamato alla modifica del token.

`ChangeToken` ha un overload [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) che acquisisce un parametro `TState` aggiuntivo che viene passato nell'elemento `Action` consumer del token.

`OnChange` restituisce un elemento [IDisposable](/dotnet/api/system.idisposable). La chiamata a [Dispose](/dotnet/api/system.idisposable.dispose) interrompe l'ascolto di altre modifiche da parte del token e rilascia le risorse del token.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Esempi di uso di token di modifica in ASP.NET Core

I token di modifica vengono usati nelle aree principali di ASP.NET Core per monitorare le modifiche apportate agli oggetti:

* Per monitorare le modifiche ai file, il metodo [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) di [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) crea un elemento `IChangeToken` per le cartelle o i file specificati da controllare.
* È possibile aggiungere token `IChangeToken` alle voci della cache per attivare le eliminazioni dalla cache alla modifica.
* Per le modifiche `TOptions`, l'implementazione [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) predefinita di [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) ha un overload che accetta una o più istanze di [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1). Ogni istanza restituisce un elemento `IChangeToken` per registrare un callback di notifica delle modifiche per il rilevamento delle modifiche apportate alle opzioni.

## <a name="monitoring-for-configuration-changes"></a>Monitoraggio delle modifiche alla configurazione

Per impostazione predefinita, i modelli ASP.NET Core usano [file di configurazione JSON](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json* e *appsettings.Production.json*) per caricare le impostazioni di configurazione dell'app.

Questi file vengono configurati usando il metodo di estensione [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) su [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) che accetta un parametro `reloadOnChange` (ASP.NET Core 1.1 e versioni successive). `reloadOnChange` indica se la configurazione deve essere ricaricata alla modifica del file. Vedere questa impostazione nel metodo pratico [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) di [WebHost](/dotnet/api/microsoft.aspnetcore.webhost):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

La configurazione basata su file è rappresentata da [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource` usa [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) per monitorare i file.

Per impostazione predefinita, `IFileMonitor` viene offerto da una classe [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) che usa [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) per monitorare le modifiche apportate al file di configurazione.

L'app di esempio illustra due implementazioni per il monitoraggio delle modifiche alla configurazione. In caso di modifiche al file *appsettings.json* o alla versione dell'ambiente del file, ogni implementazione esegue codice personalizzato. L'app di esempio scrive un messaggio nella console.

L'elemento `FileSystemWatcher` di un file di configurazione può attivare più callback del token per una singola modifica del file di configurazione. L'implementazione dell'esempio evita questo problema controllando gli hash dei file nei file di configurazione. Grazie al controllo degli hash dei file è possibile assicurarsi che almeno uno dei file di configurazione sia stato modificato prima di eseguire il codice personalizzato. Nell'esempio viene usato l'hash di file SHA1 (*Utilities/Utilities.cs*):

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Viene implementato un nuovo tentativo con un'interruzione temporanea esponenziale. Il nuovo tentativo è presente perché potrebbe verificarsi un blocco di file che impedisce temporaneamente l'elaborazione di un nuovo hash in uno dei file.

### <a name="simple-startup-change-token"></a>Semplice token di modifica dell'avvio

Registrare un callback dell'elemento `Action` consumer del token per le notifiche di modifica al token di ricaricamento della configurazione (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` fornisce il token. Il callback è il metodo `InvokeChanged`:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

L'elemento `state` del callback viene usato per passare `IHostingEnvironment`. Ciò risulta utile per determinare il file JSON di configurazione *appsettings* corretto da monitorare, *appsettings.&lt;Environment&gt;.json*. Gli hash dei file vengono usati per impedire che l'istruzione `WriteConsole` venga eseguita più volte a causa di più callback del token quando il file di configurazione è stato modificato una sola volta.

Questo sistema rimane in esecuzione finché l'app è in esecuzione e non può essere disabilitato dall'utente.

### <a name="monitoring-configuration-changes-as-a-service"></a>Monitoraggio delle modifiche alla configurazione come servizio

L'esempio implementa:

* Monitoraggio di base del token di avvio.
* Monitoraggio come servizio.
* Un meccanismo per abilitare e disabilitare il monitoraggio.

L'esempio stabilisce un'interfaccia `IConfigurationMonitor` (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Il costruttore della classe implementata, `ConfigurationMonitor`, registra un callback per le notifiche di modifica:

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` fornisce il token. `InvokeChanged` è il metodo di callback. L'elemento `state` in questa istanza è un riferimento all'istanza `IConfigurationMonitor` usata per accedere allo stato di monitoraggio. Vengono usate due proprietà:

* `MonitoringEnabled` indica se il callback deve eseguire il proprio codice personalizzato.
* `CurrentState` descrive lo stato di monitoraggio corrente per l'uso nell'interfaccia utente.

Il metodo `InvokeChanged` è simile all'approccio precedente, ad eccezione del fatto che:

* Non esegue il proprio codice a meno che `MonitoringEnabled` non sia impostato su `true`.
* Annota l'elemento `state` corrente nell'output `WriteConsole`.

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Un'istanza di `ConfigurationMonitor` viene registrata come servizio in `ConfigureServices` di *Startup.cs*:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

La pagina di indice offre all'utente il controllo sul monitoraggio della configurazione. L'istanza di `IConfigurationMonitor` viene inserita in `IndexModel`:

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Un pulsante abilita e disabilita il monitoraggio:

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Quando viene attivato `OnPostStartMonitoring`, il monitoraggio è abilitato e lo stato corrente viene cancellato. Quando viene attivato `OnPostStopMonitoring`, il monitoraggio è disabilitato e lo stato viene impostato in modo da indicare che il monitoraggio non viene eseguito.

## <a name="monitoring-cached-file-changes"></a>Monitoraggio delle modifiche al file nella cache

È possibile memorizzare il contenuto del file nella cache in memoria usando [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). La memorizzazione nella cache in memoria è descritta nell'argomento [Cache in memoria](xref:performance/caching/memory). Senza eseguire passaggi aggiuntivi, ad esempio l'implementazione descritta di seguito, la cache restituirà dati *non aggiornati* (obsoleti) se i dati di origine vengono modificati.

Se durante il rinnovo di un periodo di [scadenza variabile](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) non si tiene conto dello stato di un file di origine memorizzato nella cache, i dati nella cache risulteranno non aggiornati. Ogni richiesta di dati rinnoverà il periodo di scadenza variabile, ma il file non verrà mai ricaricato nella cache. Le funzionalità dell'app che usano il contenuto del file memorizzato nella cache sono soggette alla possibile ricezione di contenuto non aggiornato.

L'uso dei token di modifica in uno scenario di memorizzazione di file nella cache consente di evitare la presenza di contenuto non aggiornato nella cache. L'app di esempio illustra un'implementazione dell'approccio.

Nell'esempio viene usato `GetFileContent` per:

* Restituire il contenuto del file.
* Implementare un algoritmo di nuovi tentativi con interruzione temporanea esponenziale per i casi in cui un blocco di file impedisca temporaneamente la lettura di un file.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Viene creato un elemento `FileService` per gestire le ricerche nel file memorizzato nella cache. La chiamata del servizio al metodo `GetFileContent` prova a ottenere il contenuto del file dalla cache in memoria e a restituirlo al chiamante (*Services/FileService.cs*).

Se non è possibile reperire il contenuto memorizzato nella cache usando la chiave della cache, verranno intraprese le azioni seguenti:

1. Il contenuto del file viene ottenuto usando `GetFileContent`.
1. Un token di modifica viene ottenuto dal provider di file con [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Il callback del token viene attivato alla modifica del file.
1. Il contenuto del file viene memorizzato nella cache con un periodo di [scadenza variabile](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration). Al token di modifica viene associato [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) per eliminare la voce della cache se il file viene modificato mentre è memorizzato nella cache.

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` è registrato nel contenitore di servizi insieme al servizio di memorizzazione nella cache (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

Il modello di pagina carica il contenuto del file usando il servizio (*Pages/Index.cshtml.cs*):

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Classe CompositeChangeToken

Per rappresentare una o più istanze di `IChangeToken` in un singolo oggetto, usare la classe [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken).

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged` nel token composito indica `true` se l'elemento `HasChanged` di qualsiasi token rappresentato è `true`. `ActiveChangeCallbacks` nel token composito indica `true` se l'elemento `ActiveChangeCallbacks` di qualsiasi token rappresentato è `true`. Se si verificano più eventi di modifica simultanei, il callback della modifica composita viene richiamato esattamente una volta.

## <a name="see-also"></a>Vedere anche

* [Cache in memoria](xref:performance/caching/memory)
* [Usare una cache distribuita](xref:performance/caching/distributed)
* [Memorizzazione nella cache delle risposte](xref:performance/caching/response)
* [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
* [Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
