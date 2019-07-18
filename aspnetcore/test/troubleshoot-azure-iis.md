---
title: Risolvere i problemi relativi a ASP.NET Core in app Azure servizio e IIS
author: guardrex
description: Informazioni su come diagnosticare i problemi relativi alle distribuzioni del servizio app Azure e Internet Information Services (IIS) delle app di ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2019
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 46d4a11c594844e059fa8555255d7321f7b48631
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308794"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="77f84-103">Risolvere i problemi relativi a ASP.NET Core in app Azure servizio e IIS</span><span class="sxs-lookup"><span data-stu-id="77f84-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="77f84-104">Di [Luke Latham](https://github.com/guardrex) e [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="77f84-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="77f84-105">Questo articolo fornisce informazioni sugli errori comuni di avvio delle app e istruzioni su come diagnosticare gli errori quando un'app viene distribuita in app Azure servizio o IIS:</span><span class="sxs-lookup"><span data-stu-id="77f84-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="77f84-106">Errori di avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="77f84-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="77f84-107">Vengono illustrati gli scenari di codice di stato HTTP di avvio comuni.</span><span class="sxs-lookup"><span data-stu-id="77f84-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="77f84-108">Risolvere i problemi relativi al servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="77f84-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="77f84-109">Fornisce consigli per la risoluzione dei problemi per le app distribuite nel servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="77f84-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="77f84-110">Risolvere i problemi in IIS</span><span class="sxs-lookup"><span data-stu-id="77f84-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="77f84-111">Fornisce consigli per la risoluzione dei problemi per le app distribuite in IIS o in esecuzione in IIS Express localmente.</span><span class="sxs-lookup"><span data-stu-id="77f84-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="77f84-112">Le linee guida sono valide per le distribuzioni di Windows Server e Windows desktop.</span><span class="sxs-lookup"><span data-stu-id="77f84-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="77f84-113">Cancella cache di pacchetti</span><span class="sxs-lookup"><span data-stu-id="77f84-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="77f84-114">Spiega cosa fare quando i pacchetti incoerenti interrompono un'app durante l'esecuzione di aggiornamenti principali o la modifica delle versioni del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="77f84-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="77f84-115">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="77f84-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="77f84-116">Elenca ulteriori argomenti sulla risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="77f84-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="77f84-117">Errori di avvio delle app</span><span class="sxs-lookup"><span data-stu-id="77f84-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="77f84-118">In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug.</span><span class="sxs-lookup"><span data-stu-id="77f84-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="77f84-119">Un errore *502,5-Process* o un *errore di avvio 500,30* che si verifica quando il debug locale può essere diagnosticato utilizzando i consigli in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="77f84-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="77f84-120">In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug.</span><span class="sxs-lookup"><span data-stu-id="77f84-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="77f84-121">Un *errore del processo 502,5* che si verifica quando è possibile diagnosticare localmente il debug usando i consigli in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="77f84-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="77f84-122">500 Errore interno del server</span><span class="sxs-lookup"><span data-stu-id="77f84-122">500 Internal Server Error</span></span>

<span data-ttu-id="77f84-123">L'app viene avviata, ma un errore impedisce al server di soddisfare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="77f84-123">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="77f84-124">Questo errore si verifica all'interno del codice dell'app durante l'avvio o durante la creazione di una risposta.</span><span class="sxs-lookup"><span data-stu-id="77f84-124">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="77f84-125">La risposta potrebbe non avere alcun contenuto o essere visualizzata nel browser come un codice *500 Errore interno del server*.</span><span class="sxs-lookup"><span data-stu-id="77f84-125">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="77f84-126">Il log eventi dell'applicazione in genere indica che l'app è stata avviata normalmente.</span><span class="sxs-lookup"><span data-stu-id="77f84-126">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="77f84-127">Dal punto di vista del server, questo è corretto.</span><span class="sxs-lookup"><span data-stu-id="77f84-127">From the server's perspective, that's correct.</span></span> <span data-ttu-id="77f84-128">L'app è stata effettivamente avviata, ma non può generare una risposta valida.</span><span class="sxs-lookup"><span data-stu-id="77f84-128">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="77f84-129">Eseguire l'app al prompt dei comandi nel server o abilitare il log stdout del modulo ASP.NET Core per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="77f84-129">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="77f84-130">500.0 Errore di caricamento del gestore in-process</span><span class="sxs-lookup"><span data-stu-id="77f84-130">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="77f84-131">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="77f84-131">The worker process fails.</span></span> <span data-ttu-id="77f84-132">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="77f84-132">The app doesn't start.</span></span>

<span data-ttu-id="77f84-133">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) non riesce a trovare il CLR .NET Core e a trovare il gestore della richiesta in-process (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="77f84-133">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="77f84-134">Controllare che:</span><span class="sxs-lookup"><span data-stu-id="77f84-134">Check that:</span></span>

* <span data-ttu-id="77f84-135">L'app specifichi come destinazione il pacchetto NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) o il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="77f84-135">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="77f84-136">Le versione del framework condiviso ASP.NET Core specificata come destinazione dall'app sia installata nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-136">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="77f84-137">500.0 Errore di caricamento del gestore out-of-process</span><span class="sxs-lookup"><span data-stu-id="77f84-137">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="77f84-138">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="77f84-138">The worker process fails.</span></span> <span data-ttu-id="77f84-139">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="77f84-139">The app doesn't start.</span></span>

<span data-ttu-id="77f84-140">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) non riesce a trovare il gestore della richiesta di hosting out-of-process.</span><span class="sxs-lookup"><span data-stu-id="77f84-140">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="77f84-141">Verificare che *aspnetcorev2_outofprocess.dll* sia presente in una sottocartella accanto ad *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="77f84-141">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="77f84-142">500.0 Errore di caricamento del gestore in-process</span><span class="sxs-lookup"><span data-stu-id="77f84-142">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="77f84-143">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="77f84-143">The worker process fails.</span></span> <span data-ttu-id="77f84-144">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="77f84-144">The app doesn't start.</span></span>

