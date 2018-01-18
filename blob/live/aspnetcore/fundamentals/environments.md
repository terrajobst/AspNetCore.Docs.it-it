---
title: "Utilizzo di più ambienti in ASP.NET Core"
author: rick-anderson
description: Scopri come ASP.NET Core fornisce supporto per il controllo del comportamento dell'app in ambienti diversi.
keywords: ASP.NET di base, le impostazioni di ambiente, ASPNETCORE_ENVIRONMENT
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="36ab5-104">Utilizzo di più ambienti</span><span class="sxs-lookup"><span data-stu-id="36ab5-104">Working with multiple environments</span></span>

<span data-ttu-id="36ab5-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="36ab5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="36ab5-106">ASP.NET Core fornisce supporto per l'impostazione di comportamento dell'applicazione in fase di esecuzione con le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="36ab5-106">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="36ab5-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="36ab5-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="36ab5-108">Ambienti</span><span class="sxs-lookup"><span data-stu-id="36ab5-108">Environments</span></span>

<span data-ttu-id="36ab5-109">ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'applicazione e archivia tale valore in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="36ab5-109">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="36ab5-110">`ASPNETCORE_ENVIRONMENT`Impostare su qualsiasi valore, ma [tre valori](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) sono supportate dal framework: [sviluppo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [gestione temporanea](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), e [produzione](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="36ab5-110">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="36ab5-111">Se `ASPNETCORE_ENVIRONMENT` non è impostata, verrà aperta `Production`.</span><span class="sxs-lookup"><span data-stu-id="36ab5-111">If `ASPNETCORE_ENVIRONMENT` is not set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="36ab5-112">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="36ab5-112">The preceding code:</span></span>

