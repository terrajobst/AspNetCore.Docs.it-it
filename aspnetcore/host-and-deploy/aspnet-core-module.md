---
title: Modulo ASP.NET Core
author: guardrex
description: Informazioni su come configurare il modulo di ASP.NET Core per l'hosting di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/08/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 9270d7b462bbac1ae0ad896c0937ea6dd909b2cd
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159555"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="23732-103">Modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23732-103">ASP.NET Core Module</span></span>

<span data-ttu-id="23732-104">Di [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="23732-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23732-105">Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="23732-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="23732-106">Ospitare un app ASP.NET Core all'interno del processo di lavoro IIS (`w3wp.exe`), denominato [modello di hosting in-process](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="23732-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="23732-107">Inoltrare le richieste Web all'app back-end ASP.NET Core che esegue il [server Kestrel](xref:fundamentals/servers/kestrel), denominato [modello di hosting out-of-process](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="23732-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="23732-108">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="23732-108">Supported Windows versions:</span></span>

* <span data-ttu-id="23732-109">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="23732-109">Windows 7 or later</span></span>
* <span data-ttu-id="23732-110">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="23732-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="23732-111">In caso di hosting in-process, il modulo usa un'implementazione di server in-process per IIS detta server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="23732-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="23732-112">In caso di hosting out-of-process, il modulo funziona solo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23732-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="23732-113">Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="23732-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="23732-114">Modelli di hosting</span><span class="sxs-lookup"><span data-stu-id="23732-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="23732-115">Modello di hosting in-process</span><span class="sxs-lookup"><span data-stu-id="23732-115">In-process hosting model</span></span>

<span data-ttu-id="23732-116">Per configurare un'app per l'hosting in-process, aggiungere la proprietà `<AspNetCoreHostingModel>` al file di progetto dell'app usando il valore `InProcess` (l'hosting out-of-process viene impostato con `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="23732-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="23732-117">Il modello di hosting In-Process non è supportato per le app ASP.NET Core destinate a .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="23732-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="23732-118">Se la proprietà `<AspNetCoreHostingModel>` non è presente nel file, il valore predefinito è `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="23732-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="23732-119">In caso di hosting in-process, vengono applicate le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="23732-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="23732-120">Viene usato un server HTTP di IIS (`IISHttpServer`) al posto del server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="23732-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="23732-121">Per in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> per:</span><span class="sxs-lookup"><span data-stu-id="23732-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="23732-122">Registrare `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="23732-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="23732-123">Configurare la porta e il percorso di base su cui il server deve eseguire l'ascolto in caso di esecuzione dietro il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23732-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="23732-124">Configurare l'host per l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="23732-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="23732-125">L'[attributo requestTimeout ](#attributes-of-the-aspnetcore-element) non viene applicato all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="23732-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="23732-126">Non è supportata la condivisione di un pool di app tra le app.</span><span class="sxs-lookup"><span data-stu-id="23732-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="23732-127">Usare un pool di app per ogni app.</span><span class="sxs-lookup"><span data-stu-id="23732-127">Use one app pool per app.</span></span>

* <span data-ttu-id="23732-128">Quando si usa la [Distribuzione Web ](/iis/publish/using-web-deploy/introduction-to-web-deploy) o si inserisce manualmente un [file app_offline.htm nella distribuzione](xref:host-and-deploy/iis/index#locked-deployment-files), l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="23732-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="23732-129">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="23732-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="23732-130">L'architettura, vale a dire il numero di bit dell'app, e il runtime installato (x64 o x86) devono corrispondere all'architettura del pool di app.</span><span class="sxs-lookup"><span data-stu-id="23732-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="23732-131">Se si configura l'host dell'app manualmente con `WebHostBuilder` (non con [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) e l'app viene sempre eseguita direttamente nel server di Kestrel (self-hosted), chiamare `UseKestrel` prima di chiamare `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="23732-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="23732-132">Se l'ordine viene invertito, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="23732-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="23732-133">Le disconnessioni del client vengono rilevate.</span><span class="sxs-lookup"><span data-stu-id="23732-133">Client disconnects are detected.</span></span> <span data-ttu-id="23732-134">Il token di annullamento [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) viene cancellato quando il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="23732-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="23732-135"><xref:System.IO.Directory.GetCurrentDirectory*> restituisce la directory di lavoro del processo avviato da IIS invece che la directory dell'app (ad esempio, *C:\Windows\System32\inetsrv* per *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="23732-135"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="23732-136">Per vedere il codice di esempio che imposta la directory corrente dell'app, vedere la [classe CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="23732-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="23732-137">Chiamare il metodo `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="23732-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="23732-138">Le chiamate successive a <xref:System.IO.Directory.GetCurrentDirectory*> specificano la directory dell'app.</span><span class="sxs-lookup"><span data-stu-id="23732-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="23732-139">Modello di hosting out-of-process</span><span class="sxs-lookup"><span data-stu-id="23732-139">Out-of-process hosting model</span></span>

<span data-ttu-id="23732-140">Per configurare un'app per l'hosting out-of-process, usare uno dei due approcci seguenti nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="23732-140">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="23732-141">Non specificare la proprietà `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="23732-141">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="23732-142">Se la proprietà `<AspNetCoreHostingModel>` non è presente nel file, il valore predefinito è `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="23732-142">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="23732-143">Impostare il valore della proprietà `<AspNetCoreHostingModel>` su `OutOfProcess` (l'hosting in-process è impostato con `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="23732-143">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="23732-144">Viene usato il server [Kestrel](xref:fundamentals/servers/kestrel) al posto di un server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="23732-144">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="23732-145">Per out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> per:</span><span class="sxs-lookup"><span data-stu-id="23732-145">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="23732-146">Configurare la porta e il percorso di base su cui il server deve eseguire l'ascolto in caso di esecuzione dietro il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23732-146">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="23732-147">Configurare l'host per l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="23732-147">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="23732-148">Modifiche del modello di hosting</span><span class="sxs-lookup"><span data-stu-id="23732-148">Hosting model changes</span></span>

<span data-ttu-id="23732-149">Se l'impostazione `hostingModel` viene modificata nel file *web.config* (descritto nella sezione [Configurazione con web.config](#configuration-with-webconfig)), il modulo ricicla il processo di lavoro per IIS.</span><span class="sxs-lookup"><span data-stu-id="23732-149">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="23732-150">Per IIS Express, il modulo non ricicla il processo di lavoro. Attiva invece un arresto normale del processo di IIS Express corrente.</span><span class="sxs-lookup"><span data-stu-id="23732-150">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="23732-151">La richiesta successiva all'app attiva un nuovo processo di IIS Express.</span><span class="sxs-lookup"><span data-stu-id="23732-151">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="23732-152">Nome processo</span><span class="sxs-lookup"><span data-stu-id="23732-152">Process name</span></span>

<span data-ttu-id="23732-153">`Process.GetCurrentProcess().ProcessName` dichiara `w3wp`/`iisexpress` (In-Process) o `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="23732-153">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="23732-154">Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS che inoltra le richieste Web alle app back-end ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23732-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="23732-155">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="23732-155">Supported Windows versions:</span></span>

* <span data-ttu-id="23732-156">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="23732-156">Windows 7 or later</span></span>
* <span data-ttu-id="23732-157">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="23732-157">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="23732-158">Il modulo funziona solo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23732-158">The module only works with Kestrel.</span></span> <span data-ttu-id="23732-159">Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="23732-159">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="23732-160">Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue anche la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="23732-160">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="23732-161">Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se si verifica un arresto anomalo di questa.</span><span class="sxs-lookup"><span data-stu-id="23732-161">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="23732-162">Si tratta essenzialmente dello stesso comportamento delle app ASP.NET 4.x eseguite In-Process in IIS e gestite dal [servizio Attivazione processo Windows (WAS, Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="23732-162">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="23732-163">Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app:</span><span class="sxs-lookup"><span data-stu-id="23732-163">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="23732-165">Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel.</span><span class="sxs-lookup"><span data-stu-id="23732-165">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="23732-166">Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="23732-166">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="23732-167">Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.</span><span class="sxs-lookup"><span data-stu-id="23732-167">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="23732-168">Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il middleware di integrazione di IIS configura il server per l'ascolto su `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="23732-168">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="23732-169">Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate.</span><span class="sxs-lookup"><span data-stu-id="23732-169">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="23732-170">Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="23732-170">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="23732-171">Dopo che Kestrel ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23732-171">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="23732-172">La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app.</span><span class="sxs-lookup"><span data-stu-id="23732-172">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="23732-173">Il middleware aggiunto dall'integrazione di IIS aggiorna lo schema, l'IP remoto e il percorso di base all'account per l'inoltro della richiesta a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23732-173">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="23732-174">La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.</span><span class="sxs-lookup"><span data-stu-id="23732-174">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="23732-175">Molti moduli nativi, ad esempio l'autenticazione di Windows, rimangono attivi.</span><span class="sxs-lookup"><span data-stu-id="23732-175">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="23732-176">Per altre informazioni sui moduli IIS attivi con il modulo ASP.NET Core, vedere <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="23732-176">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="23732-177">Il modulo ASP.NET Core può anche:</span><span class="sxs-lookup"><span data-stu-id="23732-177">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="23732-178">Impostare variabili di ambiente per il processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="23732-178">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="23732-179">Registrare output stdout in una risorsa di archiviazione di file per la risoluzione di problemi di avvio.</span><span class="sxs-lookup"><span data-stu-id="23732-179">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="23732-180">Inoltrare token di autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="23732-180">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="23732-181">Come installare e usare il modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23732-181">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="23732-182">Per istruzioni su come installare e usare il modulo ASP.NET Core, vedere <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="23732-182">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="23732-183">Configurazione con web.config</span><span class="sxs-lookup"><span data-stu-id="23732-183">Configuration with web.config</span></span>

<span data-ttu-id="23732-184">Il modulo ASP.NET Core viene configurato tramite la sezione `aspNetCore` del nodo `system.webServer` nel file *web.config* del sito.</span><span class="sxs-lookup"><span data-stu-id="23732-184">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="23732-185">Il file *web.config* seguente viene pubblicato per una [distribuzione dipendente dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo ASP.NET Core per gestire le richieste del sito:</span><span class="sxs-lookup"><span data-stu-id="23732-185">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="23732-186">Il file *web.config* seguente viene pubblicato per una [distribuzione autonoma](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="23732-186">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="23732-187">La proprietà <xref:System.Configuration.SectionInformation.InheritInChildApplications*> è impostata su `false` per indicare che le impostazioni specificate nell'elemento [\<percorso>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) non sono ereditate da app che risiedono in una sottodirectory dell'app.</span><span class="sxs-lookup"><span data-stu-id="23732-187">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="23732-188">Quando un'app viene distribuita in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), il percorso `stdoutLogFile` è impostato su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="23732-188">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="23732-189">Il percorso salva i log stdout nella cartella *LogFiles*, ovvero una posizione creata automaticamente dal servizio.</span><span class="sxs-lookup"><span data-stu-id="23732-189">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="23732-190">Per informazioni sulla configurazione delle applicazioni secondarie IIS, vedere <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="23732-190">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="23732-191">Attributi dell'elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="23732-191">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="23732-192">Attributo</span><span class="sxs-lookup"><span data-stu-id="23732-192">Attribute</span></span> | <span data-ttu-id="23732-193">Descrizione</span><span class="sxs-lookup"><span data-stu-id="23732-193">Description</span></span> | <span data-ttu-id="23732-194">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="23732-194">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="23732-195">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-195">Optional string attribute.</span></span></p><p><span data-ttu-id="23732-196">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23732-196">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="23732-197">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-197">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23732-198">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="23732-198">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="23732-199">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-199">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23732-200">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="23732-200">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="23732-201">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="23732-201">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="23732-202">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-202">Optional string attribute.</span></span></p><p><span data-ttu-id="23732-203">Specifica il modello di hosting in-process (`InProcess`) o out-of-process (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="23732-203">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="23732-204">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-204">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-205">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="23732-205">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="23732-206">&dagger;Per l'hosting in-process, il valore è limitato a `1`.</span><span class="sxs-lookup"><span data-stu-id="23732-206">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="23732-207">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="23732-207">Default: `1`</span></span><br><span data-ttu-id="23732-208">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="23732-208">Min: `1`</span></span><br><span data-ttu-id="23732-209">Max: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="23732-209">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="23732-210">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="23732-210">Required string attribute.</span></span></p><p><span data-ttu-id="23732-211">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="23732-211">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="23732-212">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="23732-212">Relative paths are supported.</span></span> <span data-ttu-id="23732-213">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="23732-213">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="23732-214">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-214">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-215">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23732-215">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="23732-216">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="23732-216">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="23732-217">Non supportato con l'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="23732-217">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="23732-218">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="23732-218">Default: `10`</span></span><br><span data-ttu-id="23732-219">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="23732-219">Min: `0`</span></span><br><span data-ttu-id="23732-220">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="23732-220">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="23732-221">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-221">Optional timespan attribute.</span></span></p><p><span data-ttu-id="23732-222">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="23732-222">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="23732-223">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="23732-223">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="23732-224">Non è applicabile all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="23732-224">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="23732-225">Per l'hosting in-process, il modulo resta in attesa che l'app elabori la richiesta.</span><span class="sxs-lookup"><span data-stu-id="23732-225">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="23732-226">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="23732-226">Default: `00:02:00`</span></span><br><span data-ttu-id="23732-227">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="23732-227">Min: `00:00:00`</span></span><br><span data-ttu-id="23732-228">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="23732-228">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="23732-229">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-229">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-230">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="23732-230">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="23732-231">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="23732-231">Default: `10`</span></span><br><span data-ttu-id="23732-232">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="23732-232">Min: `0`</span></span><br><span data-ttu-id="23732-233">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="23732-233">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="23732-234">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-235">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="23732-235">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="23732-236">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="23732-236">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="23732-237">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="23732-237">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="23732-238">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="23732-238">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="23732-239">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="23732-239">Default: `120`</span></span><br><span data-ttu-id="23732-240">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="23732-240">Min: `0`</span></span><br><span data-ttu-id="23732-241">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="23732-241">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="23732-242">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-242">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23732-243">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23732-243">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="23732-244">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-244">Optional string attribute.</span></span></p><p><span data-ttu-id="23732-245">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23732-245">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="23732-246">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="23732-246">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="23732-247">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="23732-247">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="23732-248">Le eventuali cartelle specificate nel percorso vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="23732-248">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="23732-249">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23732-249">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="23732-250">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="23732-250">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="23732-251">Attributo</span><span class="sxs-lookup"><span data-stu-id="23732-251">Attribute</span></span> | <span data-ttu-id="23732-252">Descrizione</span><span class="sxs-lookup"><span data-stu-id="23732-252">Description</span></span> | <span data-ttu-id="23732-253">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="23732-253">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="23732-254">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-254">Optional string attribute.</span></span></p><p><span data-ttu-id="23732-255">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23732-255">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="23732-256">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-256">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23732-257">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="23732-257">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="23732-258">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23732-259">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="23732-259">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="23732-260">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="23732-260">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="23732-261">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-261">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-262">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="23732-262">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="23732-263">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="23732-263">Default: `1`</span></span><br><span data-ttu-id="23732-264">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="23732-264">Min: `1`</span></span><br><span data-ttu-id="23732-265">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="23732-265">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="23732-266">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="23732-266">Required string attribute.</span></span></p><p><span data-ttu-id="23732-267">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="23732-267">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="23732-268">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="23732-268">Relative paths are supported.</span></span> <span data-ttu-id="23732-269">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="23732-269">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="23732-270">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-270">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-271">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23732-271">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="23732-272">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="23732-272">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="23732-273">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="23732-273">Default: `10`</span></span><br><span data-ttu-id="23732-274">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="23732-274">Min: `0`</span></span><br><span data-ttu-id="23732-275">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="23732-275">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="23732-276">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-276">Optional timespan attribute.</span></span></p><p><span data-ttu-id="23732-277">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="23732-277">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="23732-278">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="23732-278">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="23732-279">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="23732-279">Default: `00:02:00`</span></span><br><span data-ttu-id="23732-280">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="23732-280">Min: `00:00:00`</span></span><br><span data-ttu-id="23732-281">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="23732-281">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="23732-282">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-282">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-283">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="23732-283">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="23732-284">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="23732-284">Default: `10`</span></span><br><span data-ttu-id="23732-285">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="23732-285">Min: `0`</span></span><br><span data-ttu-id="23732-286">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="23732-286">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="23732-287">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-288">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="23732-288">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="23732-289">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="23732-289">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="23732-290">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="23732-290">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="23732-291">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="23732-291">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="23732-292">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="23732-292">Default: `120`</span></span><br><span data-ttu-id="23732-293">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="23732-293">Min: `0`</span></span><br><span data-ttu-id="23732-294">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="23732-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="23732-295">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23732-296">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23732-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="23732-297">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-297">Optional string attribute.</span></span></p><p><span data-ttu-id="23732-298">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23732-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="23732-299">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="23732-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="23732-300">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="23732-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="23732-301">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="23732-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="23732-302">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23732-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="23732-303">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="23732-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="23732-304">Attributo</span><span class="sxs-lookup"><span data-stu-id="23732-304">Attribute</span></span> | <span data-ttu-id="23732-305">Descrizione</span><span class="sxs-lookup"><span data-stu-id="23732-305">Description</span></span> | <span data-ttu-id="23732-306">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="23732-306">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="23732-307">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-307">Optional string attribute.</span></span></p><p><span data-ttu-id="23732-308">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23732-308">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="23732-309">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-309">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23732-310">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="23732-310">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="23732-311">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-311">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23732-312">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="23732-312">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="23732-313">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="23732-313">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="23732-314">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-314">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-315">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="23732-315">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="23732-316">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="23732-316">Default: `1`</span></span><br><span data-ttu-id="23732-317">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="23732-317">Min: `1`</span></span><br><span data-ttu-id="23732-318">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="23732-318">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="23732-319">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="23732-319">Required string attribute.</span></span></p><p><span data-ttu-id="23732-320">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="23732-320">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="23732-321">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="23732-321">Relative paths are supported.</span></span> <span data-ttu-id="23732-322">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="23732-322">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="23732-323">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-323">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-324">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23732-324">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="23732-325">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="23732-325">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="23732-326">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="23732-326">Default: `10`</span></span><br><span data-ttu-id="23732-327">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="23732-327">Min: `0`</span></span><br><span data-ttu-id="23732-328">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="23732-328">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="23732-329">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-329">Optional timespan attribute.</span></span></p><p><span data-ttu-id="23732-330">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="23732-330">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="23732-331">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.0 o versioni precedenti, `requestTimeout` deve essere specificato solo in minuti interi. In caso contrario, verrà usato il valore predefinito di 2 minuti.</span><span class="sxs-lookup"><span data-stu-id="23732-331">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="23732-332">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="23732-332">Default: `00:02:00`</span></span><br><span data-ttu-id="23732-333">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="23732-333">Min: `00:00:00`</span></span><br><span data-ttu-id="23732-334">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="23732-334">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="23732-335">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-335">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-336">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="23732-336">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="23732-337">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="23732-337">Default: `10`</span></span><br><span data-ttu-id="23732-338">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="23732-338">Min: `0`</span></span><br><span data-ttu-id="23732-339">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="23732-339">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="23732-340">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-340">Optional integer attribute.</span></span></p><p><span data-ttu-id="23732-341">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="23732-341">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="23732-342">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="23732-342">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="23732-343">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="23732-343">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="23732-344">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="23732-344">Default: `120`</span></span><br><span data-ttu-id="23732-345">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="23732-345">Min: `0`</span></span><br><span data-ttu-id="23732-346">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="23732-346">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="23732-347">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-347">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23732-348">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23732-348">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="23732-349">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="23732-349">Optional string attribute.</span></span></p><p><span data-ttu-id="23732-350">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23732-350">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="23732-351">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="23732-351">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="23732-352">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="23732-352">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="23732-353">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="23732-353">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="23732-354">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23732-354">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="23732-355">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="23732-355">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="23732-356">Impostazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="23732-356">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="23732-357">È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="23732-357">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="23732-358">Specificare una variabile di ambiente con l'elemento figlio `<environmentVariable>` di un elemento della raccolta `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="23732-358">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="23732-359">Le variabili di ambiente impostate in questa sezione hanno la precedenza sulle variabili di ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="23732-359">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="23732-360">È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="23732-360">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="23732-361">Specificare una variabile di ambiente con l'elemento figlio `<environmentVariable>` di un elemento della raccolta `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="23732-361">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="23732-362">Le variabili di ambiente impostate in questa sezione sono in conflitto con le variabili di ambiente di sistema impostate con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="23732-362">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="23732-363">Se una variabile di ambiente è impostata sia nel file *web.config* sia a livello di sistema in Windows, il valore del file *web.config* viene aggiunto al valore della variabile di ambiente di sistema, ad esempio `ASPNETCORE_ENVIRONMENT: Development;Development`, e ciò impedisce l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="23732-363">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="23732-364">Nell'esempio seguente vengono impostate due variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="23732-364">The following example sets two environment variables.</span></span> <span data-ttu-id="23732-365">`ASPNETCORE_ENVIRONMENT` configura l'ambiente dell'app su `Development`.</span><span class="sxs-lookup"><span data-stu-id="23732-365">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="23732-366">Uno sviluppatore può impostare temporaneamente questo valore nel file *web.config* per forzare il caricamento della [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) durante il debug di un'eccezione dell'app.</span><span class="sxs-lookup"><span data-stu-id="23732-366">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="23732-367">`CONFIG_DIR` è un esempio di variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento del file di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="23732-367">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="23732-368">Un'alternativa all'impostazione dell'ambiente direttamente in *web.config* è quella di includere la proprietà `<EnvironmentName>` nel profilo di pubblicazione (*.pubxml*) o nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="23732-368">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="23732-369">Questo approccio imposta l'ambiente in *web.config* quando viene pubblicato il progetto:</span><span class="sxs-lookup"><span data-stu-id="23732-369">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="23732-370">Impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` su `Development` solo in server di gestione temporanea e test che non sono accessibili da reti non attendibili, ad esempio Internet.</span><span class="sxs-lookup"><span data-stu-id="23732-370">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="23732-371">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="23732-371">app_offline.htm</span></span>

<span data-ttu-id="23732-372">Se un file denominato *app_offline.htm* viene rilevato nella directory radice di un'app, il modulo ASP.NET Core tenta di arrestare normalmente l'app e arresta l'elaborazione delle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="23732-372">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="23732-373">Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo ASP.NET Core termina il processo in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="23732-373">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="23732-374">Mentre il file *app_offline.htm* è presente, il modulo ASP.NET Core risponde alle richieste restituendo il contenuto del file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="23732-374">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="23732-375">Quando il file *app_offline.htm* viene rimosso, la richiesta successiva avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="23732-375">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23732-376">Quando si usa il modello di hosting out-of-process, l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="23732-376">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="23732-377">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="23732-377">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="23732-378">Pagina di errore di avvio</span><span class="sxs-lookup"><span data-stu-id="23732-378">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23732-379">Sia l'hosting In-Process che quello out-of-process producono pagine di errore personalizzate quando non riescono ad avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="23732-379">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="23732-380">Se il modulo ASP.NET Core non riesce a trovare il gestore richieste In-Process o out-of-process, viene visualizzata una tabella codici di stato *500.0 - Errore di caricamento del gestore In-Process/out-of-process*.</span><span class="sxs-lookup"><span data-stu-id="23732-380">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="23732-381">Per l'hosting In-Process, se il modulo ASP.NET Core non riesce ad avviare l'app, viene visualizzata una tabella codici di stato *500.30 - Errore di avvio*.</span><span class="sxs-lookup"><span data-stu-id="23732-381">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="23732-382">Per l'hosting out-of-process, se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="23732-382">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="23732-383">Per non visualizzare questa pagina e tornare alla tabella codici di stato 5xx predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="23732-383">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="23732-384">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="23732-384">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="23732-385">Se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="23732-385">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="23732-386">Per non visualizzare questa pagina e tornare alla tabella codici di stato 502 predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="23732-386">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="23732-387">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="23732-387">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Tabella codici di stato 502.5 Errore del processo](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="23732-389">Creazione e reindirizzamento dei log</span><span class="sxs-lookup"><span data-stu-id="23732-389">Log creation and redirection</span></span>

<span data-ttu-id="23732-390">Il modulo ASP.NET Core reindirizza su disco l'output della console stdout e stderr se sono impostati gli attributi `stdoutLogEnabled` e `stdoutLogFile` dell'elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="23732-390">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="23732-391">Le eventuali le cartelle nel percorso `stdoutLogFile` vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="23732-391">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="23732-392">Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).</span><span class="sxs-lookup"><span data-stu-id="23732-392">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="23732-393">I log non vengono ruotati, a meno che non si verifichi il riciclo/riavvio del processo.</span><span class="sxs-lookup"><span data-stu-id="23732-393">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="23732-394">È responsabilità del provider di servizi di hosting limitare lo spazio su disco usato dai log.</span><span class="sxs-lookup"><span data-stu-id="23732-394">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="23732-395">L'uso del log stdout è consigliato solo per la risoluzione dei problemi di avvio delle app.</span><span class="sxs-lookup"><span data-stu-id="23732-395">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="23732-396">Non usare il log stdout per scopi di registrazione generale delle app.</span><span class="sxs-lookup"><span data-stu-id="23732-396">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="23732-397">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="23732-397">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="23732-398">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="23732-398">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="23732-399">Un timestamp e l'estensione del file vengono aggiunti automaticamente al momento della creazione del file di log.</span><span class="sxs-lookup"><span data-stu-id="23732-399">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="23732-400">Il nome del file di log è composto aggiungendo il timestamp, l'ID processo e l'estensione del file (*.log*) all'ultimo segmento del percorso `stdoutLogFile` (in genere *stdout*), con caratteri di sottolineatura come delimitatori.</span><span class="sxs-lookup"><span data-stu-id="23732-400">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="23732-401">Se il percorso `stdoutLogFile` termina con *stdout*, un log per un'app con un PID 1934 creata il 5/2/2018 alle 19:42:32 sarà denominato *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="23732-401">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23732-402">Se `stdoutLogEnabled` è false, gli errori che si verificano all'avvio dell'app vengono acquisiti ed emessi nel log eventi fino a 30 KB.</span><span class="sxs-lookup"><span data-stu-id="23732-402">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="23732-403">Dopo l'avvio, tutti i log aggiuntivi vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="23732-403">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="23732-404">L'elemento di esempio `aspNetCore` seguente configura la registrazione stdout per un'app ospitata in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="23732-404">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="23732-405">Un percorso locale o un percorso di una condivisione di rete è accettabile per la registrazione locale.</span><span class="sxs-lookup"><span data-stu-id="23732-405">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="23732-406">Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="23732-406">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="23732-407">Log di diagnostica avanzati</span><span class="sxs-lookup"><span data-stu-id="23732-407">Enhanced diagnostic logs</span></span>

<span data-ttu-id="23732-408">Il modulo ASP.NET Core può essere configurato per restituire log di diagnostica avanzata.</span><span class="sxs-lookup"><span data-stu-id="23732-408">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="23732-409">Aggiungere l'elemento `<handlerSettings>` all'elemento `<aspNetCore>` in *web.config*. L'impostazione di `debugLevel` su `TRACE` restituisce una maggior fedeltà dei dati diagnostici:</span><span class="sxs-lookup"><span data-stu-id="23732-409">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="23732-410">I valori di livello debug (`debugLevel`) possono includere sia il livello che il percorso.</span><span class="sxs-lookup"><span data-stu-id="23732-410">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="23732-411">Livelli (in ordine dal meno dettagliato al più dettagliato):</span><span class="sxs-lookup"><span data-stu-id="23732-411">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="23732-412">ERROR</span><span class="sxs-lookup"><span data-stu-id="23732-412">ERROR</span></span>
* <span data-ttu-id="23732-413">WARNING</span><span class="sxs-lookup"><span data-stu-id="23732-413">WARNING</span></span>
* <span data-ttu-id="23732-414">INFO</span><span class="sxs-lookup"><span data-stu-id="23732-414">INFO</span></span>
* <span data-ttu-id="23732-415">TRACE</span><span class="sxs-lookup"><span data-stu-id="23732-415">TRACE</span></span>

<span data-ttu-id="23732-416">Posizioni (sono consentite più posizioni):</span><span class="sxs-lookup"><span data-stu-id="23732-416">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="23732-417">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="23732-417">CONSOLE</span></span>
* <span data-ttu-id="23732-418">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="23732-418">EVENTLOG</span></span>
* <span data-ttu-id="23732-419">FILE</span><span class="sxs-lookup"><span data-stu-id="23732-419">FILE</span></span>

<span data-ttu-id="23732-420">Le impostazioni del gestore possono essere specificate anche tramite le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="23732-420">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="23732-421">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Percorso del file di logo di debug.</span><span class="sxs-lookup"><span data-stu-id="23732-421">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="23732-422">(Impostazione predefinita: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="23732-422">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="23732-423">`ASPNETCORE_MODULE_DEBUG` &ndash; Impostazione del livello di debug.</span><span class="sxs-lookup"><span data-stu-id="23732-423">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="23732-424">**Non** lasciare la registrazione del debug abilitata nella distribuzione per un tempo superiore a quello necessario alla risoluzione di problema.</span><span class="sxs-lookup"><span data-stu-id="23732-424">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="23732-425">Le dimensioni del log non sono limitate.</span><span class="sxs-lookup"><span data-stu-id="23732-425">The size of the log isn't limited.</span></span> <span data-ttu-id="23732-426">Se si lascia abilitato il log di debug, lo spazio disponibile su disco può esaurirsi e il server o il servizio app può registrare un arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="23732-426">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="23732-427">Vedere [Configurazione con web.config](#configuration-with-webconfig) per un esempio dell'elemento `aspNetCore` nel file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="23732-427">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="23732-428">La configurazione del proxy usa il protocollo HTTP e un token di associazione</span><span class="sxs-lookup"><span data-stu-id="23732-428">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23732-429">*Si applica solo all'hosting out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="23732-429">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="23732-430">Il proxy creato tra il modulo ASP.NET Core e Kestrel usa il protocollo HTTP.</span><span class="sxs-lookup"><span data-stu-id="23732-430">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="23732-431">L'uso di HTTP è un'ottimizzazione delle prestazioni in cui il traffico tra il modulo e Kestrel avviene in un indirizzo di loopback dell'interfaccia di rete.</span><span class="sxs-lookup"><span data-stu-id="23732-431">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="23732-432">Non vi è alcun rischio di intercettazione del traffico tra il modulo e Kestrel da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="23732-432">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="23732-433">Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine.</span><span class="sxs-lookup"><span data-stu-id="23732-433">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="23732-434">Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo.</span><span class="sxs-lookup"><span data-stu-id="23732-434">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="23732-435">Il token di associazione viene impostato anche in un'intestazione (`MS-ASPNETCORE-TOKEN`) in tutte le richieste trasmesse tramite proxy.</span><span class="sxs-lookup"><span data-stu-id="23732-435">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="23732-436">Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="23732-436">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="23732-437">Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata.</span><span class="sxs-lookup"><span data-stu-id="23732-437">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="23732-438">La variabile di ambiente del token di associazione e il traffico tra il modulo e Kestrel non sono accessibili da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="23732-438">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="23732-439">Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.</span><span class="sxs-lookup"><span data-stu-id="23732-439">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="23732-440">Modulo ASP.NET Core con configurazione condivisa di IIS</span><span class="sxs-lookup"><span data-stu-id="23732-440">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="23732-441">Il programma di installazione del modulo ASP.NET Core viene eseguito con i privilegi dell'account **SYSTEM**.</span><span class="sxs-lookup"><span data-stu-id="23732-441">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="23732-442">Poiché l'account di sistema locale non dispone dell'autorizzazione di modifica per il percorso della condivisione usato dalla configurazione condivisa di IIS, il programma di installazione rileva un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo in *applicationHost.config* nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="23732-442">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="23732-443">Quando si usa una configurazione condivisa di IIS, attenersi ai passaggi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="23732-443">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="23732-444">Disabilitare la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="23732-444">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="23732-445">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="23732-445">Run the installer.</span></span>
1. <span data-ttu-id="23732-446">Esportare il file *applicationHost.config* aggiornato nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="23732-446">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="23732-447">Abilitare di nuovo la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="23732-447">Re-enable the IIS Shared Configuration.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization"></a><span data-ttu-id="23732-448">Inizializzazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="23732-448">Application Initialization</span></span>

<span data-ttu-id="23732-449">[Inizializzazione applicazione di IIS](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) è una funzionalità di IIS che invia una richiesta HTTP all'app quando il pool di app viene avviato o riciclato.</span><span class="sxs-lookup"><span data-stu-id="23732-449">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="23732-450">La richiesta attiva l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="23732-450">The request triggers the app to start.</span></span> <span data-ttu-id="23732-451">La funzionalità Inizializzazione applicazione può essere usata sia dal [modello di hosting in-process](xref:fundamentals/servers/index#in-process-hosting-model) che dal e [modello di hosting out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model) con il modulo ASP.NET Core versione 2.</span><span class="sxs-lookup"><span data-stu-id="23732-451">Application Initialization can be used by both the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model) and [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) with the ASP.NET Core Module version 2.</span></span>

<span data-ttu-id="23732-452">Per abilitare Inizializzazione applicazione:</span><span class="sxs-lookup"><span data-stu-id="23732-452">To enable Application Initialization:</span></span>

1. <span data-ttu-id="23732-453">Verificare che la funzionalità del ruolo Inizializzazione applicazione di IIS sia abilitata:</span><span class="sxs-lookup"><span data-stu-id="23732-453">Confirm that the IIS Application Initialization role feature in enabled:</span></span>
   * <span data-ttu-id="23732-454">In Windows 7 o versione successiva: Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="23732-454">On Windows 7 or later: Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="23732-455">Aprire **Internet Information Services** > **Servizi Web** > **Funzionalità per lo sviluppo di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="23732-455">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="23732-456">Selezionare la casella di controllo **Inizializzazione applicazione**.</span><span class="sxs-lookup"><span data-stu-id="23732-456">Select the check box for **Application Initialization**.</span></span>
   * <span data-ttu-id="23732-457">In Windows Server 2008 R2 o versioni successive aprire **Aggiunta guidata ruoli e funzionalità**.</span><span class="sxs-lookup"><span data-stu-id="23732-457">On Windows Server 2008 R2 or later, open the **Add Roles and Features Wizard**.</span></span> <span data-ttu-id="23732-458">Quando si raggiunge il pannello **Selezione servizi ruolo**, aprire il nodo **Sviluppo applicazioni** e selezionare la casella di controllo **Inizializzazione applicazione**.</span><span class="sxs-lookup"><span data-stu-id="23732-458">When you reach the **Select role services** panel, open the **Application Development** node and select the **Application Initialization** check box.</span></span>
1. <span data-ttu-id="23732-459">In Gestione IIS selezionare **Pool di applicazioni** nel pannello **Connessioni**.</span><span class="sxs-lookup"><span data-stu-id="23732-459">In IIS Manager, select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="23732-460">Selezionare il pool di app dell'app nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="23732-460">Select the app's app pool in the list.</span></span>
1. <span data-ttu-id="23732-461">Selezionare **Impostazioni avanzate** in **Modifica pool di applicazioni** nel pannello **Azioni**.</span><span class="sxs-lookup"><span data-stu-id="23732-461">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="23732-462">Impostare **Modalità di avvio** su **AlwaysRunning**.</span><span class="sxs-lookup"><span data-stu-id="23732-462">Set **Start Mode** to **AlwaysRunning**.</span></span>
1. <span data-ttu-id="23732-463">Aprire il nodo **Siti** nel pannello **Connessioni**.</span><span class="sxs-lookup"><span data-stu-id="23732-463">Open the **Sites** node in the **Connections** panel.</span></span>
1. <span data-ttu-id="23732-464">Selezionare l'app.</span><span class="sxs-lookup"><span data-stu-id="23732-464">Select the app.</span></span>
1. <span data-ttu-id="23732-465">Selezionare **Impostazioni avanzate** in **Gestisci sito Web** nel pannello **Azioni**.</span><span class="sxs-lookup"><span data-stu-id="23732-465">Select **Advanced Settings** under **Manage Website** in the **Actions** panel.</span></span>
1. <span data-ttu-id="23732-466">Impostare **Precaricamento abilitato** su **True**.</span><span class="sxs-lookup"><span data-stu-id="23732-466">Set **Preload Enabled** to **True**.</span></span>

<span data-ttu-id="23732-467">Per altre informazioni, vedere [IIS 8.0 Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) (Inizializzazione applicazione IIS 8.0).</span><span class="sxs-lookup"><span data-stu-id="23732-467">For more information, see [IIS 8.0 Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span></span>

<span data-ttu-id="23732-468">Le app che usano il [modello di hosting out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model) devono usare un servizio esterno per eseguire periodicamente il ping dell'app per mantenerla in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="23732-468">Apps that use the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) must use an external service to periodically ping the app in order to keep it running.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="23732-469">Versione del modulo e log del programma di installazione del bundle di hosting</span><span class="sxs-lookup"><span data-stu-id="23732-469">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="23732-470">Per determinare la versione del modulo ASP.NET Core installato:</span><span class="sxs-lookup"><span data-stu-id="23732-470">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="23732-471">Nel sistema host passare a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="23732-471">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="23732-472">Individuare il file *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="23732-472">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="23732-473">Fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="23732-473">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="23732-474">Selezionare la scheda **Dettagli**. **Versione file** e **Versione prodotto** rappresentano la versione installata del modulo.</span><span class="sxs-lookup"><span data-stu-id="23732-474">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="23732-475">I log del programma di installazione del bundle di hosting per il modulo sono disponibili in *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Il file è denominato *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="23732-475">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="23732-476">Percorsi dei file di modulo, schema e configurazione</span><span class="sxs-lookup"><span data-stu-id="23732-476">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="23732-477">Modulo</span><span class="sxs-lookup"><span data-stu-id="23732-477">Module</span></span>

<span data-ttu-id="23732-478">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="23732-478">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="23732-479">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="23732-479">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="23732-480">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="23732-480">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="23732-481">%Programmi%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="23732-481">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="23732-482">%Programmi(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="23732-482">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="23732-483">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="23732-483">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="23732-484">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="23732-484">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="23732-485">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="23732-485">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="23732-486">%Programmi%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="23732-486">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="23732-487">%Programmi(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="23732-487">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="23732-488">Schema</span><span class="sxs-lookup"><span data-stu-id="23732-488">Schema</span></span>

<span data-ttu-id="23732-489">**IIS**</span><span class="sxs-lookup"><span data-stu-id="23732-489">**IIS**</span></span>

   * <span data-ttu-id="23732-490">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="23732-490">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="23732-491">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="23732-491">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="23732-492">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="23732-492">**IIS Express**</span></span>

   * <span data-ttu-id="23732-493">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="23732-493">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="23732-494">%Programmi%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="23732-494">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="23732-495">Configurazione</span><span class="sxs-lookup"><span data-stu-id="23732-495">Configuration</span></span>

<span data-ttu-id="23732-496">**IIS**</span><span class="sxs-lookup"><span data-stu-id="23732-496">**IIS**</span></span>

   * <span data-ttu-id="23732-497">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="23732-497">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="23732-498">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="23732-498">**IIS Express**</span></span>

   * <span data-ttu-id="23732-499">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="23732-499">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="23732-500">Interfaccia della riga di comando di *iisexpress.exe*: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="23732-500">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="23732-501">È possibile trovare i file eseguendo una ricerca di *aspnetcore* nel file *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="23732-501">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="23732-502">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="23732-502">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="23732-503">Repository GitHub del modulo ASP.NET Core (origine riferimento)</span><span class="sxs-lookup"><span data-stu-id="23732-503">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
