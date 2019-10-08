---
title: Rilevare le modifiche apportate con i token di modifica in ASP.NET Core
author: guardrex
description: Informazioni su come usare i token di modifica per rilevare le modifiche.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/07/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: bb30d7a4c7dc82200821c60a49c314b246562111
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007205"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="3d45e-103">Rilevare le modifiche apportate con i token di modifica in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d45e-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="3d45e-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3d45e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3d45e-105">Un *token di modifica* è un blocco predefinito di uso generico e di basso livello che viene usato per il rilevamento delle modifiche di stato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-105">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="3d45e-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3d45e-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="3d45e-107">Interfaccia IChangeToken</span><span class="sxs-lookup"><span data-stu-id="3d45e-107">IChangeToken interface</span></span>

<span data-ttu-id="3d45e-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propaga le notifiche relative a una modifica apportata.</span><span class="sxs-lookup"><span data-stu-id="3d45e-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="3d45e-109">`IChangeToken` risiede nello spazio dei nomi <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-109">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="3d45e-110">Il pacchetto NuGet [Microsoft. Extensions. Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) viene fornito in modo implicito alle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3d45e-110">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="3d45e-111">`IChangeToken` ha due proprietà:</span><span class="sxs-lookup"><span data-stu-id="3d45e-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="3d45e-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indica se il token genera callback in modo proattivo.</span><span class="sxs-lookup"><span data-stu-id="3d45e-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="3d45e-113">Se `ActiveChangedCallbacks` è impostato su `false`, non viene mai chiamato alcun callback e l'app deve eseguire il polling di `HasChanged` per ottenere le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3d45e-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="3d45e-114">È anche possibile che un token non venga mai annullato se non vengono apportate modifiche o se il listener di modifica sottostante viene eliminato o disabilitato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="3d45e-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> riceve un valore che indica se è stata apportata una modifica.</span><span class="sxs-lookup"><span data-stu-id="3d45e-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="3d45e-116">L'interfaccia `IChangeToken` include il metodo [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*), che registra un callback che viene richiamato in seguito alla modifica del token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-116">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="3d45e-117">Prima che il callback venga richiamato è necessario impostare `HasChanged`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="3d45e-118">Classe ChangeToken</span><span class="sxs-lookup"><span data-stu-id="3d45e-118">ChangeToken class</span></span>

