---
title: Configurazione in ASP.NET Core
author: rick-anderson
description: "È possibile usare l'API di configurazione per configurare un'app ASP.NET Core in diversi modi."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="88380-103">Configurare un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88380-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="88380-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="88380-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="88380-105">L'API di configurazione fornisce un modo per configurare un'app Web ASP.NET Core in base a un elenco di coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="88380-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="88380-106">La configurazione viene letta in fase di esecuzione da più origini.</span><span class="sxs-lookup"><span data-stu-id="88380-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="88380-107">È possibile raggruppare queste coppie nome-valore in una gerarchia a più livelli.</span><span class="sxs-lookup"><span data-stu-id="88380-107">You can group these name-value pairs into a multi-level hierarchy.</span></span> 

<span data-ttu-id="88380-108">Esistono provider di configurazione per:</span><span class="sxs-lookup"><span data-stu-id="88380-108">There are configuration providers for:</span></span>

* <span data-ttu-id="88380-109">Formati di file (INI, JSON e XML)</span><span class="sxs-lookup"><span data-stu-id="88380-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="88380-110">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="88380-110">Command-line arguments</span></span>
* <span data-ttu-id="88380-111">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="88380-111">Environment variables</span></span>
* <span data-ttu-id="88380-112">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="88380-112">In-memory .NET objects</span></span>
* <span data-ttu-id="88380-113">Un archivio utente crittografato</span><span class="sxs-lookup"><span data-stu-id="88380-113">An encrypted user store</span></span>
* [<span data-ttu-id="88380-114">Insieme di credenziali delle chiavi di Azure</span><span class="sxs-lookup"><span data-stu-id="88380-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="88380-115">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="88380-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="88380-116">Ogni valore di configurazione è associato a una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="88380-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="88380-117">È disponibile il supporto di associazione incorporato per deserializzare le impostazioni in un oggetto personalizzato [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (una classe .NET semplice con proprietà).</span><span class="sxs-lookup"><span data-stu-id="88380-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="88380-118">Il modello di opzioni usa le classi di opzioni per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="88380-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="88380-119">Per altre informazioni sull'uso del modello di opzioni, vedere l'argomento [Opzioni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="88380-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="88380-120">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="88380-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="88380-121">Configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="88380-121">JSON configuration</span></span>

<span data-ttu-id="88380-122">La seguente app di console usa il provider di configurazione JSON:</span><span class="sxs-lookup"><span data-stu-id="88380-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="88380-123">L'app legge e visualizza le impostazioni di configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="88380-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="88380-124">La configurazione è costituita da un elenco gerarchico di coppie nome-valore in cui i nodi sono separati dai due punti.</span><span class="sxs-lookup"><span data-stu-id="88380-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="88380-125">Per recuperare un valore, accedere all'indicizzatore `Configuration` con la chiave dell'elemento corrispondente:</span><span class="sxs-lookup"><span data-stu-id="88380-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="88380-126">Per lavorare con le matrici nelle origini di configurazione in formato JSON, usare un indice di matrice come parte della stringa separata da due punti.</span><span class="sxs-lookup"><span data-stu-id="88380-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="88380-127">L'esempio seguente recupera il nome del primo elemento della matrice precedente `wizards`:</span><span class="sxs-lookup"><span data-stu-id="88380-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="88380-128">Le coppie nome-valore scritte nei provider predefiniti `Configuration` **non** sono salvate in modo permanente.</span><span class="sxs-lookup"><span data-stu-id="88380-128">Name-value pairs written to the built-in `Configuration` providers are **not** persisted.</span></span> <span data-ttu-id="88380-129">Tuttavia è possibile creare un provider personalizzato che salva i valori.</span><span class="sxs-lookup"><span data-stu-id="88380-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="88380-130">Vedere [provider di configurazione personalizzati](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="88380-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="88380-131">L'esempio precedente usa l'indicizzatore di configurazione per leggere i valori.</span><span class="sxs-lookup"><span data-stu-id="88380-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="88380-132">Per accedere alla configurazione fuori da `Startup`, usare il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="88380-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="88380-133">Per altre informazioni, vedere l'argomento [Opzioni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="88380-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="88380-134">Spesso si hanno diverse impostazioni di configurazione per ambienti diversi, ad esempio per sviluppo, test e produzione.</span><span class="sxs-lookup"><span data-stu-id="88380-134">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="88380-135">Il metodo di estensione `CreateDefaultBuilder` in un'app ASP.NET Core 2.x (o l'uso di `AddJsonFile` e `AddEnvironmentVariables` direttamente in un'app ASP.NET Core 1.x) aggiunge dei provider di configurazione per la lettura dei file JSON e le origini configurazione del sistema:</span><span class="sxs-lookup"><span data-stu-id="88380-135">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="88380-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="88380-136">*appsettings.json*</span></span>
* <span data-ttu-id="88380-137">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="88380-137">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="88380-138">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="88380-138">Environment variables</span></span>

<span data-ttu-id="88380-139">Vedere [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) per una spiegazione dei parametri.</span><span class="sxs-lookup"><span data-stu-id="88380-139">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="88380-140">`reloadOnChange` è supportato solo in ASP.NET Core 1.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="88380-140">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span> 

<span data-ttu-id="88380-141">Le origini di configurazione vengono lette nell'ordine in cui sono specificate.</span><span class="sxs-lookup"><span data-stu-id="88380-141">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="88380-142">Nel codice precedente, le variabili di ambiente vengono lette per ultime.</span><span class="sxs-lookup"><span data-stu-id="88380-142">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="88380-143">I valori di configurazione impostati tramite l'ambiente sostituiscono quelli impostati nei due provider precedenti.</span><span class="sxs-lookup"><span data-stu-id="88380-143">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="88380-144">L'ambiente è in genere impostato su `Development`, `Staging` o `Production`.</span><span class="sxs-lookup"><span data-stu-id="88380-144">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="88380-145">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="88380-145">See [Working with multiple environments](xref:fundamentals/environments) for more information.</span></span>

<span data-ttu-id="88380-146">Considerazioni sulla configurazione:</span><span class="sxs-lookup"><span data-stu-id="88380-146">Configuration considerations:</span></span>

* <span data-ttu-id="88380-147">`IOptionsSnapshot` può ricaricare i dati di configurazione in caso di modifiche.</span><span class="sxs-lookup"><span data-stu-id="88380-147">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="88380-148">Vedere [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="88380-148">See [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="88380-149">Le chiavi di configurazione non fanno distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="88380-149">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="88380-150">Specificare le variabili di ambiente per ultime in modo che l'ambiente locale possa eseguire l'override delle impostazioni nei file di configurazione distribuiti.</span><span class="sxs-lookup"><span data-stu-id="88380-150">Specify environment variables last so that the local environment can override settings in deployed configuration files.</span></span>
* <span data-ttu-id="88380-151">Non archiviare **mai** la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="88380-151">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="88380-152">Non usare i segreti di produzione nell'ambiente di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="88380-152">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="88380-153">Specificare invece i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati nel repository.</span><span class="sxs-lookup"><span data-stu-id="88380-153">Instead, specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="88380-154">Altre informazioni sull'[uso di più ambienti](xref:fundamentals/environments) e sulla gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="88380-154">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="88380-155">Se non è possibile usare i due punti (`:`) nelle variabili di ambiente nel sistema, sostituire i due punti (`:`) con un doppio carattere di sottolineatura (`__`).</span><span class="sxs-lookup"><span data-stu-id="88380-155">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="88380-156">Provider in memoria e associazione a una classe POCO</span><span class="sxs-lookup"><span data-stu-id="88380-156">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="88380-157">L'esempio seguente illustra come usare il provider in memoria e associarlo a una classe:</span><span class="sxs-lookup"><span data-stu-id="88380-157">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="88380-158">I valori di configurazione vengono restituiti come stringhe, ma l'associazione consente la costruzione di oggetti.</span><span class="sxs-lookup"><span data-stu-id="88380-158">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="88380-159">L'associazione consente di recuperare gli oggetti POCO o addirittura interi oggetti grafici.</span><span class="sxs-lookup"><span data-stu-id="88380-159">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="88380-160">GetValue</span><span class="sxs-lookup"><span data-stu-id="88380-160">GetValue</span></span>

<span data-ttu-id="88380-161">Nell'esempio seguente viene illustrato il metodo di estensione [GetValue&lt;T&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_):</span><span class="sxs-lookup"><span data-stu-id="88380-161">The following sample demonstrates the [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="88380-162">Il metodo `GetValue<T>` di ConfigurationBinder consente di specificare un valore predefinito (80 nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="88380-162">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="88380-163">`GetValue<T>` è per scenari semplici e non viene associato a intere sezioni.</span><span class="sxs-lookup"><span data-stu-id="88380-163">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="88380-164">`GetValue<T>` ottiene valori scalari da `GetSection(key).Value` convertiti in un tipo specifico.</span><span class="sxs-lookup"><span data-stu-id="88380-164">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="88380-165">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="88380-165">Bind to an object graph</span></span>

<span data-ttu-id="88380-166">È possibile impostare associazioni ricorsive a ogni oggetto di una classe.</span><span class="sxs-lookup"><span data-stu-id="88380-166">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="88380-167">Si consideri la classe `AppSettings` seguente:</span><span class="sxs-lookup"><span data-stu-id="88380-167">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="88380-168">L'esempio seguente è associato alla classe `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="88380-168">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="88380-169">**ASP.NET Core 1.1** e versioni successive può usare `Get<T>`, che funziona con intere sezioni.</span><span class="sxs-lookup"><span data-stu-id="88380-169">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="88380-170">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="88380-170">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="88380-171">L'esempio di codice seguente illustra come usare il metodo `Get<T>` con l'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="88380-171">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="88380-172">Tramite il file *appSettings.json* seguente:</span><span class="sxs-lookup"><span data-stu-id="88380-172">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="88380-173">Il programma visualizza `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="88380-173">The program displays `Height 11`.</span></span>

<span data-ttu-id="88380-174">Il codice seguente consente di seguire un unit test della configurazione:</span><span class="sxs-lookup"><span data-stu-id="88380-174">The following code can be used to unit test the configuration:</span></span>

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

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="88380-175">Creare un provider personalizzato di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="88380-175">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="88380-176">In questa sezione viene creato un provider di configurazione di base che legge le coppie nome-valore da un database tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="88380-176">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="88380-177">Definire un'entità `ConfigurationValue` per l'archiviazione dei valori di configurazione nel database:</span><span class="sxs-lookup"><span data-stu-id="88380-177">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="88380-178">Aggiungere `ConfigurationContext` per archiviare e accedere ai valori configurati:</span><span class="sxs-lookup"><span data-stu-id="88380-178">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="88380-179">Creare una classe che implementa [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="88380-179">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="88380-180">Creare il provider di configurazione personalizzato ereditando da [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="88380-180">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="88380-181">Il provider di configurazione inizializza il database quando è vuoto:</span><span class="sxs-lookup"><span data-stu-id="88380-181">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="88380-182">Quando l'esempio viene eseguito, vengono visualizzati i valori evidenziati del database ("value_from_ef_1" e "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="88380-182">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="88380-183">È possibile aggiungere un metodo di estensione `EFConfigSource` per aggiungere l'origine di configurazione:</span><span class="sxs-lookup"><span data-stu-id="88380-183">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="88380-184">L'esempio di codice seguente illustra come usare il metodo `EFConfigProvider` personalizzato:</span><span class="sxs-lookup"><span data-stu-id="88380-184">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="88380-185">Notare che l'esempio aggiunge l'oggetto personalizzato `EFConfigProvider` dopo il provider JSON, pertanto le impostazioni del database sostituiranno le impostazioni del file *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="88380-185">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="88380-186">Tramite il file *appSettings.json* seguente:</span><span class="sxs-lookup"><span data-stu-id="88380-186">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="88380-187">Verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="88380-187">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="88380-188">Provider di configurazione CommandLine</span><span class="sxs-lookup"><span data-stu-id="88380-188">CommandLine configuration provider</span></span>

<span data-ttu-id="88380-189">Il [provider di configurazione CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) riceve coppie chiave-valore di argomenti della riga di comando per la configurazione in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="88380-189">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="88380-190">Visualizzare o scaricare l'esempio di configurazione CommandLine</span><span class="sxs-lookup"><span data-stu-id="88380-190">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="88380-191">Impostazione e uso del provider di configurazione CommandLine</span><span class="sxs-lookup"><span data-stu-id="88380-191">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="88380-192">Configurazione di base</span><span class="sxs-lookup"><span data-stu-id="88380-192">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="88380-193">Per attivare la configurazione della riga di comando, chiamare il metodo di estensione `AddCommandLine` su un'istanza di [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="88380-193">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="88380-194">Eseguendo il codice, verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="88380-194">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="88380-195">Passando coppie chiave-valore degli argomenti nella riga di comando si modificano i valori di `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="88380-195">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="88380-196">Nella finestra della console viene visualizzato:</span><span class="sxs-lookup"><span data-stu-id="88380-196">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="88380-197">Per eseguire l'override della configurazione fornita da altri provider di configurazione con la configurazione della riga di comando, chiamare `AddCommandLine` per ultimo su `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="88380-197">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88380-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88380-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="88380-199">Le tipiche app ASP.NET Core 2.x usano il metodo statico `CreateDefaultBuilder` per compilare l'host:</span><span class="sxs-lookup"><span data-stu-id="88380-199">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="88380-200">`CreateDefaultBuilder` carica la configurazione facoltativa da *appSettings.json*, *appsettings.{Environment}.json*, [i segreti utente](xref:security/app-secrets) (nell'ambiente `Development`), le variabili di ambiente e gli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="88380-200">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="88380-201">Il provider di configurazione CommandLine è chiamato per ultimo.</span><span class="sxs-lookup"><span data-stu-id="88380-201">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="88380-202">Chiamando il provider per ultimo, gli argomenti della riga di comando passati in fase di esecuzione possono eseguire l'override della configurazione impostata da altri provider di configurazione chiamati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="88380-202">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="88380-203">Si noti che per i file *appsettings* `reloadOnChange` è abilitato.</span><span class="sxs-lookup"><span data-stu-id="88380-203">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="88380-204">Gli argomenti della riga di comando vengono sottoposti a override se un valore di configurazione corrispondente in un file *appsettings* viene modificato dopo l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="88380-204">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="88380-205">Come alternativa all'uso del metodo `CreateDefaultBuilder`, è supportata la creazione di un host tramite [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) e la compilazione manuale della configurazione con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) in ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="88380-205">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="88380-206">Per altre informazioni, vedere la scheda ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="88380-206">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88380-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88380-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="88380-208">Creare un oggetto [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chiamare il metodo `AddCommandLine` per usare il provider di configurazione CommandLine.</span><span class="sxs-lookup"><span data-stu-id="88380-208">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="88380-209">Chiamando il provider per ultimo, gli argomenti della riga di comando passati in fase di esecuzione possono eseguire l'override della configurazione impostata da altri provider di configurazione chiamati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="88380-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="88380-210">Applicare la configurazione a [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con il metodo `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="88380-210">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="88380-211">Argomenti</span><span class="sxs-lookup"><span data-stu-id="88380-211">Arguments</span></span>

<span data-ttu-id="88380-212">Gli argomenti passati alla riga di comando devono essere conformi a uno dei due formati indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="88380-212">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="88380-213">Formato dell'argomento</span><span class="sxs-lookup"><span data-stu-id="88380-213">Argument format</span></span>                                                     | <span data-ttu-id="88380-214">Esempio</span><span class="sxs-lookup"><span data-stu-id="88380-214">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="88380-215">Singolo argomento: una coppia chiave-valore separata da un segno di uguale (`=`)</span><span class="sxs-lookup"><span data-stu-id="88380-215">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="88380-216">Sequenza di due argomenti: una coppia chiave-valore separata da uno spazio</span><span class="sxs-lookup"><span data-stu-id="88380-216">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="88380-217">**Singolo argomento**</span><span class="sxs-lookup"><span data-stu-id="88380-217">**Single argument**</span></span>

<span data-ttu-id="88380-218">Il valore deve seguire un segno di uguale (`=`).</span><span class="sxs-lookup"><span data-stu-id="88380-218">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="88380-219">Il valore può essere null (ad esempio `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="88380-219">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="88380-220">La chiave può avere un prefisso.</span><span class="sxs-lookup"><span data-stu-id="88380-220">The key may have a prefix.</span></span>

| <span data-ttu-id="88380-221">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="88380-221">Key prefix</span></span>               | <span data-ttu-id="88380-222">Esempio</span><span class="sxs-lookup"><span data-stu-id="88380-222">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="88380-223">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="88380-223">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="88380-224">Trattino singolo (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="88380-224">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="88380-225">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="88380-225">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="88380-226">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="88380-226">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="88380-227">&#8224;Una chiave con un prefisso con trattino singolo (`-`) deve essere fornita nei [mapping di sostituzione](#switch-mappings), come descritto di seguito.</span><span class="sxs-lookup"><span data-stu-id="88380-227">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="88380-228">Comando di esempio:</span><span class="sxs-lookup"><span data-stu-id="88380-228">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="88380-229">Nota: se `-key1` non è presente nei [mapping di sostituzione](#switch-mappings) forniti al provider di configurazione, viene generata un'eccezione `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="88380-229">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="88380-230">**Sequenza di due argomenti**</span><span class="sxs-lookup"><span data-stu-id="88380-230">**Sequence of two arguments**</span></span>

<span data-ttu-id="88380-231">Il valore non può essere null e deve seguire la chiave separata da uno spazio.</span><span class="sxs-lookup"><span data-stu-id="88380-231">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="88380-232">La chiave deve avere un prefisso.</span><span class="sxs-lookup"><span data-stu-id="88380-232">The key must have a prefix.</span></span>

| <span data-ttu-id="88380-233">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="88380-233">Key prefix</span></span>               | <span data-ttu-id="88380-234">Esempio</span><span class="sxs-lookup"><span data-stu-id="88380-234">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="88380-235">Trattino singolo (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="88380-235">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="88380-236">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="88380-236">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="88380-237">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="88380-237">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="88380-238">&#8224;Una chiave con un prefisso con trattino singolo (`-`) deve essere fornita nei [mapping di sostituzione](#switch-mappings), come descritto di seguito.</span><span class="sxs-lookup"><span data-stu-id="88380-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="88380-239">Comando di esempio:</span><span class="sxs-lookup"><span data-stu-id="88380-239">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="88380-240">Nota: se `-key1` non è presente nei [mapping di sostituzione](#switch-mappings) forniti al provider di configurazione, viene generata un'eccezione `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="88380-240">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="88380-241">Chiavi duplicate</span><span class="sxs-lookup"><span data-stu-id="88380-241">Duplicate keys</span></span>

<span data-ttu-id="88380-242">Se vengono fornite chiavi duplicate, viene usata l'ultima coppia chiave-valore.</span><span class="sxs-lookup"><span data-stu-id="88380-242">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="88380-243">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="88380-243">Switch mappings</span></span>

<span data-ttu-id="88380-244">Quando si compila manualmente la configurazione con `ConfigurationBuilder`, è possibile fornire facoltativamente un dizionario di mapping di sostituzione per il metodo `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="88380-244">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="88380-245">I mapping di sostituzione consentono di fornire la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="88380-245">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="88380-246">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="88380-246">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="88380-247">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="88380-247">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="88380-248">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="88380-248">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="88380-249">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="88380-249">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="88380-250">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="88380-250">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="88380-251">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="88380-251">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="88380-252">Nell'esempio seguente, il metodo `GetSwitchMappings` consente agli argomenti della riga di comando di usare un prefisso di chiave con trattino singolo (`-`) ed evitare i prefissi iniziali nelle sottochiavi.</span><span class="sxs-lookup"><span data-stu-id="88380-252">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="88380-253">Senza fornire argomenti della riga di comando, il dizionario per `AddInMemoryCollection` imposta i valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="88380-253">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="88380-254">Eseguire l'app con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="88380-254">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="88380-255">Nella finestra della console viene visualizzato:</span><span class="sxs-lookup"><span data-stu-id="88380-255">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="88380-256">Per passare le impostazioni di configurazione, usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="88380-256">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="88380-257">Nella finestra della console viene visualizzato:</span><span class="sxs-lookup"><span data-stu-id="88380-257">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="88380-258">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="88380-258">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="88380-259">Chiave</span><span class="sxs-lookup"><span data-stu-id="88380-259">Key</span></span>            | <span data-ttu-id="88380-260">Valore</span><span class="sxs-lookup"><span data-stu-id="88380-260">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="88380-261">Per illustrare la sostituzione della chiave tramite il dizionario, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="88380-261">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="88380-262">Le chiavi della riga di comando vengono scambiate.</span><span class="sxs-lookup"><span data-stu-id="88380-262">The command-line keys are swapped.</span></span> <span data-ttu-id="88380-263">Nella finestra della console vengono visualizzati i valori di configurazione per `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="88380-263">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="88380-264">File web.config</span><span class="sxs-lookup"><span data-stu-id="88380-264">The web.config file</span></span>

<span data-ttu-id="88380-265">Un file *web.config* è obbligatorio quando si ospita l'app in IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="88380-265">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="88380-266">*Web.config* attiva AspNetCoreModule in IIS per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="88380-266">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="88380-267">Le impostazioni in *web.config* consentono ad AspNetCoreModule in IIS di avviare l'app e configurare altre impostazioni e moduli di IIS.</span><span class="sxs-lookup"><span data-stu-id="88380-267">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="88380-268">Se si usa Visual Studio e si elimina *web.config*, Visual Studio ne creerà uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="88380-268">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="88380-269">Note aggiuntive</span><span class="sxs-lookup"><span data-stu-id="88380-269">Additional notes</span></span>

* <span data-ttu-id="88380-270">L'inserimento delle dipendenze non viene impostato fino quando non viene richiamato `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="88380-270">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="88380-271">Il sistema di configurazione non riconosce l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="88380-271">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="88380-272">`IConfiguration` ha due specializzazioni:</span><span class="sxs-lookup"><span data-stu-id="88380-272">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="88380-273">`IConfigurationRoot`  Usata per il nodo radice.</span><span class="sxs-lookup"><span data-stu-id="88380-273">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="88380-274">Può attivare un ricaricamento.</span><span class="sxs-lookup"><span data-stu-id="88380-274">Can trigger a reload.</span></span>
  * <span data-ttu-id="88380-275">`IConfigurationSection`  Rappresenta una sezione dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="88380-275">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="88380-276">I metodi `GetSection` e `GetChildren` restituiscono un oggetto `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="88380-276">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="88380-277">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="88380-277">Additional resources</span></span>

* [<span data-ttu-id="88380-278">Opzioni</span><span class="sxs-lookup"><span data-stu-id="88380-278">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="88380-279">Uso di più ambienti</span><span class="sxs-lookup"><span data-stu-id="88380-279">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="88380-280">Archiviazione sicura di segreti dell'app durante lo sviluppo</span><span class="sxs-lookup"><span data-stu-id="88380-280">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="88380-281">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88380-281">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="88380-282">Inserimento dipendenze</span><span class="sxs-lookup"><span data-stu-id="88380-282">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* <span data-ttu-id="88380-283">[Azure Key Vault configuration provider](xref:security/key-vault-configuration) (Provider di configurazione di Azure Key Vault)</span><span class="sxs-lookup"><span data-stu-id="88380-283">[Azure Key Vault configuration provider](xref:security/key-vault-configuration)</span></span>
