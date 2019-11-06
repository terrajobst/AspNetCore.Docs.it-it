---
title: Usare più ambienti in ASP.NET Core
author: rick-anderson
description: Informazioni su come controllare il comportamento di app ASP.NET Core in più ambienti.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: fundamentals/environments
ms.openlocfilehash: 91fa2a78e62dff65704a3dda826f45f27bad6064
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634097"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="d10f9-103">Usare più ambienti in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d10f9-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="d10f9-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d10f9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d10f9-105">ASP.NET Core configura il comportamento dell'app in base all'ambiente di runtime e tramite una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="d10f9-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d10f9-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="d10f9-107">Ambienti</span><span class="sxs-lookup"><span data-stu-id="d10f9-107">Environments</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d10f9-108">ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'app e archivia il valore in [IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="d10f9-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="d10f9-109">`ASPNETCORE_ENVIRONMENT` può essere impostato su qualsiasi valore, ma vengono forniti tre valori dal Framework:</span><span class="sxs-lookup"><span data-stu-id="d10f9-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="d10f9-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="d10f9-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d10f9-111">ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'app e ne archivia il valore in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="d10f9-111">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="d10f9-112">`ASPNETCORE_ENVIRONMENT` può essere impostato su qualsiasi valore, ma vengono forniti tre valori dal Framework:</span><span class="sxs-lookup"><span data-stu-id="d10f9-112">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="d10f9-113"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="d10f9-113"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

::: moniker-end

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="d10f9-114">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="d10f9-114">The preceding code:</span></span>

* <span data-ttu-id="d10f9-115">Chiama [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) quando `ASPNETCORE_ENVIRONMENT` è impostato su `Development`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-115">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="d10f9-116">Chiama [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quando il valore di `ASPNETCORE_ENVIRONMENT` è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="d10f9-116">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="d10f9-117">L'[helper per tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere il markup nell'elemento:</span><span class="sxs-lookup"><span data-stu-id="d10f9-117">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="d10f9-118">In Windows e macOS le variabili di ambiente e i relativi valori non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="d10f9-118">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="d10f9-119">In Linux le variabili di ambiente e i relativi valori **applicano la distinzione tra maiuscole e minuscole** per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="d10f9-119">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="d10f9-120">Sviluppo</span><span class="sxs-lookup"><span data-stu-id="d10f9-120">Development</span></span>

<span data-ttu-id="d10f9-121">L'ambiente di sviluppo può abilitare funzionalità che non devono essere esposte nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="d10f9-121">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="d10f9-122">Ad esempio i modelli ASP.NET Core abilitano la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling#developer-exception-page) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d10f9-122">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="d10f9-123">L'ambiente di sviluppo computer locale può essere impostato nel file *Properties\launchSettings.json* del progetto.</span><span class="sxs-lookup"><span data-stu-id="d10f9-123">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="d10f9-124">I valori di ambiente in *launchSettings.json* sostituiscono i valori impostati nell'ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="d10f9-124">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="d10f9-125">Il codice JSON seguente visualizza tre profili di un file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d10f9-125">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="d10f9-126">La proprietà `applicationUrl` in *launchSettings.json* può specificare un elenco di URL di server.</span><span class="sxs-lookup"><span data-stu-id="d10f9-126">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="d10f9-127">Usare un punto e virgola tra gli URL dell'elenco:</span><span class="sxs-lookup"><span data-stu-id="d10f9-127">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="d10f9-128">Quando l'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run) viene usato il primo profilo con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-128">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="d10f9-129">Il valore di `commandName` specifica il server Web da avviare.</span><span class="sxs-lookup"><span data-stu-id="d10f9-129">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="d10f9-130">`commandName` può avere uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="d10f9-130">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="d10f9-131">`Project` (che avvia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="d10f9-131">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="d10f9-132">Quando un'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="d10f9-132">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="d10f9-133">*launchSettings.json*, se disponibile, viene letto.</span><span class="sxs-lookup"><span data-stu-id="d10f9-133">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="d10f9-134">Le impostazioni `environmentVariables` in *launchSettings.json* sostituiscono le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-134">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="d10f9-135">Viene visualizzato l'ambiente host.</span><span class="sxs-lookup"><span data-stu-id="d10f9-135">The hosting environment is displayed.</span></span>

<span data-ttu-id="d10f9-136">L'output seguente visualizza un'app avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="d10f9-136">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="d10f9-137">La scheda **Debug** nelle proprietà di progetto di Visual Studio visualizza una GUI per la modifica del file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d10f9-137">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proprietà progetto - Impostazione delle variabili di ambiente](environments/_static/project-properties-debug.png)

