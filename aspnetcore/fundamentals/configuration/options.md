---
title: Modello di opzioni in ASP.NET Core
author: guardrex
description: Informazioni su come usare il modello di opzioni per rappresentare gruppi di opzioni correlate nelle app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
uid: fundamentals/configuration/options
ms.openlocfilehash: 1f3625380d816c7d4df5a7a24b0ac146500330de
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447205"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="d25fc-103">Modello di opzioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d25fc-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="d25fc-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d25fc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d25fc-105">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="d25fc-106">Quando le [impostazioni di configurazione](xref:fundamentals/configuration/index) vengono isolate in base allo scenario in classi separate, l'app aderisce a due importanti principi di progettazione del software:</span><span class="sxs-lookup"><span data-stu-id="d25fc-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="d25fc-107">Il [principio di separazione dell'interfaccia (ISP) o l'Incapsulamento](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; scenari (classi) che dipendono dalle impostazioni di configurazione dipendono solo dalle impostazioni di configurazione che usano.</span><span class="sxs-lookup"><span data-stu-id="d25fc-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="d25fc-108">La [separazione delle problematiche](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; le impostazioni per parti diverse dell'app non sono dipendenti o collegati tra loro.</span><span class="sxs-lookup"><span data-stu-id="d25fc-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="d25fc-109">Le opzioni offrono anche un meccanismo per convalidare i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="d25fc-110">Per altre informazioni, vedere la sezione [Opzioni di convalida](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="d25fc-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="d25fc-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d25fc-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="d25fc-112">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="d25fc-112">Package</span></span>

<span data-ttu-id="d25fc-113">Al pacchetto [Microsoft. Extensions. Options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) viene fatto riferimento in modo implicito nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d25fc-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="d25fc-114">Interfacce per le opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-114">Options interfaces</span></span>

<span data-ttu-id="d25fc-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> consente di recuperare le opzioni e gestire le notifiche di opzioni per le istanze `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="d25fc-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supporta gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="d25fc-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="d25fc-117">Notifiche di modifica</span><span class="sxs-lookup"><span data-stu-id="d25fc-117">Change notifications</span></span>
* [<span data-ttu-id="d25fc-118">Opzioni denominate</span><span class="sxs-lookup"><span data-stu-id="d25fc-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="d25fc-119">Configurazione ricaricabile</span><span class="sxs-lookup"><span data-stu-id="d25fc-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="d25fc-120">Invalidamento selettivo di opzioni (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="d25fc-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="d25fc-121">Gli scenari di [post-configurazione](#options-post-configuration) consentono di impostare o modificare le opzioni dopo la configurazione di tutte le <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="d25fc-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> è responsabile della creazione di nuove istanze di opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="d25fc-123">Include un singolo metodo <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="d25fc-124">L'implementazione predefinita accetta tutte le interfacce <xref:Microsoft.Extensions.Options.IConfigureOptions%601> e <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> registrate ed esegue tutte le configurazioni, seguite dalla post-configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="d25fc-125">Fa distinzione tra <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> e <xref:Microsoft.Extensions.Options.IConfigureOptions%601> e chiama solo l'interfaccia appropriata.</span><span class="sxs-lookup"><span data-stu-id="d25fc-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="d25fc-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> viene usata da <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> per memorizzare nella cache le istanze di `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="d25fc-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalida le istanze delle opzioni nel monitoraggio in modo che il valore venga ricalcolato (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="d25fc-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="d25fc-128">I valori possono essere introdotti manualmente con <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="d25fc-129">Il metodo <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> viene usato quando tutte le istanze denominate devono essere ricreate su richiesta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="d25fc-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> è utile negli scenari in cui le opzioni devono essere ricalcolate a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="d25fc-131">Per altre informazioni, vedere la sezione [Ricaricare i dati di configurazione con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="d25fc-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="d25fc-132"><xref:Microsoft.Extensions.Options.IOptions%601> può essere usata a supporto delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="d25fc-133">Tuttavia <xref:Microsoft.Extensions.Options.IOptions%601> non supporta gli scenari precedenti di <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="d25fc-134">È possibile continuare a usare <xref:Microsoft.Extensions.Options.IOptions%601> in framework e librerie esistenti che già usano l'interfaccia <xref:Microsoft.Extensions.Options.IOptions%601> e non richiedono gli scenari forniti da <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="d25fc-135">Configurazione delle opzioni generali</span><span class="sxs-lookup"><span data-stu-id="d25fc-135">General options configuration</span></span>

<span data-ttu-id="d25fc-136">La configurazione delle opzioni generali è illustrata nell'esempio 1 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-136">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="d25fc-137">Una classe di opzioni deve essere non astratta con un costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="d25fc-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="d25fc-138">La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="d25fc-139">Sebbene l'impostazione dei valori predefiniti sia facoltativa, il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="d25fc-140">`Option2` ha un valore impostato tramite l'inizializzazione diretta della proprietà (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="d25fc-141">La classe `MyOptions` viene aggiunta al contenitore di servizi con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> e associata alla configurazione:</span><span class="sxs-lookup"><span data-stu-id="d25fc-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="d25fc-142">Il modello di pagina seguente usa l'[inserimento delle dipendenze del costruttore](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> per accedere alle impostazioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="d25fc-143">Il file *appsettings.json* dell'esempio specifica i valori di `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="d25fc-144">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="d25fc-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="d25fc-145">Se si usa un <xref:System.Configuration.ConfigurationBuilder> personalizzato per caricare la configurazione delle opzioni da un file di impostazioni, verificare che il percorso base sia impostato correttamente:</span><span class="sxs-lookup"><span data-stu-id="d25fc-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="d25fc-146">L'impostazione esplicita del percorso base non è necessaria se si carica la configurazione delle opzioni dal file di impostazioni tramite <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="d25fc-147">Configurare opzioni semplici con un delegato</span><span class="sxs-lookup"><span data-stu-id="d25fc-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="d25fc-148">La configurazione di semplici opzioni con un delegato è illustrata nell'esempio 2 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-148">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="d25fc-149">Usare un delegato per impostare i valori delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-149">Use a delegate to set options values.</span></span> <span data-ttu-id="d25fc-150">L'app di esempio usa la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="d25fc-151">Nel codice seguente viene aggiunto un secondo servizio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="d25fc-152">Viene usato un delegato per configurare l'associazione a `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="d25fc-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d25fc-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="d25fc-154">È possibile aggiungere più provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="d25fc-155">I provider di configurazione sono disponibili dai pacchetti NuGet e vengono applicati nell'ordine in cui sono registrati.</span><span class="sxs-lookup"><span data-stu-id="d25fc-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="d25fc-156">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="d25fc-157">Ogni chiamata a <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> aggiunge un servizio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="d25fc-158">Nell'esempio precedente i valori di `Option1` e `Option2` sono entrambi specificati in *appsettings.json*, mentre i valori di `Option1` e `Option2` sono sottoposti a override dal delegato configurato.</span><span class="sxs-lookup"><span data-stu-id="d25fc-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="d25fc-159">Quando sono abilitati più servizi di configurazione, l'origine di configurazione più recente specificata *ha priorità* e imposta il valore di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="d25fc-160">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="d25fc-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="d25fc-161">Configurazione delle opzioni secondarie</span><span class="sxs-lookup"><span data-stu-id="d25fc-161">Suboptions configuration</span></span>

<span data-ttu-id="d25fc-162">La configurazione delle sottoopzioni è illustrata nell'esempio 3 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-162">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="d25fc-163">Le app devono creare classi di opzioni che riguardano gruppi di scenari (classi) specifici nell'app.</span><span class="sxs-lookup"><span data-stu-id="d25fc-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="d25fc-164">Le parti dell'app che richiedono valori di configurazione devono avere accesso solo ai valori di configurazione necessari.</span><span class="sxs-lookup"><span data-stu-id="d25fc-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="d25fc-165">Durante l'associazione delle opzioni alla configurazione, ogni proprietà del tipo di opzioni viene associata a una chiave di configurazione con formato `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="d25fc-166">Ad esempio, la proprietà `MyOptions.Option1` viene associata alla chiave `Option1`, che viene letta dalla proprietà `option1` in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="d25fc-167">Nel codice seguente viene aggiunto un terzo servizio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="d25fc-168">`MySubOptions` viene associato alla sezione `subsection` del file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d25fc-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="d25fc-169">Il metodo `GetSection` richiede lo spazio dei nomi <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="d25fc-170">Il file *appsettings.json* dell'esempio definisce un membro `subsection` con chiavi per `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="d25fc-171">La classe `MySubOptions` definisce le proprietà `SubOption1` e `SubOption2` per contenere i valori delle opzioni (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="d25fc-172">Il metodo `OnGet` del modello di pagina restituisce una stringa con i valori delle opzioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="d25fc-173">Quando viene eseguita l'app, il metodo `OnGet` restituisce una stringa che mostra i valori delle classi delle opzioni secondarie:</span><span class="sxs-lookup"><span data-stu-id="d25fc-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="d25fc-174">Inserimento di opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-174">Options injection</span></span>

<span data-ttu-id="d25fc-175">L'inserimento di opzioni viene illustrato come esempio 4 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-175">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="d25fc-176">Inserire <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in:</span><span class="sxs-lookup"><span data-stu-id="d25fc-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="d25fc-177">Una pagina Razor o una visualizzazione MVC con la direttiva [`@inject`](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="d25fc-177">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="d25fc-178">Modello di pagina o di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-178">A page or view model.</span></span>

<span data-ttu-id="d25fc-179">L'esempio seguente dall'app di esempio inserisce <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in un modello di pagina (*pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="d25fc-180">L'app di esempio mostra come inserire `IOptionsMonitor<MyOptions>` con una direttiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="d25fc-181">Quando l'app viene eseguita, i valori delle opzioni sono visibili nella pagina di cui è stato eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="d25fc-181">When the app is run, the options values are shown in the rendered page:</span></span>

![I valori delle opzioni Option1: value1_from_json e Option2: -1 vengono caricati dal modello e tramite inserimento nella visualizzazione.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="d25fc-183">Ricaricare i dati di configurazione con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="d25fc-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="d25fc-184">Il ricaricamento dei dati di configurazione con <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> viene illustrato nell'esempio 5 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="d25fc-185">Utilizzando <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, le opzioni vengono calcolate una volta per ogni richiesta in caso di accesso e memorizzati nella cache per la durata della richiesta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="d25fc-186">La differenza tra `IOptionsMonitor` e `IOptionsSnapshot` è che:</span><span class="sxs-lookup"><span data-stu-id="d25fc-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="d25fc-187">`IOptionsMonitor` è un [servizio singleton](xref:fundamentals/dependency-injection#singleton) che recupera i valori correnti delle opzioni in qualsiasi momento, operazione particolarmente utile nelle dipendenze singleton.</span><span class="sxs-lookup"><span data-stu-id="d25fc-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="d25fc-188">`IOptionsSnapshot` è un [servizio con ambito](xref:fundamentals/dependency-injection#scoped) e fornisce uno snapshot delle opzioni nel momento in cui viene costruito l'oggetto `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="d25fc-189">Gli snapshot delle opzioni sono progettati per l'uso con dipendenze temporanee e con ambito.</span><span class="sxs-lookup"><span data-stu-id="d25fc-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="d25fc-190">L'esempio seguente illustra la creazione di un nuovo <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> dopo le modifiche ad *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="d25fc-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="d25fc-191">Più richieste al server restituiscono valori costanti forniti dal file *appsettings.json* fino a quando il file non viene modificato e la configurazione non viene ricaricata.</span><span class="sxs-lookup"><span data-stu-id="d25fc-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="d25fc-192">L'immagine seguente illustra i valori iniziali `option1` e `option2` caricati dal file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d25fc-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="d25fc-193">Modificare i valori nel file *appsettings.json* in `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="d25fc-194">Salvare il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="d25fc-195">Aggiornare il browser per visualizzare i valori delle opzioni aggiornati:</span><span class="sxs-lookup"><span data-stu-id="d25fc-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="d25fc-196">Supporto delle opzioni denominate con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="d25fc-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="d25fc-197">Il supporto delle opzioni denominate con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> viene illustrato come esempio 6 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="d25fc-198">Il supporto delle opzioni denominate consente all'app di distinguere tra le configurazioni delle opzioni denominate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-198">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="d25fc-199">Nell'app di esempio, le opzioni denominate sono dichiarate con [OptionsServiceCollectionExtensions. Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), che chiama il [ConfigureNamedOptions\<>. Configurare](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) il metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="d25fc-200">Le opzioni denominate fanno distinzione maiuscole/minuscole</span><span class="sxs-lookup"><span data-stu-id="d25fc-200">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="d25fc-201">L'app di esempio accede alle opzioni denominate con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-201">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="d25fc-202">Eseguendo l'app di esempio, vengono restituite le opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="d25fc-202">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="d25fc-203">I valori `named_options_1` vengono specificati dalla configurazione, caricata dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-203">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="d25fc-204">I valori `named_options_2` sono specificati da:</span><span class="sxs-lookup"><span data-stu-id="d25fc-204">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="d25fc-205">Delegato `named_options_2` in `ConfigureServices` per `Option1`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-205">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="d25fc-206">Valore predefinito per `Option2` specificato dalla classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-206">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="d25fc-207">Configurare tutte le opzioni con il metodo ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="d25fc-207">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="d25fc-208">Configurare tutte le istanze delle opzioni con il metodo <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-208">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="d25fc-209">Il codice seguente configura `Option1` per tutte le istanze di configurazione con un valore comune.</span><span class="sxs-lookup"><span data-stu-id="d25fc-209">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="d25fc-210">Aggiungere manualmente il codice seguente al metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-210">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="d25fc-211">L'esecuzione dell'app di esempio dopo l'aggiunta del codice produce il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="d25fc-211">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="d25fc-212">Tutte le opzioni sono istanze denominate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-212">All options are named instances.</span></span> <span data-ttu-id="d25fc-213">Le istanze di <xref:Microsoft.Extensions.Options.IConfigureOptions%601> esistenti sono trattate come se fossero indirizzate all'istanza di `Options.DefaultName`, ovvero `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-213">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="d25fc-214"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementa anche <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-214"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="d25fc-215">L'implementazione predefinita di <xref:Microsoft.Extensions.Options.IOptionsFactory%601> include la logica per usarle in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="d25fc-215">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="d25fc-216">L'opzione denominata `null` viene usata per avere come destinazione tutte le istanze denominate anziché un'istanza specifica denominata (questa convenzione è usata da <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> e <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>).</span><span class="sxs-lookup"><span data-stu-id="d25fc-216">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="d25fc-217">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="d25fc-217">OptionsBuilder API</span></span>

<span data-ttu-id="d25fc-218"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> viene usata per configurare le istanze di `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-218"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="d25fc-219">`OptionsBuilder` semplifica la creazione di opzioni denominate perché è costituita da un singolo parametro nella chiamata iniziale ad `AddOptions<TOptions>(string optionsName)` invece di essere visualizzata in tutte le chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="d25fc-219">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="d25fc-220">La convalida delle opzioni e gli overload `ConfigureOptions` che accettano le dipendenze dei servizi sono disponibili solo tramite `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-220">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="d25fc-221">Usare i servizi di inserimento delle dipendenze per configurare le opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-221">Use DI services to configure options</span></span>

<span data-ttu-id="d25fc-222">È possibile accedere ad altri servizi dall'inserimento delle dipendenze durante la configurazione delle opzioni in due modi:</span><span class="sxs-lookup"><span data-stu-id="d25fc-222">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="d25fc-223">Passare un delegato di configurazione a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) in [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="d25fc-223">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="d25fc-224">`OptionsBuilder<TOptions>` fornisce overload di [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) che consentono l'uso di un massimo di cinque servizi per la configurazione delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="d25fc-224">`OptionsBuilder<TOptions>` provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="d25fc-225">Creare un tipo personalizzato che implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> e registrare il tipo come servizio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-225">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="d25fc-226">È consigliabile passare un delegato di configurazione a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) perché la creazione di un servizio è più complessa.</span><span class="sxs-lookup"><span data-stu-id="d25fc-226">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="d25fc-227">La creazione di un tipo personalizzato equivale alle operazioni che il framework esegue automaticamente quando si usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="d25fc-227">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="d25fc-228">La chiamata a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra un'interfaccia <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> generica temporanea, che ha un costruttore che accetta i tipi di servizi generici specificati.</span><span class="sxs-lookup"><span data-stu-id="d25fc-228">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="d25fc-229">Convalida delle opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-229">Options validation</span></span>

<span data-ttu-id="d25fc-230">La convalida le opzioni consente di convalidare le opzioni quando vengono configurate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-230">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="d25fc-231">Chiamare `Validate` con un metodo di convalida che restituisce `true` se le opzioni sono valide e `false` se non sono valide:</span><span class="sxs-lookup"><span data-stu-id="d25fc-231">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="d25fc-232">L'esempio precedente imposta l'istanza di opzioni denominata su `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-232">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="d25fc-233">L'istanza di opzioni predefinita è `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-233">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="d25fc-234">La convalida viene eseguita quando viene creata l'istanza di opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-234">Validation runs when the options instance is created.</span></span> <span data-ttu-id="d25fc-235">Si garantisce che un'istanza di opzioni superi la convalida la prima volta che si accede.</span><span class="sxs-lookup"><span data-stu-id="d25fc-235">An options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d25fc-236">La convalida delle opzioni non protegge le modifiche delle opzioni dopo la creazione dell'istanza di Options.</span><span class="sxs-lookup"><span data-stu-id="d25fc-236">Options validation doesn't guard against options modifications after the options instance is created.</span></span> <span data-ttu-id="d25fc-237">Le opzioni `IOptionsSnapshot`, ad esempio, vengono create e convalidate una volta per ogni richiesta quando si accede per la prima volta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-237">For example, `IOptionsSnapshot` options are created and validated once per request when the options are first accessed.</span></span> <span data-ttu-id="d25fc-238">Le opzioni di `IOptionsSnapshot` non vengono convalidate di nuovo nei tentativi di accesso successivi *per la stessa richiesta*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-238">The `IOptionsSnapshot` options aren't validated again on subsequent access attempts *for the same request*.</span></span>

<span data-ttu-id="d25fc-239">Il metodo `Validate` accetta `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-239">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="d25fc-240">Per personalizzare completamente la convalida, implementare `IValidateOptions<TOptions>`, che consente:</span><span class="sxs-lookup"><span data-stu-id="d25fc-240">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="d25fc-241">La convalida di più tipi di opzioni: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="d25fc-241">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="d25fc-242">La convalida che dipende da un altro tipo di opzione: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="d25fc-242">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="d25fc-243">`IValidateOptions` convalida:</span><span class="sxs-lookup"><span data-stu-id="d25fc-243">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="d25fc-244">Un'istanza di opzioni denominata specifica.</span><span class="sxs-lookup"><span data-stu-id="d25fc-244">A specific named options instance.</span></span>
* <span data-ttu-id="d25fc-245">Tutte le opzioni quando `name` è `null`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-245">All options when `name` is `null`.</span></span>

<span data-ttu-id="d25fc-246">Restituire `ValidateOptionsResult` dall'implementazione dell'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="d25fc-246">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="d25fc-247">La convalida basata sull'annotazione dei dati è disponibile dal pacchetto [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) chiamando il metodo <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> su `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-247">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="d25fc-248">`Microsoft.Extensions.Options.DataAnnotations` viene fatto riferimento in modo implicito nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d25fc-248">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.Extensions.DependencyInjection;

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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="d25fc-249">La convalida eager (fallimento e risposta immediata agli errori all'avvio) è pianificata per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="d25fc-249">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="d25fc-250">Post-configurazione delle opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-250">Options post-configuration</span></span>

<span data-ttu-id="d25fc-251">Impostare la post-configurazione con <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-251">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="d25fc-252">La post-configurazione viene eseguita dopo il completamento della configurazione di tutte le <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="d25fc-252">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="d25fc-253"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> è disponibile per la post-configurazione delle opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="d25fc-253"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="d25fc-254">Usare <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> per la post-configurazione di tutte le istanze di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d25fc-254">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="d25fc-255">Accesso alle opzioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="d25fc-255">Accessing options during startup</span></span>

<span data-ttu-id="d25fc-256"><xref:Microsoft.Extensions.Options.IOptions%601> e <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> possono essere usate in `Startup.Configure`, perché i servizi vengono compilati prima dell'esecuzione del metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-256"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="d25fc-257">Non usare <xref:Microsoft.Extensions.Options.IOptions%601> oppure <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-257">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d25fc-258">Le opzioni potrebbero avere uno stato incoerente a causa dell'ordinamento delle registrazioni dei servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-258">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="d25fc-259">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-259">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="d25fc-260">Quando le [impostazioni di configurazione](xref:fundamentals/configuration/index) vengono isolate in base allo scenario in classi separate, l'app aderisce a due importanti principi di progettazione del software:</span><span class="sxs-lookup"><span data-stu-id="d25fc-260">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="d25fc-261">Il [principio di separazione dell'interfaccia (ISP) o l'Incapsulamento](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; scenari (classi) che dipendono dalle impostazioni di configurazione dipendono solo dalle impostazioni di configurazione che usano.</span><span class="sxs-lookup"><span data-stu-id="d25fc-261">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="d25fc-262">La [separazione delle problematiche](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; le impostazioni per parti diverse dell'app non sono dipendenti o collegati tra loro.</span><span class="sxs-lookup"><span data-stu-id="d25fc-262">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="d25fc-263">Le opzioni offrono anche un meccanismo per convalidare i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-263">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="d25fc-264">Per altre informazioni, vedere la sezione [Opzioni di convalida](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="d25fc-264">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="d25fc-265">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d25fc-265">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d25fc-266">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="d25fc-266">Prerequisites</span></span>

<span data-ttu-id="d25fc-267">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="d25fc-267">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="d25fc-268">Interfacce per le opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-268">Options interfaces</span></span>

<span data-ttu-id="d25fc-269"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> consente di recuperare le opzioni e gestire le notifiche di opzioni per le istanze `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-269"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="d25fc-270"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supporta gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="d25fc-270"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="d25fc-271">Notifiche di modifica</span><span class="sxs-lookup"><span data-stu-id="d25fc-271">Change notifications</span></span>
* [<span data-ttu-id="d25fc-272">Opzioni denominate</span><span class="sxs-lookup"><span data-stu-id="d25fc-272">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="d25fc-273">Configurazione ricaricabile</span><span class="sxs-lookup"><span data-stu-id="d25fc-273">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="d25fc-274">Invalidamento selettivo di opzioni (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="d25fc-274">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="d25fc-275">Gli scenari di [post-configurazione](#options-post-configuration) consentono di impostare o modificare le opzioni dopo la configurazione di tutte le <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-275">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="d25fc-276"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> è responsabile della creazione di nuove istanze di opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-276"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="d25fc-277">Include un singolo metodo <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-277">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="d25fc-278">L'implementazione predefinita accetta tutte le interfacce <xref:Microsoft.Extensions.Options.IConfigureOptions%601> e <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> registrate ed esegue tutte le configurazioni, seguite dalla post-configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-278">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="d25fc-279">Fa distinzione tra <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> e <xref:Microsoft.Extensions.Options.IConfigureOptions%601> e chiama solo l'interfaccia appropriata.</span><span class="sxs-lookup"><span data-stu-id="d25fc-279">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="d25fc-280"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> viene usata da <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> per memorizzare nella cache le istanze di `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-280"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="d25fc-281"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalida le istanze delle opzioni nel monitoraggio in modo che il valore venga ricalcolato (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="d25fc-281">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="d25fc-282">I valori possono essere introdotti manualmente con <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-282">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="d25fc-283">Il metodo <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> viene usato quando tutte le istanze denominate devono essere ricreate su richiesta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-283">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="d25fc-284"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> è utile negli scenari in cui le opzioni devono essere ricalcolate a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-284"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="d25fc-285">Per altre informazioni, vedere la sezione [Ricaricare i dati di configurazione con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="d25fc-285">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="d25fc-286"><xref:Microsoft.Extensions.Options.IOptions%601> può essere usata a supporto delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-286"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="d25fc-287">Tuttavia <xref:Microsoft.Extensions.Options.IOptions%601> non supporta gli scenari precedenti di <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-287">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="d25fc-288">È possibile continuare a usare <xref:Microsoft.Extensions.Options.IOptions%601> in framework e librerie esistenti che già usano l'interfaccia <xref:Microsoft.Extensions.Options.IOptions%601> e non richiedono gli scenari forniti da <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-288">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="d25fc-289">Configurazione delle opzioni generali</span><span class="sxs-lookup"><span data-stu-id="d25fc-289">General options configuration</span></span>

<span data-ttu-id="d25fc-290">La configurazione delle opzioni generali è illustrata nell'esempio 1 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-290">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="d25fc-291">Una classe di opzioni deve essere non astratta con un costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="d25fc-291">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="d25fc-292">La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-292">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="d25fc-293">Sebbene l'impostazione dei valori predefiniti sia facoltativa, il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-293">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="d25fc-294">`Option2` ha un valore impostato tramite l'inizializzazione diretta della proprietà (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-294">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="d25fc-295">La classe `MyOptions` viene aggiunta al contenitore di servizi con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> e associata alla configurazione:</span><span class="sxs-lookup"><span data-stu-id="d25fc-295">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="d25fc-296">Il modello di pagina seguente usa l'[inserimento delle dipendenze del costruttore](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> per accedere alle impostazioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-296">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="d25fc-297">Il file *appsettings.json* dell'esempio specifica i valori di `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-297">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="d25fc-298">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="d25fc-298">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="d25fc-299">Se si usa un <xref:System.Configuration.ConfigurationBuilder> personalizzato per caricare la configurazione delle opzioni da un file di impostazioni, verificare che il percorso base sia impostato correttamente:</span><span class="sxs-lookup"><span data-stu-id="d25fc-299">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="d25fc-300">L'impostazione esplicita del percorso base non è necessaria se si carica la configurazione delle opzioni dal file di impostazioni tramite <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-300">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="d25fc-301">Configurare opzioni semplici con un delegato</span><span class="sxs-lookup"><span data-stu-id="d25fc-301">Configure simple options with a delegate</span></span>

<span data-ttu-id="d25fc-302">La configurazione di semplici opzioni con un delegato è illustrata nell'esempio 2 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-302">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="d25fc-303">Usare un delegato per impostare i valori delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-303">Use a delegate to set options values.</span></span> <span data-ttu-id="d25fc-304">L'app di esempio usa la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-304">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="d25fc-305">Nel codice seguente viene aggiunto un secondo servizio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-305">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="d25fc-306">Viene usato un delegato per configurare l'associazione a `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-306">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="d25fc-307">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d25fc-307">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="d25fc-308">È possibile aggiungere più provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-308">You can add multiple configuration providers.</span></span> <span data-ttu-id="d25fc-309">I provider di configurazione sono disponibili dai pacchetti NuGet e vengono applicati nell'ordine in cui sono registrati.</span><span class="sxs-lookup"><span data-stu-id="d25fc-309">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="d25fc-310">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-310">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="d25fc-311">Ogni chiamata a <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> aggiunge un servizio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-311">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="d25fc-312">Nell'esempio precedente i valori di `Option1` e `Option2` sono entrambi specificati in *appsettings.json*, mentre i valori di `Option1` e `Option2` sono sottoposti a override dal delegato configurato.</span><span class="sxs-lookup"><span data-stu-id="d25fc-312">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="d25fc-313">Quando sono abilitati più servizi di configurazione, l'origine di configurazione più recente specificata *ha priorità* e imposta il valore di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-313">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="d25fc-314">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="d25fc-314">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="d25fc-315">Configurazione delle opzioni secondarie</span><span class="sxs-lookup"><span data-stu-id="d25fc-315">Suboptions configuration</span></span>

<span data-ttu-id="d25fc-316">La configurazione delle sottoopzioni è illustrata nell'esempio 3 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-316">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="d25fc-317">Le app devono creare classi di opzioni che riguardano gruppi di scenari (classi) specifici nell'app.</span><span class="sxs-lookup"><span data-stu-id="d25fc-317">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="d25fc-318">Le parti dell'app che richiedono valori di configurazione devono avere accesso solo ai valori di configurazione necessari.</span><span class="sxs-lookup"><span data-stu-id="d25fc-318">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="d25fc-319">Durante l'associazione delle opzioni alla configurazione, ogni proprietà del tipo di opzioni viene associata a una chiave di configurazione con formato `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-319">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="d25fc-320">Ad esempio, la proprietà `MyOptions.Option1` viene associata alla chiave `Option1`, che viene letta dalla proprietà `option1` in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-320">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="d25fc-321">Nel codice seguente viene aggiunto un terzo servizio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-321">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="d25fc-322">`MySubOptions` viene associato alla sezione `subsection` del file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d25fc-322">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="d25fc-323">Il metodo `GetSection` richiede lo spazio dei nomi <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-323">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="d25fc-324">Il file *appsettings.json* dell'esempio definisce un membro `subsection` con chiavi per `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-324">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="d25fc-325">La classe `MySubOptions` definisce le proprietà `SubOption1` e `SubOption2` per contenere i valori delle opzioni (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-325">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="d25fc-326">Il metodo `OnGet` del modello di pagina restituisce una stringa con i valori delle opzioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-326">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="d25fc-327">Quando viene eseguita l'app, il metodo `OnGet` restituisce una stringa che mostra i valori delle classi delle opzioni secondarie:</span><span class="sxs-lookup"><span data-stu-id="d25fc-327">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="d25fc-328">Inserimento di opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-328">Options injection</span></span>

<span data-ttu-id="d25fc-329">L'inserimento di opzioni viene illustrato come esempio 4 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-329">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="d25fc-330">Inserire <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in:</span><span class="sxs-lookup"><span data-stu-id="d25fc-330">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="d25fc-331">Una pagina Razor o una visualizzazione MVC con la direttiva [`@inject`](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="d25fc-331">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="d25fc-332">Modello di pagina o di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-332">A page or view model.</span></span>

<span data-ttu-id="d25fc-333">L'esempio seguente dall'app di esempio inserisce <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in un modello di pagina (*pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-333">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="d25fc-334">L'app di esempio mostra come inserire `IOptionsMonitor<MyOptions>` con una direttiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-334">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="d25fc-335">Quando l'app viene eseguita, i valori delle opzioni sono visibili nella pagina di cui è stato eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="d25fc-335">When the app is run, the options values are shown in the rendered page:</span></span>

![I valori delle opzioni Option1: value1_from_json e Option2: -1 vengono caricati dal modello e tramite inserimento nella visualizzazione.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="d25fc-337">Ricaricare i dati di configurazione con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="d25fc-337">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="d25fc-338">Il ricaricamento dei dati di configurazione con <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> viene illustrato nell'esempio 5 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-338">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="d25fc-339">Utilizzando <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, le opzioni vengono calcolate una volta per ogni richiesta in caso di accesso e memorizzati nella cache per la durata della richiesta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-339">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="d25fc-340">La differenza tra `IOptionsMonitor` e `IOptionsSnapshot` è che:</span><span class="sxs-lookup"><span data-stu-id="d25fc-340">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="d25fc-341">`IOptionsMonitor` è un [servizio singleton](xref:fundamentals/dependency-injection#singleton) che recupera i valori correnti delle opzioni in qualsiasi momento, operazione particolarmente utile nelle dipendenze singleton.</span><span class="sxs-lookup"><span data-stu-id="d25fc-341">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="d25fc-342">`IOptionsSnapshot` è un [servizio con ambito](xref:fundamentals/dependency-injection#scoped) e fornisce uno snapshot delle opzioni nel momento in cui viene costruito l'oggetto `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-342">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="d25fc-343">Gli snapshot delle opzioni sono progettati per l'uso con dipendenze temporanee e con ambito.</span><span class="sxs-lookup"><span data-stu-id="d25fc-343">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="d25fc-344">L'esempio seguente illustra la creazione di un nuovo <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> dopo le modifiche ad *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="d25fc-344">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="d25fc-345">Più richieste al server restituiscono valori costanti forniti dal file *appsettings.json* fino a quando il file non viene modificato e la configurazione non viene ricaricata.</span><span class="sxs-lookup"><span data-stu-id="d25fc-345">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="d25fc-346">L'immagine seguente illustra i valori iniziali `option1` e `option2` caricati dal file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d25fc-346">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="d25fc-347">Modificare i valori nel file *appsettings.json* in `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-347">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="d25fc-348">Salvare il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-348">Save the *appsettings.json* file.</span></span> <span data-ttu-id="d25fc-349">Aggiornare il browser per visualizzare i valori delle opzioni aggiornati:</span><span class="sxs-lookup"><span data-stu-id="d25fc-349">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="d25fc-350">Supporto delle opzioni denominate con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="d25fc-350">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="d25fc-351">Il supporto delle opzioni denominate con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> viene illustrato come esempio 6 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-351">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="d25fc-352">Il supporto delle opzioni denominate consente all'app di distinguere tra le configurazioni delle opzioni denominate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-352">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="d25fc-353">Nell'app di esempio, le opzioni denominate sono dichiarate con [OptionsServiceCollectionExtensions. Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), che chiama il [ConfigureNamedOptions\<>. Configurare](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) il metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-353">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="d25fc-354">Le opzioni denominate fanno distinzione maiuscole/minuscole</span><span class="sxs-lookup"><span data-stu-id="d25fc-354">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="d25fc-355">L'app di esempio accede alle opzioni denominate con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-355">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="d25fc-356">Eseguendo l'app di esempio, vengono restituite le opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="d25fc-356">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="d25fc-357">I valori `named_options_1` vengono specificati dalla configurazione, caricata dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-357">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="d25fc-358">I valori `named_options_2` sono specificati da:</span><span class="sxs-lookup"><span data-stu-id="d25fc-358">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="d25fc-359">Delegato `named_options_2` in `ConfigureServices` per `Option1`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-359">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="d25fc-360">Valore predefinito per `Option2` specificato dalla classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-360">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="d25fc-361">Configurare tutte le opzioni con il metodo ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="d25fc-361">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="d25fc-362">Configurare tutte le istanze delle opzioni con il metodo <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-362">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="d25fc-363">Il codice seguente configura `Option1` per tutte le istanze di configurazione con un valore comune.</span><span class="sxs-lookup"><span data-stu-id="d25fc-363">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="d25fc-364">Aggiungere manualmente il codice seguente al metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-364">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="d25fc-365">L'esecuzione dell'app di esempio dopo l'aggiunta del codice produce il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="d25fc-365">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="d25fc-366">Tutte le opzioni sono istanze denominate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-366">All options are named instances.</span></span> <span data-ttu-id="d25fc-367">Le istanze di <xref:Microsoft.Extensions.Options.IConfigureOptions%601> esistenti sono trattate come se fossero indirizzate all'istanza di `Options.DefaultName`, ovvero `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-367">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="d25fc-368"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementa anche <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-368"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="d25fc-369">L'implementazione predefinita di <xref:Microsoft.Extensions.Options.IOptionsFactory%601> include la logica per usarle in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="d25fc-369">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="d25fc-370">L'opzione denominata `null` viene usata per avere come destinazione tutte le istanze denominate anziché un'istanza specifica denominata (questa convenzione è usata da <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> e <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>).</span><span class="sxs-lookup"><span data-stu-id="d25fc-370">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="d25fc-371">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="d25fc-371">OptionsBuilder API</span></span>

<span data-ttu-id="d25fc-372"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> viene usata per configurare le istanze di `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-372"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="d25fc-373">`OptionsBuilder` semplifica la creazione di opzioni denominate perché è costituita da un singolo parametro nella chiamata iniziale ad `AddOptions<TOptions>(string optionsName)` invece di essere visualizzata in tutte le chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="d25fc-373">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="d25fc-374">La convalida delle opzioni e gli overload `ConfigureOptions` che accettano le dipendenze dei servizi sono disponibili solo tramite `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-374">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="d25fc-375">Usare i servizi di inserimento delle dipendenze per configurare le opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-375">Use DI services to configure options</span></span>

<span data-ttu-id="d25fc-376">È possibile accedere ad altri servizi dall'inserimento delle dipendenze durante la configurazione delle opzioni in due modi:</span><span class="sxs-lookup"><span data-stu-id="d25fc-376">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="d25fc-377">Passare un delegato di configurazione a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) in [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="d25fc-377">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="d25fc-378">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) fornisce overload di [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) che consentono di usare fino a cinque servizi per configurare le opzioni:</span><span class="sxs-lookup"><span data-stu-id="d25fc-378">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="d25fc-379">Creare un tipo personalizzato che implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> e registrare il tipo come servizio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-379">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="d25fc-380">È consigliabile passare un delegato di configurazione a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) perché la creazione di un servizio è più complessa.</span><span class="sxs-lookup"><span data-stu-id="d25fc-380">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="d25fc-381">La creazione di un tipo personalizzato equivale alle operazioni che il framework esegue automaticamente quando si usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="d25fc-381">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="d25fc-382">La chiamata a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra un'interfaccia <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> generica temporanea, che ha un costruttore che accetta i tipi di servizi generici specificati.</span><span class="sxs-lookup"><span data-stu-id="d25fc-382">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="d25fc-383">Convalida delle opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-383">Options validation</span></span>

<span data-ttu-id="d25fc-384">La convalida le opzioni consente di convalidare le opzioni quando vengono configurate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-384">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="d25fc-385">Chiamare `Validate` con un metodo di convalida che restituisce `true` se le opzioni sono valide e `false` se non sono valide:</span><span class="sxs-lookup"><span data-stu-id="d25fc-385">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="d25fc-386">L'esempio precedente imposta l'istanza di opzioni denominata su `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-386">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="d25fc-387">L'istanza di opzioni predefinita è `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-387">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="d25fc-388">La convalida viene eseguita quando viene creata l'istanza di opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-388">Validation runs when the options instance is created.</span></span> <span data-ttu-id="d25fc-389">Si garantisce che un'istanza di opzioni superi la convalida la prima volta che si accede.</span><span class="sxs-lookup"><span data-stu-id="d25fc-389">An options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d25fc-390">La convalida delle opzioni non protegge le modifiche delle opzioni dopo la creazione dell'istanza di Options.</span><span class="sxs-lookup"><span data-stu-id="d25fc-390">Options validation doesn't guard against options modifications after the options instance is created.</span></span> <span data-ttu-id="d25fc-391">Le opzioni `IOptionsSnapshot`, ad esempio, vengono create e convalidate una volta per ogni richiesta quando si accede per la prima volta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-391">For example, `IOptionsSnapshot` options are created and validated once per request when the options are first accessed.</span></span> <span data-ttu-id="d25fc-392">Le opzioni di `IOptionsSnapshot` non vengono convalidate di nuovo nei tentativi di accesso successivi *per la stessa richiesta*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-392">The `IOptionsSnapshot` options aren't validated again on subsequent access attempts *for the same request*.</span></span>

<span data-ttu-id="d25fc-393">Il metodo `Validate` accetta `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-393">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="d25fc-394">Per personalizzare completamente la convalida, implementare `IValidateOptions<TOptions>`, che consente:</span><span class="sxs-lookup"><span data-stu-id="d25fc-394">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="d25fc-395">La convalida di più tipi di opzioni: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="d25fc-395">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="d25fc-396">La convalida che dipende da un altro tipo di opzione: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="d25fc-396">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="d25fc-397">`IValidateOptions` convalida:</span><span class="sxs-lookup"><span data-stu-id="d25fc-397">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="d25fc-398">Un'istanza di opzioni denominata specifica.</span><span class="sxs-lookup"><span data-stu-id="d25fc-398">A specific named options instance.</span></span>
* <span data-ttu-id="d25fc-399">Tutte le opzioni quando `name` è `null`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-399">All options when `name` is `null`.</span></span>

<span data-ttu-id="d25fc-400">Restituire `ValidateOptionsResult` dall'implementazione dell'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="d25fc-400">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="d25fc-401">La convalida basata sull'annotazione dei dati è disponibile dal pacchetto [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) chiamando il metodo <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> su `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-401">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="d25fc-402">`Microsoft.Extensions.Options.DataAnnotations` è incluso nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d25fc-402">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="d25fc-403">La convalida eager (fallimento e risposta immediata agli errori all'avvio) è pianificata per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="d25fc-403">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="d25fc-404">Post-configurazione delle opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-404">Options post-configuration</span></span>

<span data-ttu-id="d25fc-405">Impostare la post-configurazione con <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-405">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="d25fc-406">La post-configurazione viene eseguita dopo il completamento della configurazione di tutte le <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="d25fc-406">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="d25fc-407"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> è disponibile per la post-configurazione delle opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="d25fc-407"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="d25fc-408">Usare <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> per la post-configurazione di tutte le istanze di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d25fc-408">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="d25fc-409">Accesso alle opzioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="d25fc-409">Accessing options during startup</span></span>

<span data-ttu-id="d25fc-410"><xref:Microsoft.Extensions.Options.IOptions%601> e <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> possono essere usate in `Startup.Configure`, perché i servizi vengono compilati prima dell'esecuzione del metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-410"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="d25fc-411">Non usare <xref:Microsoft.Extensions.Options.IOptions%601> oppure <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-411">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d25fc-412">Le opzioni potrebbero avere uno stato incoerente a causa dell'ordinamento delle registrazioni dei servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-412">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="d25fc-413">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-413">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="d25fc-414">Quando le [impostazioni di configurazione](xref:fundamentals/configuration/index) vengono isolate in base allo scenario in classi separate, l'app aderisce a due importanti principi di progettazione del software:</span><span class="sxs-lookup"><span data-stu-id="d25fc-414">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="d25fc-415">Il [principio di separazione dell'interfaccia (ISP) o l'Incapsulamento](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; scenari (classi) che dipendono dalle impostazioni di configurazione dipendono solo dalle impostazioni di configurazione che usano.</span><span class="sxs-lookup"><span data-stu-id="d25fc-415">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="d25fc-416">La [separazione delle problematiche](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; le impostazioni per parti diverse dell'app non sono dipendenti o collegati tra loro.</span><span class="sxs-lookup"><span data-stu-id="d25fc-416">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="d25fc-417">Le opzioni offrono anche un meccanismo per convalidare i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-417">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="d25fc-418">Per altre informazioni, vedere la sezione [Opzioni di convalida](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="d25fc-418">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="d25fc-419">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d25fc-419">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d25fc-420">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="d25fc-420">Prerequisites</span></span>

<span data-ttu-id="d25fc-421">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="d25fc-421">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="d25fc-422">Interfacce per le opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-422">Options interfaces</span></span>

<span data-ttu-id="d25fc-423"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> consente di recuperare le opzioni e gestire le notifiche di opzioni per le istanze `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-423"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="d25fc-424"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supporta gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="d25fc-424"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="d25fc-425">Notifiche di modifica</span><span class="sxs-lookup"><span data-stu-id="d25fc-425">Change notifications</span></span>
* [<span data-ttu-id="d25fc-426">Opzioni denominate</span><span class="sxs-lookup"><span data-stu-id="d25fc-426">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="d25fc-427">Configurazione ricaricabile</span><span class="sxs-lookup"><span data-stu-id="d25fc-427">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="d25fc-428">Invalidamento selettivo di opzioni (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="d25fc-428">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="d25fc-429">Gli scenari di [post-configurazione](#options-post-configuration) consentono di impostare o modificare le opzioni dopo la configurazione di tutte le <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-429">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="d25fc-430"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> è responsabile della creazione di nuove istanze di opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-430"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="d25fc-431">Include un singolo metodo <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-431">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="d25fc-432">L'implementazione predefinita accetta tutte le interfacce <xref:Microsoft.Extensions.Options.IConfigureOptions%601> e <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> registrate ed esegue tutte le configurazioni, seguite dalla post-configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-432">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="d25fc-433">Fa distinzione tra <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> e <xref:Microsoft.Extensions.Options.IConfigureOptions%601> e chiama solo l'interfaccia appropriata.</span><span class="sxs-lookup"><span data-stu-id="d25fc-433">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="d25fc-434"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> viene usata da <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> per memorizzare nella cache le istanze di `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-434"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="d25fc-435"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalida le istanze delle opzioni nel monitoraggio in modo che il valore venga ricalcolato (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="d25fc-435">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="d25fc-436">I valori possono essere introdotti manualmente con <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-436">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="d25fc-437">Il metodo <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> viene usato quando tutte le istanze denominate devono essere ricreate su richiesta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-437">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="d25fc-438"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> è utile negli scenari in cui le opzioni devono essere ricalcolate a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-438"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="d25fc-439">Per altre informazioni, vedere la sezione [Ricaricare i dati di configurazione con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="d25fc-439">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="d25fc-440"><xref:Microsoft.Extensions.Options.IOptions%601> può essere usata a supporto delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-440"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="d25fc-441">Tuttavia <xref:Microsoft.Extensions.Options.IOptions%601> non supporta gli scenari precedenti di <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-441">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="d25fc-442">È possibile continuare a usare <xref:Microsoft.Extensions.Options.IOptions%601> in framework e librerie esistenti che già usano l'interfaccia <xref:Microsoft.Extensions.Options.IOptions%601> e non richiedono gli scenari forniti da <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-442">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="d25fc-443">Configurazione delle opzioni generali</span><span class="sxs-lookup"><span data-stu-id="d25fc-443">General options configuration</span></span>

<span data-ttu-id="d25fc-444">La configurazione delle opzioni generali è illustrata nell'esempio 1 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-444">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="d25fc-445">Una classe di opzioni deve essere non astratta con un costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="d25fc-445">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="d25fc-446">La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-446">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="d25fc-447">Sebbene l'impostazione dei valori predefiniti sia facoltativa, il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-447">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="d25fc-448">`Option2` ha un valore impostato tramite l'inizializzazione diretta della proprietà (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-448">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="d25fc-449">La classe `MyOptions` viene aggiunta al contenitore di servizi con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> e associata alla configurazione:</span><span class="sxs-lookup"><span data-stu-id="d25fc-449">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="d25fc-450">Il modello di pagina seguente usa l'[inserimento delle dipendenze del costruttore](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> per accedere alle impostazioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-450">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="d25fc-451">Il file *appsettings.json* dell'esempio specifica i valori di `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-451">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="d25fc-452">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="d25fc-452">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="d25fc-453">Se si usa un <xref:System.Configuration.ConfigurationBuilder> personalizzato per caricare la configurazione delle opzioni da un file di impostazioni, verificare che il percorso base sia impostato correttamente:</span><span class="sxs-lookup"><span data-stu-id="d25fc-453">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="d25fc-454">L'impostazione esplicita del percorso base non è necessaria se si carica la configurazione delle opzioni dal file di impostazioni tramite <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-454">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="d25fc-455">Configurare opzioni semplici con un delegato</span><span class="sxs-lookup"><span data-stu-id="d25fc-455">Configure simple options with a delegate</span></span>

<span data-ttu-id="d25fc-456">La configurazione di semplici opzioni con un delegato è illustrata nell'esempio 2 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-456">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="d25fc-457">Usare un delegato per impostare i valori delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="d25fc-457">Use a delegate to set options values.</span></span> <span data-ttu-id="d25fc-458">L'app di esempio usa la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-458">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="d25fc-459">Nel codice seguente viene aggiunto un secondo servizio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-459">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="d25fc-460">Viene usato un delegato per configurare l'associazione a `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-460">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="d25fc-461">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d25fc-461">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="d25fc-462">È possibile aggiungere più provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-462">You can add multiple configuration providers.</span></span> <span data-ttu-id="d25fc-463">I provider di configurazione sono disponibili dai pacchetti NuGet e vengono applicati nell'ordine in cui sono registrati.</span><span class="sxs-lookup"><span data-stu-id="d25fc-463">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="d25fc-464">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-464">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="d25fc-465">Ogni chiamata a <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> aggiunge un servizio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-465">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="d25fc-466">Nell'esempio precedente i valori di `Option1` e `Option2` sono entrambi specificati in *appsettings.json*, mentre i valori di `Option1` e `Option2` sono sottoposti a override dal delegato configurato.</span><span class="sxs-lookup"><span data-stu-id="d25fc-466">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="d25fc-467">Quando sono abilitati più servizi di configurazione, l'origine di configurazione più recente specificata *ha priorità* e imposta il valore di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-467">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="d25fc-468">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="d25fc-468">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="d25fc-469">Configurazione delle opzioni secondarie</span><span class="sxs-lookup"><span data-stu-id="d25fc-469">Suboptions configuration</span></span>

<span data-ttu-id="d25fc-470">La configurazione delle sottoopzioni è illustrata nell'esempio 3 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-470">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="d25fc-471">Le app devono creare classi di opzioni che riguardano gruppi di scenari (classi) specifici nell'app.</span><span class="sxs-lookup"><span data-stu-id="d25fc-471">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="d25fc-472">Le parti dell'app che richiedono valori di configurazione devono avere accesso solo ai valori di configurazione necessari.</span><span class="sxs-lookup"><span data-stu-id="d25fc-472">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="d25fc-473">Durante l'associazione delle opzioni alla configurazione, ogni proprietà del tipo di opzioni viene associata a una chiave di configurazione con formato `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-473">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="d25fc-474">Ad esempio, la proprietà `MyOptions.Option1` viene associata alla chiave `Option1`, che viene letta dalla proprietà `option1` in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-474">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="d25fc-475">Nel codice seguente viene aggiunto un terzo servizio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-475">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="d25fc-476">`MySubOptions` viene associato alla sezione `subsection` del file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d25fc-476">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="d25fc-477">Il metodo `GetSection` richiede lo spazio dei nomi <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-477">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="d25fc-478">Il file *appsettings.json* dell'esempio definisce un membro `subsection` con chiavi per `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-478">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="d25fc-479">La classe `MySubOptions` definisce le proprietà `SubOption1` e `SubOption2` per contenere i valori delle opzioni (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-479">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="d25fc-480">Il metodo `OnGet` del modello di pagina restituisce una stringa con i valori delle opzioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-480">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="d25fc-481">Quando viene eseguita l'app, il metodo `OnGet` restituisce una stringa che mostra i valori delle classi delle opzioni secondarie:</span><span class="sxs-lookup"><span data-stu-id="d25fc-481">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="d25fc-482">Opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione</span><span class="sxs-lookup"><span data-stu-id="d25fc-482">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="d25fc-483">Le opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione sono illustrate come esempio 4 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-483">Options provided by a view model or with direct view injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="d25fc-484">Le opzioni possono essere rese disponibili in un modello di visualizzazione oppure inserendo <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> direttamente in una visualizzazione (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-484">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="d25fc-485">L'app di esempio mostra come inserire `IOptionsMonitor<MyOptions>` con una direttiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-485">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="d25fc-486">Quando l'app viene eseguita, i valori delle opzioni sono visibili nella pagina di cui è stato eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="d25fc-486">When the app is run, the options values are shown in the rendered page:</span></span>

![I valori delle opzioni Option1: value1_from_json e Option2: -1 vengono caricati dal modello e tramite inserimento nella visualizzazione.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="d25fc-488">Ricaricare i dati di configurazione con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="d25fc-488">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="d25fc-489">Il ricaricamento dei dati di configurazione con <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> viene illustrato nell'esempio 5 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-489">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="d25fc-490"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supporta il ricaricamento delle opzioni con un overhead di elaborazione minimo.</span><span class="sxs-lookup"><span data-stu-id="d25fc-490"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="d25fc-491">Le opzioni vengono calcolate una volta per richiesta quando viene eseguito l'accesso e la memorizzazione nella cache per la durata della richiesta.</span><span class="sxs-lookup"><span data-stu-id="d25fc-491">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="d25fc-492">L'esempio seguente illustra la creazione di un nuovo <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> dopo le modifiche ad *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="d25fc-492">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="d25fc-493">Più richieste al server restituiscono valori costanti forniti dal file *appsettings.json* fino a quando il file non viene modificato e la configurazione non viene ricaricata.</span><span class="sxs-lookup"><span data-stu-id="d25fc-493">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="d25fc-494">L'immagine seguente illustra i valori iniziali `option1` e `option2` caricati dal file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d25fc-494">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="d25fc-495">Modificare i valori nel file *appsettings.json* in `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-495">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="d25fc-496">Salvare il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-496">Save the *appsettings.json* file.</span></span> <span data-ttu-id="d25fc-497">Aggiornare il browser per visualizzare i valori delle opzioni aggiornati:</span><span class="sxs-lookup"><span data-stu-id="d25fc-497">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="d25fc-498">Supporto delle opzioni denominate con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="d25fc-498">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="d25fc-499">Il supporto delle opzioni denominate con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> viene illustrato come esempio 6 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-499">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="d25fc-500">Il supporto delle opzioni denominate consente all'app di distinguere tra le configurazioni delle opzioni denominate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-500">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="d25fc-501">Nell'app di esempio, le opzioni denominate sono dichiarate con [OptionsServiceCollectionExtensions. Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), che chiama il [ConfigureNamedOptions\<>. Configurare](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) il metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="d25fc-501">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="d25fc-502">Le opzioni denominate fanno distinzione maiuscole/minuscole</span><span class="sxs-lookup"><span data-stu-id="d25fc-502">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="d25fc-503">L'app di esempio accede alle opzioni denominate con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d25fc-503">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="d25fc-504">Eseguendo l'app di esempio, vengono restituite le opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="d25fc-504">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="d25fc-505">I valori `named_options_1` vengono specificati dalla configurazione, caricata dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d25fc-505">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="d25fc-506">I valori `named_options_2` sono specificati da:</span><span class="sxs-lookup"><span data-stu-id="d25fc-506">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="d25fc-507">Delegato `named_options_2` in `ConfigureServices` per `Option1`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-507">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="d25fc-508">Valore predefinito per `Option2` specificato dalla classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-508">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="d25fc-509">Configurare tutte le opzioni con il metodo ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="d25fc-509">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="d25fc-510">Configurare tutte le istanze delle opzioni con il metodo <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-510">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="d25fc-511">Il codice seguente configura `Option1` per tutte le istanze di configurazione con un valore comune.</span><span class="sxs-lookup"><span data-stu-id="d25fc-511">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="d25fc-512">Aggiungere manualmente il codice seguente al metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d25fc-512">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="d25fc-513">L'esecuzione dell'app di esempio dopo l'aggiunta del codice produce il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="d25fc-513">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="d25fc-514">Tutte le opzioni sono istanze denominate.</span><span class="sxs-lookup"><span data-stu-id="d25fc-514">All options are named instances.</span></span> <span data-ttu-id="d25fc-515">Le istanze di <xref:Microsoft.Extensions.Options.IConfigureOptions%601> esistenti sono trattate come se fossero indirizzate all'istanza di `Options.DefaultName`, ovvero `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-515">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="d25fc-516"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementa anche <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-516"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="d25fc-517">L'implementazione predefinita di <xref:Microsoft.Extensions.Options.IOptionsFactory%601> include la logica per usarle in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="d25fc-517">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="d25fc-518">L'opzione denominata `null` viene usata per avere come destinazione tutte le istanze denominate anziché un'istanza specifica denominata (questa convenzione è usata da <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> e <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>).</span><span class="sxs-lookup"><span data-stu-id="d25fc-518">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="d25fc-519">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="d25fc-519">OptionsBuilder API</span></span>

<span data-ttu-id="d25fc-520"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> viene usata per configurare le istanze di `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-520"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="d25fc-521">`OptionsBuilder` semplifica la creazione di opzioni denominate perché è costituita da un singolo parametro nella chiamata iniziale ad `AddOptions<TOptions>(string optionsName)` invece di essere visualizzata in tutte le chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="d25fc-521">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="d25fc-522">La convalida delle opzioni e gli overload `ConfigureOptions` che accettano le dipendenze dei servizi sono disponibili solo tramite `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-522">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="d25fc-523">Usare i servizi di inserimento delle dipendenze per configurare le opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-523">Use DI services to configure options</span></span>

<span data-ttu-id="d25fc-524">È possibile accedere ad altri servizi dall'inserimento delle dipendenze durante la configurazione delle opzioni in due modi:</span><span class="sxs-lookup"><span data-stu-id="d25fc-524">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="d25fc-525">Passare un delegato di configurazione a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) in [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="d25fc-525">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="d25fc-526">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) fornisce overload di [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) che consentono di usare fino a cinque servizi per configurare le opzioni:</span><span class="sxs-lookup"><span data-stu-id="d25fc-526">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="d25fc-527">Creare un tipo personalizzato che implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> e registrare il tipo come servizio.</span><span class="sxs-lookup"><span data-stu-id="d25fc-527">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="d25fc-528">È consigliabile passare un delegato di configurazione a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) perché la creazione di un servizio è più complessa.</span><span class="sxs-lookup"><span data-stu-id="d25fc-528">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="d25fc-529">La creazione di un tipo personalizzato equivale alle operazioni che il framework esegue automaticamente quando si usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="d25fc-529">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="d25fc-530">La chiamata a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra un'interfaccia <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> generica temporanea, che ha un costruttore che accetta i tipi di servizi generici specificati.</span><span class="sxs-lookup"><span data-stu-id="d25fc-530">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="d25fc-531">Post-configurazione delle opzioni</span><span class="sxs-lookup"><span data-stu-id="d25fc-531">Options post-configuration</span></span>

<span data-ttu-id="d25fc-532">Impostare la post-configurazione con <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="d25fc-532">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="d25fc-533">La post-configurazione viene eseguita dopo il completamento della configurazione di tutte le <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="d25fc-533">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="d25fc-534"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> è disponibile per la post-configurazione delle opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="d25fc-534"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="d25fc-535">Usare <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> per la post-configurazione di tutte le istanze di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d25fc-535">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="d25fc-536">Accesso alle opzioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="d25fc-536">Accessing options during startup</span></span>

<span data-ttu-id="d25fc-537"><xref:Microsoft.Extensions.Options.IOptions%601> e <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> possono essere usate in `Startup.Configure`, perché i servizi vengono compilati prima dell'esecuzione del metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-537"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="d25fc-538">Non usare <xref:Microsoft.Extensions.Options.IOptions%601> oppure <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d25fc-538">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d25fc-539">Le opzioni potrebbero avere uno stato incoerente a causa dell'ordinamento delle registrazioni dei servizi.</span><span class="sxs-lookup"><span data-stu-id="d25fc-539">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d25fc-540">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d25fc-540">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
