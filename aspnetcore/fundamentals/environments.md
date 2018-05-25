---
title: Usare più ambienti in ASP.NET Core
author: rick-anderson
description: Informazioni sul supporto di ASP.NET Core per il controllo del comportamento delle app in ambienti diversi.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/07/2018
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="d6ea0-103">Usare più ambienti in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6ea0-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="d6ea0-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d6ea0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d6ea0-105">ASP.NET Core offre supporto per l'impostazione del comportamento dell'applicazione in fase di runtime mediante variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="d6ea0-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6ea0-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="d6ea0-107">Ambienti</span><span class="sxs-lookup"><span data-stu-id="d6ea0-107">Environments</span></span>

<span data-ttu-id="d6ea0-108">ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'applicazione e archivia tale valore in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="d6ea0-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="d6ea0-109">`ASPNETCORE_ENVIRONMENT` può essere impostata su qualsiasi valore, ma il framework supporta [tre valori](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) e [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="d6ea0-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="d6ea0-110">Se `ASPNETCORE_ENVIRONMENT` non è impostata, assume come impostazione predefinita `Production`.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="d6ea0-111">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-111">The preceding code:</span></span>

* <span data-ttu-id="d6ea0-112">Chiama [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` è impostata su `Development`.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="d6ea0-113">Chiama [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando il valore di `ASPNETCORE_ENVIRONMENT` è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="d6ea0-114">L'[helper tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere il markup nell'elemento:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="d6ea0-115">Nota: in Windows e macOS, le variabili di ambiente e i relativi valori non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="d6ea0-116">In Linux le variabili di ambiente e i relativi valori **applicano la distinzione tra maiuscole e minuscole** per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="d6ea0-117">Sviluppo</span><span class="sxs-lookup"><span data-stu-id="d6ea0-117">Development</span></span>

<span data-ttu-id="d6ea0-118">L'ambiente di sviluppo può abilitare funzionalità che non devono essere esposte nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="d6ea0-119">Ad esempio i modelli ASP.NET Core abilitano la [pagina di eccezione per sviluppatori](xref:fundamentals/error-handling#the-developer-exception-page) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="d6ea0-120">L'ambiente di sviluppo computer locale può essere impostato nel file *Properties\launchSettings.json* del progetto.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="d6ea0-121">I valori di ambiente in *launchSettings.json* sostituiscono i valori impostati nell'ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="d6ea0-122">Il codice JSON seguente visualizza tre profili di un file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="d6ea0-123">La proprietà `applicationUrl` in *launchSettings.json* può specificare un elenco di URL di server.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="d6ea0-124">Usare un punto e virgola tra gli URL dell'elenco:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="d6ea0-125">Quando l'applicazione viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run) viene usato il primo profilo con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="d6ea0-126">Il valore di `commandName` specifica il server Web da avviare.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="d6ea0-127">`commandName` può essere uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="d6ea0-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="d6ea0-128">IIS Express</span></span>
* <span data-ttu-id="d6ea0-129">IIS</span><span class="sxs-lookup"><span data-stu-id="d6ea0-129">IIS</span></span>
* <span data-ttu-id="d6ea0-130">Project (che avvia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="d6ea0-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="d6ea0-131">Quando un'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="d6ea0-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="d6ea0-132">*launchSettings.json*, se disponibile, viene letto.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="d6ea0-133">Le impostazioni `environmentVariables` in *launchSettings.json* sostituiscono le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="d6ea0-134">Viene visualizzato l'ambiente host.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-134">The hosting environment is displayed.</span></span>


<span data-ttu-id="d6ea0-135">L'output seguente visualizza un'app avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="d6ea0-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="d6ea0-136">La scheda **Debug** di Visual Studio visualizza una GUI per la modifica del file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proprietà progetto - Impostazione delle variabili di ambiente](environments/_static/project-properties-debug.png)

<span data-ttu-id="d6ea0-138">È possibile che le modifiche apportate ai profili di progetto abbiano effetto solo dopo il riavvio del server Web.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="d6ea0-139">Perché Kestrel rilevi le modifiche apportate al suo ambiente, deve essere riavviato.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="d6ea0-140">Evitare di archiviare segreti in *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="d6ea0-141">Per l'archiviazione di segreti per lo sviluppo locale, usare lo [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d6ea0-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="d6ea0-142">Produzione</span><span class="sxs-lookup"><span data-stu-id="d6ea0-142">Production</span></span>

<span data-ttu-id="d6ea0-143">L'ambiente di produzione deve essere configurato per ottimizzare la sicurezza, le prestazioni e affidabilità dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-143">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="d6ea0-144">Alcune impostazioni comuni che differiscono dallo sviluppo includono:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-144">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="d6ea0-145">Memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-145">Caching.</span></span>
* <span data-ttu-id="d6ea0-146">Risorse lato client in bundle, minimizzate e potenzialmente offerte da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-146">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="d6ea0-147">Pagine di errore di diagnostica disabilitate.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-147">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="d6ea0-148">Pagine di errore descrittive abilitate.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-148">Friendly error pages enabled.</span></span>
* <span data-ttu-id="d6ea0-149">Registrazione e monitoraggio della produzione abilitati.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-149">Production logging and monitoring enabled.</span></span> <span data-ttu-id="d6ea0-150">Ad esempio [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="d6ea0-150">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="d6ea0-151">Impostazione dell'ambiente</span><span class="sxs-lookup"><span data-stu-id="d6ea0-151">Setting the environment</span></span>

<span data-ttu-id="d6ea0-152">Spesso risulta utile impostare un ambiente specifico per i test.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-152">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="d6ea0-153">Se l'ambiente non è impostato, l'impostazione predefinita è `Production`, che disabilita la maggior parte delle funzionalità di debug.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-153">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="d6ea0-154">Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-154">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="d6ea0-155">Azure</span><span class="sxs-lookup"><span data-stu-id="d6ea0-155">Azure</span></span>

<span data-ttu-id="d6ea0-156">Per il Servizio app di Azure:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-156">For Azure app service:</span></span>

* <span data-ttu-id="d6ea0-157">Selezionare il pannello **Impostazioni applicazione**.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-157">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="d6ea0-158">Aggiungere la chiave e il valore in **Impostazioni App**.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-158">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="d6ea0-159">WINDOWS</span><span class="sxs-lookup"><span data-stu-id="d6ea0-159">Windows</span></span>
<span data-ttu-id="d6ea0-160">Per impostare `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite [dotnet run](/dotnet/core/tools/dotnet-run) vengono usati i seguenti comandi:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-160">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="d6ea0-161">**Riga di comando**</span><span class="sxs-lookup"><span data-stu-id="d6ea0-161">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="d6ea0-162">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="d6ea0-162">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="d6ea0-163">Questi comandi hanno effetto solo per la finestra corrente.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-163">These commands take effect only for the current window.</span></span> <span data-ttu-id="d6ea0-164">Quando la finestra viene chiusa l'impostazione ASPNETCORE_ENVIRONMENT torna all'impostazione predefinita o al valore del computer predefinito.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-164">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="d6ea0-165">Per impostare il valore a livello globale in Windows aprire il **Pannello di controllo** > **Sistema** > **Impostazioni di sistema avanzate** e aggiungere o modificare il valore `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-165">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Proprietà di sistema avanzate](environments/_static/systemsetting_environment.png)

