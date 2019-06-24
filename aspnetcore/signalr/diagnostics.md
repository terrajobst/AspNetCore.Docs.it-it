---
title: Registrazione e diagnostica in ASP.NET Core SignalR
author: anurse
description: Informazioni su come raccogliere dati diagnostici dalle app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 06/19/2019
uid: signalr/diagnostics
ms.openlocfilehash: 69dbd057b3dcadeb3ca5d94ede1234530fb447db
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313701"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="70dce-103">Registrazione e diagnostica in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="70dce-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="70dce-104">Da [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="70dce-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="70dce-105">Questo articolo fornisce indicazioni per la raccolta di diagnostica dalla propria app ASP.NET Core SignalR per risolvere i problemi.</span><span class="sxs-lookup"><span data-stu-id="70dce-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="70dce-106">Registrazione sul lato server</span><span class="sxs-lookup"><span data-stu-id="70dce-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="70dce-107">File di log lato server possono contenere informazioni riservate dall'app.</span><span class="sxs-lookup"><span data-stu-id="70dce-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="70dce-108">**Mai** invio di log non elaborati dalle app di produzione ai forum pubblico come GitHub.</span><span class="sxs-lookup"><span data-stu-id="70dce-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="70dce-109">Poiché SignalR fa parte di ASP.NET Core, Usa il sistema di registrazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70dce-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="70dce-110">Nella configurazione predefinita, SignalR registra informazioni molto piccolo, ma questo può configurato.</span><span class="sxs-lookup"><span data-stu-id="70dce-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="70dce-111">Vedere la documentazione sul [registrazione di ASP.NET Core](xref:fundamentals/logging/index#configuration) per informazioni dettagliate sulla configurazione della registrazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70dce-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="70dce-112">SignalR utilizza due categorie di logger:</span><span class="sxs-lookup"><span data-stu-id="70dce-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="70dce-113">`Microsoft.AspNetCore.SignalR` &ndash; per i log relativi ai protocolli di Hub, attivando hub, richiamo di metodi e le altre attività correlati all'Hub.</span><span class="sxs-lookup"><span data-stu-id="70dce-113">`Microsoft.AspNetCore.SignalR` &ndash; for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="70dce-114">`Microsoft.AspNetCore.Http.Connections` &ndash; per i log correlati ai trasporti, ad esempio WebSocket, di Polling lungo Server-Sent eventi e a basso livello infrastruttura SignalR.</span><span class="sxs-lookup"><span data-stu-id="70dce-114">`Microsoft.AspNetCore.Http.Connections` &ndash; for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="70dce-115">Per abilitare i log dettagliati da SignalR, configurare entrambi i prefissi precedenti per il `Debug` a livello del *appSettings. JSON* aggiungendo quanto segue al file il `LogLevel` sottosezione `Logging`:</span><span class="sxs-lookup"><span data-stu-id="70dce-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="70dce-116">È anche possibile configurare questo nel codice nel `CreateWebHostBuilder` metodo:</span><span class="sxs-lookup"><span data-stu-id="70dce-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="70dce-117">Se non si usa la configurazione basata su JSON, impostare i valori di configurazione seguenti nel sistema di configurazione:</span><span class="sxs-lookup"><span data-stu-id="70dce-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="70dce-118">Consultare la documentazione per il sistema di configurazione determinare come specificare i valori di configurazione annidata.</span><span class="sxs-lookup"><span data-stu-id="70dce-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="70dce-119">Ad esempio, quando si usano le variabili di ambiente, due `_` vengono utilizzati caratteri anziché il `:` (ad esempio, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span><span class="sxs-lookup"><span data-stu-id="70dce-119">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="70dce-120">È consigliabile usare il `Debug` livello per la raccolta di informazioni dettagliate di diagnostica per l'app.</span><span class="sxs-lookup"><span data-stu-id="70dce-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="70dce-121">Il `Trace` livello produce diagnostica di livello molto basso e raramente è necessario per diagnosticare i problemi nell'app.</span><span class="sxs-lookup"><span data-stu-id="70dce-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="70dce-122">Log lato server di accesso</span><span class="sxs-lookup"><span data-stu-id="70dce-122">Access server-side logs</span></span>

<span data-ttu-id="70dce-123">Modalità di accesso a file di log lato server dipende dall'ambiente in cui in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="70dce-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="70dce-124">Come un'app console all'esterno di IIS</span><span class="sxs-lookup"><span data-stu-id="70dce-124">As a console app outside IIS</span></span>

<span data-ttu-id="70dce-125">Se si esegue in un'app console, il [logger di Console](xref:fundamentals/logging/index#console-provider) deve essere abilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="70dce-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="70dce-126">SignalR log saranno visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="70dce-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="70dce-127">All'interno di IIS Express da Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70dce-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="70dce-128">Viene visualizzato l'output del log in Visual Studio il **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="70dce-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="70dce-129">Selezionare il **ASP.NET Core Web Server** elenco a discesa di opzione.</span><span class="sxs-lookup"><span data-stu-id="70dce-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="70dce-130">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="70dce-130">Azure App Service</span></span>

<span data-ttu-id="70dce-131">Abilitare la **registrazione applicazioni (file System)** opzione il **log di diagnostica** sezione del portale del servizio App di Azure e configurare il **a livello di** a `Verbose`.</span><span class="sxs-lookup"><span data-stu-id="70dce-131">Enable the **Application Logging (Filesystem)** option in the **Diagnostics logs** section of the Azure App Service portal and configure the **Level** to `Verbose`.</span></span> <span data-ttu-id="70dce-132">I log dovrebbero essere disponibili il **lo streaming dei Log** servizio e nei log nel file system del servizio App.</span><span class="sxs-lookup"><span data-stu-id="70dce-132">Logs should be available from the **Log streaming** service and in logs on the file system of the App Service.</span></span> <span data-ttu-id="70dce-133">Per altre informazioni, vedere [lo streaming dei log di Azure](xref:fundamentals/logging/index#azure-log-streaming).</span><span class="sxs-lookup"><span data-stu-id="70dce-133">For more information, see [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="70dce-134">Altri ambienti</span><span class="sxs-lookup"><span data-stu-id="70dce-134">Other environments</span></span>

<span data-ttu-id="70dce-135">Se l'app viene distribuita in un altro ambiente (ad esempio, Docker, Kubernetes o del servizio Windows), vedere <xref:fundamentals/logging/index> per altre informazioni su come configurare i provider di registrazione adatti per l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="70dce-135">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="70dce-136">La registrazione del client JavaScript</span><span class="sxs-lookup"><span data-stu-id="70dce-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="70dce-137">File di log lato client possono contenere informazioni riservate dall'app.</span><span class="sxs-lookup"><span data-stu-id="70dce-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="70dce-138">**Mai** invio di log non elaborati dalle app di produzione ai forum pubblico come GitHub.</span><span class="sxs-lookup"><span data-stu-id="70dce-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="70dce-139">Quando si usa il client JavaScript, è possibile configurare le opzioni di registrazione usando il `configureLogging` metodo `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="70dce-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="70dce-140">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nella `configureLogging` (metodo).</span><span class="sxs-lookup"><span data-stu-id="70dce-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="70dce-141">La tabella seguente illustra i livelli di log disponibili per il client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="70dce-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="70dce-142">Impostazione del livello di log a uno di questi valori consente la registrazione a tale livello e tutti i livelli superiori nella tabella.</span><span class="sxs-lookup"><span data-stu-id="70dce-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="70dce-143">Livello</span><span class="sxs-lookup"><span data-stu-id="70dce-143">Level</span></span> | <span data-ttu-id="70dce-144">Descrizione</span><span class="sxs-lookup"><span data-stu-id="70dce-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="70dce-145">Viene registrato alcun messaggio.</span><span class="sxs-lookup"><span data-stu-id="70dce-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="70dce-146">Messaggi che indicano un errore nell'intera app.</span><span class="sxs-lookup"><span data-stu-id="70dce-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="70dce-147">Messaggi che indicano un errore dell'operazione corrente.</span><span class="sxs-lookup"><span data-stu-id="70dce-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="70dce-148">Messaggi che indicano un problema non grave.</span><span class="sxs-lookup"><span data-stu-id="70dce-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="70dce-149">Messaggi informativi.</span><span class="sxs-lookup"><span data-stu-id="70dce-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="70dce-150">Messaggi di diagnostica utili per il debug.</span><span class="sxs-lookup"><span data-stu-id="70dce-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="70dce-151">Messaggi di diagnostica molto dettagliati progettati per la diagnosi dei problemi specifici.</span><span class="sxs-lookup"><span data-stu-id="70dce-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="70dce-152">Dopo aver configurato il livello di dettaglio, il log verrà scritto per la Console del Browser (o di Output Standard in un'app NodeJS).</span><span class="sxs-lookup"><span data-stu-id="70dce-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="70dce-153">Se si desidera inviare i log a un sistema di registrazione personalizzato, è possibile fornire un oggetto JavaScript che implementa il `ILogger` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="70dce-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="70dce-154">È l'unico metodo che deve essere implementato `log`, che accetta il livello dell'evento e il messaggio associato all'evento.</span><span class="sxs-lookup"><span data-stu-id="70dce-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="70dce-155">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="70dce-155">For example:</span></span>

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="70dce-156">La registrazione del client .NET</span><span class="sxs-lookup"><span data-stu-id="70dce-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="70dce-157">File di log lato client possono contenere informazioni riservate dall'app.</span><span class="sxs-lookup"><span data-stu-id="70dce-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="70dce-158">**Mai** invio di log non elaborati dalle app di produzione ai forum pubblico come GitHub.</span><span class="sxs-lookup"><span data-stu-id="70dce-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="70dce-159">Per ottenere i log del client .NET, è possibile usare la `ConfigureLogging` metodo `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="70dce-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="70dce-160">Questo procedimento funziona esattamente come le `ConfigureLogging` metodo sul `WebHostBuilder` e `HostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="70dce-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="70dce-161">È possibile configurare è usare gli stessi provider di registrazione in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70dce-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="70dce-162">Tuttavia, è necessario installare e abilitare i pacchetti NuGet per il provider di registrazione singola manualmente.</span><span class="sxs-lookup"><span data-stu-id="70dce-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="70dce-163">Registrazione console</span><span class="sxs-lookup"><span data-stu-id="70dce-163">Console logging</span></span>

<span data-ttu-id="70dce-164">Per abilitare la registrazione della Console, aggiungere il [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="70dce-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="70dce-165">Usare quindi il `AddConsole` metodo per configurare il logger della console:</span><span class="sxs-lookup"><span data-stu-id="70dce-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="70dce-166">La registrazione di finestra di output di debug</span><span class="sxs-lookup"><span data-stu-id="70dce-166">Debug output window logging</span></span>

<span data-ttu-id="70dce-167">È anche possibile configurare i log per visualizzare il **Output** finestra in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="70dce-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="70dce-168">Installare il [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pacchetto e usare il `AddDebug` metodo:</span><span class="sxs-lookup"><span data-stu-id="70dce-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="70dce-169">Altri provider di registrazione</span><span class="sxs-lookup"><span data-stu-id="70dce-169">Other logging providers</span></span>

<span data-ttu-id="70dce-170">SignalR supporta altri provider di registrazione, ad esempio Serilog, Seq, NLog o qualsiasi altro sistema di registrazione che si integra con `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="70dce-170">SignalR supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="70dce-171">Se il sistema di registrazione fornisce un' `ILoggerProvider`, è possibile registrarlo con `AddProvider`:</span><span class="sxs-lookup"><span data-stu-id="70dce-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="70dce-172">Livello di dettaglio di controllo</span><span class="sxs-lookup"><span data-stu-id="70dce-172">Control verbosity</span></span>

<span data-ttu-id="70dce-173">Se si sta eseguendo l'app da altre posizioni, modificare il livello predefinito da `Debug` potrebbe essere troppo dettagliata.</span><span class="sxs-lookup"><span data-stu-id="70dce-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="70dce-174">È possibile usare un filtro per configurare il livello di registrazione per i log di SignalR.</span><span class="sxs-lookup"><span data-stu-id="70dce-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="70dce-175">Questa operazione può essere eseguita nel codice, in gran parte esattamente come nel server:</span><span class="sxs-lookup"><span data-stu-id="70dce-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="70dce-176">Tracce di rete</span><span class="sxs-lookup"><span data-stu-id="70dce-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="70dce-177">Una traccia di rete include il contenuto completo di tutti i messaggi inviati dall'app.</span><span class="sxs-lookup"><span data-stu-id="70dce-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="70dce-178">**Mai** invio tracce di rete non elaborati dalle app di produzione ai forum pubblico come GitHub.</span><span class="sxs-lookup"><span data-stu-id="70dce-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="70dce-179">Se si verifica un problema, una traccia di rete in alcuni casi può fornire numerose informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="70dce-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="70dce-180">Ciò è particolarmente utile se si desidera segnalare un problema in una pagina.</span><span class="sxs-lookup"><span data-stu-id="70dce-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="70dce-181">Raccogliere una traccia di rete con Fiddler (opzione consigliata)</span><span class="sxs-lookup"><span data-stu-id="70dce-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="70dce-182">Questo metodo funziona per tutte le app.</span><span class="sxs-lookup"><span data-stu-id="70dce-182">This method works for all apps.</span></span>

<span data-ttu-id="70dce-183">Fiddler è uno strumento molto potente per raccogliere le tracce HTTP.</span><span class="sxs-lookup"><span data-stu-id="70dce-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="70dce-184">Installare l'app da [telerik.com/fiddler](https://www.telerik.com/fiddler), avviarla, quindi eseguire l'app e riprodurre il problema.</span><span class="sxs-lookup"><span data-stu-id="70dce-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="70dce-185">Fiddler è disponibile per Windows e sono disponibili versioni beta per macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="70dce-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="70dce-186">Se ci si connette tramite HTTPS, esistono alcuni passaggi aggiuntivi per garantire che Fiddler in grado di decrittografare il traffico HTTPS.</span><span class="sxs-lookup"><span data-stu-id="70dce-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="70dce-187">Per altre informazioni, vedere la [documentazione di Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span><span class="sxs-lookup"><span data-stu-id="70dce-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="70dce-188">Dopo aver raccolto la traccia, è possibile esportare la traccia, scegliendo **File** > **salvare** > **tutte le sessioni** dalla barra dei menu.</span><span class="sxs-lookup"><span data-stu-id="70dce-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions** from the menu bar.</span></span>

![Esportazione di tutte le sessioni da Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="70dce-190">Raccogliere una traccia di rete con tcpdump (macOS e Linux solo)</span><span class="sxs-lookup"><span data-stu-id="70dce-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="70dce-191">Questo metodo funziona per tutte le app.</span><span class="sxs-lookup"><span data-stu-id="70dce-191">This method works for all apps.</span></span>

<span data-ttu-id="70dce-192">È possibile raccogliere le tracce TCP non elaborate usando tcpdump eseguendo il comando seguente da una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="70dce-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="70dce-193">Potrebbe essere necessario essere `root` o un prefisso con il comando `sudo` se si verifica un errore di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="70dce-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="70dce-194">Sostituire `[interface]` con l'interfaccia di rete che si desidera acquisire in.</span><span class="sxs-lookup"><span data-stu-id="70dce-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="70dce-195">Si tratta in genere simile `/dev/eth0` (per l'interfaccia Ethernet standard) o `/dev/lo0` (per il traffico di localhost).</span><span class="sxs-lookup"><span data-stu-id="70dce-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="70dce-196">Per altre informazioni, vedere il `tcpdump` pagina di manuale nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="70dce-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="70dce-197">Raccogliere una traccia di rete nel browser</span><span class="sxs-lookup"><span data-stu-id="70dce-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="70dce-198">Questo metodo funziona solo per le app basate su browser.</span><span class="sxs-lookup"><span data-stu-id="70dce-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="70dce-199">La maggior parte degli strumenti di sviluppo del browser aperta una scheda "Rete" che consente di acquisire attività di rete tra il browser e il server.</span><span class="sxs-lookup"><span data-stu-id="70dce-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="70dce-200">Tuttavia, queste tracce non includono i messaggi di evento Server-Sent e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="70dce-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="70dce-201">Se si utilizza questi trasporti, usando uno strumento come Fiddler o TcpDump (descritti di seguito) è un approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="70dce-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="70dce-202">Microsoft Edge e Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="70dce-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="70dce-203">(Le istruzioni sono le stesse sia per Microsoft Edge che per Internet Explorer)</span><span class="sxs-lookup"><span data-stu-id="70dce-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="70dce-204">Premere F12 per aprire gli strumenti di sviluppo</span><span class="sxs-lookup"><span data-stu-id="70dce-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="70dce-205">Fare clic sulla scheda di rete</span><span class="sxs-lookup"><span data-stu-id="70dce-205">Click the Network Tab</span></span>
3. <span data-ttu-id="70dce-206">Aggiornare la pagina, se necessario e riprodurre il problema</span><span class="sxs-lookup"><span data-stu-id="70dce-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="70dce-207">Fare clic sull'icona Salva nella barra degli strumenti per esportare la traccia come "HAR" file:</span><span class="sxs-lookup"><span data-stu-id="70dce-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![Il salvataggio degli strumenti della scheda di rete sull'icona allo sviluppo di Microsoft Edge](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="70dce-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="70dce-209">Google Chrome</span></span>

1. <span data-ttu-id="70dce-210">Premere F12 per aprire gli strumenti di sviluppo</span><span class="sxs-lookup"><span data-stu-id="70dce-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="70dce-211">Fare clic sulla scheda di rete</span><span class="sxs-lookup"><span data-stu-id="70dce-211">Click the Network Tab</span></span>
3. <span data-ttu-id="70dce-212">Aggiornare la pagina, se necessario e riprodurre il problema</span><span class="sxs-lookup"><span data-stu-id="70dce-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="70dce-213">Fare clic in un punto qualsiasi nell'elenco delle richieste e scegliere "Salva come HAR con contenuto":</span><span class="sxs-lookup"><span data-stu-id="70dce-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Opzione "Salva come HAR con contenuto" nella scheda di rete di Google Chrome Dev Tools](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="70dce-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="70dce-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="70dce-216">Premere F12 per aprire gli strumenti di sviluppo</span><span class="sxs-lookup"><span data-stu-id="70dce-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="70dce-217">Fare clic sulla scheda di rete</span><span class="sxs-lookup"><span data-stu-id="70dce-217">Click the Network Tab</span></span>
3. <span data-ttu-id="70dce-218">Aggiornare la pagina, se necessario e riprodurre il problema</span><span class="sxs-lookup"><span data-stu-id="70dce-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="70dce-219">Fare clic in un punto qualsiasi nell'elenco delle richieste e scegliere "Salva tutto come HAR"</span><span class="sxs-lookup"><span data-stu-id="70dce-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Opzione "Salva tutto come HAR" nella scheda di rete strumenti di sviluppo di Mozilla Firefox](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="70dce-221">Collegare i file di diagnostica per problemi di GitHub</span><span class="sxs-lookup"><span data-stu-id="70dce-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="70dce-222">È possibile collegare i file di diagnostica per problemi di GitHub rinominandoli pertanto hanno un `.txt` estensione e quindi trascinarli e rilasciarli al problema.</span><span class="sxs-lookup"><span data-stu-id="70dce-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="70dce-223">Non incollare il contenuto del file di log o le tracce di rete in un problema di GitHub.</span><span class="sxs-lookup"><span data-stu-id="70dce-223">Please don't paste the content of log files or network traces into a GitHub issue.</span></span> <span data-ttu-id="70dce-224">Questi log e tracce possono essere piuttosto grande e in genere tronca GitHub.</span><span class="sxs-lookup"><span data-stu-id="70dce-224">These logs and traces can be quite large, and GitHub usually truncates them.</span></span>

![Il trascinamento di file di log a un problema di GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="70dce-226">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="70dce-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