<span data-ttu-id="3d45e-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> è una classe statica usata per propagare le notifiche relative a una modifica che è stata apportata.</span><span class="sxs-lookup"><span data-stu-id="3d45e-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="3d45e-120">`ChangeToken` risiede nello spazio dei nomi <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-120">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="3d45e-121">Il pacchetto NuGet [Microsoft. Extensions. Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) viene fornito in modo implicito alle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3d45e-121">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="3d45e-122">Il metodo [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) registra un elemento `Action` da chiamare quando il token viene modificato:</span><span class="sxs-lookup"><span data-stu-id="3d45e-122">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="3d45e-123">`Func<IChangeToken>` produce il token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="3d45e-124">`Action` viene chiamato alla modifica del token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="3d45e-125">L'overload [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) accetta un parametro `TState` aggiuntivo che viene passato nell'elemento `Action` consumer del token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-125">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="3d45e-126">`OnChange` restituisce un oggetto <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-126">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="3d45e-127">La chiamata a <xref:System.IDisposable.Dispose*> interrompe l'ascolto di altre modifiche da parte del token e rilascia le risorse del token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-127">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="3d45e-128">Esempi di uso di token di modifica in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d45e-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="3d45e-129">I token di modifica vengono usati nelle aree principali di ASP.NET Core per monitorare le modifiche apportate agli oggetti:</span><span class="sxs-lookup"><span data-stu-id="3d45e-129">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="3d45e-130">Per monitorare le modifiche ai file, il metodo <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> di <xref:Microsoft.Extensions.FileProviders.IFileProvider> crea un elemento `IChangeToken` per le cartelle o i file specificati da controllare.</span><span class="sxs-lookup"><span data-stu-id="3d45e-130">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="3d45e-131">È possibile aggiungere token `IChangeToken` alle voci della cache per attivare le eliminazioni dalla cache alla modifica.</span><span class="sxs-lookup"><span data-stu-id="3d45e-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="3d45e-132">Per le modifiche di `TOptions`, l'implementazione <xref:Microsoft.Extensions.Options.OptionsMonitor`1> predefinita di <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> ha un overload che accetta una o più istanze di <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-132">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="3d45e-133">Ogni istanza restituisce un elemento `IChangeToken` per registrare un callback di notifica delle modifiche per il rilevamento delle modifiche apportate alle opzioni.</span><span class="sxs-lookup"><span data-stu-id="3d45e-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="3d45e-134">Monitorare le modifiche di configurazione</span><span class="sxs-lookup"><span data-stu-id="3d45e-134">Monitor for configuration changes</span></span>

<span data-ttu-id="3d45e-135">Per impostazione predefinita, i modelli ASP.NET Core usano [file di configurazione JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json* e *appsettings.Production.json*) per caricare le impostazioni di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="3d45e-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="3d45e-136">Questi file vengono configurati usando il metodo di estensione [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) per <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> che accetta un parametro `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="3d45e-137">`reloadOnChange` indica se la configurazione deve essere ricaricata alla modifica del file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="3d45e-138">Questa impostazione viene visualizzata nel metodo pratico <xref:Microsoft.Extensions.Hosting.Host> <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="3d45e-138">This setting appears in the <xref:Microsoft.Extensions.Hosting.Host> convenience method <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="3d45e-139">La configurazione basata su file è rappresentata da <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-139">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="3d45e-140">`FileConfigurationSource` usa <xref:Microsoft.Extensions.FileProviders.IFileProvider> per monitorare i file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-140">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="3d45e-141">Per impostazione predefinita, `IFileMonitor` viene offerto da una classe <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> che usa <xref:System.IO.FileSystemWatcher> per monitorare le modifiche apportate al file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-141">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="3d45e-142">L'app di esempio illustra due implementazioni per il monitoraggio delle modifiche alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="3d45e-143">In caso di modifica di uno qualsiasi dei file *appsettings*, entrambe le implementazioni di monitoraggio dei file eseguono codice personalizzato e l'app di esempio scrive un messaggio nella console.</span><span class="sxs-lookup"><span data-stu-id="3d45e-143">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="3d45e-144">L'elemento `FileSystemWatcher` di un file di configurazione può attivare più callback del token per una singola modifica del file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-144">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="3d45e-145">Per assicurarsi che il codice personalizzato venga eseguito solo una volta quando vengono attivati più callback del token, l'implementazione dell'esempio controlla gli hash dei file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-145">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="3d45e-146">L'esempio usa l'hash di file SHA1.</span><span class="sxs-lookup"><span data-stu-id="3d45e-146">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="3d45e-147">Viene implementato un nuovo tentativo con un'interruzione temporanea esponenziale.</span><span class="sxs-lookup"><span data-stu-id="3d45e-147">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="3d45e-148">Il nuovo tentativo è presente perché potrebbe verificarsi un blocco di file che impedisce temporaneamente l'elaborazione di un nuovo hash in un file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-148">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="3d45e-149">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-149">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="3d45e-150">Semplice token di modifica dell'avvio</span><span class="sxs-lookup"><span data-stu-id="3d45e-150">Simple startup change token</span></span>

