---
title: Modello di opzioni in ASP.NET Core
author: guardrex
description: Informazioni su come usare il modello di opzioni per rappresentare gruppi di opzioni correlate nelle app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: fd3e55ec821be336501f523550f547f6049c9937
ms.sourcegitcommit: 4e34ce61e1e7f1317102b16012ce0742abf2cca6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514752"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="f6f93-103">Modello di opzioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6f93-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="f6f93-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f6f93-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f6f93-105">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="f6f93-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="f6f93-106">Quando le opzioni di configurazione vengono isolate in base alla funzionalità in classi separate, l'app aderisce a due importanti principi di progettazione del software:</span><span class="sxs-lookup"><span data-stu-id="f6f93-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="f6f93-107">[Principio di segregazione delle interfacce (Interface Segregation Principle, ISP)](http://deviq.com/interface-segregation-principle/): le funzionalità (classi) che dipendono dalle impostazioni di configurazione dipendono solo dalle impostazioni di configurazione che usano.</span><span class="sxs-lookup"><span data-stu-id="f6f93-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="f6f93-108">[Separazione delle competenze (Separation of Concerns, SoC)](http://deviq.com/separation-of-concerns/): le impostazioni di parti diverse dell'app non sono dipendenti o accoppiate l'una con l'altra.</span><span class="sxs-lookup"><span data-stu-id="f6f93-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="f6f93-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([come eseguire il download](xref:tutorials/index#how-to-download-a-sample)) Questo articolo è più semplice da seguire con l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f6f93-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="f6f93-110">Configurazione delle opzioni di base</span><span class="sxs-lookup"><span data-stu-id="f6f93-110">Basic options configuration</span></span>

<span data-ttu-id="f6f93-111">La configurazione delle opzioni di base è illustrata nell'Esempio &num;1 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="f6f93-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="f6f93-112">Una classe di opzioni deve essere non astratta con un costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="f6f93-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="f6f93-113">La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="f6f93-114">Sebbene l'impostazione dei valori predefiniti sia facoltativa, il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="f6f93-115">`Option2` ha un valore impostato tramite l'inizializzazione diretta della proprietà (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="f6f93-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="f6f93-116">La classe `MyOptions` viene aggiunta al contenitore di servizi con [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) e associata alla configurazione:</span><span class="sxs-lookup"><span data-stu-id="f6f93-116">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="f6f93-117">Il modello di pagina seguente usa l'[inserimento delle dipendenze del costruttore](xref:mvc/controllers/dependency-injection) con [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) per accedere alle impostazioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="f6f93-117">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="f6f93-118">Il file *appsettings.json* dell'esempio specifica i valori di `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="f6f93-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="f6f93-119">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="f6f93-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="f6f93-120">Se si usa un [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) personalizzato per caricare la configurazione delle opzioni da un file di impostazioni, verificare che il percorso base sia impostato correttamente:</span><span class="sxs-lookup"><span data-stu-id="f6f93-120">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="f6f93-121">L'impostazione esplicita del percorso base non è necessaria se si carica la configurazione delle opzioni dal file di impostazioni tramite [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="f6f93-121">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="f6f93-122">Configurare opzioni semplici con un delegato</span><span class="sxs-lookup"><span data-stu-id="f6f93-122">Configure simple options with a delegate</span></span>

<span data-ttu-id="f6f93-123">La configurazione di opzioni semplici con un delegato è illustrata nell'Esempio &num;2 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="f6f93-123">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="f6f93-124">Usare un delegato per impostare i valori delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="f6f93-124">Use a delegate to set options values.</span></span> <span data-ttu-id="f6f93-125">L'app di esempio usa la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="f6f93-125">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="f6f93-126">Nel codice seguente viene aggiunto un secondo servizio `IConfigureOptions<TOptions>` al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="f6f93-126">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="f6f93-127">Viene usato un delegato per configurare l'associazione a `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="f6f93-127">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="f6f93-128">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f6f93-128">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="f6f93-129">È possibile aggiungere più provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f6f93-129">You can add multiple configuration providers.</span></span> <span data-ttu-id="f6f93-130">I provider di configurazione sono disponibili in pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="f6f93-130">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="f6f93-131">Vengono applicati nell'ordine in cui sono registrati.</span><span class="sxs-lookup"><span data-stu-id="f6f93-131">They're applied in order that they're registered.</span></span>

<span data-ttu-id="f6f93-132">Ogni chiamata a [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) aggiunge un servizio `IConfigureOptions<TOptions>` al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="f6f93-132">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="f6f93-133">Nell'esempio precedente i valori di `Option1` e `Option2` sono entrambi specificati in *appsettings.json*, mentre i valori di `Option1` e `Option2` sono sottoposti a override dal delegato configurato.</span><span class="sxs-lookup"><span data-stu-id="f6f93-133">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="f6f93-134">Quando sono abilitati più servizi di configurazione, l'origine di configurazione più recente specificata *ha priorità* e imposta il valore di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f6f93-134">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="f6f93-135">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="f6f93-135">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="f6f93-136">Configurazione delle opzioni secondarie</span><span class="sxs-lookup"><span data-stu-id="f6f93-136">Suboptions configuration</span></span>

<span data-ttu-id="f6f93-137">La configurazione delle opzioni secondarie è illustrata nell'Esempio &num;3 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="f6f93-137">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="f6f93-138">Le app devono creare classi di opzioni che riguardano gruppi di funzionalità specifiche (classi) nell'app.</span><span class="sxs-lookup"><span data-stu-id="f6f93-138">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="f6f93-139">Le parti dell'app che richiedono valori di configurazione devono avere accesso solo ai valori di configurazione necessari.</span><span class="sxs-lookup"><span data-stu-id="f6f93-139">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="f6f93-140">Durante l'associazione delle opzioni alla configurazione, ogni proprietà del tipo di opzioni viene associata a una chiave di configurazione con formato `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-140">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="f6f93-141">Ad esempio, la proprietà `MyOptions.Option1` viene associata alla chiave `Option1`, che viene letta dalla proprietà `option1` in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f6f93-141">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="f6f93-142">Nel codice seguente viene aggiunto un terzo servizio `IConfigureOptions<TOptions>` al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="f6f93-142">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="f6f93-143">`MySubOptions` viene associato alla sezione `subsection` del file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f6f93-143">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="f6f93-144">Il metodo di estensione `GetSection` richiede il pacchetto NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="f6f93-144">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="f6f93-145">Se l'app usa il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive), il pacchetto è incluso automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f6f93-145">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="f6f93-146">Il file *appsettings.json* dell'esempio definisce un membro `subsection` con chiavi per `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="f6f93-146">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="f6f93-147">La classe `MySubOptions` definisce le proprietà `SubOption1` e `SubOption2` per contenere i valori delle opzioni secondarie (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="f6f93-147">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="f6f93-148">Il metodo `OnGet` del modello di pagina restituisce una stringa con i valori delle opzioni secondarie (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="f6f93-148">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="f6f93-149">Quando viene eseguita l'app, il metodo `OnGet` restituisce una stringa che mostra i valori delle classi delle opzioni secondarie:</span><span class="sxs-lookup"><span data-stu-id="f6f93-149">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="f6f93-150">Opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f6f93-150">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="f6f93-151">Le opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione sono illustrate nell'Esempio &num;4 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="f6f93-151">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="f6f93-152">Le opzioni possono essere rese disponibili in un modello di visualizzazione oppure inserendo `IOptions<TOptions>` direttamente in una visualizzazione (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="f6f93-152">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="f6f93-153">Per l'inserimento diretto, inserire `IOptions<MyOptions>` con una direttiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="f6f93-153">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="f6f93-154">Quando l'app viene eseguita, i valori delle opzioni sono visibili nella pagina di cui è stato eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="f6f93-154">When the app is run, the option values are shown in the rendered page:</span></span>

![I valori delle opzioni Option1: value1_from_json e Option2: -1 vengono caricati dal modello e tramite inserimento nella visualizzazione.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="f6f93-156">Ricaricare i dati di configurazione con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="f6f93-156">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="f6f93-157">Il ricaricamento dei dati di configurazione con `IOptionsSnapshot` è illustrato nell'Esempio &num;5 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="f6f93-157">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="f6f93-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supporta il ricaricamento delle opzioni con overhead di elaborazione minimo.</span><span class="sxs-lookup"><span data-stu-id="f6f93-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f6f93-159">Le opzioni vengono calcolate una volta per richiesta quando viene eseguito l'accesso e la memorizzazione nella cache per la durata della richiesta.</span><span class="sxs-lookup"><span data-stu-id="f6f93-159">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f6f93-160">`IOptionsSnapshot` è uno snapshot di [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e viene aggiornato automaticamente ogni volta che il monitoraggio attiva le modifiche in base alle modifiche dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="f6f93-160">`IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="f6f93-161">L'esempio seguente illustra la creazione di un nuovo `IOptionsSnapshot` dopo le modifiche ad *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="f6f93-161">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="f6f93-162">Più richieste al server restituiscono valori costanti forniti dal file *appsettings.json* fino a quando il file non viene modificato e la configurazione non viene ricaricata.</span><span class="sxs-lookup"><span data-stu-id="f6f93-162">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="f6f93-163">L'immagine seguente illustra i valori iniziali `option1` e `option2` caricati dal file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f6f93-163">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="f6f93-164">Modificare i valori nel file *appsettings.json* in `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-164">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="f6f93-165">Salvare il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f6f93-165">Save the *appsettings.json* file.</span></span> <span data-ttu-id="f6f93-166">Aggiornare il browser per visualizzare i valori delle opzioni aggiornati:</span><span class="sxs-lookup"><span data-stu-id="f6f93-166">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="f6f93-167">Supporto delle opzioni denominate con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="f6f93-167">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="f6f93-168">Il supporto delle opzioni denominate con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) è illustrato nell'Esempio &num;6 nell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="f6f93-168">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="f6f93-169">Il supporto delle *opzioni denominate* consente all'app di distinguere le configurazioni delle opzioni denominate.</span><span class="sxs-lookup"><span data-stu-id="f6f93-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="f6f93-170">Nell'app di esempio le opzioni denominate vengono dichiarate con [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) che a sua volta chiama il metodo di estensione [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure):</span><span class="sxs-lookup"><span data-stu-id="f6f93-170">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="f6f93-171">L'app di esempio accede alle opzioni denominate con [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="f6f93-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="f6f93-172">Eseguendo l'app di esempio, vengono restituite le opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="f6f93-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="f6f93-173">I valori `named_options_1` vengono specificati dalla configurazione, caricata dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f6f93-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="f6f93-174">I valori `named_options_2` sono specificati da:</span><span class="sxs-lookup"><span data-stu-id="f6f93-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="f6f93-175">Delegato `named_options_2` in `ConfigureServices` per `Option1`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="f6f93-176">Valore predefinito per `Option2` specificato dalla classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="f6f93-177">Configurare tutte le istanze delle opzioni denominate con il metodo [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall).</span><span class="sxs-lookup"><span data-stu-id="f6f93-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="f6f93-178">Il codice seguente configura `Option1` per tutte le istanze delle opzioni denominate con un valore comune.</span><span class="sxs-lookup"><span data-stu-id="f6f93-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="f6f93-179">Aggiungere manualmente il codice seguente al metodo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="f6f93-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="f6f93-180">L'esecuzione dell'app di esempio dopo l'aggiunta del codice produce il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="f6f93-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="f6f93-181">Tutte le opzioni sono istanze denominate.</span><span class="sxs-lookup"><span data-stu-id="f6f93-181">All options are named instances.</span></span> <span data-ttu-id="f6f93-182">Le istanze di `IConfigureOption` esistenti sono trattate come se fossero indirizzate all'istanza di `Options.DefaultName`, ovvero `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="f6f93-183">`IConfigureNamedOptions` implementa anche `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="f6f93-184">L'implementazione predefinita di [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origine riferimento](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) ha la logica per usare ogni opzione nel modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="f6f93-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="f6f93-185">L'opzione denominata `null` viene usata per avere come destinazione tutte le istanze denominate anziché un'istanza specifica denominata (questa convenzione è usata da [ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)).</span><span class="sxs-lookup"><span data-stu-id="f6f93-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="f6f93-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="f6f93-186">IPostConfigureOptions</span></span>

<span data-ttu-id="f6f93-187">Impostare la post-configurazione con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="f6f93-187">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="f6f93-188">La post-configurazione viene eseguita dopo la configurazione [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):</span><span class="sxs-lookup"><span data-stu-id="f6f93-188">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="f6f93-189">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) è disponibile per eseguire la post-configurazione delle opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="f6f93-189">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="f6f93-190">Usare [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) per eseguire la post-configurazione di tutte le istanze delle configurazioni denominate:</span><span class="sxs-lookup"><span data-stu-id="f6f93-190">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="f6f93-191">Factory, monitoraggio e memorizzazione nella cache delle opzioni</span><span class="sxs-lookup"><span data-stu-id="f6f93-191">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="f6f93-192">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) viene usata per le notifiche quando vengono modificate le istanze `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-192">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="f6f93-193">`IOptionsMonitor` supporta le opzioni ricaricabili, le notifiche delle modifiche e `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-193">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f6f93-194">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) è responsabile della creazione di nuove istanze delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="f6f93-194">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) is responsible for creating new options instances.</span></span> <span data-ttu-id="f6f93-195">Ha un unico metodo [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create).</span><span class="sxs-lookup"><span data-stu-id="f6f93-195">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="f6f93-196">L'implementazione predefinita accetta tutte le interfacce `IConfigureOptions` e `IPostConfigureOptions` registrate ed esegue tutte le configurazioni seguite dalle post-configurazioni.</span><span class="sxs-lookup"><span data-stu-id="f6f93-196">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="f6f93-197">Fa distinzione tra `IConfigureNamedOptions` e `IConfigureOptions` e chiama solo l'interfaccia appropriata.</span><span class="sxs-lookup"><span data-stu-id="f6f93-197">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="f6f93-198">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) viene usata da `IOptionsMonitor` per memorizzare nella cache le istanze `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-198">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="f6f93-199">`IOptionsMonitorCache` invalida le istanze delle opzioni nel monitoraggio in modo che il valore venga ricalcolato ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="f6f93-199">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="f6f93-200">È anche possibile inserire i valori manualmente con [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="f6f93-200">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="f6f93-201">Il metodo [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) viene usato quando tutte le istanze denominate devono essere ricreate su richiesta.</span><span class="sxs-lookup"><span data-stu-id="f6f93-201">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

::: moniker-end

## <a name="accessing-options-during-startup"></a><span data-ttu-id="f6f93-202">Accesso alle opzioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="f6f93-202">Accessing options during startup</span></span>

<span data-ttu-id="f6f93-203">`IOptions` può essere usata in `Startup.Configure` poiché i servizi vengono compilati prima dell'esecuzione del metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-203">`IOptions` can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

<span data-ttu-id="f6f93-204">`IOptions` non deve essere usata in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f6f93-204">`IOptions` shouldn't be used in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f6f93-205">Le opzioni potrebbero avere uno stato incoerente a causa dell'ordinamento delle registrazioni dei servizi.</span><span class="sxs-lookup"><span data-stu-id="f6f93-205">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f6f93-206">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f6f93-206">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
