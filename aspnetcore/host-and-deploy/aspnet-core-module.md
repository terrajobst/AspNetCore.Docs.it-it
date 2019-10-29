---
title: Modulo ASP.NET Core
author: guardrex
description: Informazioni su come configurare il modulo di ASP.NET Core per l'hosting di app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 42ff4438738931fde70e123031412bcfc8a83efb
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034218"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="6d3c5-103">Modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d3c5-103">ASP.NET Core Module</span></span>

<span data-ttu-id="6d3c5-104">Di [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6d3c5-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6d3c5-105">Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="6d3c5-106">Ospitare un app ASP.NET Core all'interno del processo di lavoro IIS (`w3wp.exe`), denominato [modello di hosting in-process](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="6d3c5-107">Inoltrare le richieste Web all'app back-end ASP.NET Core che esegue il [server Kestrel](xref:fundamentals/servers/kestrel), denominato [modello di hosting out-of-process](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="6d3c5-108">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-108">Supported Windows versions:</span></span>

* <span data-ttu-id="6d3c5-109">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="6d3c5-109">Windows 7 or later</span></span>
* <span data-ttu-id="6d3c5-110">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="6d3c5-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6d3c5-111">In caso di hosting in-process, il modulo usa un'implementazione di server in-process per IIS detta server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6d3c5-112">In caso di hosting out-of-process, il modulo funziona solo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="6d3c5-113">Il modulo non funziona con [http. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="6d3c5-114">Modelli di hosting</span><span class="sxs-lookup"><span data-stu-id="6d3c5-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="6d3c5-115">Modello di hosting in-process</span><span class="sxs-lookup"><span data-stu-id="6d3c5-115">In-process hosting model</span></span>

<span data-ttu-id="6d3c5-116">ASP.NET Core app predefinite per il modello di hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="6d3c5-117">In caso di hosting in-process, vengono applicate le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="6d3c5-118">Viene usato un server HTTP di IIS (`IISHttpServer`) al posto del server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="6d3c5-119">Per in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> per:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="6d3c5-120">Registrare `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="6d3c5-121">Configurare la porta e il percorso di base su cui il server deve eseguire l'ascolto in caso di esecuzione dietro il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="6d3c5-122">Configurare l'host per l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="6d3c5-123">L'[attributo requestTimeout ](#attributes-of-the-aspnetcore-element) non viene applicato all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="6d3c5-124">Non è supportata la condivisione di un pool di app tra le app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="6d3c5-125">Usare un pool di app per ogni app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-125">Use one app pool per app.</span></span>

* <span data-ttu-id="6d3c5-126">Quando si usa la [Distribuzione Web ](/iis/publish/using-web-deploy/introduction-to-web-deploy) o si inserisce manualmente un [file app_offline.htm nella distribuzione](xref:host-and-deploy/iis/index#locked-deployment-files), l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6d3c5-127">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="6d3c5-128">L'architettura, vale a dire il numero di bit dell'app, e il runtime installato (x64 o x86) devono corrispondere all'architettura del pool di app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="6d3c5-129">Le disconnessioni del client vengono rilevate.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-129">Client disconnects are detected.</span></span> <span data-ttu-id="6d3c5-130">Il token di annullamento [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) viene cancellato quando il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="6d3c5-131">In ASP.NET Core 2.2.1 o versioni precedenti, <xref:System.IO.Directory.GetCurrentDirectory*> restituisce la directory di lavoro del processo avviato da IIS invece che la directory dell'app (ad esempio, *C:\Windows\System32\inetsrv* per *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="6d3c5-132">Per vedere il codice di esempio che imposta la directory corrente dell'app, vedere la [classe CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="6d3c5-133">Chiamare il metodo `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="6d3c5-134">Le chiamate successive a <xref:System.IO.Directory.GetCurrentDirectory*> specificano la directory dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="6d3c5-135">In caso di hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> non viene chiamato internamente per inizializzare un utente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="6d3c5-136">Pertanto, un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> usate per trasformare le attestazioni dopo ogni autenticazione non viene attivata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="6d3c5-137">Quando si trasformano le attestazioni con un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, chiamare <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> per aggiungere i servizi di autenticazione:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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
  
  * <span data-ttu-id="6d3c5-138">Le [distribuzioni di pacchetti Web (file singolo)](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) non sono supportate.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-138">[Web Package (single-file) deployments](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) aren't supported.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="6d3c5-139">Modello di hosting out-of-process</span><span class="sxs-lookup"><span data-stu-id="6d3c5-139">Out-of-process hosting model</span></span>

<span data-ttu-id="6d3c5-140">Per configurare un'app per l'hosting out-of-process, impostare il valore della proprietà `<AspNetCoreHostingModel>` su `OutOfProcess` nel file di progetto (con*estensione csproj*):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-140">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6d3c5-141">L'hosting in-process viene impostato con `InProcess`, che corrisponde al valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-141">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="6d3c5-142">Il valore di `<AspNetCoreHostingModel>` non fa distinzione tra maiuscole e minuscole, pertanto `inprocess` e `outofprocess` sono valori validi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-142">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="6d3c5-143">Viene usato il server [Kestrel](xref:fundamentals/servers/kestrel) al posto di un server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-143">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6d3c5-144">Per out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> per:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-144">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="6d3c5-145">Configurare la porta e il percorso di base su cui il server deve eseguire l'ascolto in caso di esecuzione dietro il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-145">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="6d3c5-146">Configurare l'host per l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-146">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="6d3c5-147">Modifiche del modello di hosting</span><span class="sxs-lookup"><span data-stu-id="6d3c5-147">Hosting model changes</span></span>

<span data-ttu-id="6d3c5-148">Se l'impostazione `hostingModel` viene modificata nel file *web.config* (descritto nella sezione [Configurazione con web.config](#configuration-with-webconfig)), il modulo ricicla il processo di lavoro per IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-148">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="6d3c5-149">Per IIS Express, il modulo non ricicla il processo di lavoro. Attiva invece un arresto normale del processo di IIS Express corrente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-149">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="6d3c5-150">La richiesta successiva all'app attiva un nuovo processo di IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-150">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="6d3c5-151">Nome processo</span><span class="sxs-lookup"><span data-stu-id="6d3c5-151">Process name</span></span>

<span data-ttu-id="6d3c5-152">`Process.GetCurrentProcess().ProcessName` dichiara `w3wp`/`iisexpress` (In-Process) o `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-152">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="6d3c5-153">Molti moduli nativi, ad esempio l'autenticazione di Windows, rimangono attivi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-153">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="6d3c5-154">Per altre informazioni sui moduli IIS attivi con il modulo ASP.NET Core, vedere <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-154">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="6d3c5-155">Il modulo ASP.NET Core può anche:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-155">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="6d3c5-156">Impostare variabili di ambiente per il processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-156">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="6d3c5-157">Registrare output stdout in una risorsa di archiviazione di file per la risoluzione di problemi di avvio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-157">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="6d3c5-158">Inoltrare token di autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-158">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="6d3c5-159">Come installare e usare il modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d3c5-159">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="6d3c5-160">Per istruzioni su come installare e usare il modulo ASP.NET Core, vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-160">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="6d3c5-161">Configurazione con web.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-161">Configuration with web.config</span></span>

<span data-ttu-id="6d3c5-162">Il modulo ASP.NET Core viene configurato tramite la sezione `aspNetCore` del nodo `system.webServer` nel file *web.config* del sito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-162">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="6d3c5-163">Il file *web.config* seguente viene pubblicato per una [distribuzione dipendente dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo ASP.NET Core per gestire le richieste del sito:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-163">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="6d3c5-164">Il file *web.config* seguente viene pubblicato per una [distribuzione autonoma](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-164">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="6d3c5-165">La proprietà <xref:System.Configuration.SectionInformation.InheritInChildApplications*> è impostata su `false` per indicare che le impostazioni specificate nell'elemento [\<percorso>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) non sono ereditate da app che risiedono in una sottodirectory dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-165">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="6d3c5-166">Quando un'app viene distribuita in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), il percorso `stdoutLogFile` è impostato su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-166">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6d3c5-167">Il percorso salva i log stdout nella cartella *LogFiles*, ovvero una posizione creata automaticamente dal servizio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-167">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="6d3c5-168">Per informazioni sulla configurazione delle applicazioni secondarie IIS, vedere <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-168">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="6d3c5-169">Attributi dell'elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="6d3c5-169">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="6d3c5-170">Attributo</span><span class="sxs-lookup"><span data-stu-id="6d3c5-170">Attribute</span></span> | <span data-ttu-id="6d3c5-171">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-171">Description</span></span> | <span data-ttu-id="6d3c5-172">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6d3c5-172">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6d3c5-173">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-173">Optional string attribute.</span></span></p><p><span data-ttu-id="6d3c5-174">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-174">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6d3c5-175">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-175">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6d3c5-176">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-176">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6d3c5-177">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-177">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6d3c5-178">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-178">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6d3c5-179">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-179">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="6d3c5-180">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-180">Optional string attribute.</span></span></p><p><span data-ttu-id="6d3c5-181">Specifica il modello di hosting come in-process (`InProcess`/`inprocess`) o out-of-process (`OutOfProcess`/`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-181">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `InProcess`<br>`inprocess` |
| `processesPerApplication` | <p><span data-ttu-id="6d3c5-182">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-183">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-183">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6d3c5-184">&dagger;Per l'hosting in-process, il valore è limitato a `1`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-184">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="6d3c5-185">L'impostazione di `processesPerApplication` è sconsigliata.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-185">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="6d3c5-186">Questo attributo sarà rimosso nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-186">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="6d3c5-187">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-187">Default: `1`</span></span><br><span data-ttu-id="6d3c5-188">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-188">Min: `1`</span></span><br><span data-ttu-id="6d3c5-189">Max: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="6d3c5-189">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="6d3c5-190">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-190">Required string attribute.</span></span></p><p><span data-ttu-id="6d3c5-191">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-191">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6d3c5-192">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-192">Relative paths are supported.</span></span> <span data-ttu-id="6d3c5-193">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-193">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6d3c5-194">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-194">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-195">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-195">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6d3c5-196">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-196">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="6d3c5-197">Non supportato con l'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-197">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="6d3c5-198">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-198">Default: `10`</span></span><br><span data-ttu-id="6d3c5-199">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-199">Min: `0`</span></span><br><span data-ttu-id="6d3c5-200">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-200">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6d3c5-201">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-201">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6d3c5-202">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-202">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6d3c5-203">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-203">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="6d3c5-204">Non è applicabile all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-204">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="6d3c5-205">Per l'hosting in-process, il modulo resta in attesa che l'app elabori la richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-205">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="6d3c5-206">I valori validi per i segmenti della stringa relativi a minuti e secondi sono compresi nell'intervallo tra 0 e 59.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-206">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="6d3c5-207">Se si usa **60** come valore per i minuti o i secondi, viene generato un errore *500 - Errore interno del server*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-207">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="6d3c5-208">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-208">Default: `00:02:00`</span></span><br><span data-ttu-id="6d3c5-209">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-209">Min: `00:00:00`</span></span><br><span data-ttu-id="6d3c5-210">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-210">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6d3c5-211">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-211">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-212">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-212">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6d3c5-213">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-213">Default: `10`</span></span><br><span data-ttu-id="6d3c5-214">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-214">Min: `0`</span></span><br><span data-ttu-id="6d3c5-215">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-215">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6d3c5-216">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-216">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-217">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-217">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6d3c5-218">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-218">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6d3c5-219">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-219">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6d3c5-220">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-220">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6d3c5-221">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-221">Default: `120`</span></span><br><span data-ttu-id="6d3c5-222">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-222">Min: `0`</span></span><br><span data-ttu-id="6d3c5-223">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-223">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6d3c5-224">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-224">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6d3c5-225">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-225">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6d3c5-226">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-226">Optional string attribute.</span></span></p><p><span data-ttu-id="6d3c5-227">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-227">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6d3c5-228">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-228">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6d3c5-229">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-229">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6d3c5-230">Le eventuali cartelle specificate nel percorso vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-230">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="6d3c5-231">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file ( *.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-231">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6d3c5-232">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-232">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="6d3c5-233">Impostare le variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="6d3c5-233">Set environment variables</span></span>

<span data-ttu-id="6d3c5-234">È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-234">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6d3c5-235">Specificare una variabile di ambiente con l'elemento figlio `<environmentVariable>` di un elemento della raccolta `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-235">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="6d3c5-236">Le variabili di ambiente impostate in questa sezione hanno la precedenza sulle variabili di ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-236">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="6d3c5-237">Nell'esempio seguente vengono impostate due variabili di ambiente in *Web. config*. `ASPNETCORE_ENVIRONMENT` configura l'ambiente dell'app in modo da `Development`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-237">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="6d3c5-238">Uno sviluppatore può impostare temporaneamente questo valore nel file *web.config* per forzare il caricamento della [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) durante il debug di un'eccezione dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-238">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="6d3c5-239">`CONFIG_DIR` è un esempio di variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento del file di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-239">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="6d3c5-240">Un'alternativa all'impostazione dell'ambiente direttamente in *web.config* è quella di includere la proprietà `<EnvironmentName>` nel profilo di pubblicazione ( *.pubxml*) o nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-240">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="6d3c5-241">Questo approccio imposta l'ambiente in *web.config* quando viene pubblicato il progetto:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-241">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="6d3c5-242">Impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` su `Development` solo in server di gestione temporanea e test che non sono accessibili da reti non attendibili, ad esempio Internet.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-242">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="6d3c5-243">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="6d3c5-243">app_offline.htm</span></span>

<span data-ttu-id="6d3c5-244">Se un file denominato *app_offline.htm* viene rilevato nella directory radice di un'app, il modulo ASP.NET Core tenta di arrestare normalmente l'app e arresta l'elaborazione delle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-244">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="6d3c5-245">Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo ASP.NET Core termina il processo in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-245">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="6d3c5-246">Mentre il file *app_offline.htm* è presente, il modulo ASP.NET Core risponde alle richieste restituendo il contenuto del file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-246">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="6d3c5-247">Quando il file *app_offline.htm* viene rimosso, la richiesta successiva avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-247">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="6d3c5-248">Quando si usa il modello di hosting out-of-process, l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-248">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6d3c5-249">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-249">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="6d3c5-250">Pagina di errore di avvio</span><span class="sxs-lookup"><span data-stu-id="6d3c5-250">Start-up error page</span></span>

<span data-ttu-id="6d3c5-251">Sia l'hosting In-Process che quello out-of-process producono pagine di errore personalizzate quando non riescono ad avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-251">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="6d3c5-252">Se il modulo ASP.NET Core non riesce a trovare il gestore richieste In-Process o out-of-process, viene visualizzata una tabella codici di stato *500.0 - Errore di caricamento del gestore In-Process/out-of-process*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-252">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="6d3c5-253">Per l'hosting In-Process, se il modulo ASP.NET Core non riesce ad avviare l'app, viene visualizzata una tabella codici di stato *500.30 - Errore di avvio*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-253">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="6d3c5-254">Per l'hosting out-of-process, se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-254">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="6d3c5-255">Per non visualizzare questa pagina e tornare alla tabella codici di stato 5xx predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-255">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6d3c5-256">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-256">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="6d3c5-257">Creazione e reindirizzamento dei log</span><span class="sxs-lookup"><span data-stu-id="6d3c5-257">Log creation and redirection</span></span>

<span data-ttu-id="6d3c5-258">Il modulo ASP.NET Core reindirizza su disco l'output della console stdout e stderr se sono impostati gli attributi `stdoutLogEnabled` e `stdoutLogFile` dell'elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-258">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="6d3c5-259">Le eventuali le cartelle nel percorso `stdoutLogFile` vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-259">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="6d3c5-260">Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-260">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6d3c5-261">I log non vengono ruotati, a meno che non si verifichi il riciclo/riavvio del processo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-261">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="6d3c5-262">È responsabilità del provider di servizi di hosting limitare lo spazio su disco usato dai log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-262">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="6d3c5-263">L'uso del log stdout è consigliato solo per la risoluzione dei problemi di avvio dell'app durante l'hosting in IIS o quando si usa il [supporto in fase di sviluppo per IIS con Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), non durante il debug in locale e l'esecuzione dell'app con IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-263">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="6d3c5-264">Non usare il log stdout per scopi di registrazione generale delle app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-264">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="6d3c5-265">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-265">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="6d3c5-266">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-266">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="6d3c5-267">Un timestamp e l'estensione del file vengono aggiunti automaticamente al momento della creazione del file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-267">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="6d3c5-268">Il nome del file di log è composto aggiungendo il timestamp, l'ID processo e l'estensione del file ( *.log*) all'ultimo segmento del percorso `stdoutLogFile` (in genere *stdout*), con caratteri di sottolineatura come delimitatori.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-268">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="6d3c5-269">Se il percorso `stdoutLogFile` termina con *stdout*, un log per un'app con un PID 1934 creata il 5/2/2018 alle 19:42:32 sarà denominato *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-269">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="6d3c5-270">Se `stdoutLogEnabled` è false, gli errori che si verificano all'avvio dell'app vengono acquisiti ed emessi nel log eventi fino a 30 KB.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-270">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="6d3c5-271">Dopo l'avvio, tutti i log aggiuntivi vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-271">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="6d3c5-272">Nell'elemento `aspNetCore` di esempio seguente viene configurata la registrazione di stdout nel percorso relativo `.\log\`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-272">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="6d3c5-273">Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-273">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="6d3c5-274">Quando si pubblica un'app per la distribuzione di app Azure Service, l'SDK Web imposta il valore di `stdoutLogFile` su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-274">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6d3c5-275">La variabile di ambiente `%home` è predefinita per le app ospitate dal servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-275">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="6d3c5-276">Per creare regole di filtro di registrazione, vedere le sezioni [configurazione](xref:fundamentals/logging/index#log-filtering) e [filtro dei log](xref:fundamentals/logging/index#log-filtering) della documentazione relativa alla registrazione del ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-276">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="6d3c5-277">Per ulteriori informazioni sui formati di percorso, vedere [formati di percorso dei file nei sistemi Windows](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-277">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="6d3c5-278">Log di diagnostica avanzati</span><span class="sxs-lookup"><span data-stu-id="6d3c5-278">Enhanced diagnostic logs</span></span>

<span data-ttu-id="6d3c5-279">Il modulo ASP.NET Core può essere configurato per restituire log di diagnostica avanzata.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-279">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="6d3c5-280">Aggiungere l'elemento `<handlerSettings>` all'elemento `<aspNetCore>` in *Web. config*. L'impostazione del `debugLevel` su `TRACE` espone una maggiore fedeltà delle informazioni di diagnostica:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-280">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="6d3c5-281">Le eventuali cartelle nel percorso (*logs* nell'esempio precedente) vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-281">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="6d3c5-282">Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-282">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6d3c5-283">I valori di livello debug (`debugLevel`) possono includere sia il livello che il percorso.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-283">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="6d3c5-284">Livelli (in ordine dal meno dettagliato al più dettagliato):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-284">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="6d3c5-285">ERROR</span><span class="sxs-lookup"><span data-stu-id="6d3c5-285">ERROR</span></span>
* <span data-ttu-id="6d3c5-286">WARNING</span><span class="sxs-lookup"><span data-stu-id="6d3c5-286">WARNING</span></span>
* <span data-ttu-id="6d3c5-287">INFO</span><span class="sxs-lookup"><span data-stu-id="6d3c5-287">INFO</span></span>
* <span data-ttu-id="6d3c5-288">TRACE</span><span class="sxs-lookup"><span data-stu-id="6d3c5-288">TRACE</span></span>

<span data-ttu-id="6d3c5-289">Posizioni (sono consentite più posizioni):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-289">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="6d3c5-290">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="6d3c5-290">CONSOLE</span></span>
* <span data-ttu-id="6d3c5-291">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="6d3c5-291">EVENTLOG</span></span>
* <span data-ttu-id="6d3c5-292">FILE</span><span class="sxs-lookup"><span data-stu-id="6d3c5-292">FILE</span></span>

<span data-ttu-id="6d3c5-293">Le impostazioni del gestore possono essere specificate anche tramite le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-293">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="6d3c5-294">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Percorso del file di logo di debug.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-294">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="6d3c5-295">(Impostazione predefinita: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="6d3c5-295">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="6d3c5-296">`ASPNETCORE_MODULE_DEBUG` &ndash; Impostazione del livello di debug.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-296">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="6d3c5-297">**Non** lasciare la registrazione del debug abilitata nella distribuzione per un tempo superiore a quello necessario alla risoluzione di problema.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-297">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="6d3c5-298">Le dimensioni del log non sono limitate.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-298">The size of the log isn't limited.</span></span> <span data-ttu-id="6d3c5-299">Se si lascia abilitato il log di debug, lo spazio disponibile su disco può esaurirsi e il server o il servizio app può registrare un arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-299">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="6d3c5-300">Vedere [Configurazione con web.config](#configuration-with-webconfig) per un esempio dell'elemento `aspNetCore` nel file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-300">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="6d3c5-301">Modificare le dimensioni dello stack</span><span class="sxs-lookup"><span data-stu-id="6d3c5-301">Modify the stack size</span></span>

<span data-ttu-id="6d3c5-302">*Si applica solo quando si usa il modello di hosting in-process.*</span><span class="sxs-lookup"><span data-stu-id="6d3c5-302">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="6d3c5-303">Configurare la dimensione dello stack gestito usando l'impostazione `stackSize` in byte in *Web. config*. Le dimensioni predefinite sono `1048576` byte (1 MB).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-303">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="6d3c5-304">La configurazione del proxy usa il protocollo HTTP e un token di associazione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-304">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="6d3c5-305">*Si applica solo all'hosting out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="6d3c5-305">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="6d3c5-306">Il proxy creato tra il modulo ASP.NET Core e Kestrel usa il protocollo HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-306">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="6d3c5-307">Non vi è alcun rischio di intercettazione del traffico tra il modulo e Kestrel da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-307">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="6d3c5-308">Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-308">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="6d3c5-309">Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-309">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="6d3c5-310">Il token di associazione viene impostato anche in un'intestazione (`MS-ASPNETCORE-TOKEN`) in tutte le richieste trasmesse tramite proxy.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-310">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="6d3c5-311">Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-311">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="6d3c5-312">Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-312">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="6d3c5-313">La variabile di ambiente del token di associazione e il traffico tra il modulo e Kestrel non sono accessibili da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-313">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="6d3c5-314">Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-314">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="6d3c5-315">Modulo ASP.NET Core con configurazione condivisa di IIS</span><span class="sxs-lookup"><span data-stu-id="6d3c5-315">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="6d3c5-316">Il programma di installazione del modulo ASP.NET Core viene eseguito con i privilegi dell'account **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-316">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="6d3c5-317">Poiché l'account di sistema locale non dispone dell'autorizzazione di modifica per il percorso della condivisione usato dalla configurazione condivisa di IIS, il programma di installazione genera un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo nel file *applicationHost.config* nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-317">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="6d3c5-318">Quando si usa una configurazione condivisa di IIS nello stesso computer dell'installazione di IIS, eseguire il programma di installazione del bundle di hosting ASP.NET Core con il parametro `OPT_NO_SHARED_CONFIG_CHECK` impostato su `1`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-318">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="6d3c5-319">Quando il percorso della configurazione condivisa non è nello stesso computer dell'installazione di IIS, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-319">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="6d3c5-320">Disabilitare la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-320">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6d3c5-321">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-321">Run the installer.</span></span>
1. <span data-ttu-id="6d3c5-322">Esportare il file *applicationHost.config* aggiornato nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-322">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6d3c5-323">Abilitare di nuovo la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-323">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="6d3c5-324">Versione del modulo e log del programma di installazione del bundle di hosting</span><span class="sxs-lookup"><span data-stu-id="6d3c5-324">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="6d3c5-325">Per determinare la versione del modulo ASP.NET Core installato:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-325">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="6d3c5-326">Nel sistema host passare a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-326">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="6d3c5-327">Individuare il file *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-327">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="6d3c5-328">Fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-328">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="6d3c5-329">Selezionare la scheda **Dettagli** . La versione del **file** e la **versione del prodotto** rappresentano la versione installata del modulo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-329">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="6d3c5-330">I log del programma di installazione del bundle di hosting per il modulo sono disponibili in *C:\\utenti\\% username%\\AppData\\\\locale*. Il file è denominato *dd_DotNetCoreWinSvrHosting__\<timestamp > _000_AspNetCoreModule_x64. log*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-330">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="6d3c5-331">Percorsi dei file di modulo, schema e configurazione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-331">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="6d3c5-332">Modulo</span><span class="sxs-lookup"><span data-stu-id="6d3c5-332">Module</span></span>

<span data-ttu-id="6d3c5-333">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-333">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="6d3c5-334">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-334">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-335">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-335">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-336">%Programmi%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-336">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6d3c5-337">%Programmi(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-337">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="6d3c5-338">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-338">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="6d3c5-339">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-339">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-340">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-340">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-341">%Programmi%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-341">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6d3c5-342">%Programmi(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-342">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="6d3c5-343">Schema</span><span class="sxs-lookup"><span data-stu-id="6d3c5-343">Schema</span></span>

<span data-ttu-id="6d3c5-344">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-344">**IIS**</span></span>

* <span data-ttu-id="6d3c5-345">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-345">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="6d3c5-346">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-346">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="6d3c5-347">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-347">**IIS Express**</span></span>

* <span data-ttu-id="6d3c5-348">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-348">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="6d3c5-349">%Programmi%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-349">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="6d3c5-350">Configurazione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-350">Configuration</span></span>

<span data-ttu-id="6d3c5-351">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-351">**IIS**</span></span>

* <span data-ttu-id="6d3c5-352">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-352">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="6d3c5-353">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-353">**IIS Express**</span></span>

* <span data-ttu-id="6d3c5-354">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-354">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="6d3c5-355">Interfaccia della riga di comando di *iisexpress.exe*: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-355">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="6d3c5-356">È possibile trovare i file eseguendo una ricerca di *aspnetcore* nel file *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-356">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="6d3c5-357">Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-357">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="6d3c5-358">Ospitare un app ASP.NET Core all'interno del processo di lavoro IIS (`w3wp.exe`), denominato [modello di hosting in-process](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-358">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="6d3c5-359">Inoltrare le richieste Web all'app back-end ASP.NET Core che esegue il [server Kestrel](xref:fundamentals/servers/kestrel), denominato [modello di hosting out-of-process](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-359">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="6d3c5-360">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-360">Supported Windows versions:</span></span>

* <span data-ttu-id="6d3c5-361">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="6d3c5-361">Windows 7 or later</span></span>
* <span data-ttu-id="6d3c5-362">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="6d3c5-362">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6d3c5-363">In caso di hosting in-process, il modulo usa un'implementazione di server in-process per IIS detta server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-363">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6d3c5-364">In caso di hosting out-of-process, il modulo funziona solo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-364">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="6d3c5-365">Il modulo non funziona con [http. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-365">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="6d3c5-366">Modelli di hosting</span><span class="sxs-lookup"><span data-stu-id="6d3c5-366">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="6d3c5-367">Modello di hosting in-process</span><span class="sxs-lookup"><span data-stu-id="6d3c5-367">In-process hosting model</span></span>

<span data-ttu-id="6d3c5-368">Per configurare un'app per l'hosting in-process, aggiungere la proprietà `<AspNetCoreHostingModel>` al file di progetto dell'app usando il valore `InProcess` (l'hosting out-of-process viene impostato con `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-368">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6d3c5-369">Il modello di hosting In-Process non è supportato per le app ASP.NET Core destinate a .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-369">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="6d3c5-370">Il valore di `<AspNetCoreHostingModel>` non fa distinzione tra maiuscole e minuscole, pertanto `inprocess` e `outofprocess` sono valori validi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-370">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="6d3c5-371">Se la proprietà `<AspNetCoreHostingModel>` non è presente nel file, il valore predefinito è `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-371">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="6d3c5-372">In caso di hosting in-process, vengono applicate le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-372">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="6d3c5-373">Viene usato un server HTTP di IIS (`IISHttpServer`) al posto del server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-373">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="6d3c5-374">Per in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> per:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-374">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="6d3c5-375">Registrare `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-375">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="6d3c5-376">Configurare la porta e il percorso di base su cui il server deve eseguire l'ascolto in caso di esecuzione dietro il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-376">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="6d3c5-377">Configurare l'host per l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-377">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="6d3c5-378">L'[attributo requestTimeout ](#attributes-of-the-aspnetcore-element) non viene applicato all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-378">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="6d3c5-379">Non è supportata la condivisione di un pool di app tra le app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-379">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="6d3c5-380">Usare un pool di app per ogni app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-380">Use one app pool per app.</span></span>

* <span data-ttu-id="6d3c5-381">Quando si usa la [Distribuzione Web ](/iis/publish/using-web-deploy/introduction-to-web-deploy) o si inserisce manualmente un [file app_offline.htm nella distribuzione](xref:host-and-deploy/iis/index#locked-deployment-files), l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-381">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6d3c5-382">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-382">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="6d3c5-383">L'architettura, vale a dire il numero di bit dell'app, e il runtime installato (x64 o x86) devono corrispondere all'architettura del pool di app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-383">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="6d3c5-384">Le disconnessioni del client vengono rilevate.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-384">Client disconnects are detected.</span></span> <span data-ttu-id="6d3c5-385">Il token di annullamento [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) viene cancellato quando il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-385">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="6d3c5-386">In ASP.NET Core 2.2.1 o versioni precedenti, <xref:System.IO.Directory.GetCurrentDirectory*> restituisce la directory di lavoro del processo avviato da IIS invece che la directory dell'app (ad esempio, *C:\Windows\System32\inetsrv* per *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-386">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="6d3c5-387">Per vedere il codice di esempio che imposta la directory corrente dell'app, vedere la [classe CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-387">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="6d3c5-388">Chiamare il metodo `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-388">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="6d3c5-389">Le chiamate successive a <xref:System.IO.Directory.GetCurrentDirectory*> specificano la directory dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-389">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="6d3c5-390">In caso di hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> non viene chiamato internamente per inizializzare un utente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-390">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="6d3c5-391">Pertanto, un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> usate per trasformare le attestazioni dopo ogni autenticazione non viene attivata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-391">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="6d3c5-392">Quando si trasformano le attestazioni con un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, chiamare <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> per aggiungere i servizi di autenticazione:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-392">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="6d3c5-393">Modello di hosting out-of-process</span><span class="sxs-lookup"><span data-stu-id="6d3c5-393">Out-of-process hosting model</span></span>

<span data-ttu-id="6d3c5-394">Per configurare un'app per l'hosting out-of-process, usare uno dei due approcci seguenti nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-394">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="6d3c5-395">Non specificare la proprietà `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-395">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="6d3c5-396">Se la proprietà `<AspNetCoreHostingModel>` non è presente nel file, il valore predefinito è `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-396">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="6d3c5-397">Impostare il valore della proprietà `<AspNetCoreHostingModel>` su `OutOfProcess` (l'hosting in-process è impostato con `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-397">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6d3c5-398">Il valore non fa distinzione tra maiuscole e minuscole, pertanto `inprocess` e `outofprocess` sono valori validi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-398">The value is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="6d3c5-399">Viene usato il server [Kestrel](xref:fundamentals/servers/kestrel) al posto di un server HTTP di IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-399">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6d3c5-400">Per out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> per:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-400">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="6d3c5-401">Configurare la porta e il percorso di base su cui il server deve eseguire l'ascolto in caso di esecuzione dietro il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-401">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="6d3c5-402">Configurare l'host per l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-402">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="6d3c5-403">Modifiche del modello di hosting</span><span class="sxs-lookup"><span data-stu-id="6d3c5-403">Hosting model changes</span></span>

<span data-ttu-id="6d3c5-404">Se l'impostazione `hostingModel` viene modificata nel file *web.config* (descritto nella sezione [Configurazione con web.config](#configuration-with-webconfig)), il modulo ricicla il processo di lavoro per IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-404">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="6d3c5-405">Per IIS Express, il modulo non ricicla il processo di lavoro. Attiva invece un arresto normale del processo di IIS Express corrente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-405">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="6d3c5-406">La richiesta successiva all'app attiva un nuovo processo di IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-406">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="6d3c5-407">Nome processo</span><span class="sxs-lookup"><span data-stu-id="6d3c5-407">Process name</span></span>

<span data-ttu-id="6d3c5-408">`Process.GetCurrentProcess().ProcessName` dichiara `w3wp`/`iisexpress` (In-Process) o `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-408">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="6d3c5-409">Molti moduli nativi, ad esempio l'autenticazione di Windows, rimangono attivi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-409">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="6d3c5-410">Per altre informazioni sui moduli IIS attivi con il modulo ASP.NET Core, vedere <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-410">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="6d3c5-411">Il modulo ASP.NET Core può anche:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-411">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="6d3c5-412">Impostare variabili di ambiente per il processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-412">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="6d3c5-413">Registrare output stdout in una risorsa di archiviazione di file per la risoluzione di problemi di avvio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-413">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="6d3c5-414">Inoltrare token di autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-414">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="6d3c5-415">Come installare e usare il modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d3c5-415">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="6d3c5-416">Per istruzioni su come installare e usare il modulo ASP.NET Core, vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-416">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="6d3c5-417">Configurazione con web.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-417">Configuration with web.config</span></span>

<span data-ttu-id="6d3c5-418">Il modulo ASP.NET Core viene configurato tramite la sezione `aspNetCore` del nodo `system.webServer` nel file *web.config* del sito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-418">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="6d3c5-419">Il file *web.config* seguente viene pubblicato per una [distribuzione dipendente dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo ASP.NET Core per gestire le richieste del sito:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-419">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="6d3c5-420">Il file *web.config* seguente viene pubblicato per una [distribuzione autonoma](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-420">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="6d3c5-421">La proprietà <xref:System.Configuration.SectionInformation.InheritInChildApplications*> è impostata su `false` per indicare che le impostazioni specificate nell'elemento [\<percorso>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) non sono ereditate da app che risiedono in una sottodirectory dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-421">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="6d3c5-422">Quando un'app viene distribuita in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), il percorso `stdoutLogFile` è impostato su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-422">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6d3c5-423">Il percorso salva i log stdout nella cartella *LogFiles*, ovvero una posizione creata automaticamente dal servizio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-423">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="6d3c5-424">Per informazioni sulla configurazione delle applicazioni secondarie IIS, vedere <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-424">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="6d3c5-425">Attributi dell'elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="6d3c5-425">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="6d3c5-426">Attributo</span><span class="sxs-lookup"><span data-stu-id="6d3c5-426">Attribute</span></span> | <span data-ttu-id="6d3c5-427">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-427">Description</span></span> | <span data-ttu-id="6d3c5-428">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6d3c5-428">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6d3c5-429">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-429">Optional string attribute.</span></span></p><p><span data-ttu-id="6d3c5-430">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-430">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6d3c5-431">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-431">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6d3c5-432">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-432">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6d3c5-433">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-433">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6d3c5-434">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-434">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6d3c5-435">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-435">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="6d3c5-436">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-436">Optional string attribute.</span></span></p><p><span data-ttu-id="6d3c5-437">Specifica il modello di hosting come in-process (`InProcess`/`inprocess`) o out-of-process (`OutOfProcess`/`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-437">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `OutOfProcess`<br>`outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="6d3c5-438">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-438">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-439">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-439">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6d3c5-440">&dagger;Per l'hosting in-process, il valore è limitato a `1`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-440">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="6d3c5-441">L'impostazione di `processesPerApplication` è sconsigliata.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-441">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="6d3c5-442">Questo attributo sarà rimosso nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-442">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="6d3c5-443">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-443">Default: `1`</span></span><br><span data-ttu-id="6d3c5-444">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-444">Min: `1`</span></span><br><span data-ttu-id="6d3c5-445">Max: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="6d3c5-445">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="6d3c5-446">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-446">Required string attribute.</span></span></p><p><span data-ttu-id="6d3c5-447">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-447">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6d3c5-448">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-448">Relative paths are supported.</span></span> <span data-ttu-id="6d3c5-449">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-449">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6d3c5-450">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-450">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-451">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-451">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6d3c5-452">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-452">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="6d3c5-453">Non supportato con l'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-453">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="6d3c5-454">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-454">Default: `10`</span></span><br><span data-ttu-id="6d3c5-455">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-455">Min: `0`</span></span><br><span data-ttu-id="6d3c5-456">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-456">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6d3c5-457">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-457">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6d3c5-458">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-458">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6d3c5-459">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-459">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="6d3c5-460">Non è applicabile all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-460">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="6d3c5-461">Per l'hosting in-process, il modulo resta in attesa che l'app elabori la richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-461">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="6d3c5-462">I valori validi per i segmenti della stringa relativi a minuti e secondi sono compresi nell'intervallo tra 0 e 59.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-462">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="6d3c5-463">Se si usa **60** come valore per i minuti o i secondi, viene generato un errore *500 - Errore interno del server*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-463">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="6d3c5-464">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-464">Default: `00:02:00`</span></span><br><span data-ttu-id="6d3c5-465">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-465">Min: `00:00:00`</span></span><br><span data-ttu-id="6d3c5-466">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-466">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6d3c5-467">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-467">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-468">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-468">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6d3c5-469">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-469">Default: `10`</span></span><br><span data-ttu-id="6d3c5-470">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-470">Min: `0`</span></span><br><span data-ttu-id="6d3c5-471">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-471">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6d3c5-472">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-472">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-473">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-473">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6d3c5-474">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-474">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6d3c5-475">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-475">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6d3c5-476">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-476">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6d3c5-477">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-477">Default: `120`</span></span><br><span data-ttu-id="6d3c5-478">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-478">Min: `0`</span></span><br><span data-ttu-id="6d3c5-479">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-479">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6d3c5-480">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-480">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6d3c5-481">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-481">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6d3c5-482">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-482">Optional string attribute.</span></span></p><p><span data-ttu-id="6d3c5-483">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-483">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6d3c5-484">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-484">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6d3c5-485">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-485">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6d3c5-486">Le eventuali cartelle specificate nel percorso vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-486">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="6d3c5-487">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file ( *.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-487">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6d3c5-488">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-488">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="6d3c5-489">Impostazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="6d3c5-489">Setting environment variables</span></span>

<span data-ttu-id="6d3c5-490">È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-490">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6d3c5-491">Specificare una variabile di ambiente con l'elemento figlio `<environmentVariable>` di un elemento della raccolta `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-491">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="6d3c5-492">Le variabili di ambiente impostate in questa sezione hanno la precedenza sulle variabili di ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-492">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="6d3c5-493">Nell'esempio seguente vengono impostate due variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-493">The following example sets two environment variables.</span></span> <span data-ttu-id="6d3c5-494">`ASPNETCORE_ENVIRONMENT` configura l'ambiente dell'app su `Development`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-494">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="6d3c5-495">Uno sviluppatore può impostare temporaneamente questo valore nel file *web.config* per forzare il caricamento della [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) durante il debug di un'eccezione dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-495">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="6d3c5-496">`CONFIG_DIR` è un esempio di variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento del file di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-496">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="6d3c5-497">Un'alternativa all'impostazione dell'ambiente direttamente in *web.config* è quella di includere la proprietà `<EnvironmentName>` nel profilo di pubblicazione ( *.pubxml*) o nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-497">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="6d3c5-498">Questo approccio imposta l'ambiente in *web.config* quando viene pubblicato il progetto:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-498">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="6d3c5-499">Impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` su `Development` solo in server di gestione temporanea e test che non sono accessibili da reti non attendibili, ad esempio Internet.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-499">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="6d3c5-500">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="6d3c5-500">app_offline.htm</span></span>

<span data-ttu-id="6d3c5-501">Se un file denominato *app_offline.htm* viene rilevato nella directory radice di un'app, il modulo ASP.NET Core tenta di arrestare normalmente l'app e arresta l'elaborazione delle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-501">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="6d3c5-502">Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo ASP.NET Core termina il processo in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-502">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="6d3c5-503">Mentre il file *app_offline.htm* è presente, il modulo ASP.NET Core risponde alle richieste restituendo il contenuto del file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-503">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="6d3c5-504">Quando il file *app_offline.htm* viene rimosso, la richiesta successiva avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-504">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="6d3c5-505">Quando si usa il modello di hosting out-of-process, l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-505">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6d3c5-506">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-506">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="6d3c5-507">Pagina di errore di avvio</span><span class="sxs-lookup"><span data-stu-id="6d3c5-507">Start-up error page</span></span>

<span data-ttu-id="6d3c5-508">Sia l'hosting In-Process che quello out-of-process producono pagine di errore personalizzate quando non riescono ad avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-508">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="6d3c5-509">Se il modulo ASP.NET Core non riesce a trovare il gestore richieste In-Process o out-of-process, viene visualizzata una tabella codici di stato *500.0 - Errore di caricamento del gestore In-Process/out-of-process*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-509">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="6d3c5-510">Per l'hosting In-Process, se il modulo ASP.NET Core non riesce ad avviare l'app, viene visualizzata una tabella codici di stato *500.30 - Errore di avvio*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-510">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="6d3c5-511">Per l'hosting out-of-process, se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-511">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="6d3c5-512">Per non visualizzare questa pagina e tornare alla tabella codici di stato 5xx predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-512">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6d3c5-513">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-513">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="6d3c5-514">Creazione e reindirizzamento dei log</span><span class="sxs-lookup"><span data-stu-id="6d3c5-514">Log creation and redirection</span></span>

<span data-ttu-id="6d3c5-515">Il modulo ASP.NET Core reindirizza su disco l'output della console stdout e stderr se sono impostati gli attributi `stdoutLogEnabled` e `stdoutLogFile` dell'elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-515">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="6d3c5-516">Le eventuali le cartelle nel percorso `stdoutLogFile` vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-516">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="6d3c5-517">Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-517">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6d3c5-518">I log non vengono ruotati, a meno che non si verifichi il riciclo/riavvio del processo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-518">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="6d3c5-519">È responsabilità del provider di servizi di hosting limitare lo spazio su disco usato dai log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-519">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="6d3c5-520">L'uso del log stdout è consigliato solo per la risoluzione dei problemi di avvio dell'app durante l'hosting in IIS o quando si usa il [supporto in fase di sviluppo per IIS con Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), non durante il debug in locale e l'esecuzione dell'app con IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-520">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="6d3c5-521">Non usare il log stdout per scopi di registrazione generale delle app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-521">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="6d3c5-522">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-522">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="6d3c5-523">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-523">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="6d3c5-524">Un timestamp e l'estensione del file vengono aggiunti automaticamente al momento della creazione del file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-524">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="6d3c5-525">Il nome del file di log è composto aggiungendo il timestamp, l'ID processo e l'estensione del file ( *.log*) all'ultimo segmento del percorso `stdoutLogFile` (in genere *stdout*), con caratteri di sottolineatura come delimitatori.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-525">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="6d3c5-526">Se il percorso `stdoutLogFile` termina con *stdout*, un log per un'app con un PID 1934 creata il 5/2/2018 alle 19:42:32 sarà denominato *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-526">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="6d3c5-527">Se `stdoutLogEnabled` è false, gli errori che si verificano all'avvio dell'app vengono acquisiti ed emessi nel log eventi fino a 30 KB.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-527">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="6d3c5-528">Dopo l'avvio, tutti i log aggiuntivi vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-528">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="6d3c5-529">Nell'elemento `aspNetCore` di esempio seguente viene configurata la registrazione di stdout nel percorso relativo `.\log\`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-529">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="6d3c5-530">Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-530">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="6d3c5-531">Quando si pubblica un'app per la distribuzione di app Azure Service, l'SDK Web imposta il valore di `stdoutLogFile` su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-531">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6d3c5-532">La variabile di ambiente `%home` è predefinita per le app ospitate dal servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-532">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="6d3c5-533">Per ulteriori informazioni sui formati di percorso, vedere [formati di percorso dei file nei sistemi Windows](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-533">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="6d3c5-534">Log di diagnostica avanzati</span><span class="sxs-lookup"><span data-stu-id="6d3c5-534">Enhanced diagnostic logs</span></span>

<span data-ttu-id="6d3c5-535">Il modulo ASP.NET Core può essere configurato per restituire log di diagnostica avanzata.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-535">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="6d3c5-536">Aggiungere l'elemento `<handlerSettings>` all'elemento `<aspNetCore>` in *Web. config*. L'impostazione del `debugLevel` su `TRACE` espone una maggiore fedeltà delle informazioni di diagnostica:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-536">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="6d3c5-537">Le cartelle nel percorso specificato per il valore `<handlerSetting>` (*logs* nell'esempio precedente) non vengono create automaticamente dal modulo e devono essere già presenti nella distribuzione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-537">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="6d3c5-538">Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-538">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6d3c5-539">I valori di livello debug (`debugLevel`) possono includere sia il livello che il percorso.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-539">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="6d3c5-540">Livelli (in ordine dal meno dettagliato al più dettagliato):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-540">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="6d3c5-541">ERROR</span><span class="sxs-lookup"><span data-stu-id="6d3c5-541">ERROR</span></span>
* <span data-ttu-id="6d3c5-542">WARNING</span><span class="sxs-lookup"><span data-stu-id="6d3c5-542">WARNING</span></span>
* <span data-ttu-id="6d3c5-543">INFO</span><span class="sxs-lookup"><span data-stu-id="6d3c5-543">INFO</span></span>
* <span data-ttu-id="6d3c5-544">TRACE</span><span class="sxs-lookup"><span data-stu-id="6d3c5-544">TRACE</span></span>

<span data-ttu-id="6d3c5-545">Posizioni (sono consentite più posizioni):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-545">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="6d3c5-546">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="6d3c5-546">CONSOLE</span></span>
* <span data-ttu-id="6d3c5-547">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="6d3c5-547">EVENTLOG</span></span>
* <span data-ttu-id="6d3c5-548">FILE</span><span class="sxs-lookup"><span data-stu-id="6d3c5-548">FILE</span></span>

<span data-ttu-id="6d3c5-549">Le impostazioni del gestore possono essere specificate anche tramite le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-549">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="6d3c5-550">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Percorso del file di logo di debug.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-550">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="6d3c5-551">(Impostazione predefinita: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="6d3c5-551">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="6d3c5-552">`ASPNETCORE_MODULE_DEBUG` &ndash; Impostazione del livello di debug.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-552">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="6d3c5-553">**Non** lasciare la registrazione del debug abilitata nella distribuzione per un tempo superiore a quello necessario alla risoluzione di problema.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-553">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="6d3c5-554">Le dimensioni del log non sono limitate.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-554">The size of the log isn't limited.</span></span> <span data-ttu-id="6d3c5-555">Se si lascia abilitato il log di debug, lo spazio disponibile su disco può esaurirsi e il server o il servizio app può registrare un arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-555">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="6d3c5-556">Vedere [Configurazione con web.config](#configuration-with-webconfig) per un esempio dell'elemento `aspNetCore` nel file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-556">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="6d3c5-557">La configurazione del proxy usa il protocollo HTTP e un token di associazione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-557">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="6d3c5-558">*Si applica solo all'hosting out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="6d3c5-558">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="6d3c5-559">Il proxy creato tra il modulo ASP.NET Core e Kestrel usa il protocollo HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-559">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="6d3c5-560">Non vi è alcun rischio di intercettazione del traffico tra il modulo e Kestrel da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-560">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="6d3c5-561">Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-561">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="6d3c5-562">Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-562">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="6d3c5-563">Il token di associazione viene impostato anche in un'intestazione (`MS-ASPNETCORE-TOKEN`) in tutte le richieste trasmesse tramite proxy.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-563">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="6d3c5-564">Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-564">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="6d3c5-565">Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-565">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="6d3c5-566">La variabile di ambiente del token di associazione e il traffico tra il modulo e Kestrel non sono accessibili da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-566">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="6d3c5-567">Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-567">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="6d3c5-568">Modulo ASP.NET Core con configurazione condivisa di IIS</span><span class="sxs-lookup"><span data-stu-id="6d3c5-568">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="6d3c5-569">Il programma di installazione del modulo ASP.NET Core viene eseguito con i privilegi dell'account **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-569">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="6d3c5-570">Poiché l'account di sistema locale non dispone dell'autorizzazione di modifica per il percorso della condivisione usato dalla configurazione condivisa di IIS, il programma di installazione genera un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo nel file *applicationHost.config* nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-570">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="6d3c5-571">Quando si usa una configurazione condivisa di IIS nello stesso computer dell'installazione di IIS, eseguire il programma di installazione del bundle di hosting ASP.NET Core con il parametro `OPT_NO_SHARED_CONFIG_CHECK` impostato su `1`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-571">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="6d3c5-572">Quando il percorso della configurazione condivisa non è nello stesso computer dell'installazione di IIS, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-572">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="6d3c5-573">Disabilitare la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-573">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6d3c5-574">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-574">Run the installer.</span></span>
1. <span data-ttu-id="6d3c5-575">Esportare il file *applicationHost.config* aggiornato nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-575">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6d3c5-576">Abilitare di nuovo la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-576">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="6d3c5-577">Versione del modulo e log del programma di installazione del bundle di hosting</span><span class="sxs-lookup"><span data-stu-id="6d3c5-577">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="6d3c5-578">Per determinare la versione del modulo ASP.NET Core installato:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-578">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="6d3c5-579">Nel sistema host passare a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-579">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="6d3c5-580">Individuare il file *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-580">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="6d3c5-581">Fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-581">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="6d3c5-582">Selezionare la scheda **Dettagli** . La versione del **file** e la **versione del prodotto** rappresentano la versione installata del modulo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-582">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="6d3c5-583">I log del programma di installazione del bundle di hosting per il modulo sono disponibili in *C:\\utenti\\% username%\\AppData\\\\locale*. Il file è denominato *dd_DotNetCoreWinSvrHosting__\<timestamp > _000_AspNetCoreModule_x64. log*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-583">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="6d3c5-584">Percorsi dei file di modulo, schema e configurazione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-584">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="6d3c5-585">Modulo</span><span class="sxs-lookup"><span data-stu-id="6d3c5-585">Module</span></span>

<span data-ttu-id="6d3c5-586">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-586">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="6d3c5-587">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-587">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-588">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-588">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-589">%Programmi%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-589">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6d3c5-590">%Programmi(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-590">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="6d3c5-591">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-591">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="6d3c5-592">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-592">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-593">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-593">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-594">%Programmi%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-594">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6d3c5-595">%Programmi(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-595">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="6d3c5-596">Schema</span><span class="sxs-lookup"><span data-stu-id="6d3c5-596">Schema</span></span>

<span data-ttu-id="6d3c5-597">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-597">**IIS**</span></span>

* <span data-ttu-id="6d3c5-598">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-598">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="6d3c5-599">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-599">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="6d3c5-600">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-600">**IIS Express**</span></span>

* <span data-ttu-id="6d3c5-601">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-601">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="6d3c5-602">%Programmi%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-602">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="6d3c5-603">Configurazione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-603">Configuration</span></span>

<span data-ttu-id="6d3c5-604">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-604">**IIS**</span></span>

* <span data-ttu-id="6d3c5-605">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-605">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="6d3c5-606">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-606">**IIS Express**</span></span>

* <span data-ttu-id="6d3c5-607">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-607">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="6d3c5-608">Interfaccia della riga di comando di *iisexpress.exe*: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-608">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="6d3c5-609">È possibile trovare i file eseguendo una ricerca di *aspnetcore* nel file *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-609">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6d3c5-610">Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS che inoltra le richieste Web alle app back-end ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-610">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="6d3c5-611">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-611">Supported Windows versions:</span></span>

* <span data-ttu-id="6d3c5-612">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="6d3c5-612">Windows 7 or later</span></span>
* <span data-ttu-id="6d3c5-613">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="6d3c5-613">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6d3c5-614">Il modulo funziona solo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-614">The module only works with Kestrel.</span></span> <span data-ttu-id="6d3c5-615">Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-615">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="6d3c5-616">Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue anche la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-616">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="6d3c5-617">Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se si verifica un arresto anomalo di questa.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-617">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="6d3c5-618">Si tratta essenzialmente dello stesso comportamento delle app ASP.NET 4.x eseguite In-Process in IIS e gestite dal [servizio Attivazione processo Windows (WAS, Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-618">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="6d3c5-619">Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-619">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="6d3c5-621">Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-621">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="6d3c5-622">Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-622">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="6d3c5-623">Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-623">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="6d3c5-624">Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il [middleware di integrazione IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configura il server per l'ascolto su `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-624">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="6d3c5-625">Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-625">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="6d3c5-626">Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-626">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="6d3c5-627">Dopo che Kestrel ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-627">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="6d3c5-628">La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-628">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="6d3c5-629">Il middleware aggiunto dall'integrazione di IIS aggiorna lo schema, l'IP remoto e il percorso di base all'account per l'inoltro della richiesta a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-629">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="6d3c5-630">La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-630">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="6d3c5-631">Molti moduli nativi, ad esempio l'autenticazione di Windows, rimangono attivi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-631">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="6d3c5-632">Per altre informazioni sui moduli IIS attivi con il modulo ASP.NET Core, vedere <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-632">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="6d3c5-633">Il modulo ASP.NET Core può anche:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-633">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="6d3c5-634">Impostare variabili di ambiente per il processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-634">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="6d3c5-635">Registrare output stdout in una risorsa di archiviazione di file per la risoluzione di problemi di avvio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-635">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="6d3c5-636">Inoltrare token di autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-636">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="6d3c5-637">Come installare e usare il modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d3c5-637">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="6d3c5-638">Per istruzioni su come installare e usare il modulo ASP.NET Core, vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-638">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="6d3c5-639">Configurazione con web.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-639">Configuration with web.config</span></span>

<span data-ttu-id="6d3c5-640">Il modulo ASP.NET Core viene configurato tramite la sezione `aspNetCore` del nodo `system.webServer` nel file *web.config* del sito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-640">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="6d3c5-641">Il file *web.config* seguente viene pubblicato per una [distribuzione dipendente dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo ASP.NET Core per gestire le richieste del sito:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-641">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="6d3c5-642">Il file *web.config* seguente viene pubblicato per una [distribuzione autonoma](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-642">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="6d3c5-643">Quando un'app viene distribuita in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), il percorso `stdoutLogFile` è impostato su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-643">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6d3c5-644">Il percorso salva i log stdout nella cartella *LogFiles*, ovvero una posizione creata automaticamente dal servizio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-644">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="6d3c5-645">Per informazioni sulla configurazione delle applicazioni secondarie IIS, vedere <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-645">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="6d3c5-646">Attributi dell'elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="6d3c5-646">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="6d3c5-647">Attributo</span><span class="sxs-lookup"><span data-stu-id="6d3c5-647">Attribute</span></span> | <span data-ttu-id="6d3c5-648">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-648">Description</span></span> | <span data-ttu-id="6d3c5-649">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6d3c5-649">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6d3c5-650">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-650">Optional string attribute.</span></span></p><p><span data-ttu-id="6d3c5-651">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-651">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6d3c5-652">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-652">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6d3c5-653">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-653">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6d3c5-654">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-654">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6d3c5-655">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-655">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6d3c5-656">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-656">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="6d3c5-657">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-657">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-658">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-658">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6d3c5-659">L'impostazione di `processesPerApplication` è sconsigliata.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-659">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="6d3c5-660">Questo attributo sarà rimosso nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-660">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="6d3c5-661">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-661">Default: `1`</span></span><br><span data-ttu-id="6d3c5-662">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-662">Min: `1`</span></span><br><span data-ttu-id="6d3c5-663">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-663">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="6d3c5-664">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-664">Required string attribute.</span></span></p><p><span data-ttu-id="6d3c5-665">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-665">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6d3c5-666">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-666">Relative paths are supported.</span></span> <span data-ttu-id="6d3c5-667">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-667">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6d3c5-668">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-668">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-669">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-669">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6d3c5-670">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-670">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="6d3c5-671">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-671">Default: `10`</span></span><br><span data-ttu-id="6d3c5-672">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-672">Min: `0`</span></span><br><span data-ttu-id="6d3c5-673">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-673">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6d3c5-674">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-674">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6d3c5-675">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-675">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6d3c5-676">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-676">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="6d3c5-677">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-677">Default: `00:02:00`</span></span><br><span data-ttu-id="6d3c5-678">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-678">Min: `00:00:00`</span></span><br><span data-ttu-id="6d3c5-679">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-679">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6d3c5-680">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-680">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-681">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-681">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6d3c5-682">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-682">Default: `10`</span></span><br><span data-ttu-id="6d3c5-683">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-683">Min: `0`</span></span><br><span data-ttu-id="6d3c5-684">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-684">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6d3c5-685">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-685">Optional integer attribute.</span></span></p><p><span data-ttu-id="6d3c5-686">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-686">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6d3c5-687">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-687">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6d3c5-688">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-688">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6d3c5-689">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-689">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6d3c5-690">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-690">Default: `120`</span></span><br><span data-ttu-id="6d3c5-691">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-691">Min: `0`</span></span><br><span data-ttu-id="6d3c5-692">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-692">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6d3c5-693">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-693">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6d3c5-694">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-694">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6d3c5-695">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-695">Optional string attribute.</span></span></p><p><span data-ttu-id="6d3c5-696">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-696">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6d3c5-697">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-697">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6d3c5-698">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-698">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6d3c5-699">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-699">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="6d3c5-700">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file ( *.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-700">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6d3c5-701">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-701">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="6d3c5-702">Impostazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="6d3c5-702">Setting environment variables</span></span>

<span data-ttu-id="6d3c5-703">È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-703">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6d3c5-704">Specificare una variabile di ambiente con l'elemento figlio `<environmentVariable>` di un elemento della raccolta `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-704">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="6d3c5-705">Le variabili di ambiente impostate in questa sezione sono in conflitto con le variabili di ambiente di sistema impostate con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-705">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="6d3c5-706">Se una variabile di ambiente è impostata sia nel file *web.config* sia a livello di sistema in Windows, il valore del file *web.config* viene aggiunto al valore della variabile di ambiente di sistema, ad esempio `ASPNETCORE_ENVIRONMENT: Development;Development`, e ciò impedisce l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-706">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="6d3c5-707">Nell'esempio seguente vengono impostate due variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-707">The following example sets two environment variables.</span></span> <span data-ttu-id="6d3c5-708">`ASPNETCORE_ENVIRONMENT` configura l'ambiente dell'app su `Development`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-708">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="6d3c5-709">Uno sviluppatore può impostare temporaneamente questo valore nel file *web.config* per forzare il caricamento della [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) durante il debug di un'eccezione dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-709">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="6d3c5-710">`CONFIG_DIR` è un esempio di variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento del file di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-710">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!WARNING]
> <span data-ttu-id="6d3c5-711">Impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` su `Development` solo in server di gestione temporanea e test che non sono accessibili da reti non attendibili, ad esempio Internet.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-711">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="6d3c5-712">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="6d3c5-712">app_offline.htm</span></span>

<span data-ttu-id="6d3c5-713">Se un file denominato *app_offline.htm* viene rilevato nella directory radice di un'app, il modulo ASP.NET Core tenta di arrestare normalmente l'app e arresta l'elaborazione delle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-713">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="6d3c5-714">Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo ASP.NET Core termina il processo in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-714">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="6d3c5-715">Mentre il file *app_offline.htm* è presente, il modulo ASP.NET Core risponde alle richieste restituendo il contenuto del file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-715">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="6d3c5-716">Quando il file *app_offline.htm* viene rimosso, la richiesta successiva avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-716">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="6d3c5-717">Pagina di errore di avvio</span><span class="sxs-lookup"><span data-stu-id="6d3c5-717">Start-up error page</span></span>

<span data-ttu-id="6d3c5-718">Se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-718">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="6d3c5-719">Per non visualizzare questa pagina e tornare alla tabella codici di stato 502 predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-719">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6d3c5-720">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-720">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Tabella codici di stato 502.5 Errore del processo](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="6d3c5-722">Creazione e reindirizzamento dei log</span><span class="sxs-lookup"><span data-stu-id="6d3c5-722">Log creation and redirection</span></span>

<span data-ttu-id="6d3c5-723">Il modulo ASP.NET Core reindirizza su disco l'output della console stdout e stderr se sono impostati gli attributi `stdoutLogEnabled` e `stdoutLogFile` dell'elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-723">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="6d3c5-724">Le eventuali le cartelle nel percorso `stdoutLogFile` vengono create dal modulo quando viene creato il file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-724">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="6d3c5-725">Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-725">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6d3c5-726">I log non vengono ruotati, a meno che non si verifichi il riciclo/riavvio del processo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-726">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="6d3c5-727">È responsabilità del provider di servizi di hosting limitare lo spazio su disco usato dai log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-727">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="6d3c5-728">L'uso del log stdout è consigliato solo per la risoluzione dei problemi di avvio dell'app durante l'hosting in IIS o quando si usa il [supporto in fase di sviluppo per IIS con Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), non durante il debug in locale e l'esecuzione dell'app con IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-728">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="6d3c5-729">Non usare il log stdout per scopi di registrazione generale delle app.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-729">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="6d3c5-730">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-730">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="6d3c5-731">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-731">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="6d3c5-732">Un timestamp e l'estensione del file vengono aggiunti automaticamente al momento della creazione del file di log.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-732">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="6d3c5-733">Il nome del file di log è composto aggiungendo il timestamp, l'ID processo e l'estensione del file ( *.log*) all'ultimo segmento del percorso `stdoutLogFile` (in genere *stdout*), con caratteri di sottolineatura come delimitatori.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-733">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="6d3c5-734">Se il percorso `stdoutLogFile` termina con *stdout*, un log per un'app con un PID 1934 creata il 5/2/2018 alle 19:42:32 sarà denominato *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-734">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="6d3c5-735">Nell'elemento `aspNetCore` di esempio seguente viene configurata la registrazione di stdout nel percorso relativo `.\log\`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-735">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="6d3c5-736">Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-736">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout">
</aspNetCore>
```

<span data-ttu-id="6d3c5-737">Quando si pubblica un'app per la distribuzione di app Azure Service, l'SDK Web imposta il valore di `stdoutLogFile` su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-737">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6d3c5-738">La variabile di ambiente `%home` è predefinita per le app ospitate dal servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-738">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="6d3c5-739">Per creare regole di filtro di registrazione, vedere le sezioni [configurazione](xref:fundamentals/logging/index#log-filtering) e [filtro dei log](xref:fundamentals/logging/index#log-filtering) della documentazione relativa alla registrazione del ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-739">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="6d3c5-740">Per ulteriori informazioni sui formati di percorso, vedere [formati di percorso dei file nei sistemi Windows](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-740">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="6d3c5-741">La configurazione del proxy usa il protocollo HTTP e un token di associazione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-741">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="6d3c5-742">Il proxy creato tra il modulo ASP.NET Core e Kestrel usa il protocollo HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-742">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="6d3c5-743">Non vi è alcun rischio di intercettazione del traffico tra il modulo e Kestrel da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-743">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="6d3c5-744">Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-744">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="6d3c5-745">Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-745">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="6d3c5-746">Il token di associazione viene impostato anche in un'intestazione (`MS-ASPNETCORE-TOKEN`) in tutte le richieste trasmesse tramite proxy.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-746">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="6d3c5-747">Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-747">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="6d3c5-748">Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-748">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="6d3c5-749">La variabile di ambiente del token di associazione e il traffico tra il modulo e Kestrel non sono accessibili da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-749">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="6d3c5-750">Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-750">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="6d3c5-751">Modulo ASP.NET Core con configurazione condivisa di IIS</span><span class="sxs-lookup"><span data-stu-id="6d3c5-751">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="6d3c5-752">Il programma di installazione del modulo ASP.NET Core viene eseguito con i privilegi dell'account **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-752">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="6d3c5-753">Poiché l'account di sistema locale non dispone dell'autorizzazione di modifica per il percorso della condivisione usato dalla configurazione condivisa di IIS, il programma di installazione genera un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo nel file *applicationHost.config* nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-753">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="6d3c5-754">Quando si usa una configurazione condivisa di IIS, attenersi ai passaggi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-754">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="6d3c5-755">Disabilitare la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-755">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6d3c5-756">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-756">Run the installer.</span></span>
1. <span data-ttu-id="6d3c5-757">Esportare il file *applicationHost.config* aggiornato nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-757">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6d3c5-758">Abilitare di nuovo la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-758">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="6d3c5-759">Versione del modulo e log del programma di installazione del bundle di hosting</span><span class="sxs-lookup"><span data-stu-id="6d3c5-759">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="6d3c5-760">Per determinare la versione del modulo ASP.NET Core installato:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-760">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="6d3c5-761">Nel sistema host passare a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-761">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="6d3c5-762">Individuare il file *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-762">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="6d3c5-763">Fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-763">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="6d3c5-764">Selezionare la scheda **Dettagli** . La versione del **file** e la **versione del prodotto** rappresentano la versione installata del modulo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-764">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="6d3c5-765">I log del programma di installazione del bundle di hosting per il modulo sono disponibili in *C:\\utenti\\% username%\\AppData\\\\locale*. Il file è denominato *dd_DotNetCoreWinSvrHosting__\<timestamp > _000_AspNetCoreModule_x64. log*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-765">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="6d3c5-766">Percorsi dei file di modulo, schema e configurazione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-766">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="6d3c5-767">Modulo</span><span class="sxs-lookup"><span data-stu-id="6d3c5-767">Module</span></span>

<span data-ttu-id="6d3c5-768">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-768">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="6d3c5-769">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-769">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-770">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-770">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="6d3c5-771">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-771">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="6d3c5-772">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-772">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6d3c5-773">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6d3c5-773">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="6d3c5-774">Schema</span><span class="sxs-lookup"><span data-stu-id="6d3c5-774">Schema</span></span>

<span data-ttu-id="6d3c5-775">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-775">**IIS**</span></span>

* <span data-ttu-id="6d3c5-776">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-776">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="6d3c5-777">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-777">**IIS Express**</span></span>

* <span data-ttu-id="6d3c5-778">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6d3c5-778">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="6d3c5-779">Configurazione</span><span class="sxs-lookup"><span data-stu-id="6d3c5-779">Configuration</span></span>

<span data-ttu-id="6d3c5-780">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-780">**IIS**</span></span>

* <span data-ttu-id="6d3c5-781">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-781">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="6d3c5-782">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6d3c5-782">**IIS Express**</span></span>

* <span data-ttu-id="6d3c5-783">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-783">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="6d3c5-784">Interfaccia della riga di comando di *iisexpress.exe*: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="6d3c5-784">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="6d3c5-785">È possibile trovare i file eseguendo una ricerca di *aspnetcore* nel file *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-785">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6d3c5-786">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6d3c5-786">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="6d3c5-787">Repository GitHub del modulo ASP.NET Core (origine riferimento)</span><span class="sxs-lookup"><span data-stu-id="6d3c5-787">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
