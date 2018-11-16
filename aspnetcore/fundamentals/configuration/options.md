---
title: Modello di opzioni in ASP.NET Core
author: guardrex
description: Informazioni su come usare il modello di opzioni per rappresentare gruppi di opzioni correlate nelle app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/09/2018
uid: fundamentals/configuration/options
ms.openlocfilehash: 99aa5028a8704c7e9e3010415137e2560213a2ad
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505797"
---
# <a name="options-pattern-in-aspnet-core"></a>Modello di opzioni in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate. Quando le [impostazioni di configurazione](xref:fundamentals/configuration/index) vengono isolate in base allo scenario in classi separate, l'app aderisce a due importanti principi di progettazione del software:

* [Principio di segregazione delle interfacce (Interface Segregation Principle, ISP)](http://deviq.com/interface-segregation-principle/): gli scenari (classi) che dipendono dalle impostazioni di configurazione dipendono solo dalle impostazioni di configurazione che usano.
* [Separazione delle competenze (Separation of Concerns, SoC)](http://deviq.com/separation-of-concerns/): le impostazioni di parti diverse dell'app non sono dipendenti o accoppiate l'una con l'altra.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([come eseguire il download](xref:index#how-to-download-a-sample)) Questo articolo è più semplice da seguire con l'app di esempio.

## <a name="prerequisites"></a>Prerequisiti

::: moniker range=">= aspnetcore-2.1"

Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Fare riferimento al [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aggiungere un riferimento al pacchetto [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).

::: moniker-end

## <a name="basic-options-configuration"></a>Configurazione delle opzioni di base

La configurazione delle opzioni di base è illustrata nell'Esempio &num;1 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Una classe di opzioni deve essere non astratta con un costruttore pubblico senza parametri. La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`. Sebbene l'impostazione dei valori predefiniti sia facoltativa, il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`. `Option2` ha un valore impostato tramite l'inizializzazione diretta della proprietà (*Models/MyOptions.cs*):

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

La classe `MyOptions` viene aggiunta al contenitore di servizi con [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) e associata alla configurazione:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

Il modello di pagina seguente usa l'[inserimento delle dipendenze del costruttore](xref:mvc/controllers/dependency-injection) con [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) per accedere alle impostazioni (*Pages/Index.cshtml.cs*):

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

Le app devono creare classi di opzioni che riguardano gruppi di scenari (classi) specifici nell'app. Le parti dell'app che richiedono valori di configurazione devono avere accesso solo ai valori di configurazione necessari.

Durante l'associazione delle opzioni alla configurazione, ogni proprietà del tipo di opzioni viene associata a una chiave di configurazione con formato `property[:sub-property:]`. Ad esempio, la proprietà `MyOptions.Option1` viene associata alla chiave `Option1`, che viene letta dalla proprietà `option1` in *appsettings.json*.

Nel codice seguente viene aggiunto un terzo servizio `IConfigureOptions<TOptions>` al contenitore di servizi. `MySubOptions` viene associato alla sezione `subsection` del file *appsettings.json*:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

Il metodo di estensione `GetSection` richiede il pacchetto NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/). Se l'app usa il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive), il pacchetto è incluso automaticamente.

Il file *appsettings.json* dell'esempio definisce un membro `subsection` con chiavi per `suboption1` e `suboption2`:

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

La classe `MySubOptions` definisce le proprietà `SubOption1` e `SubOption2` per contenere i valori delle opzioni (*Models/MySubOptions.cs*):

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

Il metodo `OnGet` del modello di pagina restituisce una stringa con i valori delle opzioni (*Pages/Index.cshtml.cs*):

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

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Ricaricare i dati di configurazione con IOptionsSnapshot

Il ricaricamento dei dati di configurazione con `IOptionsSnapshot` è illustrato nell'Esempio &num;5 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supporta il ricaricamento delle opzioni con overhead di elaborazione minimo.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Le opzioni vengono calcolate una volta per richiesta quando viene eseguito l'accesso e la memorizzazione nella cache per la durata della richiesta.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`IOptionsSnapshot` è uno snapshot di [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e viene aggiornato automaticamente ogni volta che il monitoraggio attiva le modifiche in base alle modifiche dell'origine dati.

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

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

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Supporto delle opzioni denominate con IConfigureNamedOptions

Il supporto delle opzioni denominate con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) è illustrato nell'Esempio &num;6 nell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

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

## <a name="configure-all-options-with-the-configureall-method"></a>Configurare tutte le opzioni con il metodo ConfigureAll

Configurare tutte le istanze delle opzioni con il metodo [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall). Il codice seguente configura `Option1` per tutte le istanze di configurazione con un valore comune. Aggiungere manualmente il codice seguente al metodo `ConfigureServices`:

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
> Tutte le opzioni sono istanze denominate. Le istanze di `IConfigureOption` esistenti sono trattate come se fossero indirizzate all'istanza di `Options.DefaultName`, ovvero `string.Empty`. `IConfigureNamedOptions` implementa anche `IConfigureOptions`. L'implementazione predefinita di [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origine riferimento](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) ha la logica per usare ogni opzione nel modo appropriato. L'opzione denominata `null` viene usata per avere come destinazione tutte le istanze denominate anziché un'istanza specifica denominata (questa convenzione è usata da [ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="optionsbuilder-api"></a>API OptionsBuilder

<xref:Microsoft.Extensions.Options.OptionsBuilder`1> viene usata per configurare le istanze di `TOptions`. `OptionsBuilder` semplifica la creazione di opzioni denominate perché è costituita da un singolo parametro nella chiamata iniziale ad `AddOptions<TOptions>(string optionsName)` invece di essere visualizzata in tutte le chiamate successive. La convalida delle opzioni e gli overload `ConfigureOptions` che accettano le dipendenze dei servizi sono disponibili solo tramite `OptionsBuilder`.

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");
    
services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="configurelttoptions-tdep1--tdep4gt-method"></a>Metodo Configure&lt;TOptions, TDep1, ... TDep4&gt;

L'uso di servizi dall'inserimento di dipendenze per configurare le opzioni implementando `IConfigure[Named]Options` in modo standard risulta faticoso. Gli overload per `ConfigureOptions` in `OptionsBuilder<TOptions>` consentono di usare fino a cinque servizi per configurare le opzioni:

```csharp
services.AddOptions<MyOptions>("optionalName")
    .Configure<Service1, Service2, Service3, Service4, Service5>(
        (o, s, s2, s3, s4, s5) => 
            o.Property = DoSomethingWith(s, s2, s3, s4, s5));
```

L'overload registra un'interfaccia <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> generica temporanea, che ha un costruttore che accetta i tipi di servizi generici specificati. 

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a>Convalida delle opzioni

La convalida le opzioni consente di convalidare le opzioni quando vengono configurate. Chiamare `Validate` con un metodo di convalida che restituisce `true` se le opzioni sono valide e `false` se non sono valide:

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

L'esempio precedente imposta l'istanza di opzioni denominata su `optionalOptionsName`. L'istanza di opzioni predefinita è `Options.DefaultName`.

La convalida viene eseguita quando viene creata l'istanza di opzioni. L'istanza di opzioni supera sicuramente la convalida al primo accesso.

> [!IMPORTANT]
> La convalida delle opzioni non protegge da eventuali modifiche dopo la configurazione e la convalida iniziali delle opzioni.

Il metodo `Validate` accetta `Func<TOptions, bool>`. Per personalizzare completamente la convalida, implementare `IValidateOptions<TOptions>`, che consente:

* La convalida di più tipi di opzioni: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`
* La convalida che dipende da un altro tipo di opzione: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`

`IValidateOptions` convalida:

* Un'istanza di opzioni denominata specifica.
* Tutte le opzioni quando `name` è `null`.

Restituire `ValidateOptionsResult` dall'implementazione dell'interfaccia:

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

La convalida basata sull'annotazione dei dati è disponibile nel pacchetto [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) chiamando il metodo `ValidateDataAnnotations` su `OptionsBuilder<TOptions>`:

```csharp
private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}
    
[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptions<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}    
```

La convalida eager (fallimento e risposta immediata agli errori all'avvio) è pianificata per una versione futura.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

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

Usare [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) per eseguire la post-configurazione di tutte le istanze di configurazione:

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a>Factory, monitoraggio e memorizzazione nella cache delle opzioni

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) viene usata per le notifiche quando vengono modificate le istanze `TOptions`. `IOptionsMonitor` supporta le opzioni ricaricabili, le notifiche delle modifiche e `IPostConfigureOptions`.

::: moniker range=">= aspnetcore-2.0"

[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) è responsabile della creazione di nuove istanze delle opzioni. Ha un unico metodo [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create). L'implementazione predefinita accetta tutte le interfacce `IConfigureOptions` e `IPostConfigureOptions` registrate ed esegue tutte le configurazioni seguite dalle post-configurazioni. Fa distinzione tra `IConfigureNamedOptions` e `IConfigureOptions` e chiama solo l'interfaccia appropriata.

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) viene usata da `IOptionsMonitor` per memorizzare nella cache le istanze `TOptions`. `IOptionsMonitorCache` invalida le istanze delle opzioni nel monitoraggio in modo che il valore venga ricalcolato ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). È anche possibile inserire i valori manualmente con [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). Il metodo [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) viene usato quando tutte le istanze denominate devono essere ricreate su richiesta.

::: moniker-end

## <a name="accessing-options-during-startup"></a>Accesso alle opzioni durante l'avvio

`IOptions` può essere usata in `Startup.Configure` poiché i servizi vengono compilati prima dell'esecuzione del metodo `Configure`.

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

`IOptions` non deve essere usata in `Startup.ConfigureServices`. Le opzioni potrebbero avere uno stato incoerente a causa dell'ordinamento delle registrazioni dei servizi.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/configuration/index>