<span data-ttu-id="77f84-145">Si è verificato un errore sconosciuto durante il caricamento dei componenti del [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="77f84-145">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="77f84-146">Eseguire una delle azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="77f84-146">Take one of the following actions:</span></span>

* <span data-ttu-id="77f84-147">Contattare il [supporto tecnico Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832). Selezionare **Strumenti di sviluppo** e quindi **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="77f84-147">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="77f84-148">Porre una domanda in Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="77f84-148">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="77f84-149">Segnalare il problema nel [repository GitHub](https://github.com/aspnet/AspNetCore) di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77f84-149">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="77f84-150">500.30 Errore di avvio in-process</span><span class="sxs-lookup"><span data-stu-id="77f84-150">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="77f84-151">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="77f84-151">The worker process fails.</span></span> <span data-ttu-id="77f84-152">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="77f84-152">The app doesn't start.</span></span>

<span data-ttu-id="77f84-153">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tenta di avviare .NET Core CLR in-process, ma non è possibile avviarlo.</span><span class="sxs-lookup"><span data-stu-id="77f84-153">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="77f84-154">La cause di un errore di avvio del processo può in genere essere determinata dalle voci nel registro eventi dell'applicazione e dal log stdout del modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77f84-154">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="77f84-155">Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente.</span><span class="sxs-lookup"><span data-stu-id="77f84-155">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="77f84-156">Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-156">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="77f84-157">500.31 ANCM Failed to Find Native Dependencies (500.31 Il modulo ASP.NET Core non è riuscito a trovare le dipendenze native)</span><span class="sxs-lookup"><span data-stu-id="77f84-157">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="77f84-158">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="77f84-158">The worker process fails.</span></span> <span data-ttu-id="77f84-159">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="77f84-159">The app doesn't start.</span></span>

<span data-ttu-id="77f84-160">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tenta di avviare il runtime di .NET Core in-process, ma non è possibile avviarlo.</span><span class="sxs-lookup"><span data-stu-id="77f84-160">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="77f84-161">La causa più comune di questo errore di avvio è la mancata installazione del runtime di `Microsoft.NETCore.App` o di `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="77f84-161">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="77f84-162">Se l'app viene distribuita per ASP.NET Core 3.0 e tale versione non esiste nel computer, si verifica questo errore.</span><span class="sxs-lookup"><span data-stu-id="77f84-162">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="77f84-163">Ecco un esempio di messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="77f84-163">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="77f84-164">Il messaggio di errore elenca tutte le versioni di .NET Core installate e la versione richiesta dall'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-164">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="77f84-165">Per correggere l'errore, eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="77f84-165">To fix this error, either:</span></span>

* <span data-ttu-id="77f84-166">Installare la versione appropriata di .NET Core nel computer.</span><span class="sxs-lookup"><span data-stu-id="77f84-166">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="77f84-167">Modificare l'app in modo che usi una versione di .NET Core presente nel computer.</span><span class="sxs-lookup"><span data-stu-id="77f84-167">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="77f84-168">Pubblicare l'app come [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="77f84-168">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="77f84-169">Durante l'esecuzione in fase di sviluppo (con la variabile di ambiente `ASPNETCORE_ENVIRONMENT` impostata su `Development`), l'errore specifico viene scritto nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="77f84-169">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="77f84-170">La cause di un errore di avvio del processo è disponibile anche nel registro eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-170">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="77f84-171">500.32 ANCM Failed to Load dll (500.32 Il modulo ASP.NET Core non è riuscito a caricare la DLL)</span><span class="sxs-lookup"><span data-stu-id="77f84-171">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="77f84-172">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="77f84-172">The worker process fails.</span></span> <span data-ttu-id="77f84-173">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="77f84-173">The app doesn't start.</span></span>

<span data-ttu-id="77f84-174">La causa più comune di questo errore è la pubblicazione dell'app per processori con architettura non compatibile.</span><span class="sxs-lookup"><span data-stu-id="77f84-174">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="77f84-175">Se il processo di lavoro è in esecuzione come app a 32 bit e l'app è stata pubblicata per un'architettura a 64 bit, si verifica questo errore.</span><span class="sxs-lookup"><span data-stu-id="77f84-175">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="77f84-176">Per correggere l'errore, eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="77f84-176">To fix this error, either:</span></span>

* <span data-ttu-id="77f84-177">Ripubblicare l'app per processori con la stessa architettura del processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="77f84-177">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="77f84-178">Pubblicare l'app come [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="77f84-178">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="77f84-179">500.33 ANCM Request Handler Load Failure (500.33 Errore di caricamento del gestore della richiesta del modulo ASP.Net Core)</span><span class="sxs-lookup"><span data-stu-id="77f84-179">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="77f84-180">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="77f84-180">The worker process fails.</span></span> <span data-ttu-id="77f84-181">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="77f84-181">The app doesn't start.</span></span>

<span data-ttu-id="77f84-182">L'app non fa riferimento al framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="77f84-182">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="77f84-183">Solo le app destinate `Microsoft.AspNetCore.App` al Framework possono essere ospitate dal [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="77f84-183">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="77f84-184">Per correggere questo errore, verificare che l'app sia destinata al framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="77f84-184">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="77f84-185">Controllare il framework di destinazione dell'app nel file `.runtimeconfig.json`.</span><span class="sxs-lookup"><span data-stu-id="77f84-185">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="77f84-186">500.34 ANCM Mixed Hosting Models Not Supported (500.34 Modelli di hosting misto non supportati nel modulo ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="77f84-186">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="77f84-187">Il processo di lavoro non è in grado di eseguire un'app In-Process e un'app Out-of-process nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="77f84-187">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="77f84-188">Per correggere questo errore, eseguire le app in pool di applicazioni IIS separati.</span><span class="sxs-lookup"><span data-stu-id="77f84-188">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="77f84-189">500.35 ANCM Multiple In-Process Applications in same Process (500.35 Modulo ASP.NET Core: più applicazioni In-Process nello stesso processo)</span><span class="sxs-lookup"><span data-stu-id="77f84-189">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="77f84-190">Il processo di lavoro non è in grado di eseguire un'app In-Process e un'app Out-of-process nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="77f84-190">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="77f84-191">Per correggere questo errore, eseguire le app in pool di applicazioni IIS separati.</span><span class="sxs-lookup"><span data-stu-id="77f84-191">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="77f84-192">500.36 ANCM Out-Of-Process Handler Load Failure (500.36 Modulo ASP.NET Core: Errore di caricamento del gestore Out-of-process)</span><span class="sxs-lookup"><span data-stu-id="77f84-192">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="77f84-193">Il gestore delle richieste Out-of-process, *aspnetcorev2_outofprocess.dll*, non è accanto al file *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="77f84-193">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="77f84-194">Indica un'installazione danneggiata del [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="77f84-194">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="77f84-195">Per correggere questo errore, riparare l'installazione del [bundle di hosting di .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (per IIS) o di Visual Studio (per IIS Express).</span><span class="sxs-lookup"><span data-stu-id="77f84-195">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="77f84-196">500.37 ANCM Failed to Start Within Startup Time Limit (500.37 Avvio del modulo ASP.NET Core non riuscito entro il limite di tempo stabilito)</span><span class="sxs-lookup"><span data-stu-id="77f84-196">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="77f84-197">Il modulo ASP.NET Core non è stato avviato entro il limite di tempo specificato.</span><span class="sxs-lookup"><span data-stu-id="77f84-197">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="77f84-198">Per impostazione predefinita, il timeout è di 120 secondi.</span><span class="sxs-lookup"><span data-stu-id="77f84-198">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="77f84-199">Questo errore può verificarsi quando si avvia un numero elevato di app nello stesso computer.</span><span class="sxs-lookup"><span data-stu-id="77f84-199">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="77f84-200">Controllare i picchi di utilizzo della CPU e della memoria nel server durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="77f84-200">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="77f84-201">Può essere necessario scaglionare il processo di avvio di più app.</span><span class="sxs-lookup"><span data-stu-id="77f84-201">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="77f84-202">502.5 Errore del processo</span><span class="sxs-lookup"><span data-stu-id="77f84-202">502.5 Process Failure</span></span>

<span data-ttu-id="77f84-203">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="77f84-203">The worker process fails.</span></span> <span data-ttu-id="77f84-204">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="77f84-204">The app doesn't start.</span></span>

<span data-ttu-id="77f84-205">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tenta di avviare il processo di lavoro, ma l'avvio non riesce.</span><span class="sxs-lookup"><span data-stu-id="77f84-205">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="77f84-206">La cause di un errore di avvio del processo può in genere essere determinata dalle voci nel registro eventi dell'applicazione e dal log stdout del modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77f84-206">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="77f84-207">Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente.</span><span class="sxs-lookup"><span data-stu-id="77f84-207">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="77f84-208">Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-208">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="77f84-209">Il *framework condiviso* è il set di assembly (file *DLL*) che vengono installati nel computer e cui fa riferimento un metapacchetto, ad esempio `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="77f84-209">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="77f84-210">Il riferimento del metapacchetto può specificare una versione minima richiesta.</span><span class="sxs-lookup"><span data-stu-id="77f84-210">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="77f84-211">Per altre informazioni, vedere [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) (Il framework condiviso).</span><span class="sxs-lookup"><span data-stu-id="77f84-211">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="77f84-212">La pagina di errore *502.5 Errore del processo* viene restituita quando il processo di lavoro ha esito negativo a causa di un errore di configurazione dell'hosting o dell'app:</span><span class="sxs-lookup"><span data-stu-id="77f84-212">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="77f84-213">Non è stato possibile avviare l'aggiornamento dell'applicazione. Errore: 0x800700c1</span><span class="sxs-lookup"><span data-stu-id="77f84-213">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="77f84-214">L'app non è stata avviata perché non è stato possibile caricarne l'assembly ( *.dll*).</span><span class="sxs-lookup"><span data-stu-id="77f84-214">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="77f84-215">Questo errore si verifica quando è presente una mancata corrispondenza del numero di bit tra l'app pubblicata e il processo w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="77f84-215">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="77f84-216">Verificare che l'impostazione del pool di app a 32 bit sia corretta:</span><span class="sxs-lookup"><span data-stu-id="77f84-216">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="77f84-217">Selezionare il pool di app in **Pool di applicazioni** di Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="77f84-217">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="77f84-218">Selezionare **Impostazioni avanzate** in **Modifica pool di applicazioni** nel pannello **Azioni**.</span><span class="sxs-lookup"><span data-stu-id="77f84-218">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="77f84-219">Impostare **Attiva applicazioni a 32 bit**:</span><span class="sxs-lookup"><span data-stu-id="77f84-219">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="77f84-220">Se si sta sviluppando un'app a 32 bit (x86), impostare il valore su `True`.</span><span class="sxs-lookup"><span data-stu-id="77f84-220">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="77f84-221">Se si sta sviluppando un'app a 64 bit (x64), impostare il valore su `False`.</span><span class="sxs-lookup"><span data-stu-id="77f84-221">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="77f84-222">Reimpostazione della connessione</span><span class="sxs-lookup"><span data-stu-id="77f84-222">Connection reset</span></span>

<span data-ttu-id="77f84-223">Se si verifica un errore dopo l'invio delle intestazioni, è troppo tardi perché il server possa inviare un codice **500 Errore interno del server** quando si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="77f84-223">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="77f84-224">Questo spesso accade quando si verifica un errore durante la serializzazione di oggetti complessi per una risposta.</span><span class="sxs-lookup"><span data-stu-id="77f84-224">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="77f84-225">Questo tipo di errore viene visualizzato come un errore di *reimpostazione della connessione* nel client.</span><span class="sxs-lookup"><span data-stu-id="77f84-225">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="77f84-226">La [registrazione delle applicazioni](xref:fundamentals/logging/index) può consentire di risolvere questi tipi di errori.</span><span class="sxs-lookup"><span data-stu-id="77f84-226">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="77f84-227">Limiti di avvio predefiniti</span><span class="sxs-lookup"><span data-stu-id="77f84-227">Default startup limits</span></span>

<span data-ttu-id="77f84-228">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) è configurato con un valore predefinito di *startupTimeLimit* di 120 secondi.</span><span class="sxs-lookup"><span data-stu-id="77f84-228">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="77f84-229">Quando si mantiene il valore predefinito, l'avvio di un'app potrebbe richiedere fino a due minuti prima che il modulo registri un errore del processo.</span><span class="sxs-lookup"><span data-stu-id="77f84-229">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="77f84-230">Per informazioni sulla configurazione del modulo, vedere [Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="77f84-230">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="77f84-231">Risolvere i problemi relativi al servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="77f84-231">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="77f84-232">Registro eventi applicazioni (servizio app Azure)</span><span class="sxs-lookup"><span data-stu-id="77f84-232">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="77f84-233">Per accedere al log eventi dell'applicazione, usare il pannello **Diagnostica e risolvi i problemi** nel portale di Azure:</span><span class="sxs-lookup"><span data-stu-id="77f84-233">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="77f84-234">Nel portale di Azure aprire l'app in **Servizi app**.</span><span class="sxs-lookup"><span data-stu-id="77f84-234">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="77f84-235">Selezionare **Diagnostica e risolvi i problemi**.</span><span class="sxs-lookup"><span data-stu-id="77f84-235">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="77f84-236">Selezionare l'intestazione **Strumenti di diagnostica**.</span><span class="sxs-lookup"><span data-stu-id="77f84-236">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="77f84-237">In **Strumenti di supporto** selezionare il pulsante **Eventi dell'applicazione**.</span><span class="sxs-lookup"><span data-stu-id="77f84-237">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="77f84-238">Esaminare l'errore più recente dalla voce *IIS AspNetCoreModule* o *IIS AspNetCoreModule V2* nella colonna **Origine**.</span><span class="sxs-lookup"><span data-stu-id="77f84-238">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="77f84-239">Un'alternativa all'uso del pannello **Diagnostica e risolvi i problemi** consiste nell'esaminare direttamente il file del log eventi dell'applicazione usando [Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="77f84-239">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="77f84-240">Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="77f84-240">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="77f84-241">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="77f84-241">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="77f84-242">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="77f84-242">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="77f84-243">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="77f84-243">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="77f84-244">Aprire la cartella **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="77f84-244">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="77f84-245">Selezionare l'icona della matita accanto al file *eventlog.xml*.</span><span class="sxs-lookup"><span data-stu-id="77f84-245">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="77f84-246">Esaminare il log.</span><span class="sxs-lookup"><span data-stu-id="77f84-246">Examine the log.</span></span> <span data-ttu-id="77f84-247">Scorrere fino alla fine del log per visualizzare gli eventi più recenti.</span><span class="sxs-lookup"><span data-stu-id="77f84-247">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="77f84-248">Eseguire l'app nella console Kudu</span><span class="sxs-lookup"><span data-stu-id="77f84-248">Run the app in the Kudu console</span></span>

<span data-ttu-id="77f84-249">Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-249">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="77f84-250">È possibile eseguire l'app nella console di esecuzione remota [Kudu](https://github.com/projectkudu/kudu/wiki) per individuare l'errore:</span><span class="sxs-lookup"><span data-stu-id="77f84-250">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="77f84-251">Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="77f84-251">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="77f84-252">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="77f84-252">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="77f84-253">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="77f84-253">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="77f84-254">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="77f84-254">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="77f84-255">Test di un'app a 32 bit (x86)</span><span class="sxs-lookup"><span data-stu-id="77f84-255">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="77f84-256">**Versione corrente**</span><span class="sxs-lookup"><span data-stu-id="77f84-256">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="77f84-257">Eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="77f84-257">Run the app:</span></span>
   * <span data-ttu-id="77f84-258">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="77f84-258">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="77f84-259">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="77f84-259">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="77f84-260">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.</span><span class="sxs-lookup"><span data-stu-id="77f84-260">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="77f84-261">**Distribuzione dipendente dal Framework in esecuzione in una versione di anteprima**</span><span class="sxs-lookup"><span data-stu-id="77f84-261">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="77f84-262">*Richiede l'installazione dell'estensione del sito del runtime ASP.NET Core {VERSION} (x86).*</span><span class="sxs-lookup"><span data-stu-id="77f84-262">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="77f84-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` è la versione di runtime)</span><span class="sxs-lookup"><span data-stu-id="77f84-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="77f84-264">Eseguire l'app. `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="77f84-264">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="77f84-265">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.</span><span class="sxs-lookup"><span data-stu-id="77f84-265">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="77f84-266">Test di un'app a 64 bit (x64)</span><span class="sxs-lookup"><span data-stu-id="77f84-266">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="77f84-267">**Versione corrente**</span><span class="sxs-lookup"><span data-stu-id="77f84-267">**Current release**</span></span>

* <span data-ttu-id="77f84-268">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) a 64 bit (x64):</span><span class="sxs-lookup"><span data-stu-id="77f84-268">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="77f84-269">Eseguire l'app. `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="77f84-269">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="77f84-270">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="77f84-270">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="77f84-271">Eseguire l'app: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="77f84-271">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="77f84-272">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.</span><span class="sxs-lookup"><span data-stu-id="77f84-272">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="77f84-273">**Distribuzione dipendente dal Framework in esecuzione in una versione di anteprima**</span><span class="sxs-lookup"><span data-stu-id="77f84-273">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="77f84-274">*Richiede l'installazione dell'estensione del sito del runtime ASP.NET Core {VERSION} (x64).*</span><span class="sxs-lookup"><span data-stu-id="77f84-274">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="77f84-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` è la versione di runtime)</span><span class="sxs-lookup"><span data-stu-id="77f84-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="77f84-276">Eseguire l'app. `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="77f84-276">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="77f84-277">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.</span><span class="sxs-lookup"><span data-stu-id="77f84-277">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="77f84-278">Log stdout del modulo ASP.NET Core (servizio app Azure)</span><span class="sxs-lookup"><span data-stu-id="77f84-278">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="77f84-279">Il log stdout del modulo ASP.NET Core spesso registra utili messaggi di errore non disponibili nel log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-279">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="77f84-280">Per abilitare e visualizzare i log stdout:</span><span class="sxs-lookup"><span data-stu-id="77f84-280">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="77f84-281">Passare al pannello **Diagnostica e risolvi i problemi** nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="77f84-281">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="77f84-282">In **SELECT PROBLEM CATEGORY** (SELEZIONARE LA CATEGORIA DEL PROBLEMA) selezionare il pulsante **Web App Down** (App Web inattiva).</span><span class="sxs-lookup"><span data-stu-id="77f84-282">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="77f84-283">In **Suggested Solutions** (Soluzioni consigliate)  > **Enable Stdout Log Redirection** (Abilita il reindirizzamento del log Stdout) selezionare il pulsante **Open Kudu Console to edit Web.Config** (Apri la console Kudu per modificare Web.Config).</span><span class="sxs-lookup"><span data-stu-id="77f84-283">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="77f84-284">Nella **console diagnostica** Kudu aprire le cartelle nel percorso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="77f84-284">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="77f84-285">Scorrere verso il basso fino a visualizzare il file *web.config* in fondo all'elenco.</span><span class="sxs-lookup"><span data-stu-id="77f84-285">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="77f84-286">Fare clic sull'icona della matita accanto al file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="77f84-286">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="77f84-287">Impostare **stdoutLogEnabled** su `true` e cambiare il percorso di **stdoutLogFile** in: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="77f84-287">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="77f84-288">Selezionare **Salva** per salvare il file *web.config* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="77f84-288">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="77f84-289">Effettuare una richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-289">Make a request to the app.</span></span>
1. <span data-ttu-id="77f84-290">Tornare al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="77f84-290">Return to the Azure portal.</span></span> <span data-ttu-id="77f84-291">Selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**.</span><span class="sxs-lookup"><span data-stu-id="77f84-291">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="77f84-292">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="77f84-292">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="77f84-293">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="77f84-293">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="77f84-294">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="77f84-294">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="77f84-295">Selezionare la cartella **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="77f84-295">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="77f84-296">Controllare la colonna **Data modifica** e selezionare l'icona della matita per modificare il log stdout con la data di modifica più recente.</span><span class="sxs-lookup"><span data-stu-id="77f84-296">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="77f84-297">Quando si apre il file di log, verrà visualizzato l'errore.</span><span class="sxs-lookup"><span data-stu-id="77f84-297">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="77f84-298">Al termine della risoluzione dei problemi, disabilitare la registrazione stdout:</span><span class="sxs-lookup"><span data-stu-id="77f84-298">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="77f84-299">Nella **console diagnostica** Kudu tornare al percorso **site** > **wwwroot** per visualizzare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="77f84-299">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="77f84-300">Aprire nuovamente il file **web.config** selezionando l'icona della matita.</span><span class="sxs-lookup"><span data-stu-id="77f84-300">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="77f84-301">Impostare **stdoutLogEnabled** su `false`.</span><span class="sxs-lookup"><span data-stu-id="77f84-301">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="77f84-302">Selezionare **Salva** per salvare il file.</span><span class="sxs-lookup"><span data-stu-id="77f84-302">Select **Save** to save the file.</span></span>

<span data-ttu-id="77f84-303">Per altre informazioni, vedere <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="77f84-303">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="77f84-304">La mancata disabilitazione del log stdout può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="77f84-304">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="77f84-305">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="77f84-305">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="77f84-306">Usare la registrazione stdout solo per la risoluzione dei problemi di avvio delle app.</span><span class="sxs-lookup"><span data-stu-id="77f84-306">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="77f84-307">Per la registrazione generale in un'app ASP.NET Core dopo l'avvio, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-307">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="77f84-308">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="77f84-308">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="77f84-309">Log di debug del modulo ASP.NET Core (servizio app Azure)</span><span class="sxs-lookup"><span data-stu-id="77f84-309">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="77f84-310">Il log di debug del modulo ASP.NET Core fornisce dati di registrazione aggiuntivi e più approfonditi dal modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77f84-310">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="77f84-311">Per abilitare e visualizzare i log stdout:</span><span class="sxs-lookup"><span data-stu-id="77f84-311">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="77f84-312">Per abilitare il log di diagnostica avanzato, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="77f84-312">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="77f84-313">Seguire le istruzioni in [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) per configurare l'app per la registrazione di diagnostica avanzata.</span><span class="sxs-lookup"><span data-stu-id="77f84-313">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="77f84-314">Ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-314">Redeploy the app.</span></span>
   * <span data-ttu-id="77f84-315">Aggiungere le impostazioni `<handlerSettings>` indicate in [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) al file *web.config* dell'app distribuita, usando la console Kudu:</span><span class="sxs-lookup"><span data-stu-id="77f84-315">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="77f84-316">Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="77f84-316">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="77f84-317">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="77f84-317">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="77f84-318">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="77f84-318">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="77f84-319">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="77f84-319">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="77f84-320">Aprire le cartelle nel percorso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="77f84-320">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="77f84-321">Modificare il file *web.config* selezionando il pulsante a forma di matita.</span><span class="sxs-lookup"><span data-stu-id="77f84-321">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="77f84-322">Aggiungere la sezione `<handlerSettings>` come illustrato in [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="77f84-322">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="77f84-323">Selezionare il pulsante **Salva**.</span><span class="sxs-lookup"><span data-stu-id="77f84-323">Select the **Save** button.</span></span>
1. <span data-ttu-id="77f84-324">Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="77f84-324">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="77f84-325">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="77f84-325">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="77f84-326">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="77f84-326">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="77f84-327">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="77f84-327">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="77f84-328">Aprire le cartelle nel percorso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="77f84-328">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="77f84-329">Se non è stato specificato un percorso per il file *aspnetcore-debug.log*, il file viene visualizzato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="77f84-329">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="77f84-330">Se è stato specificato un percorso, passare al percorso del file di log.</span><span class="sxs-lookup"><span data-stu-id="77f84-330">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="77f84-331">Aprire il file di log con il pulsante a forma di matita accanto al nome del file.</span><span class="sxs-lookup"><span data-stu-id="77f84-331">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="77f84-332">Al termine della risoluzione dei problemi, disabilitare la registrazione di debug:</span><span class="sxs-lookup"><span data-stu-id="77f84-332">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="77f84-333">Per disabilitare il log di debug avanzato, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="77f84-333">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="77f84-334">Rimuovere `<handlerSettings>` dal file *web.config* in locale e ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-334">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="77f84-335">Usare la console Kudu per modificare il file *web.config* e rimuovere la sezione `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="77f84-335">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="77f84-336">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="77f84-336">Save the file.</span></span>

<span data-ttu-id="77f84-337">Per altre informazioni, vedere <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="77f84-337">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="77f84-338">La mancata disabilitazione del log di debug può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="77f84-338">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="77f84-339">Non è previsto alcun limite per le dimensioni del file di log.</span><span class="sxs-lookup"><span data-stu-id="77f84-339">There's no limit on log file size.</span></span> <span data-ttu-id="77f84-340">Usare solo la registrazione di debug per la risoluzione dei problemi di avvio delle app.</span><span class="sxs-lookup"><span data-stu-id="77f84-340">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="77f84-341">Per la registrazione generale in un'app ASP.NET Core dopo l'avvio, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-341">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="77f84-342">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="77f84-342">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="77f84-343">App lenta o sospesa (servizio app Azure)</span><span class="sxs-lookup"><span data-stu-id="77f84-343">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="77f84-344">Quando un'app risponde lentamente o si blocca durante una richiesta, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="77f84-344">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* <span data-ttu-id="77f84-345">[Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) (Risoluzione dei problemi di prestazioni delle app Web lente in Servizio app di Azure)</span><span class="sxs-lookup"><span data-stu-id="77f84-345">[Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)</span></span>
* <span data-ttu-id="77f84-346">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) (Usare l'estensione del sito Crash Diagnoser per acquisire il dump per i problemi di eccezioni intermittenti o i problemi di prestazioni nell'app Web di Azure)</span><span class="sxs-lookup"><span data-stu-id="77f84-346">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="77f84-347">Pannelli di monitoraggio</span><span class="sxs-lookup"><span data-stu-id="77f84-347">Monitoring blades</span></span>

