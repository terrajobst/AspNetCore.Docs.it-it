---
title: Modello di opzioni in ASP.NET Core
author: guardrex
description: Informazioni su come usare il modello di opzioni per rappresentare gruppi di opzioni correlate nelle app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2018
uid: fundamentals/configuration/options
ms.openlocfilehash: 0e3784de18be16e3217a015dd94f1b43b6621c1c
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577890"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="355eb-103">Modello di opzioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="355eb-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="355eb-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="355eb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="355eb-105">Per la versione 1.1 di questo argomento, scaricare [Options pattern in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf) (Modello di opzioni in ASP.NET Core, versione 1.1, PDF).</span><span class="sxs-lookup"><span data-stu-id="355eb-105">For the 1.1 version of this topic, download [Options pattern in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="355eb-106">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="355eb-106">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="355eb-107">Quando le [impostazioni di configurazione](xref:fundamentals/configuration/index) vengono isolate in base allo scenario in classi separate, l'app aderisce a due importanti principi di progettazione del software:</span><span class="sxs-lookup"><span data-stu-id="355eb-107">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="355eb-108">[Principio di segregazione delle interfacce (Interface Segregation Principle, ISP) o incapsulamento](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Gli scenari (classi) che dipendono dalle impostazioni di configurazione dipendono solo dalle impostazioni di configurazione che usano.</span><span class="sxs-lookup"><span data-stu-id="355eb-108">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="355eb-109">[Separazione delle competenze (Separation of Concerns, SoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Le impostazioni di parti diverse dell'app non sono dipendenti o accoppiate l'una con l'altra.</span><span class="sxs-lookup"><span data-stu-id="355eb-109">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="355eb-110">Le opzioni offrono anche un meccanismo per convalidare i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="355eb-110">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="355eb-111">Per altre informazioni, vedere la sezione [Opzioni di convalida](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="355eb-111">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="355eb-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="355eb-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="355eb-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="355eb-113">Prerequisites</span></span>

<span data-ttu-id="355eb-114">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="355eb-114">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="355eb-115">Interfacce per le opzioni</span><span class="sxs-lookup"><span data-stu-id="355eb-115">Options interfaces</span></span>

<span data-ttu-id="355eb-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> consente di recuperare le opzioni e gestire le notifiche di opzioni per le istanze `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="355eb-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="355eb-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> supporta gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="355eb-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> supports the following scenarios:</span></span>

* <span data-ttu-id="355eb-118">Notifiche di modifica</span><span class="sxs-lookup"><span data-stu-id="355eb-118">Change notifications</span></span>
* [<span data-ttu-id="355eb-119">Opzioni denominate</span><span class="sxs-lookup"><span data-stu-id="355eb-119">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="355eb-120">Configurazione ricaricabile</span><span class="sxs-lookup"><span data-stu-id="355eb-120">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="355eb-121">Invalidamento selettivo di opzioni (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)</span><span class="sxs-lookup"><span data-stu-id="355eb-121">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)</span></span>

<span data-ttu-id="355eb-122">Gli scenari di [post-configurazione](#options-post-configuration) consentono di impostare o modificare le opzioni dopo la configurazione di tutte le <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="355eb-122">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions`1> configuration occurs.</span></span>

<span data-ttu-id="355eb-123"><xref:Microsoft.Extensions.Options.IOptionsFactory`1> è responsabile della creazione di nuove istanze di opzioni.</span><span class="sxs-lookup"><span data-stu-id="355eb-123"><xref:Microsoft.Extensions.Options.IOptionsFactory`1> is responsible for creating new options instances.</span></span> <span data-ttu-id="355eb-124">Include un singolo metodo <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="355eb-124">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="355eb-125">L'implementazione predefinita accetta tutte le interfacce <xref:Microsoft.Extensions.Options.IConfigureOptions`1> e <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1> registrate ed esegue tutte le configurazioni, seguite dalla post-configurazione.</span><span class="sxs-lookup"><span data-stu-id="355eb-125">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions`1> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="355eb-126">Fa distinzione tra <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> e <xref:Microsoft.Extensions.Options.IConfigureOptions`1> e chiama solo l'interfaccia appropriata.</span><span class="sxs-lookup"><span data-stu-id="355eb-126">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> and <xref:Microsoft.Extensions.Options.IConfigureOptions`1> and only calls the appropriate interface.</span></span>

<span data-ttu-id="355eb-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> viene usata da <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> per memorizzare nella cache le istanze di `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="355eb-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> to cache `TOptions` instances.</span></span> <span data-ttu-id="355eb-128"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> invalida le istanze delle opzioni nel monitoraggio in modo che il valore venga ricalcolato (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="355eb-128">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="355eb-129">I valori possono essere introdotti manualmente con <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="355eb-129">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="355eb-130">Il metodo <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> viene usato quando tutte le istanze denominate devono essere ricreate su richiesta.</span><span class="sxs-lookup"><span data-stu-id="355eb-130">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="355eb-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> è utile negli scenari in cui le opzioni devono essere ricalcolate a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="355eb-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="355eb-132">Per altre informazioni, vedere la sezione [Ricaricare i dati di configurazione con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="355eb-132">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="355eb-133"><xref:Microsoft.Extensions.Options.IOptions`1> può essere usata a supporto delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="355eb-133"><xref:Microsoft.Extensions.Options.IOptions`1> can be used to support options.</span></span> <span data-ttu-id="355eb-134">Tuttavia <xref:Microsoft.Extensions.Options.IOptions`1> non supporta gli scenari precedenti di <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span><span class="sxs-lookup"><span data-stu-id="355eb-134">However, <xref:Microsoft.Extensions.Options.IOptions`1> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span></span> <span data-ttu-id="355eb-135">È possibile continuare a usare <xref:Microsoft.Extensions.Options.IOptions`1> in framework e librerie esistenti che già usano l'interfaccia <xref:Microsoft.Extensions.Options.IOptions`1> e non richiedono gli scenari forniti da <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span><span class="sxs-lookup"><span data-stu-id="355eb-135">You may continue to use <xref:Microsoft.Extensions.Options.IOptions`1> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions`1> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="355eb-136">Configurazione delle opzioni generali</span><span class="sxs-lookup"><span data-stu-id="355eb-136">General options configuration</span></span>

<span data-ttu-id="355eb-137">La configurazione delle opzioni generali è illustrata nell'Esempio &num;1 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="355eb-137">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="355eb-138">Una classe di opzioni deve essere non astratta con un costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="355eb-138">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="355eb-139">La classe seguente, `MyOptions`, ha due proprietà, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="355eb-139">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="355eb-140">Sebbene l'impostazione dei valori predefiniti sia facoltativa, il costruttore della classe nell'esempio seguente imposta il valore predefinito di `Option1`.</span><span class="sxs-lookup"><span data-stu-id="355eb-140">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="355eb-141">`Option2` ha un valore impostato tramite l'inizializzazione diretta della proprietà (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="355eb-141">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="355eb-142">La classe `MyOptions` viene aggiunta al contenitore di servizi con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> e associata alla configurazione:</span><span class="sxs-lookup"><span data-stu-id="355eb-142">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="355eb-143">Il modello di pagina seguente usa l'[inserimento delle dipendenze del costruttore](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> per accedere alle impostazioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="355eb-143">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="355eb-144">Il file *appsettings.json* dell'esempio specifica i valori di `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="355eb-144">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="355eb-145">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="355eb-145">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="355eb-146">Se si usa un <xref:System.Configuration.ConfigurationBuilder> personalizzato per caricare la configurazione delle opzioni da un file di impostazioni, verificare che il percorso base sia impostato correttamente:</span><span class="sxs-lookup"><span data-stu-id="355eb-146">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="355eb-147">L'impostazione esplicita del percorso base non è necessaria se si carica la configurazione delle opzioni dal file di impostazioni tramite <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="355eb-147">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="355eb-148">Configurare opzioni semplici con un delegato</span><span class="sxs-lookup"><span data-stu-id="355eb-148">Configure simple options with a delegate</span></span>

<span data-ttu-id="355eb-149">La configurazione di opzioni semplici con un delegato è illustrata nell'Esempio &num;2 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="355eb-149">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="355eb-150">Usare un delegato per impostare i valori delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="355eb-150">Use a delegate to set options values.</span></span> <span data-ttu-id="355eb-151">L'app di esempio usa la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="355eb-151">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="355eb-152">Nel codice seguente viene aggiunto un secondo servizio <xref:Microsoft.Extensions.Options.IConfigureOptions`1> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="355eb-152">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service is added to the service container.</span></span> <span data-ttu-id="355eb-153">Viene usato un delegato per configurare l'associazione a `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="355eb-153">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="355eb-154">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="355eb-154">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="355eb-155">È possibile aggiungere più provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="355eb-155">You can add multiple configuration providers.</span></span> <span data-ttu-id="355eb-156">I provider di configurazione sono disponibili dai pacchetti NuGet e vengono applicati nell'ordine in cui sono registrati.</span><span class="sxs-lookup"><span data-stu-id="355eb-156">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="355eb-157">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="355eb-157">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="355eb-158">Ogni chiamata a <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> aggiunge un servizio <xref:Microsoft.Extensions.Options.IConfigureOptions`1> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="355eb-158">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service to the service container.</span></span> <span data-ttu-id="355eb-159">Nell'esempio precedente i valori di `Option1` e `Option2` sono entrambi specificati in *appsettings.json*, mentre i valori di `Option1` e `Option2` sono sottoposti a override dal delegato configurato.</span><span class="sxs-lookup"><span data-stu-id="355eb-159">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="355eb-160">Quando sono abilitati più servizi di configurazione, l'origine di configurazione più recente specificata *ha priorità* e imposta il valore di configurazione.</span><span class="sxs-lookup"><span data-stu-id="355eb-160">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="355eb-161">Quando viene eseguita l'app, il metodo `OnGet` del modello di pagina restituisce una stringa che mostra i valori delle classi delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="355eb-161">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="355eb-162">Configurazione delle opzioni secondarie</span><span class="sxs-lookup"><span data-stu-id="355eb-162">Suboptions configuration</span></span>

<span data-ttu-id="355eb-163">La configurazione delle opzioni secondarie è illustrata nell'Esempio &num;3 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="355eb-163">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="355eb-164">Le app devono creare classi di opzioni che riguardano gruppi di scenari (classi) specifici nell'app.</span><span class="sxs-lookup"><span data-stu-id="355eb-164">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="355eb-165">Le parti dell'app che richiedono valori di configurazione devono avere accesso solo ai valori di configurazione necessari.</span><span class="sxs-lookup"><span data-stu-id="355eb-165">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="355eb-166">Durante l'associazione delle opzioni alla configurazione, ogni proprietà del tipo di opzioni viene associata a una chiave di configurazione con formato `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="355eb-166">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="355eb-167">Ad esempio, la proprietà `MyOptions.Option1` viene associata alla chiave `Option1`, che viene letta dalla proprietà `option1` in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="355eb-167">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="355eb-168">Nel codice seguente viene aggiunto un terzo servizio <xref:Microsoft.Extensions.Options.IConfigureOptions`1> al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="355eb-168">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service is added to the service container.</span></span> <span data-ttu-id="355eb-169">`MySubOptions` viene associato alla sezione `subsection` del file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="355eb-169">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="355eb-170">Il metodo di estensione `GetSection` richiede il pacchetto NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="355eb-170">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="355eb-171">Se l'app usa il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive), il pacchetto è incluso automaticamente.</span><span class="sxs-lookup"><span data-stu-id="355eb-171">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="355eb-172">Il file *appsettings.json* dell'esempio definisce un membro `subsection` con chiavi per `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="355eb-172">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="355eb-173">La classe `MySubOptions` definisce le proprietà `SubOption1` e `SubOption2` per contenere i valori delle opzioni (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="355eb-173">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="355eb-174">Il metodo `OnGet` del modello di pagina restituisce una stringa con i valori delle opzioni (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="355eb-174">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="355eb-175">Quando viene eseguita l'app, il metodo `OnGet` restituisce una stringa che mostra i valori delle classi delle opzioni secondarie:</span><span class="sxs-lookup"><span data-stu-id="355eb-175">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="355eb-176">Opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione</span><span class="sxs-lookup"><span data-stu-id="355eb-176">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="355eb-177">Le opzioni fornite da un modello di visualizzazione o con l'inserimento diretto della visualizzazione sono illustrate nell'Esempio &num;4 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="355eb-177">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="355eb-178">Le opzioni possono essere rese disponibili in un modello di visualizzazione oppure inserendo <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> direttamente in una visualizzazione (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="355eb-178">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="355eb-179">L'app di esempio mostra come inserire `IOptionsMonitor<MyOptions>` con una direttiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="355eb-179">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="355eb-180">Quando l'app viene eseguita, i valori delle opzioni sono visibili nella pagina di cui è stato eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="355eb-180">When the app is run, the options values are shown in the rendered page:</span></span>

![I valori delle opzioni Option1: value1_from_json e Option2: -1 vengono caricati dal modello e tramite inserimento nella visualizzazione.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="355eb-182">Ricaricare i dati di configurazione con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="355eb-182">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="355eb-183">Il ricaricamento dei dati di configurazione con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> è illustrato nell'Esempio &num;5 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="355eb-183">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="355eb-184"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> supporta il ricaricamento delle opzioni con un overhead di elaborazione minimo.</span><span class="sxs-lookup"><span data-stu-id="355eb-184"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="355eb-185">Le opzioni vengono calcolate una volta per richiesta quando viene eseguito l'accesso e la memorizzazione nella cache per la durata della richiesta.</span><span class="sxs-lookup"><span data-stu-id="355eb-185">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="355eb-186">L'esempio seguente illustra la creazione di un nuovo <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> dopo le modifiche ad *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="355eb-186">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="355eb-187">Più richieste al server restituiscono valori costanti forniti dal file *appsettings.json* fino a quando il file non viene modificato e la configurazione non viene ricaricata.</span><span class="sxs-lookup"><span data-stu-id="355eb-187">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="355eb-188">L'immagine seguente illustra i valori iniziali `option1` e `option2` caricati dal file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="355eb-188">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="355eb-189">Modificare i valori nel file *appsettings.json* in `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="355eb-189">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="355eb-190">Salvare il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="355eb-190">Save the *appsettings.json* file.</span></span> <span data-ttu-id="355eb-191">Aggiornare il browser per visualizzare i valori delle opzioni aggiornati:</span><span class="sxs-lookup"><span data-stu-id="355eb-191">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="355eb-192">Supporto delle opzioni denominate con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="355eb-192">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="355eb-193">Il supporto delle opzioni denominate con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> è illustrato nell'Esempio &num;6 nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="355eb-193">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="355eb-194">Il supporto delle *opzioni denominate* consente all'app di distinguere le configurazioni delle opzioni denominate.</span><span class="sxs-lookup"><span data-stu-id="355eb-194">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="355eb-195">Nell'app di esempio, le opzioni denominate vengono dichiarate con <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*>.</span><span class="sxs-lookup"><span data-stu-id="355eb-195">In the sample app, named options are declared with <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*>.</span></span> <span data-ttu-id="355eb-196">`Configure` chiama il metodo <xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*> del metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="355eb-196">`Configure` calls the extension method <xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*> method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="355eb-197">L'app di esempio accede alle opzioni denominate con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="355eb-197">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="355eb-198">Eseguendo l'app di esempio, vengono restituite le opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="355eb-198">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="355eb-199">I valori `named_options_1` vengono specificati dalla configurazione, caricata dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="355eb-199">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="355eb-200">I valori `named_options_2` sono specificati da:</span><span class="sxs-lookup"><span data-stu-id="355eb-200">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="355eb-201">Delegato `named_options_2` in `ConfigureServices` per `Option1`.</span><span class="sxs-lookup"><span data-stu-id="355eb-201">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="355eb-202">Valore predefinito per `Option2` specificato dalla classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="355eb-202">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="355eb-203">Configurare tutte le opzioni con il metodo ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="355eb-203">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="355eb-204">Configurare tutte le istanze delle opzioni con il metodo <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="355eb-204">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="355eb-205">Il codice seguente configura `Option1` per tutte le istanze di configurazione con un valore comune.</span><span class="sxs-lookup"><span data-stu-id="355eb-205">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="355eb-206">Aggiungere manualmente il codice seguente al metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="355eb-206">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="355eb-207">L'esecuzione dell'app di esempio dopo l'aggiunta del codice produce il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="355eb-207">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="355eb-208">Tutte le opzioni sono istanze denominate.</span><span class="sxs-lookup"><span data-stu-id="355eb-208">All options are named instances.</span></span> <span data-ttu-id="355eb-209">Le istanze di <xref:Microsoft.Extensions.Options.IConfigureOptions`1> esistenti sono trattate come se fossero indirizzate all'istanza di `Options.DefaultName`, ovvero `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="355eb-209">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions`1> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="355eb-210"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> implementa anche <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="355eb-210"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span></span> <span data-ttu-id="355eb-211">L'implementazione predefinita di <xref:Microsoft.Extensions.Options.IOptionsFactory`1> include la logica per usarle in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="355eb-211">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory`1> has logic to use each appropriately.</span></span> <span data-ttu-id="355eb-212">L'opzione denominata `null` viene usata per avere come destinazione tutte le istanze denominate anziché un'istanza specifica denominata (questa convenzione è usata da <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> e <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>).</span><span class="sxs-lookup"><span data-stu-id="355eb-212">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="355eb-213">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="355eb-213">OptionsBuilder API</span></span>

<span data-ttu-id="355eb-214"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> viene usata per configurare le istanze di `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="355eb-214"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="355eb-215">`OptionsBuilder` semplifica la creazione di opzioni denominate perché è costituita da un singolo parametro nella chiamata iniziale ad `AddOptions<TOptions>(string optionsName)` invece di essere visualizzata in tutte le chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="355eb-215">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="355eb-216">La convalida delle opzioni e gli overload `ConfigureOptions` che accettano le dipendenze dei servizi sono disponibili solo tramite `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="355eb-216">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="configurelttoptions-tdep1--tdep4gt-method"></a><span data-ttu-id="355eb-217">Metodo Configure&lt;TOptions, TDep1, ... TDep4&gt;</span><span class="sxs-lookup"><span data-stu-id="355eb-217">Configure&lt;TOptions, TDep1, ... TDep4&gt; method</span></span>

<span data-ttu-id="355eb-218">L'uso di servizi dall'inserimento di dipendenze per configurare le opzioni implementando `IConfigure[Named]Options` in modo standard risulta faticoso.</span><span class="sxs-lookup"><span data-stu-id="355eb-218">Using services from DI to configure options by implementing `IConfigure[Named]Options` in a boilerplate manner is verbose.</span></span> <span data-ttu-id="355eb-219">Gli overload per `ConfigureOptions` in `OptionsBuilder<TOptions>` consentono di usare fino a cinque servizi per configurare le opzioni:</span><span class="sxs-lookup"><span data-stu-id="355eb-219">Overloads for `ConfigureOptions` on `OptionsBuilder<TOptions>` allow you to use up to five services to configure options:</span></span>

```csharp
services.AddOptions<MyOptions>("optionalName")
    .Configure<Service1, Service2, Service3, Service4, Service5>(
        (o, s, s2, s3, s4, s5) => 
            o.Property = DoSomethingWith(s, s2, s3, s4, s5));
```

<span data-ttu-id="355eb-220">L'overload registra un'interfaccia <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> generica temporanea, che ha un costruttore che accetta i tipi di servizi generici specificati.</span><span class="sxs-lookup"><span data-stu-id="355eb-220">The overload registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, which has a constructor that accepts the generic service types specified.</span></span> 

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="355eb-221">Convalida delle opzioni</span><span class="sxs-lookup"><span data-stu-id="355eb-221">Options validation</span></span>

<span data-ttu-id="355eb-222">La convalida le opzioni consente di convalidare le opzioni quando vengono configurate.</span><span class="sxs-lookup"><span data-stu-id="355eb-222">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="355eb-223">Chiamare `Validate` con un metodo di convalida che restituisce `true` se le opzioni sono valide e `false` se non sono valide:</span><span class="sxs-lookup"><span data-stu-id="355eb-223">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="355eb-224">L'esempio precedente imposta l'istanza di opzioni denominata su `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="355eb-224">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="355eb-225">L'istanza di opzioni predefinita è `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="355eb-225">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="355eb-226">La convalida viene eseguita quando viene creata l'istanza di opzioni.</span><span class="sxs-lookup"><span data-stu-id="355eb-226">Validation runs when the options instance is created.</span></span> <span data-ttu-id="355eb-227">L'istanza di opzioni supera sicuramente la convalida al primo accesso.</span><span class="sxs-lookup"><span data-stu-id="355eb-227">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="355eb-228">La convalida delle opzioni non protegge da eventuali modifiche dopo la configurazione e la convalida iniziali delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="355eb-228">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="355eb-229">Il metodo `Validate` accetta `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="355eb-229">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="355eb-230">Per personalizzare completamente la convalida, implementare `IValidateOptions<TOptions>`, che consente:</span><span class="sxs-lookup"><span data-stu-id="355eb-230">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="355eb-231">La convalida di più tipi di opzioni: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="355eb-231">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="355eb-232">La convalida che dipende da un altro tipo di opzione: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="355eb-232">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="355eb-233">`IValidateOptions` convalida:</span><span class="sxs-lookup"><span data-stu-id="355eb-233">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="355eb-234">Un'istanza di opzioni denominata specifica.</span><span class="sxs-lookup"><span data-stu-id="355eb-234">A specific named options instance.</span></span>
* <span data-ttu-id="355eb-235">Tutte le opzioni quando `name` è `null`.</span><span class="sxs-lookup"><span data-stu-id="355eb-235">All options when `name` is `null`.</span></span>

<span data-ttu-id="355eb-236">Restituire `ValidateOptionsResult` dall'implementazione dell'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="355eb-236">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="355eb-237">La convalida basata sull'annotazione dei dati è disponibile dal pacchetto [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) chiamando il metodo `ValidateDataAnnotations` su `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="355eb-237">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the `ValidateDataAnnotations` method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="355eb-238">`Microsoft.Extensions.Options.DataAnnotations` è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="355eb-238">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 or later).</span></span>

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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="355eb-239">La convalida eager (fallimento e risposta immediata agli errori all'avvio) è pianificata per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="355eb-239">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

::: moniker-end

## <a name="options-post-configuration"></a><span data-ttu-id="355eb-240">Post-configurazione delle opzioni</span><span class="sxs-lookup"><span data-stu-id="355eb-240">Options post-configuration</span></span>

<span data-ttu-id="355eb-241">Impostare la post-configurazione con <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="355eb-241">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>.</span></span> <span data-ttu-id="355eb-242">La post-configurazione viene eseguita dopo il completamento della configurazione di tutte le <xref:Microsoft.Extensions.Options.IConfigureOptions`1>:</span><span class="sxs-lookup"><span data-stu-id="355eb-242">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions`1> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="355eb-243"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> è disponibile per la post-configurazione delle opzioni denominate:</span><span class="sxs-lookup"><span data-stu-id="355eb-243"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="355eb-244">Usare <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> per la post-configurazione di tutte le istanze di configurazione:</span><span class="sxs-lookup"><span data-stu-id="355eb-244">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="355eb-245">Accesso alle opzioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="355eb-245">Accessing options during startup</span></span>

<span data-ttu-id="355eb-246"><xref:Microsoft.Extensions.Options.IOptions`1> e <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> possono essere usate in `Startup.Configure`, perché i servizi vengono compilati prima dell'esecuzione del metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="355eb-246"><xref:Microsoft.Extensions.Options.IOptions`1> and <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="355eb-247">Non usare <xref:Microsoft.Extensions.Options.IOptions`1> oppure <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="355eb-247">Don't use <xref:Microsoft.Extensions.Options.IOptions`1> or <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="355eb-248">Le opzioni potrebbero avere uno stato incoerente a causa dell'ordinamento delle registrazioni dei servizi.</span><span class="sxs-lookup"><span data-stu-id="355eb-248">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="355eb-249">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="355eb-249">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
