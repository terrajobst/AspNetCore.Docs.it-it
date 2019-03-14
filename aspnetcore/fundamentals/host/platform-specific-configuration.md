---
title: Usare assembly di avvio dell'hosting in ASP.NET Core
author: guardrex
description: Informazioni su come migliorare un'app ASP.NET Core da un assembly esterno con un'implementazione IHostingStartup.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 03/10/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 6111ae77369608e828eebf6229b5702630bc63f8
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841462"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="eab75-103">Usare assembly di avvio dell'hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eab75-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="eab75-104">Di [Luke Latham](https://github.com/guardrex) e [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="eab75-104">By [Luke Latham](https://github.com/guardrex) and [Pavel Krymets](https://github.com/pakrym)</span></span>

<span data-ttu-id="eab75-105">Un'implementazione [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (avvio dell'hosting) consente l'aggiunta di miglioramenti a un'app all'avvio da un assembly esterno.</span><span class="sxs-lookup"><span data-stu-id="eab75-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="eab75-106">Una libreria esterna può ad esempio usare un'implementazione di avvio dell'hosting per offrire servizi o provider di configurazione aggiuntivi a un'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="eab75-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eab75-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="eab75-108">Attributo HostingStartup</span><span class="sxs-lookup"><span data-stu-id="eab75-108">HostingStartup attribute</span></span>

