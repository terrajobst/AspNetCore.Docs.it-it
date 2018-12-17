---
title: Modulo ASP.NET Core
author: guardrex
description: Informazioni su come il modulo ASP.NET Core consenta al server Web Kestrel di usare IIS o IIS Express.
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: d3f3a42dd7aebc425905b865376a584bcf0e5153
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861459"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="5287a-103">Modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5287a-103">ASP.NET Core Module</span></span>

<span data-ttu-id="5287a-104">Di [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="5287a-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5287a-105">Il modulo ASP.NET Core consente l'esecuzione delle app ASP.NET Core in un processo di lavoro IIS (*in-process*) o dietro a IIS in una configurazione di proxy inverso (*out-of-process*).</span><span class="sxs-lookup"><span data-stu-id="5287a-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="5287a-106">IIS offre funzionalità di sicurezza avanzate per le app Web e ne garantisce la facilità di gestione.</span><span class="sxs-lookup"><span data-stu-id="5287a-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5287a-107">Il modulo ASP.NET Core consente l'esecuzione delle app ASP.NET Core dietro a IIS in una configurazione di proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="5287a-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="5287a-108">IIS offre funzionalità di sicurezza avanzate per le app Web e ne garantisce la facilità di gestione.</span><span class="sxs-lookup"><span data-stu-id="5287a-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="5287a-109">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="5287a-109">Supported Windows versions:</span></span>

* <span data-ttu-id="5287a-110">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="5287a-110">Windows 7 or later</span></span>
* <span data-ttu-id="5287a-111">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="5287a-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5287a-112">In caso di hosting in-process, il modulo usa un'implementazione di server in-process IIS, un server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="5287a-112">When hosting in-process, the module uses an IIS in-process server implementation, IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="5287a-113">In caso di hosting out-of-process, il modulo funziona solo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5287a-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="5287a-114">Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza detto [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="5287a-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5287a-115">Il modulo funziona solo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5287a-115">The module only works with Kestrel.</span></span> <span data-ttu-id="5287a-116">Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza detto [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="5287a-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="5287a-117">Descrizione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5287a-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5287a-118">Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5287a-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="5287a-119">Ospitare un app ASP.NET Core all'interno del processo di lavoro IIS (`w3wp.exe`), denominato [modello di hosting in-process](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="5287a-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="5287a-120">Inoltrare le richieste Web all'app back-end ASP.NET Core che esegue il [server Kestrel](xref:fundamentals/servers/kestrel), denominato [modello di hosting out-of-process](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="5287a-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="5287a-121">Modello di hosting in-process</span><span class="sxs-lookup"><span data-stu-id="5287a-121">In-process hosting model</span></span>

<span data-ttu-id="5287a-122">Se si usa l'hosting in-process, un'app ASP.NET Core esegue lo stesso processo del processo di lavoro IIS.</span><span class="sxs-lookup"><span data-stu-id="5287a-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="5287a-123">In questo modo le prestazioni non vengono ridotte per inoltrare i dati delle richieste sulla scheda Loopback quando si usa il modello di hosting out-of-process.</span><span class="sxs-lookup"><span data-stu-id="5287a-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="5287a-124">Per gestire il processo, IIS usa il [servizio Attivazione processo Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="5287a-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="5287a-125">Il modulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5287a-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="5287a-126">Esegue l'inizializzazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="5287a-126">Performs app initialization.</span></span>
  * <span data-ttu-id="5287a-127">Carica [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="5287a-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="5287a-128">Chiama `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="5287a-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="5287a-129">Gestisce la durata della richiesta nativa di IIS.</span><span class="sxs-lookup"><span data-stu-id="5287a-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="5287a-130">Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app ospitata in-process:</span><span class="sxs-lookup"><span data-stu-id="5287a-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="5287a-132">Una richiesta arriva dal Web al driver HTTP.sys in modalità kernel.</span><span class="sxs-lookup"><span data-stu-id="5287a-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="5287a-133">Il driver instrada la richiesta nativa IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="5287a-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="5287a-134">Il modulo riceve la richiesta nativa e la trasmette al server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="5287a-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="5287a-135">Il server HTTP di IIS è un'implementazione di server in-process IIS che converte la richiesta da nativa a gestita.</span><span class="sxs-lookup"><span data-stu-id="5287a-135">IIS HTTP Server is an IIS in-process server implementation that converts the request from native to managed.</span></span>

<span data-ttu-id="5287a-136">Dopo che la richiesta è stata elaborata dal server HTTP di IIS, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5287a-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="5287a-137">La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app.</span><span class="sxs-lookup"><span data-stu-id="5287a-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="5287a-138">La risposta dell'app viene quindi restituita a IIS, che ne esegue di nuovo il push al client che ha avviato la richiesta.</span><span class="sxs-lookup"><span data-stu-id="5287a-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="5287a-139">Modello di hosting out-of-process</span><span class="sxs-lookup"><span data-stu-id="5287a-139">Out-of-process hosting model</span></span>

<span data-ttu-id="5287a-140">Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="5287a-140">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="5287a-141">Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se viene arrestata o si arresta in modo anomalo.</span><span class="sxs-lookup"><span data-stu-id="5287a-141">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="5287a-142">Si tratta essenzialmente dello stesso comportamento delle app eseguite in-process e gestite dal [servizio Attivazione processo Windows](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="5287a-142">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="5287a-143">Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app ospitata out-of-process:</span><span class="sxs-lookup"><span data-stu-id="5287a-143">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="5287a-145">Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel.</span><span class="sxs-lookup"><span data-stu-id="5287a-145">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="5287a-146">Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="5287a-146">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="5287a-147">Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.</span><span class="sxs-lookup"><span data-stu-id="5287a-147">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="5287a-148">Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il middleware di integrazione di IIS configura il server per l'ascolto su `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="5287a-148">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="5287a-149">Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate.</span><span class="sxs-lookup"><span data-stu-id="5287a-149">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="5287a-150">Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5287a-150">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="5287a-151">Dopo che Kestrel ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5287a-151">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="5287a-152">La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app.</span><span class="sxs-lookup"><span data-stu-id="5287a-152">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="5287a-153">Il middleware aggiunto dall'integrazione di IIS aggiorna lo schema, l'IP remoto e il percorso di base all'account per l'inoltro della richiesta a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5287a-153">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="5287a-154">La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.</span><span class="sxs-lookup"><span data-stu-id="5287a-154">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5287a-155">Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS che inoltra le richieste Web alle app back-end ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5287a-155">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="5287a-156">Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue anche la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="5287a-156">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="5287a-157">Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se si verifica un arresto anomalo di questa.</span><span class="sxs-lookup"><span data-stu-id="5287a-157">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="5287a-158">Si tratta essenzialmente dello stesso comportamento delle app ASP.NET 4.x eseguite In-Process in IIS e gestite dal [servizio Attivazione processo Windows (WAS, Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="5287a-158">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="5287a-159">Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app:</span><span class="sxs-lookup"><span data-stu-id="5287a-159">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="5287a-161">Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel.</span><span class="sxs-lookup"><span data-stu-id="5287a-161">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="5287a-162">Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="5287a-162">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="5287a-163">Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.</span><span class="sxs-lookup"><span data-stu-id="5287a-163">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="5287a-164">Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il middleware di integrazione di IIS configura il server per l'ascolto su `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="5287a-164">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="5287a-165">Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate.</span><span class="sxs-lookup"><span data-stu-id="5287a-165">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="5287a-166">Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5287a-166">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="5287a-167">Dopo che Kestrel ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5287a-167">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="5287a-168">La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app.</span><span class="sxs-lookup"><span data-stu-id="5287a-168">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="5287a-169">Il middleware aggiunto dall'integrazione di IIS aggiorna lo schema, l'IP remoto e il percorso di base all'account per l'inoltro della richiesta a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5287a-169">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="5287a-170">La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.</span><span class="sxs-lookup"><span data-stu-id="5287a-170">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="5287a-171">Molti moduli nativi, ad esempio l'autenticazione di Windows, rimangono attivi.</span><span class="sxs-lookup"><span data-stu-id="5287a-171">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="5287a-172">Per altre informazioni sui moduli IIS attivi con il modulo, vedere <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="5287a-172">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="5287a-173">Il modulo ASP.NET Core ha anche qualche altra funzione.</span><span class="sxs-lookup"><span data-stu-id="5287a-173">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="5287a-174">Il modulo è in grado di:</span><span class="sxs-lookup"><span data-stu-id="5287a-174">The module can:</span></span>

* <span data-ttu-id="5287a-175">Impostare variabili di ambiente per il processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="5287a-175">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="5287a-176">Registrare output stdout in una risorsa di archiviazione di file per la risoluzione di problemi di avvio.</span><span class="sxs-lookup"><span data-stu-id="5287a-176">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="5287a-177">Inoltrare token di autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="5287a-177">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="5287a-178">Come installare e usare il modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5287a-178">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="5287a-179">Per istruzioni dettagliate su come installare e usare il modulo ASP.NET Core, vedere <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="5287a-179">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="5287a-180">Per informazioni sulla configurazione del modulo, vedere <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="5287a-180">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5287a-181">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5287a-181">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="5287a-182">Repository GitHub del modulo ASP.NET Core (codice sorgente)</span><span class="sxs-lookup"><span data-stu-id="5287a-182">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
