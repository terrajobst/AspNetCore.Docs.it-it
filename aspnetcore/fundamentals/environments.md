---
title: Usare più ambienti in ASP.NET Core
author: rick-anderson
description: Informazioni su come controllare il comportamento di app ASP.NET Core in più ambienti.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 8983a0ce81beb16d68c799d30bfbfce6e7b693b1
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433948"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="c205a-103">Usare più ambienti in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c205a-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="c205a-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c205a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c205a-105">ASP.NET Core configura il comportamento dell'app in base all'ambiente di runtime e tramite una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c205a-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="c205a-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c205a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="c205a-107">Ambienti</span><span class="sxs-lookup"><span data-stu-id="c205a-107">Environments</span></span>

<span data-ttu-id="c205a-108">ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'app e ne archivia il valore in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="c205a-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="c205a-109">La variabile `ASPNETCORE_ENVIRONMENT` può essere impostata su qualsiasi valore, ma il framework supporta [tre valori](/dotnet/api/microsoft.aspnetcore.hosting.environmentname): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) e [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="c205a-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="c205a-110">Se la variabile `ASPNETCORE_ENVIRONMENT` non viene impostata, il valore predefinito è `Production`.</span><span class="sxs-lookup"><span data-stu-id="c205a-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="c205a-111">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="c205a-111">The preceding code:</span></span>

* <span data-ttu-id="c205a-112">Chiama [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) e [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) quando `ASPNETCORE_ENVIRONMENT` è impostata su `Development`.</span><span class="sxs-lookup"><span data-stu-id="c205a-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="c205a-113">Chiama [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quando il valore di `ASPNETCORE_ENVIRONMENT` è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="c205a-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="c205a-114">L'[helper per tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere il markup nell'elemento:</span><span class="sxs-lookup"><span data-stu-id="c205a-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="c205a-115">In Windows e macOS le variabili di ambiente e i relativi valori non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c205a-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="c205a-116">In Linux le variabili di ambiente e i relativi valori **applicano la distinzione tra maiuscole e minuscole** per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c205a-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="c205a-117">Sviluppo</span><span class="sxs-lookup"><span data-stu-id="c205a-117">Development</span></span>

<span data-ttu-id="c205a-118">L'ambiente di sviluppo può abilitare funzionalità che non devono essere esposte nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="c205a-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="c205a-119">Ad esempio i modelli ASP.NET Core abilitano la [pagina di eccezione per sviluppatori](xref:fundamentals/error-handling#the-developer-exception-page) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="c205a-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="c205a-120">L'ambiente di sviluppo computer locale può essere impostato nel file *Properties\launchSettings.json* del progetto.</span><span class="sxs-lookup"><span data-stu-id="c205a-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="c205a-121">I valori di ambiente in *launchSettings.json* sostituiscono i valori impostati nell'ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="c205a-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="c205a-122">Il codice JSON seguente visualizza tre profili di un file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="c205a-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="c205a-123">La proprietà `applicationUrl` in *launchSettings.json* può specificare un elenco di URL di server.</span><span class="sxs-lookup"><span data-stu-id="c205a-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="c205a-124">Usare un punto e virgola tra gli URL dell'elenco:</span><span class="sxs-lookup"><span data-stu-id="c205a-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

<span data-ttu-id="c205a-125">Quando l'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run) viene usato il primo profilo con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="c205a-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="c205a-126">Il valore di `commandName` specifica il server Web da avviare.</span><span class="sxs-lookup"><span data-stu-id="c205a-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="c205a-127">`commandName` può avere uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="c205a-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="c205a-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="c205a-128">IIS Express</span></span>
* <span data-ttu-id="c205a-129">IIS</span><span class="sxs-lookup"><span data-stu-id="c205a-129">IIS</span></span>
* <span data-ttu-id="c205a-130">Project (che avvia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="c205a-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="c205a-131">Quando un'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="c205a-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="c205a-132">*launchSettings.json*, se disponibile, viene letto.</span><span class="sxs-lookup"><span data-stu-id="c205a-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="c205a-133">Le impostazioni `environmentVariables` in *launchSettings.json* sostituiscono le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c205a-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="c205a-134">Viene visualizzato l'ambiente host.</span><span class="sxs-lookup"><span data-stu-id="c205a-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="c205a-135">L'output seguente visualizza un'app avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="c205a-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="c205a-136">La scheda **Debug** nelle proprietà di progetto di Visual Studio visualizza una GUI per la modifica del file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="c205a-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proprietà progetto - Impostazione delle variabili di ambiente](environments/_static/project-properties-debug.png)

