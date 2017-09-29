---
title: Configurazione in ASP.NET Core
author: rick-anderson
description: "Informazioni su come usare l'API di configurazione per configurare un'app di ASP.NET Core da più origini."
keywords: ASP.NET Core, configurazione, JSON, configurazione
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: 379030df4ca91a38fce251aeaab9c5dfaf11e915
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="2e625-104">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e625-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="2e625-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [feed di Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2e625-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2e625-106">L'API di configurazione fornisce un modo per configurare un'app in base a un elenco di coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="2e625-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="2e625-107">Configurazione è di lettura in fase di esecuzione da più origini.</span><span class="sxs-lookup"><span data-stu-id="2e625-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="2e625-108">Le coppie nome-valore possono essere raggruppate in una gerarchia a più livello.</span><span class="sxs-lookup"><span data-stu-id="2e625-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="2e625-109">Sono disponibili provider di configurazione per:</span><span class="sxs-lookup"><span data-stu-id="2e625-109">There are configuration providers for:</span></span>

* <span data-ttu-id="2e625-110">Formati di file (INI, JSON e XML)</span><span class="sxs-lookup"><span data-stu-id="2e625-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="2e625-111">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="2e625-111">Command-line arguments</span></span>
* <span data-ttu-id="2e625-112">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="2e625-112">Environment variables</span></span>
* <span data-ttu-id="2e625-113">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="2e625-113">In-memory .NET objects</span></span>
* <span data-ttu-id="2e625-114">Un archivio utente crittografato</span><span class="sxs-lookup"><span data-stu-id="2e625-114">An encrypted user store</span></span>
* [<span data-ttu-id="2e625-115">Insieme di credenziali chiave di Azure</span><span class="sxs-lookup"><span data-stu-id="2e625-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="2e625-116">Provider personalizzati, cui installa o crea</span><span class="sxs-lookup"><span data-stu-id="2e625-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="2e625-117">Ogni valore di configurazione esegue il mapping a una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="2e625-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="2e625-118">Supporto di associazione predefinita per deserializzare le impostazioni in un oggetto personalizzato [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) oggetto (una classe .NET semplice con proprietà).</span><span class="sxs-lookup"><span data-stu-id="2e625-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="2e625-119">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="2e625-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="2e625-120">Configurazione semplice</span><span class="sxs-lookup"><span data-stu-id="2e625-120">Simple configuration</span></span>

<span data-ttu-id="2e625-121">La seguente applicazione console utilizza il provider di configurazione JSON:</span><span class="sxs-lookup"><span data-stu-id="2e625-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="2e625-122">L'app legge e visualizza le impostazioni di configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="2e625-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="2e625-123">Configurazione è costituita da un elenco gerarchico di coppie nome-valore in cui i nodi sono separati dai due punti.</span><span class="sxs-lookup"><span data-stu-id="2e625-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="2e625-124">Per recuperare un valore, accedere il `Configuration` un indicizzatore con la corrispondente chiave dell'elemento:</span><span class="sxs-lookup"><span data-stu-id="2e625-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="2e625-125">Per lavorare con le matrici nelle origini configurazione in formato JSON, utilizzare un indice della matrice come parte della stringa separate da virgola.</span><span class="sxs-lookup"><span data-stu-id="2e625-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="2e625-126">Nell'esempio seguente ottiene il nome del primo elemento nel passaggio precedente `wizards` matrice:</span><span class="sxs-lookup"><span data-stu-id="2e625-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="2e625-127">Coppie nome-valore scritte incorporato in `Configuration` provider sono **non** persistente, tuttavia, è possibile creare un provider personalizzato che salva i valori.</span><span class="sxs-lookup"><span data-stu-id="2e625-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="2e625-128">Vedere [provider di configurazione personalizzato](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="2e625-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="2e625-129">L'esempio precedente Usa l'indicizzatore di configurazione per leggere i valori.</span><span class="sxs-lookup"><span data-stu-id="2e625-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="2e625-130">Per la configurazione di accesso di fuori di `Startup`, utilizzare il [modello opzioni](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="2e625-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="2e625-131">Il *modello opzioni* è illustrato più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="2e625-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="2e625-132">È in genere hanno diverse impostazioni di configurazione per gli ambienti diversi, ad esempio sviluppo, test e produzione.</span><span class="sxs-lookup"><span data-stu-id="2e625-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="2e625-133">Il `CreateDefaultBuilder` il metodo di estensione in un'app di ASP.NET Core 2. x (o tramite `AddJsonFile` e `AddEnvironmentVariables` direttamente in un'app di ASP.NET Core 1. x) aggiunge dei provider di configurazione per la lettura dei file JSON e sistema origini configurazione:</span><span class="sxs-lookup"><span data-stu-id="2e625-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="2e625-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="2e625-134">*appsettings.json*</span></span>
* <span data-ttu-id="2e625-135">*appSettings. \<EnvironmentName > JSON*</span><span class="sxs-lookup"><span data-stu-id="2e625-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="2e625-136">variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="2e625-136">environment variables</span></span>