<span data-ttu-id="3d45e-151">Registrare un callback dell'elemento `Action` consumer del token per le notifiche di modifica al token di ricaricamento della configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-151">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="3d45e-152">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3d45e-152">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="3d45e-153">`config.GetReloadToken()` fornisce il token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="3d45e-154">Il callback è il metodo `InvokeChanged`:</span><span class="sxs-lookup"><span data-stu-id="3d45e-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="3d45e-155">Il valore `state` del callback viene usato per passare `IWebHostEnvironment`, che è utile per specificare il file di configurazione *appsettings* corretto da monitorare (ad esempio, *appsettings.Development.json* nell'ambiente di sviluppo).</span><span class="sxs-lookup"><span data-stu-id="3d45e-155">The `state` of the callback is used to pass in the `IWebHostEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="3d45e-156">Gli hash dei file vengono usati per impedire che l'istruzione `WriteConsole` venga eseguita più volte a causa di più callback del token quando il file di configurazione è stato modificato una sola volta.</span><span class="sxs-lookup"><span data-stu-id="3d45e-156">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="3d45e-157">Questo sistema rimane in esecuzione finché l'app è in esecuzione e non può essere disabilitato dall'utente.</span><span class="sxs-lookup"><span data-stu-id="3d45e-157">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="3d45e-158">Monitorare le modifiche di configurazione come servizio</span><span class="sxs-lookup"><span data-stu-id="3d45e-158">Monitor configuration changes as a service</span></span>

<span data-ttu-id="3d45e-159">L'esempio implementa:</span><span class="sxs-lookup"><span data-stu-id="3d45e-159">The sample implements:</span></span>

* <span data-ttu-id="3d45e-160">Monitoraggio di base del token di avvio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-160">Basic startup token monitoring.</span></span>
* <span data-ttu-id="3d45e-161">Monitoraggio come servizio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-161">Monitoring as a service.</span></span>
* <span data-ttu-id="3d45e-162">Un meccanismo per abilitare e disabilitare il monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-162">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="3d45e-163">L'esempio stabilisce un'interfaccia `IConfigurationMonitor`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-163">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="3d45e-164">*Extensions/ConfigurationMonitor.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-164">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="3d45e-165">Il costruttore della classe implementata, `ConfigurationMonitor`, registra un callback per le notifiche di modifica:</span><span class="sxs-lookup"><span data-stu-id="3d45e-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="3d45e-166">`config.GetReloadToken()` fornisce il token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="3d45e-167">`InvokeChanged` è il metodo di callback.</span><span class="sxs-lookup"><span data-stu-id="3d45e-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="3d45e-168">Il valore `state` in questa istanza è un riferimento all'istanza `IConfigurationMonitor` usata per accedere allo stato di monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="3d45e-169">Vengono usate due proprietà:</span><span class="sxs-lookup"><span data-stu-id="3d45e-169">Two properties are used:</span></span>

* <span data-ttu-id="3d45e-170">`MonitoringEnabled` &ndash; Indica se il callback deve eseguire il proprio codice personalizzato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-170">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="3d45e-171">`CurrentState` &ndash; Descrive lo stato di monitoraggio corrente per l'uso nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="3d45e-171">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="3d45e-172">Il metodo `InvokeChanged` è simile all'approccio precedente, ad eccezione del fatto che:</span><span class="sxs-lookup"><span data-stu-id="3d45e-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="3d45e-173">Non esegue il proprio codice a meno che `MonitoringEnabled` non sia impostato su `true`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="3d45e-174">Restituisce l'elemento `state` corrente nell'output `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-174">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="3d45e-175">Un'istanza di `ConfigurationMonitor` viene registrata come servizio in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3d45e-175">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="3d45e-176">La pagina di indice offre all'utente il controllo sul monitoraggio della configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="3d45e-177">L'istanza di `IConfigurationMonitor` viene inserita in `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="3d45e-178">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-178">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="3d45e-179">Il monitoraggio di configurazione (`_monitor`) viene usato per abilitare o disabilitare il monitoraggio e impostare lo stato corrente per commenti e suggerimenti dell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="3d45e-179">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="3d45e-180">Quando viene attivato `OnPostStartMonitoring`, il monitoraggio è abilitato e lo stato corrente viene cancellato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="3d45e-181">Quando viene attivato `OnPostStopMonitoring`, il monitoraggio è disabilitato e lo stato viene impostato in modo da indicare che il monitoraggio non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="3d45e-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="3d45e-182">I pulsanti nell'interfaccia utente abilitano e disabilitano il monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-182">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="3d45e-183">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-183">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="3d45e-184">Monitorare le modifiche al file nella cache</span><span class="sxs-lookup"><span data-stu-id="3d45e-184">Monitor cached file changes</span></span>

<span data-ttu-id="3d45e-185">È possibile memorizzare il contenuto del file nella cache in memoria usando <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-185">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="3d45e-186">La memorizzazione nella cache in memoria è descritta nell'argomento [Cache in memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="3d45e-186">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="3d45e-187">Senza eseguire passaggi aggiuntivi, ad esempio l'implementazione descritta di seguito, la cache restituirà dati *non aggiornati* (obsoleti) se i dati di origine vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="3d45e-187">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="3d45e-188">Ad esempio, se durante il rinnovo di un periodo di [scadenza variabile](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) non si tiene conto dello stato di un file di origine memorizzato nella cache, i dati relativi ai file nella cache risulteranno non aggiornati.</span><span class="sxs-lookup"><span data-stu-id="3d45e-188">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="3d45e-189">Ogni richiesta di dati rinnoverà il periodo di scadenza variabile, ma il file non verrà mai ricaricato nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-189">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="3d45e-190">Le funzionalità dell'app che usano il contenuto del file memorizzato nella cache sono soggette alla possibile ricezione di contenuto non aggiornato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-190">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="3d45e-191">L'uso dei token di modifica in uno scenario di memorizzazione di file nella cache consente di evitare la presenza di contenuto dei file non aggiornato nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-191">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="3d45e-192">L'app di esempio illustra un'implementazione dell'approccio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-192">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="3d45e-193">Nell'esempio viene usato `GetFileContent` per:</span><span class="sxs-lookup"><span data-stu-id="3d45e-193">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="3d45e-194">Restituire il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-194">Return file content.</span></span>
* <span data-ttu-id="3d45e-195">Implementare un algoritmo di nuovi tentativi con interruzione temporanea esponenziale per i casi in cui un blocco di file impedisca temporaneamente la lettura di un file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-195">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="3d45e-196">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-196">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="3d45e-197">Viene creato un elemento `FileService` per gestire le ricerche nel file memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-197">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="3d45e-198">La chiamata del servizio al metodo `GetFileContent` prova a ottenere il contenuto del file dalla cache in memoria e a restituirlo al chiamante (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="3d45e-198">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="3d45e-199">Se non è possibile reperire il contenuto memorizzato nella cache usando la chiave della cache, verranno intraprese le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="3d45e-199">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="3d45e-200">Il contenuto del file viene ottenuto usando `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-200">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="3d45e-201">Un token di modifica viene ottenuto dal provider di file con [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="3d45e-201">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="3d45e-202">Il callback del token viene attivato alla modifica del file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-202">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="3d45e-203">Il contenuto del file viene memorizzato nella cache con un periodo di [scadenza variabile](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration).</span><span class="sxs-lookup"><span data-stu-id="3d45e-203">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="3d45e-204">Al token di modifica viene associato [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) per eliminare la voce della cache se il file viene modificato mentre è memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-204">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="3d45e-205">Nell'esempio seguente i file vengono archiviati nella [radice del contenuto](xref:fundamentals/index#content-root)dell'app.</span><span class="sxs-lookup"><span data-stu-id="3d45e-205">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="3d45e-206">`IWebHostEnvironment.ContentRootFileProvider` viene usato per ottenere un <xref:Microsoft.Extensions.FileProviders.IFileProvider> che punta al `IWebHostEnvironment.ContentRootPath` dell'app.</span><span class="sxs-lookup"><span data-stu-id="3d45e-206">`IWebHostEnvironment.ContentRootFileProvider` is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's `IWebHostEnvironment.ContentRootPath`.</span></span> <span data-ttu-id="3d45e-207">`filePath` viene ottenuto con [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="3d45e-207">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="3d45e-208">`FileService` è registrato nel contenitore di servizi insieme al servizio di memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-208">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="3d45e-209">In `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3d45e-209">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="3d45e-210">Il modello di pagina carica il contenuto del file usando il servizio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-210">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="3d45e-211">Nel metodo `OnGet` della pagina Index (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="3d45e-211">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="3d45e-212">Classe CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="3d45e-212">CompositeChangeToken class</span></span>

<span data-ttu-id="3d45e-213">Per rappresentare una o più istanze di `IChangeToken` in un singolo oggetto, usare la classe <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-213">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

<span data-ttu-id="3d45e-214">`HasChanged` nel token composito indica `true` se l'elemento `HasChanged` di qualsiasi token rappresentato è `true`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-214">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="3d45e-215">`ActiveChangeCallbacks` nel token composito indica `true` se l'elemento `ActiveChangeCallbacks` di qualsiasi token rappresentato è `true`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-215">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="3d45e-216">Se si verificano più eventi di modifica simultanei, il callback della modifica composita viene richiamato una volta.</span><span class="sxs-lookup"><span data-stu-id="3d45e-216">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3d45e-217">Un *token di modifica* è un blocco predefinito di uso generico e di basso livello che viene usato per il rilevamento delle modifiche di stato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-217">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="3d45e-218">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3d45e-218">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="3d45e-219">Interfaccia IChangeToken</span><span class="sxs-lookup"><span data-stu-id="3d45e-219">IChangeToken interface</span></span>

<span data-ttu-id="3d45e-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propaga le notifiche relative a una modifica apportata.</span><span class="sxs-lookup"><span data-stu-id="3d45e-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="3d45e-221">`IChangeToken` risiede nello spazio dei nomi <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-221">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="3d45e-222">Per le app che non usano il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto per il pacchetto NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/).</span><span class="sxs-lookup"><span data-stu-id="3d45e-222">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="3d45e-223">`IChangeToken` ha due proprietà:</span><span class="sxs-lookup"><span data-stu-id="3d45e-223">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="3d45e-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indica se il token genera callback in modo proattivo.</span><span class="sxs-lookup"><span data-stu-id="3d45e-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="3d45e-225">Se `ActiveChangedCallbacks` è impostato su `false`, non viene mai chiamato alcun callback e l'app deve eseguire il polling di `HasChanged` per ottenere le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3d45e-225">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="3d45e-226">È anche possibile che un token non venga mai annullato se non vengono apportate modifiche o se il listener di modifica sottostante viene eliminato o disabilitato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-226">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="3d45e-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> riceve un valore che indica se è stata apportata una modifica.</span><span class="sxs-lookup"><span data-stu-id="3d45e-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="3d45e-228">L'interfaccia `IChangeToken` include il metodo [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*), che registra un callback che viene richiamato in seguito alla modifica del token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-228">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="3d45e-229">Prima che il callback venga richiamato è necessario impostare `HasChanged`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-229">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="3d45e-230">Classe ChangeToken</span><span class="sxs-lookup"><span data-stu-id="3d45e-230">ChangeToken class</span></span>

<span data-ttu-id="3d45e-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> è una classe statica usata per propagare le notifiche relative a una modifica che è stata apportata.</span><span class="sxs-lookup"><span data-stu-id="3d45e-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="3d45e-232">`ChangeToken` risiede nello spazio dei nomi <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-232">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="3d45e-233">Per le app che non usano il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto per il pacchetto NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/).</span><span class="sxs-lookup"><span data-stu-id="3d45e-233">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="3d45e-234">Il metodo [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) registra un elemento `Action` da chiamare quando il token viene modificato:</span><span class="sxs-lookup"><span data-stu-id="3d45e-234">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="3d45e-235">`Func<IChangeToken>` produce il token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-235">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="3d45e-236">`Action` viene chiamato alla modifica del token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-236">`Action` is called when the token changes.</span></span>

<span data-ttu-id="3d45e-237">L'overload [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) accetta un parametro `TState` aggiuntivo che viene passato nell'elemento `Action` consumer del token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-237">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="3d45e-238">`OnChange` restituisce un oggetto <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-238">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="3d45e-239">La chiamata a <xref:System.IDisposable.Dispose*> interrompe l'ascolto di altre modifiche da parte del token e rilascia le risorse del token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-239">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="3d45e-240">Esempi di uso di token di modifica in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d45e-240">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="3d45e-241">I token di modifica vengono usati nelle aree principali di ASP.NET Core per monitorare le modifiche apportate agli oggetti:</span><span class="sxs-lookup"><span data-stu-id="3d45e-241">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="3d45e-242">Per monitorare le modifiche ai file, il metodo <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> di <xref:Microsoft.Extensions.FileProviders.IFileProvider> crea un elemento `IChangeToken` per le cartelle o i file specificati da controllare.</span><span class="sxs-lookup"><span data-stu-id="3d45e-242">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="3d45e-243">È possibile aggiungere token `IChangeToken` alle voci della cache per attivare le eliminazioni dalla cache alla modifica.</span><span class="sxs-lookup"><span data-stu-id="3d45e-243">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="3d45e-244">Per le modifiche di `TOptions`, l'implementazione <xref:Microsoft.Extensions.Options.OptionsMonitor`1> predefinita di <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> ha un overload che accetta una o più istanze di <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-244">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="3d45e-245">Ogni istanza restituisce un elemento `IChangeToken` per registrare un callback di notifica delle modifiche per il rilevamento delle modifiche apportate alle opzioni.</span><span class="sxs-lookup"><span data-stu-id="3d45e-245">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="3d45e-246">Monitorare le modifiche di configurazione</span><span class="sxs-lookup"><span data-stu-id="3d45e-246">Monitor for configuration changes</span></span>