<span data-ttu-id="d10f9-139">È possibile che le modifiche apportate ai profili di progetto abbiano effetto solo dopo il riavvio del server Web.</span><span class="sxs-lookup"><span data-stu-id="d10f9-139">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="d10f9-140">Perché Kestrel rilevi le modifiche apportate al suo ambiente, deve essere riavviato.</span><span class="sxs-lookup"><span data-stu-id="d10f9-140">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="d10f9-141">Evitare di archiviare segreti in *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d10f9-141">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="d10f9-142">Per l'archiviazione di segreti per lo sviluppo locale, usare lo [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d10f9-142">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="d10f9-143">Quando si usa [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="d10f9-143">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="d10f9-144">Nell'esempio seguente l'ambiente viene impostato su `Development`:</span><span class="sxs-lookup"><span data-stu-id="d10f9-144">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="d10f9-145">Un file *.vscode/launch.json* del progetto non viene letto quando si avvia l'app con `dotnet run` come accade per il file *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d10f9-145">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="d10f9-146">Quando si avvia un'app in un ambiente di sviluppo che non ha un file *launchSettings.json*, impostare una variabile di ambiente o un argomento della riga di comando sul comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-146">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="d10f9-147">Produzione</span><span class="sxs-lookup"><span data-stu-id="d10f9-147">Production</span></span>

<span data-ttu-id="d10f9-148">È necessario che l'ambiente di produzione sia configurato per ottimizzare la sicurezza, le prestazioni e l'affidabilità dell'app.</span><span class="sxs-lookup"><span data-stu-id="d10f9-148">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="d10f9-149">Alcune impostazioni comuni che differiscono dallo sviluppo includono:</span><span class="sxs-lookup"><span data-stu-id="d10f9-149">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="d10f9-150">Memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="d10f9-150">Caching.</span></span>
* <span data-ttu-id="d10f9-151">Risorse lato client in bundle, minimizzate e potenzialmente offerte da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="d10f9-151">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="d10f9-152">Pagine di errore di diagnostica disabilitate.</span><span class="sxs-lookup"><span data-stu-id="d10f9-152">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="d10f9-153">Pagine di errore descrittive abilitate.</span><span class="sxs-lookup"><span data-stu-id="d10f9-153">Friendly error pages enabled.</span></span>
* <span data-ttu-id="d10f9-154">Registrazione e monitoraggio della produzione abilitati.</span><span class="sxs-lookup"><span data-stu-id="d10f9-154">Production logging and monitoring enabled.</span></span> <span data-ttu-id="d10f9-155">Ad esempio [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="d10f9-155">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="d10f9-156">Impostare l'ambiente</span><span class="sxs-lookup"><span data-stu-id="d10f9-156">Set the environment</span></span>

<span data-ttu-id="d10f9-157">Spesso è utile impostare un ambiente specifico per il test con una variabile di ambiente o un'impostazione della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="d10f9-157">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="d10f9-158">Se l'ambiente non viene impostato, il valore predefinito è `Production`, che disabilita la maggior parte delle funzionalità di debug.</span><span class="sxs-lookup"><span data-stu-id="d10f9-158">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="d10f9-159">Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="d10f9-159">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="d10f9-160">Quando viene compilato l'host, l'ultima impostazione dell'ambiente letta dall'app determina l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="d10f9-160">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="d10f9-161">L'ambiente dell'app non può essere modificato mentre l'app è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d10f9-161">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="d10f9-162">Impostazione della piattaforma o della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="d10f9-162">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="d10f9-163">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="d10f9-163">Azure App Service</span></span>

<span data-ttu-id="d10f9-164">Per impostare l'ambiente in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="d10f9-164">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="d10f9-165">Selezionare l'app dal pannello **Servizi app**.</span><span class="sxs-lookup"><span data-stu-id="d10f9-165">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="d10f9-166">Nel gruppo **IMPOSTAZIONI** selezionare il pannello **Impostazioni dell'applicazione**.</span><span class="sxs-lookup"><span data-stu-id="d10f9-166">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="d10f9-167">Nell'area **Impostazioni dell'applicazione** selezionare **Aggiungi nuova impostazione**.</span><span class="sxs-lookup"><span data-stu-id="d10f9-167">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="d10f9-168">In **Immettere un nome** specificare `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-168">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="d10f9-169">In **Immettere un valore** specificare l'ambiente (ad esempio, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="d10f9-169">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="d10f9-170">Selezionare la casella di controllo **Impostazione slot** per lasciare invariata l'impostazione dell'ambiente sullo slot corrente quando gli slot di distribuzione vengono scambiati.</span><span class="sxs-lookup"><span data-stu-id="d10f9-170">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="d10f9-171">Per altre informazioni, vedere [Documentazione di Azure: Impostazioni incluse nello scambio](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="d10f9-171">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="d10f9-172">Selezionare **Salva** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="d10f9-172">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="d10f9-173">Servizio app di Azure riavvia automaticamente l'app quando un'impostazione dell'app (una variabile di ambiente) viene aggiunta, modificata o eliminata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="d10f9-173">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="d10f9-174">WINDOWS</span><span class="sxs-lookup"><span data-stu-id="d10f9-174">Windows</span></span>

<span data-ttu-id="d10f9-175">Per impostare la variabile `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite [dotnet run](/dotnet/core/tools/dotnet-run), vengono usati i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="d10f9-175">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="d10f9-176">**Prompt dei comandi**</span><span class="sxs-lookup"><span data-stu-id="d10f9-176">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="d10f9-177">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="d10f9-177">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="d10f9-178">Questi comandi hanno effetto solo per la finestra corrente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-178">These commands only take effect for the current window.</span></span> <span data-ttu-id="d10f9-179">Quando la finestra viene chiusa, l'impostazione `ASPNETCORE_ENVIRONMENT` viene ripristinata sull'impostazione predefinita o sul valore del computer.</span><span class="sxs-lookup"><span data-stu-id="d10f9-179">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="d10f9-180">Per impostare il valore a livello globale in Windows, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="d10f9-180">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="d10f9-181">Scegliere **Pannello di controllo** > **Sistema** > **Impostazioni di sistema avanzate** e aggiungere o modificare il valore `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="d10f9-181">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Proprietà di sistema avanzate](environments/_static/systemsetting_environment.png)

  ![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="d10f9-184">Aprire un prompt dei comandi di amministrazione e usare il comando `setx` o aprire un prompt dei comandi di PowerShell di amministrazione e usare `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="d10f9-184">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="d10f9-185">**Prompt dei comandi**</span><span class="sxs-lookup"><span data-stu-id="d10f9-185">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="d10f9-186">L'opzione `/M` indica di impostare la variabile di ambiente a livello del sistema.</span><span class="sxs-lookup"><span data-stu-id="d10f9-186">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="d10f9-187">Se non viene usata l'opzione `/M`, la variabile di ambiente viene impostata per l'account utente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-187">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="d10f9-188">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="d10f9-188">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="d10f9-189">Il valore dell'opzione `Machine` indica di impostare la variabile di ambiente a livello del sistema.</span><span class="sxs-lookup"><span data-stu-id="d10f9-189">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="d10f9-190">Se non viene usata l'opzione `User`, la variabile di ambiente viene impostata per l'account utente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-190">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="d10f9-191">Quando la variabile di ambiente `ASPNETCORE_ENVIRONMENT` è impostata a livello globale, viene applicata per `dotnet run` in tutte le finestre di comando aperte dopo l'impostazione del valore.</span><span class="sxs-lookup"><span data-stu-id="d10f9-191">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="d10f9-192">**web.config**</span><span class="sxs-lookup"><span data-stu-id="d10f9-192">**web.config**</span></span>

<span data-ttu-id="d10f9-193">Per impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` con *web.config*, vedere la sezione *Impostazione delle variabili di ambiente* di <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="d10f9-193">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d10f9-194">**File di progetto o profilo di pubblicazione**</span><span class="sxs-lookup"><span data-stu-id="d10f9-194">**Project file or publish profile**</span></span>

<span data-ttu-id="d10f9-195">**Per le distribuzioni di Windows IIS:** Includere la proprietà `<EnvironmentName>` nel profilo di pubblicazione (con*estensione pubxml*) o nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="d10f9-195">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="d10f9-196">Questo approccio imposta l'ambiente in *web.config* quando viene pubblicato il progetto:</span><span class="sxs-lookup"><span data-stu-id="d10f9-196">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

::: moniker-end

<span data-ttu-id="d10f9-197">**Pool di applicazioni IIS singoli**</span><span class="sxs-lookup"><span data-stu-id="d10f9-197">**Per IIS Application Pool**</span></span>

<span data-ttu-id="d10f9-198">Per impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` per un'app in esecuzione in un pool di applicazioni isolato (supportato in IIS 10.0 o versioni successive), vedere la sezione *AppCmd.exe* dell'argomento [Variabili di ambiente &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="d10f9-198">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="d10f9-199">Quando la variabile di ambiente `ASPNETCORE_ENVIRONMENT` viene impostata per un pool di app, il suo valore esegue l'override di un'impostazione a livello di sistema.</span><span class="sxs-lookup"><span data-stu-id="d10f9-199">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d10f9-200">Durante l'hosting di un'app in IIS, quando si aggiunge o si modifica la variabile di ambiente `ASPNETCORE_ENVIRONMENT`, usare uno degli approcci seguenti per fare in modo che il nuovo valore venga selezionato dalle app:</span><span class="sxs-lookup"><span data-stu-id="d10f9-200">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="d10f9-201">Eseguire `net stop was /y` seguito da `net start w3svc` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="d10f9-201">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="d10f9-202">Riavviare il server.</span><span class="sxs-lookup"><span data-stu-id="d10f9-202">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="d10f9-203">macOS</span><span class="sxs-lookup"><span data-stu-id="d10f9-203">macOS</span></span>

<span data-ttu-id="d10f9-204">L'impostazione dell'ambiente corrente per macOS può essere eseguita in linea durante l'esecuzione dell'app:</span><span class="sxs-lookup"><span data-stu-id="d10f9-204">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="d10f9-205">In alternativa, impostare l'ambiente tramite `export` prima di eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="d10f9-205">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="d10f9-206">Le variabili di ambiente a livello computer sono impostate nel file con estensione *bashrc* o *bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="d10f9-206">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="d10f9-207">Modificare il file usando un qualsiasi editor di testo.</span><span class="sxs-lookup"><span data-stu-id="d10f9-207">Edit the file using any text editor.</span></span> <span data-ttu-id="d10f9-208">Aggiungere l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="d10f9-208">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="d10f9-209">Linux</span><span class="sxs-lookup"><span data-stu-id="d10f9-209">Linux</span></span>

<span data-ttu-id="d10f9-210">Per le distribuzioni di Linux, usare il comando `export` al prompt dei comandi per le impostazioni delle variabili basate sulla sessione e il file *bash_profile* per le impostazioni di ambiente a livello di computer.</span><span class="sxs-lookup"><span data-stu-id="d10f9-210">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="d10f9-211">Impostazione dell'ambiente nel codice</span><span class="sxs-lookup"><span data-stu-id="d10f9-211">Set the environment in code</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d10f9-212">Chiamare <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> durante la compilazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="d10f9-212">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="d10f9-213">Vedere <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="d10f9-213">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d10f9-214">Chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> durante la compilazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="d10f9-214">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="d10f9-215">Vedere <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="d10f9-215">See <xref:fundamentals/host/web-host#environment>.</span></span>

::: moniker-end

### <a name="configuration-by-environment"></a><span data-ttu-id="d10f9-216">Configurazione per ambiente</span><span class="sxs-lookup"><span data-stu-id="d10f9-216">Configuration by environment</span></span>

<span data-ttu-id="d10f9-217">Per caricare la configurazione dall'ambiente, sono consigliabili:</span><span class="sxs-lookup"><span data-stu-id="d10f9-217">To load configuration by environment, we recommend:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="d10f9-218">file *appSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="d10f9-218">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="d10f9-219">Vedere <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="d10f9-219">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="d10f9-220">Variabili di ambiente (impostate in ogni sistema in cui è ospitata l'app).</span><span class="sxs-lookup"><span data-stu-id="d10f9-220">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="d10f9-221">Vedere <xref:fundamentals/host/generic-host#environmentname> e <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="d10f9-221">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="d10f9-222">Secret Manager (solo nell'ambiente di sviluppo).</span><span class="sxs-lookup"><span data-stu-id="d10f9-222">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="d10f9-223">Vedere <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="d10f9-223">See <xref:security/app-secrets>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="d10f9-224">file *appSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="d10f9-224">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="d10f9-225">Vedere <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="d10f9-225">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="d10f9-226">Variabili di ambiente (impostate in ogni sistema in cui è ospitata l'app).</span><span class="sxs-lookup"><span data-stu-id="d10f9-226">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="d10f9-227">Vedere <xref:fundamentals/host/web-host#environment> e <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="d10f9-227">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="d10f9-228">Secret Manager (solo nell'ambiente di sviluppo).</span><span class="sxs-lookup"><span data-stu-id="d10f9-228">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="d10f9-229">Vedere <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="d10f9-229">See <xref:security/app-secrets>.</span></span>

::: moniker-end

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="d10f9-230">Classe Startup e metodi basati sull'ambiente</span><span class="sxs-lookup"><span data-stu-id="d10f9-230">Environment-based Startup class and methods</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="d10f9-231">Inserire IWebHostEnvironment in startup. Configure</span><span class="sxs-lookup"><span data-stu-id="d10f9-231">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="d10f9-232">Inserire <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-232">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="d10f9-233">Questo approccio è utile quando l'app richiede solo la regolazione `Startup.Configure` per alcuni ambienti con differenze minime di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-233">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="d10f9-234">Inserire IWebHostEnvironment nella classe Startup</span><span class="sxs-lookup"><span data-stu-id="d10f9-234">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="d10f9-235">Inserire <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> nel costruttore di `Startup`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-235">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="d10f9-236">Questo approccio è utile quando l'app richiede la configurazione di `Startup` solo per alcuni ambienti con differenze minime di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-236">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="d10f9-237">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="d10f9-237">In the following example:</span></span>

* <span data-ttu-id="d10f9-238">L'ambiente viene mantenuto nel campo `_env`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-238">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="d10f9-239">`_env` viene usato in `ConfigureServices` e `Configure` per applicare la configurazione di avvio in base all'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="d10f9-239">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IWebHostEnvironment _env;

    public Startup(IWebHostEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="d10f9-240">Inserire IHostingEnvironment in startup. Configure</span><span class="sxs-lookup"><span data-stu-id="d10f9-240">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="d10f9-241">Inserire <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-241">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="d10f9-242">Questo approccio è utile quando l'app richiede solo la configurazione di `Startup.Configure` solo per alcuni ambienti con differenze minime di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-242">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="d10f9-243">Inserire IHostingEnvironment nella classe Startup</span><span class="sxs-lookup"><span data-stu-id="d10f9-243">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="d10f9-244">Inserire <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> nel costruttore di `Startup` e assegnare il servizio a un campo da utilizzare in tutta la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-244">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="d10f9-245">Questo approccio è utile quando l'app richiede la configurazione dell'avvio per solo alcuni ambienti con differenze minime di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-245">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="d10f9-246">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="d10f9-246">In the following example:</span></span>

* <span data-ttu-id="d10f9-247">L'ambiente viene mantenuto nel campo `_env`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-247">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="d10f9-248">`_env` viene usato in `ConfigureServices` e `Configure` per applicare la configurazione di avvio in base all'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="d10f9-248">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

::: moniker-end

### <a name="startup-class-conventions"></a><span data-ttu-id="d10f9-249">Convenzioni delle classi di avvio</span><span class="sxs-lookup"><span data-stu-id="d10f9-249">Startup class conventions</span></span>

<span data-ttu-id="d10f9-250">Quando viene avviata un'app ASP.NET Core, la [classe Startup](xref:fundamentals/startup) avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="d10f9-250">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="d10f9-251">L'app può definire classi di `Startup` separate per ambienti diversi, ad esempio `StartupDevelopment`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-251">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="d10f9-252">La classe `Startup` appropriata viene selezionata in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d10f9-252">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="d10f9-253">La classe il cui suffisso di nome corrisponde all'ambiente corrente ha la priorità.</span><span class="sxs-lookup"><span data-stu-id="d10f9-253">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="d10f9-254">Se non viene trovata una classe `Startup{EnvironmentName}` corrispondente, viene usata la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-254">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="d10f9-255">Questo approccio è utile quando l'app richiede la configurazione dell'avvio per diversi ambienti con molte differenze di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-255">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="d10f9-256">Per implementare le classi `Startup` basate sull'ambiente, creare una classe `Startup{EnvironmentName}` per ogni ambiente in uso e una classe `Startup` di fallback:</span><span class="sxs-lookup"><span data-stu-id="d10f9-256">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="d10f9-257">Usare l'overload [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) che accetta un nome di assembly:</span><span class="sxs-lookup"><span data-stu-id="d10f9-257">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

### <a name="startup-method-conventions"></a><span data-ttu-id="d10f9-258">Convenzioni dei metodi di avvio</span><span class="sxs-lookup"><span data-stu-id="d10f9-258">Startup method conventions</span></span>

<span data-ttu-id="d10f9-259">[Configurare](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) supportare versioni specifiche dell'ambiente del modulo `Configure<EnvironmentName>` e `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="d10f9-259">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="d10f9-260">Questo approccio è utile quando l'app richiede la configurazione dell'avvio per diversi ambienti con molte differenze di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="d10f9-260">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="d10f9-261">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d10f9-261">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