* <span data-ttu-id="36ab5-113">Chiamate [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` è impostato su `Development`.</span><span class="sxs-lookup"><span data-stu-id="36ab5-113">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="36ab5-114">Chiamate [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando il valore di `ASPNETCORE_ENVIRONMENT` è impostato uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="36ab5-114">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="36ab5-115">Il [Helper di Tag di ambiente ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilizza il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere markup nell'elemento:</span><span class="sxs-lookup"><span data-stu-id="36ab5-115">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="36ab5-116">Nota: In Windows e Mac OS, valori e variabili di ambiente non sono tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="36ab5-116">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="36ab5-117">Le variabili di ambiente Linux e i valori sono **tra maiuscole e minuscole** per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="36ab5-117">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="36ab5-118">Sviluppo</span><span class="sxs-lookup"><span data-stu-id="36ab5-118">Development</span></span>

<span data-ttu-id="36ab5-119">L'ambiente di sviluppo è possibile abilitare le funzionalità che non devono essere esposti nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="36ab5-119">The development environment can enable features that should not be exposed in production.</span></span> <span data-ttu-id="36ab5-120">Ad esempio, i modelli ASP.NET Core abilitano il [pagina eccezione developer](xref:fundamentals/error-handling#the-developer-exception-page) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="36ab5-120">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="36ab5-121">L'ambiente per lo sviluppo di computer locale può essere impostata nella *Properties\launchSettings.json* file del progetto.</span><span class="sxs-lookup"><span data-stu-id="36ab5-121">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="36ab5-122">Impostare i valori di ambiente in *launchSettings.json* sovrascrivono i valori impostati nell'ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="36ab5-122">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="36ab5-123">Il codice XML seguente mostra tre profili da un *launchSettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="36ab5-123">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="36ab5-124">Quando viene avviata l'applicazione con `dotnet run`, il primo profilo con `"commandName": "Project"` verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="36ab5-124">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="36ab5-125">Il valore di `commandName` specifica il server web da avviare.</span><span class="sxs-lookup"><span data-stu-id="36ab5-125">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="36ab5-126">`commandName`può essere uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="36ab5-126">`commandName` can be one of :</span></span>

* <span data-ttu-id="36ab5-127">IIS Express</span><span class="sxs-lookup"><span data-stu-id="36ab5-127">IIS Express</span></span>
* <span data-ttu-id="36ab5-128">IIS</span><span class="sxs-lookup"><span data-stu-id="36ab5-128">IIS</span></span>
* <span data-ttu-id="36ab5-129">Progetto (che avvia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="36ab5-129">Project (which launches Kestrel)</span></span>

<span data-ttu-id="36ab5-130">Quando viene avviata un'app con `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="36ab5-130">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="36ab5-131">*launchSettings.json* è di lettura, se disponibile.</span><span class="sxs-lookup"><span data-stu-id="36ab5-131">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="36ab5-132">`environmentVariables`le impostazioni in *launchSettings.json* eseguire l'override delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="36ab5-132">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="36ab5-133">Viene visualizzato l'ambiente di hosting.</span><span class="sxs-lookup"><span data-stu-id="36ab5-133">The hosting environment is displayed.</span></span>


<span data-ttu-id="36ab5-134">L'output seguente mostra un'app introduttiva `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="36ab5-134">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="36ab5-135">Visual Studio **Debug** scheda fornisce un'interfaccia utente grafica per modificare il *launchSettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="36ab5-135">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variabili di ambiente di impostazione delle proprietà del progetto](environments/_static/project-properties-debug.png)

<span data-ttu-id="36ab5-137">Le modifiche apportate ai profili di progetto non abbiano effetto, è necessario riavviare il server web.</span><span class="sxs-lookup"><span data-stu-id="36ab5-137">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="36ab5-138">È necessario riavviare kestrel rileverà le modifiche apportate al relativo ambiente.</span><span class="sxs-lookup"><span data-stu-id="36ab5-138">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="36ab5-139">*launchSettings.json* non devono essere archiviate informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="36ab5-139">*launchSettings.json* should not store secrets.</span></span> <span data-ttu-id="36ab5-140">Il [lo strumento Gestione segreto](xref:security/app-secrets) può essere utilizzato per archiviare i segreti per lo sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="36ab5-140">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="36ab5-141">Produzione</span><span class="sxs-lookup"><span data-stu-id="36ab5-141">Production</span></span>

<span data-ttu-id="36ab5-142">L'ambiente di produzione deve essere configurato per ottimizzare la sicurezza, prestazioni e affidabilità dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36ab5-142">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="36ab5-143">Alcune impostazioni comuni che potrebbe avere un ambiente di produzione che verrebbero differisce dallo sviluppo includono:</span><span class="sxs-lookup"><span data-stu-id="36ab5-143">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="36ab5-144">La memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="36ab5-144">Caching.</span></span>
* <span data-ttu-id="36ab5-145">Le risorse sul lato client sono inclusi, minimizzare e potenzialmente servite da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="36ab5-145">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="36ab5-146">Pagine di errore diagnostico disabilitate.</span><span class="sxs-lookup"><span data-stu-id="36ab5-146">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="36ab5-147">Pagine di errore descrittivo abilitate.</span><span class="sxs-lookup"><span data-stu-id="36ab5-147">Friendly error pages enabled.</span></span>
* <span data-ttu-id="36ab5-148">Registrazione di produzione e di monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="36ab5-148">Production logging and monitoring enabled.</span></span> <span data-ttu-id="36ab5-149">Ad esempio, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="36ab5-149">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="36ab5-150">Impostazione dell'ambiente</span><span class="sxs-lookup"><span data-stu-id="36ab5-150">Setting the environment</span></span>

<span data-ttu-id="36ab5-151">Spesso è utile impostare un ambiente di test specifico.</span><span class="sxs-lookup"><span data-stu-id="36ab5-151">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="36ab5-152">Se l'ambiente non è impostata, verrà aperta `Production` che disabilita la maggior parte delle funzionalità di debug.</span><span class="sxs-lookup"><span data-stu-id="36ab5-152">If the environment is not set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="36ab5-153">Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="36ab5-153">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="36ab5-154">Azure</span><span class="sxs-lookup"><span data-stu-id="36ab5-154">Azure</span></span>

<span data-ttu-id="36ab5-155">Per il servizio app di Azure:</span><span class="sxs-lookup"><span data-stu-id="36ab5-155">For Azure app service:</span></span>

* <span data-ttu-id="36ab5-156">Selezionare il **le impostazioni dell'applicazione** blade.</span><span class="sxs-lookup"><span data-stu-id="36ab5-156">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="36ab5-157">Aggiungere la chiave e il valore **impostazioni App**.</span><span class="sxs-lookup"><span data-stu-id="36ab5-157">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="36ab5-158">WINDOWS</span><span class="sxs-lookup"><span data-stu-id="36ab5-158">Windows</span></span>
<span data-ttu-id="36ab5-159">Per impostare il `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite `dotnet run`, vengono utilizzati i seguenti comandi</span><span class="sxs-lookup"><span data-stu-id="36ab5-159">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="36ab5-160">**Riga di comando**</span><span class="sxs-lookup"><span data-stu-id="36ab5-160">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="36ab5-161">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="36ab5-161">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="36ab5-162">Questi comandi diventano effettive solo per la finestra corrente.</span><span class="sxs-lookup"><span data-stu-id="36ab5-162">These commands take effect only for the current window.</span></span> <span data-ttu-id="36ab5-163">Quando la finestra viene chiusa, l'impostazione ASPNETCORE_ENVIRONMENT verrà ripristinata l'impostazione predefinita o un valore di computer.</span><span class="sxs-lookup"><span data-stu-id="36ab5-163">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="36ab5-164">Per impostare il valore globale all'apertura di Windows il **Pannello di controllo** > **sistema** > **impostazioni di sistema avanzate** e aggiungere o modificare il `ASPNETCORE_ENVIRONMENT` valore.</span><span class="sxs-lookup"><span data-stu-id="36ab5-164">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Sistema di proprietà avanzate](environments/_static/systemsetting_environment.png)

![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="36ab5-167">**web.config**</span><span class="sxs-lookup"><span data-stu-id="36ab5-167">**web.config**</span></span>

<span data-ttu-id="36ab5-168">Vedere il *impostare variabili di ambiente* sezione la [riferimento di configurazione di ASP.NET Core modulo](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) argomento.</span><span class="sxs-lookup"><span data-stu-id="36ab5-168">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="36ab5-169">**Per ogni Pool di applicazioni IIS**</span><span class="sxs-lookup"><span data-stu-id="36ab5-169">**Per IIS Application Pool**</span></span>

<span data-ttu-id="36ab5-170">Per impostare le variabili di ambiente per le singole applicazioni in esecuzione nel pool di applicazioni isolate (supportata in IIS 10.0 +), vedere il *comando AppCmd.exe* sezione la [le variabili di ambiente \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) argomento.</span><span class="sxs-lookup"><span data-stu-id="36ab5-170">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="36ab5-171">macOS</span><span class="sxs-lookup"><span data-stu-id="36ab5-171">macOS</span></span>
<span data-ttu-id="36ab5-172">L'impostazione l'ambiente corrente per macOS può essere effettuata in linea, durante l'esecuzione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36ab5-172">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="36ab5-173">o tramite `export` impostarlo prima di eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="36ab5-173">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="36ab5-174">Le variabili di ambiente di livello del computer sono impostate *.bashrc* o *.bash_profile* file.</span><span class="sxs-lookup"><span data-stu-id="36ab5-174">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="36ab5-175">Modificare il file utilizzando un editor di testo e aggiungere l'istruzione seguente.</span><span class="sxs-lookup"><span data-stu-id="36ab5-175">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="36ab5-176">Linux</span><span class="sxs-lookup"><span data-stu-id="36ab5-176">Linux</span></span>
<span data-ttu-id="36ab5-177">Per le distribuzioni di Linux, usare il `export` comando dalla riga di comando per sessione in base a impostazioni delle variabili e *bash_profile* file per le impostazioni di ambiente di livello computer.</span><span class="sxs-lookup"><span data-stu-id="36ab5-177">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="36ab5-178">Configurazione di ambiente</span><span class="sxs-lookup"><span data-stu-id="36ab5-178">Configuration by environment</span></span>

<span data-ttu-id="36ab5-179">Vedere [configurazione dall'ambiente](xref:fundamentals/configuration/index#configuration-by-environment) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="36ab5-179">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="36ab5-180">Ambiente basato su metodi e classe di avvio</span><span class="sxs-lookup"><span data-stu-id="36ab5-180">Environment based Startup class and methods</span></span>

<span data-ttu-id="36ab5-181">Quando viene avviata un'applicazione ASP.NET di base, il [classe Startup](xref:fundamentals/startup) avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="36ab5-181">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="36ab5-182">Se una classe `Startup{EnvironmentName}` esista, che verrà chiamata per tale classe `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="36ab5-182">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="36ab5-183">Nota: La chiamata [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) esegue l'override delle sezioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="36ab5-183">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="36ab5-184">[Configurare](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) supporta versioni specifiche dell'ambiente del form `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="36ab5-184">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="36ab5-185">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="36ab5-185">Additional Resources</span></span>

* [<span data-ttu-id="36ab5-186">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="36ab5-186">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="36ab5-187">Configurazione</span><span class="sxs-lookup"><span data-stu-id="36ab5-187">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="36ab5-188">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="36ab5-188">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
