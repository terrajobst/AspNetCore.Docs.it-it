---
title: Migliorare un'app da un assembly esterno in ASP.NET Core con IHostingStartup
author: guardrex
description: Informazioni su come migliorare un'app ASP.NET Core da un assembly esterno con un'implementazione IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: a06c2da04c1631f5811a535c891ca5190b0d8864
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207563"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="bb4af-103">Migliorare un'app da un assembly esterno in ASP.NET Core con IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="bb4af-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="bb4af-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bb4af-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bb4af-105">Un'implementazione [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (avvio dell'hosting) consente l'aggiunta di miglioramenti a un'app all'avvio da un assembly esterno.</span><span class="sxs-lookup"><span data-stu-id="bb4af-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="bb4af-106">Una libreria esterna può ad esempio usare un'implementazione di avvio dell'hosting per offrire servizi o provider di configurazione aggiuntivi a un'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="bb4af-107">`IHostingStartup`  *è disponibile in ASP.NET Core 2.0 o versioni successive.*</span><span class="sxs-lookup"><span data-stu-id="bb4af-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="bb4af-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bb4af-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="bb4af-109">Attributo HostingStartup</span><span class="sxs-lookup"><span data-stu-id="bb4af-109">HostingStartup attribute</span></span>

<span data-ttu-id="bb4af-110">Un attributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) indica la presenza di un assembly di avvio dell'hosting da attivare in fase di runtime.</span><span class="sxs-lookup"><span data-stu-id="bb4af-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="bb4af-111">L'assembly di ingresso o l'assembly contenente la classe `Startup` viene analizzato automaticamente per la ricerca dell'attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="bb4af-112">L'elenco di assembly in cui cercare gli attributi `HostingStartup` viene caricato in fase di runtime dalla configurazione in [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="bb4af-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="bb4af-113">L'elenco di assembly da escludere dalla ricerca viene caricato da [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="bb4af-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="bb4af-114">Per altre informazioni, vedere [Host Web: Assembly di avvio dell'hosting](xref:fundamentals/host/web-host#hosting-startup-assemblies) e [Host Web: Assembly di avvio dell'hosting da escludere](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="bb4af-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="bb4af-115">Nell'esempio seguente lo spazio dei nomi dell'assembly di avvio dell'hosting è `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="bb4af-116">La classe che contiene il codice di avvio dell'hosting è `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="bb4af-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="bb4af-117">L'attributo `HostingStartup` si trova in genere nel file della classe di implementazione `IHostingStartup` dell'assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="bb4af-118">Individuare gli assembly di avvio dell'hosting caricati</span><span class="sxs-lookup"><span data-stu-id="bb4af-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="bb4af-119">Per individuare gli assembly di avvio dell'hosting caricati, abilitare la registrazione e controllare i log dell'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="bb4af-120">Gli errori che si verificano durante il caricamento degli assembly vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="bb4af-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="bb4af-121">Gli assembly di avvio dell'hosting caricati vengono registrati a livello di debug e tutti gli errori vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="bb4af-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="bb4af-122">Disabilitare il caricamento automatico di assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="bb4af-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bb4af-123">Per disabilitare il caricamento automatico degli assembly di avvio dell'hosting, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb4af-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="bb4af-124">Per evitare il caricamento di tutti gli assembly di avvio dell'hosting, impostare uno degli elementi seguenti su `true` o `1`:</span><span class="sxs-lookup"><span data-stu-id="bb4af-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="bb4af-125">Impostazione di configurazione host per [impedire l'avvio dell'hosting](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="bb4af-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="bb4af-126">La variabile di ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="bb4af-127">Per impedire il caricamento di specifici assembly di avvio dell'hosting, impostare uno degli elementi seguenti su una stringa delimitata da punti e virgola di assembly di avvio dell'hosting da escludere all'avvio:</span><span class="sxs-lookup"><span data-stu-id="bb4af-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="bb4af-128">Impostazione di configurazione host per [assembly di avvio dell'hosting da escludere](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="bb4af-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="bb4af-129">La variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bb4af-130">Per disabilitare il caricamento automatico degli assembly di avvio dell'hosting, impostare uno degli elementi seguenti su `true` o `1`:</span><span class="sxs-lookup"><span data-stu-id="bb4af-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="bb4af-131">Impostazione di configurazione host per [impedire l'avvio dell'hosting](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="bb4af-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="bb4af-132">La variabile di ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="bb4af-133">Se l'impostazione di configurazione host e la variabile di ambiente sono entrambe impostate, l'impostazione host controlla il comportamento.</span><span class="sxs-lookup"><span data-stu-id="bb4af-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="bb4af-134">La disabilitazione degli assembly di avvio dell'hosting tramite l'impostazione host o la variabile di ambiente ne determina la disabilitazione globale e la possibile disabilitazione di diverse caratteristiche di un'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="bb4af-135">Progetto</span><span class="sxs-lookup"><span data-stu-id="bb4af-135">Project</span></span>

<span data-ttu-id="bb4af-136">Creare l'avvio dell'hosting con uno dei tipi di progetto seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb4af-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="bb4af-137">Libreria di classi</span><span class="sxs-lookup"><span data-stu-id="bb4af-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="bb4af-138">App console senza un punto di ingresso</span><span class="sxs-lookup"><span data-stu-id="bb4af-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="bb4af-139">Libreria di classi</span><span class="sxs-lookup"><span data-stu-id="bb4af-139">Class library</span></span>

<span data-ttu-id="bb4af-140">È possibile includere un miglioramento di avvio dell'hosting in una libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="bb4af-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="bb4af-141">La libreria contiene un attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="bb4af-142">Il [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) include un'app Razor Pages, *HostingStartupApp*, e una libreria di classi, *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="bb4af-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="bb4af-143">La libreria di classi:</span><span class="sxs-lookup"><span data-stu-id="bb4af-143">The class library:</span></span>

* <span data-ttu-id="bb4af-144">Contiene una classe di avvio dell'hosting, `ServiceKeyInjection`, che implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="bb4af-145">`ServiceKeyInjection` aggiunge una coppia di stringhe di servizio alla configurazione dell'app tramite il provider di configurazione in memoria ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="bb4af-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="bb4af-146">Include un attributo `HostingStartup` che identifica lo spazio dei nomi e la classe di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="bb4af-147">Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) della classe `ServiceKeyInjection` usa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere miglioramenti a un'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="bb4af-148">Nell'assembly di avvio dell'hosting `IHostingStartup.Configure` viene chiamato dal runtime prima di `Startup.Configure` nel codice utente, il che consente al codice utente di sovrascrivere qualsiasi configurazione fornita dall'assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="bb4af-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb4af-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="bb4af-150">La pagina Indice dell'app legge ed esegue il rendering dei valori di configurazione per le due chiavi impostate dall'assembly di avvio dell'hosting della libreria di classi:</span><span class="sxs-lookup"><span data-stu-id="bb4af-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="bb4af-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb4af-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="bb4af-152">Il [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) include anche un progetto di pacchetto NuGet che offre un avvio dell'hosting separato, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="bb4af-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="bb4af-153">Il pacchetto ha le stesse caratteristiche della libreria di classi descritta in precedenza.</span><span class="sxs-lookup"><span data-stu-id="bb4af-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="bb4af-154">Il pacchetto:</span><span class="sxs-lookup"><span data-stu-id="bb4af-154">The package:</span></span>

* <span data-ttu-id="bb4af-155">Contiene una classe di avvio dell'hosting, `ServiceKeyInjection`, che implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="bb4af-156">`ServiceKeyInjection` aggiunge una coppia di stringhe di servizio alla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="bb4af-157">Include un attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="bb4af-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb4af-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="bb4af-159">La pagina Indice dell'app legge ed esegue il rendering dei valori di configurazione per le due chiavi impostate dall'assembly di avvio dell'hosting del pacchetto:</span><span class="sxs-lookup"><span data-stu-id="bb4af-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="bb4af-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb4af-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="bb4af-161">App console senza un punto di ingresso</span><span class="sxs-lookup"><span data-stu-id="bb4af-161">Console app without an entry point</span></span>

<span data-ttu-id="bb4af-162">*Questo approccio è disponibile solo per le app .NET Core, non .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="bb4af-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="bb4af-163">Un miglioramento di avvio dell'hosting dinamico che non richiede un riferimento in fase di compilazione per l'attivazione può essere fornito in un'app console senza un punto di ingresso.</span><span class="sxs-lookup"><span data-stu-id="bb4af-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point.</span></span> <span data-ttu-id="bb4af-164">L'app contiene un attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-164">The app contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="bb4af-165">Per creare un avvio dell'hosting dinamico:</span><span class="sxs-lookup"><span data-stu-id="bb4af-165">To create a dynamic hosting startup:</span></span>

1. <span data-ttu-id="bb4af-166">Viene creata una libreria di implementazione dalla classe che contiene l'implementazione `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-166">An implementation library is created from the class that contains the `IHostingStartup` implementation.</span></span> <span data-ttu-id="bb4af-167">La libreria di implementazione viene trattata come un normale pacchetto.</span><span class="sxs-lookup"><span data-stu-id="bb4af-167">The implementation library is treated as a normal package.</span></span>
1. <span data-ttu-id="bb4af-168">Un'app console senza un punto di ingresso fa riferimento al pacchetto della libreria di implementazione.</span><span class="sxs-lookup"><span data-stu-id="bb4af-168">A console app without an entry point references the implementation library package.</span></span> <span data-ttu-id="bb4af-169">Viene usata un'app console perché:</span><span class="sxs-lookup"><span data-stu-id="bb4af-169">A console app is used because:</span></span>
   * <span data-ttu-id="bb4af-170">Poiché un file di dipendenze è un asset di app eseguibile, una libreria non può fornire un file di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="bb4af-170">A dependencies file is a runnable app asset, so a library can't furnish a dependencies file.</span></span>
   * <span data-ttu-id="bb4af-171">Una libreria non può essere aggiunta direttamente all'[archivio dei pacchetti di runtime](/dotnet/core/deploying/runtime-store) che richiede un progetto eseguibile indirizzato al runtime condiviso.</span><span class="sxs-lookup"><span data-stu-id="bb4af-171">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>
1. <span data-ttu-id="bb4af-172">L'app console viene pubblicata per ottenere le dipendenze dell'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-172">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="bb4af-173">Una conseguenza della pubblicazione dell'app console è che le dipendenze non usate vengono eliminate dal file di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="bb4af-173">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
1. <span data-ttu-id="bb4af-174">L'app e il relativo file di dipendenze vengono inseriti nell'archivio dei pacchetti di runtime.</span><span class="sxs-lookup"><span data-stu-id="bb4af-174">The app and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="bb4af-175">Per individuare l'assembly di avvio dell'hosting e il relativo file di dipendenze, viene fatto riferimento a essi in una coppia di variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="bb4af-175">To discover the hosting startup assembly and its dependencies file, they're referenced in a pair of environment variables.</span></span>

<span data-ttu-id="bb4af-176">L'app console fa riferimento al pacchetto [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="bb4af-176">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="bb4af-177">Un attributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica una classe come un'implementazione di `IHostingStartup` per il caricamento e l'esecuzione al momento della compilazione di [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="bb4af-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="bb4af-178">Nell'esempio seguente, lo spazio dei nomi è `StartupEnhancement` e la classe è `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="bb4af-178">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="bb4af-179">Una classe implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-179">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="bb4af-180">Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) della classe usa un'interfaccia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere miglioramenti a un'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-180">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="bb4af-181">Nell'assembly di avvio dell'hosting `IHostingStartup.Configure` viene chiamato dal runtime prima di `Startup.Configure` nel codice utente, il che consente al codice utente di sovrascrivere qualsiasi configurazione fornita dall'assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-181">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="bb4af-182">Quando si compila un progetto `IHostingStartup`, il file delle dipendenze (*\*.deps.json*) imposta il percorso di `runtime` dell'assembly sulla cartella *bin*:</span><span class="sxs-lookup"><span data-stu-id="bb4af-182">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="bb4af-183">Viene visualizzata solo una parte del file.</span><span class="sxs-lookup"><span data-stu-id="bb4af-183">Only part of the file is shown.</span></span> <span data-ttu-id="bb4af-184">Il nome dell'assembly nell'esempio è `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-184">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="bb4af-185">Specificare l'assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="bb4af-185">Specify the hosting startup assembly</span></span>

<span data-ttu-id="bb4af-186">Per un avvio dell'hosting fornito da una libreria di classi o da un'app console, specificare il nome dell'assembly di avvio dell'hosting nella variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-186">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="bb4af-187">La variabile di ambiente è un elenco di assembly delimitato da punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="bb4af-187">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="bb4af-188">L'attributo `HostingStartup` viene cercato solo negli assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-188">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="bb4af-189">Per l'app di esempio *HostingStartupApp*, per individuare gli avvii dell'hosting descritti in precedenza, la variabile di ambiente viene impostata sul valore seguente:</span><span class="sxs-lookup"><span data-stu-id="bb4af-189">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="bb4af-190">È possibile impostare l'assembly di avvio dell'hosting anche tramite l'impostazione di configurazione host [Assembly di avvio dell'hosting](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="bb4af-190">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="bb4af-191">Quando sono presenti più assembly di avvio dell'hosting, i relativi metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) vengono eseguiti nell'ordine in cui sono elencati gli assembly.</span><span class="sxs-lookup"><span data-stu-id="bb4af-191">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="bb4af-192">Attivazione</span><span class="sxs-lookup"><span data-stu-id="bb4af-192">Activation</span></span>

<span data-ttu-id="bb4af-193">Le opzioni di attivazione dell'avvio dell'hosting sono:</span><span class="sxs-lookup"><span data-stu-id="bb4af-193">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="bb4af-194">[Archivio di runtime](#runtime-store) &ndash; L'attivazione non richiede un riferimento in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="bb4af-194">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="bb4af-195">L'app di esempio inserisce l'assembly di avvio dell'hosting e i file di dipendenze in una cartella *deployment* per facilitare la distribuzione dell'avvio dell'hosting in un ambiente con più computer.</span><span class="sxs-lookup"><span data-stu-id="bb4af-195">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="bb4af-196">La cartella *deployment* include anche uno script PowerShell che crea o modifica le variabili di ambiente nel sistema di distribuzione per abilitare l'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-196">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="bb4af-197">Riferimento in fase di compilazione richiesto per l'attivazione</span><span class="sxs-lookup"><span data-stu-id="bb4af-197">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="bb4af-198">Pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="bb4af-198">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="bb4af-199">Cartella bin del progetto</span><span class="sxs-lookup"><span data-stu-id="bb4af-199">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="bb4af-200">Archivio di runtime</span><span class="sxs-lookup"><span data-stu-id="bb4af-200">Runtime store</span></span>

<span data-ttu-id="bb4af-201">L'implementazione dell'avvio dell'hosting viene inserita nell'[archivio di runtime](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="bb4af-201">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="bb4af-202">L'app migliorata non richiede un riferimento in fase di compilazione all'assembly.</span><span class="sxs-lookup"><span data-stu-id="bb4af-202">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="bb4af-203">Dopo la compilazione dell'avvio dell'hosting, il file di progetto dell'avvio dell'hosting svolge la funzione di file manifesto per il comando [dotnet store](/dotnet/core/tools/dotnet-store).</span><span class="sxs-lookup"><span data-stu-id="bb4af-203">After the hosting startup is built, the hosting startup's project file serves as the manifest file for the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

<span data-ttu-id="bb4af-204">Questo comando inserisce l'assembly di avvio dell'hosting e altre dipendenze che non fanno parte del framework condiviso nell'archivio di runtime del profilo utente in:</span><span class="sxs-lookup"><span data-stu-id="bb4af-204">This command places the hosting startup assembly and other dependencies that aren't part of the shared framework in the user profile's runtime store at:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bb4af-205">Windows</span><span class="sxs-lookup"><span data-stu-id="bb4af-205">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="bb4af-206">macOS</span><span class="sxs-lookup"><span data-stu-id="bb4af-206">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="bb4af-207">Linux</span><span class="sxs-lookup"><span data-stu-id="bb4af-207">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="bb4af-208">Se si desidera inserire l'assembly e le dipendenze per l'utilizzo globale, aggiungere l'opzione `-o|--output` al comando `dotnet store` con il percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="bb4af-208">If you desire to place the assembly and dependencies for global use, add the `-o|--output` option to the `dotnet store` command with the following path:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bb4af-209">Windows</span><span class="sxs-lookup"><span data-stu-id="bb4af-209">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="bb4af-210">macOS</span><span class="sxs-lookup"><span data-stu-id="bb4af-210">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="bb4af-211">Linux</span><span class="sxs-lookup"><span data-stu-id="bb4af-211">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="bb4af-212">**Modificare e inserire il file di dipendenze dell'avvio dell'hosting**</span><span class="sxs-lookup"><span data-stu-id="bb4af-212">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="bb4af-213">Il percorso di runtime è specificato nel file *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="bb4af-213">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="bb4af-214">Per attivare il miglioramento, l'elemento `runtime` deve specificare il percorso dell'assembly di runtime del miglioramento.</span><span class="sxs-lookup"><span data-stu-id="bb4af-214">To activate the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="bb4af-215">Anteporre al percorso di `runtime` il prefisso `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span><span class="sxs-lookup"><span data-stu-id="bb4af-215">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="bb4af-216">Nel codice di esempio (progetto *StartupDiagnostics*) la modifica del file *\*.deps.json* viene eseguita da uno script [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="bb4af-216">In the sample code (*StartupDiagnostics* project), modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="bb4af-217">Lo script di PowerShell viene automaticamente attivato da una destinazione di compilazione nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="bb4af-217">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

<span data-ttu-id="bb4af-218">Il file *\*.deps.json* dell'implementazione deve trovarsi in un percorso accessibile.</span><span class="sxs-lookup"><span data-stu-id="bb4af-218">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="bb4af-219">Per l'uso per utente, inserire il file nella cartella *additonalDeps* delle impostazioni `.dotnet` del profilo utente:</span><span class="sxs-lookup"><span data-stu-id="bb4af-219">For per-user use, place the file in the *additonalDeps* folder of the user profile's `.dotnet` settings:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bb4af-220">Windows</span><span class="sxs-lookup"><span data-stu-id="bb4af-220">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="bb4af-221">macOS</span><span class="sxs-lookup"><span data-stu-id="bb4af-221">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="bb4af-222">Linux</span><span class="sxs-lookup"><span data-stu-id="bb4af-222">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="bb4af-223">Per l'uso globale, inserire il file nella cartella *additonalDeps* dell'installazione di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="bb4af-223">For global use, place the file in the *additonalDeps* folder of the .NET Core installation:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bb4af-224">Windows</span><span class="sxs-lookup"><span data-stu-id="bb4af-224">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="bb4af-225">macOS</span><span class="sxs-lookup"><span data-stu-id="bb4af-225">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="bb4af-226">Linux</span><span class="sxs-lookup"><span data-stu-id="bb4af-226">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="bb4af-227">La versione del framework condiviso riflette quella del runtime condiviso usato dall'app di destinazione.</span><span class="sxs-lookup"><span data-stu-id="bb4af-227">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="bb4af-228">Il runtime condiviso è indicato nel file *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="bb4af-228">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="bb4af-229">Nell'app di esempio (*HostingStartupApp*) il runtime condiviso è specificato nel file *HostingStartupApp.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="bb4af-229">In the sample app (*HostingStartupApp*), the shared runtime is specified in the *HostingStartupApp.runtimeconfig.json* file.</span></span>

<span data-ttu-id="bb4af-230">**Visualizzare il file di dipendenze dell'avvio dell'hosting**</span><span class="sxs-lookup"><span data-stu-id="bb4af-230">**List the hosting startup's dependencies file**</span></span>

<span data-ttu-id="bb4af-231">Il percorso del file *\*.deps.json* dell'implementazione è indicato nella variabile di ambiente `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-231">The location of the implementation's *\*.deps.json* file is listed in the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="bb4af-232">Se il file viene inserito nella cartella *.dotnet* del profilo utente, impostare il valore della variabile di ambiente su:</span><span class="sxs-lookup"><span data-stu-id="bb4af-232">If the file is placed in the user profile's *.dotnet* folder, set the environment variable's value to:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bb4af-233">Windows</span><span class="sxs-lookup"><span data-stu-id="bb4af-233">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="bb4af-234">macOS</span><span class="sxs-lookup"><span data-stu-id="bb4af-234">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="bb4af-235">Linux</span><span class="sxs-lookup"><span data-stu-id="bb4af-235">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

<span data-ttu-id="bb4af-236">Se il file è inserito nella cartella di installazione di .NET Core per l'uso globale, specificare il percorso completo al file:</span><span class="sxs-lookup"><span data-stu-id="bb4af-236">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bb4af-237">Windows</span><span class="sxs-lookup"><span data-stu-id="bb4af-237">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[<span data-ttu-id="bb4af-238">macOS</span><span class="sxs-lookup"><span data-stu-id="bb4af-238">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="bb4af-239">Linux</span><span class="sxs-lookup"><span data-stu-id="bb4af-239">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

<span data-ttu-id="bb4af-240">Per consentire all'app di esempio (*HostingStartupApp*) di trovare il file di dipendenze (*HostingStartupApp.runtimeconfig.json*), il file di dipendenze viene inserito nel profilo utente.</span><span class="sxs-lookup"><span data-stu-id="bb4af-240">For the sample app (*HostingStartupApp*) to find the dependencies file (*HostingStartupApp.runtimeconfig.json*), the dependencies file is placed in the user's profile.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bb4af-241">Windows</span><span class="sxs-lookup"><span data-stu-id="bb4af-241">Windows</span></span>](#tab/windows)

<span data-ttu-id="bb4af-242">Impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` sul valore seguente:</span><span class="sxs-lookup"><span data-stu-id="bb4af-242">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="bb4af-243">macOS</span><span class="sxs-lookup"><span data-stu-id="bb4af-243">macOS</span></span>](#tab/macos)

<span data-ttu-id="bb4af-244">Impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` sul valore seguente:</span><span class="sxs-lookup"><span data-stu-id="bb4af-244">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="bb4af-245">Linux</span><span class="sxs-lookup"><span data-stu-id="bb4af-245">Linux</span></span>](#tab/linux)

<span data-ttu-id="bb4af-246">Impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` sul valore seguente:</span><span class="sxs-lookup"><span data-stu-id="bb4af-246">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

<span data-ttu-id="bb4af-247">Per esempi che illustrano come impostare le variabili di ambiente per diversi sistemi operativi, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="bb4af-247">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="bb4af-248">**Distribuzione**</span><span class="sxs-lookup"><span data-stu-id="bb4af-248">**Deployment**</span></span>

<span data-ttu-id="bb4af-249">Per facilitare la distribuzione di un avvio dell'hosting in un ambiente con più computer, l'app di esempio crea una cartella *deplyment* nell'output pubblicato che contiene:</span><span class="sxs-lookup"><span data-stu-id="bb4af-249">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="bb4af-250">L'assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-250">The hosting startup assembly.</span></span>
* <span data-ttu-id="bb4af-251">Il file di dipendenze dell'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-251">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="bb4af-252">Uno script di PowerShell che crea o modifica `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` e `DOTNET_ADDITIONAL_DEPS` per supportare l'attivazione dell'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-252">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="bb4af-253">Eseguire lo script da un prompt dei comandi di PowerShell amministrativo del sistema di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="bb4af-253">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="bb4af-254">Pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="bb4af-254">NuGet package</span></span>

<span data-ttu-id="bb4af-255">È possibile includere un miglioramento di avvio dell'hosting in un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="bb4af-255">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="bb4af-256">Il pacchetto include un attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-256">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="bb4af-257">I tipi di avvio dell'hosting offerti dal pacchetto vengono resi disponibili all'app usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb4af-257">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="bb4af-258">Il file di progetto dell'app migliorata crea un riferimento al pacchetto per l'avvio dell'hosting nel file di progetto dell'app (un riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="bb4af-258">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="bb4af-259">Dopo aver inserito il riferimento in fase di compilazione, l'assembly di avvio dell'hosting e tutte le relative dipendenze vengono incorporate nel file delle dipendenze dell'app (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="bb4af-259">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="bb4af-260">Questo approccio si applica a un pacchetto di assembly di avvio dell'hosting pubblicato in [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="bb4af-260">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="bb4af-261">Il file di dipendenze dell'avvio dell'hosting viene reso disponibile all'app migliorata come descritto nella sezione [Archivio di runtime](#runtime-store) (senza un riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="bb4af-261">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="bb4af-262">Per altre informazioni su pacchetti NuGet e l'archivio di runtime, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb4af-262">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="bb4af-263">Come creare un pacchetto NuGet con strumenti multipiattaforma</span><span class="sxs-lookup"><span data-stu-id="bb4af-263">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="bb4af-264">Pubblicazione di pacchetti</span><span class="sxs-lookup"><span data-stu-id="bb4af-264">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="bb4af-265">Archivio pacchetti di runtime</span><span class="sxs-lookup"><span data-stu-id="bb4af-265">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="bb4af-266">Cartella bin del progetto</span><span class="sxs-lookup"><span data-stu-id="bb4af-266">Project bin folder</span></span>

<span data-ttu-id="bb4af-267">È possibile inserire un avvio dell'hosting tramite un assembly distribuito tramite *bin* nell'app migliorata.</span><span class="sxs-lookup"><span data-stu-id="bb4af-267">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="bb4af-268">I tipi di avvio dell'hosting offerti dall'assembly vengono resi disponibili all'app usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb4af-268">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="bb4af-269">Il file di progetto dell'app migliorata crea un riferimento all'assembly nell'avvio dell'hosting startup (riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="bb4af-269">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="bb4af-270">Dopo aver inserito il riferimento in fase di compilazione, l'assembly di avvio dell'hosting e tutte le relative dipendenze vengono incorporate nel file delle dipendenze dell'app (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="bb4af-270">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="bb4af-271">Questo approccio si applica quando lo scenario di distribuzione richiede lo spostamento dell'assembly della libreria di avvio dell'hosting compilata (file DLL) nel progetto utilizzato o in un percorso accessibile dal progetto utilizzato e viene creato un riferimento in fase di compilazione all'assembly dell'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-271">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="bb4af-272">Il file di dipendenze dell'avvio dell'hosting viene reso disponibile all'app migliorata come descritto nella sezione [Archivio di runtime](#runtime-store) (senza un riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="bb4af-272">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="bb4af-273">Codice di esempio</span><span class="sxs-lookup"><span data-stu-id="bb4af-273">Sample code</span></span>

<span data-ttu-id="bb4af-274">Il [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([come eseguire il download](xref:index#how-to-download-a-sample)) illustra gli scenari di implementazione dell'avvio dell'hosting:</span><span class="sxs-lookup"><span data-stu-id="bb4af-274">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="bb4af-275">Due assembly di avvio dell'hosting (librerie di classi) impostano ognuno una coppia chiave-valore di configurazione in memoria:</span><span class="sxs-lookup"><span data-stu-id="bb4af-275">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="bb4af-276">Pacchetto NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="bb4af-276">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="bb4af-277">Libreria di classi (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="bb4af-277">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="bb4af-278">L'avvio dell'hosting viene attivato da un assembly distribuito tramite archivio di runtime (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="bb4af-278">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="bb4af-279">All'avvio l'assembly aggiunge all'app due middleware che offrono informazioni di diagnostica su:</span><span class="sxs-lookup"><span data-stu-id="bb4af-279">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="bb4af-280">Servizi registrati</span><span class="sxs-lookup"><span data-stu-id="bb4af-280">Registered services</span></span>
  * <span data-ttu-id="bb4af-281">Indirizzo (schema, host, base del percorso, percorso, stringa di query)</span><span class="sxs-lookup"><span data-stu-id="bb4af-281">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="bb4af-282">Connessione (IP remoto, porta remota, IP locale, porta locale, certificato client)</span><span class="sxs-lookup"><span data-stu-id="bb4af-282">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="bb4af-283">Intestazioni della richiesta</span><span class="sxs-lookup"><span data-stu-id="bb4af-283">Request headers</span></span>
  * <span data-ttu-id="bb4af-284">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="bb4af-284">Environment variables</span></span>

<span data-ttu-id="bb4af-285">Per eseguire l'esempio:</span><span class="sxs-lookup"><span data-stu-id="bb4af-285">To run the sample:</span></span>

<span data-ttu-id="bb4af-286">**Attivazione da un pacchetto NuGet**</span><span class="sxs-lookup"><span data-stu-id="bb4af-286">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="bb4af-287">Compilare il pacchetto *HostingStartupPackage* con il comando [dotnet pack](/dotnet/core/tools/dotnet-pack).</span><span class="sxs-lookup"><span data-stu-id="bb4af-287">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="bb4af-288">Aggiungere il nome dell'assembly del pacchetto *HostingStartupPackage* alla variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-288">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="bb4af-289">Compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-289">Compile and run the app.</span></span> <span data-ttu-id="bb4af-290">L'app migliorata include un riferimento al pacchetto (riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="bb4af-290">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="bb4af-291">`<PropertyGroup>` nel file di progetto dell'app specifica l'output del progetto del pacchetto (*../HostingStartupPackage/bin/Debug*) come origine del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="bb4af-291">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="bb4af-292">Ciò consente all'app di usare il pacchetto senza caricarlo in [nuget.org](https://www.nuget.org/). Per altre informazioni, vedere le note nel file di progetto di HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="bb4af-292">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="bb4af-293">Si noti che i valori di chiave di configurazione del servizio di cui è stato eseguito il rendering tramite la pagina Indice corrispondono ai valori impostati dal metodo `ServiceKeyInjection.Configure` del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="bb4af-293">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="bb4af-294">Se si apportano modifiche al progetto *HostingStartupPackage* e lo si ricompila, cancellare le cache del pacchetto NuGet locale per assicurarsi che *HostingStartupApp* riceva il pacchetto aggiornato e non un pacchetto non aggiornato proveniente dalla cache locale.</span><span class="sxs-lookup"><span data-stu-id="bb4af-294">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="bb4af-295">Per cancellare le cache NuGet locali, eseguire il comando [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) seguente:</span><span class="sxs-lookup"><span data-stu-id="bb4af-295">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="bb4af-296">**Attivazione da una libreria di classi**</span><span class="sxs-lookup"><span data-stu-id="bb4af-296">**Activation from a class library**</span></span>

1. <span data-ttu-id="bb4af-297">Compilare la libreria di classi *HostingStartupLibrary* con il comando [dotnet build](/dotnet/core/tools/dotnet-build).</span><span class="sxs-lookup"><span data-stu-id="bb4af-297">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="bb4af-298">Aggiungere il nome dell'assembly della libreria di classi di *HostingStartupLibrary* alla variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-298">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="bb4af-299">Distribuire tramite *bin* l'assembly della libreria di classi all'app copiando il file *HostingStartupLibrary.dll* dall'output compilato della libreria di classi alla cartella *bin/Debug* dell'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-299">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="bb4af-300">Compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-300">Compile and run the app.</span></span> <span data-ttu-id="bb4af-301">`<ItemGroup>` nel file di progetto dell'app fa riferimento all'assembly della libreria di classi (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="bb4af-301">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="bb4af-302">Per altre informazioni, vedere le note nel file di progetto di HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="bb4af-302">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="bb4af-303">Si noti che i valori di chiave di configurazione del servizio di cui è stato eseguito il rendering tramite la pagina Indice corrispondono ai valori impostati dal metodo `ServiceKeyInjection.Configure` della libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="bb4af-303">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="bb4af-304">**Attivazione da un assembly distribuito tramite l'archivio di runtime**</span><span class="sxs-lookup"><span data-stu-id="bb4af-304">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="bb4af-305">Il progetto *StartupDiagnostics* usa [PowerShell](/powershell/scripting/powershell-scripting) per modificare il relativo file *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="bb4af-305">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="bb4af-306">PowerShell viene installato per impostazione predefinita in Windows a partire da Windows 7 SP1 e Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="bb4af-306">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="bb4af-307">Per ottenere PowerShell su altre piattaforme, vedere [Installazione di Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="bb4af-307">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="bb4af-308">Compilare il progetto *StartupDiagnostics*.</span><span class="sxs-lookup"><span data-stu-id="bb4af-308">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="bb4af-309">Dopo aver compilato il progetto, una destinazione di compilazione nel file di progetto esegue automaticamente le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb4af-309">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="bb4af-310">Attiva lo script di PowerShell per modificare il file *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="bb4af-310">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="bb4af-311">Sposta il file *StartupDiagnostics.deps.json* nella cartella *additionalDeps* del profilo utente.</span><span class="sxs-lookup"><span data-stu-id="bb4af-311">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="bb4af-312">Eseguire il comando `dotnet store` a un prompt dei comandi nella directory di avvio dell'hosting per archiviare l'assembly e le relative dipendenze nell'archivio di runtime del profilo utente:</span><span class="sxs-lookup"><span data-stu-id="bb4af-312">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="bb4af-313">Per Windows, il comando usa l'[identificatore di runtime (RID)](/dotnet/core/rid-catalog) `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-313">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="bb4af-314">Quando si specifica l'avvio dell'hosting per un runtime diverso, immettere il RID corretto.</span><span class="sxs-lookup"><span data-stu-id="bb4af-314">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="bb4af-315">Impostare le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="bb4af-315">Set the environment variables:</span></span>
   * <span data-ttu-id="bb4af-316">Aggiungere il nome dell'assembly di *StartupDiagnostics* alla variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-316">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="bb4af-317">In Windows impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` su `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span><span class="sxs-lookup"><span data-stu-id="bb4af-317">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="bb4af-318">In macOS/Linux impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` su `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, dove `<USER>` è il profilo utente che contiene l'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="bb4af-318">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="bb4af-319">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="bb4af-319">Run the sample app.</span></span>
1. <span data-ttu-id="bb4af-320">Richiedere l'endpoint `/services` per visualizzare i servizi registrati dell'app.</span><span class="sxs-lookup"><span data-stu-id="bb4af-320">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="bb4af-321">Richiedere l'endpoint `/diag` per visualizzare le informazioni di diagnostica.</span><span class="sxs-lookup"><span data-stu-id="bb4af-321">Request the `/diag` endpoint to see the diagnostic information.</span></span>
