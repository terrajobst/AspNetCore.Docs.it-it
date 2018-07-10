---
title: Modello di opzioni in ASP.NET Core
author: guardrex
description: Informazioni su come usare il modello di opzioni per rappresentare gruppi di opzioni correlate nelle app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: 96d7d2956fa9bf72706cde0532ee7f4ff753b72c
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126261"
---
# <a name="options-pattern-in-aspnet-core"></a>Modello di opzioni in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate. Quando le opzioni di configurazione vengono isolate in base alla funzionalità in classi separate, l'app aderisce a due importanti principi di progettazione del software:

* [Principio di segregazione delle interfacce (Interface Segregation Principle, ISP)](http://deviq.com/interface-segregation-principle/): le funzionalità (classi) che dipendono dalle impostazioni di configurazione dipendono solo dalle impostazioni di configurazione che usano.
* [Separazione delle competenze (Separation of Concerns, SoC)](http://deviq.com/separation-of-concerns/): le impostazioni di parti diverse dell'app non sono dipendenti o accoppiate l'una con l'altra.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([come eseguire il download](xref:tutorials/index#how-to-download-a-sample)) Questo articolo è più semplice da seguire con l'app di esempio.

## <a name="basic-options-configuration"></a>Configurazione delle opzioni di base

La configurazione delle opzioni di base è illustrata nell'Esempio &num;1 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Una classe di opzioni deve essere non astratta con un costruttore pubblico senza parametri. La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`. Sebbene l'impostazione dei valori predefiniti sia facoltativa, il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`. `Option2` ha un valore impostato tramite l'inizializzazione diretta della proprietà (*Models/MyOptions.cs*):

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

La classe `MyOptions` viene aggiunta al contenitore di servizi con [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) e associata alla configurazione:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

Il modello di pagina seguente usa l'[inserimento delle dipendenze del costruttore](xref:fundamentals/dependency-injection#what-is-dependency-injection) con [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) per accedere alle impostazioni (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Il file *appsettings.json* dell'esempio specifica i valori di `option1` e `option2`:

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> Se si usa un [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) personalizzato per caricare la configurazione delle opzioni da un file di impostazioni, verificare che il percorso base sia impostato correttamente:
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> L'impostazione esplicita del percorso base non è necessaria se si carica la configurazione delle opzioni dal file di impostazioni tramite [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

## <a name="configure-simple-options-with-a-delegate"></a>Configurare opzioni semplici con un delegato

La configurazione di opzioni semplici con un delegato è illustrata nell'Esempio &num;2 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Usare un delegato per impostare i valori delle opzioni. L'app di esempio usa la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

Nel codice seguente viene aggiunto un secondo servizio `IConfigureOptions<TOptions>` al contenitore di servizi. Viene usato un delegato per configurare l'associazione a `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

È possibile aggiungere più provider di configurazione. I provider di configurazione sono disponibili in pacchetti NuGet. Vengono applicati nell'ordine in cui sono registrati.

Ogni chiamata a [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) aggiunge un servizio `IConfigureOptions<TOptions>` al contenitore di servizi. Nell'esempio precedente i valori di `Option1` e `Option2` sono entrambi specificati in *appsettings.json*, mentre i valori di `Option1` e `Option2` sono sottoposti a override dal delegato configurato.

Quando sono abilitati più servizi di configurazione, l'origine di configurazione più recente specificata *ha priorità* e imposta il valore di configurazione. Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Configurazione delle opzioni secondarie

La configurazione delle opzioni secondarie è illustrata nell'Esempio &num;3 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Le app devono creare classi di opzioni che riguardano gruppi di funzionalità specifiche (classi) nell'app. Le parti dell'app che richiedono valori di configurazione devono avere accesso solo ai valori di configurazione necessari.

Durante l'associazione delle opzioni alla configurazione, ogni proprietà del tipo di opzioni viene associata a una chiave di configurazione con formato `property[:sub-property:]`. Ad esempio, la proprietà `MyOptions.Option1` viene associata alla chiave `Option1`, che viene letta dalla proprietà `option1` in *appsettings.json*.

Nel codice seguente viene aggiunto un terzo servizio `IConfigureOptions<TOptions>` al contenitore di servizi. `MySubOptions` viene associato alla sezione `subsection` del file *appsettings.json*:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

Il metodo di estensione `GetSection` richiede il pacchetto NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/). Se l'app usa il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive), il pacchetto è incluso automaticamente.

Il file *appsettings.json* dell'esempio definisce un membro `subsection` con chiavi per `suboption1` e `suboption2`:

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

La classe `MySubOptions` definisce le proprietà `SubOption1` e `SubOption2` per contenere i valori delle opzioni secondarie (*Models/MySubOptions.cs*):

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

Il metodo `OnGet` del modello di pagina restituisce una stringa con i valori delle opzioni secondarie (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Quando viene eseguita l'app, il metodo `OnGet` restituisce una stringa che mostra i valori delle classi delle opzioni secondarie:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione

Le opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione sono illustrate nell'Esempio &num;4 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Le opzioni possono essere rese disponibili in un modello di visualizzazione oppure inserendo `IOptions<TOptions>` direttamente in una visualizzazione (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Per l'inserimento diretto, inserire `IOptions<MyOptions>` con una direttiva `@inject`:

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Quando l'app viene eseguita, i valori delle opzioni sono visibili nella pagina di cui è stato eseguito il rendering:

![I valori delle opzioni Option1: value1_from_json e Option2: -1 vengono caricati dal modello e tramite inserimento nella visualizzazione.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Ricaricare i dati di configurazione con IOptionsSnapshot

Il ricaricamento dei dati di configurazione con `IOptionsSnapshot` è illustrato nell'Esempio &num;5 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Richiede ASP.NET Core 1.1 o versione successiva.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supporta il ricaricamento delle opzioni con overhead di elaborazione minimo. In ASP.NET Core 1.1 `IOptionsSnapshot` è uno snapshot di [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e viene aggiornato automaticamente ogni volta che il monitoraggio attiva le modifiche in base alle modifiche dell'origine dati. In ASP.NET Core 2.0 e versioni successive le opzioni vengono calcolate una volta per richiesta quando viene eseguito l'accesso e la memorizzazione nella cache per la durata della richiesta.

L'esempio seguente illustra la creazione di un nuovo `IOptionsSnapshot` dopo le modifiche ad *appsettings.json* (*Pages/Index.cshtml.cs*). Più richieste al server restituiscono valori costanti forniti dal file *appsettings.json* fino a quando il file non viene modificato e la configurazione non viene ricaricata.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

L'immagine seguente illustra i valori iniziali `option1` e `option2` caricati dal file *appsettings.json*:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Modificare i valori nel file *appsettings.json* in `value1_from_json UPDATED` e `200`. Salvare il file *appsettings.json*. Aggiornare il browser per visualizzare i valori delle opzioni aggiornati:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Supporto delle opzioni denominate con IConfigureNamedOptions

Il supporto delle opzioni denominate con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) è illustrato nell'Esempio &num;6 nell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Richiede ASP.NET Core 2.0 o versione successiva.*

Il supporto delle *opzioni denominate* consente all'app di distinguere le configurazioni delle opzioni denominate. Nell'app di esempio le opzioni denominate vengono dichiarate con [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) che a sua volta chiama il metodo di estensione [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure):

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

L'app di esempio accede alle opzioni denominate con [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Eseguendo l'app di esempio, vengono restituite le opzioni denominate:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

I valori `named_options_1` vengono specificati dalla configurazione, caricata dal file *appsettings.json*. I valori `named_options_2` sono specificati da:

* Delegato `named_options_2` in `ConfigureServices` per `Option1`.
* Valore predefinito per `Option2` specificato dalla classe `MyOptions`.

Configurare tutte le istanze delle opzioni denominate con il metodo [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall). Il codice seguente configura `Option1` per tutte le istanze delle opzioni denominate con un valore comune. Aggiungere manualmente il codice seguente al metodo `Configure`:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

L'esecuzione dell'app di esempio dopo l'aggiunta del codice produce il risultato seguente:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> In ASP.NET Core 2.0 e versioni successive tutte le opzioni sono istanze denominate. Le istanze di `IConfigureOption` esistenti sono trattate come se fossero indirizzate all'istanza di `Options.DefaultName`, ovvero `string.Empty`. `IConfigureNamedOptions` implementa anche `IConfigureOptions`. L'implementazione predefinita di [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origine riferimento](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) ha la logica per usare ogni opzione nel modo appropriato. L'opzione denominata `null` viene usata per avere come destinazione tutte le istanze denominate anziché un'istanza specifica denominata (questa convenzione è usata da [ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*Richiede ASP.NET Core 2.0 o versione successiva.*

Impostare la post-configurazione con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). La post-configurazione viene eseguita dopo la configurazione [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) è disponibile per eseguire la post-configurazione delle opzioni denominate:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Usare [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) per eseguire la post-configurazione di tutte le istanze delle configurazioni denominate:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Factory, monitoraggio e memorizzazione nella cache delle opzioni

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) viene usata per le notifiche quando vengono modificate le istanze `TOptions`. `IOptionsMonitor` supporta le opzioni ricaricabili, le notifiche delle modifiche e `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 o versione successiva) è responsabile della creazione di nuove istanze delle opzioni. Ha un unico metodo [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create). L'implementazione predefinita accetta tutte le interfacce `IConfigureOptions` e `IPostConfigureOptions` registrate ed esegue tutte le configurazioni seguite dalle post-configurazioni. Fa distinzione tra `IConfigureNamedOptions` e `IConfigureOptions` e chiama solo l'interfaccia appropriata.

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 o versione successiva) viene usata da `IOptionsMonitor` per memorizzare nella cache le istanze `TOptions`. `IOptionsMonitorCache` invalida le istanze delle opzioni nel monitoraggio in modo che il valore venga ricalcolato ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). È anche possibile inserire i valori manualmente con [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). Il metodo [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) viene usato quando tutte le istanze denominate devono essere ricreate su richiesta.

## <a name="accessing-options-during-startup"></a>Accesso alle opzioni durante l'avvio

`IOptions` può essere usata in `Configure` poiché i servizi vengono compilati prima dell'esecuzione del metodo `Configure`. Se un provider di servizi viene compilato in `ConfigureServices` per accedere alle opzioni, non conterrà le configurazioni di opzioni specificate dopo la compilazione del provider. Per questa ragione, le opzioni potrebbero avere uno stato incoerente a causa dell'ordinamento delle registrazioni dei servizi.

Poiché le opzioni vengono in genere caricate dalla configurazione, è possibile usare la configurazione all'avvio in `Configure` e in `ConfigureServices`. Per esempi di utilizzo della configurazione durante l'avvio, vedere l'argomento [Avvio dell'applicazione](xref:fundamentals/startup).

## <a name="see-also"></a>Vedere anche

* [Configurazione](xref:fundamentals/configuration/index)
