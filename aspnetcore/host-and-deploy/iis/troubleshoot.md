---
title: Risolvere i problemi di ASP.NET Core in IIS
author: guardrex
description: Informazioni su come diagnosticare i problemi delle distribuzioni Internet Information Services (IIS) di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 6d43057639ea88bb21ac66f2799062e06fffc530
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121687"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="25cdf-103">Risolvere i problemi di ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="25cdf-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="25cdf-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="25cdf-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="25cdf-105">In questo articolo vengono fornite istruzioni su come diagnosticare un problema di avvio di un'app ASP.NET Core durante l'hosting con [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="25cdf-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="25cdf-106">Le informazioni in questo articolo si applicano all'hosting in IIS su Windows Server e Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="25cdf-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="25cdf-107">In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug.</span><span class="sxs-lookup"><span data-stu-id="25cdf-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="25cdf-108">È possibile risolvere un codice *502.5 - Errore del processo* o *500.30 - Errore di avvio* che si verifica nel corso del debug in locale usando le indicazioni riportate in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="25cdf-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="25cdf-109">In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug.</span><span class="sxs-lookup"><span data-stu-id="25cdf-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="25cdf-110">È possibile risolvere un codice *502.5 Errore del processo* che si verifica nel corso del debug in locale usando le indicazioni riportate in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="25cdf-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="25cdf-111">Altri argomenti sulla risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="25cdf-111">Additional troubleshooting topics:</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="25cdf-112">Anche se Servizio app usa il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e IIS per ospitare le app, vedere l'argomento dedicato per istruzioni specifiche su Servizio app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-112">Although App Service uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="25cdf-113">Informazioni su come gestire gli errori nelle app ASP.NET Core durante lo sviluppo in un sistema locale.</span><span class="sxs-lookup"><span data-stu-id="25cdf-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="25cdf-114">Informazioni sul debug tramite Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25cdf-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="25cdf-115">Questo argomento presenta le funzionalità del debugger di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25cdf-115">This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="25cdf-116">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) (Debug con Visual Studio Code)</span><span class="sxs-lookup"><span data-stu-id="25cdf-116">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)</span></span>  
<span data-ttu-id="25cdf-117">Informazioni sul supporto del debug incorporato in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="25cdf-117">Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="25cdf-118">Errori di avvio delle app</span><span class="sxs-lookup"><span data-stu-id="25cdf-118">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="25cdf-119">502.5 Errore del processo</span><span class="sxs-lookup"><span data-stu-id="25cdf-119">502.5 Process Failure</span></span>

<span data-ttu-id="25cdf-120">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="25cdf-120">The worker process fails.</span></span> <span data-ttu-id="25cdf-121">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="25cdf-121">The app doesn't start.</span></span>

<span data-ttu-id="25cdf-122">Il modulo ASP.NET Core prova ad avviare il processo dotnet back-end, ma l'avvio non riesce.</span><span class="sxs-lookup"><span data-stu-id="25cdf-122">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="25cdf-123">In genere è possibile determinare la causa di un errore di avvio del processo dalle voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="25cdf-123">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="25cdf-124">Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente.</span><span class="sxs-lookup"><span data-stu-id="25cdf-124">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="25cdf-125">Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="25cdf-125">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

<span data-ttu-id="25cdf-126">La pagina di errore *502.5 Errore del processo* viene restituita quando il processo di lavoro ha esito negativo a causa di un errore di configurazione dell'hosting o dell'app:</span><span class="sxs-lookup"><span data-stu-id="25cdf-126">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Finestra del browser con la pagina 502.5 Errore del processo](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="25cdf-128">500.30 Errore di avvio in-process</span><span class="sxs-lookup"><span data-stu-id="25cdf-128">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="25cdf-129">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="25cdf-129">The worker process fails.</span></span> <span data-ttu-id="25cdf-130">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="25cdf-130">The app doesn't start.</span></span>

<span data-ttu-id="25cdf-131">Il modulo ASP.NET Core prova ad avviare .NET Core CLR In-Process, ma l'avvio non riesce.</span><span class="sxs-lookup"><span data-stu-id="25cdf-131">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="25cdf-132">In genere è possibile determinare la causa di un errore di avvio del processo dalle voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="25cdf-132">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="25cdf-133">Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente.</span><span class="sxs-lookup"><span data-stu-id="25cdf-133">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="25cdf-134">Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="25cdf-134">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="25cdf-135">500.0 Errore di caricamento del gestore in-process</span><span class="sxs-lookup"><span data-stu-id="25cdf-135">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="25cdf-136">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="25cdf-136">The worker process fails.</span></span> <span data-ttu-id="25cdf-137">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="25cdf-137">The app doesn't start.</span></span>

<span data-ttu-id="25cdf-138">Il modulo ASP.NET Core non riesce a trovare .NET Core CLR e trova il gestore richieste In-Process (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="25cdf-138">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="25cdf-139">Controllare che:</span><span class="sxs-lookup"><span data-stu-id="25cdf-139">Check that:</span></span>

* <span data-ttu-id="25cdf-140">L'app specifichi come destinazione il pacchetto NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) o il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="25cdf-140">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="25cdf-141">Le versione del framework condiviso ASP.NET Core specificata come destinazione dall'app sia installata nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="25cdf-141">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="25cdf-142">500.0 Errore di caricamento del gestore out-of-process</span><span class="sxs-lookup"><span data-stu-id="25cdf-142">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="25cdf-143">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="25cdf-143">The worker process fails.</span></span> <span data-ttu-id="25cdf-144">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="25cdf-144">The app doesn't start.</span></span>

<span data-ttu-id="25cdf-145">Il modulo ASP.NET Core non riesce a trovare il gestore richieste di hosting out-of-process.</span><span class="sxs-lookup"><span data-stu-id="25cdf-145">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="25cdf-146">Verificare che *aspnetcorev2_outofprocess.dll* sia presente in una sottocartella accanto ad *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-146">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span> 

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="25cdf-147">500 Errore interno del server</span><span class="sxs-lookup"><span data-stu-id="25cdf-147">500 Internal Server Error</span></span>

<span data-ttu-id="25cdf-148">L'app viene avviata, ma un errore impedisce al server di soddisfare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="25cdf-148">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="25cdf-149">Questo errore si verifica all'interno del codice dell'app durante l'avvio o durante la creazione di una risposta.</span><span class="sxs-lookup"><span data-stu-id="25cdf-149">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="25cdf-150">La risposta potrebbe non avere alcun contenuto o essere visualizzata nel browser come un codice *500 Errore interno del server*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-150">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="25cdf-151">Il log eventi dell'applicazione in genere indica che l'app è stata avviata normalmente.</span><span class="sxs-lookup"><span data-stu-id="25cdf-151">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="25cdf-152">Dal punto di vista del server, questo è corretto.</span><span class="sxs-lookup"><span data-stu-id="25cdf-152">From the server's perspective, that's correct.</span></span> <span data-ttu-id="25cdf-153">L'app è stata effettivamente avviata, ma non può generare una risposta valida.</span><span class="sxs-lookup"><span data-stu-id="25cdf-153">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="25cdf-154">Per risolvere il problema, [eseguire l'app dal prompt dei comandi](#run-the-app-at-a-command-prompt) nel server o [abilitare il log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="25cdf-154">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="25cdf-155">Non è stato possibile avviare l'aggiornamento dell'applicazione. Errore: 0x800700c1</span><span class="sxs-lookup"><span data-stu-id="25cdf-155">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="25cdf-156">L'app non è stata avviata perché non è stato possibile caricarne l'assembly (*.dll*).</span><span class="sxs-lookup"><span data-stu-id="25cdf-156">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="25cdf-157">Questo errore si verifica quando è presente una mancata corrispondenza del numero di bit tra l'app pubblicata e il processo w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="25cdf-157">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="25cdf-158">Verificare che l'impostazione del pool di app a 32 bit sia corretta:</span><span class="sxs-lookup"><span data-stu-id="25cdf-158">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="25cdf-159">Selezionare il pool di app in **Pool di applicazioni** di Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="25cdf-159">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="25cdf-160">Selezionare **Impostazioni avanzate** in **Modifica pool di applicazioni** nel pannello **Azioni**.</span><span class="sxs-lookup"><span data-stu-id="25cdf-160">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="25cdf-161">Impostare **Attiva applicazioni a 32 bit**:</span><span class="sxs-lookup"><span data-stu-id="25cdf-161">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="25cdf-162">Se si sta sviluppando un'app a 32 bit (x86), impostare il valore su `True`.</span><span class="sxs-lookup"><span data-stu-id="25cdf-162">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="25cdf-163">Se si sta sviluppando un'app a 64 bit (x64), impostare il valore su `False`.</span><span class="sxs-lookup"><span data-stu-id="25cdf-163">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="25cdf-164">Reimpostazione della connessione</span><span class="sxs-lookup"><span data-stu-id="25cdf-164">Connection reset</span></span>

<span data-ttu-id="25cdf-165">Se si verifica un errore dopo l'invio delle intestazioni, è troppo tardi perché il server possa inviare un codice **500 Errore interno del server** quando si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="25cdf-165">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="25cdf-166">Questo spesso accade quando si verifica un errore durante la serializzazione di oggetti complessi per una risposta.</span><span class="sxs-lookup"><span data-stu-id="25cdf-166">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="25cdf-167">Questo tipo di errore viene visualizzato come un errore di *reimpostazione della connessione* nel client.</span><span class="sxs-lookup"><span data-stu-id="25cdf-167">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="25cdf-168">La [registrazione delle applicazioni](xref:fundamentals/logging/index) può consentire di risolvere questi tipi di errori.</span><span class="sxs-lookup"><span data-stu-id="25cdf-168">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="25cdf-169">Limiti di avvio predefiniti</span><span class="sxs-lookup"><span data-stu-id="25cdf-169">Default startup limits</span></span>

<span data-ttu-id="25cdf-170">Il modulo ASP.NET Core è configurato con un valore predefinito *startupTimeLimit* di 120 secondi.</span><span class="sxs-lookup"><span data-stu-id="25cdf-170">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="25cdf-171">Quando si mantiene il valore predefinito, l'avvio di un'app potrebbe richiedere fino a due minuti prima che il modulo registri un errore del processo.</span><span class="sxs-lookup"><span data-stu-id="25cdf-171">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="25cdf-172">Per informazioni sulla configurazione del modulo, vedere [Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="25cdf-172">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="25cdf-173">Risolvere gli errori di avvio delle app</span><span class="sxs-lookup"><span data-stu-id="25cdf-173">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="25cdf-174">Abilitare il log di debug del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25cdf-174">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="25cdf-175">Aggiungere le impostazioni del gestore seguenti al file dell'app *web.config* per abilitare i log di debug del modulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="25cdf-175">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="25cdf-176">Verificare che il percorso specificato per il log esista e che l'identità del pool di applicazioni abbia le autorizzazioni di scrittura nel percorso.</span><span class="sxs-lookup"><span data-stu-id="25cdf-176">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="25cdf-177">Registro eventi dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="25cdf-177">Application Event Log</span></span>

<span data-ttu-id="25cdf-178">Accedere al log eventi dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="25cdf-178">Access the Application Event Log:</span></span>

1. <span data-ttu-id="25cdf-179">Aprire il menu Start, cercare **Visualizzatore eventi** e quindi selezionare l'app **Visualizzatore eventi**.</span><span class="sxs-lookup"><span data-stu-id="25cdf-179">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="25cdf-180">In **Visualizzatore eventi** aprire il nodo **Registri di Windows**.</span><span class="sxs-lookup"><span data-stu-id="25cdf-180">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="25cdf-181">Selezionare **Applicazione** per aprire il log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="25cdf-181">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="25cdf-182">Cercare gli errori associati all'app in cui si è verificato il problema.</span><span class="sxs-lookup"><span data-stu-id="25cdf-182">Search for errors associated with the failing app.</span></span> <span data-ttu-id="25cdf-183">Gli errori presentano un valore *Modulo AspNetCore IIS* o *Modulo AspNetCore IIS Express* nella colonna *Origine*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-183">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="25cdf-184">Eseguire l'app da un prompt dei comandi</span><span class="sxs-lookup"><span data-stu-id="25cdf-184">Run the app at a command prompt</span></span>

<span data-ttu-id="25cdf-185">Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="25cdf-185">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="25cdf-186">È possibile individuare la causa di alcuni errori eseguendo l'app da un prompt dei comandi nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="25cdf-186">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="25cdf-187">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="25cdf-187">Framework-dependent deployment</span></span>

<span data-ttu-id="25cdf-188">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="25cdf-188">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="25cdf-189">Da un prompt dei comandi passare alla cartella di distribuzione e avviare l'app eseguendo l'assembly dell'app con *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-189">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="25cdf-190">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="25cdf-190">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="25cdf-191">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="25cdf-191">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="25cdf-192">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="25cdf-192">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="25cdf-193">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="25cdf-193">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="25cdf-194">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-194">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="25cdf-195">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="25cdf-195">Self-contained deployment</span></span>

<span data-ttu-id="25cdf-196">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="25cdf-196">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="25cdf-197">Da un prompt dei comandi passare alla cartella di distribuzione e avviare il file eseguibile dell'app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-197">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="25cdf-198">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="25cdf-198">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="25cdf-199">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="25cdf-199">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="25cdf-200">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="25cdf-200">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="25cdf-201">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="25cdf-201">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="25cdf-202">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-202">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="25cdf-203">Log stdout del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25cdf-203">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="25cdf-204">Per abilitare e visualizzare i log stdout:</span><span class="sxs-lookup"><span data-stu-id="25cdf-204">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="25cdf-205">Passare alla cartella di distribuzione del sito nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="25cdf-205">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="25cdf-206">Se la cartella *logs* non è presente, creare la cartella.</span><span class="sxs-lookup"><span data-stu-id="25cdf-206">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="25cdf-207">Per istruzioni su come impostare MSBuild per la creazione automatica della cartella *logs* nella distribuzione, vedere l'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="25cdf-207">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="25cdf-208">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-208">Edit the *web.config* file.</span></span> <span data-ttu-id="25cdf-209">Impostare **stdoutLogEnabled** su `true` e modificare il percorso di **stdoutLogFile** in modo da fare riferimento alla cartella *logs*, ad esempio `.\logs\stdout`.</span><span class="sxs-lookup"><span data-stu-id="25cdf-209">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="25cdf-210">`stdout` nel percorso è il prefisso del nome del file di log.</span><span class="sxs-lookup"><span data-stu-id="25cdf-210">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="25cdf-211">Un timestamp, l'ID del processo e l'estensione del file vengono aggiunti automaticamente al momento della creazione del log.</span><span class="sxs-lookup"><span data-stu-id="25cdf-211">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="25cdf-212">Usando `stdout` come prefisso del nome del file, un tipico file di log è denominato *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-212">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="25cdf-213">Assicurarsi che l'identità del pool di applicazioni disponga delle autorizzazioni di scrittura per la cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-213">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="25cdf-214">Salvare il file *web.config* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="25cdf-214">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="25cdf-215">Effettuare una richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-215">Make a request to the app.</span></span>
1. <span data-ttu-id="25cdf-216">Passare alla cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-216">Navigate to the *logs* folder.</span></span> <span data-ttu-id="25cdf-217">Trovare e aprire il log stdout più recente.</span><span class="sxs-lookup"><span data-stu-id="25cdf-217">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="25cdf-218">Esaminare il log per verificare se sono presenti errori.</span><span class="sxs-lookup"><span data-stu-id="25cdf-218">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25cdf-219">Al termine della risoluzione dei problemi, disabilitare la registrazione stdout.</span><span class="sxs-lookup"><span data-stu-id="25cdf-219">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="25cdf-220">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-220">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="25cdf-221">Impostare **stdoutLogEnabled** su `false`.</span><span class="sxs-lookup"><span data-stu-id="25cdf-221">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="25cdf-222">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="25cdf-222">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="25cdf-223">La mancata disabilitazione del log stdout può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="25cdf-223">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="25cdf-224">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="25cdf-224">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="25cdf-225">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="25cdf-225">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="25cdf-226">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="25cdf-226">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="25cdf-227">Abilitare la pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="25cdf-227">Enable the Developer Exception Page</span></span>

<span data-ttu-id="25cdf-228">La variabile di ambiente `ASPNETCORE_ENVIRONMENT` [ può essere aggiunta a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) per eseguire l'app nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="25cdf-228">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="25cdf-229">Purché l'ambiente non sia sottoposto a override durante l'avvio dell'app tramite `UseEnvironment` nel generatore di host, l'impostazione della variabile di ambiente consente di visualizzare la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) quando viene eseguita l'app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-229">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="25cdf-230">L'impostazione della variabile di ambiente per `ASPNETCORE_ENVIRONMENT` è consigliata solo per l'uso in server di gestione temporanea e test non esposti a Internet.</span><span class="sxs-lookup"><span data-stu-id="25cdf-230">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="25cdf-231">Al termine della risoluzione dei problemi, rimuovere la variabile di ambiente dal file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-231">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="25cdf-232">Per informazioni sull'impostazione delle variabili di ambiente in *web.config*, vedere [elemento figlio environmentVariables di aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="25cdf-232">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="25cdf-233">Errori di avvio comuni</span><span class="sxs-lookup"><span data-stu-id="25cdf-233">Common startup errors</span></span>

<span data-ttu-id="25cdf-234">Vedere <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="25cdf-234">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="25cdf-235">Nell'argomento di riferimento è descritta la maggior parte dei problemi comuni che impediscono l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-235">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="25cdf-236">Ottenere dati da un'app</span><span class="sxs-lookup"><span data-stu-id="25cdf-236">Obtain data from an app</span></span>

<span data-ttu-id="25cdf-237">Se un'app è in grado di rispondere alle richieste, è possibile ottenere dati sulle richieste, le connessioni e altri dati dall'app tramite middleware inline di terminale.</span><span class="sxs-lookup"><span data-stu-id="25cdf-237">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="25cdf-238">Per altre informazioni e codice di esempio, vedere <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="25cdf-238">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="25cdf-239">App lenta o bloccata</span><span class="sxs-lookup"><span data-stu-id="25cdf-239">Slow or hanging app</span></span>

<span data-ttu-id="25cdf-240">Quando un'app risponde lentamente o si blocca durante una richiesta, ottenere e analizzare un [file dump](/visualstudio/debugger/using-dump-files).</span><span class="sxs-lookup"><span data-stu-id="25cdf-240">When an app responds slowly or hangs on a request, obtain and analyze a [dump file](/visualstudio/debugger/using-dump-files).</span></span> <span data-ttu-id="25cdf-241">I file dump possono essere ottenuti usando uno degli strumenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="25cdf-241">Dump files can be obtained using any of the following tools:</span></span>

* [<span data-ttu-id="25cdf-242">ProcDump</span><span class="sxs-lookup"><span data-stu-id="25cdf-242">ProcDump</span></span>](/sysinternals/downloads/procdump)
* [<span data-ttu-id="25cdf-243">DebugDiag</span><span class="sxs-lookup"><span data-stu-id="25cdf-243">DebugDiag</span></span>](https://www.microsoft.com/download/details.aspx?id=49924)
* <span data-ttu-id="25cdf-244">WinDbg: [download degli strumenti di debug per Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [debug tramite WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span><span class="sxs-lookup"><span data-stu-id="25cdf-244">WinDbg: [Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="25cdf-245">Debug remoto</span><span class="sxs-lookup"><span data-stu-id="25cdf-245">Remote debugging</span></span>

<span data-ttu-id="25cdf-246">Vedere [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) (Eseguire il debug remoto di ASP.NET Core in un computer remoto con IIS in Visual Studio 2017) nella documentazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25cdf-246">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="25cdf-247">Application Insights</span><span class="sxs-lookup"><span data-stu-id="25cdf-247">Application Insights</span></span>

<span data-ttu-id="25cdf-248">[Application Insights](/azure/application-insights/) fornisce dati di telemetria dalle app ospitate da IIS, inclusi gli errori di registrazione e le funzionalità di creazione di report.</span><span class="sxs-lookup"><span data-stu-id="25cdf-248">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="25cdf-249">Application Insights può segnalare solo gli errori che si verificano dopo l'avvio dell'app, quando diventano disponibili le funzionalità di registrazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-249">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="25cdf-250">Per altre informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="25cdf-250">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="25cdf-251">Suggerimenti aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="25cdf-251">Additional advice</span></span>

<span data-ttu-id="25cdf-252">Talvolta in un'app funzionante si verifica un problema subito dopo l'aggiornamento di .NET Core SDK nelle versioni computer o pacchetto di sviluppo all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-252">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="25cdf-253">In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali.</span><span class="sxs-lookup"><span data-stu-id="25cdf-253">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="25cdf-254">La maggior parte di questi problemi può essere risolta attenendosi alle istruzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="25cdf-254">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="25cdf-255">Eliminare le cartelle *bin* e *obj*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-255">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="25cdf-256">Cancellare le cache dei pacchetti in *%UserProfile%\\.nuget\\packages* e *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="25cdf-256">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="25cdf-257">Ripristinare e ricompilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="25cdf-257">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="25cdf-258">Verificare che la distribuzione precedente nel server sia stata completamente eliminata prima di ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="25cdf-258">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="25cdf-259">Un modo pratico per cancellare le cache dei pacchetti consiste nell'eseguire `dotnet nuget locals all --clear` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="25cdf-259">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="25cdf-260">La cancellazione delle cache dei pacchetti può anche essere eseguita usando lo strumento [nuget.exe](https://www.nuget.org/downloads) ed eseguendo il comando `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="25cdf-260">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="25cdf-261">*nuget.exe* non è un'installazione inclusa con il sistema operativo desktop Windows e deve essere ottenuta separatamente dal [sito Web NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="25cdf-261">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25cdf-262">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="25cdf-262">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
