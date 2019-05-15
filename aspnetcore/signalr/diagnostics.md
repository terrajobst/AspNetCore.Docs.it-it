---
title: Registrazione e diagnostica in ASP.NET Core SignalR
author: anurse
description: Informazioni su come raccogliere dati diagnostici dalle app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 02/27/2019
uid: signalr/diagnostics
ms.openlocfilehash: b6bd21314ed183488999bcff3553e53493537a11
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896888"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="879ad-103">Registrazione e diagnostica in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="879ad-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="879ad-104">Da [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="879ad-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="879ad-105">Questo articolo fornisce indicazioni per la raccolta di diagnostica dalla propria app ASP.NET Core SignalR per risolvere i problemi.</span><span class="sxs-lookup"><span data-stu-id="879ad-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="879ad-106">Registrazione sul lato server</span><span class="sxs-lookup"><span data-stu-id="879ad-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="879ad-107">File di log lato server possono contenere informazioni riservate dall'app.</span><span class="sxs-lookup"><span data-stu-id="879ad-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="879ad-108">**Mai** invio di log non elaborati dalle app di produzione ai forum pubblico come GitHub.</span><span class="sxs-lookup"><span data-stu-id="879ad-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="879ad-109">Poiché SignalR fa parte di ASP.NET Core, Usa il sistema di registrazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="879ad-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="879ad-110">Nella configurazione predefinita, SignalR registra informazioni molto piccolo, ma questo può configurato.</span><span class="sxs-lookup"><span data-stu-id="879ad-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="879ad-111">Vedere la documentazione sul [registrazione di ASP.NET Core](xref:fundamentals/logging/index#configuration) per informazioni dettagliate sulla configurazione della registrazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="879ad-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="879ad-112">SignalR utilizza due categorie di logger:</span><span class="sxs-lookup"><span data-stu-id="879ad-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="879ad-113">`Microsoft.AspNetCore.SignalR` -per i log relativi ai protocolli di Hub, attivando l'hub, richiamo di metodi e le altre attività correlati all'Hub.</span><span class="sxs-lookup"><span data-stu-id="879ad-113">`Microsoft.AspNetCore.SignalR` - for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="879ad-114">`Microsoft.AspNetCore.Http.Connections` -per i log relativi ai trasporti, ad esempio WebSocket, di Polling lungo Server-Sent eventi e a basso livello infrastruttura SignalR.</span><span class="sxs-lookup"><span data-stu-id="879ad-114">`Microsoft.AspNetCore.Http.Connections` - for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="879ad-115">Per abilitare i log dettagliati da SignalR, configurare entrambi i prefissi precedenti per il `Debug` a livello del `appsettings.json` aggiungendo quanto segue al file il `LogLevel` sottosezione `Logging`:</span><span class="sxs-lookup"><span data-stu-id="879ad-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your `appsettings.json` file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[Configuring logging](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="879ad-116">È anche possibile configurare questo nel codice nel `CreateWebHostBuilder` metodo:</span><span class="sxs-lookup"><span data-stu-id="879ad-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[Configuring logging in code](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="879ad-117">Se non si usa la configurazione basata su JSON, impostare i valori di configurazione seguenti nel sistema di configurazione:</span><span class="sxs-lookup"><span data-stu-id="879ad-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="879ad-118">Consultare la documentazione per il sistema di configurazione determinare come specificare i valori di configurazione annidata.</span><span class="sxs-lookup"><span data-stu-id="879ad-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="879ad-119">Ad esempio, quando si usano le variabili di ambiente, due `_` vengono utilizzati caratteri anziché il `:` (ad esempio: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span><span class="sxs-lookup"><span data-stu-id="879ad-119">For example, when using environment variables, two `_` characters are used instead of the `:` (such as: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="879ad-120">È consigliabile usare il `Debug` livello per la raccolta di informazioni dettagliate di diagnostica per l'app.</span><span class="sxs-lookup"><span data-stu-id="879ad-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="879ad-121">Il `Trace` livello produce diagnostica di livello molto basso e raramente è necessario per diagnosticare i problemi nell'app.</span><span class="sxs-lookup"><span data-stu-id="879ad-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="879ad-122">Log lato server di accesso</span><span class="sxs-lookup"><span data-stu-id="879ad-122">Access server-side logs</span></span>

<span data-ttu-id="879ad-123">Modalità di accesso a file di log lato server dipende dall'ambiente in cui in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="879ad-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="879ad-124">Come un'app console all'esterno di IIS</span><span class="sxs-lookup"><span data-stu-id="879ad-124">As a console app outside IIS</span></span>

<span data-ttu-id="879ad-125">Se si esegue in un'app console, il [logger di Console](xref:fundamentals/logging/index#console-provider) deve essere abilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="879ad-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="879ad-126">SignalR log saranno visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="879ad-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="879ad-127">All'interno di IIS Express da Visual Studio</span><span class="sxs-lookup"><span data-stu-id="879ad-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="879ad-128">Viene visualizzato l'output del log in Visual Studio il **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="879ad-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="879ad-129">Selezionare il **ASP.NET Core Web Server** elenco a discesa di opzione.</span><span class="sxs-lookup"><span data-stu-id="879ad-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="879ad-130">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="879ad-130">Azure App Service</span></span>

<span data-ttu-id="879ad-131">Abilitare l'opzione "Registrazione applicazioni (file System)" nella sezione "Log di diagnostica" del portale del servizio App di Azure e configurare il livello a `Verbose`.</span><span class="sxs-lookup"><span data-stu-id="879ad-131">Enable the "Application Logging (Filesystem)" option in the "Diagnostics logs" section of the Azure App Service portal and configure the Level to `Verbose`.</span></span> <span data-ttu-id="879ad-132">I log devono essere disponibili da "Lo streaming dei Log" anche il servizio, come i log nel file system del servizio App.</span><span class="sxs-lookup"><span data-stu-id="879ad-132">Logs should be available from the "Log streaming" service, as well as in logs on the file system of your App Service.</span></span> <span data-ttu-id="879ad-133">Per altre informazioni, vedere la documentazione sul [lo streaming dei log di Azure](xref:fundamentals/logging/index#azure-log-streaming).</span><span class="sxs-lookup"><span data-stu-id="879ad-133">For more information, see the documentation on [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="879ad-134">Altri ambienti</span><span class="sxs-lookup"><span data-stu-id="879ad-134">Other environments</span></span>

<span data-ttu-id="879ad-135">Se si esegue in un altro ambiente (Docker, Kubernetes, servizio di Windows, e così via), vedere la documentazione completa sul [registrazione di ASP.NET Core](xref:fundamentals/logging/index) per altre informazioni su come configurare i provider di registrazione adatti al proprio ambiente.</span><span class="sxs-lookup"><span data-stu-id="879ad-135">If you're running in another environment (Docker, Kubernetes, Windows Service, etc.), see the full documentation on [ASP.NET Core Logging](xref:fundamentals/logging/index) for more information on how to configure logging providers suitable to your environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="879ad-136">La registrazione del client JavaScript</span><span class="sxs-lookup"><span data-stu-id="879ad-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="879ad-137">File di log lato client possono contenere informazioni riservate dall'app.</span><span class="sxs-lookup"><span data-stu-id="879ad-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="879ad-138">**Mai** invio di log non elaborati dalle app di produzione ai forum pubblico come GitHub.</span><span class="sxs-lookup"><span data-stu-id="879ad-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="879ad-139">Quando si usa il client JavaScript, è possibile configurare le opzioni di registrazione usando il `configureLogging` metodo `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="879ad-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[Configuring logging in the JavaScript client](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="879ad-140">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nella `configureLogging` (metodo).</span><span class="sxs-lookup"><span data-stu-id="879ad-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="879ad-141">La tabella seguente illustra i livelli di log disponibili per il client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="879ad-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="879ad-142">Impostazione del livello di log a uno di questi valori consente la registrazione a tale livello e tutti i livelli superiori nella tabella.</span><span class="sxs-lookup"><span data-stu-id="879ad-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="879ad-143">Livello</span><span class="sxs-lookup"><span data-stu-id="879ad-143">Level</span></span> | <span data-ttu-id="879ad-144">Descrizione</span><span class="sxs-lookup"><span data-stu-id="879ad-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="879ad-145">Viene registrato alcun messaggio.</span><span class="sxs-lookup"><span data-stu-id="879ad-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="879ad-146">Messaggi che indicano un errore nell'intera app.</span><span class="sxs-lookup"><span data-stu-id="879ad-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="879ad-147">Messaggi che indicano un errore dell'operazione corrente.</span><span class="sxs-lookup"><span data-stu-id="879ad-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="879ad-148">Messaggi che indicano un problema non grave.</span><span class="sxs-lookup"><span data-stu-id="879ad-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="879ad-149">Messaggi informativi.</span><span class="sxs-lookup"><span data-stu-id="879ad-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="879ad-150">Messaggi di diagnostica utili per il debug.</span><span class="sxs-lookup"><span data-stu-id="879ad-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="879ad-151">Messaggi di diagnostica molto dettagliati progettati per la diagnosi dei problemi specifici.</span><span class="sxs-lookup"><span data-stu-id="879ad-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="879ad-152">Dopo aver configurato il livello di dettaglio, il log verrà scritto per la Console del Browser (o di Output Standard in un'app NodeJS).</span><span class="sxs-lookup"><span data-stu-id="879ad-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="879ad-153">Se si desidera inviare i log a un sistema di registrazione personalizzato, è possibile fornire un oggetto JavaScript che implementa il `ILogger` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="879ad-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="879ad-154">È l'unico metodo che deve essere implementato `log`, che accetta il livello dell'evento e il messaggio associato all'evento.</span><span class="sxs-lookup"><span data-stu-id="879ad-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="879ad-155">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="879ad-155">For example:</span></span>

[!code-typescript[Creating a custom logger](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="879ad-156">La registrazione del client .NET</span><span class="sxs-lookup"><span data-stu-id="879ad-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="879ad-157">File di log lato client possono contenere informazioni riservate dall'app.</span><span class="sxs-lookup"><span data-stu-id="879ad-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="879ad-158">**Mai** invio di log non elaborati dalle app di produzione ai forum pubblico come GitHub.</span><span class="sxs-lookup"><span data-stu-id="879ad-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="879ad-159">Per ottenere i log del client .NET, è possibile usare la `ConfigureLogging` metodo `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="879ad-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="879ad-160">Questo procedimento funziona esattamente come le `ConfigureLogging` metodo sul `WebHostBuilder` e `HostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="879ad-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="879ad-161">È possibile configurare è usare gli stessi provider di registrazione in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="879ad-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="879ad-162">Tuttavia, è necessario installare e abilitare i pacchetti NuGet per il provider di registrazione singola manualmente.</span><span class="sxs-lookup"><span data-stu-id="879ad-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="879ad-163">Registrazione console</span><span class="sxs-lookup"><span data-stu-id="879ad-163">Console logging</span></span>

<span data-ttu-id="879ad-164">Per abilitare la registrazione della Console, aggiungere il [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="879ad-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="879ad-165">Usare quindi il `AddConsole` metodo per configurare il logger della console:</span><span class="sxs-lookup"><span data-stu-id="879ad-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[Configuring console logging in .NET client](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="879ad-166">La registrazione di finestra di output di debug</span><span class="sxs-lookup"><span data-stu-id="879ad-166">Debug output window logging</span></span>

<span data-ttu-id="879ad-167">È anche possibile configurare i log per visualizzare il **Output** finestra in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="879ad-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="879ad-168">Installare il [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pacchetto e usare il `AddDebug` metodo:</span><span class="sxs-lookup"><span data-stu-id="879ad-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[Configuring debug output window logging in .NET client](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="879ad-169">Altri provider di registrazione</span><span class="sxs-lookup"><span data-stu-id="879ad-169">Other logging providers</span></span>

<span data-ttu-id="879ad-170">SignalR supporta altri provider di registrazione, ad esempio Serilog, Seq, NLog o qualsiasi altro sistema di registrazione che si integra con `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="879ad-170">SignalR supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="879ad-171">Se il sistema di registrazione fornisce un' `ILoggerProvider`, è possibile registrarlo con `AddProvider`:</span><span class="sxs-lookup"><span data-stu-id="879ad-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[Configuring a custom logging provider in .NET client](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="879ad-172">Livello di dettaglio di controllo</span><span class="sxs-lookup"><span data-stu-id="879ad-172">Control verbosity</span></span>

<span data-ttu-id="879ad-173">Se si sta eseguendo l'app da altre posizioni, modificare il livello predefinito da `Debug` potrebbe essere troppo dettagliata.</span><span class="sxs-lookup"><span data-stu-id="879ad-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="879ad-174">È possibile usare un filtro per configurare il livello di registrazione per i log di SignalR.</span><span class="sxs-lookup"><span data-stu-id="879ad-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="879ad-175">Questa operazione può essere eseguita nel codice, in gran parte esattamente come nel server:</span><span class="sxs-lookup"><span data-stu-id="879ad-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="879ad-176">Tracce di rete</span><span class="sxs-lookup"><span data-stu-id="879ad-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="879ad-177">Una traccia di rete include il contenuto completo di tutti i messaggi inviati dall'app.</span><span class="sxs-lookup"><span data-stu-id="879ad-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="879ad-178">**Mai** invio tracce di rete non elaborati dalle app di produzione ai forum pubblico come GitHub.</span><span class="sxs-lookup"><span data-stu-id="879ad-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="879ad-179">Se si verifica un problema, una traccia di rete in alcuni casi può fornire numerose informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="879ad-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="879ad-180">Ciò è particolarmente utile se si desidera segnalare un problema in una pagina.</span><span class="sxs-lookup"><span data-stu-id="879ad-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="879ad-181">Raccogliere una traccia di rete con Fiddler (opzione consigliata)</span><span class="sxs-lookup"><span data-stu-id="879ad-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="879ad-182">Questo metodo funziona per tutte le app.</span><span class="sxs-lookup"><span data-stu-id="879ad-182">This method works for all apps.</span></span>

<span data-ttu-id="879ad-183">Fiddler è uno strumento molto potente per raccogliere le tracce HTTP.</span><span class="sxs-lookup"><span data-stu-id="879ad-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="879ad-184">Installare l'app da [telerik.com/fiddler](https://www.telerik.com/fiddler), avviarla, quindi eseguire l'app e riprodurre il problema.</span><span class="sxs-lookup"><span data-stu-id="879ad-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="879ad-185">Fiddler è disponibile per Windows e sono disponibili versioni beta per macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="879ad-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="879ad-186">Se ci si connette tramite HTTPS, esistono alcuni passaggi aggiuntivi per garantire che Fiddler in grado di decrittografare il traffico HTTPS.</span><span class="sxs-lookup"><span data-stu-id="879ad-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="879ad-187">Per altre informazioni, vedere la [documentazione di Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span><span class="sxs-lookup"><span data-stu-id="879ad-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="879ad-188">Dopo aver raccolto la traccia, è possibile esportare la traccia, scegliendo **File** > **salvare** > **tutte le sessioni...**  dalla barra dei menu.</span><span class="sxs-lookup"><span data-stu-id="879ad-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions...** from the menu bar.</span></span>

![Esportazione di tutte le sessioni da Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="879ad-190">Raccogliere una traccia di rete con tcpdump (macOS e Linux solo)</span><span class="sxs-lookup"><span data-stu-id="879ad-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="879ad-191">Questo metodo funziona per tutte le app.</span><span class="sxs-lookup"><span data-stu-id="879ad-191">This method works for all apps.</span></span>

<span data-ttu-id="879ad-192">È possibile raccogliere le tracce TCP non elaborate usando tcpdump eseguendo il comando seguente da una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="879ad-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="879ad-193">Potrebbe essere necessario essere `root` o un prefisso con il comando `sudo` se si verifica un errore di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="879ad-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="879ad-194">Sostituire `[interface]` con l'interfaccia di rete che si desidera acquisire in.</span><span class="sxs-lookup"><span data-stu-id="879ad-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="879ad-195">Si tratta in genere simile `/dev/eth0` (per l'interfaccia Ethernet standard) o `/dev/lo0` (per il traffico di localhost).</span><span class="sxs-lookup"><span data-stu-id="879ad-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="879ad-196">Per altre informazioni, vedere il `tcpdump` pagina di manuale nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="879ad-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="879ad-197">Raccogliere una traccia di rete nel browser</span><span class="sxs-lookup"><span data-stu-id="879ad-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="879ad-198">Questo metodo funziona solo per le app basate su browser.</span><span class="sxs-lookup"><span data-stu-id="879ad-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="879ad-199">La maggior parte degli strumenti di sviluppo del browser aperta una scheda "Rete" che consente di acquisire attività di rete tra il browser e il server.</span><span class="sxs-lookup"><span data-stu-id="879ad-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="879ad-200">Tuttavia, queste tracce non includono i messaggi di evento Server-Sent e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="879ad-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="879ad-201">Se si utilizza questi trasporti, usando uno strumento come Fiddler o TcpDump (descritti di seguito) è un approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="879ad-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="879ad-202">Microsoft Edge e Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="879ad-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="879ad-203">(Le istruzioni sono le stesse sia per Microsoft Edge che per Internet Explorer)</span><span class="sxs-lookup"><span data-stu-id="879ad-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="879ad-204">Premere F12 per aprire gli strumenti di sviluppo</span><span class="sxs-lookup"><span data-stu-id="879ad-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="879ad-205">Fare clic sulla scheda di rete</span><span class="sxs-lookup"><span data-stu-id="879ad-205">Click the Network Tab</span></span>
3. <span data-ttu-id="879ad-206">Aggiornare la pagina, se necessario e riprodurre il problema</span><span class="sxs-lookup"><span data-stu-id="879ad-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="879ad-207">Fare clic sull'icona Salva nella barra degli strumenti per esportare la traccia come "HAR" file:</span><span class="sxs-lookup"><span data-stu-id="879ad-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![Il salvataggio degli strumenti della scheda di rete sull'icona allo sviluppo di Microsoft Edge](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="879ad-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="879ad-209">Google Chrome</span></span>

1. <span data-ttu-id="879ad-210">Premere F12 per aprire gli strumenti di sviluppo</span><span class="sxs-lookup"><span data-stu-id="879ad-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="879ad-211">Fare clic sulla scheda di rete</span><span class="sxs-lookup"><span data-stu-id="879ad-211">Click the Network Tab</span></span>
3. <span data-ttu-id="879ad-212">Aggiornare la pagina, se necessario e riprodurre il problema</span><span class="sxs-lookup"><span data-stu-id="879ad-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="879ad-213">Fare clic in un punto qualsiasi nell'elenco delle richieste e scegliere "Salva come HAR con contenuto":</span><span class="sxs-lookup"><span data-stu-id="879ad-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Opzione "Salva come HAR con contenuto" nella scheda di rete di Google Chrome Dev Tools](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="879ad-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="879ad-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="879ad-216">Premere F12 per aprire gli strumenti di sviluppo</span><span class="sxs-lookup"><span data-stu-id="879ad-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="879ad-217">Fare clic sulla scheda di rete</span><span class="sxs-lookup"><span data-stu-id="879ad-217">Click the Network Tab</span></span>
3. <span data-ttu-id="879ad-218">Aggiornare la pagina, se necessario e riprodurre il problema</span><span class="sxs-lookup"><span data-stu-id="879ad-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="879ad-219">Fare clic in un punto qualsiasi nell'elenco delle richieste e scegliere "Salva tutto come HAR"</span><span class="sxs-lookup"><span data-stu-id="879ad-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Opzione "Salva tutto come HAR" nella scheda di rete strumenti di sviluppo di Mozilla Firefox](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="879ad-221">Collegare i file di diagnostica per problemi di GitHub</span><span class="sxs-lookup"><span data-stu-id="879ad-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="879ad-222">È possibile collegare i file di diagnostica per problemi di GitHub rinominandoli pertanto hanno un `.txt` estensione e quindi trascinarli e rilasciarli al problema.</span><span class="sxs-lookup"><span data-stu-id="879ad-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="879ad-223">Problema di GitHub, non incollare il contenuto del file di log o le tracce di rete.</span><span class="sxs-lookup"><span data-stu-id="879ad-223">Please don't paste the content of log files or network traces in GitHub issue.</span></span> <span data-ttu-id="879ad-224">Questi log e tracce possono essere piuttosto grande e GitHub in genere non visualizzarle.</span><span class="sxs-lookup"><span data-stu-id="879ad-224">These logs and traces can be quite large and GitHub will usually truncate them.</span></span>

![Il trascinamento di file di log a un problema di GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="879ad-226">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="879ad-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