<span data-ttu-id="3d45e-247">Per impostazione predefinita, i modelli ASP.NET Core usano [file di configurazione JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json* e *appsettings.Production.json*) per caricare le impostazioni di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="3d45e-247">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="3d45e-248">Questi file vengono configurati usando il metodo di estensione [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) per <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> che accetta un parametro `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-248">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="3d45e-249">`reloadOnChange` indica se la configurazione deve essere ricaricata alla modifica del file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-249">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="3d45e-250">Questa impostazione viene visualizzata nel metodo pratico <xref:Microsoft.AspNetCore.WebHost> <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="3d45e-250">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="3d45e-251">La configurazione basata su file è rappresentata da <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-251">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="3d45e-252">`FileConfigurationSource` usa <xref:Microsoft.Extensions.FileProviders.IFileProvider> per monitorare i file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-252">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="3d45e-253">Per impostazione predefinita, `IFileMonitor` viene offerto da una classe <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> che usa <xref:System.IO.FileSystemWatcher> per monitorare le modifiche apportate al file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-253">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="3d45e-254">L'app di esempio illustra due implementazioni per il monitoraggio delle modifiche alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-254">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="3d45e-255">In caso di modifica di uno qualsiasi dei file *appsettings*, entrambe le implementazioni di monitoraggio dei file eseguono codice personalizzato e l'app di esempio scrive un messaggio nella console.</span><span class="sxs-lookup"><span data-stu-id="3d45e-255">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="3d45e-256">L'elemento `FileSystemWatcher` di un file di configurazione può attivare più callback del token per una singola modifica del file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-256">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="3d45e-257">Per assicurarsi che il codice personalizzato venga eseguito solo una volta quando vengono attivati più callback del token, l'implementazione dell'esempio controlla gli hash dei file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-257">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="3d45e-258">L'esempio usa l'hash di file SHA1.</span><span class="sxs-lookup"><span data-stu-id="3d45e-258">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="3d45e-259">Viene implementato un nuovo tentativo con un'interruzione temporanea esponenziale.</span><span class="sxs-lookup"><span data-stu-id="3d45e-259">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="3d45e-260">Il nuovo tentativo è presente perché potrebbe verificarsi un blocco di file che impedisce temporaneamente l'elaborazione di un nuovo hash in un file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-260">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="3d45e-261">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-261">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="3d45e-262">Semplice token di modifica dell'avvio</span><span class="sxs-lookup"><span data-stu-id="3d45e-262">Simple startup change token</span></span>

<span data-ttu-id="3d45e-263">Registrare un callback dell'elemento `Action` consumer del token per le notifiche di modifica al token di ricaricamento della configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-263">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="3d45e-264">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3d45e-264">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="3d45e-265">`config.GetReloadToken()` fornisce il token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-265">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="3d45e-266">Il callback è il metodo `InvokeChanged`:</span><span class="sxs-lookup"><span data-stu-id="3d45e-266">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="3d45e-267">Il valore `state` del callback viene usato per passare `IHostingEnvironment`, che è utile per specificare il file di configurazione *appsettings* corretto da monitorare (ad esempio, *appsettings.Development.json* nell'ambiente di sviluppo).</span><span class="sxs-lookup"><span data-stu-id="3d45e-267">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="3d45e-268">Gli hash dei file vengono usati per impedire che l'istruzione `WriteConsole` venga eseguita più volte a causa di più callback del token quando il file di configurazione è stato modificato una sola volta.</span><span class="sxs-lookup"><span data-stu-id="3d45e-268">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="3d45e-269">Questo sistema rimane in esecuzione finché l'app è in esecuzione e non può essere disabilitato dall'utente.</span><span class="sxs-lookup"><span data-stu-id="3d45e-269">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="3d45e-270">Monitorare le modifiche di configurazione come servizio</span><span class="sxs-lookup"><span data-stu-id="3d45e-270">Monitor configuration changes as a service</span></span>

<span data-ttu-id="3d45e-271">L'esempio implementa:</span><span class="sxs-lookup"><span data-stu-id="3d45e-271">The sample implements:</span></span>

* <span data-ttu-id="3d45e-272">Monitoraggio di base del token di avvio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-272">Basic startup token monitoring.</span></span>
* <span data-ttu-id="3d45e-273">Monitoraggio come servizio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-273">Monitoring as a service.</span></span>
* <span data-ttu-id="3d45e-274">Un meccanismo per abilitare e disabilitare il monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-274">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="3d45e-275">L'esempio stabilisce un'interfaccia `IConfigurationMonitor`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-275">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="3d45e-276">*Extensions/ConfigurationMonitor.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-276">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="3d45e-277">Il costruttore della classe implementata, `ConfigurationMonitor`, registra un callback per le notifiche di modifica:</span><span class="sxs-lookup"><span data-stu-id="3d45e-277">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="3d45e-278">`config.GetReloadToken()` fornisce il token.</span><span class="sxs-lookup"><span data-stu-id="3d45e-278">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="3d45e-279">`InvokeChanged` è il metodo di callback.</span><span class="sxs-lookup"><span data-stu-id="3d45e-279">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="3d45e-280">Il valore `state` in questa istanza è un riferimento all'istanza `IConfigurationMonitor` usata per accedere allo stato di monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-280">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="3d45e-281">Vengono usate due proprietà:</span><span class="sxs-lookup"><span data-stu-id="3d45e-281">Two properties are used:</span></span>

* <span data-ttu-id="3d45e-282">`MonitoringEnabled` &ndash; Indica se il callback deve eseguire il proprio codice personalizzato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-282">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="3d45e-283">`CurrentState` &ndash; Descrive lo stato di monitoraggio corrente per l'uso nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="3d45e-283">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="3d45e-284">Il metodo `InvokeChanged` è simile all'approccio precedente, ad eccezione del fatto che:</span><span class="sxs-lookup"><span data-stu-id="3d45e-284">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="3d45e-285">Non esegue il proprio codice a meno che `MonitoringEnabled` non sia impostato su `true`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-285">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="3d45e-286">Restituisce l'elemento `state` corrente nell'output `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-286">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="3d45e-287">Un'istanza di `ConfigurationMonitor` viene registrata come servizio in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3d45e-287">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="3d45e-288">La pagina di indice offre all'utente il controllo sul monitoraggio della configurazione.</span><span class="sxs-lookup"><span data-stu-id="3d45e-288">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="3d45e-289">L'istanza di `IConfigurationMonitor` viene inserita in `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-289">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="3d45e-290">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-290">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="3d45e-291">Il monitoraggio di configurazione (`_monitor`) viene usato per abilitare o disabilitare il monitoraggio e impostare lo stato corrente per commenti e suggerimenti dell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="3d45e-291">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="3d45e-292">Quando viene attivato `OnPostStartMonitoring`, il monitoraggio è abilitato e lo stato corrente viene cancellato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-292">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="3d45e-293">Quando viene attivato `OnPostStopMonitoring`, il monitoraggio è disabilitato e lo stato viene impostato in modo da indicare che il monitoraggio non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="3d45e-293">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="3d45e-294">I pulsanti nell'interfaccia utente abilitano e disabilitano il monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-294">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="3d45e-295">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-295">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="3d45e-296">Monitorare le modifiche al file nella cache</span><span class="sxs-lookup"><span data-stu-id="3d45e-296">Monitor cached file changes</span></span>

<span data-ttu-id="3d45e-297">È possibile memorizzare il contenuto del file nella cache in memoria usando <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-297">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="3d45e-298">La memorizzazione nella cache in memoria è descritta nell'argomento [Cache in memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="3d45e-298">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="3d45e-299">Senza eseguire passaggi aggiuntivi, ad esempio l'implementazione descritta di seguito, la cache restituirà dati *non aggiornati* (obsoleti) se i dati di origine vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="3d45e-299">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="3d45e-300">Ad esempio, se durante il rinnovo di un periodo di [scadenza variabile](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) non si tiene conto dello stato di un file di origine memorizzato nella cache, i dati relativi ai file nella cache risulteranno non aggiornati.</span><span class="sxs-lookup"><span data-stu-id="3d45e-300">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="3d45e-301">Ogni richiesta di dati rinnoverà il periodo di scadenza variabile, ma il file non verrà mai ricaricato nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-301">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="3d45e-302">Le funzionalità dell'app che usano il contenuto del file memorizzato nella cache sono soggette alla possibile ricezione di contenuto non aggiornato.</span><span class="sxs-lookup"><span data-stu-id="3d45e-302">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="3d45e-303">L'uso dei token di modifica in uno scenario di memorizzazione di file nella cache consente di evitare la presenza di contenuto dei file non aggiornato nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-303">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="3d45e-304">L'app di esempio illustra un'implementazione dell'approccio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-304">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="3d45e-305">Nell'esempio viene usato `GetFileContent` per:</span><span class="sxs-lookup"><span data-stu-id="3d45e-305">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="3d45e-306">Restituire il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-306">Return file content.</span></span>
* <span data-ttu-id="3d45e-307">Implementare un algoritmo di nuovi tentativi con interruzione temporanea esponenziale per i casi in cui un blocco di file impedisca temporaneamente la lettura di un file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-307">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="3d45e-308">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d45e-308">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="3d45e-309">Viene creato un elemento `FileService` per gestire le ricerche nel file memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-309">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="3d45e-310">La chiamata del servizio al metodo `GetFileContent` prova a ottenere il contenuto del file dalla cache in memoria e a restituirlo al chiamante (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="3d45e-310">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="3d45e-311">Se non è possibile reperire il contenuto memorizzato nella cache usando la chiave della cache, verranno intraprese le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="3d45e-311">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="3d45e-312">Il contenuto del file viene ottenuto usando `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-312">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="3d45e-313">Un token di modifica viene ottenuto dal provider di file con [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="3d45e-313">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="3d45e-314">Il callback del token viene attivato alla modifica del file.</span><span class="sxs-lookup"><span data-stu-id="3d45e-314">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="3d45e-315">Il contenuto del file viene memorizzato nella cache con un periodo di [scadenza variabile](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration).</span><span class="sxs-lookup"><span data-stu-id="3d45e-315">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="3d45e-316">Al token di modifica viene associato [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) per eliminare la voce della cache se il file viene modificato mentre è memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-316">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="3d45e-317">Nell'esempio seguente i file vengono archiviati nella [radice del contenuto](xref:fundamentals/index#content-root)dell'app.</span><span class="sxs-lookup"><span data-stu-id="3d45e-317">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="3d45e-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) viene usato per ottenere un <xref:Microsoft.Extensions.FileProviders.IFileProvider> che punta al <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath> dell'app.</span><span class="sxs-lookup"><span data-stu-id="3d45e-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="3d45e-319">`filePath` viene ottenuto con [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="3d45e-319">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="3d45e-320">`FileService` è registrato nel contenitore di servizi insieme al servizio di memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d45e-320">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="3d45e-321">In `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3d45e-321">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="3d45e-322">Il modello di pagina carica il contenuto del file usando il servizio.</span><span class="sxs-lookup"><span data-stu-id="3d45e-322">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="3d45e-323">Nel metodo `OnGet` della pagina Index (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="3d45e-323">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="3d45e-324">Classe CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="3d45e-324">CompositeChangeToken class</span></span>

<span data-ttu-id="3d45e-325">Per rappresentare una o più istanze di `IChangeToken` in un singolo oggetto, usare la classe <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="3d45e-325">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

<span data-ttu-id="3d45e-326">`HasChanged` nel token composito indica `true` se l'elemento `HasChanged` di qualsiasi token rappresentato è `true`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-326">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="3d45e-327">`ActiveChangeCallbacks` nel token composito indica `true` se l'elemento `ActiveChangeCallbacks` di qualsiasi token rappresentato è `true`.</span><span class="sxs-lookup"><span data-stu-id="3d45e-327">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="3d45e-328">Se si verificano più eventi di modifica simultanei, il callback della modifica composita viene richiamato una volta.</span><span class="sxs-lookup"><span data-stu-id="3d45e-328">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3d45e-329">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3d45e-329">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
