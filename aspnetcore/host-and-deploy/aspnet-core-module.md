---
title: Modulo ASP.NET Core
author: guardrex
description: Informazioni su come configurare il modulo di ASP.NET Core per l'hosting di app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 11906f34f4aa358fda126772e2147dc805c28e81
ms.sourcegitcommit: 06c4f2910dd54ded25e1b8750e09c66578748bc9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66395938"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="7e538-103">Modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e538-103">ASP.NET Core Module</span></span>

<span data-ttu-id="7e538-104">Di [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7e538-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e538-105">Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e538-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="7e538-106">Ospitare un app ASP.NET Core all'interno del processo di lavoro IIS (`w3wp.exe`), denominato [modello di hosting in-process](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="7e538-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="7e538-107">Inoltrare le richieste Web all'app back-end ASP.NET Core che esegue il [server Kestrel](xref:fundamentals/servers/kestrel), denominato [modello di hosting out-of-process](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="7e538-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="7e538-108">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="7e538-108">Supported Windows versions:</span></span>

* <span data-ttu-id="7e538-109">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="7e538-109">Windows 7 or later</span></span>
* <span data-ttu-id="7e538-110">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="7e538-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="7e538-111">In caso di hosting in-process, il modulo usa un'implementazione di server in-process per IIS detta server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="7e538-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="7e538-112">In caso di hosting out-of-process, il modulo funziona solo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7e538-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="7e538-113">Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="7e538-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="7e538-114">Modelli di hosting</span><span class="sxs-lookup"><span data-stu-id="7e538-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="7e538-115">Modello di hosting in-process</span><span class="sxs-lookup"><span data-stu-id="7e538-115">In-process hosting model</span></span>

<span data-ttu-id="7e538-116">Per configurare un'app per l'hosting in-process, aggiungere la proprietà `<AspNetCoreHostingModel>` al file di progetto dell'app usando il valore `InProcess` (l'hosting out-of-process viene impostato con `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="7e538-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="7e538-117">Il modello di hosting In-Process non è supportato per le app ASP.NET Core destinate a .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7e538-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="7e538-118">Se la proprietà `<AspNetCoreHostingModel>` non è presente nel file, il valore predefinito è `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="7e538-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="7e538-119">In caso di hosting in-process, vengono applicate le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e538-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="7e538-120">Viene usato un server HTTP di IIS (`IISHttpServer`) al posto del server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="7e538-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="7e538-121">Per in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> per:</span><span class="sxs-lookup"><span data-stu-id="7e538-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="7e538-122">Registrare `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="7e538-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="7e538-123">Configurare la porta e il percorso di base su cui il server deve eseguire l'ascolto in caso di esecuzione dietro il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7e538-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="7e538-124">Configurare l'host per l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="7e538-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="7e538-125">L'[attributo requestTimeout ](#attributes-of-the-aspnetcore-element) non viene applicato all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="7e538-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="7e538-126">Non è supportata la condivisione di un pool di app tra le app.</span><span class="sxs-lookup"><span data-stu-id="7e538-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="7e538-127">Usare un pool di app per ogni app.</span><span class="sxs-lookup"><span data-stu-id="7e538-127">Use one app pool per app.</span></span>

* <span data-ttu-id="7e538-128">Quando si usa la [Distribuzione Web ](/iis/publish/using-web-deploy/introduction-to-web-deploy) o si inserisce manualmente un [file app_offline.htm nella distribuzione](xref:host-and-deploy/iis/index#locked-deployment-files), l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="7e538-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="7e538-129">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="7e538-130">L'architettura, vale a dire il numero di bit dell'app, e il runtime installato (x64 o x86) devono corrispondere all'architettura del pool di app.</span><span class="sxs-lookup"><span data-stu-id="7e538-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="7e538-131">Se si configura l'host dell'app manualmente con `WebHostBuilder` (non con [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) e l'app viene sempre eseguita direttamente nel server di Kestrel (self-hosted), chiamare `UseKestrel` prima di chiamare `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="7e538-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="7e538-132">Se l'ordine viene invertito, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="7e538-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="7e538-133">Le disconnessioni del client vengono rilevate.</span><span class="sxs-lookup"><span data-stu-id="7e538-133">Client disconnects are detected.</span></span> <span data-ttu-id="7e538-134">Il token di annullamento [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) viene cancellato quando il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="7e538-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="7e538-135">In ASP.NET Core 2.2.1 o versioni precedenti, <xref:System.IO.Directory.GetCurrentDirectory*> restituisce la directory di lavoro del processo avviato da IIS invece che la directory dell'app (ad esempio, *C:\Windows\System32\inetsrv* per *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="7e538-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="7e538-136">Per vedere il codice di esempio che imposta la directory corrente dell'app, vedere la [classe CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="7e538-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="7e538-137">Chiamare il metodo `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="7e538-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="7e538-138">Le chiamate successive a <xref:System.IO.Directory.GetCurrentDirectory*> specificano la directory dell'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="7e538-139">In caso di hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> non viene chiamato internamente per inizializzare un utente.</span><span class="sxs-lookup"><span data-stu-id="7e538-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="7e538-140">Pertanto, un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> usate per trasformare le attestazioni dopo ogni autenticazione non viene attivata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="7e538-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="7e538-141">Quando si trasformano le attestazioni con un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, chiamare <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> per aggiungere i servizi di autenticazione:</span><span class="sxs-lookup"><span data-stu-id="7e538-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="7e538-142">Modello di hosting out-of-process</span><span class="sxs-lookup"><span data-stu-id="7e538-142">Out-of-process hosting model</span></span>

<span data-ttu-id="7e538-143">Per configurare un'app per l'hosting out-of-process, usare uno dei due approcci seguenti nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="7e538-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="7e538-144">Non specificare la proprietà `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="7e538-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="7e538-145">Se la proprietà `<AspNetCoreHostingModel>` non è presente nel file, il valore predefinito è `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="7e538-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="7e538-146">Impostare il valore della proprietà `<AspNetCoreHostingModel>` su `OutOfProcess` (l'hosting in-process è impostato con `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="7e538-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="7e538-147">Viene usato il server [Kestrel](xref:fundamentals/servers/kestrel) al posto di un server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="7e538-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="7e538-148">Per out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> per:</span><span class="sxs-lookup"><span data-stu-id="7e538-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="7e538-149">Configurare la porta e il percorso di base su cui il server deve eseguire l'ascolto in caso di esecuzione dietro il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7e538-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="7e538-150">Configurare l'host per l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="7e538-150">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="7e538-151">Modifiche del modello di hosting</span><span class="sxs-lookup"><span data-stu-id="7e538-151">Hosting model changes</span></span>

<span data-ttu-id="7e538-152">Se l'impostazione `hostingModel` viene modificata nel file *web.config* (descritto nella sezione [Configurazione con web.config](#configuration-with-webconfig)), il modulo ricicla il processo di lavoro per IIS.</span><span class="sxs-lookup"><span data-stu-id="7e538-152">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="7e538-153">Per IIS Express, il modulo non ricicla il processo di lavoro. Attiva invece un arresto normale del processo di IIS Express corrente.</span><span class="sxs-lookup"><span data-stu-id="7e538-153">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="7e538-154">La richiesta successiva all'app attiva un nuovo processo di IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7e538-154">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="7e538-155">Nome processo</span><span class="sxs-lookup"><span data-stu-id="7e538-155">Process name</span></span>

<span data-ttu-id="7e538-156">`Process.GetCurrentProcess().ProcessName` dichiara `w3wp`/`iisexpress` (In-Process) o `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="7e538-156">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7e538-157">Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS che inoltra le richieste Web alle app back-end ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7e538-157">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="7e538-158">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="7e538-158">Supported Windows versions:</span></span>

* <span data-ttu-id="7e538-159">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="7e538-159">Windows 7 or later</span></span>
* <span data-ttu-id="7e538-160">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="7e538-160">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="7e538-161">Il modulo funziona solo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7e538-161">The module only works with Kestrel.</span></span> <span data-ttu-id="7e538-162">Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="7e538-162">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="7e538-163">Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue anche la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="7e538-163">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="7e538-164">Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se si verifica un arresto anomalo di questa.</span><span class="sxs-lookup"><span data-stu-id="7e538-164">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="7e538-165">Si tratta essenzialmente dello stesso comportamento delle app ASP.NET 4.x eseguite In-Process in IIS e gestite dal [servizio Attivazione processo Windows (WAS, Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="7e538-165">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="7e538-166">Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app:</span><span class="sxs-lookup"><span data-stu-id="7e538-166">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="7e538-168">Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel.</span><span class="sxs-lookup"><span data-stu-id="7e538-168">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="7e538-169">Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7e538-169">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="7e538-170">Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.</span><span class="sxs-lookup"><span data-stu-id="7e538-170">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="7e538-171">Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il [middleware di integrazione IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configura il server per l'ascolto su `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="7e538-171">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="7e538-172">Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate.</span><span class="sxs-lookup"><span data-stu-id="7e538-172">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="7e538-173">Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7e538-173">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="7e538-174">Dopo che Kestrel ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7e538-174">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="7e538-175">La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-175">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="7e538-176">Il middleware aggiunto dall'integrazione di IIS aggiorna lo schema, l'IP remoto e il percorso di base all'account per l'inoltro della richiesta a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7e538-176">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="7e538-177">La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.</span><span class="sxs-lookup"><span data-stu-id="7e538-177">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="7e538-178">Molti moduli nativi, ad esempio l'autenticazione di Windows, rimangono attivi.</span><span class="sxs-lookup"><span data-stu-id="7e538-178">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="7e538-179">Per altre informazioni sui moduli IIS attivi con il modulo ASP.NET Core, vedere <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="7e538-179">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="7e538-180">Il modulo ASP.NET Core può anche:</span><span class="sxs-lookup"><span data-stu-id="7e538-180">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="7e538-181">Impostare variabili di ambiente per il processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="7e538-181">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="7e538-182">Registrare output stdout in una risorsa di archiviazione di file per la risoluzione di problemi di avvio.</span><span class="sxs-lookup"><span data-stu-id="7e538-182">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="7e538-183">Inoltrare token di autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="7e538-183">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="7e538-184">Come installare e usare il modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e538-184">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="7e538-185">Per istruzioni su come installare e usare il modulo ASP.NET Core, vedere <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="7e538-185">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="7e538-186">Configurazione con web.config</span><span class="sxs-lookup"><span data-stu-id="7e538-186">Configuration with web.config</span></span>

<span data-ttu-id="7e538-187">Il modulo ASP.NET Core viene configurato tramite la sezione `aspNetCore` del nodo `system.webServer` nel file *web.config* del sito.</span><span class="sxs-lookup"><span data-stu-id="7e538-187">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="7e538-188">Il file *web.config* seguente viene pubblicato per una [distribuzione dipendente dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo ASP.NET Core per gestire le richieste del sito:</span><span class="sxs-lookup"><span data-stu-id="7e538-188">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="7e538-189">Il file *web.config* seguente viene pubblicato per una [distribuzione autonoma](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="7e538-189">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="7e538-190">La proprietà <xref:System.Configuration.SectionInformation.InheritInChildApplications*> è impostata su `false` per indicare che le impostazioni specificate nell'elemento [\<percorso>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) non sono ereditate da app che risiedono in una sottodirectory dell'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-190">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="7e538-191">Quando un'app viene distribuita in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), il percorso `stdoutLogFile` è impostato su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="7e538-191">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="7e538-192">Il percorso salva i log stdout nella cartella *LogFiles*, ovvero una posizione creata automaticamente dal servizio.</span><span class="sxs-lookup"><span data-stu-id="7e538-192">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="7e538-193">Per informazioni sulla configurazione delle applicazioni secondarie IIS, vedere <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="7e538-193">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="7e538-194">Attributi dell'elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="7e538-194">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="7e538-195">Attributo</span><span class="sxs-lookup"><span data-stu-id="7e538-195">Attribute</span></span> | <span data-ttu-id="7e538-196">Description</span><span class="sxs-lookup"><span data-stu-id="7e538-196">Description</span></span> | <span data-ttu-id="7e538-197">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="7e538-197">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="7e538-198">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-198">Optional string attribute.</span></span></p><p><span data-ttu-id="7e538-199">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="7e538-199">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="7e538-200">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-200">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="7e538-201">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="7e538-201">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="7e538-202">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-202">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="7e538-203">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="7e538-203">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="7e538-204">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="7e538-204">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="7e538-205">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-205">Optional string attribute.</span></span></p><p><span data-ttu-id="7e538-206">Specifica il modello di hosting in-process (`InProcess`) o out-of-process (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="7e538-206">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="7e538-207">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="7e538-208">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="7e538-208">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="7e538-209">&dagger;Per l'hosting in-process, il valore è limitato a `1`.</span><span class="sxs-lookup"><span data-stu-id="7e538-209">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="7e538-210">L'impostazione di `processesPerApplication` è sconsigliata.</span><span class="sxs-lookup"><span data-stu-id="7e538-210">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="7e538-211">Questo attributo sarà rimosso nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="7e538-211">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="7e538-212">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="7e538-212">Default: `1`</span></span><br><span data-ttu-id="7e538-213">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="7e538-213">Min: `1`</span></span><br><span data-ttu-id="7e538-214">Max: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="7e538-214">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="7e538-215">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="7e538-215">Required string attribute.</span></span></p><p><span data-ttu-id="7e538-216">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e538-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="7e538-217">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="7e538-217">Relative paths are supported.</span></span> <span data-ttu-id="7e538-218">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="7e538-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="7e538-219">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="7e538-220">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="7e538-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="7e538-221">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="7e538-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="7e538-222">Non supportato con l'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="7e538-222">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="7e538-223">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="7e538-223">Default: `10`</span></span><br><span data-ttu-id="7e538-224">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="7e538-224">Min: `0`</span></span><br><span data-ttu-id="7e538-225">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="7e538-225">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="7e538-226">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-226">Optional timespan attribute.</span></span></p><p><span data-ttu-id="7e538-227">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="7e538-227">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="7e538-228">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="7e538-228">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="7e538-229">Non è applicabile all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="7e538-229">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="7e538-230">Per l'hosting in-process, il modulo resta in attesa che l'app elabori la richiesta.</span><span class="sxs-lookup"><span data-stu-id="7e538-230">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="7e538-231">I valori validi per i segmenti della stringa relativi a minuti e secondi sono compresi nell'intervallo tra 0 e 59.</span><span class="sxs-lookup"><span data-stu-id="7e538-231">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="7e538-232">Se si usa **60** come valore per i minuti o i secondi, viene generato un errore *500 - Errore interno del server*.</span><span class="sxs-lookup"><span data-stu-id="7e538-232">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="7e538-233">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="7e538-233">Default: `00:02:00`</span></span><br><span data-ttu-id="7e538-234">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="7e538-234">Min: `00:00:00`</span></span><br><span data-ttu-id="7e538-235">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="7e538-235">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="7e538-236">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-236">Optional integer attribute.</span></span></p><p><span data-ttu-id="7e538-237">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="7e538-237">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="7e538-238">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="7e538-238">Default: `10`</span></span><br><span data-ttu-id="7e538-239">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="7e538-239">Min: `0`</span></span><br><span data-ttu-id="7e538-240">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="7e538-240">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="7e538-241">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-241">Optional integer attribute.</span></span></p><p><span data-ttu-id="7e538-242">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="7e538-242">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="7e538-243">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="7e538-243">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="7e538-244">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="7e538-244">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="7e538-245">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="7e538-245">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="7e538-246">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="7e538-246">Default: `120`</span></span><br><span data-ttu-id="7e538-247">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="7e538-247">Min: `0`</span></span><br><span data-ttu-id="7e538-248">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="7e538-248">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="7e538-249">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-249">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="7e538-250">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="7e538-250">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="7e538-251">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-251">Optional string attribute.</span></span></p><p><span data-ttu-id="7e538-252">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="7e538-252">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="7e538-253">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="7e538-253">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="7e538-254">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="7e538-254">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="7e538-255">Le eventuali cartelle specificate nel percorso vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="7e538-255">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="7e538-256">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file ( *.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="7e538-256">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="7e538-257">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="7e538-257">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="7e538-258">Attributo</span><span class="sxs-lookup"><span data-stu-id="7e538-258">Attribute</span></span> | <span data-ttu-id="7e538-259">Description</span><span class="sxs-lookup"><span data-stu-id="7e538-259">Description</span></span> | <span data-ttu-id="7e538-260">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="7e538-260">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="7e538-261">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-261">Optional string attribute.</span></span></p><p><span data-ttu-id="7e538-262">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="7e538-262">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="7e538-263">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="7e538-264">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="7e538-264">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="7e538-265">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-265">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="7e538-266">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="7e538-266">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="7e538-267">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="7e538-267">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="7e538-268">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-268">Optional integer attribute.</span></span></p><p><span data-ttu-id="7e538-269">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="7e538-269">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="7e538-270">L'impostazione di `processesPerApplication` è sconsigliata.</span><span class="sxs-lookup"><span data-stu-id="7e538-270">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="7e538-271">Questo attributo sarà rimosso nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="7e538-271">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="7e538-272">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="7e538-272">Default: `1`</span></span><br><span data-ttu-id="7e538-273">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="7e538-273">Min: `1`</span></span><br><span data-ttu-id="7e538-274">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="7e538-274">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="7e538-275">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="7e538-275">Required string attribute.</span></span></p><p><span data-ttu-id="7e538-276">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e538-276">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="7e538-277">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="7e538-277">Relative paths are supported.</span></span> <span data-ttu-id="7e538-278">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="7e538-278">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="7e538-279">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-279">Optional integer attribute.</span></span></p><p><span data-ttu-id="7e538-280">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="7e538-280">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="7e538-281">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="7e538-281">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="7e538-282">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="7e538-282">Default: `10`</span></span><br><span data-ttu-id="7e538-283">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="7e538-283">Min: `0`</span></span><br><span data-ttu-id="7e538-284">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="7e538-284">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="7e538-285">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-285">Optional timespan attribute.</span></span></p><p><span data-ttu-id="7e538-286">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="7e538-286">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="7e538-287">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="7e538-287">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="7e538-288">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="7e538-288">Default: `00:02:00`</span></span><br><span data-ttu-id="7e538-289">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="7e538-289">Min: `00:00:00`</span></span><br><span data-ttu-id="7e538-290">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="7e538-290">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="7e538-291">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-291">Optional integer attribute.</span></span></p><p><span data-ttu-id="7e538-292">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="7e538-292">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="7e538-293">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="7e538-293">Default: `10`</span></span><br><span data-ttu-id="7e538-294">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="7e538-294">Min: `0`</span></span><br><span data-ttu-id="7e538-295">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="7e538-295">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="7e538-296">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-296">Optional integer attribute.</span></span></p><p><span data-ttu-id="7e538-297">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="7e538-297">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="7e538-298">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="7e538-298">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="7e538-299">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="7e538-299">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="7e538-300">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="7e538-300">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="7e538-301">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="7e538-301">Default: `120`</span></span><br><span data-ttu-id="7e538-302">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="7e538-302">Min: `0`</span></span><br><span data-ttu-id="7e538-303">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="7e538-303">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="7e538-304">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-304">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="7e538-305">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="7e538-305">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="7e538-306">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="7e538-306">Optional string attribute.</span></span></p><p><span data-ttu-id="7e538-307">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="7e538-307">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="7e538-308">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="7e538-308">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="7e538-309">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="7e538-309">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="7e538-310">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="7e538-310">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="7e538-311">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file ( *.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="7e538-311">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="7e538-312">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="7e538-312">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="7e538-313">Impostazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="7e538-313">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7e538-314">È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="7e538-314">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="7e538-315">Specificare una variabile di ambiente con l'elemento figlio `<environmentVariable>` di un elemento della raccolta `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="7e538-315">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="7e538-316">Le variabili di ambiente impostate in questa sezione hanno la precedenza sulle variabili di ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="7e538-316">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7e538-317">È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="7e538-317">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="7e538-318">Specificare una variabile di ambiente con l'elemento figlio `<environmentVariable>` di un elemento della raccolta `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="7e538-318">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="7e538-319">Le variabili di ambiente impostate in questa sezione sono in conflitto con le variabili di ambiente di sistema impostate con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="7e538-319">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="7e538-320">Se una variabile di ambiente è impostata sia nel file *web.config* sia a livello di sistema in Windows, il valore del file *web.config* viene aggiunto al valore della variabile di ambiente di sistema, ad esempio `ASPNETCORE_ENVIRONMENT: Development;Development`, e ciò impedisce l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-320">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="7e538-321">Nell'esempio seguente vengono impostate due variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="7e538-321">The following example sets two environment variables.</span></span> <span data-ttu-id="7e538-322">`ASPNETCORE_ENVIRONMENT` configura l'ambiente dell'app su `Development`.</span><span class="sxs-lookup"><span data-stu-id="7e538-322">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="7e538-323">Uno sviluppatore può impostare temporaneamente questo valore nel file *web.config* per forzare il caricamento della [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) durante il debug di un'eccezione dell'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-323">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="7e538-324">`CONFIG_DIR` è un esempio di variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento del file di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-324">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="7e538-325">Un'alternativa all'impostazione dell'ambiente direttamente in *web.config* è quella di includere la proprietà `<EnvironmentName>` nel profilo di pubblicazione ( *.pubxml*) o nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="7e538-325">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="7e538-326">Questo approccio imposta l'ambiente in *web.config* quando viene pubblicato il progetto:</span><span class="sxs-lookup"><span data-stu-id="7e538-326">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="7e538-327">Impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` su `Development` solo in server di gestione temporanea e test che non sono accessibili da reti non attendibili, ad esempio Internet.</span><span class="sxs-lookup"><span data-stu-id="7e538-327">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="7e538-328">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="7e538-328">app_offline.htm</span></span>

<span data-ttu-id="7e538-329">Se un file denominato *app_offline.htm* viene rilevato nella directory radice di un'app, il modulo ASP.NET Core tenta di arrestare normalmente l'app e arresta l'elaborazione delle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="7e538-329">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="7e538-330">Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo ASP.NET Core termina il processo in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7e538-330">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="7e538-331">Mentre il file *app_offline.htm* è presente, il modulo ASP.NET Core risponde alle richieste restituendo il contenuto del file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="7e538-331">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="7e538-332">Quando il file *app_offline.htm* viene rimosso, la richiesta successiva avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-332">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e538-333">Quando si usa il modello di hosting out-of-process, l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="7e538-333">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="7e538-334">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-334">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="7e538-335">Pagina di errore di avvio</span><span class="sxs-lookup"><span data-stu-id="7e538-335">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e538-336">Sia l'hosting In-Process che quello out-of-process producono pagine di errore personalizzate quando non riescono ad avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="7e538-336">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="7e538-337">Se il modulo ASP.NET Core non riesce a trovare il gestore richieste In-Process o out-of-process, viene visualizzata una tabella codici di stato *500.0 - Errore di caricamento del gestore In-Process/out-of-process*.</span><span class="sxs-lookup"><span data-stu-id="7e538-337">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="7e538-338">Per l'hosting In-Process, se il modulo ASP.NET Core non riesce ad avviare l'app, viene visualizzata una tabella codici di stato *500.30 - Errore di avvio*.</span><span class="sxs-lookup"><span data-stu-id="7e538-338">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="7e538-339">Per l'hosting out-of-process, se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="7e538-339">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="7e538-340">Per non visualizzare questa pagina e tornare alla tabella codici di stato 5xx predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="7e538-340">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="7e538-341">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="7e538-341">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7e538-342">Se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="7e538-342">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="7e538-343">Per non visualizzare questa pagina e tornare alla tabella codici di stato 502 predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="7e538-343">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="7e538-344">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="7e538-344">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Tabella codici di stato 502.5 Errore del processo](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="7e538-346">Creazione e reindirizzamento dei log</span><span class="sxs-lookup"><span data-stu-id="7e538-346">Log creation and redirection</span></span>

<span data-ttu-id="7e538-347">Il modulo ASP.NET Core reindirizza su disco l'output della console stdout e stderr se sono impostati gli attributi `stdoutLogEnabled` e `stdoutLogFile` dell'elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="7e538-347">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="7e538-348">Le eventuali le cartelle nel percorso `stdoutLogFile` vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="7e538-348">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="7e538-349">Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).</span><span class="sxs-lookup"><span data-stu-id="7e538-349">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="7e538-350">I log non vengono ruotati, a meno che non si verifichi il riciclo/riavvio del processo.</span><span class="sxs-lookup"><span data-stu-id="7e538-350">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="7e538-351">È responsabilità del provider di servizi di hosting limitare lo spazio su disco usato dai log.</span><span class="sxs-lookup"><span data-stu-id="7e538-351">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="7e538-352">L'uso del log stdout è consigliato solo per la risoluzione dei problemi di avvio delle app.</span><span class="sxs-lookup"><span data-stu-id="7e538-352">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="7e538-353">Non usare il log stdout per scopi di registrazione generale delle app.</span><span class="sxs-lookup"><span data-stu-id="7e538-353">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="7e538-354">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="7e538-354">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7e538-355">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="7e538-355">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="7e538-356">Un timestamp e l'estensione del file vengono aggiunti automaticamente al momento della creazione del file di log.</span><span class="sxs-lookup"><span data-stu-id="7e538-356">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="7e538-357">Il nome del file di log è composto aggiungendo il timestamp, l'ID processo e l'estensione del file ( *.log*) all'ultimo segmento del percorso `stdoutLogFile` (in genere *stdout*), con caratteri di sottolineatura come delimitatori.</span><span class="sxs-lookup"><span data-stu-id="7e538-357">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="7e538-358">Se il percorso `stdoutLogFile` termina con *stdout*, un log per un'app con un PID 1934 creata il 5/2/2018 alle 19:42:32 sarà denominato *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="7e538-358">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e538-359">Se `stdoutLogEnabled` è false, gli errori che si verificano all'avvio dell'app vengono acquisiti ed emessi nel log eventi fino a 30 KB.</span><span class="sxs-lookup"><span data-stu-id="7e538-359">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="7e538-360">Dopo l'avvio, tutti i log aggiuntivi vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="7e538-360">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="7e538-361">L'elemento di esempio `aspNetCore` seguente configura la registrazione stdout per un'app ospitata in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="7e538-361">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="7e538-362">Un percorso locale o un percorso di una condivisione di rete è accettabile per la registrazione locale.</span><span class="sxs-lookup"><span data-stu-id="7e538-362">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="7e538-363">Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="7e538-363">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="7e538-364">Log di diagnostica avanzati</span><span class="sxs-lookup"><span data-stu-id="7e538-364">Enhanced diagnostic logs</span></span>

<span data-ttu-id="7e538-365">Il modulo ASP.NET Core può essere configurato per restituire log di diagnostica avanzata.</span><span class="sxs-lookup"><span data-stu-id="7e538-365">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="7e538-366">Aggiungere l'elemento `<handlerSettings>` all'elemento `<aspNetCore>` in *web.config*. L'impostazione di `debugLevel` su `TRACE` restituisce una maggior fedeltà dei dati diagnostici:</span><span class="sxs-lookup"><span data-stu-id="7e538-366">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="7e538-367">I valori di livello debug (`debugLevel`) possono includere sia il livello che il percorso.</span><span class="sxs-lookup"><span data-stu-id="7e538-367">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="7e538-368">Livelli (in ordine dal meno dettagliato al più dettagliato):</span><span class="sxs-lookup"><span data-stu-id="7e538-368">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="7e538-369">ERROR</span><span class="sxs-lookup"><span data-stu-id="7e538-369">ERROR</span></span>
* <span data-ttu-id="7e538-370">WARNING</span><span class="sxs-lookup"><span data-stu-id="7e538-370">WARNING</span></span>
* <span data-ttu-id="7e538-371">INFO</span><span class="sxs-lookup"><span data-stu-id="7e538-371">INFO</span></span>
* <span data-ttu-id="7e538-372">TRACE</span><span class="sxs-lookup"><span data-stu-id="7e538-372">TRACE</span></span>

<span data-ttu-id="7e538-373">Posizioni (sono consentite più posizioni):</span><span class="sxs-lookup"><span data-stu-id="7e538-373">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="7e538-374">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="7e538-374">CONSOLE</span></span>
* <span data-ttu-id="7e538-375">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="7e538-375">EVENTLOG</span></span>
* <span data-ttu-id="7e538-376">FILE</span><span class="sxs-lookup"><span data-stu-id="7e538-376">FILE</span></span>

<span data-ttu-id="7e538-377">Le impostazioni del gestore possono essere specificate anche tramite le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="7e538-377">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="7e538-378">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Percorso del file di logo di debug.</span><span class="sxs-lookup"><span data-stu-id="7e538-378">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="7e538-379">(Impostazione predefinita: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="7e538-379">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="7e538-380">`ASPNETCORE_MODULE_DEBUG` &ndash; Impostazione del livello di debug.</span><span class="sxs-lookup"><span data-stu-id="7e538-380">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="7e538-381">**Non** lasciare la registrazione del debug abilitata nella distribuzione per un tempo superiore a quello necessario alla risoluzione di problema.</span><span class="sxs-lookup"><span data-stu-id="7e538-381">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="7e538-382">Le dimensioni del log non sono limitate.</span><span class="sxs-lookup"><span data-stu-id="7e538-382">The size of the log isn't limited.</span></span> <span data-ttu-id="7e538-383">Se si lascia abilitato il log di debug, lo spazio disponibile su disco può esaurirsi e il server o il servizio app può registrare un arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="7e538-383">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="7e538-384">Vedere [Configurazione con web.config](#configuration-with-webconfig) per un esempio dell'elemento `aspNetCore` nel file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="7e538-384">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="modify-the-stack-size"></a><span data-ttu-id="7e538-385">Modificare le dimensioni dello stack</span><span class="sxs-lookup"><span data-stu-id="7e538-385">Modify the stack size</span></span>

<span data-ttu-id="7e538-386">Configurare le dimensioni dello stack gestito tramite l'impostazione `stackSize` in byte.</span><span class="sxs-lookup"><span data-stu-id="7e538-386">Configure the managed stack size using the `stackSize` setting in bytes.</span></span> <span data-ttu-id="7e538-387">Le dimensioni predefinite sono pari a `1048576` byte (1 MB).</span><span class="sxs-lookup"><span data-stu-id="7e538-387">The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="7e538-388">La configurazione del proxy usa il protocollo HTTP e un token di associazione</span><span class="sxs-lookup"><span data-stu-id="7e538-388">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e538-389">*Si applica solo all'hosting out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="7e538-389">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="7e538-390">Il proxy creato tra il modulo ASP.NET Core e Kestrel usa il protocollo HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e538-390">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="7e538-391">L'uso di HTTP è un'ottimizzazione delle prestazioni in cui il traffico tra il modulo e Kestrel avviene in un indirizzo di loopback dell'interfaccia di rete.</span><span class="sxs-lookup"><span data-stu-id="7e538-391">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="7e538-392">Non vi è alcun rischio di intercettazione del traffico tra il modulo e Kestrel da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="7e538-392">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="7e538-393">Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine.</span><span class="sxs-lookup"><span data-stu-id="7e538-393">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="7e538-394">Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo.</span><span class="sxs-lookup"><span data-stu-id="7e538-394">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="7e538-395">Il token di associazione viene impostato anche in un'intestazione (`MS-ASPNETCORE-TOKEN`) in tutte le richieste trasmesse tramite proxy.</span><span class="sxs-lookup"><span data-stu-id="7e538-395">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="7e538-396">Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="7e538-396">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="7e538-397">Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata.</span><span class="sxs-lookup"><span data-stu-id="7e538-397">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="7e538-398">La variabile di ambiente del token di associazione e il traffico tra il modulo e Kestrel non sono accessibili da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="7e538-398">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="7e538-399">Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.</span><span class="sxs-lookup"><span data-stu-id="7e538-399">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="7e538-400">Modulo ASP.NET Core con configurazione condivisa di IIS</span><span class="sxs-lookup"><span data-stu-id="7e538-400">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="7e538-401">Il programma di installazione del modulo ASP.NET Core viene eseguito con i privilegi dell'account **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="7e538-401">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="7e538-402">Poiché l'account di sistema locale non dispone dell'autorizzazione di modifica per il percorso della condivisione usato dalla configurazione condivisa di IIS, il programma di installazione genera un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo nel file *applicationHost.config* nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="7e538-402">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e538-403">Quando si usa una configurazione condivisa di IIS nello stesso computer dell'installazione di IIS, eseguire il programma di installazione del bundle di hosting ASP.NET Core con il parametro `OPT_NO_SHARED_CONFIG_CHECK` impostato su `1`:</span><span class="sxs-lookup"><span data-stu-id="7e538-403">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="7e538-404">Quando il percorso della configurazione condivisa non è nello stesso computer dell'installazione di IIS, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="7e538-404">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="7e538-405">Disabilitare la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="7e538-405">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="7e538-406">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="7e538-406">Run the installer.</span></span>
1. <span data-ttu-id="7e538-407">Esportare il file *applicationHost.config* aggiornato nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="7e538-407">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="7e538-408">Abilitare di nuovo la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="7e538-408">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7e538-409">Quando si usa una configurazione condivisa di IIS, attenersi ai passaggi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="7e538-409">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="7e538-410">Disabilitare la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="7e538-410">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="7e538-411">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="7e538-411">Run the installer.</span></span>
1. <span data-ttu-id="7e538-412">Esportare il file *applicationHost.config* aggiornato nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="7e538-412">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="7e538-413">Abilitare di nuovo la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="7e538-413">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="7e538-414">Versione del modulo e log del programma di installazione del bundle di hosting</span><span class="sxs-lookup"><span data-stu-id="7e538-414">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="7e538-415">Per determinare la versione del modulo ASP.NET Core installato:</span><span class="sxs-lookup"><span data-stu-id="7e538-415">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="7e538-416">Nel sistema host passare a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="7e538-416">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="7e538-417">Individuare il file *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="7e538-417">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="7e538-418">Fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="7e538-418">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="7e538-419">Selezionare la scheda **Dettagli**. **Versione file** e **Versione prodotto** rappresentano la versione installata del modulo.</span><span class="sxs-lookup"><span data-stu-id="7e538-419">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="7e538-420">I log del programma di installazione del bundle di hosting per il modulo sono disponibili in *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Il file è denominato *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="7e538-420">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="7e538-421">Percorsi dei file di modulo, schema e configurazione</span><span class="sxs-lookup"><span data-stu-id="7e538-421">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="7e538-422">Modulo</span><span class="sxs-lookup"><span data-stu-id="7e538-422">Module</span></span>

<span data-ttu-id="7e538-423">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="7e538-423">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="7e538-424">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="7e538-424">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="7e538-425">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="7e538-425">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="7e538-426">%Programmi%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="7e538-426">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="7e538-427">%Programmi(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="7e538-427">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="7e538-428">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="7e538-428">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="7e538-429">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="7e538-429">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="7e538-430">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="7e538-430">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="7e538-431">%Programmi%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="7e538-431">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="7e538-432">%Programmi(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="7e538-432">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="7e538-433">Schema</span><span class="sxs-lookup"><span data-stu-id="7e538-433">Schema</span></span>

<span data-ttu-id="7e538-434">**IIS**</span><span class="sxs-lookup"><span data-stu-id="7e538-434">**IIS**</span></span>

* <span data-ttu-id="7e538-435">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="7e538-435">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="7e538-436">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="7e538-436">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

<span data-ttu-id="7e538-437">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="7e538-437">**IIS Express**</span></span>

* <span data-ttu-id="7e538-438">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="7e538-438">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="7e538-439">%Programmi%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="7e538-439">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="7e538-440">Configurazione</span><span class="sxs-lookup"><span data-stu-id="7e538-440">Configuration</span></span>

<span data-ttu-id="7e538-441">**IIS**</span><span class="sxs-lookup"><span data-stu-id="7e538-441">**IIS**</span></span>

* <span data-ttu-id="7e538-442">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="7e538-442">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="7e538-443">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="7e538-443">**IIS Express**</span></span>

* <span data-ttu-id="7e538-444">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="7e538-444">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="7e538-445">Interfaccia della riga di comando di *iisexpress.exe*: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="7e538-445">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="7e538-446">È possibile trovare i file eseguendo una ricerca di *aspnetcore* nel file *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="7e538-446">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e538-447">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7e538-447">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="7e538-448">Repository GitHub del modulo ASP.NET Core (origine riferimento)</span><span class="sxs-lookup"><span data-stu-id="7e538-448">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
