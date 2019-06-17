---
title: Risolvere i problemi di ASP.NET Core in IIS
author: guardrex
description: Informazioni su come diagnosticare i problemi delle distribuzioni Internet Information Services (IIS) di app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/28/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: cb42a262c89c27fa350e936184f8ddb3a02788f0
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034747"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="075a7-103">Risolvere i problemi di ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="075a7-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="075a7-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="075a7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="075a7-105">In questo articolo vengono fornite istruzioni su come diagnosticare un problema di avvio di un'app ASP.NET Core durante l'hosting con [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="075a7-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="075a7-106">Le informazioni in questo articolo si applicano all'hosting in IIS su Windows Server e Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="075a7-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="075a7-107">In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug.</span><span class="sxs-lookup"><span data-stu-id="075a7-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="075a7-108">È possibile risolvere un codice *502.5 - Errore del processo* o *500.30 - Errore di avvio* che si verifica nel corso del debug in locale usando le indicazioni riportate in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="075a7-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="075a7-109">In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug.</span><span class="sxs-lookup"><span data-stu-id="075a7-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="075a7-110">È possibile risolvere un codice *502.5 Errore del processo* che si verifica nel corso del debug in locale usando le indicazioni riportate in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="075a7-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="075a7-111">Altri argomenti sulla risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="075a7-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="075a7-112"><xref:host-and-deploy/azure-apps/troubleshoot>Anche se Servizio app usa il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e IIS per ospitare le app, vedere l'argomento dedicato per istruzioni specifiche su Servizio app.</span><span class="sxs-lookup"><span data-stu-id="075a7-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="075a7-113"><xref:fundamentals/error-handling>Informazioni su come gestire gli errori nelle app ASP.NET Core durante lo sviluppo in un sistema locale.</span><span class="sxs-lookup"><span data-stu-id="075a7-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="075a7-114">[Informazioni su come eseguire il debug con Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) Questo argomento presenta le funzionalità del debugger di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="075a7-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="075a7-115">[Debug con Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Informazioni sul supporto del debug incorporato in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="075a7-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="075a7-116">Errori di avvio delle app</span><span class="sxs-lookup"><span data-stu-id="075a7-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="075a7-117">502.5 Errore del processo</span><span class="sxs-lookup"><span data-stu-id="075a7-117">502.5 Process Failure</span></span>

<span data-ttu-id="075a7-118">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="075a7-118">The worker process fails.</span></span> <span data-ttu-id="075a7-119">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="075a7-119">The app doesn't start.</span></span>

<span data-ttu-id="075a7-120">Il modulo ASP.NET Core prova ad avviare il processo dotnet back-end, ma l'avvio non riesce.</span><span class="sxs-lookup"><span data-stu-id="075a7-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="075a7-121">In genere è possibile determinare la causa di un errore di avvio del processo dalle voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="075a7-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="075a7-122">Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente.</span><span class="sxs-lookup"><span data-stu-id="075a7-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="075a7-123">Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="075a7-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="075a7-124">Il *framework condiviso* è il set di assembly (file *DLL*) che vengono installati nel computer e cui fa riferimento un metapacchetto, ad esempio `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="075a7-124">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="075a7-125">Il riferimento del metapacchetto può specificare una versione minima richiesta.</span><span class="sxs-lookup"><span data-stu-id="075a7-125">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="075a7-126">Per altre informazioni, vedere [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) (Il framework condiviso).</span><span class="sxs-lookup"><span data-stu-id="075a7-126">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="075a7-127">La pagina di errore *502.5 Errore del processo* viene restituita quando il processo di lavoro ha esito negativo a causa di un errore di configurazione dell'hosting o dell'app:</span><span class="sxs-lookup"><span data-stu-id="075a7-127">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Finestra del browser con la pagina 502.5 Errore del processo](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="075a7-129">500.30 Errore di avvio in-process</span><span class="sxs-lookup"><span data-stu-id="075a7-129">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="075a7-130">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="075a7-130">The worker process fails.</span></span> <span data-ttu-id="075a7-131">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="075a7-131">The app doesn't start.</span></span>

<span data-ttu-id="075a7-132">Il modulo ASP.NET Core prova ad avviare .NET Core CLR In-Process, ma l'avvio non riesce.</span><span class="sxs-lookup"><span data-stu-id="075a7-132">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="075a7-133">In genere è possibile determinare la causa di un errore di avvio del processo dalle voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="075a7-133">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="075a7-134">Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente.</span><span class="sxs-lookup"><span data-stu-id="075a7-134">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="075a7-135">Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="075a7-135">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="075a7-136">500.0 Errore di caricamento del gestore in-process</span><span class="sxs-lookup"><span data-stu-id="075a7-136">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="075a7-137">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="075a7-137">The worker process fails.</span></span> <span data-ttu-id="075a7-138">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="075a7-138">The app doesn't start.</span></span>

<span data-ttu-id="075a7-139">Il modulo ASP.NET Core non riesce a trovare .NET Core CLR e trova il gestore richieste In-Process (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="075a7-139">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="075a7-140">Controllare che:</span><span class="sxs-lookup"><span data-stu-id="075a7-140">Check that:</span></span>

* <span data-ttu-id="075a7-141">L'app specifichi come destinazione il pacchetto NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) o il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="075a7-141">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="075a7-142">Le versione del framework condiviso ASP.NET Core specificata come destinazione dall'app sia installata nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="075a7-142">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="075a7-143">500.0 Errore di caricamento del gestore out-of-process</span><span class="sxs-lookup"><span data-stu-id="075a7-143">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="075a7-144">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="075a7-144">The worker process fails.</span></span> <span data-ttu-id="075a7-145">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="075a7-145">The app doesn't start.</span></span>

<span data-ttu-id="075a7-146">Il modulo ASP.NET Core non riesce a trovare il gestore richieste di hosting out-of-process.</span><span class="sxs-lookup"><span data-stu-id="075a7-146">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="075a7-147">Verificare che *aspnetcorev2_outofprocess.dll* sia presente in una sottocartella accanto ad *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="075a7-147">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="075a7-148">500.31 ANCM Failed to Find Native Dependencies (500.31 Il modulo ASP.NET Core non è riuscito a trovare le dipendenze native)</span><span class="sxs-lookup"><span data-stu-id="075a7-148">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="075a7-149">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="075a7-149">The worker process fails.</span></span> <span data-ttu-id="075a7-150">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="075a7-150">The app doesn't start.</span></span>

<span data-ttu-id="075a7-151">Il modulo ASP.NET Core prova ad avviare il runtime di .NET Core In-Process, ma l'avvio non riesce.</span><span class="sxs-lookup"><span data-stu-id="075a7-151">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="075a7-152">La causa più comune di questo errore di avvio è la mancata installazione del runtime di `Microsoft.NETCore.App` o di `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="075a7-152">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="075a7-153">Se l'app viene distribuita per ASP.NET Core 3.0 e tale versione non esiste nel computer, si verifica questo errore.</span><span class="sxs-lookup"><span data-stu-id="075a7-153">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="075a7-154">Ecco un esempio di messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="075a7-154">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="075a7-155">Il messaggio di errore elenca tutte le versioni di .NET Core installate e la versione richiesta dall'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-155">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="075a7-156">Per correggere l'errore, eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="075a7-156">To fix this error, either:</span></span>

* <span data-ttu-id="075a7-157">Installare la versione appropriata di .NET Core nel computer.</span><span class="sxs-lookup"><span data-stu-id="075a7-157">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="075a7-158">Modificare l'app in modo che usi una versione di .NET Core presente nel computer.</span><span class="sxs-lookup"><span data-stu-id="075a7-158">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="075a7-159">Pubblicare l'app come [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="075a7-159">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="075a7-160">Durante l'esecuzione in fase di sviluppo (con la variabile di ambiente `ASPNETCORE_ENVIRONMENT` impostata su `Development`), l'errore specifico viene scritto nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="075a7-160">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="075a7-161">La causa di un errore di avvio del processo è disponibile anche nel [log eventi dell'applicazione](#application-event-log).</span><span class="sxs-lookup"><span data-stu-id="075a7-161">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="075a7-162">500.32 ANCM Failed to Load dll (500.32 Il modulo ASP.NET Core non è riuscito a caricare la DLL)</span><span class="sxs-lookup"><span data-stu-id="075a7-162">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="075a7-163">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="075a7-163">The worker process fails.</span></span> <span data-ttu-id="075a7-164">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="075a7-164">The app doesn't start.</span></span>

<span data-ttu-id="075a7-165">La causa più comune di questo errore è la pubblicazione dell'app per processori con architettura non compatibile.</span><span class="sxs-lookup"><span data-stu-id="075a7-165">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="075a7-166">Se il processo di lavoro è in esecuzione come app a 32 bit e l'app è stata pubblicata per un'architettura a 64 bit, si verifica questo errore.</span><span class="sxs-lookup"><span data-stu-id="075a7-166">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="075a7-167">Per correggere l'errore, eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="075a7-167">To fix this error, either:</span></span>

* <span data-ttu-id="075a7-168">Ripubblicare l'app per processori con la stessa architettura del processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="075a7-168">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="075a7-169">Pubblicare l'app come [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="075a7-169">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="075a7-170">500.33 ANCM Request Handler Load Failure (500.33 Errore di caricamento del gestore della richiesta del modulo ASP.Net Core)</span><span class="sxs-lookup"><span data-stu-id="075a7-170">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="075a7-171">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="075a7-171">The worker process fails.</span></span> <span data-ttu-id="075a7-172">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="075a7-172">The app doesn't start.</span></span>

<span data-ttu-id="075a7-173">L'app non fa riferimento al framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="075a7-173">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="075a7-174">Solo le app destinate al framework `Microsoft.AspNetCore.App` possono essere ospitate dal modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="075a7-174">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the ASP.NET Core Module.</span></span>

<span data-ttu-id="075a7-175">Per correggere questo errore, verificare che l'app sia destinata al framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="075a7-175">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="075a7-176">Controllare il framework di destinazione dell'app nel file `.runtimeconfig.json`.</span><span class="sxs-lookup"><span data-stu-id="075a7-176">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="075a7-177">500.34 ANCM Mixed Hosting Models Not Supported (500.34 Modelli di hosting misto non supportati nel modulo ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="075a7-177">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="075a7-178">Il processo di lavoro non è in grado di eseguire un'app In-Process e un'app Out-of-process nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="075a7-178">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="075a7-179">Per correggere questo errore, eseguire le app in pool di applicazioni IIS separati.</span><span class="sxs-lookup"><span data-stu-id="075a7-179">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="075a7-180">500.35 ANCM Multiple In-Process Applications in same Process (500.35 Modulo ASP.NET Core: più applicazioni In-Process nello stesso processo)</span><span class="sxs-lookup"><span data-stu-id="075a7-180">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="075a7-181">Il processo di lavoro non è in grado di eseguire un'app In-Process e un'app Out-of-process nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="075a7-181">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="075a7-182">Per correggere questo errore, eseguire le app in pool di applicazioni IIS separati.</span><span class="sxs-lookup"><span data-stu-id="075a7-182">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="075a7-183">500.36 ANCM Out-Of-Process Handler Load Failure (500.36 Modulo ASP.NET Core: Errore di caricamento del gestore Out-of-process)</span><span class="sxs-lookup"><span data-stu-id="075a7-183">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="075a7-184">Il gestore delle richieste Out-of-process, *aspnetcorev2_outofprocess.dll*, non è accanto al file *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="075a7-184">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="075a7-185">Questo indica un'installazione danneggiata del modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="075a7-185">This indicates a corrupted installation of the ASP.NET Core Module.</span></span>

<span data-ttu-id="075a7-186">Per correggere questo errore, riparare l'installazione del [bundle di hosting di .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (per IIS) o di Visual Studio (per IIS Express).</span><span class="sxs-lookup"><span data-stu-id="075a7-186">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="075a7-187">500.37 ANCM Failed to Start Within Startup Time Limit (500.37 Avvio del modulo ASP.NET Core non riuscito entro il limite di tempo stabilito)</span><span class="sxs-lookup"><span data-stu-id="075a7-187">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="075a7-188">Il modulo ASP.NET Core non è stato avviato entro il limite di tempo specificato.</span><span class="sxs-lookup"><span data-stu-id="075a7-188">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="075a7-189">Per impostazione predefinita, il timeout è di 120 secondi.</span><span class="sxs-lookup"><span data-stu-id="075a7-189">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="075a7-190">Questo errore può verificarsi quando si avvia un numero elevato di app nello stesso computer.</span><span class="sxs-lookup"><span data-stu-id="075a7-190">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="075a7-191">Controllare i picchi di utilizzo della CPU e della memoria nel server durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="075a7-191">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="075a7-192">Può essere necessario scaglionare il processo di avvio di più app.</span><span class="sxs-lookup"><span data-stu-id="075a7-192">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="075a7-193">500.30 Errore di avvio in-process</span><span class="sxs-lookup"><span data-stu-id="075a7-193">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="075a7-194">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="075a7-194">The worker process fails.</span></span> <span data-ttu-id="075a7-195">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="075a7-195">The app doesn't start.</span></span>

<span data-ttu-id="075a7-196">Il modulo ASP.NET Core prova ad avviare il runtime di .NET Core In-Process, ma l'avvio non riesce.</span><span class="sxs-lookup"><span data-stu-id="075a7-196">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="075a7-197">La causa di un errore di avvio di un processo viene in genere determinata osservando le voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="075a7-197">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="075a7-198">500.0 Errore di caricamento del gestore in-process</span><span class="sxs-lookup"><span data-stu-id="075a7-198">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="075a7-199">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="075a7-199">The worker process fails.</span></span> <span data-ttu-id="075a7-200">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="075a7-200">The app doesn't start.</span></span>

<span data-ttu-id="075a7-201">Si è verificato un errore sconosciuto durante il caricamento dei componenti del modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="075a7-201">An unknown error occurred loading ASP.NET Core Module components.</span></span> <span data-ttu-id="075a7-202">Eseguire una delle azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="075a7-202">Take one of the following actions:</span></span>

* <span data-ttu-id="075a7-203">Contattare il [supporto tecnico Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832). Selezionare **Strumenti di sviluppo** e quindi **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="075a7-203">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="075a7-204">Porre una domanda in Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="075a7-204">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="075a7-205">Segnalare il problema nel [repository GitHub](https://github.com/aspnet/AspNetCore) di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="075a7-205">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="075a7-206">500 Errore interno del server</span><span class="sxs-lookup"><span data-stu-id="075a7-206">500 Internal Server Error</span></span>

<span data-ttu-id="075a7-207">L'app viene avviata, ma un errore impedisce al server di soddisfare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="075a7-207">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="075a7-208">Questo errore si verifica all'interno del codice dell'app durante l'avvio o durante la creazione di una risposta.</span><span class="sxs-lookup"><span data-stu-id="075a7-208">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="075a7-209">La risposta potrebbe non avere alcun contenuto o essere visualizzata nel browser come un codice *500 Errore interno del server*.</span><span class="sxs-lookup"><span data-stu-id="075a7-209">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="075a7-210">Il log eventi dell'applicazione in genere indica che l'app è stata avviata normalmente.</span><span class="sxs-lookup"><span data-stu-id="075a7-210">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="075a7-211">Dal punto di vista del server, questo è corretto.</span><span class="sxs-lookup"><span data-stu-id="075a7-211">From the server's perspective, that's correct.</span></span> <span data-ttu-id="075a7-212">L'app è stata effettivamente avviata, ma non può generare una risposta valida.</span><span class="sxs-lookup"><span data-stu-id="075a7-212">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="075a7-213">Per risolvere il problema, [eseguire l'app dal prompt dei comandi](#run-the-app-at-a-command-prompt) nel server o [abilitare il log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="075a7-213">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="075a7-214">Non è stato possibile avviare l'aggiornamento dell'applicazione. Errore: 0x800700c1</span><span class="sxs-lookup"><span data-stu-id="075a7-214">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="075a7-215">L'app non è stata avviata perché non è stato possibile caricarne l'assembly ( *.dll*).</span><span class="sxs-lookup"><span data-stu-id="075a7-215">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="075a7-216">Questo errore si verifica quando è presente una mancata corrispondenza del numero di bit tra l'app pubblicata e il processo w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="075a7-216">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="075a7-217">Verificare che l'impostazione del pool di app a 32 bit sia corretta:</span><span class="sxs-lookup"><span data-stu-id="075a7-217">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="075a7-218">Selezionare il pool di app in **Pool di applicazioni** di Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="075a7-218">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="075a7-219">Selezionare **Impostazioni avanzate** in **Modifica pool di applicazioni** nel pannello **Azioni**.</span><span class="sxs-lookup"><span data-stu-id="075a7-219">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="075a7-220">Impostare **Attiva applicazioni a 32 bit**:</span><span class="sxs-lookup"><span data-stu-id="075a7-220">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="075a7-221">Se si sta sviluppando un'app a 32 bit (x86), impostare il valore su `True`.</span><span class="sxs-lookup"><span data-stu-id="075a7-221">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="075a7-222">Se si sta sviluppando un'app a 64 bit (x64), impostare il valore su `False`.</span><span class="sxs-lookup"><span data-stu-id="075a7-222">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="075a7-223">Reimpostazione della connessione</span><span class="sxs-lookup"><span data-stu-id="075a7-223">Connection reset</span></span>

<span data-ttu-id="075a7-224">Se si verifica un errore dopo l'invio delle intestazioni, è troppo tardi perché il server possa inviare un codice **500 Errore interno del server** quando si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="075a7-224">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="075a7-225">Questo spesso accade quando si verifica un errore durante la serializzazione di oggetti complessi per una risposta.</span><span class="sxs-lookup"><span data-stu-id="075a7-225">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="075a7-226">Questo tipo di errore viene visualizzato come un errore di *reimpostazione della connessione* nel client.</span><span class="sxs-lookup"><span data-stu-id="075a7-226">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="075a7-227">La [registrazione delle applicazioni](xref:fundamentals/logging/index) può consentire di risolvere questi tipi di errori.</span><span class="sxs-lookup"><span data-stu-id="075a7-227">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="075a7-228">Limiti di avvio predefiniti</span><span class="sxs-lookup"><span data-stu-id="075a7-228">Default startup limits</span></span>

<span data-ttu-id="075a7-229">Il modulo ASP.NET Core è configurato con un valore predefinito *startupTimeLimit* di 120 secondi.</span><span class="sxs-lookup"><span data-stu-id="075a7-229">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="075a7-230">Quando si mantiene il valore predefinito, l'avvio di un'app potrebbe richiedere fino a due minuti prima che il modulo registri un errore del processo.</span><span class="sxs-lookup"><span data-stu-id="075a7-230">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="075a7-231">Per informazioni sulla configurazione del modulo, vedere [Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="075a7-231">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="075a7-232">Risolvere gli errori di avvio delle app</span><span class="sxs-lookup"><span data-stu-id="075a7-232">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="075a7-233">Abilitare il log di debug del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="075a7-233">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="075a7-234">Aggiungere le impostazioni del gestore seguenti al file dell'app *web.config* per abilitare i log di debug del modulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="075a7-234">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="075a7-235">Verificare che il percorso specificato per il log esista e che l'identità del pool di applicazioni abbia le autorizzazioni di scrittura nel percorso.</span><span class="sxs-lookup"><span data-stu-id="075a7-235">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="075a7-236">Registro eventi dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="075a7-236">Application Event Log</span></span>

<span data-ttu-id="075a7-237">Accedere al log eventi dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="075a7-237">Access the Application Event Log:</span></span>

1. <span data-ttu-id="075a7-238">Aprire il menu Start, cercare **Visualizzatore eventi** e quindi selezionare l'app **Visualizzatore eventi**.</span><span class="sxs-lookup"><span data-stu-id="075a7-238">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="075a7-239">In **Visualizzatore eventi** aprire il nodo **Registri di Windows**.</span><span class="sxs-lookup"><span data-stu-id="075a7-239">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="075a7-240">Selezionare **Applicazione** per aprire il log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="075a7-240">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="075a7-241">Cercare gli errori associati all'app in cui si è verificato il problema.</span><span class="sxs-lookup"><span data-stu-id="075a7-241">Search for errors associated with the failing app.</span></span> <span data-ttu-id="075a7-242">Gli errori presentano un valore *Modulo AspNetCore IIS* o *Modulo AspNetCore IIS Express* nella colonna *Origine*.</span><span class="sxs-lookup"><span data-stu-id="075a7-242">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="075a7-243">Eseguire l'app da un prompt dei comandi</span><span class="sxs-lookup"><span data-stu-id="075a7-243">Run the app at a command prompt</span></span>

<span data-ttu-id="075a7-244">Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="075a7-244">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="075a7-245">È possibile individuare la causa di alcuni errori eseguendo l'app da un prompt dei comandi nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="075a7-245">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="075a7-246">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="075a7-246">Framework-dependent deployment</span></span>

<span data-ttu-id="075a7-247">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="075a7-247">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="075a7-248">Da un prompt dei comandi passare alla cartella di distribuzione e avviare l'app eseguendo l'assembly dell'app con *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="075a7-248">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="075a7-249">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="075a7-249">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="075a7-250">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="075a7-250">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="075a7-251">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="075a7-251">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="075a7-252">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="075a7-252">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="075a7-253">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-253">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="075a7-254">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="075a7-254">Self-contained deployment</span></span>

<span data-ttu-id="075a7-255">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="075a7-255">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="075a7-256">Da un prompt dei comandi passare alla cartella di distribuzione e avviare il file eseguibile dell'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-256">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="075a7-257">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="075a7-257">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="075a7-258">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="075a7-258">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="075a7-259">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="075a7-259">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="075a7-260">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="075a7-260">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="075a7-261">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-261">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="075a7-262">Log stdout del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="075a7-262">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="075a7-263">Per abilitare e visualizzare i log stdout:</span><span class="sxs-lookup"><span data-stu-id="075a7-263">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="075a7-264">Passare alla cartella di distribuzione del sito nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="075a7-264">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="075a7-265">Se la cartella *logs* non è presente, creare la cartella.</span><span class="sxs-lookup"><span data-stu-id="075a7-265">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="075a7-266">Per istruzioni su come impostare MSBuild per la creazione automatica della cartella *logs* nella distribuzione, vedere l'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="075a7-266">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="075a7-267">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="075a7-267">Edit the *web.config* file.</span></span> <span data-ttu-id="075a7-268">Impostare **stdoutLogEnabled** su `true` e modificare il percorso di **stdoutLogFile** in modo da fare riferimento alla cartella *logs*, ad esempio `.\logs\stdout`.</span><span class="sxs-lookup"><span data-stu-id="075a7-268">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="075a7-269">`stdout` nel percorso è il prefisso del nome del file di log.</span><span class="sxs-lookup"><span data-stu-id="075a7-269">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="075a7-270">Un timestamp, l'ID del processo e l'estensione del file vengono aggiunti automaticamente al momento della creazione del log.</span><span class="sxs-lookup"><span data-stu-id="075a7-270">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="075a7-271">Usando `stdout` come prefisso del nome del file, un tipico file di log è denominato *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="075a7-271">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="075a7-272">Assicurarsi che l'identità del pool di applicazioni disponga delle autorizzazioni di scrittura per la cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="075a7-272">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="075a7-273">Salvare il file *web.config* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="075a7-273">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="075a7-274">Effettuare una richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-274">Make a request to the app.</span></span>
1. <span data-ttu-id="075a7-275">Passare alla cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="075a7-275">Navigate to the *logs* folder.</span></span> <span data-ttu-id="075a7-276">Trovare e aprire il log stdout più recente.</span><span class="sxs-lookup"><span data-stu-id="075a7-276">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="075a7-277">Esaminare il log per verificare se sono presenti errori.</span><span class="sxs-lookup"><span data-stu-id="075a7-277">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="075a7-278">Al termine della risoluzione dei problemi, disabilitare la registrazione stdout.</span><span class="sxs-lookup"><span data-stu-id="075a7-278">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="075a7-279">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="075a7-279">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="075a7-280">Impostare **stdoutLogEnabled** su `false`.</span><span class="sxs-lookup"><span data-stu-id="075a7-280">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="075a7-281">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="075a7-281">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="075a7-282">La mancata disabilitazione del log stdout può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="075a7-282">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="075a7-283">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="075a7-283">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="075a7-284">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="075a7-284">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="075a7-285">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="075a7-285">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="075a7-286">Abilitare la pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="075a7-286">Enable the Developer Exception Page</span></span>

<span data-ttu-id="075a7-287">La variabile di ambiente `ASPNETCORE_ENVIRONMENT` [ può essere aggiunta a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) per eseguire l'app nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="075a7-287">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="075a7-288">Purché l'ambiente non sia sottoposto a override durante l'avvio dell'app tramite `UseEnvironment` nel generatore di host, l'impostazione della variabile di ambiente consente di visualizzare la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) quando viene eseguita l'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-288">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="075a7-289">L'impostazione della variabile di ambiente per `ASPNETCORE_ENVIRONMENT` è consigliata solo per l'uso in server di gestione temporanea e test non esposti a Internet.</span><span class="sxs-lookup"><span data-stu-id="075a7-289">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="075a7-290">Al termine della risoluzione dei problemi, rimuovere la variabile di ambiente dal file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="075a7-290">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="075a7-291">Per informazioni sull'impostazione delle variabili di ambiente in *web.config*, vedere [elemento figlio environmentVariables di aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="075a7-291">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="075a7-292">Errori di avvio comuni</span><span class="sxs-lookup"><span data-stu-id="075a7-292">Common startup errors</span></span>

<span data-ttu-id="075a7-293">Vedere <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="075a7-293">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="075a7-294">Nell'argomento di riferimento è descritta la maggior parte dei problemi comuni che impediscono l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-294">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="075a7-295">Ottenere dati da un'app</span><span class="sxs-lookup"><span data-stu-id="075a7-295">Obtain data from an app</span></span>

<span data-ttu-id="075a7-296">Se un'app è in grado di rispondere alle richieste, è possibile ottenere dati sulle richieste, le connessioni e altri dati dall'app tramite middleware inline di terminale.</span><span class="sxs-lookup"><span data-stu-id="075a7-296">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="075a7-297">Per altre informazioni e codice di esempio, vedere <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="075a7-297">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="075a7-298">Creare un dump</span><span class="sxs-lookup"><span data-stu-id="075a7-298">Create a dump</span></span>

<span data-ttu-id="075a7-299">Un *dump* è uno snapshot della memoria del sistema e può essere utile per determinare la causa di un arresto anomalo dell'app, un errore di avvio o un'app lenta.</span><span class="sxs-lookup"><span data-stu-id="075a7-299">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="075a7-300">Arresto anomalo o eccezione di un'app</span><span class="sxs-lookup"><span data-stu-id="075a7-300">App crashes or encounters an exception</span></span>

<span data-ttu-id="075a7-301">Ottenere e analizzare un dump da [Segnalazione errori Windows](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="075a7-301">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="075a7-302">Creare una cartella per i file dump di arresto anomalo del sistema in `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="075a7-302">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="075a7-303">Il pool di app deve avere accesso in scrittura alla cartella.</span><span class="sxs-lookup"><span data-stu-id="075a7-303">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="075a7-304">Eseguire lo [script di PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="075a7-304">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="075a7-305">Se l'app usa il [modello di hosting in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="075a7-305">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="075a7-306">Se l'app usa il [modello di hosting out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="075a7-306">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="075a7-307">Eseguire l'app nelle condizioni che causano l'arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="075a7-307">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="075a7-308">Dopo che si è verificato l'arresto anomalo, eseguire lo [script di PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="075a7-308">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="075a7-309">Se l'app usa il [modello di hosting in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="075a7-309">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="075a7-310">Se l'app usa il [modello di hosting out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="075a7-310">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="075a7-311">Dopo l'arresto anomalo di un'app e la raccolta dei dump, l'app può terminare normalmente.</span><span class="sxs-lookup"><span data-stu-id="075a7-311">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="075a7-312">Lo script di PowerShell configura Segnalazione errori Windows per raccogliere fino a cinque dump per ogni app.</span><span class="sxs-lookup"><span data-stu-id="075a7-312">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="075a7-313">I dump di arresto anomalo del sistema potrebbero richiedere una grande quantità di spazio su disco (fino a diversi gigabyte ognuno).</span><span class="sxs-lookup"><span data-stu-id="075a7-313">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="075a7-314">L'app si blocca, si verifica un errore durante l'avvio o viene eseguita normalmente</span><span class="sxs-lookup"><span data-stu-id="075a7-314">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="075a7-315">Quando un'app si *blocca* (smette di rispondere ma senza arresto anomalo), si verifica un errore durante l'avvio o viene eseguita normalmente, vedere [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (File dump in modalità utente: scelta dello strumento migliore) per selezionare uno strumento appropriato per produrre il dump.</span><span class="sxs-lookup"><span data-stu-id="075a7-315">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="075a7-316">Analizzare il dump</span><span class="sxs-lookup"><span data-stu-id="075a7-316">Analyze the dump</span></span>

<span data-ttu-id="075a7-317">È possibile analizzare un dump usando diversi approcci.</span><span class="sxs-lookup"><span data-stu-id="075a7-317">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="075a7-318">Per altre informazioni, vedere [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analisi di un file dump in modalità utente).</span><span class="sxs-lookup"><span data-stu-id="075a7-318">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="075a7-319">Debug remoto</span><span class="sxs-lookup"><span data-stu-id="075a7-319">Remote debugging</span></span>

<span data-ttu-id="075a7-320">Vedere [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) (Eseguire il debug remoto di ASP.NET Core in un computer remoto con IIS in Visual Studio 2017) nella documentazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="075a7-320">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="075a7-321">Application Insights</span><span class="sxs-lookup"><span data-stu-id="075a7-321">Application Insights</span></span>

<span data-ttu-id="075a7-322">[Application Insights](/azure/application-insights/) fornisce dati di telemetria dalle app ospitate da IIS, inclusi gli errori di registrazione e le funzionalità di creazione di report.</span><span class="sxs-lookup"><span data-stu-id="075a7-322">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="075a7-323">Application Insights può segnalare solo gli errori che si verificano dopo l'avvio dell'app, quando diventano disponibili le funzionalità di registrazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-323">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="075a7-324">Per altre informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="075a7-324">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="075a7-325">Suggerimenti aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="075a7-325">Additional advice</span></span>

<span data-ttu-id="075a7-326">Talvolta in un'app funzionante si verifica un problema subito dopo l'aggiornamento di .NET Core SDK nelle versioni computer o pacchetto di sviluppo all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-326">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="075a7-327">In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali.</span><span class="sxs-lookup"><span data-stu-id="075a7-327">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="075a7-328">La maggior parte di questi problemi può essere risolta attenendosi alle istruzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="075a7-328">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="075a7-329">Eliminare le cartelle *bin* e *obj*.</span><span class="sxs-lookup"><span data-stu-id="075a7-329">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="075a7-330">Cancellare le cache dei pacchetti in *%UserProfile%\\.nuget\\packages* e *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="075a7-330">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="075a7-331">Ripristinare e ricompilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="075a7-331">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="075a7-332">Verificare che la distribuzione precedente nel server sia stata completamente eliminata prima di ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="075a7-332">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="075a7-333">Un modo pratico per cancellare le cache dei pacchetti consiste nell'eseguire `dotnet nuget locals all --clear` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="075a7-333">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="075a7-334">La cancellazione delle cache dei pacchetti può anche essere eseguita usando lo strumento [nuget.exe](https://www.nuget.org/downloads) ed eseguendo il comando `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="075a7-334">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="075a7-335">*nuget.exe* non è un'installazione inclusa con il sistema operativo desktop Windows e deve essere ottenuta separatamente dal [sito Web NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="075a7-335">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="075a7-336">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="075a7-336">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