![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="d6ea0-168">**web.config**</span><span class="sxs-lookup"><span data-stu-id="d6ea0-168">**web.config**</span></span>

<span data-ttu-id="d6ea0-169">Vedere la sezione *Setting environment variables* (Impostazione di variabili di ambiente) dell'argomento [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) (Guida di riferimento per la configurazione del modulo ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="d6ea0-169">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="d6ea0-170">**Pool di applicazioni IIS singoli**</span><span class="sxs-lookup"><span data-stu-id="d6ea0-170">**Per IIS Application Pool**</span></span>

<span data-ttu-id="d6ea0-171">Per impostare variabili di ambiente per singole app in esecuzione in pool di applicazioni isolati (supportati in IIS 10.0+), vedere la sezione *Comando AppCmd.exe* dell'argomento [Variabili di ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="d6ea0-171">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="d6ea0-172">macOS</span><span class="sxs-lookup"><span data-stu-id="d6ea0-172">macOS</span></span>
<span data-ttu-id="d6ea0-173">L'impostazione dell'ambiente corrente per macOS può essere effettuata in linea durante l'esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="d6ea0-173">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="d6ea0-174">oppure usando `export` per impostare l'ambiente prima di eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-174">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="d6ea0-175">Le variabili di ambiente di livello computer sono impostate nel file con estensione *bashrc* o *bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-175">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="d6ea0-176">Modificare il file con un editor di testo e aggiungere l'istruzione seguente.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-176">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="d6ea0-177">Linux</span><span class="sxs-lookup"><span data-stu-id="d6ea0-177">Linux</span></span>
<span data-ttu-id="d6ea0-178">Per le distribuzioni di Linux, usare il comando `export` della riga di comando per le impostazioni delle variabili basate sulla sessione e il file *bash_profile* per le impostazioni di ambiente di livello computer.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-178">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="d6ea0-179">Configurazione per ambiente</span><span class="sxs-lookup"><span data-stu-id="d6ea0-179">Configuration by environment</span></span>

<span data-ttu-id="d6ea0-180">Per altre informazioni, vedere [Configurazione per ambiente](xref:fundamentals/configuration/index#configuration-by-environment).</span><span class="sxs-lookup"><span data-stu-id="d6ea0-180">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="d6ea0-181">Classe Startup e metodi basati sull'ambiente</span><span class="sxs-lookup"><span data-stu-id="d6ea0-181">Environment based Startup class and methods</span></span>

<span data-ttu-id="d6ea0-182">Quando viene avviata un'app ASP.NET Core, la [classe Startup](xref:fundamentals/startup) avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-182">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="d6ea0-183">Se esiste una classe `Startup{EnvironmentName}` tale classe viene chiamata per `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-183">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="d6ea0-184">Nota: la chiamata di [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) sostituisce le sezioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d6ea0-184">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="d6ea0-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) supportano versioni specifiche per l'ambiente con i formati `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="d6ea0-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="d6ea0-186">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d6ea0-186">Additional resources</span></span>

* [<span data-ttu-id="d6ea0-187">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="d6ea0-187">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="d6ea0-188">Configurazione</span><span class="sxs-lookup"><span data-stu-id="d6ea0-188">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="d6ea0-189">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="d6ea0-189">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
