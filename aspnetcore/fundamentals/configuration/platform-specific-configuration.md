---
title: Migliorare un'app da un assembly esterno in ASP.NET Core con IHostingStartup
author: guardrex
description: Informazioni su come migliorare un'app ASP.NET Core da un assembly esterno con un'implementazione IHostingStartup.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 793169b491596cd7326d747a3f19d7fdaf7e2b65
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="7b77a-103">Migliorare un'app da un assembly esterno in ASP.NET Core con IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="7b77a-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="7b77a-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7b77a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7b77a-105">Un' implementazione [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="7b77a-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="7b77a-106">Una libreria di strumenti esterna può ad esempio usare un'implementazione `IHostingStartup` per offrire servizi o provider di configurazione aggiuntivi a un'app.</span><span class="sxs-lookup"><span data-stu-id="7b77a-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="7b77a-107">`IHostingStartup` *è disponibile in ASP.NET Core 2.0 e versioni successive.*</span><span class="sxs-lookup"><span data-stu-id="7b77a-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="7b77a-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7b77a-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="7b77a-109">Individuare gli assembly di avvio dell'hosting caricati</span><span class="sxs-lookup"><span data-stu-id="7b77a-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="7b77a-110">Per individuare gli assembly di avvio dell'hosting caricati dall'app o dalle librerie, abilitare la registrazione e controllare i registri applicazioni.</span><span class="sxs-lookup"><span data-stu-id="7b77a-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="7b77a-111">Gli errori che si verificano durante il caricamento degli assembly vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="7b77a-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="7b77a-112">Gli assembly di avvio dell'hosting caricati vengono registrati a livello di debug e tutti gli errori vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="7b77a-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="7b77a-113">L'app di esempio legge [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) in una matrice `string` e visualizza il risultato nella pagina di indice dell'app:</span><span class="sxs-lookup"><span data-stu-id="7b77a-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="7b77a-114">Disabilitare il caricamento automatico di assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="7b77a-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="7b77a-115">Per disabilitare il caricamento automatico di assembly di avvio dell'hosting sono disponibili due modi:</span><span class="sxs-lookup"><span data-stu-id="7b77a-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="7b77a-116">Usare l'impostazione di configurazione host per [Impedire l'avvio dell'hosting](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="7b77a-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="7b77a-117">Impostare la variabile di ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="7b77a-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="7b77a-118">Quando l'impostazione host o la variabile di ambiente è impostata su `true` o `1`, gli assembly di avvio dell'hosting non vengono caricati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="7b77a-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="7b77a-119">Se entrambe sono impostate, il comportamento viene controllato dall'impostazione host.</span><span class="sxs-lookup"><span data-stu-id="7b77a-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="7b77a-120">La disabilitazione degli assembly di avvio dell'hosting tramite l'impostazione host o la variabile di ambiente ne determina la disabilitazione globale e la possibile disabilitazione di diverse caratteristiche di un'app.</span><span class="sxs-lookup"><span data-stu-id="7b77a-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="7b77a-121">Attualmente non è possibile disabilitare in modo selettivo un assembly di avvio dell'hosting aggiunto da una libreria, a meno che questa non offra una propria opzione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7b77a-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="7b77a-122">Una versione futura offrirà la possibilità di disabilitare in modo selettivo gli assembly di avvio dell'hosting (vedere il [problema aspnet/Hosting n. 1243 in GitHub](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="7b77a-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="7b77a-123">Implementare IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="7b77a-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="7b77a-124">Creare l'assembly</span><span class="sxs-lookup"><span data-stu-id="7b77a-124">Create the assembly</span></span>

<span data-ttu-id="7b77a-125">Viene distribuito un miglioramento `IHostingStartup` come assembly basato su un'applicazione console senza un punto di ingresso.</span><span class="sxs-lookup"><span data-stu-id="7b77a-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="7b77a-126">L'assembly fa riferimento al pacchetto [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="7b77a-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="7b77a-127">Un attributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica una classe come un'implementazione di `IHostingStartup` per il caricamento e l'esecuzione al momento della compilazione di [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="7b77a-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="7b77a-128">Nell'esempio seguente, lo spazio dei nomi è `StartupEnhancement` e la classe è `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="7b77a-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="7b77a-129">Una classe implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="7b77a-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="7b77a-130">Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) della classe usa un'interfaccia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere miglioramenti a un'app:</span><span class="sxs-lookup"><span data-stu-id="7b77a-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="7b77a-131">Quando si compila un progetto `IHostingStartup`, il file delle dipendenze (*\*.deps.json*) imposta il percorso di `runtime` dell'assembly sulla cartella *bin*:</span><span class="sxs-lookup"><span data-stu-id="7b77a-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="7b77a-132">Viene visualizzata solo una parte del file.</span><span class="sxs-lookup"><span data-stu-id="7b77a-132">Only part of the file is shown.</span></span> <span data-ttu-id="7b77a-133">Il nome dell'assembly nell'esempio è `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="7b77a-133">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="7b77a-134">Aggiornare il file delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="7b77a-134">Update the dependencies file</span></span>

<span data-ttu-id="7b77a-135">Il percorso di runtime è specificato nel file *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="7b77a-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="7b77a-136">Per attivare il miglioramento, l'elemento `runtime` deve specificare il percorso dell'assembly di runtime del miglioramento.</span><span class="sxs-lookup"><span data-stu-id="7b77a-136">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="7b77a-137">Anteporre al percorso di `runtime` il prefisso `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="7b77a-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="7b77a-138">Nell'app di esempio la modifica del file *\*.deps.json* viene eseguita da uno script di [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="7b77a-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="7b77a-139">Lo script di PowerShell viene automaticamente attivato da una destinazione di compilazione nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="7b77a-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="7b77a-140">Attivazione del miglioramento</span><span class="sxs-lookup"><span data-stu-id="7b77a-140">Enhancement activation</span></span>

<span data-ttu-id="7b77a-141">**Inserire il file di assembly**</span><span class="sxs-lookup"><span data-stu-id="7b77a-141">**Place the assembly file**</span></span>

<span data-ttu-id="7b77a-142">Il file di assembly dell'implementazione `IHostingStartup` deve essere distribuito alla cartella *bin* nell'app o inserito nell'[archivio di runtime](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="7b77a-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="7b77a-143">Per l'uso per utente, inserire l'assembly nell'archivio di runtime del profilo utente in:</span><span class="sxs-lookup"><span data-stu-id="7b77a-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="7b77a-144">Per l'uso globale, inserire l'assembly nell'archivio di runtime dell'installazione di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="7b77a-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="7b77a-145">Quando si distribuisce l'assembly all'archivio di runtime è possibile che venga distribuito anche il file di simboli che tuttavia non è necessario per il funzionamento del miglioramento.</span><span class="sxs-lookup"><span data-stu-id="7b77a-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="7b77a-146">**Inserire il file delle dipendenze**</span><span class="sxs-lookup"><span data-stu-id="7b77a-146">**Place the dependencies file**</span></span>

<span data-ttu-id="7b77a-147">Il file *\*.deps.json* dell'implementazione deve trovarsi in un percorso accessibile.</span><span class="sxs-lookup"><span data-stu-id="7b77a-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="7b77a-148">Per l'uso per utente, inserire il file nella cartella `additonalDeps` delle impostazioni `.dotnet` del profilo utente:</span><span class="sxs-lookup"><span data-stu-id="7b77a-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="7b77a-149">Per l'uso globale, inserire il file nella cartella `additonalDeps` dell'installazione di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="7b77a-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="7b77a-150">Si noti che la versione, `2.0.0`, riflette quella del runtime condiviso usato dall'app di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7b77a-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="7b77a-151">Il runtime condiviso è indicato nel file *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="7b77a-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="7b77a-152">Nell'app di esempio il runtime condiviso è specificato nel file *HostingStartupSample.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="7b77a-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="7b77a-153">**Impostare le variabili di ambiente**</span><span class="sxs-lookup"><span data-stu-id="7b77a-153">**Set environment variables**</span></span>

<span data-ttu-id="7b77a-154">Impostare le variabili di ambiente seguenti nel contesto dell'app che usa il miglioramento.</span><span class="sxs-lookup"><span data-stu-id="7b77a-154">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="7b77a-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="7b77a-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="7b77a-156">`HostingStartupAttribute` viene cercato solo negli assembly di avvio dell'hosting.</span><span class="sxs-lookup"><span data-stu-id="7b77a-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="7b77a-157">Il nome di assembly dell'implementazione viene specificato in questa variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="7b77a-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="7b77a-158">L'app di esempio imposta questo valore su `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="7b77a-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="7b77a-159">È possibile impostare il valore anche tramite l'impostazione di configurazione host [Assembly di avvio dell'hosting](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="7b77a-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="7b77a-160">DOTNET\_ADDITIONAL\_DEPS</span><span class="sxs-lookup"><span data-stu-id="7b77a-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="7b77a-161">Il percorso del file *\*.deps.json* dell'implementazione.</span><span class="sxs-lookup"><span data-stu-id="7b77a-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="7b77a-162">Se il file è inserito nella cartella *.dotnet* del profilo utente per l'uso per utente:</span><span class="sxs-lookup"><span data-stu-id="7b77a-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="7b77a-163">Se il file è inserito nella cartella di installazione di .NET Core per l'uso globale, specificare il percorso completo al file:</span><span class="sxs-lookup"><span data-stu-id="7b77a-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="7b77a-164">L'app di esempio imposta questo valore su:</span><span class="sxs-lookup"><span data-stu-id="7b77a-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="7b77a-165">Per esempi che illustrano come impostare le variabili di ambiente per diversi sistemi operativi, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7b77a-165">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="7b77a-166">App di esempio</span><span class="sxs-lookup"><span data-stu-id="7b77a-166">Sample app</span></span>

<span data-ttu-id="7b77a-167">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` per creare uno strumento di diagnostica.</span><span class="sxs-lookup"><span data-stu-id="7b77a-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="7b77a-168">All'avvio dell'app, lo strumento aggiunge due middleware che offrono informazioni di diagnostica:</span><span class="sxs-lookup"><span data-stu-id="7b77a-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="7b77a-169">Servizi registrati</span><span class="sxs-lookup"><span data-stu-id="7b77a-169">Registered services</span></span>
* <span data-ttu-id="7b77a-170">Indirizzo: schema, host, base del percorso, percorso, stringa di query</span><span class="sxs-lookup"><span data-stu-id="7b77a-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="7b77a-171">Connessione: IP remoto, porta remota, IP locale, porta locale, certificato client</span><span class="sxs-lookup"><span data-stu-id="7b77a-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="7b77a-172">Intestazioni della richiesta</span><span class="sxs-lookup"><span data-stu-id="7b77a-172">Request headers</span></span>
* <span data-ttu-id="7b77a-173">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="7b77a-173">Environment variables</span></span>

<span data-ttu-id="7b77a-174">Per eseguire l'esempio:</span><span class="sxs-lookup"><span data-stu-id="7b77a-174">To run the sample:</span></span>

1. <span data-ttu-id="7b77a-175">Il progetto Startup Diagnostic usa [PowerShell](/powershell/scripting/powershell-scripting) per modificare il file *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="7b77a-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="7b77a-176">PowerShell viene installato per impostazione predefinita nel sistema operativo Windows a partire da Windows 7 SP1 e Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="7b77a-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="7b77a-177">Per ottenere PowerShell su altre piattaforme, vedere [Installazione di Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="7b77a-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="7b77a-178">Compilare il progetto Startup Diagnostic.</span><span class="sxs-lookup"><span data-stu-id="7b77a-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="7b77a-179">Una destinazione di compilazione nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="7b77a-179">A build target in the project file:</span></span>
   * <span data-ttu-id="7b77a-180">Sposta l'assembly e i file di simboli nell'archivio di runtime del profilo utente.</span><span class="sxs-lookup"><span data-stu-id="7b77a-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="7b77a-181">Attiva lo script di PowerShell per modificare il file *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="7b77a-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="7b77a-182">Sposta il file *StartupDiagnostics.deps.json* nella cartella `additionalDeps` del profilo utente.</span><span class="sxs-lookup"><span data-stu-id="7b77a-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="7b77a-183">Impostare le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="7b77a-183">Set the environment variables:</span></span>
    * <span data-ttu-id="7b77a-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="7b77a-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="7b77a-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="7b77a-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="7b77a-186">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="7b77a-186">Run the sample app.</span></span>
5. <span data-ttu-id="7b77a-187">Richiedere l'endpoint `/services` per visualizzare i servizi registrati dell'app.</span><span class="sxs-lookup"><span data-stu-id="7b77a-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="7b77a-188">Richiedere l'endpoint `/diag` per visualizzare le informazioni di diagnostica.</span><span class="sxs-lookup"><span data-stu-id="7b77a-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
