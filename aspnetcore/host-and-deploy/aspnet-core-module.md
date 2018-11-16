---
title: Guida di riferimento per la configurazione del modulo ASP.NET Core
author: guardrex
description: Informazioni su come configurare il modulo di ASP.NET Core per l'hosting di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ca86b1548c7c28a64fd391617b2e8290c1c264cf
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191360"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="6aee5-103">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6aee5-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="6aee5-104">Di [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="6aee5-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="6aee5-105">Questo documento fornisce istruzioni su come configurare il modulo ASP.NET Core per l'hosting di app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6aee5-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="6aee5-106">Per un'introduzione al modulo ASP.NET Core e istruzioni per l'installazione, vedere la [panoramica del modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="6aee5-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="6aee5-107">Modello di hosting</span><span class="sxs-lookup"><span data-stu-id="6aee5-107">Hosting model</span></span>

<span data-ttu-id="6aee5-108">Per le app che vengono eseguite in .NET Core 2.2 o versione successiva, il modulo supporta un modello di hosting in-process, che garantisce prestazioni migliori rispetto al tipo di hosting a proxy inverso (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="6aee5-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="6aee5-109">Per ulteriori informazioni, vedere <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="6aee5-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="6aee5-110">L'hosting in-process acconsente esplicitamente le app esistenti, mentre i modelli [dotnet new](/dotnet/core/tools/dotnet-new) usano per impostazione predefinita il modello di hosting in-process per tutti gli scenari IIS e IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6aee5-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="6aee5-111">Per configurare un'app per l'hosting In-Process, aggiungere la proprietà `<AspNetCoreHostingModel>` al file di progetto dell'app (ad esempio, *MyApp.csproj*), usando il valore `inprocess` (l'hosting out-of-process viene impostato con `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="6aee5-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6aee5-112">In caso di hosting in-process, vengono applicate le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="6aee5-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="6aee5-113">Non viene usato il [server Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="6aee5-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="6aee5-114">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.Server.IServer> personalizzata, `IISHttpServer` funge da server dell'app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="6aee5-115">L'[attributo requestTimeout ](#attributes-of-the-aspnetcore-element) non viene applicato all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6aee5-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="6aee5-116">Non è supportata la condivisione di un pool di app tra le app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="6aee5-117">Usare un pool di app per ogni app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-117">Use one app pool per app.</span></span>

* <span data-ttu-id="6aee5-118">Quando si usa la [Distribuzione Web ](/iis/publish/using-web-deploy/introduction-to-web-deploy) o si inserisce manualmente un [file app_offline.htm nella distribuzione](xref:host-and-deploy/iis/index#locked-deployment-files), l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="6aee5-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6aee5-119">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="6aee5-120">L'architettura, vale a dire il numero di bit dell'app, e il runtime installato (x64 o x86) devono corrispondere all'architettura del pool di app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="6aee5-121">Se si configura l'host dell'app manualmente con `WebHostBuilder` (non con [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) e l'app viene sempre eseguita direttamente nel server di Kestrel (self-hosted), chiamare `UseKestrel` prima di chiamare `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="6aee5-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="6aee5-122">Se l'ordine viene invertito, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="6aee5-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="6aee5-123">Le disconnessioni del client vengono rilevate.</span><span class="sxs-lookup"><span data-stu-id="6aee5-123">Client disconnects are detected.</span></span> <span data-ttu-id="6aee5-124">Il token di annullamento [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) viene cancellato quando il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="6aee5-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="6aee5-125">`Directory.GetCurrentDirectory()` restituisce la directory di lavoro del processo avviato da IIS invece che la directory dell'applicazione (ad esempio, *C:\Windows\System32\inetsrv* per *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="6aee5-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="6aee5-126">Modifiche del modello di hosting</span><span class="sxs-lookup"><span data-stu-id="6aee5-126">Hosting model changes</span></span>

<span data-ttu-id="6aee5-127">Se l'impostazione `hostingModel` viene modificata nel file *web.config* (descritto nella sezione [Configurazione con web.config](#configuration-with-webconfig)), il modulo ricicla il processo di lavoro per IIS.</span><span class="sxs-lookup"><span data-stu-id="6aee5-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="6aee5-128">Per IIS Express, il modulo non ricicla il processo di lavoro. Attiva invece un arresto normale del processo di IIS Express corrente.</span><span class="sxs-lookup"><span data-stu-id="6aee5-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="6aee5-129">La richiesta successiva all'app attiva un nuovo processo di IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6aee5-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="6aee5-130">Nome processo</span><span class="sxs-lookup"><span data-stu-id="6aee5-130">Process name</span></span>

<span data-ttu-id="6aee5-131">`Process.GetCurrentProcess().ProcessName` dichiara `w3wp`/`iisexpress` (In-Process) o `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="6aee5-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="6aee5-132">Configurazione con web.config</span><span class="sxs-lookup"><span data-stu-id="6aee5-132">Configuration with web.config</span></span>

<span data-ttu-id="6aee5-133">Il modulo ASP.NET Core viene configurato tramite la sezione `aspNetCore` del nodo `system.webServer` nel file *web.config* del sito.</span><span class="sxs-lookup"><span data-stu-id="6aee5-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="6aee5-134">Il file *web.config* seguente viene pubblicato per una [distribuzione dipendente dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo ASP.NET Core per gestire le richieste del sito:</span><span class="sxs-lookup"><span data-stu-id="6aee5-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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
                  hostingModel="inprocess" />
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

<span data-ttu-id="6aee5-135">Il file *web.config* seguente viene pubblicato per una [distribuzione autonoma](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="6aee5-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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
                  hostingModel="inprocess" />
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
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="6aee5-136">Quando un'app viene distribuita in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), il percorso `stdoutLogFile` è impostato su `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6aee5-136">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6aee5-137">Il percorso salva i log stdout nella cartella *LogFiles*, ovvero una posizione creata automaticamente dal servizio.</span><span class="sxs-lookup"><span data-stu-id="6aee5-137">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="6aee5-138">Vedere [Configurazione delle applicazioni secondarie](xref:host-and-deploy/iis/index#sub-application-configuration) per una nota importante relativa alla configurazione di *web.config* file nelle app secondarie.</span><span class="sxs-lookup"><span data-stu-id="6aee5-138">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="6aee5-139">Attributi dell'elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="6aee5-139">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="6aee5-140">Attributo</span><span class="sxs-lookup"><span data-stu-id="6aee5-140">Attribute</span></span> | <span data-ttu-id="6aee5-141">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6aee5-141">Description</span></span> | <span data-ttu-id="6aee5-142">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6aee5-142">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6aee5-143">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-143">Optional string attribute.</span></span></p><p><span data-ttu-id="6aee5-144">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-144">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6aee5-145">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-145">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6aee5-146">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="6aee5-146">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6aee5-147">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6aee5-148">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6aee5-148">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6aee5-149">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6aee5-149">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="6aee5-150">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-150">Optional string attribute.</span></span></p><p><span data-ttu-id="6aee5-151">Specifica il modello di hosting in-process (`inprocess`) o out-of-process (`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="6aee5-151">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="6aee5-152">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-152">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-153">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-153">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6aee5-154">&dagger;Per l'hosting in-process, il valore è limitato a `1`.</span><span class="sxs-lookup"><span data-stu-id="6aee5-154">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="6aee5-155">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="6aee5-155">Default: `1`</span></span><br><span data-ttu-id="6aee5-156">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="6aee5-156">Min: `1`</span></span><br><span data-ttu-id="6aee5-157">Max: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="6aee5-157">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="6aee5-158">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="6aee5-158">Required string attribute.</span></span></p><p><span data-ttu-id="6aee5-159">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6aee5-159">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6aee5-160">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="6aee5-160">Relative paths are supported.</span></span> <span data-ttu-id="6aee5-161">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6aee5-161">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6aee5-162">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-162">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-163">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-163">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6aee5-164">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="6aee5-164">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="6aee5-165">Non supportato con l'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6aee5-165">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="6aee5-166">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6aee5-166">Default: `10`</span></span><br><span data-ttu-id="6aee5-167">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6aee5-167">Min: `0`</span></span><br><span data-ttu-id="6aee5-168">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="6aee5-168">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6aee5-169">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-169">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6aee5-170">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6aee5-170">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6aee5-171">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="6aee5-171">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="6aee5-172">Non è applicabile all'hosting in-process.</span><span class="sxs-lookup"><span data-stu-id="6aee5-172">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="6aee5-173">Per l'hosting in-process, il modulo resta in attesa che l'app elabori la richiesta.</span><span class="sxs-lookup"><span data-stu-id="6aee5-173">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="6aee5-174">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6aee5-174">Default: `00:02:00`</span></span><br><span data-ttu-id="6aee5-175">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6aee5-175">Min: `00:00:00`</span></span><br><span data-ttu-id="6aee5-176">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6aee5-176">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6aee5-177">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-177">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-178">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-178">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6aee5-179">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6aee5-179">Default: `10`</span></span><br><span data-ttu-id="6aee5-180">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6aee5-180">Min: `0`</span></span><br><span data-ttu-id="6aee5-181">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="6aee5-181">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6aee5-182">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-183">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="6aee5-183">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6aee5-184">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-184">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6aee5-185">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-185">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6aee5-186">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="6aee5-186">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6aee5-187">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="6aee5-187">Default: `120`</span></span><br><span data-ttu-id="6aee5-188">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6aee5-188">Min: `0`</span></span><br><span data-ttu-id="6aee5-189">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="6aee5-189">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6aee5-190">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6aee5-191">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-191">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6aee5-192">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-192">Optional string attribute.</span></span></p><p><span data-ttu-id="6aee5-193">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-193">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6aee5-194">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6aee5-194">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6aee5-195">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="6aee5-195">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6aee5-196">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="6aee5-196">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="6aee5-197">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-197">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6aee5-198">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-198">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="6aee5-199">Attributo</span><span class="sxs-lookup"><span data-stu-id="6aee5-199">Attribute</span></span> | <span data-ttu-id="6aee5-200">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6aee5-200">Description</span></span> | <span data-ttu-id="6aee5-201">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6aee5-201">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6aee5-202">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-202">Optional string attribute.</span></span></p><p><span data-ttu-id="6aee5-203">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-203">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6aee5-204">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-204">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6aee5-205">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="6aee5-205">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6aee5-206">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-206">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6aee5-207">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6aee5-207">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6aee5-208">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6aee5-208">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="6aee5-209">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-210">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-210">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="6aee5-211">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="6aee5-211">Default: `1`</span></span><br><span data-ttu-id="6aee5-212">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="6aee5-212">Min: `1`</span></span><br><span data-ttu-id="6aee5-213">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="6aee5-213">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="6aee5-214">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="6aee5-214">Required string attribute.</span></span></p><p><span data-ttu-id="6aee5-215">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6aee5-215">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6aee5-216">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="6aee5-216">Relative paths are supported.</span></span> <span data-ttu-id="6aee5-217">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6aee5-217">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6aee5-218">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-218">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-219">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-219">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6aee5-220">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="6aee5-220">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="6aee5-221">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6aee5-221">Default: `10`</span></span><br><span data-ttu-id="6aee5-222">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6aee5-222">Min: `0`</span></span><br><span data-ttu-id="6aee5-223">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="6aee5-223">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6aee5-224">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-224">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6aee5-225">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6aee5-225">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6aee5-226">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</span><span class="sxs-lookup"><span data-stu-id="6aee5-226">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="6aee5-227">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6aee5-227">Default: `00:02:00`</span></span><br><span data-ttu-id="6aee5-228">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6aee5-228">Min: `00:00:00`</span></span><br><span data-ttu-id="6aee5-229">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6aee5-229">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6aee5-230">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-230">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-231">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-231">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6aee5-232">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6aee5-232">Default: `10`</span></span><br><span data-ttu-id="6aee5-233">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6aee5-233">Min: `0`</span></span><br><span data-ttu-id="6aee5-234">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="6aee5-234">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6aee5-235">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-235">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-236">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="6aee5-236">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6aee5-237">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-237">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6aee5-238">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-238">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6aee5-239">Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</span><span class="sxs-lookup"><span data-stu-id="6aee5-239">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6aee5-240">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="6aee5-240">Default: `120`</span></span><br><span data-ttu-id="6aee5-241">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6aee5-241">Min: `0`</span></span><br><span data-ttu-id="6aee5-242">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="6aee5-242">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6aee5-243">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-243">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6aee5-244">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-244">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6aee5-245">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-245">Optional string attribute.</span></span></p><p><span data-ttu-id="6aee5-246">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-246">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6aee5-247">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6aee5-247">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6aee5-248">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="6aee5-248">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6aee5-249">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="6aee5-249">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="6aee5-250">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-250">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6aee5-251">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-251">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="6aee5-252">Attributo</span><span class="sxs-lookup"><span data-stu-id="6aee5-252">Attribute</span></span> | <span data-ttu-id="6aee5-253">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6aee5-253">Description</span></span> | <span data-ttu-id="6aee5-254">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6aee5-254">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6aee5-255">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-255">Optional string attribute.</span></span></p><p><span data-ttu-id="6aee5-256">Argomenti per l'eseguibile specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-256">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6aee5-257">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-257">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6aee5-258">Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="6aee5-258">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6aee5-259">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-259">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6aee5-260">Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6aee5-260">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6aee5-261">È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6aee5-261">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="6aee5-262">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-262">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-263">Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-263">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="6aee5-264">Valore predefinito: `1`</span><span class="sxs-lookup"><span data-stu-id="6aee5-264">Default: `1`</span></span><br><span data-ttu-id="6aee5-265">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="6aee5-265">Min: `1`</span></span><br><span data-ttu-id="6aee5-266">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="6aee5-266">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="6aee5-267">Attributo stringa obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="6aee5-267">Required string attribute.</span></span></p><p><span data-ttu-id="6aee5-268">Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6aee5-268">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6aee5-269">I percorsi relativi sono supportati.</span><span class="sxs-lookup"><span data-stu-id="6aee5-269">Relative paths are supported.</span></span> <span data-ttu-id="6aee5-270">Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6aee5-270">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6aee5-271">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-272">Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-272">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6aee5-273">Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</span><span class="sxs-lookup"><span data-stu-id="6aee5-273">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="6aee5-274">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6aee5-274">Default: `10`</span></span><br><span data-ttu-id="6aee5-275">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6aee5-275">Min: `0`</span></span><br><span data-ttu-id="6aee5-276">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="6aee5-276">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6aee5-277">Attributo Timespan facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-277">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6aee5-278">Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6aee5-278">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6aee5-279">Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.0 o versioni precedenti, `requestTimeout` deve essere specificato solo in minuti interi. In caso contrario, verrà usato il valore predefinito di 2 minuti.</span><span class="sxs-lookup"><span data-stu-id="6aee5-279">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="6aee5-280">Valore predefinito: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6aee5-280">Default: `00:02:00`</span></span><br><span data-ttu-id="6aee5-281">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6aee5-281">Min: `00:00:00`</span></span><br><span data-ttu-id="6aee5-282">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6aee5-282">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6aee5-283">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-283">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-284">Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-284">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6aee5-285">Valore predefinito: `10`</span><span class="sxs-lookup"><span data-stu-id="6aee5-285">Default: `10`</span></span><br><span data-ttu-id="6aee5-286">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6aee5-286">Min: `0`</span></span><br><span data-ttu-id="6aee5-287">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="6aee5-287">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6aee5-288">Attributo Integer facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-288">Optional integer attribute.</span></span></p><p><span data-ttu-id="6aee5-289">Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="6aee5-289">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6aee5-290">Se questo limite di tempo viene superato, il modulo termina il processo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-290">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6aee5-291">Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-291">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="6aee5-292">Valore predefinito: `120`</span><span class="sxs-lookup"><span data-stu-id="6aee5-292">Default: `120`</span></span><br><span data-ttu-id="6aee5-293">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="6aee5-293">Min: `0`</span></span><br><span data-ttu-id="6aee5-294">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="6aee5-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6aee5-295">Attributo booleano facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6aee5-296">Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6aee5-297">Attributo stringa facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-297">Optional string attribute.</span></span></p><p><span data-ttu-id="6aee5-298">Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6aee5-299">I percorsi relativi sono relativi alla radice del sito.</span><span class="sxs-lookup"><span data-stu-id="6aee5-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6aee5-300">Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti.</span><span class="sxs-lookup"><span data-stu-id="6aee5-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6aee5-301">Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="6aee5-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="6aee5-302">Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6aee5-303">Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="6aee5-304">Impostazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="6aee5-304">Setting environment variables</span></span>

<span data-ttu-id="6aee5-305">È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="6aee5-305">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6aee5-306">Specificare una variabile di ambiente con l'elemento figlio `environmentVariable` di un elemento della raccolta `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="6aee5-306">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="6aee5-307">Le variabili di ambiente impostate in questa sezione hanno la precedenza sulle variabili di ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="6aee5-307">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="6aee5-308">Nell'esempio seguente vengono impostate due variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="6aee5-308">The following example sets two environment variables.</span></span> <span data-ttu-id="6aee5-309">`ASPNETCORE_ENVIRONMENT` configura l'ambiente dell'app su `Development`.</span><span class="sxs-lookup"><span data-stu-id="6aee5-309">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="6aee5-310">Uno sviluppatore può impostare temporaneamente questo valore nel file *web.config* per forzare il caricamento della [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) durante il debug di un'eccezione dell'app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-310">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="6aee5-311">`CONFIG_DIR` è un esempio di variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento del file di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-311">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
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
> <span data-ttu-id="6aee5-312">Impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` su `Development` solo in server di gestione temporanea e test che non sono accessibili da reti non attendibili, ad esempio Internet.</span><span class="sxs-lookup"><span data-stu-id="6aee5-312">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="6aee5-313">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="6aee5-313">app_offline.htm</span></span>

<span data-ttu-id="6aee5-314">Se un file denominato *app_offline.htm* viene rilevato nella directory radice di un'app, il modulo ASP.NET Core tenta di arrestare normalmente l'app e arresta l'elaborazione delle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="6aee5-314">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="6aee5-315">Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo ASP.NET Core termina il processo in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6aee5-315">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="6aee5-316">Mentre il file *app_offline.htm* è presente, il modulo ASP.NET Core risponde alle richieste restituendo il contenuto del file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-316">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="6aee5-317">Quando il file *app_offline.htm* viene rimosso, la richiesta successiva avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-317">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6aee5-318">Quando si usa il modello di hosting out-of-process, l'app potrebbe non arrestarsi immediatamente se una connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="6aee5-318">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6aee5-319">Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-319">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="6aee5-320">Pagina di errore di avvio</span><span class="sxs-lookup"><span data-stu-id="6aee5-320">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6aee5-321">Sia l'hosting In-Process che quello out-of-process producono pagine di errore personalizzate quando non riescono ad avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-321">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="6aee5-322">Se il modulo ASP.NET Core non riesce a trovare il gestore richieste In-Process o out-of-process, viene visualizzata una tabella codici di stato *500.0 - Errore di caricamento del gestore In-Process/out-of-process*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-322">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="6aee5-323">Per l'hosting In-Process, se il modulo ASP.NET Core non riesce ad avviare l'app, viene visualizzata una tabella codici di stato *500.30 - Errore di avvio*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-323">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="6aee5-324">Per l'hosting out-of-process, se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-324">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="6aee5-325">Per non visualizzare questa pagina e tornare alla tabella codici di stato 5xx predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="6aee5-325">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6aee5-326">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6aee5-326">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6aee5-327">Se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-327">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="6aee5-328">Per non visualizzare questa pagina e tornare alla tabella codici di stato 502 predefinita di IIS, usare l'attributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="6aee5-328">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6aee5-329">Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6aee5-329">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Tabella codici di stato 502.5 Errore del processo](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="6aee5-331">Creazione e reindirizzamento dei log</span><span class="sxs-lookup"><span data-stu-id="6aee5-331">Log creation and redirection</span></span>

<span data-ttu-id="6aee5-332">Il modulo ASP.NET Core reindirizza su disco l'output della console stdout e stderr se sono impostati gli attributi `stdoutLogEnabled` e `stdoutLogFile` dell'elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="6aee5-332">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="6aee5-333">Le eventuali cartelle nel percorso `stdoutLogFile` devono essere già esistenti affinché il modulo possa creare il file di log.</span><span class="sxs-lookup"><span data-stu-id="6aee5-333">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="6aee5-334">Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).</span><span class="sxs-lookup"><span data-stu-id="6aee5-334">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6aee5-335">I log non vengono ruotati, a meno che non si verifichi il riciclo/riavvio del processo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-335">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="6aee5-336">È responsabilità del provider di servizi di hosting limitare lo spazio su disco usato dai log.</span><span class="sxs-lookup"><span data-stu-id="6aee5-336">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="6aee5-337">L'uso del log stdout è consigliato solo per la risoluzione dei problemi di avvio delle app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-337">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="6aee5-338">Non usare il log stdout per scopi di registrazione generale delle app.</span><span class="sxs-lookup"><span data-stu-id="6aee5-338">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="6aee5-339">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="6aee5-339">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="6aee5-340">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="6aee5-340">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="6aee5-341">Un timestamp e l'estensione del file vengono aggiunti automaticamente al momento della creazione del file di log.</span><span class="sxs-lookup"><span data-stu-id="6aee5-341">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="6aee5-342">Il nome del file di log è composto aggiungendo il timestamp, l'ID processo e l'estensione del file (*.log*) all'ultimo segmento del percorso `stdoutLogFile` (in genere *stdout*), con caratteri di sottolineatura come delimitatori.</span><span class="sxs-lookup"><span data-stu-id="6aee5-342">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="6aee5-343">Se il percorso `stdoutLogFile` termina con *stdout*, un log per un'app con un PID 1934 creata il 5/2/2018 alle 19:42:32 sarà denominato *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-343">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6aee5-344">Se `stdoutLogEnabled` è false, gli errori che si verificano all'avvio dell'app vengono acquisiti ed emessi nel log eventi fino a 30 KB.</span><span class="sxs-lookup"><span data-stu-id="6aee5-344">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="6aee5-345">Dopo l'avvio, tutti i log aggiuntivi vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="6aee5-345">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="6aee5-346">L'elemento di esempio `aspNetCore` seguente configura la registrazione stdout per un'app ospitata in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="6aee5-346">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="6aee5-347">Un percorso locale o un percorso di una condivisione di rete è accettabile per la registrazione locale.</span><span class="sxs-lookup"><span data-stu-id="6aee5-347">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="6aee5-348">Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="6aee5-348">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="6aee5-349">Log di diagnostica avanzati</span><span class="sxs-lookup"><span data-stu-id="6aee5-349">Enhanced diagnostic logs</span></span>

<span data-ttu-id="6aee5-350">Il modulo ASP.NET Core può essere configurato per restituire log di diagnostica avanzata.</span><span class="sxs-lookup"><span data-stu-id="6aee5-350">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="6aee5-351">Aggiungere l'elemento `<handlerSettings>` all'elemento `<aspNetCore>` in *web.config*. L'impostazione di `debugLevel` su `TRACE` restituisce una maggior fedeltà dei dati diagnostici:</span><span class="sxs-lookup"><span data-stu-id="6aee5-351">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="6aee5-352">I valori di livello debug (`debugLevel`) possono includere sia il livello che il percorso.</span><span class="sxs-lookup"><span data-stu-id="6aee5-352">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="6aee5-353">Livelli (in ordine dal meno dettagliato al più dettagliato):</span><span class="sxs-lookup"><span data-stu-id="6aee5-353">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="6aee5-354">ERROR</span><span class="sxs-lookup"><span data-stu-id="6aee5-354">ERROR</span></span>
* <span data-ttu-id="6aee5-355">WARNING</span><span class="sxs-lookup"><span data-stu-id="6aee5-355">WARNING</span></span>
* <span data-ttu-id="6aee5-356">INFO</span><span class="sxs-lookup"><span data-stu-id="6aee5-356">INFO</span></span>
* <span data-ttu-id="6aee5-357">TRACE</span><span class="sxs-lookup"><span data-stu-id="6aee5-357">TRACE</span></span>

<span data-ttu-id="6aee5-358">Posizioni (sono consentite più posizioni):</span><span class="sxs-lookup"><span data-stu-id="6aee5-358">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="6aee5-359">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="6aee5-359">CONSOLE</span></span>
* <span data-ttu-id="6aee5-360">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="6aee5-360">EVENTLOG</span></span>
* <span data-ttu-id="6aee5-361">FILE</span><span class="sxs-lookup"><span data-stu-id="6aee5-361">FILE</span></span>

<span data-ttu-id="6aee5-362">Le impostazioni del gestore possono essere specificate anche tramite le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="6aee5-362">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="6aee5-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Percorso del file di logo di debug.</span><span class="sxs-lookup"><span data-stu-id="6aee5-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="6aee5-364">(Impostazione predefinita: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="6aee5-364">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="6aee5-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Impostazione del livello di debug.</span><span class="sxs-lookup"><span data-stu-id="6aee5-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="6aee5-366">**Non** lasciare la registrazione del debug abilitata nella distribuzione per un tempo superiore a quello necessario alla risoluzione di problema.</span><span class="sxs-lookup"><span data-stu-id="6aee5-366">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="6aee5-367">Le dimensioni del log non sono limitate.</span><span class="sxs-lookup"><span data-stu-id="6aee5-367">The size of the log isn't limited.</span></span> <span data-ttu-id="6aee5-368">Se si lascia abilitato il log di debug, lo spazio disponibile su disco può esaurirsi e il server o il servizio app può registrare un arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-368">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="6aee5-369">Vedere [Configurazione con web.config](#configuration-with-webconfig) per un esempio dell'elemento `aspNetCore` nel file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-369">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="6aee5-370">La configurazione del proxy usa il protocollo HTTP e un token di associazione</span><span class="sxs-lookup"><span data-stu-id="6aee5-370">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6aee5-371">*Si applica solo all'hosting out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="6aee5-371">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="6aee5-372">Il proxy creato tra il modulo ASP.NET Core e Kestrel usa il protocollo HTTP.</span><span class="sxs-lookup"><span data-stu-id="6aee5-372">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="6aee5-373">L'uso di HTTP è un'ottimizzazione delle prestazioni in cui il traffico tra il modulo e Kestrel avviene in un indirizzo di loopback dell'interfaccia di rete.</span><span class="sxs-lookup"><span data-stu-id="6aee5-373">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="6aee5-374">Non vi è alcun rischio di intercettazione del traffico tra il modulo e Kestrel da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="6aee5-374">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="6aee5-375">Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine.</span><span class="sxs-lookup"><span data-stu-id="6aee5-375">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="6aee5-376">Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-376">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="6aee5-377">Il token di associazione viene impostato anche in un'intestazione (`MSAspNetCoreToken`) in tutte le richieste trasmesse tramite proxy.</span><span class="sxs-lookup"><span data-stu-id="6aee5-377">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="6aee5-378">Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="6aee5-378">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="6aee5-379">Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata.</span><span class="sxs-lookup"><span data-stu-id="6aee5-379">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="6aee5-380">La variabile di ambiente del token di associazione e il traffico tra il modulo e Kestrel non sono accessibili da una posizione esterna al server.</span><span class="sxs-lookup"><span data-stu-id="6aee5-380">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="6aee5-381">Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.</span><span class="sxs-lookup"><span data-stu-id="6aee5-381">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="6aee5-382">Modulo ASP.NET Core con configurazione condivisa di IIS</span><span class="sxs-lookup"><span data-stu-id="6aee5-382">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="6aee5-383">Il programma di installazione del modulo ASP.NET Core viene eseguito con i privilegi dell'account **SYSTEM**.</span><span class="sxs-lookup"><span data-stu-id="6aee5-383">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="6aee5-384">Poiché l'account di sistema locale non dispone dell'autorizzazione di modifica per il percorso della condivisione usato dalla configurazione condivisa di IIS, il programma di installazione rileva un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo in *applicationHost.config* nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="6aee5-384">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="6aee5-385">Quando si usa una configurazione condivisa di IIS, attenersi ai passaggi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="6aee5-385">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="6aee5-386">Disabilitare la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="6aee5-386">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6aee5-387">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="6aee5-387">Run the installer.</span></span>
1. <span data-ttu-id="6aee5-388">Esportare il file *applicationHost.config* aggiornato nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="6aee5-388">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6aee5-389">Abilitare di nuovo la configurazione condivisa di IIS.</span><span class="sxs-lookup"><span data-stu-id="6aee5-389">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="6aee5-390">Versione del modulo e log del programma di installazione del bundle di hosting</span><span class="sxs-lookup"><span data-stu-id="6aee5-390">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="6aee5-391">Per determinare la versione del modulo ASP.NET Core installato:</span><span class="sxs-lookup"><span data-stu-id="6aee5-391">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="6aee5-392">Nel sistema host passare a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-392">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="6aee5-393">Individuare il file *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-393">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="6aee5-394">Fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6aee5-394">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="6aee5-395">Selezionare la scheda **Dettagli**. **Versione file** e **Versione prodotto** rappresentano la versione installata del modulo.</span><span class="sxs-lookup"><span data-stu-id="6aee5-395">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="6aee5-396">I log del programma di installazione del bundle di hosting per il modulo sono disponibili in *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Il file è denominato *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-396">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="6aee5-397">Percorsi dei file di modulo, schema e configurazione</span><span class="sxs-lookup"><span data-stu-id="6aee5-397">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="6aee5-398">Modulo</span><span class="sxs-lookup"><span data-stu-id="6aee5-398">Module</span></span>

<span data-ttu-id="6aee5-399">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6aee5-399">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="6aee5-400">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6aee5-400">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="6aee5-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6aee5-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="6aee5-402">%Programmi%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6aee5-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="6aee5-403">%Programmi(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6aee5-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="6aee5-404">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6aee5-404">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="6aee5-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6aee5-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="6aee5-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6aee5-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="6aee5-407">%Programmi%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6aee5-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="6aee5-408">%Programmi(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6aee5-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="6aee5-409">Schema</span><span class="sxs-lookup"><span data-stu-id="6aee5-409">Schema</span></span>

<span data-ttu-id="6aee5-410">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6aee5-410">**IIS**</span></span>

   * <span data-ttu-id="6aee5-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6aee5-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="6aee5-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6aee5-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="6aee5-413">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6aee5-413">**IIS Express**</span></span>

   * <span data-ttu-id="6aee5-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6aee5-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="6aee5-415">%Programmi%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6aee5-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="6aee5-416">Configurazione</span><span class="sxs-lookup"><span data-stu-id="6aee5-416">Configuration</span></span>

<span data-ttu-id="6aee5-417">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6aee5-417">**IIS**</span></span>

   * <span data-ttu-id="6aee5-418">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6aee5-418">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="6aee5-419">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6aee5-419">**IIS Express**</span></span>

   * <span data-ttu-id="6aee5-420">%Programmi%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6aee5-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="6aee5-421">È possibile trovare i file eseguendo una ricerca di *aspnetcore* nel file *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="6aee5-421">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>