<span data-ttu-id="77f84-348">I pannelli di monitoraggio forniscono un'esperienza di risoluzione dei problemi alternativa ai metodi descritti in precedenza in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="77f84-348">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="77f84-349">È possibile usare questi pannelli per diagnosticare gli errori della serie 500.</span><span class="sxs-lookup"><span data-stu-id="77f84-349">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="77f84-350">Verificare che le estensioni di ASP.NET Core siano installate.</span><span class="sxs-lookup"><span data-stu-id="77f84-350">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="77f84-351">Se le estensioni non sono installate, installarle manualmente:</span><span class="sxs-lookup"><span data-stu-id="77f84-351">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="77f84-352">Nella sezione del pannello **STRUMENTI DI SVILUPPO** selezionare il pannello **Estensioni**.</span><span class="sxs-lookup"><span data-stu-id="77f84-352">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="77f84-353">Nell'elenco dovrebbe essere visualizzato **ASP.NET Core Extensions** (Estensioni ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="77f84-353">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="77f84-354">Se le estensioni non sono installate, selezionare il pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="77f84-354">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="77f84-355">Scegliere **ASP.NET Core Extensions** (Estensioni ASP.NET Core) dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="77f84-355">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="77f84-356">Selezionare **OK** per accettare le condizioni legali.</span><span class="sxs-lookup"><span data-stu-id="77f84-356">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="77f84-357">Selezionare **OK** nel pannello **Aggiungi estensione**.</span><span class="sxs-lookup"><span data-stu-id="77f84-357">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="77f84-358">Un messaggio popup informativo indica quando le estensioni sono state installate correttamente.</span><span class="sxs-lookup"><span data-stu-id="77f84-358">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="77f84-359">Se la registrazione stdout non è abilitata, attenersi ai passaggi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="77f84-359">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="77f84-360">Nel portale di Azure selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**.</span><span class="sxs-lookup"><span data-stu-id="77f84-360">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="77f84-361">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="77f84-361">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="77f84-362">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="77f84-362">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="77f84-363">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="77f84-363">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="77f84-364">Aprire le cartelle nel percorso **site** > **wwwroot** e scorrere verso il basso fino a visualizzare il file *web.config* in fondo all'elenco.</span><span class="sxs-lookup"><span data-stu-id="77f84-364">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="77f84-365">Fare clic sull'icona della matita accanto al file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="77f84-365">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="77f84-366">Impostare **stdoutLogEnabled** su `true` e cambiare il percorso di **stdoutLogFile** in: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="77f84-366">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="77f84-367">Selezionare **Salva** per salvare il file *web.config* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="77f84-367">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="77f84-368">Passare all'attivazione della registrazione diagnostica:</span><span class="sxs-lookup"><span data-stu-id="77f84-368">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="77f84-369">Nel portale di Azure selezionare il pannello **Log di diagnostica**.</span><span class="sxs-lookup"><span data-stu-id="77f84-369">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="77f84-370">Selezionare l'interruttore **Attivato** per **Registrazione applicazioni (file system)** e **Messaggi di errore dettagliati**.</span><span class="sxs-lookup"><span data-stu-id="77f84-370">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="77f84-371">Selezionare il pulsante **Salva** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="77f84-371">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="77f84-372">Per includere la traccia delle richieste non riuscite, anche nota come registrazione FREB (Failed Request Event Buffering), selezionare l'interruttore **Attivato** per **Traccia delle richieste non riuscite**.</span><span class="sxs-lookup"><span data-stu-id="77f84-372">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="77f84-373">Selezionare il pannello **Flusso di registrazione**, immediatamente sotto il pannello **Log di diagnostica** nel portale.</span><span class="sxs-lookup"><span data-stu-id="77f84-373">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="77f84-374">Effettuare una richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-374">Make a request to the app.</span></span>
1. <span data-ttu-id="77f84-375">Nei dati del flusso di registrazione viene indicata la causa dell'errore.</span><span class="sxs-lookup"><span data-stu-id="77f84-375">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="77f84-376">Al termine della risoluzione dei problemi, assicurarsi di disabilitare la registrazione stdout.</span><span class="sxs-lookup"><span data-stu-id="77f84-376">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="77f84-377">Per visualizzare i log di traccia delle richieste non riuscite (log FREB):</span><span class="sxs-lookup"><span data-stu-id="77f84-377">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="77f84-378">Passare al pannello **Diagnostica e risolvi i problemi** nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="77f84-378">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="77f84-379">Selezionare **Failed Request Tracing Logs** (Log di traccia delle richieste non riuscite) nell'area **STRUMENTI DI SUPPORTO** della barra laterale.</span><span class="sxs-lookup"><span data-stu-id="77f84-379">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="77f84-380">Per altre informazioni, vedere la [sezione relativa alla traccia delle richieste non riuscite nell'argomento Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) e [Domande frequenti sulle prestazioni delle applicazioni in App Web di Azure: Come si abilita la traccia delle richieste non riuscite?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing).</span><span class="sxs-lookup"><span data-stu-id="77f84-380">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="77f84-381">Per altre informazioni, vedere [Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="77f84-381">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="77f84-382">La mancata disabilitazione del log stdout può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="77f84-382">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="77f84-383">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="77f84-383">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="77f84-384">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="77f84-385">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="77f84-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="77f84-386">Risoluzione dei problemi in IIS</span><span class="sxs-lookup"><span data-stu-id="77f84-386">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="77f84-387">Registro eventi applicazioni (IIS)</span><span class="sxs-lookup"><span data-stu-id="77f84-387">Application Event Log (IIS)</span></span>

<span data-ttu-id="77f84-388">Accedere al log eventi dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="77f84-388">Access the Application Event Log:</span></span>

1. <span data-ttu-id="77f84-389">Aprire il menu Start, cercare **Visualizzatore eventi** e quindi selezionare l'app **Visualizzatore eventi**.</span><span class="sxs-lookup"><span data-stu-id="77f84-389">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="77f84-390">In **Visualizzatore eventi** aprire il nodo **Registri di Windows**.</span><span class="sxs-lookup"><span data-stu-id="77f84-390">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="77f84-391">Selezionare **Applicazione** per aprire il log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-391">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="77f84-392">Cercare gli errori associati all'app in cui si è verificato il problema.</span><span class="sxs-lookup"><span data-stu-id="77f84-392">Search for errors associated with the failing app.</span></span> <span data-ttu-id="77f84-393">Gli errori presentano un valore *Modulo AspNetCore IIS* o *Modulo AspNetCore IIS Express* nella colonna *Origine*.</span><span class="sxs-lookup"><span data-stu-id="77f84-393">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="77f84-394">Eseguire l'app da un prompt dei comandi</span><span class="sxs-lookup"><span data-stu-id="77f84-394">Run the app at a command prompt</span></span>

<span data-ttu-id="77f84-395">Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-395">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="77f84-396">È possibile individuare la causa di alcuni errori eseguendo l'app da un prompt dei comandi nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="77f84-396">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="77f84-397">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="77f84-397">Framework-dependent deployment</span></span>

<span data-ttu-id="77f84-398">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="77f84-398">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="77f84-399">Da un prompt dei comandi passare alla cartella di distribuzione e avviare l'app eseguendo l'assembly dell'app con *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="77f84-399">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="77f84-400">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="77f84-400">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="77f84-401">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="77f84-401">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="77f84-402">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="77f84-402">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="77f84-403">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="77f84-403">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="77f84-404">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-404">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="77f84-405">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="77f84-405">Self-contained deployment</span></span>

<span data-ttu-id="77f84-406">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="77f84-406">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="77f84-407">Da un prompt dei comandi passare alla cartella di distribuzione e avviare il file eseguibile dell'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-407">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="77f84-408">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="77f84-408">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="77f84-409">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="77f84-409">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="77f84-410">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="77f84-410">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="77f84-411">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="77f84-411">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="77f84-412">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-412">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="77f84-413">Log stdout del modulo ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="77f84-413">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="77f84-414">Per abilitare e visualizzare i log stdout:</span><span class="sxs-lookup"><span data-stu-id="77f84-414">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="77f84-415">Passare alla cartella di distribuzione del sito nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="77f84-415">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="77f84-416">Se la cartella *logs* non è presente, creare la cartella.</span><span class="sxs-lookup"><span data-stu-id="77f84-416">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="77f84-417">Per istruzioni su come impostare MSBuild per la creazione automatica della cartella *logs* nella distribuzione, vedere l'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="77f84-417">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="77f84-418">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="77f84-418">Edit the *web.config* file.</span></span> <span data-ttu-id="77f84-419">Impostare **stdoutLogEnabled** su `true` e modificare il percorso di **stdoutLogFile** in modo da fare riferimento alla cartella *logs*, ad esempio `.\logs\stdout`.</span><span class="sxs-lookup"><span data-stu-id="77f84-419">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="77f84-420">`stdout` nel percorso è il prefisso del nome del file di log.</span><span class="sxs-lookup"><span data-stu-id="77f84-420">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="77f84-421">Un timestamp, l'ID del processo e l'estensione del file vengono aggiunti automaticamente al momento della creazione del log.</span><span class="sxs-lookup"><span data-stu-id="77f84-421">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="77f84-422">Usando `stdout` come prefisso del nome del file, un tipico file di log è denominato *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="77f84-422">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="77f84-423">Assicurarsi che l'identità del pool di applicazioni disponga delle autorizzazioni di scrittura per la cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="77f84-423">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="77f84-424">Salvare il file *web.config* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="77f84-424">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="77f84-425">Effettuare una richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-425">Make a request to the app.</span></span>
1. <span data-ttu-id="77f84-426">Passare alla cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="77f84-426">Navigate to the *logs* folder.</span></span> <span data-ttu-id="77f84-427">Trovare e aprire il log stdout più recente.</span><span class="sxs-lookup"><span data-stu-id="77f84-427">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="77f84-428">Esaminare il log per verificare se sono presenti errori.</span><span class="sxs-lookup"><span data-stu-id="77f84-428">Study the log for errors.</span></span>

<span data-ttu-id="77f84-429">Al termine della risoluzione dei problemi, disabilitare la registrazione stdout:</span><span class="sxs-lookup"><span data-stu-id="77f84-429">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="77f84-430">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="77f84-430">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="77f84-431">Impostare **stdoutLogEnabled** su `false`.</span><span class="sxs-lookup"><span data-stu-id="77f84-431">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="77f84-432">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="77f84-432">Save the file.</span></span>

<span data-ttu-id="77f84-433">Per altre informazioni, vedere <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="77f84-433">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="77f84-434">La mancata disabilitazione del log stdout può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="77f84-434">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="77f84-435">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="77f84-435">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="77f84-436">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="77f84-436">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="77f84-437">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="77f84-437">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="77f84-438">Log di debug del modulo ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="77f84-438">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="77f84-439">Aggiungere le impostazioni del gestore seguenti al file *Web. config* dell'app per abilitare il log di debug del modulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="77f84-439">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="77f84-440">Verificare che il percorso specificato per il log esista e che l'identità del pool di applicazioni abbia le autorizzazioni di scrittura nel percorso.</span><span class="sxs-lookup"><span data-stu-id="77f84-440">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="77f84-441">Per altre informazioni, vedere <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="77f84-441">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="77f84-442">Abilitare la pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="77f84-442">Enable the Developer Exception Page</span></span>

<span data-ttu-id="77f84-443">La variabile di ambiente `ASPNETCORE_ENVIRONMENT` [ può essere aggiunta a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) per eseguire l'app nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="77f84-443">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="77f84-444">Purché l'ambiente non sia sottoposto a override durante l'avvio dell'app tramite `UseEnvironment` nel generatore di host, l'impostazione della variabile di ambiente consente di visualizzare la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) quando viene eseguita l'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-444">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

<span data-ttu-id="77f84-445">L'impostazione della variabile di ambiente per `ASPNETCORE_ENVIRONMENT` è consigliata solo per l'uso in server di gestione temporanea e test non esposti a Internet.</span><span class="sxs-lookup"><span data-stu-id="77f84-445">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="77f84-446">Al termine della risoluzione dei problemi, rimuovere la variabile di ambiente dal file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="77f84-446">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="77f84-447">Per informazioni sull'impostazione delle variabili di ambiente in *web.config*, vedere [elemento figlio environmentVariables di aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="77f84-447">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="77f84-448">Ottenere dati da un'app</span><span class="sxs-lookup"><span data-stu-id="77f84-448">Obtain data from an app</span></span>

<span data-ttu-id="77f84-449">Se un'app è in grado di rispondere alle richieste, è possibile ottenere dati sulle richieste, le connessioni e altri dati dall'app tramite middleware inline di terminale.</span><span class="sxs-lookup"><span data-stu-id="77f84-449">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="77f84-450">Per altre informazioni e codice di esempio, vedere <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="77f84-450">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="77f84-451">App lenta o sospesa (IIS)</span><span class="sxs-lookup"><span data-stu-id="77f84-451">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="77f84-452">Un *dump di arresto anomalo* del sistema è uno snapshot della memoria del sistema e può contribuire a determinare la provocazione di un arresto anomalo dell'app, dell'avvio o dell'applicazione lenta.</span><span class="sxs-lookup"><span data-stu-id="77f84-452">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="77f84-453">Arresto anomalo o eccezione di un'app</span><span class="sxs-lookup"><span data-stu-id="77f84-453">App crashes or encounters an exception</span></span>

<span data-ttu-id="77f84-454">Ottenere e analizzare un dump da [Segnalazione errori Windows](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="77f84-454">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="77f84-455">Creare una cartella per i file dump di arresto anomalo del sistema in `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="77f84-455">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="77f84-456">Il pool di app deve avere accesso in scrittura alla cartella.</span><span class="sxs-lookup"><span data-stu-id="77f84-456">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="77f84-457">Eseguire lo [script di PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="77f84-457">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="77f84-458">Se l'app usa il [modello di hosting in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="77f84-458">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="77f84-459">Se l'app usa il [modello di hosting out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="77f84-459">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="77f84-460">Eseguire l'app nelle condizioni che causano l'arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="77f84-460">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="77f84-461">Dopo che si è verificato l'arresto anomalo, eseguire lo [script di PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="77f84-461">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="77f84-462">Se l'app usa il [modello di hosting in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="77f84-462">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="77f84-463">Se l'app usa il [modello di hosting out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="77f84-463">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="77f84-464">Dopo l'arresto anomalo di un'app e la raccolta dei dump, l'app può terminare normalmente.</span><span class="sxs-lookup"><span data-stu-id="77f84-464">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="77f84-465">Lo script di PowerShell configura Segnalazione errori Windows per raccogliere fino a cinque dump per ogni app.</span><span class="sxs-lookup"><span data-stu-id="77f84-465">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="77f84-466">I dump di arresto anomalo del sistema potrebbero richiedere una grande quantità di spazio su disco (fino a diversi gigabyte ognuno).</span><span class="sxs-lookup"><span data-stu-id="77f84-466">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="77f84-467">L'app si blocca, si verifica un errore durante l'avvio o viene eseguita normalmente</span><span class="sxs-lookup"><span data-stu-id="77f84-467">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="77f84-468">Quando un'app si *blocca* (smette di rispondere ma senza arresto anomalo), si verifica un errore durante l'avvio o viene eseguita normalmente, vedere [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (File dump in modalità utente: scelta dello strumento migliore) per selezionare uno strumento appropriato per produrre il dump.</span><span class="sxs-lookup"><span data-stu-id="77f84-468">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="77f84-469">Analizzare il dump</span><span class="sxs-lookup"><span data-stu-id="77f84-469">Analyze the dump</span></span>

<span data-ttu-id="77f84-470">È possibile analizzare un dump usando diversi approcci.</span><span class="sxs-lookup"><span data-stu-id="77f84-470">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="77f84-471">Per altre informazioni, vedere [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analisi di un file dump in modalità utente).</span><span class="sxs-lookup"><span data-stu-id="77f84-471">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="77f84-472">Cancella cache di pacchetti</span><span class="sxs-lookup"><span data-stu-id="77f84-472">Clear package caches</span></span>

<span data-ttu-id="77f84-473">A volte un'app funzionante ha esito negativo immediatamente dopo l'aggiornamento del .NET Core SDK nel computer di sviluppo o la modifica delle versioni del pacchetto all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-473">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="77f84-474">In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali.</span><span class="sxs-lookup"><span data-stu-id="77f84-474">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="77f84-475">La maggior parte di questi problemi può essere risolta attenendosi alle istruzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="77f84-475">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="77f84-476">Eliminare le cartelle *bin* e *obj*.</span><span class="sxs-lookup"><span data-stu-id="77f84-476">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="77f84-477">Cancellare le cache `dotnet nuget locals all --clear` dei pacchetti eseguendo da una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="77f84-477">Clear the package caches by executing `dotnet nuget locals all --clear` from a command shell.</span></span>

   <span data-ttu-id="77f84-478">La cancellazione delle cache dei pacchetti può essere eseguita anche con lo strumento [NuGet. exe](https://www.nuget.org/downloads) ed eseguendo il `nuget locals all -clear`comando.</span><span class="sxs-lookup"><span data-stu-id="77f84-478">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="77f84-479">*nuget.exe* non è un'installazione inclusa con il sistema operativo desktop Windows e deve essere ottenuta separatamente dal [sito Web NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="77f84-479">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="77f84-480">Ripristinare e ricompilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="77f84-480">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="77f84-481">Eliminare tutti i file nella cartella di distribuzione nel server prima di ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="77f84-481">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77f84-482">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="77f84-482">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="77f84-483">Documentazione di Azure</span><span class="sxs-lookup"><span data-stu-id="77f84-483">Azure documentation</span></span>

* [<span data-ttu-id="77f84-484">Application Insights per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77f84-484">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="77f84-485">Sezione app Web per il debug remoto di risoluzione dei problemi di un'app Web nel servizio app Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77f84-485">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="77f84-486">Panoramica della diagnostica del servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="77f84-486">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="77f84-487">Procedura: Monitorare le app in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="77f84-487">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="77f84-488">Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77f84-488">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="77f84-489">Risolvere gli errori HTTP "502 - Gateway non valido" e "503 - Servizio non disponibile" nelle App Web di Azure</span><span class="sxs-lookup"><span data-stu-id="77f84-489">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* <span data-ttu-id="77f84-490">[Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) (Risoluzione dei problemi di prestazioni delle app Web lente in Servizio app di Azure)</span><span class="sxs-lookup"><span data-stu-id="77f84-490">[Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)</span></span>
* [<span data-ttu-id="77f84-491">Domande frequenti sulle prestazioni delle applicazioni in App Web di Azure</span><span class="sxs-lookup"><span data-stu-id="77f84-491">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="77f84-492">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Sandbox per app Web di Azure - Limitazioni di esecuzione di runtime di Servizio app di Azure)</span><span class="sxs-lookup"><span data-stu-id="77f84-492">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* <span data-ttu-id="77f84-493">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)</span><span class="sxs-lookup"><span data-stu-id="77f84-493">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)</span></span>

### <a name="visual-studio-documentation"></a><span data-ttu-id="77f84-494">Documentazione di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77f84-494">Visual Studio documentation</span></span>

* [<span data-ttu-id="77f84-495">ASP.NET Core di debug remoto in IIS in Azure in Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="77f84-495">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="77f84-496">ASP.NET Core di debug remoto in un computer IIS remoto in Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="77f84-496">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="77f84-497">Informazioni sul debug tramite Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77f84-497">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="77f84-498">Documentazione di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77f84-498">Visual Studio Code documentation</span></span>

* <span data-ttu-id="77f84-499">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) (Debug con Visual Studio Code)</span><span class="sxs-lookup"><span data-stu-id="77f84-499">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)</span></span>