<span data-ttu-id="2e625-137">Vedere [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) per una spiegazione dei parametri.</span><span class="sxs-lookup"><span data-stu-id="2e625-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="2e625-138">`reloadOnChange`è supportato solo in ASP.NET Core 1.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="2e625-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="2e625-139">Origini di configurazione vengono letti nell'ordine in che cui vengono specificati.</span><span class="sxs-lookup"><span data-stu-id="2e625-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="2e625-140">Nel codice precedente, le variabili di ambiente vengono lette ultima.</span><span class="sxs-lookup"><span data-stu-id="2e625-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="2e625-141">Tutti i valori di configurazione impostati tramite l'ambiente di sostituire quelli impostati nei due provider precedente.</span><span class="sxs-lookup"><span data-stu-id="2e625-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="2e625-142">L'ambiente è in genere impostato su uno dei `Development`, `Staging`, o `Production`.</span><span class="sxs-lookup"><span data-stu-id="2e625-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="2e625-143">Vedere [utilizzo di più ambienti](environments.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="2e625-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="2e625-144">Considerazioni sulla configurazione di:</span><span class="sxs-lookup"><span data-stu-id="2e625-144">Configuration considerations:</span></span>

* <span data-ttu-id="2e625-145">`IOptionsSnapshot`possibile ricaricare i dati di configurazione in caso di modifiche.</span><span class="sxs-lookup"><span data-stu-id="2e625-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="2e625-146">Utilizzare `IOptionsSnapshot` se è necessario ricaricare i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="2e625-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="2e625-147">Vedere [IOptionsSnapshot](#ioptionssnapshot) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="2e625-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="2e625-148">Le chiavi di configurazione vengono fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="2e625-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="2e625-149">Una procedura consigliata è specificare le variabili di ambiente per ultimo, in modo che l'ambiente locale può eseguire l'override di qualsiasi elemento impostati nei file di configurazione distribuita.</span><span class="sxs-lookup"><span data-stu-id="2e625-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="2e625-150">**Mai** archiviare password o altri dati sensibili nel codice di provider di configurazione o nel file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="2e625-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="2e625-151">Non è possibile utilizzare i segreti di produzione nell'ambiente di sviluppo o ambienti di test.</span><span class="sxs-lookup"><span data-stu-id="2e625-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="2e625-152">Specificare invece i segreti di fuori della struttura del progetto, in modo che non possono accidentalmente salvate nel repository.</span><span class="sxs-lookup"><span data-stu-id="2e625-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="2e625-153">Altre informazioni, vedere [utilizzo di più ambienti](environments.md) e la gestione di [archiviazione sicura di segreti dell'app durante lo sviluppo](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="2e625-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="2e625-154">Se `:` non può essere utilizzato in variabili di ambiente del sistema, sostituire `:` con `__` (doppio carattere di sottolineatura).</span><span class="sxs-lookup"><span data-stu-id="2e625-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="2e625-155">Utilizzando le opzioni e gli oggetti di configurazione</span><span class="sxs-lookup"><span data-stu-id="2e625-155">Using Options and configuration objects</span></span>

<span data-ttu-id="2e625-156">Il modello di opzioni Usa le classi di opzioni personalizzate per rappresentare un gruppo di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="2e625-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="2e625-157">Si consiglia di creare classi separate per ogni funzionalità all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="2e625-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="2e625-158">Classi disaccoppiate seguono:</span><span class="sxs-lookup"><span data-stu-id="2e625-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="2e625-159">Il [interfaccia separazione principio (ISP)](http://deviq.com/interface-segregation-principle/) : classi dipendono solo usano le impostazioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="2e625-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="2e625-160">[La separazione dei compiti](http://deviq.com/separation-of-concerns/) : le impostazioni per le diverse parti dell'app non sono dipendenti o a uno di loro.</span><span class="sxs-lookup"><span data-stu-id="2e625-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="2e625-161">La classe di opzioni deve essere non astratto con costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="2e625-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="2e625-162">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2e625-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

<span data-ttu-id="2e625-163">Nel codice seguente, il provider di configurazione JSON è abilitato.</span><span class="sxs-lookup"><span data-stu-id="2e625-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="2e625-164">La `MyOptions` classe viene aggiunta al contenitore del servizio e l'associazione alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="2e625-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="2e625-165">Nell'esempio [controller](../mvc/controllers/index.md) utilizza [costruttore Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) su [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) per accedere alle impostazioni:</span><span class="sxs-lookup"><span data-stu-id="2e625-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="2e625-166">Con i seguenti *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="2e625-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="2e625-167">Il `HomeController.Index` restituisce `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="2e625-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="2e625-168">App tipica non associare l'intera configurazione in un file con le opzioni single.</span><span class="sxs-lookup"><span data-stu-id="2e625-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="2e625-169">In seguito verrà illustrato come utilizzare `GetSection` da associare a una sezione.</span><span class="sxs-lookup"><span data-stu-id="2e625-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="2e625-170">Nel codice seguente, una seconda `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="2e625-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="2e625-171">Usa un delegato per configurare l'associazione con `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="2e625-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="2e625-172">È possibile aggiungere più provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="2e625-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="2e625-173">Provider di configurazione sono disponibili in pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="2e625-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="2e625-174">Vengono applicate nell'ordine in cui sono registrati.</span><span class="sxs-lookup"><span data-stu-id="2e625-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="2e625-175">Ogni chiamata a `Configure<TOptions>` aggiunge un `IConfigureOptions<TOptions>` servizio al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="2e625-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="2e625-176">Nell'esempio precedente, i valori di `Option1` e `Option2` sono entrambi specificati nel *appSettings. JSON* , ma il valore del `Option1` viene sottoposto a override dal delegato configurato.</span><span class="sxs-lookup"><span data-stu-id="2e625-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="2e625-177">Quando più di un servizio di configurazione è abilitato, l'ultima origine di configurazione specificato "wins" (imposta il valore di configurazione).</span><span class="sxs-lookup"><span data-stu-id="2e625-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="2e625-178">Nel codice precedente, il `HomeController.Index` restituisce `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="2e625-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="2e625-179">Quando si associa opzioni a configurazione, ogni proprietà nel tipo di opzioni è associata a una chiave di configurazione del modulo `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="2e625-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="2e625-180">Ad esempio, il `MyOptions.Option1` proprietà è associata alla chiave `Option1`, che viene letto dal `option1` proprietà *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="2e625-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="2e625-181">Un esempio delle sottoproprietà è illustrato più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="2e625-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="2e625-182">Nel codice seguente, una terza `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="2e625-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="2e625-183">Associa `MySubOptions` alla sezione `subsection` del *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="2e625-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="2e625-184">Nota: Questo metodo di estensione richiede il `Microsoft.Extensions.Options.ConfigurationExtensions` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="2e625-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="2e625-185">Con i seguenti *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="2e625-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="2e625-186">La `MySubOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="2e625-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="2e625-187">Con i seguenti `Controller`:</span><span class="sxs-lookup"><span data-stu-id="2e625-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="2e625-188">`subOption1 = subvalue1_from_json, subOption2 = 200`viene restituito.</span><span class="sxs-lookup"><span data-stu-id="2e625-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="2e625-189">È inoltre possibile visualizzare le opzioni disponibili in un modello di visualizzazione o inserire `IOptions<TOptions>` direttamente in una vista:</span><span class="sxs-lookup"><span data-stu-id="2e625-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="2e625-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="2e625-190">IOptionsSnapshot</span></span>

<span data-ttu-id="2e625-191">*Richiede ASP.NET Core 1.1 o versione successiva.*</span><span class="sxs-lookup"><span data-stu-id="2e625-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="2e625-192">`IOptionsSnapshot`supporta il ricaricamento di dati di configurazione quando viene modificato il file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="2e625-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="2e625-193">Ha un overhead minimo.</span><span class="sxs-lookup"><span data-stu-id="2e625-193">It has minimal overhead.</span></span> <span data-ttu-id="2e625-194">Utilizzando `IOptionsSnapshot` con `reloadOnChange: true`, le opzioni sono associate a `IConfiguration` e ricaricati quando modificato.</span><span class="sxs-lookup"><span data-stu-id="2e625-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="2e625-195">Nell'esempio seguente viene illustrato come un nuovo `IOptionsSnapshot` viene creato dopo *config. JSON* le modifiche.</span><span class="sxs-lookup"><span data-stu-id="2e625-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="2e625-196">Le richieste per il server restituirà lo stesso ora *config. JSON* è **non** modificato.</span><span class="sxs-lookup"><span data-stu-id="2e625-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="2e625-197">La prima richiesta dopo *config. JSON* le modifiche verranno visualizzate una nuova ora.</span><span class="sxs-lookup"><span data-stu-id="2e625-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="2e625-198">La figura seguente mostra l'output del server:</span><span class="sxs-lookup"><span data-stu-id="2e625-198">The following image shows the server output:</span></span>

![visualizzazione di immagini browser "ultimo aggiornamento: 22/11/2016 4:43 PM"](configuration/_static/first.png)

<span data-ttu-id="2e625-200">Aggiornamento del browser non modifica il valore del messaggio o l'ora visualizzata (quando *config. JSON* non è stato modificato).</span><span class="sxs-lookup"><span data-stu-id="2e625-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="2e625-201">Modificare e salvare il *config. JSON* e quindi aggiornare il browser:</span><span class="sxs-lookup"><span data-stu-id="2e625-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![visualizzazione di immagini browser "ultimo aggiornamento a, e: 22/11/2016 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="2e625-203">Provider in memoria e l'associazione a una classe POCO</span><span class="sxs-lookup"><span data-stu-id="2e625-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="2e625-204">L'esempio seguente viene illustrato come utilizzare il provider in memoria e associare a una classe:</span><span class="sxs-lookup"><span data-stu-id="2e625-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="2e625-205">I valori di configurazione vengono restituiti come stringhe, ma l'associazione consente la costruzione di oggetti.</span><span class="sxs-lookup"><span data-stu-id="2e625-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="2e625-206">Associazione consente di recuperare gli oggetti POCO o addirittura interi oggetti grafici.</span><span class="sxs-lookup"><span data-stu-id="2e625-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="2e625-207">Nell'esempio seguente viene illustrato come associare a `MyWindow` e utilizzare il modello di opzioni con un'applicazione ASP.NET MVC di base:</span><span class="sxs-lookup"><span data-stu-id="2e625-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="2e625-208">Associare la classe personalizzata in `ConfigureServices` durante la compilazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="2e625-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="2e625-209">Visualizzare le impostazioni dal `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="2e625-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="2e625-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="2e625-210">GetValue</span></span>

<span data-ttu-id="2e625-211">Nell'esempio seguente viene illustrato il [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="2e625-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="2e625-212">Il ConfigurationBinder `GetValue<T>` metodo consente di specificare un valore predefinito (80 nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="2e625-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="2e625-213">`GetValue<T>`per scenari semplici e non viene associato a intere sezioni.</span><span class="sxs-lookup"><span data-stu-id="2e625-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="2e625-214">`GetValue<T>`Ottiene i valori scalari `GetSection(key).Value` convertito in un tipo specifico.</span><span class="sxs-lookup"><span data-stu-id="2e625-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="2e625-215">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="2e625-215">Binding to an object graph</span></span>

<span data-ttu-id="2e625-216">È possibile associare in modo ricorsivo a ogni oggetto in una classe.</span><span class="sxs-lookup"><span data-stu-id="2e625-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="2e625-217">Tenere presente quanto segue `AppOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="2e625-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="2e625-218">Associa l'esempio seguente la `AppOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="2e625-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="2e625-219">**ASP.NET Core 1.1** e versioni successive è possibile utilizzare `Get<T>`, che funziona con intere sezioni.</span><span class="sxs-lookup"><span data-stu-id="2e625-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="2e625-220">`Get<T>`può essere più convienent rispetto all'utilizzo di `Bind`.</span><span class="sxs-lookup"><span data-stu-id="2e625-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="2e625-221">Il codice seguente viene illustrato come utilizzare `Get<T>` con l'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="2e625-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="2e625-222">Con i seguenti *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="2e625-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="2e625-223">Il programma visualizza `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="2e625-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="2e625-224">Il codice seguente consente di unit test della configurazione:</span><span class="sxs-lookup"><span data-stu-id="2e625-224">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="2e625-225">Esempio di base di provider di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2e625-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="2e625-226">In questa sezione viene creato un provider di configurazione di base che legge le coppie nome-valore da un database tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2e625-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="2e625-227">Definire un `ConfigurationValue` entità per l'archiviazione dei valori di configurazione nel database:</span><span class="sxs-lookup"><span data-stu-id="2e625-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="2e625-228">Aggiungere un `ConfigurationContext` archiviare e accedere ai valori configurati:</span><span class="sxs-lookup"><span data-stu-id="2e625-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="2e625-229">Creare una classe che implementa [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="2e625-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="2e625-230">Creare il provider di configurazione personalizzato ereditando dalla [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="2e625-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="2e625-231">Il provider di configurazione consente di inizializzare il database quando è vuoto:</span><span class="sxs-lookup"><span data-stu-id="2e625-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="2e625-232">Quando si esegue l'esempio, vengono visualizzati i valori evidenziati dal database ("value_from_ef_1" e "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="2e625-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="2e625-233">È possibile aggiungere un `EFConfigSource` metodo di estensione per l'aggiunta dell'origine di configurazione:</span><span class="sxs-lookup"><span data-stu-id="2e625-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="2e625-234">Il codice seguente viene illustrato come utilizzare l'oggetto personalizzato `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="2e625-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="2e625-235">Nota L'esempio aggiunge l'oggetto personalizzato `EFConfigProvider` quando il provider JSON, pertanto le impostazioni dal database sostituiranno le impostazioni dal *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="2e625-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="2e625-236">Con i seguenti *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="2e625-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="2e625-237">Verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="2e625-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="2e625-238">Provider di configurazione di riga di comando</span><span class="sxs-lookup"><span data-stu-id="2e625-238">CommandLine configuration provider</span></span>

<span data-ttu-id="2e625-239">Il [il provider di configurazione di riga di comando](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) riceve coppie chiave-valore di argomento della riga di comando per la configurazione in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2e625-239">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="2e625-240">Consente di visualizzare o scaricare l'esempio di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="2e625-240">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a><span data-ttu-id="2e625-241">Il provider di configurazione</span><span class="sxs-lookup"><span data-stu-id="2e625-241">Setting up the provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="2e625-242">Configurazione di base</span><span class="sxs-lookup"><span data-stu-id="2e625-242">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="2e625-243">Per attivare la configurazione della riga di comando, chiamare il `AddCommandLine` il metodo di estensione in un'istanza di [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="2e625-243">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="2e625-244">Esecuzione del codice, viene visualizzato il seguente output:</span><span class="sxs-lookup"><span data-stu-id="2e625-244">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="2e625-245">Il passaggio di coppie chiave-valore argomento nella riga di comando di modifica i valori di `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="2e625-245">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="2e625-246">Consente di visualizzare la finestra della console:</span><span class="sxs-lookup"><span data-stu-id="2e625-246">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="2e625-247">Per eseguire l'override di configurazione fornita da altri provider di configurazione con la configurazione della riga di comando, chiamare `AddCommandLine` l'ultimo `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2e625-247">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e625-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e625-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2e625-249">App di 2. x di ASP.NET Core tipico utilizzare il metodo pratici statici `CreateDefaultBuilder` per compilare l'host:</span><span class="sxs-lookup"><span data-stu-id="2e625-249">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

<span data-ttu-id="2e625-250">`CreateDefaultBuilder`Carica la configurazione facoltativa da *appSettings. JSON*, *appsettings. { Ambiente} JSON*, [informazioni riservate dell'utente](xref:security/app-secrets) (nel `Development` ambiente), le variabili di ambiente e gli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2e625-250">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="2e625-251">Il provider di configurazione della riga di comando viene chiamato per ultimo.</span><span class="sxs-lookup"><span data-stu-id="2e625-251">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="2e625-252">Chiamare il provider di ultima consente gli argomenti della riga di comando passati in fase di esecuzione per eseguire l'override di configurazione impostata da altri provider di configurazione è stato chiamato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2e625-252">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="2e625-253">Si noti che per *appsettings* file `reloadOnChange` è abilitata.</span><span class="sxs-lookup"><span data-stu-id="2e625-253">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="2e625-254">Argomenti della riga di comando vengono ignorati se un valore di configurazione corrispondente in un *appsettings* file è stato modificato dopo l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2e625-254">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="2e625-255">Come alternativa all'utilizzo di `CreateDefaultBuilder` (metodo), la creazione di un host utilizzando [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) e creare manualmente la configurazione con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) è supportato in ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="2e625-255">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="2e625-256">Vedere la scheda di 1. x di ASP.NET Core per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="2e625-256">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e625-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e625-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2e625-258">Creare un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chiamare il `AddCommandLine` metodo da utilizzare il provider di configurazione della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2e625-258">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="2e625-259">Chiamare il provider di ultima consente gli argomenti della riga di comando passati in fase di esecuzione per eseguire l'override di configurazione impostata da altri provider di configurazione è stato chiamato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2e625-259">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="2e625-260">Applicare la configurazione per [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con il `UseConfiguration` metodo:</span><span class="sxs-lookup"><span data-stu-id="2e625-260">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="2e625-261">Argomenti</span><span class="sxs-lookup"><span data-stu-id="2e625-261">Arguments</span></span>

<span data-ttu-id="2e625-262">Gli argomenti passati alla riga di comando devono essere conforme a uno dei due formati indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="2e625-262">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="2e625-263">Formato dell'argomento</span><span class="sxs-lookup"><span data-stu-id="2e625-263">Argument format</span></span>                                                     | <span data-ttu-id="2e625-264">Esempio</span><span class="sxs-lookup"><span data-stu-id="2e625-264">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="2e625-265">Singolo argomento: una coppia chiave-valore separate da un segno di uguale (`=`)</span><span class="sxs-lookup"><span data-stu-id="2e625-265">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="2e625-266">Sequenza di due argomenti: una coppia chiave-valore separate da uno spazio</span><span class="sxs-lookup"><span data-stu-id="2e625-266">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="2e625-267">**Singolo argomento**</span><span class="sxs-lookup"><span data-stu-id="2e625-267">**Single argument**</span></span>

<span data-ttu-id="2e625-268">Il valore deve seguire un segno di uguale (`=`).</span><span class="sxs-lookup"><span data-stu-id="2e625-268">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="2e625-269">Il valore può essere null (ad esempio, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="2e625-269">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="2e625-270">La chiave potrebbe essere un prefisso.</span><span class="sxs-lookup"><span data-stu-id="2e625-270">The key may have a prefix.</span></span>

| <span data-ttu-id="2e625-271">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="2e625-271">Key prefix</span></span>               | <span data-ttu-id="2e625-272">Esempio</span><span class="sxs-lookup"><span data-stu-id="2e625-272">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="2e625-273">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="2e625-273">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="2e625-274">Singolo dash (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="2e625-274">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="2e625-275">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="2e625-275">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="2e625-276">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="2e625-276">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="2e625-277">&#8224; Una chiave con un prefisso singolo dash (`-`) deve essere specificato in [passare mapping](#switch-mappings), descritto di seguito.</span><span class="sxs-lookup"><span data-stu-id="2e625-277">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="2e625-278">Comando di esempio:</span><span class="sxs-lookup"><span data-stu-id="2e625-278">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="2e625-279">Nota: Se `-key1` non è presente nel [passare mapping](#switch-mappings) fornito al provider di configurazione, un `FormatException` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2e625-279">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="2e625-280">**Sequenza di due argomenti**</span><span class="sxs-lookup"><span data-stu-id="2e625-280">**Sequence of two arguments**</span></span>

<span data-ttu-id="2e625-281">Il valore non può essere null e deve seguire la chiave separata da uno spazio.</span><span class="sxs-lookup"><span data-stu-id="2e625-281">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="2e625-282">La chiave deve essere un prefisso.</span><span class="sxs-lookup"><span data-stu-id="2e625-282">The key must have a prefix.</span></span>

| <span data-ttu-id="2e625-283">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="2e625-283">Key prefix</span></span>               | <span data-ttu-id="2e625-284">Esempio</span><span class="sxs-lookup"><span data-stu-id="2e625-284">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="2e625-285">Singolo dash (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="2e625-285">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="2e625-286">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="2e625-286">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="2e625-287">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="2e625-287">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="2e625-288">&#8224; Una chiave con un prefisso singolo dash (`-`) deve essere specificato in [passare mapping](#switch-mappings), descritto di seguito.</span><span class="sxs-lookup"><span data-stu-id="2e625-288">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="2e625-289">Comando di esempio:</span><span class="sxs-lookup"><span data-stu-id="2e625-289">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="2e625-290">Nota: Se `-key1` non è presente nel [passare mapping](#switch-mappings) fornito al provider di configurazione, un `FormatException` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2e625-290">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="2e625-291">Chiavi duplicate</span><span class="sxs-lookup"><span data-stu-id="2e625-291">Duplicate keys</span></span>

<span data-ttu-id="2e625-292">Se le chiavi duplicate, viene utilizzata l'ultima coppia chiave-valore.</span><span class="sxs-lookup"><span data-stu-id="2e625-292">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="2e625-293">Mapping di commutatore</span><span class="sxs-lookup"><span data-stu-id="2e625-293">Switch mappings</span></span>

<span data-ttu-id="2e625-294">Quando si compila manualmente configurazione con `ConfigurationBuilder`, è possibile fornire facoltativamente un dizionario di mapping del commutatore per il `AddCommandLine` metodo.</span><span class="sxs-lookup"><span data-stu-id="2e625-294">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="2e625-295">Mapping di commutatore consentono di fornire la logica di sostituzione nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="2e625-295">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="2e625-296">Quando viene utilizzato il dizionario di mapping di parametro, il dizionario viene controllato per una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2e625-296">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="2e625-297">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="2e625-297">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="2e625-298">Un mapping del commutatore è necessario per una chiave della riga di comando, preceduta da un trattino singolo (`-`).</span><span class="sxs-lookup"><span data-stu-id="2e625-298">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="2e625-299">L'opzione regole di chiave del dizionario di mapping:</span><span class="sxs-lookup"><span data-stu-id="2e625-299">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="2e625-300">Switch devono iniziare con un trattino (`-`) o doppio-trattino (`--`).</span><span class="sxs-lookup"><span data-stu-id="2e625-300">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="2e625-301">Il dizionario di mapping commutatore non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="2e625-301">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="2e625-302">Nell'esempio seguente, il `GetSwitchMappings` metodo consente gli argomenti della riga di comando usare un singolo trattino (`-`) chiave prefisso ed evitare i prefissi sottochiavi iniziali.</span><span class="sxs-lookup"><span data-stu-id="2e625-302">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="2e625-303">Senza fornire argomenti della riga di comando, il dizionario fornito per `AddInMemoryCollection` imposta i valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="2e625-303">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="2e625-304">Eseguire l'app con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2e625-304">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="2e625-305">Consente di visualizzare la finestra della console:</span><span class="sxs-lookup"><span data-stu-id="2e625-305">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="2e625-306">Per passare le impostazioni di configurazione, usare la seguente:</span><span class="sxs-lookup"><span data-stu-id="2e625-306">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="2e625-307">Consente di visualizzare la finestra della console:</span><span class="sxs-lookup"><span data-stu-id="2e625-307">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="2e625-308">Dopo aver creato il dizionario di mapping di commutatore, contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="2e625-308">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="2e625-309">Chiave</span><span class="sxs-lookup"><span data-stu-id="2e625-309">Key</span></span>            | <span data-ttu-id="2e625-310">Valore</span><span class="sxs-lookup"><span data-stu-id="2e625-310">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="2e625-311">Per dimostrare la commutazione chiave utilizzando il dizionario, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2e625-311">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="2e625-312">Le chiavi della riga di comando vengono scambiate.</span><span class="sxs-lookup"><span data-stu-id="2e625-312">The command-line keys are swapped.</span></span> <span data-ttu-id="2e625-313">Nella finestra della console vengono visualizzati i valori di configurazione per `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="2e625-313">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="2e625-314">Il file Web. config</span><span class="sxs-lookup"><span data-stu-id="2e625-314">The web.config file</span></span>

<span data-ttu-id="2e625-315">Oggetto *Web. config* file è obbligatorio quando si ospita l'applicazione in IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2e625-315">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="2e625-316">*Web. config* attiva AspNetCoreModule in IIS per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="2e625-316">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="2e625-317">Le impostazioni in *Web. config* abilitare AspNetCoreModule in IIS per avviare l'app e configurare altre impostazioni di IIS e i moduli.</span><span class="sxs-lookup"><span data-stu-id="2e625-317">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="2e625-318">Se si utilizza Visual Studio ed Elimina *Web. config*, Visual Studio creerà uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="2e625-318">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="2e625-319">Note aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2e625-319">Additional notes</span></span>

* <span data-ttu-id="2e625-320">Dependency Injection (DI) non viene impostato fino a dopo `ConfigureServices` viene richiamato.</span><span class="sxs-lookup"><span data-stu-id="2e625-320">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="2e625-321">Il sistema di configurazione non è in grado di riconoscere SEN.</span><span class="sxs-lookup"><span data-stu-id="2e625-321">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="2e625-322">`IConfiguration`presenta due specializzazioni:</span><span class="sxs-lookup"><span data-stu-id="2e625-322">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="2e625-323">`IConfigurationRoot`Utilizzato per il nodo radice.</span><span class="sxs-lookup"><span data-stu-id="2e625-323">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="2e625-324">Può attivare un ricaricamento.</span><span class="sxs-lookup"><span data-stu-id="2e625-324">Can trigger a reload.</span></span>
  * <span data-ttu-id="2e625-325">`IConfigurationSection`Rappresenta una sezione dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="2e625-325">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="2e625-326">Il `GetSection` e `GetChildren` i metodi restituiscono un `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="2e625-326">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e625-327">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2e625-327">Additional resources</span></span>

* [<span data-ttu-id="2e625-328">Uso di più ambienti</span><span class="sxs-lookup"><span data-stu-id="2e625-328">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="2e625-329">Archiviazione sicura di segreti dell'app durante lo sviluppo</span><span class="sxs-lookup"><span data-stu-id="2e625-329">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="2e625-330">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e625-330">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="2e625-331">Inserimento dipendenze</span><span class="sxs-lookup"><span data-stu-id="2e625-331">Dependency Injection</span></span>](dependency-injection.md)
* <span data-ttu-id="2e625-332">[Azure Key Vault configuration provider](xref:security/key-vault-configuration) (Provider di configurazione di Azure Key Vault)</span><span class="sxs-lookup"><span data-stu-id="2e625-332">[Azure Key Vault configuration provider](xref:security/key-vault-configuration)</span></span>
