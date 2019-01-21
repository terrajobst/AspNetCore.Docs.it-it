---
title: Implementazioni di server Web in ASP.NET Core
author: guardrex
description: Individuare i server Web Kestrel e HTTP.sys per ASP.NET Core. Informazioni su come scegliere un server e quando usare un server proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/servers/index
ms.openlocfilehash: a9f40ad7e9a63d6f88b6533578db8dfc55490bc9
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396246"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="4017f-104">Implementazioni di server Web in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4017f-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="4017f-105">Di [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="4017f-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="4017f-106">Un'app ASP.NET Core viene eseguita con un'implementazione del server HTTP in-process.</span><span class="sxs-lookup"><span data-stu-id="4017f-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="4017f-107">L'implementazione del server attende le richieste HTTP e le rende visibili all'app come un set di [funzionalità di richiesta](xref:fundamentals/request-features) combinate in un <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="4017f-107">The server implementation listens for HTTP requests and surfaces them to the app as a set of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="4017f-108">Windows</span><span class="sxs-lookup"><span data-stu-id="4017f-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="4017f-109">ASP.NET Core include quanto segue:</span><span class="sxs-lookup"><span data-stu-id="4017f-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="4017f-110">Il [server Kestrel](xref:fundamentals/servers/kestrel) è l'implementazione di server HTTP multipiattaforma predefinita.</span><span class="sxs-lookup"><span data-stu-id="4017f-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server implementation.</span></span>
* <span data-ttu-id="4017f-111">Il server HTTP IIS è un [server in-process](#in-process-hosting-model) per IIS.</span><span class="sxs-lookup"><span data-stu-id="4017f-111">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="4017f-112">Il [server HTTP.sys](xref:fundamentals/servers/httpsys) è un server HTTP solo per Windows basato sul [driver del kernel HTTP.sys e l'API HTTP Server](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="4017f-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="4017f-113">Quando si usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) oppure [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), l'app viene eseguita:</span><span class="sxs-lookup"><span data-stu-id="4017f-113">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app either runs:</span></span>

* <span data-ttu-id="4017f-114">Nello stesso processo del processo di lavoro IIS ([modello di hosting in-process](#in-process-hosting-model)) con il [server HTTP IIS](#iis-http-server).</span><span class="sxs-lookup"><span data-stu-id="4017f-114">In the same process as the IIS worker process (the [in-process hosting model](#in-process-hosting-model)) with the [IIS HTTP Server](#iis-http-server).</span></span> <span data-ttu-id="4017f-115">*In-process* è la configurazione consigliata.</span><span class="sxs-lookup"><span data-stu-id="4017f-115">*In-process* is the recommended configuration.</span></span>
* <span data-ttu-id="4017f-116">In un processo separato dal processo di lavoro IIS ([modello di hosting out-of-process](#out-of-process-hosting-model)) con il [server Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="4017f-116">In a process separate from the IIS worker process (the [out-of-process hosting model](#out-of-process-hosting-model)) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="4017f-117">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) è un modulo IIS nativo che gestisce le richieste IIS native tra IIS e il server HTTP IIS in-process o il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4017f-117">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is a native IIS module that handles native IIS requests between IIS and the in-process IIS HTTP Server or Kestrel.</span></span> <span data-ttu-id="4017f-118">Per ulteriori informazioni, vedere <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="4017f-118">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="4017f-119">Modelli di hosting</span><span class="sxs-lookup"><span data-stu-id="4017f-119">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="4017f-120">Modello di hosting in-process</span><span class="sxs-lookup"><span data-stu-id="4017f-120">In-process hosting model</span></span>

<span data-ttu-id="4017f-121">Se si usa l'hosting in-process, un'app ASP.NET Core esegue lo stesso processo del processo di lavoro IIS.</span><span class="sxs-lookup"><span data-stu-id="4017f-121">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="4017f-122">L'hosting in-process offre prestazioni migliori rispetto all'hosting out-of-process perché le richieste non vengono inserite in proxy sulla scheda di loopback, un'interfaccia di rete che restituisce il traffico di rete in uscita allo stesso computer.</span><span class="sxs-lookup"><span data-stu-id="4017f-122">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="4017f-123">Per gestire il processo, IIS usa il [servizio Attivazione processo Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="4017f-123">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="4017f-124">Il modulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4017f-124">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="4017f-125">Esegue l'inizializzazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4017f-125">Performs app initialization.</span></span>
  * <span data-ttu-id="4017f-126">Carica [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="4017f-126">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="4017f-127">Chiama `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="4017f-127">Calls `Program.Main`.</span></span>
* <span data-ttu-id="4017f-128">Gestisce la durata della richiesta nativa di IIS.</span><span class="sxs-lookup"><span data-stu-id="4017f-128">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="4017f-129">Il modello di hosting In-Process non è supportato per le app ASP.NET Core destinate a .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4017f-129">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="4017f-130">Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app ospitata in-process:</span><span class="sxs-lookup"><span data-stu-id="4017f-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Modulo ASP.NET Core](_static/ancm-inprocess.png)

<span data-ttu-id="4017f-132">Una richiesta arriva dal Web al driver HTTP.sys in modalità kernel.</span><span class="sxs-lookup"><span data-stu-id="4017f-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="4017f-133">Il driver instrada la richiesta nativa IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4017f-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="4017f-134">Il modulo riceve la richiesta nativa e la trasmette al server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="4017f-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="4017f-135">Il server HTTP di IIS è un'implementazione di server in-process per IIS che converte la richiesta da nativa a gestita.</span><span class="sxs-lookup"><span data-stu-id="4017f-135">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="4017f-136">Dopo che la richiesta è stata elaborata dal server HTTP di IIS, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4017f-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="4017f-137">La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app.</span><span class="sxs-lookup"><span data-stu-id="4017f-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="4017f-138">La risposta dell'app viene quindi restituita a IIS, che ne esegue di nuovo il push al client che ha avviato la richiesta.</span><span class="sxs-lookup"><span data-stu-id="4017f-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

<span data-ttu-id="4017f-139">L'hosting in-process è una funzionalità che richiede il consenso esplicito per le app esistenti, mentre i modelli [dotnet new](/dotnet/core/tools/dotnet-new) usano per impostazione predefinita il modello di hosting in-process per tutti gli scenari IIS e IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4017f-139">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="4017f-140">Modello di hosting out-of-process</span><span class="sxs-lookup"><span data-stu-id="4017f-140">Out-of-process hosting model</span></span>

<span data-ttu-id="4017f-141">Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="4017f-141">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="4017f-142">Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se viene arrestata o si arresta in modo anomalo.</span><span class="sxs-lookup"><span data-stu-id="4017f-142">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="4017f-143">Si tratta essenzialmente dello stesso comportamento delle app eseguite in-process e gestite dal [servizio Attivazione processo Windows](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="4017f-143">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="4017f-144">Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app ospitata out-of-process:</span><span class="sxs-lookup"><span data-stu-id="4017f-144">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Modulo ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="4017f-146">Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel.</span><span class="sxs-lookup"><span data-stu-id="4017f-146">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="4017f-147">Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4017f-147">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="4017f-148">Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.</span><span class="sxs-lookup"><span data-stu-id="4017f-148">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="4017f-149">Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il middleware di integrazione di IIS configura il server per l'ascolto su `http://localhost:{PORT}`.</span><span class="sxs-lookup"><span data-stu-id="4017f-149">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="4017f-150">Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate.</span><span class="sxs-lookup"><span data-stu-id="4017f-150">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="4017f-151">Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4017f-151">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="4017f-152">Dopo che Kestrel ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4017f-152">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="4017f-153">La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app.</span><span class="sxs-lookup"><span data-stu-id="4017f-153">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="4017f-154">Il middleware aggiunto dall'integrazione di IIS aggiorna lo schema, l'IP remoto e il percorso di base all'account per l'inoltro della richiesta a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4017f-154">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="4017f-155">La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.</span><span class="sxs-lookup"><span data-stu-id="4017f-155">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="4017f-156">Per indicazioni sulla configurazione di IIS e del modulo ASP.NET Core, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="4017f-156">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="4017f-157">macOS</span><span class="sxs-lookup"><span data-stu-id="4017f-157">macOS</span></span>](#tab/macos)

<span data-ttu-id="4017f-158">ASP.NET Core viene fornito con il [server Kestrel](xref:fundamentals/servers/kestrel), ovvero il server HTTP multipiattaforma predefinito.</span><span class="sxs-lookup"><span data-stu-id="4017f-158">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="4017f-159">Linux</span><span class="sxs-lookup"><span data-stu-id="4017f-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="4017f-160">ASP.NET Core viene fornito con il [server Kestrel](xref:fundamentals/servers/kestrel), ovvero il server HTTP multipiattaforma predefinito.</span><span class="sxs-lookup"><span data-stu-id="4017f-160">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="4017f-161">Windows</span><span class="sxs-lookup"><span data-stu-id="4017f-161">Windows</span></span>](#tab/windows)

<span data-ttu-id="4017f-162">ASP.NET Core include quanto segue:</span><span class="sxs-lookup"><span data-stu-id="4017f-162">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="4017f-163">Il [server kestrel](xref:fundamentals/servers/kestrel) è il server HTTP multipiattaforma predefinito.</span><span class="sxs-lookup"><span data-stu-id="4017f-163">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="4017f-164">Il [server HTTP.sys](xref:fundamentals/servers/httpsys) è un server HTTP solo per Windows basato sul [driver del kernel HTTP.sys e l'API HTTP Server](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="4017f-164">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="4017f-165">Quando si usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), l'app viene eseguita in un processo separato dal processo di lavoro IIS (*out-of-process*) con il [server Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="4017f-165">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="4017f-166">Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="4017f-166">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="4017f-167">Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se viene arrestata o si arresta in modo anomalo.</span><span class="sxs-lookup"><span data-stu-id="4017f-167">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="4017f-168">Si tratta essenzialmente dello stesso comportamento delle app eseguite in-process e gestite dal [servizio Attivazione processo Windows](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="4017f-168">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="4017f-169">Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app ospitata out-of-process:</span><span class="sxs-lookup"><span data-stu-id="4017f-169">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Modulo ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="4017f-171">Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel.</span><span class="sxs-lookup"><span data-stu-id="4017f-171">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="4017f-172">Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4017f-172">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="4017f-173">Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.</span><span class="sxs-lookup"><span data-stu-id="4017f-173">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="4017f-174">Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il middleware di integrazione di IIS configura il server per l'ascolto su `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="4017f-174">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="4017f-175">Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate.</span><span class="sxs-lookup"><span data-stu-id="4017f-175">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="4017f-176">Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4017f-176">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="4017f-177">Dopo che Kestrel ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4017f-177">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="4017f-178">La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app.</span><span class="sxs-lookup"><span data-stu-id="4017f-178">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="4017f-179">Il middleware aggiunto dall'integrazione di IIS aggiorna lo schema, l'IP remoto e il percorso di base all'account per l'inoltro della richiesta a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4017f-179">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="4017f-180">La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.</span><span class="sxs-lookup"><span data-stu-id="4017f-180">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="4017f-181">Per indicazioni sulla configurazione di IIS e del modulo ASP.NET Core, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="4017f-181">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="4017f-182">macOS</span><span class="sxs-lookup"><span data-stu-id="4017f-182">macOS</span></span>](#tab/macos)

<span data-ttu-id="4017f-183">ASP.NET Core viene fornito con il [server Kestrel](xref:fundamentals/servers/kestrel), ovvero il server HTTP multipiattaforma predefinito.</span><span class="sxs-lookup"><span data-stu-id="4017f-183">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="4017f-184">Linux</span><span class="sxs-lookup"><span data-stu-id="4017f-184">Linux</span></span>](#tab/linux)

<span data-ttu-id="4017f-185">ASP.NET Core viene fornito con il [server Kestrel](xref:fundamentals/servers/kestrel), ovvero il server HTTP multipiattaforma predefinito.</span><span class="sxs-lookup"><span data-stu-id="4017f-185">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="4017f-186">Kestrel</span><span class="sxs-lookup"><span data-stu-id="4017f-186">Kestrel</span></span>

<span data-ttu-id="4017f-187">Kestrel è il server Web predefinito incluso nei modelli di progetto di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4017f-187">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4017f-188">Kestrel può essere usato:</span><span class="sxs-lookup"><span data-stu-id="4017f-188">Kestrel can be used:</span></span>

* <span data-ttu-id="4017f-189">Da solo come server perimetrale che elabora le richieste direttamente da una rete, inclusa Internet.</span><span class="sxs-lookup"><span data-stu-id="4017f-189">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>

  ![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

* <span data-ttu-id="4017f-191">Con un *server proxy inverso*, ad esempio [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="4017f-191">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="4017f-192">Il server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4017f-192">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

  ![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="4017f-194">Entrambe le configurazioni di hosting, con o senza un server proxy inverso, sono supportate per le app ASP.NET Core 2.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="4017f-194">Either hosting configuration&mdash;with or without a reverse proxy server&mdash;is supported for ASP.NET Core 2.1 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4017f-195">Se l'app accetta solo le richieste provenienti da una rete interna, Kestrel è utilizzabile in modo autonomo.</span><span class="sxs-lookup"><span data-stu-id="4017f-195">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="4017f-197">Se l'app è esposta a Internet, Kestrel deve usare un *server proxy inverso*, come [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="4017f-197">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="4017f-198">Il server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4017f-198">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="4017f-200">Il motivo principale per cui si usa un proxy inverso per le distribuzioni server perimetrali pubbliche, esposte direttamente a Internet, è la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="4017f-200">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="4017f-201">Le versioni 1.x di Kestrel non includono funzionalità di sicurezza importanti per difendersi da attacchi da Internet,</span><span class="sxs-lookup"><span data-stu-id="4017f-201">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="4017f-202">ad esempio timeout appropriati, limiti delle dimensioni delle richieste e limiti delle connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="4017f-202">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="4017f-203">Per indicazioni per la configurazione di Kestrel e per informazioni su quando usare Kestrel in una configurazione con proxy inverso, vedere <xref:fundamentals/servers/kestrel>.</span><span class="sxs-lookup"><span data-stu-id="4017f-203">For Kestrel configuration guidance and information on when to use Kestrel in a reverse proxy configuration, see <xref:fundamentals/servers/kestrel>.</span></span>

### <a name="nginx-with-kestrel"></a><span data-ttu-id="4017f-204">Nginx con Kestrel</span><span class="sxs-lookup"><span data-stu-id="4017f-204">Nginx with Kestrel</span></span>

<span data-ttu-id="4017f-205">Per informazioni su come usare Nginx in Linux come server proxy inverso per Kestrel, vedere <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="4017f-205">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="4017f-206">Apache con Kestrel</span><span class="sxs-lookup"><span data-stu-id="4017f-206">Apache with Kestrel</span></span>

<span data-ttu-id="4017f-207">Per informazioni su come usare Apache in Linux come server proxy inverso per Kestrel, vedere <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="4017f-207">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a><span data-ttu-id="4017f-208">Server HTTP IIS</span><span class="sxs-lookup"><span data-stu-id="4017f-208">IIS HTTP Server</span></span>

<span data-ttu-id="4017f-209">Server HTTP IIS è un [server in-process](#in-process-hosting-model) per IIS ed è richiesto per le distribuzioni in-process.</span><span class="sxs-lookup"><span data-stu-id="4017f-209">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS and required for in-process deployments.</span></span> <span data-ttu-id="4017f-210">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) gestisce le richieste IIS native tra IIS e Server HTTP IIS.</span><span class="sxs-lookup"><span data-stu-id="4017f-210">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) handles native IIS requests between IIS and IIS HTTP Server.</span></span> <span data-ttu-id="4017f-211">Per ulteriori informazioni, vedere <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="4017f-211">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="4017f-212">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4017f-212">HTTP.sys</span></span>

<span data-ttu-id="4017f-213">Se si eseguono app ASP.NET Core in Windows, HTTP.sys è un'alternativa a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4017f-213">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="4017f-214">Kestrel è in genere consigliato per ottenere prestazioni ottimali.</span><span class="sxs-lookup"><span data-stu-id="4017f-214">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="4017f-215">HTTP.sys è utilizzabile negli scenari in cui l'app viene esposta a Internet e le funzionalità necessarie sono supportate da HTTP.sys, ma non da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4017f-215">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="4017f-216">Per ulteriori informazioni, vedere <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="4017f-216">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="4017f-218">HTTP.sys può essere usato anche per le app che vengono esposte solo a una rete interna.</span><span class="sxs-lookup"><span data-stu-id="4017f-218">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="4017f-220">Per indicazioni per la configurazione di HTTP.sys, vedere <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="4017f-220">For HTTP.sys configuration guidance, see <xref:fundamentals/servers/httpsys>.</span></span>

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="4017f-221">Infrastruttura del server ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4017f-221">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="4017f-222">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder), disponibile nel metodo `Startup.Configure`, espone la proprietà [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) di tipo [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="4017f-222">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="4017f-223">Kestrel e HTTP.sys espongono una singola funzionalità ognuno, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ma implementazioni server diverse potrebbero esporre funzionalità aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="4017f-223">Kestrel and HTTP.sys only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="4017f-224">È possibile usare `IServerAddressesFeature` per individuare la porta a cui è associata l'implementazione server in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4017f-224">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="4017f-225">Server personalizzati</span><span class="sxs-lookup"><span data-stu-id="4017f-225">Custom servers</span></span>

<span data-ttu-id="4017f-226">Se i server predefiniti non soddisfano i requisiti dell'app, è possibile creare un'implementazione server personalizzata.</span><span class="sxs-lookup"><span data-stu-id="4017f-226">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="4017f-227">La [guida all'interfaccia OWIN (Open Web Interface for .NET)](xref:fundamentals/owin) spiega come scrivere un'implementazione [Nowin](https://github.com/Bobris/Nowin) di [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver).</span><span class="sxs-lookup"><span data-stu-id="4017f-227">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="4017f-228">Solo le interfacce delle funzionalità usate dall'app devono essere implementate, anche se è richiesto come minimo il supporto di [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="4017f-228">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="4017f-229">Avvio del server</span><span class="sxs-lookup"><span data-stu-id="4017f-229">Server startup</span></span>

<span data-ttu-id="4017f-230">Il server viene avviato quando l'editor o l'ambiente di sviluppo integrato (IDE) avvia l'app:</span><span class="sxs-lookup"><span data-stu-id="4017f-230">The server is launched when the Integrated Development Environment (IDE) or editor starts the app:</span></span>

* <span data-ttu-id="4017f-231">[Visual Studio](https://www.visualstudio.com/vs/) &ndash; È possibile usare i profili di avvio per avviare l'app e il server con [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) o la console.</span><span class="sxs-lookup"><span data-stu-id="4017f-231">[Visual Studio](https://www.visualstudio.com/vs/) &ndash; Launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) or the console.</span></span>
* <span data-ttu-id="4017f-232">[Visual Studio Code](https://code.visualstudio.com/) &ndash; L'app e il server vengono avviati da [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), che attiva il debugger CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="4017f-232">[Visual Studio Code](https://code.visualstudio.com/) &ndash; The app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span>
* <span data-ttu-id="4017f-233">[Visual Studio per Mac](https://www.visualstudio.com/vs/mac/) &ndash; L'app e il server vengono avviati dal [debugger in modalità Mono Soft](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="4017f-233">[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) &ndash; The app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="4017f-234">All'avvio dell'app dal prompt dei comandi nella cartella del progetto, [dotnet run](/dotnet/core/tools/dotnet-run) avvia l'app e il server (solo Kestrel e HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="4017f-234">When launching the app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="4017f-235">La configurazione viene specificata dall'opzione `-c|--configuration`, impostata su `Debug` (valore predefinito) o su `Release`.</span><span class="sxs-lookup"><span data-stu-id="4017f-235">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="4017f-236">Se sono presenti profili di avvio in un file *launchSettings.json*, usare l'opzione `--launch-profile <NAME>` per impostare il profilo di avvio (ad esempio, `Development` o `Production`).</span><span class="sxs-lookup"><span data-stu-id="4017f-236">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="4017f-237">Per altre informazioni, vedere [dotnet run](/dotnet/core/tools/dotnet-run) e [Creazione di pacchetti di distribuzione di .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="4017f-237">For more information, see [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

## <a name="http2-support"></a><span data-ttu-id="4017f-238">Supporto per HTTP/2</span><span class="sxs-lookup"><span data-stu-id="4017f-238">HTTP/2 support</span></span>

<span data-ttu-id="4017f-239">[HTTP/2](https://httpwg.org/specs/rfc7540.html) è supportato con ASP.NET Core negli scenari di distribuzione seguenti:</span><span class="sxs-lookup"><span data-stu-id="4017f-239">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="4017f-240">Kestrel</span><span class="sxs-lookup"><span data-stu-id="4017f-240">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="4017f-241">Sistema operativo</span><span class="sxs-lookup"><span data-stu-id="4017f-241">Operating system</span></span>
    * <span data-ttu-id="4017f-242">Windows Server 2016/Windows 10 o versioni successive&dagger;</span><span class="sxs-lookup"><span data-stu-id="4017f-242">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="4017f-243">Linux con OpenSSL 1.0.2 o versioni successive (ad esempio, Ubuntu 16.04 o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="4017f-243">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="4017f-244">HTTP/2 verrà supportato in macOS in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="4017f-244">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="4017f-245">Framework di destinazione: .NET Core 2.2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="4017f-245">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="4017f-246">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4017f-246">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="4017f-247">Windows Server 2016/Windows 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="4017f-247">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="4017f-248">Framework di destinazione: non applicabile alle distribuzioni HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="4017f-248">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="4017f-249">IIS (in-process)</span><span class="sxs-lookup"><span data-stu-id="4017f-249">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="4017f-250">Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="4017f-250">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="4017f-251">Framework di destinazione: .NET Core 2.2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="4017f-251">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="4017f-252">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="4017f-252">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="4017f-253">Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="4017f-253">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="4017f-254">Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="4017f-254">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="4017f-255">Framework di destinazione: non applicabile alle distribuzioni IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="4017f-255">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="4017f-256">&dagger;Kestrel ha un supporto limitato per HTTP/2 in Windows Server 2012 R2 e Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="4017f-256">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="4017f-257">Il supporto è limitato perché l'elenco di suite di crittografia TLS supportate disponibili in questi sistemi operativi è limitato.</span><span class="sxs-lookup"><span data-stu-id="4017f-257">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="4017f-258">Un certificato generato con un algoritmo ECDSA potrebbe essere necessario per proteggere le connessioni TLS.</span><span class="sxs-lookup"><span data-stu-id="4017f-258">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="4017f-259">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4017f-259">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="4017f-260">Windows Server 2016/Windows 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="4017f-260">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="4017f-261">Framework di destinazione: non applicabile alle distribuzioni HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="4017f-261">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="4017f-262">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="4017f-262">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="4017f-263">Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="4017f-263">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="4017f-264">Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="4017f-264">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="4017f-265">Framework di destinazione: non applicabile alle distribuzioni IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="4017f-265">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="4017f-266">Una connessione HTTP/2 deve usare [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) e TLS 1.2 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="4017f-266">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="4017f-267">Per altre informazioni, vedere gli argomenti relativi agli scenari di distribuzione server.</span><span class="sxs-lookup"><span data-stu-id="4017f-267">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4017f-268">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4017f-268">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
