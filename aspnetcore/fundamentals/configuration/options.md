---
title: Serie di opzioni in ASP.NET Core
author: guardrex
description: Viene descritto come utilizzare il modello di opzioni per rappresentare i gruppi di impostazioni correlate in applicazioni ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: aab96b5313a8632950e51f5586612c1d0d3d176e
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2018
---
# <a name="options-pattern-in-aspnet-core"></a>Serie di opzioni in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Il modello di opzioni usa le classi di opzioni per rappresentare i gruppi di impostazioni correlate. Quando le impostazioni di configurazione sono isolate dalla funzionalità in classi di opzioni distinte, l'app conforme alle due principi di progettazione del software importanti:

* Il [interfaccia separazione principio (ISP)](http://deviq.com/interface-segregation-principle/): funzionalità (classi) che dipendono dalle impostazioni di configurazione dipendono solo le impostazioni di configurazione che usano.
* [La separazione dei compiti](http://deviq.com/separation-of-concerns/): le impostazioni per le diverse parti dell'app non sono dipendenti o a uno a altro.

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) in questo articolo è più facile da seguire con l'app di esempio.

## <a name="basic-options-configuration"></a>Configurazione delle opzioni di base

Configurazione delle opzioni di base viene illustrata come esempio &num;1 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Classe di opzioni deve essere non astratto con costruttore pubblico senza parametri. La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`. Impostazione dei valori predefiniti è facoltativo, ma il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`. `Option2`il valore predefinito impostato inizializzando direttamente la proprietà è (*Models/MyOptions.cs*):

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

Il `MyOptions` classe viene aggiunto al contenitore del servizio con [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) e associato alla configurazione:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

La seguente pagina modello Usa [inserimento di dipendenze di costruttore](xref:fundamentals/dependency-injection#what-is-dependency-injection) con [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) per accedere alle impostazioni (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

L'esempio *appSettings. JSON* file specifica i valori per `option1` e `option2`:

[!code-json[Main](options/sample/appsettings.json)]

Quando si esegue l'app, il modello di pagina `OnGet` metodo restituisce una stringa contenente i valori di classe di opzione:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Configurare le opzioni semplice con un delegato

Configurazione delle opzioni semplice con un delegato viene illustrata come esempio &num;2 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Per impostare i valori delle opzioni, utilizzare un delegato. L'applicazione di esempio utilizza il `MyOptionsWithDelegateConfig` classe (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

Nel codice seguente, una seconda `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio. Usa un delegato per configurare l'associazione con `MyOptionsWithDelegateConfig`:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

È possibile aggiungere più provider di configurazione. Provider di configurazione sono disponibili in pacchetti NuGet. Vengono applicati nell'ordine in cui sono registrati.

Ogni chiamata a [configura&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) aggiunge un `IConfigureOptions<TOptions>` servizio al contenitore del servizio. Nell'esempio precedente, i valori di `Option1` e `Option2` sono entrambi specificati nel *appSettings. JSON*, ma i valori di `Option1` e `Option2` vengono sottoposti a override dal delegato configurato.

Quando più di un servizio di configurazione è abilitato, l'ultima origine di configurazione specificato *wins* e imposta il valore di configurazione. Quando si esegue l'app, il modello di pagina `OnGet` metodo restituisce una stringa contenente i valori di classe di opzione:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Configurazione le opzioni secondarie

Configurazione le opzioni secondarie viene illustrato come esempio &num;3 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Le app devono creare classi di opzioni che riguardano i gruppi di funzionalità specifiche (classi) nell'app. Parti dell'app che richiedono valori di configurazione solo deve accedere ai valori di configurazione che usano.

Quando l'associazione le opzioni di configurazione, ogni proprietà nel tipo di opzioni è associata a una chiave di configurazione del modulo `property[:sub-property:]`. Ad esempio, il `MyOptions.Option1` proprietà è associata alla chiave `Option1`, che viene letto dal `option1` proprietà *appSettings. JSON*.

Nel codice seguente, una terza `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio. Associa `MySubOptions` alla sezione `subsection` del *appSettings. JSON* file:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

Il `GetSection` metodo di estensione richiede il [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pacchetto NuGet. Se l'app Usa la [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, il pacchetto viene incluso automaticamente.

L'esempio *appSettings. JSON* file definisce un `subsection` membro con le chiavi per `suboption1` e `suboption2`:

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

Il `MySubOptions` classe definisce le proprietà, `SubOption1` e `SubOption2`, per contenere i valori dell'opzione secondaria (*Models/MySubOptions.cs*):

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

Il modello di pagina `OnGet` metodo restituisce una stringa con i valori dell'opzione secondaria (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Quando si esegue l'app, il `OnGet` metodo restituisce una stringa contenente l'opzione secondaria di valori di classe:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opzioni fornite da un modello di visualizzazione o con inserimento diretto di visualizzazione

Opzioni fornite da un modello di visualizzazione o con inserimento diretto di visualizzazione viene illustrato come esempio &num;4 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

È possibile specificare le opzioni in un modello di visualizzazione o inserendo `IOptions<TOptions>` direttamente in una vista (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Per l'aggiunta diretta, inserire `IOptions<MyOptions>` con un `@inject` direttiva:

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Quando si esegue l'app, i valori delle opzioni presenti nella pagina sottoposta a rendering:

![Opzioni valori opzione1: value1_from_json e opzione2: -1 vengono caricati dal modello e da inserimento nella vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Ricaricare i dati di configurazione con IOptionsSnapshot

Ricaricare i dati di configurazione con `IOptionsSnapshot` è illustrato nell'esempio &num;5 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Richiede ASP.NET Core 1.1 o successiva.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supporta il ricaricamento di opzioni con minimo l'overhead di elaborazione. In ASP.NET Core 1.1 `IOptionsSnapshot` è uno snapshot di [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e gli aggiornamenti ogni volta che il monitoraggio viene attivato automaticamente le modifiche in base alla modifica origine dati. In ASP.NET Core 2.0 e versioni successive, le opzioni vengono calcolate una volta per ogni richiesta quando si accede e memorizzato nella cache per la durata della richiesta.

Nell'esempio seguente viene illustrato come un nuovo `IOptionsSnapshot` viene creato dopo *appSettings. JSON* modifiche (*Pages/Index.cshtml.cs*). Più richieste al server di restituiranno valori costanti forniti dal *appSettings. JSON* file fino a quando non viene modificato il file e ricaricamenti configurazione.

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

L'immagine seguente mostra iniziale `option1` e `option2` caricati da valori di *appSettings. JSON* file:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Modificare i valori di *appSettings. JSON* file `value1_from_json UPDATED` e `200`. Salvare il *appSettings. JSON* file. Aggiornare il browser per vedere che vengono aggiornati i valori delle opzioni:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Opzioni di supporto con IConfigureNamedOptions denominato

Denominato opzioni di supporto con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) viene illustrato come esempio &num;6 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*È necessario ASP.NET 2.0 o versione successiva di base.*

*Opzioni denominato* supporto consente all'applicazione di distinguere tra le configurazioni di opzioni denominata. Nell'applicazione di esempio, le opzioni denominate vengono dichiarate con la [ConfigureNamedOptions&lt;TOptions&gt;. Configurare](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) metodo:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

L'app di esempio accede le opzioni denominate con [IOptionsSnapshot&lt;TOptions&gt;. Ottenere](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Esecuzione dell'applicazione di esempio, vengono restituite le opzioni denominate:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1`vengono forniti i valori dalla configurazione, che vengono caricati dal *appSettings. JSON* file. `named_options_2`i valori vengono forniti da:

* Il `named_options_2` delegare `ConfigureServices` per `Option1`.
* Il valore predefinito per `Option2` fornito dalla `MyOptions` classe.

Configurare tutte le istanze denominate opzioni con il [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) metodo. Nel codice seguente viene configurato `Option1` per tutte le istanze di configurazione con un valore comune denominate. Aggiungere il codice seguente manualmente al `Configure` metodo:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Esecuzione dell'applicazione di esempio dopo l'aggiunta del codice produce il risultato seguente:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> In ASP.NET Core 2.0 e versioni successive, tutte le opzioni sono istanze denominate. Esistente `IConfigureOption` istanze vengono considerate come destinazione il `Options.DefaultName` istanza, ovvero `string.Empty`. `IConfigureNamedOptions`implementa inoltre `IConfigureOptions`. L'implementazione predefinita del [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origine riferimento](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) ha la logica per utilizzarle in modo appropriato. Il `null` denominata opzione viene utilizzata per tutte le istanze denominate, invece di una specifica istanza denominata di destinazione ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) utilizzano questa convenzione).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*È necessario ASP.NET 2.0 o versione successiva di base.*

Impostare postconfiguration con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). Post-configurazione viene eseguita dopo che tutti [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) viene eseguita la configurazione:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) è disponibile per post-configurare le opzioni denominate:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Utilizzare [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) di post-configurare tutte denominato istanze di configurazione:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Factory di opzioni, il monitoraggio e della cache

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) viene utilizzato per notifiche quando `TOptions` istanze di modifica. `IOptionsMonitor`Opzioni reloadable supporta notifiche di modifica, e `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (Core ASP.NET 2.0 o versione successiva) è responsabile della creazione di nuove istanze di opzioni. Dispone di un singolo [crea](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) metodo. L'implementazione predefinita accetta tutti registrato `IConfigureOptions` e `IPostConfigureOptions` ed esegue tutti il Configura prima, seguita dalla post-Configura. Distingue tra `IConfigureNamedOptions` e `IConfigureOptions` e chiama solo l'interfaccia appropriata.

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (Core ASP.NET 2.0 o versione successiva) viene utilizzato da `IOptionsMonitor` cache `TOptions` istanze. Il `IOptionsMonitorCache` invalida le istanze di opzioni del monitoraggio in modo che il valore viene ricalcolato ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). I valori possono essere manualmente introdotte anche da [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). Il [deselezionare](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) metodo viene utilizzato per tutte le istanze denominate devono essere ricreate su richiesta.

## <a name="accessing-options-during-startup"></a>Accedere alle opzioni durante l'avvio

`IOptions`può essere usato in `Configure`, dal momento che i servizi vengono compilati prima di `Configure` il metodo viene eseguito. Se un provider di servizi viene compilato `ConfigureServices` per accedere alle opzioni, non non contenere opzioni configurazioni fornite una volta creato il provider del servizio. Pertanto, uno stato incoerente opzioni può esistere a causa di ordinamento di registrazioni di servizio.

Poiché le opzioni vengono in genere caricate dalla configurazione, è possibile utilizzare configurazione di avvio sia in `Configure` e `ConfigureServices`. Per esempi di utilizzo di configurazione durante l'avvio, vedere il [avvio dell'applicazione](xref:fundamentals/startup) argomento.

## <a name="see-also"></a>Vedere anche

* [Configurazione](xref:fundamentals/configuration/index)