<span data-ttu-id="c205a-138">È possibile che le modifiche apportate ai profili di progetto abbiano effetto solo dopo il riavvio del server Web.</span><span class="sxs-lookup"><span data-stu-id="c205a-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="c205a-139">Perché Kestrel rilevi le modifiche apportate al suo ambiente, deve essere riavviato.</span><span class="sxs-lookup"><span data-stu-id="c205a-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="c205a-140">Evitare di archiviare segreti in *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c205a-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="c205a-141">Per l'archiviazione di segreti per lo sviluppo locale, usare lo [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="c205a-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="c205a-142">Quando si usa [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="c205a-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="c205a-143">Nell'esempio seguente l'ambiente viene impostato su `Development`:</span><span class="sxs-lookup"><span data-stu-id="c205a-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="c205a-144">Un file *.vscode/launch.json* del progetto non viene letto quando si avvia l'app con `dotnet run` come accade per il file *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c205a-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="c205a-145">Quando si avvia un'app in un ambiente di sviluppo che non ha un file *launchSettings.json*, impostare una variabile di ambiente o un argomento della riga di comando sul comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c205a-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="c205a-146">Produzione</span><span class="sxs-lookup"><span data-stu-id="c205a-146">Production</span></span>

<span data-ttu-id="c205a-147">È necessario che l'ambiente di produzione sia configurato per ottimizzare la sicurezza, le prestazioni e l'affidabilità dell'app.</span><span class="sxs-lookup"><span data-stu-id="c205a-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="c205a-148">Alcune impostazioni comuni che differiscono dallo sviluppo includono:</span><span class="sxs-lookup"><span data-stu-id="c205a-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="c205a-149">Memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="c205a-149">Caching.</span></span>
* <span data-ttu-id="c205a-150">Risorse lato client in bundle, minimizzate e potenzialmente offerte da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="c205a-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="c205a-151">Pagine di errore di diagnostica disabilitate.</span><span class="sxs-lookup"><span data-stu-id="c205a-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="c205a-152">Pagine di errore descrittive abilitate.</span><span class="sxs-lookup"><span data-stu-id="c205a-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="c205a-153">Registrazione e monitoraggio della produzione abilitati.</span><span class="sxs-lookup"><span data-stu-id="c205a-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="c205a-154">Ad esempio [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="c205a-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="c205a-155">Impostare l'ambiente</span><span class="sxs-lookup"><span data-stu-id="c205a-155">Set the environment</span></span>

<span data-ttu-id="c205a-156">Spesso risulta utile impostare un ambiente specifico per i test.</span><span class="sxs-lookup"><span data-stu-id="c205a-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="c205a-157">Se l'ambiente non viene impostato, il valore predefinito è `Production`, che disabilita la maggior parte delle funzionalità di debug.</span><span class="sxs-lookup"><span data-stu-id="c205a-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="c205a-158">Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="c205a-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="c205a-159">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="c205a-159">Azure App Service</span></span>

<span data-ttu-id="c205a-160">Per impostare l'ambiente in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="c205a-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="c205a-161">Selezionare l'app dal pannello **Servizi app**.</span><span class="sxs-lookup"><span data-stu-id="c205a-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="c205a-162">Nel gruppo **IMPOSTAZIONI** selezionare il pannello **Impostazioni dell'applicazione**.</span><span class="sxs-lookup"><span data-stu-id="c205a-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="c205a-163">Nell'area **Impostazioni dell'applicazione** selezionare **Aggiungi nuova impostazione**.</span><span class="sxs-lookup"><span data-stu-id="c205a-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="c205a-164">In **Immettere un nome** specificare `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="c205a-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="c205a-165">In **Immettere un valore** specificare l'ambiente (ad esempio, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="c205a-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="c205a-166">Selezionare la casella di controllo **Impostazione slot** per lasciare invariata l'impostazione dell'ambiente sullo slot corrente quando gli slot di distribuzione vengono scambiati.</span><span class="sxs-lookup"><span data-stu-id="c205a-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="c205a-167">Per altre informazioni, vedere [Documentazione di Azure: Impostazioni incluse nello scambio](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="c205a-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="c205a-168">Selezionare **Salva** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="c205a-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="c205a-169">Servizio app di Azure riavvia automaticamente l'app quando un'impostazione dell'app (una variabile di ambiente) viene aggiunta, modificata o eliminata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="c205a-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="c205a-170">WINDOWS</span><span class="sxs-lookup"><span data-stu-id="c205a-170">Windows</span></span>

<span data-ttu-id="c205a-171">Per impostare la variabile `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite [dotnet run](/dotnet/core/tools/dotnet-run), vengono usati i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c205a-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="c205a-172">**Prompt dei comandi**</span><span class="sxs-lookup"><span data-stu-id="c205a-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="c205a-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="c205a-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="c205a-174">Questi comandi hanno effetto solo per la finestra corrente.</span><span class="sxs-lookup"><span data-stu-id="c205a-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="c205a-175">Quando la finestra viene chiusa, l'impostazione `ASPNETCORE_ENVIRONMENT` viene ripristinata sull'impostazione predefinita o sul valore del computer.</span><span class="sxs-lookup"><span data-stu-id="c205a-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="c205a-176">Per impostare il valore a livello globale in Windows, aprire il **Pannello di controllo** > **Sistema** > **Impostazioni di sistema avanzate** e aggiungere o modificare il valore `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="c205a-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![Proprietà di sistema avanzate](environments/_static/systemsetting_environment.png)

![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="c205a-179">**web.config**</span><span class="sxs-lookup"><span data-stu-id="c205a-179">**web.config**</span></span>

<span data-ttu-id="c205a-180">Vedere la sezione *Setting environment variables* (Impostazione di variabili di ambiente) dell'argomento [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) (Guida di riferimento per la configurazione del modulo ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="c205a-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="c205a-181">**Pool di applicazioni IIS singoli**</span><span class="sxs-lookup"><span data-stu-id="c205a-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="c205a-182">Per impostare variabili di ambiente per singole app in esecuzione in pool di applicazioni isolati (supportati in IIS 10.0+), vedere la sezione relativa al *comando AppCmd.exe* dell'argomento [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variabile di ambiente environmentVariables).</span><span class="sxs-lookup"><span data-stu-id="c205a-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="c205a-183">macOS</span><span class="sxs-lookup"><span data-stu-id="c205a-183">macOS</span></span>

<span data-ttu-id="c205a-184">L'impostazione dell'ambiente corrente per macOS può essere eseguita in linea durante l'esecuzione dell'app:</span><span class="sxs-lookup"><span data-stu-id="c205a-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="c205a-185">In alternativa, impostare l'ambiente tramite `export` prima di eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="c205a-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="c205a-186">Le variabili di ambiente a livello computer sono impostate nel file con estensione *bashrc* o *bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="c205a-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="c205a-187">Modificare il file usando un qualsiasi editor di testo.</span><span class="sxs-lookup"><span data-stu-id="c205a-187">Edit the file using any text editor.</span></span> <span data-ttu-id="c205a-188">Aggiungere l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="c205a-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="c205a-189">Linux</span><span class="sxs-lookup"><span data-stu-id="c205a-189">Linux</span></span>

<span data-ttu-id="c205a-190">Per le distribuzioni di Linux, usare il comando `export` al prompt dei comandi per le impostazioni delle variabili basate sulla sessione e il file *bash_profile* per le impostazioni di ambiente a livello di computer.</span><span class="sxs-lookup"><span data-stu-id="c205a-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="c205a-191">Configurazione per ambiente</span><span class="sxs-lookup"><span data-stu-id="c205a-191">Configuration by environment</span></span>

<span data-ttu-id="c205a-192">Per altre informazioni, vedere [Configurazione per ambiente](xref:fundamentals/configuration/index#configuration-by-environment).</span><span class="sxs-lookup"><span data-stu-id="c205a-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="c205a-193">Classe Startup e metodi basati sull'ambiente</span><span class="sxs-lookup"><span data-stu-id="c205a-193">Environment-based Startup class and methods</span></span>

<span data-ttu-id="c205a-194">Quando viene avviata un'app ASP.NET Core, la [classe Startup](xref:fundamentals/startup) avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="c205a-194">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="c205a-195">Se esiste una classe `Startup{EnvironmentName}`, tale classe viene chiamata per `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="c205a-195">If a `Startup{EnvironmentName}` class exists, the class is called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="c205a-196">[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) sostituisce le sezioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c205a-196">[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="c205a-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) supportano versioni specifiche per l'ambiente con i formati `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="c205a-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="c205a-198">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c205a-198">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="c205a-199">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="c205a-199">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
