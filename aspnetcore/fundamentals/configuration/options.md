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
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="5d6da-103">Serie di opzioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5d6da-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="5d6da-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5d6da-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5d6da-105">Il modello di opzioni Usa le classi di opzioni per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="5d6da-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="5d6da-106">Quando le impostazioni di configurazione sono isolate dalla funzionalità in classi di opzioni distinte, l'app conforme alle due principi di progettazione del software importanti:</span><span class="sxs-lookup"><span data-stu-id="5d6da-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="5d6da-107">Il [interfaccia separazione principio (ISP)](http://deviq.com/interface-segregation-principle/): funzionalità (classi) che dipendono dalle impostazioni di configurazione dipendono solo le impostazioni di configurazione che usano.</span><span class="sxs-lookup"><span data-stu-id="5d6da-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="5d6da-108">[La separazione dei compiti](http://deviq.com/separation-of-concerns/): le impostazioni per le diverse parti dell'app non sono dipendenti o a uno a altro.</span><span class="sxs-lookup"><span data-stu-id="5d6da-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="5d6da-109">[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) in questo articolo è più facile da seguire con l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="5d6da-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="5d6da-110">Configurazione delle opzioni di base</span><span class="sxs-lookup"><span data-stu-id="5d6da-110">Basic options configuration</span></span>

<span data-ttu-id="5d6da-111">Configurazione delle opzioni di base viene illustrata come esempio &num;1 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5d6da-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5d6da-112">Classe di opzioni deve essere non astratto con costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="5d6da-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="5d6da-113">La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="5d6da-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="5d6da-114">Impostazione dei valori predefiniti è facoltativo, ma il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5d6da-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="5d6da-115">`Option2`il valore predefinito impostato inizializzando direttamente la proprietà è (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5d6da-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="5d6da-116">Il `MyOptions` classe viene aggiunto al contenitore del servizio con [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) e associato alla configurazione:</span><span class="sxs-lookup"><span data-stu-id="5d6da-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="5d6da-117">La seguente pagina modello Usa [inserimento di dipendenze di costruttore](xref:fundamentals/dependency-injection#what-is-dependency-injection) con [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) per accedere alle impostazioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5d6da-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="5d6da-118">L'esempio *appSettings. JSON* file specifica i valori per `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="5d6da-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="5d6da-119">Quando si esegue l'app, il modello di pagina `OnGet` metodo restituisce una stringa contenente i valori di classe di opzione:</span><span class="sxs-lookup"><span data-stu-id="5d6da-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="5d6da-120">Configurare le opzioni semplice con un delegato</span><span class="sxs-lookup"><span data-stu-id="5d6da-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="5d6da-121">Configurazione delle opzioni semplice con un delegato viene illustrata come esempio &num;2 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5d6da-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5d6da-122">Per impostare i valori delle opzioni, utilizzare un delegato.</span><span class="sxs-lookup"><span data-stu-id="5d6da-122">Use a delegate to set options values.</span></span> <span data-ttu-id="5d6da-123">L'applicazione di esempio utilizza il `MyOptionsWithDelegateConfig` classe (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="5d6da-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="5d6da-124">Nel codice seguente, una seconda `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="5d6da-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="5d6da-125">Usa un delegato per configurare l'associazione con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="5d6da-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="5d6da-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5d6da-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="5d6da-127">È possibile aggiungere più provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5d6da-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="5d6da-128">Provider di configurazione sono disponibili in pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="5d6da-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="5d6da-129">Vengono applicati nell'ordine in cui sono registrati.</span><span class="sxs-lookup"><span data-stu-id="5d6da-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="5d6da-130">Ogni chiamata a [configura&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) aggiunge un `IConfigureOptions<TOptions>` servizio al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="5d6da-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="5d6da-131">Nell'esempio precedente, i valori di `Option1` e `Option2` sono entrambi specificati nel *appSettings. JSON*, ma i valori di `Option1` e `Option2` vengono sottoposti a override dal delegato configurato.</span><span class="sxs-lookup"><span data-stu-id="5d6da-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="5d6da-132">Quando più di un servizio di configurazione è abilitato, l'ultima origine di configurazione specificato *wins* e imposta il valore di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5d6da-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="5d6da-133">Quando si esegue l'app, il modello di pagina `OnGet` metodo restituisce una stringa contenente i valori di classe di opzione:</span><span class="sxs-lookup"><span data-stu-id="5d6da-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="5d6da-134">Configurazione le opzioni secondarie</span><span class="sxs-lookup"><span data-stu-id="5d6da-134">Suboptions configuration</span></span>

<span data-ttu-id="5d6da-135">Configurazione le opzioni secondarie viene illustrato come esempio &num;3 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5d6da-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5d6da-136">Le app devono creare classi di opzioni che riguardano i gruppi di funzionalità specifiche (classi) nell'app.</span><span class="sxs-lookup"><span data-stu-id="5d6da-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="5d6da-137">Parti dell'app che richiedono valori di configurazione solo deve accedere ai valori di configurazione che usano.</span><span class="sxs-lookup"><span data-stu-id="5d6da-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="5d6da-138">Quando l'associazione le opzioni di configurazione, ogni proprietà nel tipo di opzioni è associata a una chiave di configurazione del modulo `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="5d6da-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="5d6da-139">Ad esempio, il `MyOptions.Option1` proprietà è associata alla chiave `Option1`, che viene letto dal `option1` proprietà *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="5d6da-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="5d6da-140">Nel codice seguente, una terza `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="5d6da-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="5d6da-141">Associa `MySubOptions` alla sezione `subsection` del *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="5d6da-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="5d6da-142">Il `GetSection` metodo di estensione richiede il [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="5d6da-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="5d6da-143">Se l'app Usa la [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, il pacchetto viene incluso automaticamente.</span><span class="sxs-lookup"><span data-stu-id="5d6da-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="5d6da-144">L'esempio *appSettings. JSON* file definisce un `subsection` membro con le chiavi per `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="5d6da-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="5d6da-145">Il `MySubOptions` classe definisce le proprietà, `SubOption1` e `SubOption2`, per contenere i valori dell'opzione secondaria (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5d6da-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="5d6da-146">Il modello di pagina `OnGet` metodo restituisce una stringa con i valori dell'opzione secondaria (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5d6da-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="5d6da-147">Quando si esegue l'app, il `OnGet` metodo restituisce una stringa contenente l'opzione secondaria di valori di classe:</span><span class="sxs-lookup"><span data-stu-id="5d6da-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="5d6da-148">Opzioni fornite da un modello di visualizzazione o con inserimento diretto di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="5d6da-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="5d6da-149">Opzioni fornite da un modello di visualizzazione o con inserimento diretto di visualizzazione viene illustrato come esempio &num;4 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5d6da-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5d6da-150">È possibile specificare le opzioni in un modello di visualizzazione o inserendo `IOptions<TOptions>` direttamente in una vista (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5d6da-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="5d6da-151">Per l'aggiunta diretta, inserire `IOptions<MyOptions>` con un `@inject` direttiva:</span><span class="sxs-lookup"><span data-stu-id="5d6da-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="5d6da-152">Quando si esegue l'app, i valori delle opzioni presenti nella pagina sottoposta a rendering:</span><span class="sxs-lookup"><span data-stu-id="5d6da-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Opzioni valori opzione1: value1_from_json e opzione2: -1 vengono caricati dal modello e da inserimento nella vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="5d6da-154">Ricaricare i dati di configurazione con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="5d6da-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="5d6da-155">Ricaricare i dati di configurazione con `IOptionsSnapshot` è illustrato nell'esempio &num;5 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5d6da-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5d6da-156">*Richiede ASP.NET Core 1.1 o successiva.*</span><span class="sxs-lookup"><span data-stu-id="5d6da-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="5d6da-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supporta il ricaricamento di opzioni con minimo l'overhead di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="5d6da-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="5d6da-158">In ASP.NET Core 1.1 `IOptionsSnapshot` è uno snapshot di [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e gli aggiornamenti ogni volta che il monitoraggio viene attivato automaticamente le modifiche in base alla modifica origine dati.</span><span class="sxs-lookup"><span data-stu-id="5d6da-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="5d6da-159">In ASP.NET Core 2.0 e versioni successive, le opzioni vengono calcolate una volta per ogni richiesta quando si accede e memorizzato nella cache per la durata della richiesta.</span><span class="sxs-lookup"><span data-stu-id="5d6da-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="5d6da-160">Nell'esempio seguente viene illustrato come un nuovo `IOptionsSnapshot` viene creato dopo *appSettings. JSON* modifiche (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="5d6da-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="5d6da-161">Più richieste al server di restituiranno valori costanti forniti dal *appSettings. JSON* file fino a quando non viene modificato il file e ricaricamenti configurazione.</span><span class="sxs-lookup"><span data-stu-id="5d6da-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="5d6da-162">L'immagine seguente mostra iniziale `option1` e `option2` caricati da valori di *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="5d6da-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="5d6da-163">Modificare i valori di *appSettings. JSON* file `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="5d6da-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="5d6da-164">Salvare il *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="5d6da-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="5d6da-165">Aggiornare il browser per vedere che vengono aggiornati i valori delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="5d6da-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="5d6da-166">Opzioni di supporto con IConfigureNamedOptions denominato</span><span class="sxs-lookup"><span data-stu-id="5d6da-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="5d6da-167">Denominato opzioni di supporto con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) viene illustrato come esempio &num;6 il [app di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5d6da-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5d6da-168">*È necessario ASP.NET 2.0 o versione successiva di base.*</span><span class="sxs-lookup"><span data-stu-id="5d6da-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="5d6da-169">*Opzioni denominato* supporto consente all'applicazione di distinguere tra le configurazioni di opzioni denominata.</span><span class="sxs-lookup"><span data-stu-id="5d6da-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="5d6da-170">Nell'applicazione di esempio, le opzioni denominate vengono dichiarate con la [ConfigureNamedOptions&lt;TOptions&gt;. Configurare](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) metodo:</span><span class="sxs-lookup"><span data-stu-id="5d6da-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="5d6da-171">L'app di esempio accede le opzioni denominate con [IOptionsSnapshot&lt;TOptions&gt;. Ottenere](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5d6da-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="5d6da-172">Esecuzione dell'applicazione di esempio, vengono restituite le opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="5d6da-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="5d6da-173">`named_options_1`vengono forniti i valori dalla configurazione, che vengono caricati dal *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="5d6da-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="5d6da-174">`named_options_2`i valori vengono forniti da:</span><span class="sxs-lookup"><span data-stu-id="5d6da-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="5d6da-175">Il `named_options_2` delegare `ConfigureServices` per `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5d6da-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="5d6da-176">Il valore predefinito per `Option2` fornito dalla `MyOptions` classe.</span><span class="sxs-lookup"><span data-stu-id="5d6da-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="5d6da-177">Configurare tutte le istanze denominate opzioni con il [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) metodo.</span><span class="sxs-lookup"><span data-stu-id="5d6da-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="5d6da-178">Nel codice seguente viene configurato `Option1` per tutte le istanze di configurazione con un valore comune denominate.</span><span class="sxs-lookup"><span data-stu-id="5d6da-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="5d6da-179">Aggiungere il codice seguente manualmente al `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="5d6da-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="5d6da-180">Esecuzione dell'applicazione di esempio dopo l'aggiunta del codice produce il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="5d6da-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="5d6da-181">In ASP.NET Core 2.0 e versioni successive, tutte le opzioni sono istanze denominate.</span><span class="sxs-lookup"><span data-stu-id="5d6da-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="5d6da-182">Esistente `IConfigureOption` istanze vengono considerate come destinazione il `Options.DefaultName` istanza, ovvero `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="5d6da-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="5d6da-183">`IConfigureNamedOptions`implementa inoltre `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="5d6da-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="5d6da-184">L'implementazione predefinita del [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origine riferimento](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) ha la logica per utilizzarle in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="5d6da-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="5d6da-185">Il `null` denominata opzione viene utilizzata per tutte le istanze denominate, invece di una specifica istanza denominata di destinazione ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) utilizzano questa convenzione).</span><span class="sxs-lookup"><span data-stu-id="5d6da-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="5d6da-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="5d6da-186">IPostConfigureOptions</span></span>

<span data-ttu-id="5d6da-187">*È necessario ASP.NET 2.0 o versione successiva di base.*</span><span class="sxs-lookup"><span data-stu-id="5d6da-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="5d6da-188">Impostare postconfiguration con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="5d6da-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="5d6da-189">Post-configurazione viene eseguita dopo che tutti [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) viene eseguita la configurazione:</span><span class="sxs-lookup"><span data-stu-id="5d6da-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5d6da-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) è disponibile per post-configurare le opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="5d6da-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5d6da-191">Utilizzare [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) di post-configurare tutte denominato istanze di configurazione:</span><span class="sxs-lookup"><span data-stu-id="5d6da-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="5d6da-192">Factory di opzioni, il monitoraggio e della cache</span><span class="sxs-lookup"><span data-stu-id="5d6da-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="5d6da-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) viene utilizzato per notifiche quando `TOptions` istanze di modifica.</span><span class="sxs-lookup"><span data-stu-id="5d6da-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="5d6da-194">`IOptionsMonitor`Opzioni reloadable supporta notifiche di modifica, e `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="5d6da-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="5d6da-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (Core ASP.NET 2.0 o versione successiva) è responsabile della creazione di nuove istanze di opzioni.</span><span class="sxs-lookup"><span data-stu-id="5d6da-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="5d6da-196">Dispone di un singolo [crea](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) metodo.</span><span class="sxs-lookup"><span data-stu-id="5d6da-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="5d6da-197">L'implementazione predefinita accetta tutti registrato `IConfigureOptions` e `IPostConfigureOptions` ed esegue tutti il Configura prima, seguita dalla post-Configura.</span><span class="sxs-lookup"><span data-stu-id="5d6da-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="5d6da-198">Distingue tra `IConfigureNamedOptions` e `IConfigureOptions` e chiama solo l'interfaccia appropriata.</span><span class="sxs-lookup"><span data-stu-id="5d6da-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="5d6da-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (Core ASP.NET 2.0 o versione successiva) viene utilizzato da `IOptionsMonitor` cache `TOptions` istanze.</span><span class="sxs-lookup"><span data-stu-id="5d6da-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="5d6da-200">Il `IOptionsMonitorCache` invalida le istanze di opzioni del monitoraggio in modo che il valore viene ricalcolato ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="5d6da-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="5d6da-201">I valori possono essere manualmente introdotte anche da [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="5d6da-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="5d6da-202">Il [deselezionare](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) metodo viene utilizzato per tutte le istanze denominate devono essere ricreate su richiesta.</span><span class="sxs-lookup"><span data-stu-id="5d6da-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="5d6da-203">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="5d6da-203">See also</span></span>

* [<span data-ttu-id="5d6da-204">Configurazione</span><span class="sxs-lookup"><span data-stu-id="5d6da-204">Configuration</span></span>](xref:fundamentals/configuration/index)
