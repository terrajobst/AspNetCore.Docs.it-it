---
title: Guida di riferimento per la configurazione del modulo ASP.NET Core
author: guardrex
description: Informazioni su come configurare il modulo di ASP.NET Core per l'hosting di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0ad73d89ffa3a8a3625c6e248efaad821e1b4d0a
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121557"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="2646b-103">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2646b-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="2646b-104">Di [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="2646b-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="2646b-105">Questo documento fornisce istruzioni su come configurare il modulo ASP.NET Core per l'hosting di app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2646b-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="2646b-106">Per un'introduzione al modulo ASP.NET Core e istruzioni per l'installazione, vedere la [panoramica del modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="2646b-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="2646b-107">Modello di hosting</span><span class="sxs-lookup"><span data-stu-id="2646b-107">Hosting model</span></span>

<span data-ttu-id="2646b-108">Per le app che vengono eseguite in .NET Core 2.2 o versione successiva, il modulo supporta un modello di hosting in-process, che garantisce prestazioni migliori rispetto al tipo di hosting a proxy inverso (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="2646b-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="2646b-109">Per ulteriori informazioni, vedere <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="2646b-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="2646b-110">L'hosting in-process acconsente esplicitamente le app esistenti, mentre i modelli [dotnet new](/dotnet/core/tools/dotnet-new) usano per impostazione predefinita il modello di hosting in-process per tutti gli scenari IIS e IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2646b-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="2646b-111">Per configurare un'app per l'hosting In-Process, aggiungere la proprietà `<AspNetCoreHostingModel>` al file di progetto dell'app (ad esempio, *MyApp.csproj*), usando il valore `InProcess` (l'hosting out-of-process viene impostato con `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="2646b-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `InProcess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="2646b-112">In caso di hosting in-process, vengono applicate le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="2646b-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="2646b-113">Un server HTTP di IIS (`IISHttpServer`) viene utilizzato al posto del server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2646b-113">IIS HTTP Server (`IISHttpServer`) is used instead of the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="2646b-114">Il server HTTP di IIS (`IISHttpServer`) è un'altra implementazione <xref:Microsoft.AspNetCore.Hosting.Server.IServer> che converte le richieste native di IIS in richieste gestite di ASP.NET Core da sottoporre all'app per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="2646b-114">IIS HTTP Server (`IISHttpServer`) is another <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation that converts IIS native requests into ASP.NET Core managed requests for processing by the app.</span></span>

* <span data-ttu-id="2646b-115">L'[attributo requestTimeout ](#attributes-of-the-aspnetcore-element) non viene applicato all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="2646b-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="2646b-116">Non è supportata la condivisione di un pool di app tra le app.</span><span class="sxs-lookup"><span data-stu-id="2646b-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="2646b-117">Usare un pool di app per ogni app.</span><span class="sxs-lookup"><span data-stu-id="2646b-117">Use one app pool per app.</span></span>

* <span data-ttu-id="2646b-118">Quando si usa la [Distribuzione Web ](/iis/publish/using-web-deploy/introduction-to-web-deploy) o si inserisce manualmente un [file app_offline.htm nella distribuzione](xref:host-and-deploy/iis/index#locked-deployment-files), l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="2646b-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="2646b-119">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="2646b-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="2646b-120">L'architettura, vale a dire il numero di bit dell'app, e il runtime installato (x64 o x86) devono corrispondere all'architettura del pool di app.</span><span class="sxs-lookup"><span data-stu-id="2646b-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="2646b-121">Se si configura l'host dell'app manualmente con `WebHostBuilder` (non con [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) e l'app viene sempre eseguita direttamente nel server di Kestrel (self-hosted), chiamare `UseKestrel` prima di chiamare `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="2646b-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="2646b-122">Se l'ordine viene invertito, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="2646b-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="2646b-123">Le disconnessioni del client vengono rilevate.</span><span class="sxs-lookup"><span data-stu-id="2646b-123">Client disconnects are detected.</span></span> <span data-ttu-id="2646b-124">Il token di annullamento [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) viene cancellato quando il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="2646b-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="2646b-125"><xref:System.IO.Directory.GetCurrentDirectory*> restituisce la directory di lavoro del processo avviato da IIS invece che la directory dell'app (ad esempio, *C:\Windows\System32\inetsrv* per *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="2646b-125"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="2646b-126">Per vedere il codice di esempio che imposta la directory corrente dell'app, vedere la [classe CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="2646b-126">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="2646b-127">Chiamare il metodo `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="2646b-127">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="2646b-128">Le chiamate successive a <xref:System.IO.Directory.GetCurrentDirectory*> specificano la directory dell'app.</span><span class="sxs-lookup"><span data-stu-id="2646b-128">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="2646b-129">Modifiche del modello di hosting</span><span class="sxs-lookup"><span data-stu-id="2646b-129">Hosting model changes</span></span>

<span data-ttu-id="2646b-130">Se l'impostazione `hostingModel` viene modificata nel file *web.config* (descritto nella sezione [Configurazione con web.config](#configuration-with-webconfig)), il modulo ricicla il processo di lavoro per IIS.</span><span class="sxs-lookup"><span data-stu-id="2646b-130">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="2646b-131">Per IIS Express, il modulo non ricicla il processo di lavoro. Attiva invece un arresto normale del processo di IIS Express corrente.</span><span class="sxs-lookup"><span data-stu-id="2646b-131">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="2646b-132">La richiesta successiva all'app attiva un nuovo processo di IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2646b-132">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="2646b-133">Nome processo</span><span class="sxs-lookup"><span data-stu-id="2646b-133">Process name</span></span>

<span data-ttu-id="2646b-134">`Process.GetCurrentProcess().ProcessName` dichiara `w3wp`/`iisexpress` (In-Process) o `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="2646b-134">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="2646b-135">Configurazione con web.config</span><span class="sxs-lookup"><span data-stu-id="2646b-135">Configuration with web.config</span></span>

<span data-ttu-id="2646b-136">Il modulo ASP.NET Core viene configurato tramite la sezione `aspNetCore` del nodo `system.webServer` nel file *web.config* del sito.</span><span class="sxs-lookup"><span data-stu-id="2646b-136">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="2646b-137">Il file *web.config* seguente viene pubblicato per una [distribuzione dipendente dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo ASP.NET Core per gestire le richieste del sito:</span><span class="sxs-lookup"><span data-stu-id="2646b-137">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="2646b-138">Il file *web.config* seguente viene pubblicato per una [distribuzione autonoma](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="2646b-138">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="2646b-139">La proprietà <xref:System.Configuration.SectionInformation.InheritInChildApplications*> è impostata su `false` per indicare che le impostazioni specificate nell'elemento [\<percorso>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) non sono ereditate da app che risiedono in una sottodirectory dell'app.</span><span class="sxs-lookup"><span data-stu-id="2646b-139">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="2646b-140">Quando un'app viene distribuita in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), il percorso `stdoutLogFile` è impostato su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="2646b-140">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="2646b-141">Il percorso salva i log stdout nella cartella *LogFiles*, ovvero una posizione creata automaticamente dal servizio.</span><span class="sxs-lookup"><span data-stu-id="2646b-141">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="2646b-142">Per informazioni sulla configurazione delle applicazioni secondarie IIS, vedere <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="2646b-142">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="2646b-143">Attributi dell'elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="2646b-143">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="2646b-144">Attributo</span><span class="sxs-lookup"><span data-stu-id="2646b-144">Attribute</span></span> | <span data-ttu-id="2646b-145">Description</span><span class="sxs-lookup"><span data-stu-id="2646b-145">Description</span></span> | <span data-ttu-id="2646b-146">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2646b-146">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2646b-147">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-147">Optional string attribute.</span></span></p><p><span data-ttu-id="2646b-148">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2646b-148">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2646b-149">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-149">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2646b-150">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="2646b-150">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2646b-151">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-151">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2646b-152">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="2646b-152">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2646b-153">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="2646b-153">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="2646b-154">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-154">Optional string attribute.</span></span></p><p><span data-ttu-id="2646b-155">Specifica il modello di hosting in-process (`InProcess`) o out-of-process (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="2646b-155">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="2646b-156">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-156">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-157">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="2646b-157">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="2646b-158">&dagger;Per l'hosting in-process, il valore è limitato a `1`.</span><span class="sxs-lookup"><span data-stu-id="2646b-158">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="2646b-159">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="2646b-159">Default: `1`</span></span><br><span data-ttu-id="2646b-160">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="2646b-160">Min: `1`</span></span><br><span data-ttu-id="2646b-161">Max: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="2646b-161">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="2646b-162">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="2646b-162">Required string attribute.</span></span></p><p><span data-ttu-id="2646b-163">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="2646b-163">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2646b-164">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="2646b-164">Relative paths are supported.</span></span> <span data-ttu-id="2646b-165">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="2646b-165">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2646b-166">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-166">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-167">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2646b-167">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2646b-168">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="2646b-168">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="2646b-169">Non supportato con l'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="2646b-169">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="2646b-170">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="2646b-170">Default: `10`</span></span><br><span data-ttu-id="2646b-171">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="2646b-171">Min: `0`</span></span><br><span data-ttu-id="2646b-172">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="2646b-172">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2646b-173">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-173">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2646b-174">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="2646b-174">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2646b-175">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="2646b-175">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="2646b-176">Non è applicabile all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="2646b-176">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="2646b-177">Per l'hosting in-process, il modulo resta in attesa che l'app elabori la richiesta.</span><span class="sxs-lookup"><span data-stu-id="2646b-177">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="2646b-178">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2646b-178">Default: `00:02:00`</span></span><br><span data-ttu-id="2646b-179">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2646b-179">Min: `00:00:00`</span></span><br><span data-ttu-id="2646b-180">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2646b-180">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2646b-181">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-182">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2646b-182">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2646b-183">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="2646b-183">Default: `10`</span></span><br><span data-ttu-id="2646b-184">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="2646b-184">Min: `0`</span></span><br><span data-ttu-id="2646b-185">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="2646b-185">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2646b-186">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-186">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-187">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="2646b-187">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2646b-188">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="2646b-188">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2646b-189">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="2646b-189">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="2646b-190">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="2646b-190">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="2646b-191">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="2646b-191">Default: `120`</span></span><br><span data-ttu-id="2646b-192">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="2646b-192">Min: `0`</span></span><br><span data-ttu-id="2646b-193">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="2646b-193">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2646b-194">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-194">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2646b-195">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2646b-195">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2646b-196">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-196">Optional string attribute.</span></span></p><p><span data-ttu-id="2646b-197">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2646b-197">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2646b-198">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="2646b-198">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2646b-199">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="2646b-199">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2646b-200">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="2646b-200">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2646b-201">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2646b-201">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2646b-202">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="2646b-202">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="2646b-203">Attributo</span><span class="sxs-lookup"><span data-stu-id="2646b-203">Attribute</span></span> | <span data-ttu-id="2646b-204">Description</span><span class="sxs-lookup"><span data-stu-id="2646b-204">Description</span></span> | <span data-ttu-id="2646b-205">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2646b-205">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2646b-206">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-206">Optional string attribute.</span></span></p><p><span data-ttu-id="2646b-207">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2646b-207">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2646b-208">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-208">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2646b-209">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="2646b-209">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2646b-210">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-210">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2646b-211">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="2646b-211">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2646b-212">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="2646b-212">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="2646b-213">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-213">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-214">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="2646b-214">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="2646b-215">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="2646b-215">Default: `1`</span></span><br><span data-ttu-id="2646b-216">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="2646b-216">Min: `1`</span></span><br><span data-ttu-id="2646b-217">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="2646b-217">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="2646b-218">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="2646b-218">Required string attribute.</span></span></p><p><span data-ttu-id="2646b-219">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="2646b-219">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2646b-220">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="2646b-220">Relative paths are supported.</span></span> <span data-ttu-id="2646b-221">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="2646b-221">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2646b-222">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-223">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2646b-223">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2646b-224">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="2646b-224">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="2646b-225">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="2646b-225">Default: `10`</span></span><br><span data-ttu-id="2646b-226">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="2646b-226">Min: `0`</span></span><br><span data-ttu-id="2646b-227">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="2646b-227">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2646b-228">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-228">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2646b-229">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="2646b-229">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2646b-230">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="2646b-230">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="2646b-231">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2646b-231">Default: `00:02:00`</span></span><br><span data-ttu-id="2646b-232">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2646b-232">Min: `00:00:00`</span></span><br><span data-ttu-id="2646b-233">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2646b-233">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2646b-234">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-235">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2646b-235">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2646b-236">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="2646b-236">Default: `10`</span></span><br><span data-ttu-id="2646b-237">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="2646b-237">Min: `0`</span></span><br><span data-ttu-id="2646b-238">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="2646b-238">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2646b-239">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-240">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="2646b-240">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2646b-241">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="2646b-241">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2646b-242">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="2646b-242">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="2646b-243">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="2646b-243">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="2646b-244">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="2646b-244">Default: `120`</span></span><br><span data-ttu-id="2646b-245">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="2646b-245">Min: `0`</span></span><br><span data-ttu-id="2646b-246">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="2646b-246">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2646b-247">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-247">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2646b-248">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2646b-248">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2646b-249">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-249">Optional string attribute.</span></span></p><p><span data-ttu-id="2646b-250">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2646b-250">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2646b-251">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="2646b-251">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2646b-252">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="2646b-252">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2646b-253">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="2646b-253">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2646b-254">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2646b-254">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2646b-255">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="2646b-255">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="2646b-256">Attributo</span><span class="sxs-lookup"><span data-stu-id="2646b-256">Attribute</span></span> | <span data-ttu-id="2646b-257">Description</span><span class="sxs-lookup"><span data-stu-id="2646b-257">Description</span></span> | <span data-ttu-id="2646b-258">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2646b-258">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2646b-259">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-259">Optional string attribute.</span></span></p><p><span data-ttu-id="2646b-260">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2646b-260">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2646b-261">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2646b-262">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="2646b-262">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2646b-263">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2646b-264">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="2646b-264">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2646b-265">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="2646b-265">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="2646b-266">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-266">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-267">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="2646b-267">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="2646b-268">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="2646b-268">Default: `1`</span></span><br><span data-ttu-id="2646b-269">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="2646b-269">Min: `1`</span></span><br><span data-ttu-id="2646b-270">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="2646b-270">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="2646b-271">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="2646b-271">Required string attribute.</span></span></p><p><span data-ttu-id="2646b-272">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="2646b-272">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2646b-273">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="2646b-273">Relative paths are supported.</span></span> <span data-ttu-id="2646b-274">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="2646b-274">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2646b-275">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-276">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2646b-276">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2646b-277">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="2646b-277">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="2646b-278">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="2646b-278">Default: `10`</span></span><br><span data-ttu-id="2646b-279">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="2646b-279">Min: `0`</span></span><br><span data-ttu-id="2646b-280">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="2646b-280">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2646b-281">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-281">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2646b-282">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="2646b-282">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2646b-283">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.0 o versioni precedenti, `requestTimeout` deve essere specificato solo in minuti interi. In caso contrario, verrà usato il valore predefinito di 2 minuti.</span><span class="sxs-lookup"><span data-stu-id="2646b-283">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="2646b-284">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2646b-284">Default: `00:02:00`</span></span><br><span data-ttu-id="2646b-285">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2646b-285">Min: `00:00:00`</span></span><br><span data-ttu-id="2646b-286">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2646b-286">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2646b-287">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-288">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2646b-288">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2646b-289">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="2646b-289">Default: `10`</span></span><br><span data-ttu-id="2646b-290">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="2646b-290">Min: `0`</span></span><br><span data-ttu-id="2646b-291">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="2646b-291">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2646b-292">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-292">Optional integer attribute.</span></span></p><p><span data-ttu-id="2646b-293">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="2646b-293">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2646b-294">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="2646b-294">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2646b-295">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="2646b-295">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="2646b-296">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="2646b-296">Default: `120`</span></span><br><span data-ttu-id="2646b-297">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="2646b-297">Min: `0`</span></span><br><span data-ttu-id="2646b-298">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="2646b-298">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2646b-299">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-299">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2646b-300">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2646b-300">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2646b-301">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2646b-301">Optional string attribute.</span></span></p><p><span data-ttu-id="2646b-302">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2646b-302">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2646b-303">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="2646b-303">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2646b-304">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="2646b-304">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2646b-305">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="2646b-305">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2646b-306">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2646b-306">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2646b-307">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="2646b-307">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="2646b-308">Impostazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="2646b-308">Setting environment variables</span></span>

<span data-ttu-id="2646b-309">È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="2646b-309">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="2646b-310">Specificare una variabile di ambiente con l'elemento figlio `environmentVariable` di un elemento della raccolta `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="2646b-310">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="2646b-311">Le variabili di ambiente impostate in questa sezione hanno la precedenza sulle variabili di ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="2646b-311">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="2646b-312">Nell'esempio seguente vengono impostate due variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="2646b-312">The following example sets two environment variables.</span></span> <span data-ttu-id="2646b-313">`ASPNETCORE_ENVIRONMENT` configura l'ambiente dell'app su `Development`.</span><span class="sxs-lookup"><span data-stu-id="2646b-313">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="2646b-314">Uno sviluppatore può impostare temporaneamente questo valore nel file *web.config* per forzare il caricamento della [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) durante il debug di un'eccezione dell'app.</span><span class="sxs-lookup"><span data-stu-id="2646b-314">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="2646b-315">`CONFIG_DIR` è un esempio di variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento del file di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="2646b-315">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!WARNING]
> <span data-ttu-id="2646b-316">Impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` su `Development` solo in server di gestione temporanea e test che non sono accessibili da reti non attendibili, ad esempio Internet.</span><span class="sxs-lookup"><span data-stu-id="2646b-316">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="2646b-317">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="2646b-317">app_offline.htm</span></span>

<span data-ttu-id="2646b-318">Se un file denominato *app_offline.htm* viene rilevato nella directory radice di un'app, il modulo ASP.NET Core tenta di arrestare normalmente l'app e arresta l'elaborazione delle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="2646b-318">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="2646b-319">Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo ASP.NET Core termina il processo in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2646b-319">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="2646b-320">Mentre il file *app_offline.htm* è presente, il modulo ASP.NET Core risponde alle richieste restituendo il contenuto del file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2646b-320">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="2646b-321">Quando il file *app_offline.htm* viene rimosso, la richiesta successiva avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="2646b-321">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2646b-322">Quando si usa il modello di hosting out-of-process, l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="2646b-322">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="2646b-323">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="2646b-323">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="2646b-324">Pagina di errore di avvio</span><span class="sxs-lookup"><span data-stu-id="2646b-324">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2646b-325">Sia l'hosting In-Process che quello out-of-process producono pagine di errore personalizzate quando non riescono ad avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="2646b-325">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="2646b-326">Se il modulo ASP.NET Core non riesce a trovare il gestore richieste In-Process o out-of-process, viene visualizzata una tabella codici di stato *500.0 - Errore di caricamento del gestore In-Process/out-of-process*.</span><span class="sxs-lookup"><span data-stu-id="2646b-326">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="2646b-327">Per l'hosting In-Process, se il modulo ASP.NET Core non riesce ad avviare l'app, viene visualizzata una tabella codici di stato *500.30 - Errore di avvio*.</span><span class="sxs-lookup"><span data-stu-id="2646b-327">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="2646b-328">Per l'hosting out-of-process, se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="2646b-328">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="2646b-329">Per non visualizzare questa pagina e tornare alla tabella codici di stato 5xx predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="2646b-329">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="2646b-330">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="2646b-330">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2646b-331">Se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="2646b-331">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="2646b-332">Per non visualizzare questa pagina e tornare alla tabella codici di stato 502 predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="2646b-332">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="2646b-333">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="2646b-333">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Tabella codici di stato 502.5 Errore del processo](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="2646b-335">Creazione e reindirizzamento dei log</span><span class="sxs-lookup"><span data-stu-id="2646b-335">Log creation and redirection</span></span>

<span data-ttu-id="2646b-336">Il modulo ASP.NET Core reindirizza su disco l'output della console stdout e stderr se sono impostati gli attributi `stdoutLogEnabled` e `stdoutLogFile` dell'elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="2646b-336">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="2646b-337">Le eventuali cartelle nel percorso `stdoutLogFile` devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="2646b-337">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2646b-338">Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).</span><span class="sxs-lookup"><span data-stu-id="2646b-338">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="2646b-339">I log non vengono ruotati, a meno che non si verifichi il riciclo/riavvio del processo.</span><span class="sxs-lookup"><span data-stu-id="2646b-339">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="2646b-340">È responsabilità del provider di servizi di hosting limitare lo spazio su disco usato dai log.</span><span class="sxs-lookup"><span data-stu-id="2646b-340">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="2646b-341">L'uso del log stdout è consigliato solo per la risoluzione dei problemi di avvio delle app.</span><span class="sxs-lookup"><span data-stu-id="2646b-341">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="2646b-342">Non usare il log stdout per scopi di registrazione generale delle app.</span><span class="sxs-lookup"><span data-stu-id="2646b-342">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="2646b-343">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="2646b-343">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="2646b-344">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="2646b-344">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="2646b-345">Un timestamp e l'estensione del file vengono aggiunti automaticamente al momento della creazione del file di log.</span><span class="sxs-lookup"><span data-stu-id="2646b-345">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="2646b-346">Il nome del file di log è composto aggiungendo il timestamp, l'ID processo e l'estensione del file (*.log*) all'ultimo segmento del percorso `stdoutLogFile` (in genere *stdout*), con caratteri di sottolineatura come delimitatori.</span><span class="sxs-lookup"><span data-stu-id="2646b-346">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="2646b-347">Se il percorso `stdoutLogFile` termina con *stdout*, un log per un'app con un PID 1934 creata il 5/2/2018 alle 19:42:32 sarà denominato *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="2646b-347">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2646b-348">Se `stdoutLogEnabled` è false, gli errori che si verificano all'avvio dell'app vengono acquisiti ed emessi nel log eventi fino a 30 KB.</span><span class="sxs-lookup"><span data-stu-id="2646b-348">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="2646b-349">Dopo l'avvio, tutti i log aggiuntivi vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="2646b-349">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="2646b-350">L'elemento di esempio `aspNetCore` seguente configura la registrazione stdout per un'app ospitata in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="2646b-350">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="2646b-351">Un percorso locale o un percorso di una condivisione di rete è accettabile per la registrazione locale.</span><span class="sxs-lookup"><span data-stu-id="2646b-351">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="2646b-352">Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="2646b-352">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="2646b-353">Log di diagnostica avanzati</span><span class="sxs-lookup"><span data-stu-id="2646b-353">Enhanced diagnostic logs</span></span>

<span data-ttu-id="2646b-354">Il modulo ASP.NET Core può essere configurato per restituire log di diagnostica avanzata.</span><span class="sxs-lookup"><span data-stu-id="2646b-354">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="2646b-355">Aggiungere l'elemento `<handlerSettings>` all'elemento `<aspNetCore>` in *web.config*. L'impostazione di `debugLevel` su `TRACE` restituisce una maggior fedeltà dei dati diagnostici:</span><span class="sxs-lookup"><span data-stu-id="2646b-355">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="2646b-356">I valori di livello debug (`debugLevel`) possono includere sia il livello che il percorso.</span><span class="sxs-lookup"><span data-stu-id="2646b-356">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="2646b-357">Livelli (in ordine dal meno dettagliato al più dettagliato):</span><span class="sxs-lookup"><span data-stu-id="2646b-357">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="2646b-358">ERROR</span><span class="sxs-lookup"><span data-stu-id="2646b-358">ERROR</span></span>
* <span data-ttu-id="2646b-359">WARNING</span><span class="sxs-lookup"><span data-stu-id="2646b-359">WARNING</span></span>
* <span data-ttu-id="2646b-360">INFO</span><span class="sxs-lookup"><span data-stu-id="2646b-360">INFO</span></span>
* <span data-ttu-id="2646b-361">TRACE</span><span class="sxs-lookup"><span data-stu-id="2646b-361">TRACE</span></span>

<span data-ttu-id="2646b-362">Posizioni (sono consentite più posizioni):</span><span class="sxs-lookup"><span data-stu-id="2646b-362">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="2646b-363">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="2646b-363">CONSOLE</span></span>
* <span data-ttu-id="2646b-364">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="2646b-364">EVENTLOG</span></span>
* <span data-ttu-id="2646b-365">FILE</span><span class="sxs-lookup"><span data-stu-id="2646b-365">FILE</span></span>

<span data-ttu-id="2646b-366">Le impostazioni del gestore possono essere specificate anche tramite le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="2646b-366">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="2646b-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Percorso del file di logo di debug.</span><span class="sxs-lookup"><span data-stu-id="2646b-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="2646b-368">(Impostazione predefinita: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="2646b-368">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="2646b-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Impostazione del livello di debug.</span><span class="sxs-lookup"><span data-stu-id="2646b-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="2646b-370">**Non** lasciare la registrazione del debug abilitata nella distribuzione per un tempo superiore a quello necessario alla risoluzione di problema.</span><span class="sxs-lookup"><span data-stu-id="2646b-370">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="2646b-371">Le dimensioni del log non sono limitate.</span><span class="sxs-lookup"><span data-stu-id="2646b-371">The size of the log isn't limited.</span></span> <span data-ttu-id="2646b-372">Se si lascia abilitato il log di debug, lo spazio disponibile su disco può esaurirsi e il server o il servizio app può registrare un arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="2646b-372">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="2646b-373">Vedere [Configurazione con web.config](#configuration-with-webconfig) per un esempio dell'elemento `aspNetCore` nel file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="2646b-373">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="2646b-374">La configurazione del proxy usa il protocollo HTTP e un token di associazione</span><span class="sxs-lookup"><span data-stu-id="2646b-374">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2646b-375">*Si applica solo all'hosting out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="2646b-375">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="2646b-376">Il proxy creato tra il modulo ASP.NET Core e Kestrel usa il protocollo HTTP.</span><span class="sxs-lookup"><span data-stu-id="2646b-376">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="2646b-377">L'uso di HTTP è un'ottimizzazione delle prestazioni in cui il traffico tra il modulo e Kestrel avviene in un indirizzo di loopback dell'interfaccia di rete.</span><span class="sxs-lookup"><span data-stu-id="2646b-377">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="2646b-378">Non vi è alcun rischio di intercettazione del traffico tra il modulo e Kestrel da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="2646b-378">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="2646b-379">Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine.</span><span class="sxs-lookup"><span data-stu-id="2646b-379">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="2646b-380">Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo.</span><span class="sxs-lookup"><span data-stu-id="2646b-380">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="2646b-381">Il token di associazione viene impostato anche in un'intestazione (`MSAspNetCoreToken`) in tutte le richieste trasmesse tramite proxy.</span><span class="sxs-lookup"><span data-stu-id="2646b-381">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="2646b-382">Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="2646b-382">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="2646b-383">Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata.</span><span class="sxs-lookup"><span data-stu-id="2646b-383">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="2646b-384">La variabile di ambiente del token di associazione e il traffico tra il modulo e Kestrel non sono accessibili da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="2646b-384">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="2646b-385">Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.</span><span class="sxs-lookup"><span data-stu-id="2646b-385">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="2646b-386">Modulo ASP.NET Core con configurazione condivisa di IIS</span><span class="sxs-lookup"><span data-stu-id="2646b-386">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="2646b-387">Il programma di installazione del modulo ASP.NET Core viene eseguito con i privilegi dell'account **SYSTEM**.</span><span class="sxs-lookup"><span data-stu-id="2646b-387">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="2646b-388">Poiché l'account di sistema locale non dispone dell'autorizzazione di modifica per il percorso della condivisione usato dalla configurazione condivisa di IIS, il programma di installazione rileva un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo in *applicationHost.config* nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="2646b-388">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="2646b-389">Quando si usa una configurazione condivisa di IIS, attenersi ai passaggi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="2646b-389">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="2646b-390">Disabilitare la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="2646b-390">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="2646b-391">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="2646b-391">Run the installer.</span></span>
1. <span data-ttu-id="2646b-392">Esportare il file *applicationHost.config* aggiornato nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="2646b-392">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="2646b-393">Abilitare di nuovo la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="2646b-393">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="2646b-394">Versione del modulo e log del programma di installazione del bundle di hosting</span><span class="sxs-lookup"><span data-stu-id="2646b-394">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="2646b-395">Per determinare la versione del modulo ASP.NET Core installato:</span><span class="sxs-lookup"><span data-stu-id="2646b-395">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="2646b-396">Nel sistema host passare a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="2646b-396">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="2646b-397">Individuare il file *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="2646b-397">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="2646b-398">Fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="2646b-398">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="2646b-399">Selezionare la scheda **Dettagli**. **Versione file** e **Versione prodotto** rappresentano la versione installata del modulo.</span><span class="sxs-lookup"><span data-stu-id="2646b-399">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="2646b-400">I log del programma di installazione del bundle di hosting per il modulo sono disponibili in *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Il file è denominato *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="2646b-400">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="2646b-401">Percorsi dei file di modulo, schema e configurazione</span><span class="sxs-lookup"><span data-stu-id="2646b-401">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="2646b-402">Modulo</span><span class="sxs-lookup"><span data-stu-id="2646b-402">Module</span></span>

<span data-ttu-id="2646b-403">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="2646b-403">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="2646b-404">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2646b-404">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="2646b-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2646b-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2646b-406">%Programmi%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2646b-406">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="2646b-407">%Programmi(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2646b-407">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="2646b-408">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="2646b-408">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="2646b-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2646b-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="2646b-410">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2646b-410">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2646b-411">%Programmi%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2646b-411">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="2646b-412">%Programmi(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2646b-412">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="2646b-413">Schema</span><span class="sxs-lookup"><span data-stu-id="2646b-413">Schema</span></span>

<span data-ttu-id="2646b-414">**IIS**</span><span class="sxs-lookup"><span data-stu-id="2646b-414">**IIS**</span></span>

   * <span data-ttu-id="2646b-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="2646b-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2646b-416">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="2646b-416">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="2646b-417">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2646b-417">**IIS Express**</span></span>

   * <span data-ttu-id="2646b-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="2646b-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2646b-419">%Programmi%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="2646b-419">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="2646b-420">Configurazione</span><span class="sxs-lookup"><span data-stu-id="2646b-420">Configuration</span></span>

<span data-ttu-id="2646b-421">**IIS**</span><span class="sxs-lookup"><span data-stu-id="2646b-421">**IIS**</span></span>

   * <span data-ttu-id="2646b-422">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="2646b-422">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="2646b-423">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2646b-423">**IIS Express**</span></span>

   * <span data-ttu-id="2646b-424">%Programmi%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="2646b-424">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="2646b-425">È possibile trovare i file eseguendo una ricerca di *aspnetcore* nel file *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="2646b-425">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>