<span data-ttu-id="eab75-109">Un attributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) indica la presenza di un assembly di avvio dell'hosting da attivare in fase di runtime.</span><span class="sxs-lookup"><span data-stu-id="eab75-109">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="eab75-110">L'assembly di ingresso o l'assembly contenente la classe `Startup` viene analizzato automaticamente per la ricerca dell'attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="eab75-111">L'elenco di assembly in cui cercare gli attributi `HostingStartup` viene caricato in fase di runtime dalla configurazione in [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="eab75-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="eab75-112">L'elenco di assembly da escludere dalla ricerca viene caricato da [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="eab75-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="eab75-113">Per altre informazioni, vedere [Host Web: assembly di avvio dell'hosting](xref:fundamentals/host/web-host#hosting-startup-assemblies) e [Host Web: assembly di avvio dell'hosting da escludere](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="eab75-113">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="eab75-114">Nell'esempio seguente lo spazio dei nomi dell'assembly di avvio dell'hosting è `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="eab75-114">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="eab75-115">La classe che contiene il codice di avvio dell'hosting è `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="eab75-115">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="eab75-116">L'attributo `HostingStartup` si trova in genere nel file della classe di implementazione `IHostingStartup` dell'assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-116">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="eab75-117">Individuare gli assembly di avvio dell'hosting caricati</span><span class="sxs-lookup"><span data-stu-id="eab75-117">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="eab75-118">Per individuare gli assembly di avvio dell'hosting caricati, abilitare la registrazione e controllare i log dell'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-118">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="eab75-119">Gli errori che si verificano durante il caricamento degli assembly vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="eab75-119">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="eab75-120">Gli assembly di avvio dell'hosting caricati vengono registrati a livello di debug e tutti gli errori vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="eab75-120">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="eab75-121">Disabilitare il caricamento automatico di assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="eab75-121">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="eab75-122">Per disabilitare il caricamento automatico degli assembly di avvio dell'hosting, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="eab75-122">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="eab75-123">Per evitare il caricamento di tutti gli assembly di avvio dell'hosting, impostare uno degli elementi seguenti su `true` o `1`:</span><span class="sxs-lookup"><span data-stu-id="eab75-123">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="eab75-124">Impostazione di configurazione host per [impedire l'avvio dell'hosting](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="eab75-124">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="eab75-125">La variabile di ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="eab75-125">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="eab75-126">Per impedire il caricamento di specifici assembly di avvio dell'hosting, impostare uno degli elementi seguenti su una stringa delimitata da punti e virgola di assembly di avvio dell'hosting da escludere all'avvio:</span><span class="sxs-lookup"><span data-stu-id="eab75-126">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="eab75-127">Impostazione di configurazione host per [assembly di avvio dell'hosting da escludere](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="eab75-127">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="eab75-128">La variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="eab75-128">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="eab75-129">Se l'impostazione di configurazione host e la variabile di ambiente sono entrambe impostate, l'impostazione host controlla il comportamento.</span><span class="sxs-lookup"><span data-stu-id="eab75-129">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="eab75-130">La disabilitazione degli assembly di avvio dell'hosting tramite l'impostazione host o la variabile di ambiente ne determina la disabilitazione globale e la possibile disabilitazione di diverse caratteristiche di un'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-130">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="eab75-131">Progetto</span><span class="sxs-lookup"><span data-stu-id="eab75-131">Project</span></span>

<span data-ttu-id="eab75-132">Creare l'avvio dell'hosting con uno dei tipi di progetto seguenti:</span><span class="sxs-lookup"><span data-stu-id="eab75-132">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="eab75-133">Libreria di classi</span><span class="sxs-lookup"><span data-stu-id="eab75-133">Class library</span></span>](#class-library)
* [<span data-ttu-id="eab75-134">App console senza un punto di ingresso</span><span class="sxs-lookup"><span data-stu-id="eab75-134">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="eab75-135">Libreria di classi</span><span class="sxs-lookup"><span data-stu-id="eab75-135">Class library</span></span>

<span data-ttu-id="eab75-136">È possibile includere un miglioramento di avvio dell'hosting in una libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="eab75-136">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="eab75-137">La libreria contiene un attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-137">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="eab75-138">Il [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) include un'app Razor Pages, *HostingStartupApp*, e una libreria di classi, *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="eab75-138">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="eab75-139">La libreria di classi:</span><span class="sxs-lookup"><span data-stu-id="eab75-139">The class library:</span></span>

* <span data-ttu-id="eab75-140">Contiene una classe di avvio dell'hosting, `ServiceKeyInjection`, che implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-140">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="eab75-141">`ServiceKeyInjection` aggiunge una coppia di stringhe di servizio alla configurazione dell'app tramite il provider di configurazione in memoria ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="eab75-141">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="eab75-142">Include un attributo `HostingStartup` che identifica lo spazio dei nomi e la classe di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-142">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="eab75-143">Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) della classe `ServiceKeyInjection` usa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere miglioramenti a un'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-143">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span>

<span data-ttu-id="eab75-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="eab75-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="eab75-145">La pagina Indice dell'app legge ed esegue il rendering dei valori di configurazione per le due chiavi impostate dall'assembly di avvio dell'hosting della libreria di classi:</span><span class="sxs-lookup"><span data-stu-id="eab75-145">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="eab75-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="eab75-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="eab75-147">Il [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) include anche un progetto di pacchetto NuGet che offre un avvio dell'hosting separato, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="eab75-147">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="eab75-148">Il pacchetto ha le stesse caratteristiche della libreria di classi descritta in precedenza.</span><span class="sxs-lookup"><span data-stu-id="eab75-148">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="eab75-149">Il pacchetto:</span><span class="sxs-lookup"><span data-stu-id="eab75-149">The package:</span></span>

* <span data-ttu-id="eab75-150">Contiene una classe di avvio dell'hosting, `ServiceKeyInjection`, che implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-150">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="eab75-151">`ServiceKeyInjection` aggiunge una coppia di stringhe di servizio alla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-151">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="eab75-152">Include un attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-152">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="eab75-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="eab75-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="eab75-154">La pagina Indice dell'app legge ed esegue il rendering dei valori di configurazione per le due chiavi impostate dall'assembly di avvio dell'hosting del pacchetto:</span><span class="sxs-lookup"><span data-stu-id="eab75-154">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="eab75-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="eab75-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="eab75-156">App console senza un punto di ingresso</span><span class="sxs-lookup"><span data-stu-id="eab75-156">Console app without an entry point</span></span>

<span data-ttu-id="eab75-157">*Questo approccio è disponibile solo per le app .NET Core, non .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="eab75-157">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="eab75-158">Un miglioramento di avvio dell'hosting dinamico che non richiede un riferimento in fase di compilazione per l'attivazione può essere fornito in un'app console senza un punto di ingresso che contiene un attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-158">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="eab75-159">La pubblicazione dell'app console produce un assembly di avvio dell'hosting che può essere utilizzato dall'archivio di runtime.</span><span class="sxs-lookup"><span data-stu-id="eab75-159">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="eab75-160">In questo processo viene usata un'app console senza un punto di ingresso perché:</span><span class="sxs-lookup"><span data-stu-id="eab75-160">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="eab75-161">È necessario un file di dipendenze per utilizzare l'avvio dell'hosting nell'assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-161">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="eab75-162">Un file di dipendenze è una risorsa di app eseguibile prodotta dalla pubblicazione di un'app, non una libreria.</span><span class="sxs-lookup"><span data-stu-id="eab75-162">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="eab75-163">Una libreria non può essere aggiunta direttamente all'[archivio dei pacchetti di runtime](/dotnet/core/deploying/runtime-store) che richiede un progetto eseguibile indirizzato al runtime condiviso.</span><span class="sxs-lookup"><span data-stu-id="eab75-163">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="eab75-164">Durante la creazione di un avvio dell'hosting dinamico:</span><span class="sxs-lookup"><span data-stu-id="eab75-164">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="eab75-165">Viene creato un assembly di avvio dell'hosting dall'app console senza un punto di ingresso che:</span><span class="sxs-lookup"><span data-stu-id="eab75-165">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="eab75-166">Include una classe che contiene l'implementazione `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-166">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="eab75-167">Include un attributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) per identificare la classe di implementazione `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-167">Includes a [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="eab75-168">L'app console viene pubblicata per ottenere le dipendenze dell'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-168">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="eab75-169">Una conseguenza della pubblicazione dell'app console è che le dipendenze non usate vengono eliminate dal file di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="eab75-169">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="eab75-170">Il file delle dipendenze viene modificato per impostare il percorso di runtime dell'assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-170">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="eab75-171">L'assembly di avvio dell'hosting e il relativo file di dipendenze vengono posizionati nell'archivio dei pacchetti di runtime.</span><span class="sxs-lookup"><span data-stu-id="eab75-171">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="eab75-172">Per individuare l'assembly di avvio dell'hosting e il relativo file di dipendenze, questi elementi sono elencati in una coppia di variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="eab75-172">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="eab75-173">L'app console fa riferimento al pacchetto [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="eab75-173">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="eab75-174">Un attributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica una classe come un'implementazione di `IHostingStartup` per il caricamento e l'esecuzione al momento della compilazione di [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="eab75-174">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="eab75-175">Nell'esempio seguente, lo spazio dei nomi è `StartupEnhancement` e la classe è `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="eab75-175">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="eab75-176">Una classe implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-176">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="eab75-177">Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) della classe usa un'interfaccia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere miglioramenti a un'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-177">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="eab75-178">Nell'assembly di avvio dell'hosting `IHostingStartup.Configure` viene chiamato dal runtime prima di `Startup.Configure` nel codice utente, il che consente al codice utente di sovrascrivere qualsiasi configurazione fornita dall'assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-178">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="eab75-179">Quando si compila un progetto `IHostingStartup`, il file delle dipendenze (*\*.deps.json*) imposta il percorso di `runtime` dell'assembly sulla cartella *bin*:</span><span class="sxs-lookup"><span data-stu-id="eab75-179">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="eab75-180">Viene visualizzata solo una parte del file.</span><span class="sxs-lookup"><span data-stu-id="eab75-180">Only part of the file is shown.</span></span> <span data-ttu-id="eab75-181">Il nome dell'assembly nell'esempio è `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="eab75-181">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="eab75-182">Configurazione fornita dall'avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="eab75-182">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="eab75-183">Esistono due approcci per la gestione di configurazione, a seconda che si voglia dare la precedenza alla configurazione d avvio dell'hosting o alla configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="eab75-183">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="eab75-184">Fornire la configurazione all'app usando <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> per caricare la configurazione dopo l'esecuzione dei delegati <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> dell'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-184">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="eab75-185">La configurazione di avvio dell'hosting ha la priorità rispetto alla configurazione dell'app con questo approccio.</span><span class="sxs-lookup"><span data-stu-id="eab75-185">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="eab75-186">Fornire la configurazione all'app usando <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> per caricare la configurazione prima dell'esecuzione dei delegati <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> dell'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-186">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="eab75-187">I valori di configurazione dell'app hanno la priorità rispetto a quelli forniti dall'avvio dell'hosting usando questo approccio.</span><span class="sxs-lookup"><span data-stu-id="eab75-187">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="eab75-188">Specificare l'assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="eab75-188">Specify the hosting startup assembly</span></span>

<span data-ttu-id="eab75-189">Per un avvio dell'hosting fornito da una libreria di classi o da un'app console, specificare il nome dell'assembly di avvio dell'hosting nella variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="eab75-189">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="eab75-190">La variabile di ambiente è un elenco di assembly delimitato da punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="eab75-190">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="eab75-191">L'attributo `HostingStartup` viene cercato solo negli assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-191">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="eab75-192">Per l'app di esempio *HostingStartupApp*, per individuare gli avvii dell'hosting descritti in precedenza, la variabile di ambiente viene impostata sul valore seguente:</span><span class="sxs-lookup"><span data-stu-id="eab75-192">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="eab75-193">È possibile impostare l'assembly di avvio dell'hosting anche tramite l'impostazione di configurazione host [Assembly di avvio dell'hosting](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="eab75-193">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="eab75-194">Quando sono presenti più assembly di avvio dell'hosting, i relativi metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) vengono eseguiti nell'ordine in cui sono elencati gli assembly.</span><span class="sxs-lookup"><span data-stu-id="eab75-194">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="eab75-195">Attivazione</span><span class="sxs-lookup"><span data-stu-id="eab75-195">Activation</span></span>

<span data-ttu-id="eab75-196">Le opzioni di attivazione dell'avvio dell'hosting sono:</span><span class="sxs-lookup"><span data-stu-id="eab75-196">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="eab75-197">[Archivio di runtime](#runtime-store) &ndash; L'attivazione non richiede un riferimento in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="eab75-197">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="eab75-198">L'app di esempio inserisce l'assembly di avvio dell'hosting e i file di dipendenze in una cartella *deployment* per facilitare la distribuzione dell'avvio dell'hosting in un ambiente con più computer.</span><span class="sxs-lookup"><span data-stu-id="eab75-198">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="eab75-199">La cartella *deployment* include anche uno script PowerShell che crea o modifica le variabili di ambiente nel sistema di distribuzione per abilitare l'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-199">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="eab75-200">Riferimento in fase di compilazione richiesto per l'attivazione</span><span class="sxs-lookup"><span data-stu-id="eab75-200">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="eab75-201">Pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="eab75-201">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="eab75-202">Cartella bin del progetto</span><span class="sxs-lookup"><span data-stu-id="eab75-202">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="eab75-203">Archivio di runtime</span><span class="sxs-lookup"><span data-stu-id="eab75-203">Runtime store</span></span>

<span data-ttu-id="eab75-204">L'implementazione dell'avvio dell'hosting viene inserita nell'[archivio di runtime](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="eab75-204">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="eab75-205">L'app migliorata non richiede un riferimento in fase di compilazione all'assembly.</span><span class="sxs-lookup"><span data-stu-id="eab75-205">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="eab75-206">Dopo la compilazione dell'avvio dell'hosting, viene generato un archivio di runtime usando il file di progetto del manifesto e il comando [dotnet store](/dotnet/core/tools/dotnet-store).</span><span class="sxs-lookup"><span data-stu-id="eab75-206">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="eab75-207">Nell'app di esempio (progetto *RuntimeStore*) viene usato il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="eab75-207">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

``` console
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="eab75-208">Per consentire al runtime di individuare l'archivio di runtime, il percorso dell'archivio di runtime viene aggiunto alla variabile di ambiente `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="eab75-208">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="eab75-209">**Modificare e inserire il file di dipendenze dell'avvio dell'hosting**</span><span class="sxs-lookup"><span data-stu-id="eab75-209">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="eab75-210">Per attivare il miglioramento senza un riferimento al pacchetto per il miglioramento, specificare le dipendenze aggiuntive per il runtime con `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="eab75-210">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="eab75-211">`additionalDeps` consente di:</span><span class="sxs-lookup"><span data-stu-id="eab75-211">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="eab75-212">Estendere il grafo della libreria dell'app fornendo un set di file *\*.deps.json* aggiuntivi da unire al file *\*.deps.json* proprio dell'app all'avvio.</span><span class="sxs-lookup"><span data-stu-id="eab75-212">Extend the app's library graph by providing a set of additional *\*.deps.json* files to merge with the app's own *\*.deps.json* file on startup.</span></span>
* <span data-ttu-id="eab75-213">Rendere l'assembly di avvio dell'hosting individuabile e caricabile.</span><span class="sxs-lookup"><span data-stu-id="eab75-213">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="eab75-214">L'approccio consigliato per la generazione del file delle dipendenze aggiuntive è:</span><span class="sxs-lookup"><span data-stu-id="eab75-214">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="eab75-215">Eseguire `dotnet publish` sul file manifesto dell'archivio di runtime indicato nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="eab75-215">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="eab75-216">Rimuovere il riferimento al manifesto dalle librerie e dalla sezione `runtime` del file *\*deps.json* risultante.</span><span class="sxs-lookup"><span data-stu-id="eab75-216">Remove the manifest reference from libraries and the `runtime` section of the resulting *\*deps.json* file.</span></span>

<span data-ttu-id="eab75-217">Nel progetto di esempio la proprietà `store.manifest/1.0.0` viene rimossa da `targets` e dalla sezione `libraries`:</span><span class="sxs-lookup"><span data-stu-id="eab75-217">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="eab75-218">Posizionare il file *\*.deps.json* nel percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="eab75-218">Place the *\*.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="eab75-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Percorso aggiunto alla variabile di ambiente `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="eab75-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="eab75-220">`{SHARED FRAMEWORK NAME}` &ndash; Framework condiviso necessario per questo file di dipendenze aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="eab75-220">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="eab75-221">`{SHARED FRAMEWORK VERSION}` &ndash; Versione minima del framework condiviso.</span><span class="sxs-lookup"><span data-stu-id="eab75-221">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="eab75-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; Nome dell'assembly del miglioramento.</span><span class="sxs-lookup"><span data-stu-id="eab75-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="eab75-223">Nell'app di esempio (progetto *RuntimeStore*), il file di dipendenze aggiuntive viene posizionato nel percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="eab75-223">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="eab75-224">Affinché il runtime possa individuare il percorso dell'archivio di runtime, il percorso del file di dipendenze aggiuntive viene aggiunto alla variabile di ambiente `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="eab75-224">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="eab75-225">Nell'app di esempio (progetto *RuntimeStore*), la creazione dell'archivio di runtime e la generazione del file di dipendenze aggiuntive vengono eseguite tramite uno script di [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="eab75-225">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="eab75-226">Per esempi che illustrano come impostare le variabili di ambiente per diversi sistemi operativi, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="eab75-226">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="eab75-227">**Distribuzione**</span><span class="sxs-lookup"><span data-stu-id="eab75-227">**Deployment**</span></span>

<span data-ttu-id="eab75-228">Per facilitare la distribuzione di un avvio dell'hosting in un ambiente con più computer, l'app di esempio crea una cartella *deplyment* nell'output pubblicato che contiene:</span><span class="sxs-lookup"><span data-stu-id="eab75-228">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="eab75-229">L'archivio di runtime dell'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-229">The hosting startup runtime store.</span></span>
* <span data-ttu-id="eab75-230">Il file di dipendenze dell'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-230">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="eab75-231">Uno script di PowerShell che crea o modifica `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE` e `DOTNET_ADDITIONAL_DEPS` per supportare l'attivazione dell'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-231">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="eab75-232">Eseguire lo script da un prompt dei comandi di PowerShell amministrativo del sistema di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="eab75-232">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="eab75-233">Pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="eab75-233">NuGet package</span></span>

<span data-ttu-id="eab75-234">È possibile includere un miglioramento di avvio dell'hosting in un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="eab75-234">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="eab75-235">Il pacchetto include un attributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="eab75-235">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="eab75-236">I tipi di avvio dell'hosting offerti dal pacchetto vengono resi disponibili all'app usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="eab75-236">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="eab75-237">Il file di progetto dell'app migliorata crea un riferimento al pacchetto per l'avvio dell'hosting nel file di progetto dell'app (un riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="eab75-237">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="eab75-238">Dopo aver inserito il riferimento in fase di compilazione, l'assembly di avvio dell'hosting e tutte le relative dipendenze vengono incorporate nel file delle dipendenze dell'app (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="eab75-238">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="eab75-239">Questo approccio si applica a un pacchetto di assembly di avvio dell'hosting pubblicato in [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="eab75-239">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="eab75-240">Il file di dipendenze dell'avvio dell'hosting viene reso disponibile all'app migliorata come descritto nella sezione [Archivio di runtime](#runtime-store) (senza un riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="eab75-240">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="eab75-241">Per altre informazioni su pacchetti NuGet e l'archivio di runtime, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="eab75-241">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="eab75-242">Come creare un pacchetto NuGet con strumenti multipiattaforma</span><span class="sxs-lookup"><span data-stu-id="eab75-242">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="eab75-243">Pubblicazione di pacchetti</span><span class="sxs-lookup"><span data-stu-id="eab75-243">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="eab75-244">Archivio pacchetti di runtime</span><span class="sxs-lookup"><span data-stu-id="eab75-244">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="eab75-245">Cartella bin del progetto</span><span class="sxs-lookup"><span data-stu-id="eab75-245">Project bin folder</span></span>

<span data-ttu-id="eab75-246">È possibile inserire un avvio dell'hosting tramite un assembly distribuito tramite *bin* nell'app migliorata.</span><span class="sxs-lookup"><span data-stu-id="eab75-246">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="eab75-247">I tipi di avvio dell'hosting offerti dall'assembly vengono resi disponibili all'app usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="eab75-247">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="eab75-248">Il file di progetto dell'app migliorata crea un riferimento all'assembly nell'avvio dell'hosting startup (riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="eab75-248">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="eab75-249">Dopo aver inserito il riferimento in fase di compilazione, l'assembly di avvio dell'hosting e tutte le relative dipendenze vengono incorporate nel file delle dipendenze dell'app (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="eab75-249">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="eab75-250">Questo approccio si applica quando lo scenario di distribuzione richiede lo spostamento dell'assembly della libreria di avvio dell'hosting compilata (file DLL) nel progetto utilizzato o in un percorso accessibile dal progetto utilizzato e viene creato un riferimento in fase di compilazione all'assembly dell'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-250">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="eab75-251">Il file di dipendenze dell'avvio dell'hosting viene reso disponibile all'app migliorata come descritto nella sezione [Archivio di runtime](#runtime-store) (senza un riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="eab75-251">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="eab75-252">Codice di esempio</span><span class="sxs-lookup"><span data-stu-id="eab75-252">Sample code</span></span>

<span data-ttu-id="eab75-253">Il [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([come eseguire il download](xref:index#how-to-download-a-sample)) illustra gli scenari di implementazione dell'avvio dell'hosting:</span><span class="sxs-lookup"><span data-stu-id="eab75-253">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="eab75-254">Due assembly di avvio dell'hosting (librerie di classi) impostano ognuno una coppia chiave-valore di configurazione in memoria:</span><span class="sxs-lookup"><span data-stu-id="eab75-254">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="eab75-255">Pacchetto NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="eab75-255">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="eab75-256">Libreria di classi (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="eab75-256">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="eab75-257">L'avvio dell'hosting viene attivato da un assembly distribuito tramite archivio di runtime (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="eab75-257">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="eab75-258">All'avvio l'assembly aggiunge all'app due middleware che offrono informazioni di diagnostica su:</span><span class="sxs-lookup"><span data-stu-id="eab75-258">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="eab75-259">Servizi registrati</span><span class="sxs-lookup"><span data-stu-id="eab75-259">Registered services</span></span>
  * <span data-ttu-id="eab75-260">Indirizzo (schema, host, base del percorso, percorso, stringa di query)</span><span class="sxs-lookup"><span data-stu-id="eab75-260">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="eab75-261">Connessione (IP remoto, porta remota, IP locale, porta locale, certificato client)</span><span class="sxs-lookup"><span data-stu-id="eab75-261">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="eab75-262">Intestazioni della richiesta</span><span class="sxs-lookup"><span data-stu-id="eab75-262">Request headers</span></span>
  * <span data-ttu-id="eab75-263">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="eab75-263">Environment variables</span></span>

<span data-ttu-id="eab75-264">Per eseguire l'esempio:</span><span class="sxs-lookup"><span data-stu-id="eab75-264">To run the sample:</span></span>

<span data-ttu-id="eab75-265">**Attivazione da un pacchetto NuGet**</span><span class="sxs-lookup"><span data-stu-id="eab75-265">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="eab75-266">Compilare il pacchetto *HostingStartupPackage* con il comando [dotnet pack](/dotnet/core/tools/dotnet-pack).</span><span class="sxs-lookup"><span data-stu-id="eab75-266">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="eab75-267">Aggiungere il nome dell'assembly del pacchetto *HostingStartupPackage* alla variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="eab75-267">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="eab75-268">Compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-268">Compile and run the app.</span></span> <span data-ttu-id="eab75-269">L'app migliorata include un riferimento al pacchetto (riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="eab75-269">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="eab75-270">`<PropertyGroup>` nel file di progetto dell'app specifica l'output del progetto del pacchetto (*../HostingStartupPackage/bin/Debug*) come origine del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="eab75-270">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="eab75-271">Ciò consente all'app di usare il pacchetto senza caricarlo in [nuget.org](https://www.nuget.org/). Per altre informazioni, vedere le note nel file di progetto di HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="eab75-271">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="eab75-272">Si noti che i valori di chiave di configurazione del servizio di cui è stato eseguito il rendering tramite la pagina Indice corrispondono ai valori impostati dal metodo `ServiceKeyInjection.Configure` del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="eab75-272">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="eab75-273">Se si apportano modifiche al progetto *HostingStartupPackage* e lo si ricompila, cancellare le cache del pacchetto NuGet locale per assicurarsi che *HostingStartupApp* riceva il pacchetto aggiornato e non un pacchetto non aggiornato proveniente dalla cache locale.</span><span class="sxs-lookup"><span data-stu-id="eab75-273">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="eab75-274">Per cancellare le cache NuGet locali, eseguire il comando [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) seguente:</span><span class="sxs-lookup"><span data-stu-id="eab75-274">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="eab75-275">**Attivazione da una libreria di classi**</span><span class="sxs-lookup"><span data-stu-id="eab75-275">**Activation from a class library**</span></span>

1. <span data-ttu-id="eab75-276">Compilare la libreria di classi *HostingStartupLibrary* con il comando [dotnet build](/dotnet/core/tools/dotnet-build).</span><span class="sxs-lookup"><span data-stu-id="eab75-276">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="eab75-277">Aggiungere il nome dell'assembly della libreria di classi di *HostingStartupLibrary* alla variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="eab75-277">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="eab75-278">Distribuire tramite *bin* l'assembly della libreria di classi all'app copiando il file *HostingStartupLibrary.dll* dall'output compilato della libreria di classi alla cartella *bin/Debug* dell'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-278">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="eab75-279">Compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-279">Compile and run the app.</span></span> <span data-ttu-id="eab75-280">`<ItemGroup>` nel file di progetto dell'app fa riferimento all'assembly della libreria di classi (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (riferimento in fase di compilazione).</span><span class="sxs-lookup"><span data-stu-id="eab75-280">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="eab75-281">Per altre informazioni, vedere le note nel file di progetto di HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="eab75-281">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="eab75-282">Si noti che i valori di chiave di configurazione del servizio di cui è stato eseguito il rendering tramite la pagina Indice corrispondono ai valori impostati dal metodo `ServiceKeyInjection.Configure` della libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="eab75-282">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="eab75-283">**Attivazione da un assembly distribuito tramite l'archivio di runtime**</span><span class="sxs-lookup"><span data-stu-id="eab75-283">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="eab75-284">Il progetto *StartupDiagnostics* usa [PowerShell](/powershell/scripting/powershell-scripting) per modificare il relativo file *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="eab75-284">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="eab75-285">PowerShell viene installato per impostazione predefinita in Windows a partire da Windows 7 SP1 e Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="eab75-285">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="eab75-286">Per ottenere PowerShell su altre piattaforme, vedere [Installazione di Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="eab75-286">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="eab75-287">Compilare il progetto *StartupDiagnostics*.</span><span class="sxs-lookup"><span data-stu-id="eab75-287">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="eab75-288">Dopo aver compilato il progetto, una destinazione di compilazione nel file di progetto esegue automaticamente le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="eab75-288">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="eab75-289">Attiva lo script di PowerShell per modificare il file *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="eab75-289">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="eab75-290">Sposta il file *StartupDiagnostics.deps.json* nella cartella *additionalDeps* del profilo utente.</span><span class="sxs-lookup"><span data-stu-id="eab75-290">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="eab75-291">Eseguire il comando `dotnet store` a un prompt dei comandi nella directory di avvio dell'hosting per archiviare l'assembly e le relative dipendenze nell'archivio di runtime del profilo utente:</span><span class="sxs-lookup"><span data-stu-id="eab75-291">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="eab75-292">Per Windows, il comando usa l'[identificatore di runtime (RID)](/dotnet/core/rid-catalog) `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="eab75-292">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="eab75-293">Quando si specifica l'avvio dell'hosting per un runtime diverso, immettere il RID corretto.</span><span class="sxs-lookup"><span data-stu-id="eab75-293">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="eab75-294">Impostare le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="eab75-294">Set the environment variables:</span></span>
   * <span data-ttu-id="eab75-295">Aggiungere il nome dell'assembly di *StartupDiagnostics* alla variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="eab75-295">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="eab75-296">In Windows impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` su `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span><span class="sxs-lookup"><span data-stu-id="eab75-296">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="eab75-297">In macOS/Linux impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` su `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, dove `<USER>` è il profilo utente che contiene l'avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="eab75-297">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="eab75-298">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="eab75-298">Run the sample app.</span></span>
1. <span data-ttu-id="eab75-299">Richiedere l'endpoint `/services` per visualizzare i servizi registrati dell'app.</span><span class="sxs-lookup"><span data-stu-id="eab75-299">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="eab75-300">Richiedere l'endpoint `/diag` per visualizzare le informazioni di diagnostica.</span><span class="sxs-lookup"><span data-stu-id="eab75-300">Request the `/diag` endpoint to see the diagnostic information.</span></span>
