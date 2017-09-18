---
title: Configurazione di ASP.NET Core
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
ms.openlocfilehash: 7d591259587766a932a14bb030c76274101d16ac
ms.sourcegitcommit: f8f6b5934bd071a349f5bc1e389365c52b1c00fa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/14/2017
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="929f6-104">Configurazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="929f6-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="929f6-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [feed di Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="929f6-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="929f6-106">L'API di configurazione fornisce un modo per configurare un'app in base a un elenco di coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="929f6-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="929f6-107">Configurazione è di lettura in fase di esecuzione da più origini.</span><span class="sxs-lookup"><span data-stu-id="929f6-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="929f6-108">Le coppie nome-valore possono essere raggruppate in una gerarchia a più livello.</span><span class="sxs-lookup"><span data-stu-id="929f6-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="929f6-109">Sono disponibili provider di configurazione per:</span><span class="sxs-lookup"><span data-stu-id="929f6-109">There are configuration providers for:</span></span>

* <span data-ttu-id="929f6-110">Formati di file (INI, JSON e XML)</span><span class="sxs-lookup"><span data-stu-id="929f6-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="929f6-111">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="929f6-111">Command-line arguments</span></span>
* <span data-ttu-id="929f6-112">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="929f6-112">Environment variables</span></span>
* <span data-ttu-id="929f6-113">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="929f6-113">In-memory .NET objects</span></span>
* <span data-ttu-id="929f6-114">Un archivio utente crittografato</span><span class="sxs-lookup"><span data-stu-id="929f6-114">An encrypted user store</span></span>
* [<span data-ttu-id="929f6-115">Insieme di credenziali chiave di Azure</span><span class="sxs-lookup"><span data-stu-id="929f6-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="929f6-116">Provider personalizzati, cui installa o crea</span><span class="sxs-lookup"><span data-stu-id="929f6-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="929f6-117">Ogni valore di configurazione esegue il mapping a una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="929f6-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="929f6-118">Supporto di associazione predefinita per deserializzare le impostazioni in un oggetto personalizzato [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) oggetto (una classe .NET semplice con proprietà).</span><span class="sxs-lookup"><span data-stu-id="929f6-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="929f6-119">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="929f6-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="929f6-120">Configurazione semplice</span><span class="sxs-lookup"><span data-stu-id="929f6-120">Simple configuration</span></span>

<span data-ttu-id="929f6-121">La seguente applicazione console utilizza il provider di configurazione JSON:</span><span class="sxs-lookup"><span data-stu-id="929f6-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="929f6-122">L'app legge e visualizza le impostazioni di configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="929f6-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="929f6-123">Configurazione è costituita da un elenco gerarchico di coppie nome-valore in cui i nodi sono separati dai due punti.</span><span class="sxs-lookup"><span data-stu-id="929f6-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="929f6-124">Per recuperare un valore, accedere il `Configuration` un indicizzatore con la corrispondente chiave dell'elemento:</span><span class="sxs-lookup"><span data-stu-id="929f6-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="929f6-125">Per lavorare con le matrici nelle origini configurazione in formato JSON, utilizzare un indice della matrice come parte della stringa separate da virgola.</span><span class="sxs-lookup"><span data-stu-id="929f6-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="929f6-126">Nell'esempio seguente ottiene il nome del primo elemento nel passaggio precedente `wizards` matrice:</span><span class="sxs-lookup"><span data-stu-id="929f6-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="929f6-127">Coppie nome-valore scritte incorporato in `Configuration` provider sono **non** persistente, tuttavia, è possibile creare un provider personalizzato che salva i valori.</span><span class="sxs-lookup"><span data-stu-id="929f6-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="929f6-128">Vedere [provider di configurazione personalizzato](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="929f6-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="929f6-129">L'esempio precedente Usa l'indicizzatore di configurazione per leggere i valori.</span><span class="sxs-lookup"><span data-stu-id="929f6-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="929f6-130">Per la configurazione di accesso di fuori di `Startup`, utilizzare il [modello opzioni](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="929f6-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="929f6-131">Il *modello opzioni* è illustrato più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="929f6-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="929f6-132">È in genere hanno diverse impostazioni di configurazione per gli ambienti diversi, ad esempio sviluppo, test e produzione.</span><span class="sxs-lookup"><span data-stu-id="929f6-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="929f6-133">Il `CreateDefaultBuilder` il metodo di estensione in un'app di ASP.NET Core 2. x (o tramite `AddJsonFile` e `AddEnvironmentVariables` direttamente in un'app di ASP.NET Core 1. x) aggiunge dei provider di configurazione per la lettura dei file JSON e sistema origini configurazione:</span><span class="sxs-lookup"><span data-stu-id="929f6-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="929f6-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="929f6-134">*appsettings.json*</span></span>
* <span data-ttu-id="929f6-135">*appSettings. \<EnvironmentName > JSON*</span><span class="sxs-lookup"><span data-stu-id="929f6-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="929f6-136">variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="929f6-136">environment variables</span></span>

<span data-ttu-id="929f6-137">Vedere [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) per una spiegazione dei parametri.</span><span class="sxs-lookup"><span data-stu-id="929f6-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="929f6-138">`reloadOnChange`è supportato solo in ASP.NET Core 1.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="929f6-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="929f6-139">Origini di configurazione vengono letti nell'ordine in che cui vengono specificati.</span><span class="sxs-lookup"><span data-stu-id="929f6-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="929f6-140">Nel codice precedente, le variabili di ambiente vengono lette ultima.</span><span class="sxs-lookup"><span data-stu-id="929f6-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="929f6-141">Tutti i valori di configurazione impostati tramite l'ambiente di sostituire quelli impostati nei due provider precedente.</span><span class="sxs-lookup"><span data-stu-id="929f6-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="929f6-142">L'ambiente è in genere impostato su uno dei `Development`, `Staging`, o `Production`.</span><span class="sxs-lookup"><span data-stu-id="929f6-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="929f6-143">Vedere [utilizzo di più ambienti](environments.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="929f6-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="929f6-144">Considerazioni sulla configurazione di:</span><span class="sxs-lookup"><span data-stu-id="929f6-144">Configuration considerations:</span></span>

* <span data-ttu-id="929f6-145">`IOptionsSnapshot`possibile ricaricare i dati di configurazione in caso di modifiche.</span><span class="sxs-lookup"><span data-stu-id="929f6-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="929f6-146">Utilizzare `IOptionsSnapshot` se è necessario ricaricare i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="929f6-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="929f6-147">Vedere [IOptionsSnapshot](#ioptionssnapshot) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="929f6-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="929f6-148">Le chiavi di configurazione vengono fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="929f6-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="929f6-149">Una procedura consigliata è specificare le variabili di ambiente per ultimo, in modo che l'ambiente locale può eseguire l'override di qualsiasi elemento impostati nei file di configurazione distribuita.</span><span class="sxs-lookup"><span data-stu-id="929f6-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="929f6-150">**Mai** archiviare password o altri dati sensibili nel codice di provider di configurazione o nel file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="929f6-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="929f6-151">Non è possibile utilizzare i segreti di produzione nell'ambiente di sviluppo o ambienti di test.</span><span class="sxs-lookup"><span data-stu-id="929f6-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="929f6-152">Specificare invece i segreti di fuori della struttura del progetto, in modo che non possono accidentalmente salvate nel repository.</span><span class="sxs-lookup"><span data-stu-id="929f6-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="929f6-153">Altre informazioni, vedere [utilizzo di più ambienti](environments.md) e la gestione di [archiviazione sicura di segreti dell'app durante lo sviluppo](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="929f6-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="929f6-154">Se `:` non può essere utilizzato in variabili di ambiente del sistema, sostituire `:` con `__` (doppio carattere di sottolineatura).</span><span class="sxs-lookup"><span data-stu-id="929f6-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="929f6-155">Utilizzando le opzioni e gli oggetti di configurazione</span><span class="sxs-lookup"><span data-stu-id="929f6-155">Using Options and configuration objects</span></span>

<span data-ttu-id="929f6-156">Il modello di opzioni Usa le classi di opzioni personalizzate per rappresentare un gruppo di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="929f6-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="929f6-157">Si consiglia di creare classi separate per ogni funzionalità all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="929f6-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="929f6-158">Classi disaccoppiate seguono:</span><span class="sxs-lookup"><span data-stu-id="929f6-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="929f6-159">Il [interfaccia separazione principio (ISP)](http://deviq.com/interface-segregation-principle/) : classi dipendono solo usano le impostazioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="929f6-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="929f6-160">[La separazione dei compiti](http://deviq.com/separation-of-concerns/) : le impostazioni per le diverse parti dell'app non sono dipendenti o a uno di loro.</span><span class="sxs-lookup"><span data-stu-id="929f6-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="929f6-161">La classe di opzioni deve essere non astratto con costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="929f6-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="929f6-162">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="929f6-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

<span data-ttu-id="929f6-163">Nel codice seguente, il provider di configurazione JSON è abilitato.</span><span class="sxs-lookup"><span data-stu-id="929f6-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="929f6-164">La `MyOptions` classe viene aggiunta al contenitore del servizio e l'associazione alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="929f6-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="929f6-165">Nell'esempio [controller](../mvc/controllers/index.md) utilizza [costruttore Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) su [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) per accedere alle impostazioni:</span><span class="sxs-lookup"><span data-stu-id="929f6-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="929f6-166">Con i seguenti *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="929f6-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="929f6-167">Il `HomeController.Index` restituisce `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="929f6-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="929f6-168">App tipica non associare l'intera configurazione in un file con le opzioni single.</span><span class="sxs-lookup"><span data-stu-id="929f6-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="929f6-169">In seguito verrà illustrato come utilizzare `GetSection` da associare a una sezione.</span><span class="sxs-lookup"><span data-stu-id="929f6-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="929f6-170">Nel codice seguente, una seconda `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="929f6-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="929f6-171">Usa un delegato per configurare l'associazione con `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="929f6-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="929f6-172">È possibile aggiungere più provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="929f6-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="929f6-173">Provider di configurazione sono disponibili in pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="929f6-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="929f6-174">Vengono applicate nell'ordine in cui sono registrati.</span><span class="sxs-lookup"><span data-stu-id="929f6-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="929f6-175">Ogni chiamata a `Configure<TOptions>` aggiunge un `IConfigureOptions<TOptions>` servizio al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="929f6-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="929f6-176">Nell'esempio precedente, i valori di `Option1` e `Option2` sono entrambi specificati nel *appSettings. JSON* , ma il valore del `Option1` viene sottoposto a override dal delegato configurato.</span><span class="sxs-lookup"><span data-stu-id="929f6-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="929f6-177">Quando più di un servizio di configurazione è abilitato, l'ultima origine di configurazione specificato "wins" (imposta il valore di configurazione).</span><span class="sxs-lookup"><span data-stu-id="929f6-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="929f6-178">Nel codice precedente, il `HomeController.Index` restituisce `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="929f6-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="929f6-179">Quando si associa opzioni a configurazione, ogni proprietà nel tipo di opzioni è associata a una chiave di configurazione del modulo `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="929f6-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="929f6-180">Ad esempio, il `MyOptions.Option1` proprietà è associata alla chiave `Option1`, che viene letto dal `option1` proprietà *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="929f6-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="929f6-181">Un esempio delle sottoproprietà è illustrato più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="929f6-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="929f6-182">Nel codice seguente, una terza `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="929f6-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="929f6-183">Associa `MySubOptions` alla sezione `subsection` del *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="929f6-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="929f6-184">Nota: Questo metodo di estensione richiede il `Microsoft.Extensions.Options.ConfigurationExtensions` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="929f6-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="929f6-185">Con i seguenti *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="929f6-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="929f6-186">La `MySubOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="929f6-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="929f6-187">Con i seguenti `Controller`:</span><span class="sxs-lookup"><span data-stu-id="929f6-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="929f6-188">`subOption1 = subvalue1_from_json, subOption2 = 200`viene restituito.</span><span class="sxs-lookup"><span data-stu-id="929f6-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="929f6-189">È inoltre possibile visualizzare le opzioni disponibili in un modello di visualizzazione o inserire `IOptions<TOptions>` direttamente in una vista:</span><span class="sxs-lookup"><span data-stu-id="929f6-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="929f6-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="929f6-190">IOptionsSnapshot</span></span>

<span data-ttu-id="929f6-191">*Richiede ASP.NET Core 1.1 o versione successiva.*</span><span class="sxs-lookup"><span data-stu-id="929f6-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="929f6-192">`IOptionsSnapshot`supporta il ricaricamento di dati di configurazione quando viene modificato il file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="929f6-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="929f6-193">Ha un overhead minimo.</span><span class="sxs-lookup"><span data-stu-id="929f6-193">It has minimal overhead.</span></span> <span data-ttu-id="929f6-194">Utilizzando `IOptionsSnapshot` con `reloadOnChange: true`, le opzioni sono associate a `IConfiguration` e ricaricati quando modificato.</span><span class="sxs-lookup"><span data-stu-id="929f6-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="929f6-195">Nell'esempio seguente viene illustrato come un nuovo `IOptionsSnapshot` viene creato dopo *config. JSON* le modifiche.</span><span class="sxs-lookup"><span data-stu-id="929f6-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="929f6-196">Le richieste per il server restituirà lo stesso ora *config. JSON* è **non** modificato.</span><span class="sxs-lookup"><span data-stu-id="929f6-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="929f6-197">La prima richiesta dopo *config. JSON* le modifiche verranno visualizzate una nuova ora.</span><span class="sxs-lookup"><span data-stu-id="929f6-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="929f6-198">La figura seguente mostra l'output del server:</span><span class="sxs-lookup"><span data-stu-id="929f6-198">The following image shows the server output:</span></span>

![visualizzazione di immagini browser "ultimo aggiornamento: 22/11/2016 4:43 PM"](configuration/_static/first.png)

<span data-ttu-id="929f6-200">Aggiornamento del browser non modifica il valore del messaggio o l'ora visualizzata (quando *config. JSON* non è stato modificato).</span><span class="sxs-lookup"><span data-stu-id="929f6-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="929f6-201">Modificare e salvare il *config. JSON* e quindi aggiornare il browser:</span><span class="sxs-lookup"><span data-stu-id="929f6-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![visualizzazione di immagini browser "ultimo aggiornamento a, e: 22/11/2016 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="929f6-203">Provider in memoria e l'associazione a una classe POCO</span><span class="sxs-lookup"><span data-stu-id="929f6-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="929f6-204">L'esempio seguente viene illustrato come utilizzare il provider in memoria e associare a una classe:</span><span class="sxs-lookup"><span data-stu-id="929f6-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="929f6-205">I valori di configurazione vengono restituiti come stringhe, ma l'associazione consente la costruzione di oggetti.</span><span class="sxs-lookup"><span data-stu-id="929f6-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="929f6-206">Associazione consente di recuperare gli oggetti POCO o addirittura interi oggetti grafici.</span><span class="sxs-lookup"><span data-stu-id="929f6-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="929f6-207">Nell'esempio seguente viene illustrato come associare a `MyWindow` e utilizzare il modello di opzioni con un'applicazione ASP.NET MVC di base:</span><span class="sxs-lookup"><span data-stu-id="929f6-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="929f6-208">Associare la classe personalizzata in `ConfigureServices` durante la compilazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="929f6-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="929f6-209">Visualizzare le impostazioni dal `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="929f6-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="929f6-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="929f6-210">GetValue</span></span>

<span data-ttu-id="929f6-211">Nell'esempio seguente viene illustrato il [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="929f6-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="929f6-212">Il ConfigurationBinder `GetValue<T>` metodo consente di specificare un valore predefinito (80 nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="929f6-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="929f6-213">`GetValue<T>`per scenari semplici e non viene associato a intere sezioni.</span><span class="sxs-lookup"><span data-stu-id="929f6-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="929f6-214">`GetValue<T>`Ottiene i valori scalari `GetSection(key).Value` convertito in un tipo specifico.</span><span class="sxs-lookup"><span data-stu-id="929f6-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="929f6-215">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="929f6-215">Binding to an object graph</span></span>

<span data-ttu-id="929f6-216">È possibile associare in modo ricorsivo a ogni oggetto in una classe.</span><span class="sxs-lookup"><span data-stu-id="929f6-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="929f6-217">Tenere presente quanto segue `AppOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="929f6-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="929f6-218">Associa l'esempio seguente la `AppOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="929f6-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="929f6-219">**ASP.NET Core 1.1** e versioni successive è possibile utilizzare `Get<T>`, che funziona con intere sezioni.</span><span class="sxs-lookup"><span data-stu-id="929f6-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="929f6-220">`Get<T>`può essere più convienent rispetto all'utilizzo di `Bind`.</span><span class="sxs-lookup"><span data-stu-id="929f6-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="929f6-221">Il codice seguente viene illustrato come utilizzare `Get<T>` con l'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="929f6-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="929f6-222">Con i seguenti *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="929f6-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="929f6-223">Il programma visualizza `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="929f6-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="929f6-224">Il codice seguente consente di unit test della configurazione:</span><span class="sxs-lookup"><span data-stu-id="929f6-224">The following code can be used to unit test the configuration:</span></span>

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

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="929f6-225">Esempio di base di provider di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="929f6-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="929f6-226">In questa sezione viene creato un provider di configurazione di base che legge le coppie nome-valore da un database tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="929f6-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="929f6-227">Definire un `ConfigurationValue` entità per l'archiviazione dei valori di configurazione nel database:</span><span class="sxs-lookup"><span data-stu-id="929f6-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="929f6-228">Aggiungere un `ConfigurationContext` archiviare e accedere ai valori configurati:</span><span class="sxs-lookup"><span data-stu-id="929f6-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="929f6-229">Creare una classe che implementa [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="929f6-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="929f6-230">Creare il provider di configurazione personalizzato ereditando dalla [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="929f6-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="929f6-231">Il provider di configurazione consente di inizializzare il database quando è vuoto:</span><span class="sxs-lookup"><span data-stu-id="929f6-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="929f6-232">Quando si esegue l'esempio, vengono visualizzati i valori evidenziati dal database ("value_from_ef_1" e "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="929f6-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="929f6-233">È possibile aggiungere un `EFConfigSource` metodo di estensione per l'aggiunta dell'origine di configurazione:</span><span class="sxs-lookup"><span data-stu-id="929f6-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="929f6-234">Il codice seguente viene illustrato come utilizzare l'oggetto personalizzato `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="929f6-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="929f6-235">Nota L'esempio aggiunge l'oggetto personalizzato `EFConfigProvider` quando il provider JSON, pertanto le impostazioni dal database sostituiranno le impostazioni dal *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="929f6-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="929f6-236">Con i seguenti *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="929f6-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="929f6-237">Viene visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="929f6-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="929f6-238">Provider di configurazione di riga di comando</span><span class="sxs-lookup"><span data-stu-id="929f6-238">CommandLine configuration provider</span></span>

<span data-ttu-id="929f6-239">L'esempio seguente Abilita il provider di configurazione della riga di comando ultimo:</span><span class="sxs-lookup"><span data-stu-id="929f6-239">The following sample enables the CommandLine configuration provider last:</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]

<span data-ttu-id="929f6-240">Per passare le impostazioni di configurazione, usare la seguente:</span><span class="sxs-lookup"><span data-stu-id="929f6-240">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="929f6-241">Che consente di visualizzare:</span><span class="sxs-lookup"><span data-stu-id="929f6-241">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="929f6-242">Il `GetSwitchMappings` metodo consente di utilizzare `-` anziché `/` ed e rimuove i prefissi di sottochiave iniziali.</span><span class="sxs-lookup"><span data-stu-id="929f6-242">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="929f6-243">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="929f6-243">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="929f6-244">Consente di visualizzare:</span><span class="sxs-lookup"><span data-stu-id="929f6-244">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="929f6-245">Argomenti della riga di comando devono includere un valore (può essere null).</span><span class="sxs-lookup"><span data-stu-id="929f6-245">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="929f6-246">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="929f6-246">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="929f6-247">È accettabile, ma</span><span class="sxs-lookup"><span data-stu-id="929f6-247">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="929f6-248">Restituisce un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="929f6-248">results in an exception.</span></span> <span data-ttu-id="929f6-249">Se si specifica un prefisso della riga di comando di - o -- per cui non esiste alcun mapping di parametro corrispondente, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="929f6-249">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="929f6-250">Il file Web. config</span><span class="sxs-lookup"><span data-stu-id="929f6-250">The web.config file</span></span>

<span data-ttu-id="929f6-251">Oggetto *Web. config* file è obbligatorio quando si ospita l'applicazione in IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="929f6-251">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="929f6-252">*Web. config* attiva AspNetCoreModule in IIS per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="929f6-252">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="929f6-253">Le impostazioni in *Web. config* abilitare AspNetCoreModule in IIS per avviare l'app e configurare altre impostazioni di IIS e i moduli.</span><span class="sxs-lookup"><span data-stu-id="929f6-253">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="929f6-254">Se si utilizza Visual Studio ed Elimina *Web. config*, Visual Studio creerà uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="929f6-254">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="929f6-255">Note aggiuntive</span><span class="sxs-lookup"><span data-stu-id="929f6-255">Additional notes</span></span>

* <span data-ttu-id="929f6-256">Dependency Injection (DI) non viene impostato fino a dopo `ConfigureServices` viene richiamato.</span><span class="sxs-lookup"><span data-stu-id="929f6-256">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="929f6-257">Il sistema di configurazione non è in grado di riconoscere SEN.</span><span class="sxs-lookup"><span data-stu-id="929f6-257">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="929f6-258">`IConfiguration`presenta due specializzazioni:</span><span class="sxs-lookup"><span data-stu-id="929f6-258">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="929f6-259">`IConfigurationRoot`Utilizzato per il nodo radice.</span><span class="sxs-lookup"><span data-stu-id="929f6-259">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="929f6-260">Può attivare un ricaricamento.</span><span class="sxs-lookup"><span data-stu-id="929f6-260">Can trigger a reload.</span></span>
  * <span data-ttu-id="929f6-261">`IConfigurationSection`Rappresenta una sezione dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="929f6-261">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="929f6-262">Il `GetSection` e `GetChildren` i metodi restituiscono un `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="929f6-262">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="929f6-263">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="929f6-263">Additional resources</span></span>

* [<span data-ttu-id="929f6-264">Uso di più ambienti</span><span class="sxs-lookup"><span data-stu-id="929f6-264">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="929f6-265">Archiviazione sicura di segreti dell'app durante lo sviluppo</span><span class="sxs-lookup"><span data-stu-id="929f6-265">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="929f6-266">Inserimento dipendenze</span><span class="sxs-lookup"><span data-stu-id="929f6-266">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="929f6-267">Provider di configurazione di Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="929f6-267">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
