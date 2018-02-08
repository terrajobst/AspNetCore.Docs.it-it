---
title: "Uso di più ambienti in ASP.NET Core"
author: rick-anderson
description: Informazioni sul supporto di ASP.NET Core per il controllo del comportamento delle app in ambienti diversi.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: b40ee9b1c6feae4942f05d22dab776d3cf6c26a0
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="bc8c1-103">Uso di più ambienti</span><span class="sxs-lookup"><span data-stu-id="bc8c1-103">Working with multiple environments</span></span>

<span data-ttu-id="bc8c1-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bc8c1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bc8c1-105">ASP.NET Core offre supporto per l'impostazione del comportamento dell'applicazione in fase di runtime mediante variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="bc8c1-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bc8c1-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="bc8c1-107">Ambienti</span><span class="sxs-lookup"><span data-stu-id="bc8c1-107">Environments</span></span>

<span data-ttu-id="bc8c1-108">ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'applicazione e archivia tale valore in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="bc8c1-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="bc8c1-109">`ASPNETCORE_ENVIRONMENT` può essere impostata su qualsiasi valore, ma il framework supporta [tre valori](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0): [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) e [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="bc8c1-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="bc8c1-110">Se `ASPNETCORE_ENVIRONMENT` non è impostata, assume come impostazione predefinita `Production`.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="bc8c1-111">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-111">The preceding code:</span></span>

