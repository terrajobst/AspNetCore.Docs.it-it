---
title: "Aggiungere le funzionalità dell'app da un assembly esterno utilizzando IHostingStartup in ASP.NET Core"
author: guardrex
description: "Informazioni su come aggiungere funzionalità a un'applicazione ASP.NET di base da un assembly esterno usando un'implementazione IHostingStartup."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 12/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: 6013091d94302f0f9ecbc777d3ef64774b49aebe
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a><span data-ttu-id="ef3d8-103">Aggiungere le funzionalità dell'app da un assembly esterno utilizzando IHostingStartup in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef3d8-103">Add app features from an external assembly using IHostingStartup in ASP.NET Core</span></span>

<span data-ttu-id="ef3d8-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ef3d8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ef3d8-105">Un [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementazione consente l'aggiunta di funzionalità a un'app all'avvio all'esterno dell'app `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from outside of the app's `Startup` class.</span></span> <span data-ttu-id="ef3d8-106">Ad esempio, è possibile utilizzare una libreria di strumenti esterni un `IHostingStartup` implementazione per fornire servizi a un'app o provider di configurazione aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="ef3d8-107">`IHostingStartup`*è disponibile in ASP.NET Core 2.0 e versioni successive.*</span><span class="sxs-lookup"><span data-stu-id="ef3d8-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="ef3d8-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef3d8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="ef3d8-109">Individuare l'assembly di avvio hosting caricati</span><span class="sxs-lookup"><span data-stu-id="ef3d8-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="ef3d8-110">Per individuare gli assembly di avvio di hosting caricato dall'applicazione o dalle librerie, abilitare la registrazione e controllare i registri di applicazione.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="ef3d8-111">Vengono registrati gli errori che si verificano durante il caricamento di assembly.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="ef3d8-112">Caricare gli assembly di avvio di hosting sono connessi a livello di Debug e tutti gli errori vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="ef3d8-113">Le operazioni di lettura di app di esempio del [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) in un `string` di matrice e viene visualizzato il risultato nella pagina di indice dell'app:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-113">The sample app reads the the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="ef3d8-114">Disabilitare il caricamento automatico di hosting di assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="ef3d8-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="ef3d8-115">Esistono due modi per disabilitare il caricamento automatico di hosting di assembly di avvio:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="ef3d8-116">Impostare il [impedire l'avvio di Hosting](xref:fundamentals/hosting#prevent-hosting-startup) ospitare l'impostazione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="ef3d8-117">Impostare il `ASPNETCORE_preventHostingStartup` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="ef3d8-118">Quando l'impostazione dell'host o la variabile di ambiente è impostata su `true` o `1`, hosting gli assembly di avvio non vengono caricati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="ef3d8-119">Se entrambi sono impostati, l'host controlla il comportamento.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="ef3d8-120">La disabilitazione di hosting gli assembly di avvio usando la variabile di ambiente o impostazione host disabilitati a livello globale e può disabilitare la funzionalità diverse di un'app.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="ef3d8-121">Non è attualmente possibile disabilitare selettivamente un assembly di avvio hosting aggiunto da una libreria a meno che la libreria offre un proprio opzione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="ef3d8-122">Una versione futura offrono la possibilità di disabilitare selettivamente l'hosting degli assembly di avvio (vedere [GitHub emettere aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="ef3d8-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="ef3d8-123">Implementare le funzionalità di IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="ef3d8-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="ef3d8-124">Creare l'assembly</span><span class="sxs-lookup"><span data-stu-id="ef3d8-124">Create the assembly</span></span>

<span data-ttu-id="ef3d8-125">Un `IHostingStartup` funzionalità viene distribuito come un assembly in base a un'applicazione console senza un punto di ingresso.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="ef3d8-126">I riferimenti agli assembly di [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pacchetto:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="ef3d8-127">Oggetto [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attributo identifica una classe come un'implementazione di `IHostingStartup` per il caricamento e l'esecuzione quando si compila il [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="ef3d8-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="ef3d8-128">Nell'esempio seguente, lo spazio dei nomi è `StartupFeature`, e la classe è `StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="ef3d8-129">Una classe implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="ef3d8-130">La classe [configura](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metodo utilizza un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere funzionalità a un'app:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="ef3d8-131">Quando si compila un `IHostingStartup` file di progetto, le dipendenze (*\*. deps.json*) imposta il `runtime` percorso dell'assembly per il *bin* cartella:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="ef3d8-132">Viene visualizzata solo una parte del file.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-132">Only part of the file is shown.</span></span> <span data-ttu-id="ef3d8-133">Nome dell'assembly nell'esempio `StartupFeature`.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="ef3d8-134">Aggiornare il file di dipendenze</span><span class="sxs-lookup"><span data-stu-id="ef3d8-134">Update the dependencies file</span></span>

<span data-ttu-id="ef3d8-135">Il percorso di runtime è specificato nella  *\*. deps.json* file.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="ef3d8-136">Su attivo, la funzionalità di `runtime` elemento è necessario specificare il percorso dell'assembly di runtime della funzionalità.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="ef3d8-137">Prefisso di `runtime` posizione con `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="ef3d8-138">Nell'applicazione di esempio, la modifica del  *\*. deps.json* file viene eseguita da un [PowerShell](/powershell/scripting/powershell-scripting) script.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="ef3d8-139">Lo script di PowerShell viene automaticamente attivato da una destinazione di compilazione nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="ef3d8-140">Attivazione della caratteristica</span><span class="sxs-lookup"><span data-stu-id="ef3d8-140">Feature activation</span></span>

<span data-ttu-id="ef3d8-141">**Inserire il file di assembly**</span><span class="sxs-lookup"><span data-stu-id="ef3d8-141">**Place the assembly file**</span></span>

<span data-ttu-id="ef3d8-142">Il `IHostingStartup` deve essere il file di assembly dell'implementazione *bin*-distribuito nell'app o inserito nel [archivio runtime](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="ef3d8-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="ef3d8-143">Per l'utilizzo per singolo utente, inserire l'assembly nell'archivio di runtime del profilo utente in:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="ef3d8-144">Per l'utilizzo globale, inserire l'assembly nell'archivio di runtime dell'installazione di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="ef3d8-145">Quando si distribuisce l'assembly all'archivio di runtime, il file di simboli può essere distribuito anche ma non è necessario usare la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="ef3d8-146">**Inserire il file di dipendenze**</span><span class="sxs-lookup"><span data-stu-id="ef3d8-146">**Place the dependencies file**</span></span>

<span data-ttu-id="ef3d8-147">L'implementazione  *\*. deps.json* file deve essere in un percorso accessibile.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="ef3d8-148">Per l'utilizzo per singolo utente, inserire il file nella `additonalDeps` cartella del profilo utente `.dotnet` impostazioni:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="ef3d8-149">Per l'utilizzo globale, inserire il file nella `additonalDeps` cartella dell'installazione di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="ef3d8-150">Prendere nota della versione, `2.0.0`, corrisponde alla versione del runtime condiviso che usa l'app di destinazione.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="ef3d8-151">La fase di esecuzione condiviso viene visualizzato nel  *\*. runtimeconfig.json* file.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="ef3d8-152">Nell'applicazione di esempio, il runtime condiviso viene specificato nella *HostingStartupSample.runtimeconfig.json* file.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="ef3d8-153">**Set di variabili di ambiente**</span><span class="sxs-lookup"><span data-stu-id="ef3d8-153">**Set environment variables**</span></span>

<span data-ttu-id="ef3d8-154">Impostare le variabili di ambiente seguenti nel contesto dell'applicazione che utilizza la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="ef3d8-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="ef3d8-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="ef3d8-156">Vengono analizzati solo gli assembly di avvio di hosting per il `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="ef3d8-157">Il nome dell'assembly dell'implementazione viene fornito nella variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="ef3d8-158">Questo valore viene impostato l'app di esempio `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="ef3d8-159">Il valore può essere impostato anche tramite il [Hosting gli assembly di avvio](xref:fundamentals/hosting#hosting-startup-assemblies) ospitare l'impostazione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="ef3d8-160">DOTNET\_AGGIUNTIVI\_DEPS</span><span class="sxs-lookup"><span data-stu-id="ef3d8-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="ef3d8-161">Il percorso dell'implementazione  *\*. deps.json* file.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="ef3d8-162">Se il file si trova il profilo utente *.dotnet* cartella da utilizzare per ogni utente:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="ef3d8-163">Se il file si trova nell'installazione di .NET Core per l'utilizzo generale, specificare il percorso completo del file:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="ef3d8-164">Questo valore viene impostato l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="ef3d8-165">Per esempi di come impostare le variabili di ambiente per diversi sistemi operativi, vedere [utilizzo di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="ef3d8-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="ef3d8-166">App di esempio</span><span class="sxs-lookup"><span data-stu-id="ef3d8-166">Sample app</span></span>

<span data-ttu-id="ef3d8-167">Il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) utilizza `IHostingStartup` per creare uno strumento di diagnostica.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="ef3d8-168">Lo strumento aggiunge due middlewares all'app all'avvio che forniscono informazioni di diagnostica:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="ef3d8-169">Registrazione di servizi</span><span class="sxs-lookup"><span data-stu-id="ef3d8-169">Registered services</span></span>
* <span data-ttu-id="ef3d8-170">Indirizzo: schema, host, percorso di base, percorso, stringa di query</span><span class="sxs-lookup"><span data-stu-id="ef3d8-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="ef3d8-171">Connessione: remoto IP, porta remota, IP locale, porta locale, il certificato client</span><span class="sxs-lookup"><span data-stu-id="ef3d8-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="ef3d8-172">Le intestazioni di richiesta</span><span class="sxs-lookup"><span data-stu-id="ef3d8-172">Request headers</span></span>
* <span data-ttu-id="ef3d8-173">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="ef3d8-173">Environment variables</span></span>

<span data-ttu-id="ef3d8-174">Per eseguire l'esempio:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-174">To run the sample:</span></span>

1. <span data-ttu-id="ef3d8-175">Il progetto di avvio diagnostica utilizza [PowerShell](/powershell/scripting/powershell-scripting) per modificare il relativo *StartupDiagnostics.deps.json* file.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="ef3d8-176">PowerShell viene installato per impostazione predefinita in un sistema operativo Windows a partire da Windows 7 SP1 e Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="ef3d8-177">Per ottenere PowerShell su altre piattaforme, vedere [installazione di Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="ef3d8-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="ef3d8-178">Compilare il progetto di avvio di diagnostica.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="ef3d8-179">Una destinazione di compilazione nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-179">A build target in the project file:</span></span>
   * <span data-ttu-id="ef3d8-180">Consente di spostare l'assembly e file di archivio di runtime del profilo utente di simboli.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="ef3d8-181">Genera lo script di PowerShell per modificare il *StartupDiagnostics.deps.json* file.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="ef3d8-182">Sposta il *StartupDiagnostics.deps.json* file del profilo utente `additionalDeps` cartella.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="ef3d8-183">Impostare le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="ef3d8-183">Set the environment variables:</span></span>
    * <span data-ttu-id="ef3d8-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="ef3d8-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="ef3d8-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="ef3d8-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="ef3d8-186">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-186">Run the sample app.</span></span>
5. <span data-ttu-id="ef3d8-187">Richiedere il `/services` endpoint per l'App registrato di servizi.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="ef3d8-188">Richiedere il `/diag` endpoint per visualizzare le informazioni di diagnostica.</span><span class="sxs-lookup"><span data-stu-id="ef3d8-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
