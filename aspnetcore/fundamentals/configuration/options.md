---
title: Modello di opzioni in ASP.NET Core
author: guardrex
description: Informazioni su come usare il modello di opzioni per rappresentare gruppi di opzioni correlate nelle app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: 359bd438066aefcf572c91dacee99e85c0f10b1a
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207355"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="06271-103">Modello di opzioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06271-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="06271-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="06271-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="06271-105">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="06271-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="06271-106">Quando le [impostazioni di configurazione](xref:fundamentals/configuration/index) vengono isolate in base allo scenario in classi separate, l'app aderisce a due importanti principi di progettazione del software:</span><span class="sxs-lookup"><span data-stu-id="06271-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="06271-107">[Principio di segregazione delle interfacce (Interface Segregation Principle, ISP)](http://deviq.com/interface-segregation-principle/): gli scenari (classi) che dipendono dalle impostazioni di configurazione dipendono solo dalle impostazioni di configurazione che usano.</span><span class="sxs-lookup"><span data-stu-id="06271-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="06271-108">[Separazione delle competenze (Separation of Concerns, SoC)](http://deviq.com/separation-of-concerns/): le impostazioni di parti diverse dell'app non sono dipendenti o accoppiate l'una con l'altra.</span><span class="sxs-lookup"><span data-stu-id="06271-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="06271-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([come eseguire il download](xref:index#how-to-download-a-sample)) Questo articolo è più semplice da seguire con l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="06271-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="06271-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="06271-110">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="06271-111">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="06271-111">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="06271-112">Fare riferimento al [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="06271-112">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="06271-113">Aggiungere un riferimento al pacchetto [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="06271-113">Add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

## <a name="basic-options-configuration"></a><span data-ttu-id="06271-114">Configurazione delle opzioni di base</span><span class="sxs-lookup"><span data-stu-id="06271-114">Basic options configuration</span></span>

<span data-ttu-id="06271-115">La configurazione delle opzioni di base è illustrata nell'Esempio &num;1 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="06271-115">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="06271-116">Una classe di opzioni deve essere non astratta con un costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="06271-116">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="06271-117">La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="06271-117">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="06271-118">Sebbene l'impostazione dei valori predefiniti sia facoltativa, il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`.</span><span class="sxs-lookup"><span data-stu-id="06271-118">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="06271-119">`Option2` ha un valore impostato tramite l'inizializzazione diretta della proprietà (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="06271-119">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="06271-120">La classe `MyOptions` viene aggiunta al contenitore di servizi con [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) e associata alla configurazione:</span><span class="sxs-lookup"><span data-stu-id="06271-120">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="06271-121">Il modello di pagina seguente usa l'[inserimento delle dipendenze del costruttore](xref:mvc/controllers/dependency-injection) con [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) per accedere alle impostazioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="06271-121">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="06271-122">Il file *appsettings.json* dell'esempio specifica i valori di `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="06271-122">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="06271-123">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="06271-123">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="06271-124">Se si usa un [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) personalizzato per caricare la configurazione delle opzioni da un file di impostazioni, verificare che il percorso base sia impostato correttamente:</span><span class="sxs-lookup"><span data-stu-id="06271-124">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="06271-125">L'impostazione esplicita del percorso base non è necessaria se si carica la configurazione delle opzioni dal file di impostazioni tramite [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="06271-125">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="06271-126">Configurare opzioni semplici con un delegato</span><span class="sxs-lookup"><span data-stu-id="06271-126">Configure simple options with a delegate</span></span>

<span data-ttu-id="06271-127">La configurazione di opzioni semplici con un delegato è illustrata nell'Esempio &num;2 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="06271-127">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="06271-128">Usare un delegato per impostare i valori delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="06271-128">Use a delegate to set options values.</span></span> <span data-ttu-id="06271-129">L'app di esempio usa la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="06271-129">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="06271-130">Nel codice seguente viene aggiunto un secondo servizio `IConfigureOptions<TOptions>` al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="06271-130">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="06271-131">Viene usato un delegato per configurare l'associazione a `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="06271-131">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="06271-132">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="06271-132">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="06271-133">È possibile aggiungere più provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="06271-133">You can add multiple configuration providers.</span></span> <span data-ttu-id="06271-134">I provider di configurazione sono disponibili in pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="06271-134">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="06271-135">Vengono applicati nell'ordine in cui sono registrati.</span><span class="sxs-lookup"><span data-stu-id="06271-135">They're applied in the order that they're registered.</span></span>

<span data-ttu-id="06271-136">Ogni chiamata a [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) aggiunge un servizio `IConfigureOptions<TOptions>` al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="06271-136">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="06271-137">Nell'esempio precedente i valori di `Option1` e `Option2` sono entrambi specificati in *appsettings.json*, mentre i valori di `Option1` e `Option2` sono sottoposti a override dal delegato configurato.</span><span class="sxs-lookup"><span data-stu-id="06271-137">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="06271-138">Quando sono abilitati più servizi di configurazione, l'origine di configurazione più recente specificata *ha priorità* e imposta il valore di configurazione.</span><span class="sxs-lookup"><span data-stu-id="06271-138">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="06271-139">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="06271-139">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="06271-140">Configurazione delle opzioni secondarie</span><span class="sxs-lookup"><span data-stu-id="06271-140">Suboptions configuration</span></span>

<span data-ttu-id="06271-141">La configurazione delle opzioni secondarie è illustrata nell'Esempio &num;3 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="06271-141">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="06271-142">Le app devono creare classi di opzioni che riguardano gruppi di scenari (classi) specifici nell'app.</span><span class="sxs-lookup"><span data-stu-id="06271-142">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="06271-143">Le parti dell'app che richiedono valori di configurazione devono avere accesso solo ai valori di configurazione necessari.</span><span class="sxs-lookup"><span data-stu-id="06271-143">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="06271-144">Durante l'associazione delle opzioni alla configurazione, ogni proprietà del tipo di opzioni viene associata a una chiave di configurazione con formato `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="06271-144">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="06271-145">Ad esempio, la proprietà `MyOptions.Option1` viene associata alla chiave `Option1`, che viene letta dalla proprietà `option1` in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="06271-145">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="06271-146">Nel codice seguente viene aggiunto un terzo servizio `IConfigureOptions<TOptions>` al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="06271-146">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="06271-147">`MySubOptions` viene associato alla sezione `subsection` del file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="06271-147">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="06271-148">Il metodo di estensione `GetSection` richiede il pacchetto NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="06271-148">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="06271-149">Se l'app usa il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive), il pacchetto è incluso automaticamente.</span><span class="sxs-lookup"><span data-stu-id="06271-149">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="06271-150">Il file *appsettings.json* dell'esempio definisce un membro `subsection` con chiavi per `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="06271-150">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="06271-151">La classe `MySubOptions` definisce le proprietà `SubOption1` e `SubOption2` per contenere i valori delle opzioni (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="06271-151">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="06271-152">Il metodo `OnGet` del modello di pagina restituisce una stringa con i valori delle opzioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="06271-152">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="06271-153">Quando viene eseguita l'app, il metodo `OnGet` restituisce una stringa che mostra i valori delle classi delle opzioni secondarie:</span><span class="sxs-lookup"><span data-stu-id="06271-153">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="06271-154">Opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione</span><span class="sxs-lookup"><span data-stu-id="06271-154">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="06271-155">Le opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione sono illustrate nell'Esempio &num;4 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="06271-155">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="06271-156">Le opzioni possono essere rese disponibili in un modello di visualizzazione oppure inserendo `IOptions<TOptions>` direttamente in una visualizzazione (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="06271-156">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="06271-157">Per l'inserimento diretto, inserire `IOptions<MyOptions>` con una direttiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="06271-157">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="06271-158">Quando l'app viene eseguita, i valori delle opzioni sono visibili nella pagina di cui è stato eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="06271-158">When the app is run, the options values are shown in the rendered page:</span></span>

![I valori delle opzioni Option1: value1_from_json e Option2: -1 vengono caricati dal modello e tramite inserimento nella visualizzazione.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="06271-160">Ricaricare i dati di configurazione con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="06271-160">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="06271-161">Il ricaricamento dei dati di configurazione con `IOptionsSnapshot` è illustrato nell'Esempio &num;5 dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="06271-161">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="06271-162">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supporta il ricaricamento delle opzioni con overhead di elaborazione minimo.</span><span class="sxs-lookup"><span data-stu-id="06271-162">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="06271-163">Le opzioni vengono calcolate una volta per richiesta quando viene eseguito l'accesso e la memorizzazione nella cache per la durata della richiesta.</span><span class="sxs-lookup"><span data-stu-id="06271-163">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="06271-164">`IOptionsSnapshot` è uno snapshot di [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e viene aggiornato automaticamente ogni volta che il monitoraggio attiva le modifiche in base alle modifiche dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="06271-164">`IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="06271-165">L'esempio seguente illustra la creazione di un nuovo `IOptionsSnapshot` dopo le modifiche ad *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="06271-165">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="06271-166">Più richieste al server restituiscono valori costanti forniti dal file *appsettings.json* fino a quando il file non viene modificato e la configurazione non viene ricaricata.</span><span class="sxs-lookup"><span data-stu-id="06271-166">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="06271-167">L'immagine seguente illustra i valori iniziali `option1` e `option2` caricati dal file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="06271-167">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="06271-168">Modificare i valori nel file *appsettings.json* in `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="06271-168">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="06271-169">Salvare il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="06271-169">Save the *appsettings.json* file.</span></span> <span data-ttu-id="06271-170">Aggiornare il browser per visualizzare i valori delle opzioni aggiornati:</span><span class="sxs-lookup"><span data-stu-id="06271-170">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="06271-171">Supporto delle opzioni denominate con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="06271-171">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="06271-172">Il supporto delle opzioni denominate con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) è illustrato nell'Esempio &num;6 nell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="06271-172">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="06271-173">Il supporto delle *opzioni denominate* consente all'app di distinguere le configurazioni delle opzioni denominate.</span><span class="sxs-lookup"><span data-stu-id="06271-173">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="06271-174">Nell'app di esempio le opzioni denominate vengono dichiarate con [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) che a sua volta chiama il metodo di estensione [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure):</span><span class="sxs-lookup"><span data-stu-id="06271-174">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="06271-175">L'app di esempio accede alle opzioni denominate con [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="06271-175">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="06271-176">Eseguendo l'app di esempio, vengono restituite le opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="06271-176">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="06271-177">I valori `named_options_1` vengono specificati dalla configurazione, caricata dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="06271-177">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="06271-178">I valori `named_options_2` sono specificati da:</span><span class="sxs-lookup"><span data-stu-id="06271-178">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="06271-179">Delegato `named_options_2` in `ConfigureServices` per `Option1`.</span><span class="sxs-lookup"><span data-stu-id="06271-179">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="06271-180">Valore predefinito per `Option2` specificato dalla classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="06271-180">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="06271-181">Configurare tutte le opzioni con il metodo ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="06271-181">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="06271-182">Configurare tutte le istanze delle opzioni con il metodo [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall).</span><span class="sxs-lookup"><span data-stu-id="06271-182">Configure all options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="06271-183">Il codice seguente configura `Option1` per tutte le istanze di configurazione con un valore comune.</span><span class="sxs-lookup"><span data-stu-id="06271-183">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="06271-184">Aggiungere manualmente il codice seguente al metodo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="06271-184">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="06271-185">L'esecuzione dell'app di esempio dopo l'aggiunta del codice produce il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="06271-185">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="06271-186">Tutte le opzioni sono istanze denominate.</span><span class="sxs-lookup"><span data-stu-id="06271-186">All options are named instances.</span></span> <span data-ttu-id="06271-187">Le istanze di `IConfigureOption` esistenti sono trattate come se fossero indirizzate all'istanza di `Options.DefaultName`, ovvero `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="06271-187">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="06271-188">`IConfigureNamedOptions` implementa anche `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="06271-188">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="06271-189">L'implementazione predefinita di [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origine riferimento](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) ha la logica per usare ogni opzione nel modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="06271-189">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="06271-190">L'opzione denominata `null` viene usata per avere come destinazione tutte le istanze denominate anziché un'istanza specifica denominata (questa convenzione è usata da [ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)).</span><span class="sxs-lookup"><span data-stu-id="06271-190">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="06271-191">Convalida delle opzioni</span><span class="sxs-lookup"><span data-stu-id="06271-191">Options validation</span></span>

<span data-ttu-id="06271-192">La convalida le opzioni consente di convalidare le opzioni quando vengono configurate.</span><span class="sxs-lookup"><span data-stu-id="06271-192">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="06271-193">Chiamare `Validate` con un metodo di convalida che restituisce `true` se le opzioni sono valide e `false` se non sono valide:</span><span class="sxs-lookup"><span data-stu-id="06271-193">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="06271-194">L'esempio precedente imposta l'istanza di opzioni denominata su `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="06271-194">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="06271-195">L'istanza di opzioni predefinita è `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="06271-195">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="06271-196">La convalida viene eseguita quando viene creata l'istanza di opzioni.</span><span class="sxs-lookup"><span data-stu-id="06271-196">Validation runs when the options instance is created.</span></span> <span data-ttu-id="06271-197">L'istanza di opzioni supera sicuramente la convalida al primo accesso.</span><span class="sxs-lookup"><span data-stu-id="06271-197">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06271-198">La convalida delle opzioni non protegge da eventuali modifiche dopo la configurazione e la convalida iniziali delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="06271-198">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="06271-199">Il metodo `Validate` accetta `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="06271-199">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="06271-200">Per personalizzare completamente la convalida, implementare `IValidateOptions<TOptions>`, che consente:</span><span class="sxs-lookup"><span data-stu-id="06271-200">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="06271-201">La convalida di più tipi di opzioni: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="06271-201">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="06271-202">La convalida che dipende da un altro tipo di opzione: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="06271-202">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span></span>

<span data-ttu-id="06271-203">`IValidateOptions` convalida:</span><span class="sxs-lookup"><span data-stu-id="06271-203">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="06271-204">Un'istanza di opzioni denominata specifica.</span><span class="sxs-lookup"><span data-stu-id="06271-204">A specific named options instance.</span></span>
* <span data-ttu-id="06271-205">Tutte le opzioni quando `name` è `null`.</span><span class="sxs-lookup"><span data-stu-id="06271-205">All options when `name` is `null`.</span></span>

<span data-ttu-id="06271-206">Restituire `ValidateOptionsResult` dall'implementazione dell'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="06271-206">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="06271-207">La convalida eager (esito negativo rapido all'avvio) e la convalida in base alle annotazioni dei dati sono pianificate per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="06271-207">Eager validation (fail fast at startup) and data annotation-based validation are scheduled for a future release.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a><span data-ttu-id="06271-208">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="06271-208">IPostConfigureOptions</span></span>

<span data-ttu-id="06271-209">Impostare la post-configurazione con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="06271-209">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="06271-210">La post-configurazione viene eseguita dopo la configurazione [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):</span><span class="sxs-lookup"><span data-stu-id="06271-210">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="06271-211">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) è disponibile per eseguire la post-configurazione delle opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="06271-211">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="06271-212">Usare [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) per eseguire la post-configurazione di tutte le istanze di configurazione:</span><span class="sxs-lookup"><span data-stu-id="06271-212">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="06271-213">Factory, monitoraggio e memorizzazione nella cache delle opzioni</span><span class="sxs-lookup"><span data-stu-id="06271-213">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="06271-214">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) viene usata per le notifiche quando vengono modificate le istanze `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="06271-214">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="06271-215">`IOptionsMonitor` supporta le opzioni ricaricabili, le notifiche delle modifiche e `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="06271-215">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="06271-216">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) è responsabile della creazione di nuove istanze delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="06271-216">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) is responsible for creating new options instances.</span></span> <span data-ttu-id="06271-217">Ha un unico metodo [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create).</span><span class="sxs-lookup"><span data-stu-id="06271-217">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="06271-218">L'implementazione predefinita accetta tutte le interfacce `IConfigureOptions` e `IPostConfigureOptions` registrate ed esegue tutte le configurazioni seguite dalle post-configurazioni.</span><span class="sxs-lookup"><span data-stu-id="06271-218">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="06271-219">Fa distinzione tra `IConfigureNamedOptions` e `IConfigureOptions` e chiama solo l'interfaccia appropriata.</span><span class="sxs-lookup"><span data-stu-id="06271-219">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="06271-220">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) viene usata da `IOptionsMonitor` per memorizzare nella cache le istanze `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="06271-220">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="06271-221">`IOptionsMonitorCache` invalida le istanze delle opzioni nel monitoraggio in modo che il valore venga ricalcolato ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="06271-221">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="06271-222">È anche possibile inserire i valori manualmente con [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="06271-222">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="06271-223">Il metodo [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) viene usato quando tutte le istanze denominate devono essere ricreate su richiesta.</span><span class="sxs-lookup"><span data-stu-id="06271-223">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

::: moniker-end

## <a name="accessing-options-during-startup"></a><span data-ttu-id="06271-224">Accesso alle opzioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="06271-224">Accessing options during startup</span></span>

<span data-ttu-id="06271-225">`IOptions` può essere usata in `Startup.Configure` poiché i servizi vengono compilati prima dell'esecuzione del metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="06271-225">`IOptions` can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

<span data-ttu-id="06271-226">`IOptions` non deve essere usata in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="06271-226">`IOptions` shouldn't be used in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="06271-227">Le opzioni potrebbero avere uno stato incoerente a causa dell'ordinamento delle registrazioni dei servizi.</span><span class="sxs-lookup"><span data-stu-id="06271-227">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="06271-228">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="06271-228">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