* <span data-ttu-id="bc8c1-112">Chiama [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` è impostata su `Development`.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="bc8c1-113">Chiama [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando il valore di `ASPNETCORE_ENVIRONMENT` è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="bc8c1-114">L'[helper tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere il markup nell'elemento:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="bc8c1-115">Nota: in Windows e macOS, le variabili di ambiente e i relativi valori non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="bc8c1-116">In Linux le variabili di ambiente e i relativi valori **applicano la distinzione tra maiuscole e minuscole** per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="bc8c1-117">Sviluppo</span><span class="sxs-lookup"><span data-stu-id="bc8c1-117">Development</span></span>

<span data-ttu-id="bc8c1-118">L'ambiente di sviluppo può abilitare funzionalità che non devono essere esposte nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="bc8c1-119">Ad esempio i modelli ASP.NET Core abilitano la [pagina di eccezione per sviluppatori](xref:fundamentals/error-handling#the-developer-exception-page) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="bc8c1-120">L'ambiente di sviluppo computer locale può essere impostato nel file *Properties\launchSettings.json* del progetto.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="bc8c1-121">I valori di ambiente in *launchSettings.json* sostituiscono i valori impostati nell'ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="bc8c1-122">Il codice JSON seguente visualizza tre profili di un file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="bc8c1-123">Quando l'applicazione viene avviata con `dotnet run` viene usato il primo profilo con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="bc8c1-124">Il valore di `commandName` specifica il server Web da avviare.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="bc8c1-125">`commandName` può essere uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="bc8c1-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="bc8c1-126">IIS Express</span></span>
* <span data-ttu-id="bc8c1-127">IIS</span><span class="sxs-lookup"><span data-stu-id="bc8c1-127">IIS</span></span>
* <span data-ttu-id="bc8c1-128">Project (che avvia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="bc8c1-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="bc8c1-129">Quando un'app viene avviata con `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="bc8c1-130">*launchSettings.json*, se disponibile, viene letto.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="bc8c1-131">Le impostazioni `environmentVariables` in *launchSettings.json* sostituiscono le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="bc8c1-132">Viene visualizzato l'ambiente host.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="bc8c1-133">L'output seguente visualizza un'app avviata con `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="bc8c1-134">La scheda **Debug** di Visual Studio visualizza una GUI per la modifica del file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proprietà progetto - Impostazione delle variabili di ambiente](environments/_static/project-properties-debug.png)

<span data-ttu-id="bc8c1-136">È possibile che le modifiche apportate ai profili di progetto abbiano effetto solo dopo il riavvio del server Web.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="bc8c1-137">Perché Kestrel rilevi le modifiche apportate al suo ambiente, deve essere riavviato.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="bc8c1-138">Evitare di archiviare segreti in *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="bc8c1-139">Per l'archiviazione di segreti per lo sviluppo locale, usare lo [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="bc8c1-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="bc8c1-140">Produzione</span><span class="sxs-lookup"><span data-stu-id="bc8c1-140">Production</span></span>

<span data-ttu-id="bc8c1-141">L'ambiente di produzione deve essere configurato per ottimizzare la sicurezza, le prestazioni e affidabilità dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="bc8c1-142">Alcune impostazioni comuni di un ambiente di produzione che differiscono da quelle di un ambiente di sviluppo sono:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="bc8c1-143">Memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-143">Caching.</span></span>
* <span data-ttu-id="bc8c1-144">Risorse lato client in bundle, minimizzate e potenzialmente offerte da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="bc8c1-145">Pagine di errore di diagnostica disabilitate.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="bc8c1-146">Pagine di errore descrittive abilitate.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="bc8c1-147">Registrazione e monitoraggio della produzione abilitati.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="bc8c1-148">Ad esempio [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="bc8c1-148">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="bc8c1-149">Impostazione dell'ambiente</span><span class="sxs-lookup"><span data-stu-id="bc8c1-149">Setting the environment</span></span>

<span data-ttu-id="bc8c1-150">Spesso risulta utile impostare un ambiente specifico per i test.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="bc8c1-151">Se l'ambiente non è impostato, l'impostazione predefinita è `Production`, che disabilita la maggior parte delle funzionalità di debug.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="bc8c1-152">Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="bc8c1-153">Azure</span><span class="sxs-lookup"><span data-stu-id="bc8c1-153">Azure</span></span>

<span data-ttu-id="bc8c1-154">Per il Servizio app di Azure:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-154">For Azure app service:</span></span>

* <span data-ttu-id="bc8c1-155">Selezionare il pannello **Impostazioni applicazione**.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="bc8c1-156">Aggiungere la chiave e il valore in **Impostazioni App**.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="bc8c1-157">WINDOWS</span><span class="sxs-lookup"><span data-stu-id="bc8c1-157">Windows</span></span>
<span data-ttu-id="bc8c1-158">Per impostare `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite `dotnet run` vengono usati i seguenti comandi:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="bc8c1-159">**Riga di comando**</span><span class="sxs-lookup"><span data-stu-id="bc8c1-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="bc8c1-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="bc8c1-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="bc8c1-161">Questi comandi hanno effetto solo per la finestra corrente.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="bc8c1-162">Quando la finestra viene chiusa l'impostazione ASPNETCORE_ENVIRONMENT torna all'impostazione predefinita o al valore del computer predefinito.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="bc8c1-163">Per impostare il valore a livello globale in Windows aprire il **Pannello di controllo** > **Sistema** > **Impostazioni di sistema avanzate** e aggiungere o modificare il valore `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Proprietà di sistema avanzate](environments/_static/systemsetting_environment.png)

![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="bc8c1-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="bc8c1-166">**web.config**</span></span>

<span data-ttu-id="bc8c1-167">Vedere la sezione *Setting environment variables* (Impostazione di variabili di ambiente) dell'argomento [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) (Guida di riferimento per la configurazione del modulo ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="bc8c1-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="bc8c1-168">**Pool di applicazioni IIS singoli**</span><span class="sxs-lookup"><span data-stu-id="bc8c1-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="bc8c1-169">Per impostare variabili di ambiente per singole app in esecuzione in pool di applicazioni isolati (supportati in IIS 10.0+), vedere la sezione *Comando AppCmd.exe* dell'argomento [Variabili di ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="bc8c1-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="bc8c1-170">macOS</span><span class="sxs-lookup"><span data-stu-id="bc8c1-170">macOS</span></span>
<span data-ttu-id="bc8c1-171">L'impostazione dell'ambiente corrente per macOS può essere effettuata in linea durante l'esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="bc8c1-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="bc8c1-172">oppure usando `export` per impostare l'ambiente prima di eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="bc8c1-173">Le variabili di ambiente di livello computer sono impostate nel file con estensione *bashrc* o *bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="bc8c1-174">Modificare il file con un editor di testo e aggiungere l'istruzione seguente.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-174">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="bc8c1-175">Linux</span><span class="sxs-lookup"><span data-stu-id="bc8c1-175">Linux</span></span>
<span data-ttu-id="bc8c1-176">Per le distribuzioni di Linux, usare il comando `export` della riga di comando per le impostazioni delle variabili basate sulla sessione e il file *bash_profile* per le impostazioni di ambiente di livello computer.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="bc8c1-177">Configurazione per ambiente</span><span class="sxs-lookup"><span data-stu-id="bc8c1-177">Configuration by environment</span></span>

<span data-ttu-id="bc8c1-178">Per altre informazioni, vedere [Configurazione per ambiente](xref:fundamentals/configuration/index#configuration-by-environment).</span><span class="sxs-lookup"><span data-stu-id="bc8c1-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="bc8c1-179">Classe Startup e metodi basati sull'ambiente</span><span class="sxs-lookup"><span data-stu-id="bc8c1-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="bc8c1-180">Quando viene avviata un'app ASP.NET Core, la [classe Startup](xref:fundamentals/startup) avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="bc8c1-181">Se esiste una classe `Startup{EnvironmentName}` tale classe viene chiamata per `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="bc8c1-182">Nota: la chiamata di [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) sostituisce le sezioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="bc8c1-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="bc8c1-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) supportano versioni specifiche per l'ambiente con i formati `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="bc8c1-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="bc8c1-184">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="bc8c1-184">Additional resources</span></span>

* [<span data-ttu-id="bc8c1-185">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="bc8c1-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="bc8c1-186">Configurazione</span><span class="sxs-lookup"><span data-stu-id="bc8c1-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="bc8c1-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="bc8c1-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
