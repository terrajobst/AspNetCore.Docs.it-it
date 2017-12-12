---
title: Rilevare le modifiche apportate con i token di modifica principale di ASP.NET
author: guardrex
description: Informazioni su come usare i token di modifica per tenere traccia delle modifiche.
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: a9479e3d676ed4dc880996a4a77de30d82b84cd5
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2017
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Rilevare le modifiche apportate con i token di modifica principale di ASP.NET

Di [Luke Latham](https://github.com/guardrex)

Oggetto *modificare token* è un blocco predefinito di uso generale e di basso livello utilizzato per tenere traccia delle modifiche.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Interfaccia IChangeToken

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaga le notifiche che si è verificata una modifica. `IChangeToken`cui risiede il [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) dello spazio dei nomi. Per le app che non usano il [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, riferimento di [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pacchetto NuGet nel file di progetto.

`IChangeToken`ha due proprietà:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicare se il token genera in modo proattivo i callback. Se `ActiveChangedCallbacks` è impostato su `false`un callback non viene mai chiamato e l'app deve eseguire il polling `HasChanged` per le modifiche. È inoltre possibile per un token di annullamento mai se si verifica alcuna modifica o il listener di modifica sottostante viene eliminato o disabilitato.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) Ottiene un valore che indica se è stata modificata.

L'interfaccia dispone di un metodo, [RegisterChangeCallback (azione&lt;oggetto&gt;, oggetto)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), che registra un callback che viene richiamato quando il token è stato modificato. `HasChanged`deve essere impostata prima che venga richiamato il callback.

## <a name="changetoken-class"></a>Classe del token di modifica

`ChangeToken`una classe statica viene utilizzata per propagare le notifiche che si è verificata una modifica. `ChangeToken`cui risiede il [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) dello spazio dei nomi. Per le app che non usano il [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, riferimento di [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pacchetto NuGet nel file di progetto.

Il `ChangeToken` [OnChange (Func&lt;IChangeToken&gt;, azione)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) metodo registri un `Action` per chiamare ogni volta che cambia il token:
* `Func<IChangeToken>`Genera il token.
* `Action`viene chiamato quando cambia il token.

`ChangeToken`è un [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, azione&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload che accetta un aggiuntive`TState`parametro passato al consumer di token `Action`.

`OnChange`Restituisce un [IDisposable](/dotnet/api/system.idisposable). La chiamata [Dispose](/dotnet/api/system.idisposable.dispose) interrompe il token da ascoltare per apportare ulteriori modifiche e rilascia le risorse del token.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Esempi di utilizzo di token di modifica in ASP.NET Core

I token di modifica vengono utilizzati nelle aree principali di ASP.NET Core le modifiche agli oggetti di monitoraggio:

* Per controllare le modifiche ai file, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)del [espressioni di controllo](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) metodo crea un `IChangeToken` per il file specificati o la cartella da controllare.
* `IChangeToken`token possono essere aggiunto alle voci della cache per attivare le eliminazioni dalla cache delle modifiche.
* Per `TOptions` cambia, il valore predefinito [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementazione di [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) presenta un overload che accetta uno o più [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)istanze. Ogni istanza restituisce un `IChangeToken` per registrare un callback di notifica di modifica per opzioni di rilevamento delle modifiche.

## <a name="monitoring-for-configuration-changes"></a>Monitoraggio delle modifiche di configurazione

Per impostazione predefinita, utilizzano i modelli ASP.NET Core [i file di configurazione JSON](xref:fundamentals/configuration/index#json-configuration) (*appSettings. JSON*, *appsettings. Development.JSON*, e *appsettings. Production.JSON*) per caricare le impostazioni di configurazione app.

Questi file vengono configurati utilizzando il [AddJsonFile (IConfigurationBuilder, String, Boolean, booleano)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) metodo di estensione su [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) che accetta un `reloadOnChange` parametro (ASP.NET Core 1.1 e versioni successive). `reloadOnChange`indica se deve essere ricaricata configurazione su modifiche dei file. Vedere questa impostazione nel [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) metodo pratico [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([origine riferimento](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Configurazione basata su file è rappresentato da [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource`Usa [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([origine riferimento](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) per monitorare i file.

Per impostazione predefinita, il `IFileMonitor` viene fornito da un [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([origine riferimento](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), che usa [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) da monitorare per i file di configurazione modifiche.

L'app di esempio illustra due implementazioni per il monitoraggio delle modifiche di configurazione. Se il valore di *appSettings. JSON* modifiche al file o la versione di ambiente del file cambia, ogni implementazione esegue codice personalizzato. L'app di esempio scrive un messaggio nella console.

Un file di configurazione `FileSystemWatcher` può attivare più callback token per una modifica di file di configurazione dell'accesso single. Implementazione dell'esempio protegge il problema controllando gli hash di file nel file di configurazione. Verifica l'hash dei file assicura che almeno uno dei file di configurazione è stato modificato prima di eseguire il codice personalizzato. Nell'esempio viene utilizzato l'hash file SHA1 (*Utilities/Utilities.cs*):

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Un nuovo tentativo è implementato con un backoff esponenziale. Provare nuovamente è presente, poiché il blocco di file può verificarsi che impedisce temporaneamente l'elaborazione di un nuovo hash su uno dei file.

### <a name="simple-startup-change-token"></a>Token di modifica di avvio semplici

Registrare un consumer di token `Action` callback per le notifiche di modifica per il token di ricaricamento della configurazione (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()`fornisce il token. Il callback di `InvokeChanged` metodo:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

Il `state` del metodo di callback viene utilizzato per passare il `IHostingEnvironment`. Ciò è utile per determinare il corretto *appsettings* file JSON di configurazione per il monitoraggio, *appsettings.&lt; Ambiente&gt;JSON*. Hash dei file vengono utilizzati per impedire il `WriteConsole` istruzione da eseguire più volte a causa di più callback token quando cambia il file di configurazione solo una volta.

Questo sistema viene eseguita finché l'applicazione è in esecuzione e non può essere disabilitata dall'utente.

### <a name="monitoring-configuration-changes-as-a-service"></a>Monitoraggio delle modifiche di configurazione come servizio

Nell'esempio viene implementato:

* Monitoraggio token di avvio di base.
* Come un servizio di monitoraggio.
* Un meccanismo per abilitare e disabilitare il monitoraggio.

L'esempio stabilisce un `IConfigurationMonitor` interfaccia (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Il costruttore della classe implementata, `ConfigurationMonitor`, registra un callback per le notifiche di modifica:

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()`fornisce il token. `InvokeChanged`il metodo di callback. Il `state` in questa istanza è una stringa che descrive lo stato di monitoraggio. Vengono utilizzate due proprietà:

* `MonitoringEnabled`indica se il callback deve essere eseguito il codice personalizzato.
* `CurrentState`descrive lo stato di monitoraggio corrente per l'utilizzo nell'interfaccia utente.

Il `InvokeChanged` metodo è simile a quella precedente, a eccezione del fatto che:

* Non viene eseguito il codice a meno che non `MonitoringEnabled` è `true`.
* Imposta il `CurrentState` stringa della proprietà di un messaggio descrittivo che registra l'ora che è stato eseguito il codice.
* Note sulla corrente `state` nel relativo `WriteConsole` output.

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Un'istanza `ConfigurationMonitor` viene registrato come un servizio in `ConfigureServices` di *Startup.cs*:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

La pagina di indice offre il controllo utente tramite il monitoraggio di configurazione. L'istanza di `IConfigurationMonitor` , viene inserito nel `IndexModel`:

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Un pulsante Abilita e disabilita il monitoraggio:

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Quando `OnPostStartMonitoring` è attivato, il monitoraggio è abilitato, e lo stato corrente viene cancellato. Quando `OnPostStopMonitoring` è attivato, il monitoraggio è disabilitato e lo stato viene impostato in modo da riflettere che il monitoraggio non è in esecuzione.

## <a name="monitoring-cached-file-changes"></a>Monitoraggio delle modifiche apportate alla file memorizzato nella cache

Contenuto del file può essere memorizzato nella cache in memoria usando [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Memorizzazione nella cache è descritta nel [memorizzazione nella cache In memoria](xref:performance/caching/memory) argomento. Senza eseguire passaggi aggiuntivi, ad esempio l'implementazione descritti di seguito, *obsoleti* dati (obsoleti) viene restituiti da una cache, se i dati di origine cambiano.

Non tenendo in considerazione lo stato di un file di origine memorizzato nella cache durante il rinnovo un [scadenza con scorrimento](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) periodo comporta cache dati non aggiornati. Ogni richiesta per i dati rinnova la scadenza variabile, ma il file non viene ricaricato mai nella cache. Le funzionalità di app che usano il contenuto del file memorizzato nella cache sono soggetti a probabilmente la ricezione di contenuto non aggiornato.

Uso di token di modifica in un file di memorizzazione nella cache di scenario impedisce il contenuto nella cache i file obsoleti. L'app di esempio viene illustrata un'implementazione dell'approccio.

L'esempio utilizza `GetFileContent` per:

* Restituisce il contenuto di file.
* Implementare un algoritmo di tentativi con backoff esponenziale nei casi in cui un blocco di file temporaneamente impedisce un file da leggere.

*Utilities/Utilities.cs*:

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Oggetto `FileService` viene creato per gestire le ricerche di file memorizzati nella cache. Il `GetFileContent` chiamata al metodo del servizio tenta di ottenere il contenuto di file dalla cache in memoria e restituirlo al chiamante (*Services/FileService.cs*).

Se non viene trovato il contenuto nella cache utilizzando la chiave di cache, vengono eseguite le operazioni seguenti:

1. Il contenuto del file viene ottenuto utilizzando `GetFileContent`.
1. Un token di modifica viene ottenuto dal provider di file con [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Callback del token viene attivata quando il file viene modificato.
1. Il contenuto del file viene memorizzato nella cache con un [scadenza con scorrimento](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) periodo. Il token di modifica è collegato con [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) per rimuovere la voce della cache, se il file viene modificato mentre viene memorizzato.

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

Il `FileService` è registrato nel contenitore dei servizi con la memoria del servizio di memorizzazione nella cache (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

Il modello di pagina Carica il contenuto del file tramite il servizio (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Classe CompositeChangeToken

Per la rappresentazione di una o più `IChangeToken` le istanze di un singolo oggetto, utilizzare il [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) classe ([origine riferimento](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).

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

`HasChanged`per i report di token compositi `true` se qualsiasi rappresentato token `HasChanged` è `true`. `ActiveChangeCallbacks`per i report di token compositi `true` se qualsiasi rappresentato token `ActiveChangeCallbacks` è `true`. Se si verificano più eventi di modifica simultanea, la modifica composito richiamata esattamente una volta.

## <a name="see-also"></a>Vedere anche

* [Memorizzazione nella cache in memoria](xref:performance/caching/memory)
* [Utilizzo di una cache distribuita](xref:performance/caching/distributed)
* [Rilevare le modifiche apportate con i token di modifica](xref:fundamentals/primitives/change-tokens)
* [Memorizzazione nella cache delle risposte](xref:performance/caching/response)
* [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
* [Helper di Tag della cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper di Tag Cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
