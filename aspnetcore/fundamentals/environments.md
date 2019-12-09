---
title: Usare più ambienti in ASP.NET Core
author: rick-anderson
description: Informazioni su come controllare il comportamento di app ASP.NET Core in più ambienti.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/environments
ms.openlocfilehash: affbb95273c91fe5bf452e0e1ebefa669297304c
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944321"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="f2a9b-103">Usare più ambienti in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2a9b-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f2a9b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f2a9b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f2a9b-105">ASP.NET Core configura il comportamento dell'app in base all'ambiente di runtime e tramite una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="f2a9b-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f2a9b-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="f2a9b-107">Ambienti</span><span class="sxs-lookup"><span data-stu-id="f2a9b-107">Environments</span></span>

<span data-ttu-id="f2a9b-108">ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'app e archivia il valore in [IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="f2a9b-109">`ASPNETCORE_ENVIRONMENT` può essere impostato su qualsiasi valore, ma vengono forniti tre valori dal Framework:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="f2a9b-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="f2a9b-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="f2a9b-111">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-111">The preceding code:</span></span>

* <span data-ttu-id="f2a9b-112">Chiama [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) quando `ASPNETCORE_ENVIRONMENT` è impostato su `Development`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="f2a9b-113">Chiama [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quando il valore di `ASPNETCORE_ENVIRONMENT` è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="f2a9b-114">L'[helper per tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere il markup nell'elemento:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="f2a9b-115">In Windows e macOS le variabili di ambiente e i relativi valori non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="f2a9b-116">In Linux le variabili di ambiente e i relativi valori **applicano la distinzione tra maiuscole e minuscole** per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="f2a9b-117">Sviluppo</span><span class="sxs-lookup"><span data-stu-id="f2a9b-117">Development</span></span>

<span data-ttu-id="f2a9b-118">L'ambiente di sviluppo può abilitare funzionalità che non devono essere esposte nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="f2a9b-119">Ad esempio i modelli ASP.NET Core abilitano la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling#developer-exception-page) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="f2a9b-120">L'ambiente di sviluppo computer locale può essere impostato nel file *Properties\launchSettings.json* del progetto.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="f2a9b-121">I valori di ambiente in *launchSettings.json* sostituiscono i valori impostati nell'ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="f2a9b-122">Il codice JSON seguente visualizza tre profili di un file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="f2a9b-123">La proprietà `applicationUrl` in *launchSettings.json* può specificare un elenco di URL di server.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="f2a9b-124">Usare un punto e virgola tra gli URL dell'elenco:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="f2a9b-125">Quando l'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run) viene usato il primo profilo con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="f2a9b-126">Il valore di `commandName` specifica il server Web da avviare.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="f2a9b-127">`commandName` può avere uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="f2a9b-128">`Project` (che avvia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="f2a9b-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="f2a9b-129">Quando un'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="f2a9b-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="f2a9b-130">*launchSettings.json*, se disponibile, viene letto.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="f2a9b-131">Le impostazioni `environmentVariables` in *launchSettings.json* sostituiscono le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="f2a9b-132">Viene visualizzato l'ambiente host.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="f2a9b-133">L'output seguente visualizza un'app avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="f2a9b-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="f2a9b-134">La scheda **Debug** nelle proprietà di progetto di Visual Studio visualizza una GUI per la modifica del file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proprietà progetto - Impostazione delle variabili di ambiente](environments/_static/project-properties-debug.png)

<span data-ttu-id="f2a9b-136">È possibile che le modifiche apportate ai profili di progetto abbiano effetto solo dopo il riavvio del server Web.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="f2a9b-137">Perché Kestrel rilevi le modifiche apportate al suo ambiente, deve essere riavviato.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="f2a9b-138">Evitare di archiviare segreti in *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="f2a9b-139">Per l'archiviazione di segreti per lo sviluppo locale, usare lo [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="f2a9b-140">Quando si usa [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="f2a9b-141">Nell'esempio seguente l'ambiente viene impostato su `Development`:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="f2a9b-142">Un file *.vscode/launch.json* del progetto non viene letto quando si avvia l'app con `dotnet run` come accade per il file *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="f2a9b-143">Quando si avvia un'app in un ambiente di sviluppo che non ha un file *launchSettings.json*, impostare una variabile di ambiente o un argomento della riga di comando sul comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="f2a9b-144">Produzione</span><span class="sxs-lookup"><span data-stu-id="f2a9b-144">Production</span></span>

<span data-ttu-id="f2a9b-145">È necessario che l'ambiente di produzione sia configurato per ottimizzare la sicurezza, le prestazioni e l'affidabilità dell'app.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="f2a9b-146">Alcune impostazioni comuni che differiscono dallo sviluppo includono:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="f2a9b-147">Memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-147">Caching.</span></span>
* <span data-ttu-id="f2a9b-148">Risorse lato client in bundle, minimizzate e potenzialmente offerte da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="f2a9b-149">Pagine di errore di diagnostica disabilitate.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="f2a9b-150">Pagine di errore descrittive abilitate.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="f2a9b-151">Registrazione e monitoraggio della produzione abilitati.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="f2a9b-152">Ad esempio [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="f2a9b-153">Impostare l'ambiente</span><span class="sxs-lookup"><span data-stu-id="f2a9b-153">Set the environment</span></span>

<span data-ttu-id="f2a9b-154">Spesso è utile impostare un ambiente specifico per il test con una variabile di ambiente o un'impostazione della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="f2a9b-155">Se l'ambiente non viene impostato, il valore predefinito è `Production`, che disabilita la maggior parte delle funzionalità di debug.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="f2a9b-156">Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="f2a9b-157">Quando viene compilato l'host, l'ultima impostazione dell'ambiente letta dall'app determina l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="f2a9b-158">L'ambiente dell'app non può essere modificato mentre l'app è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="f2a9b-159">Impostazione della piattaforma o della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="f2a9b-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="f2a9b-160">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="f2a9b-160">Azure App Service</span></span>

<span data-ttu-id="f2a9b-161">Per impostare l'ambiente in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="f2a9b-162">Selezionare l'app dal pannello **Servizi app**.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="f2a9b-163">Nel gruppo **IMPOSTAZIONI** selezionare il pannello **Impostazioni dell'applicazione**.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-163">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="f2a9b-164">Nell'area **Impostazioni dell'applicazione** selezionare **Aggiungi nuova impostazione**.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-164">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="f2a9b-165">In **Immettere un nome** specificare `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-165">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="f2a9b-166">In **Immettere un valore** specificare l'ambiente (ad esempio, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-166">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="f2a9b-167">Selezionare la casella di controllo **Impostazione slot** per lasciare invariata l'impostazione dell'ambiente sullo slot corrente quando gli slot di distribuzione vengono scambiati.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-167">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="f2a9b-168">Per altre informazioni, vedere [Documentazione di Azure: Impostazioni incluse nello scambio](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-168">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="f2a9b-169">Selezionare **Salva** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-169">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="f2a9b-170">Servizio app di Azure riavvia automaticamente l'app quando un'impostazione dell'app (una variabile di ambiente) viene aggiunta, modificata o eliminata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-170">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="f2a9b-171">Portale di</span><span class="sxs-lookup"><span data-stu-id="f2a9b-171">Windows</span></span>

<span data-ttu-id="f2a9b-172">Per impostare la variabile `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite [dotnet run](/dotnet/core/tools/dotnet-run), vengono usati i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-172">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="f2a9b-173">**Prompt dei comandi**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-173">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="f2a9b-174">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-174">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="f2a9b-175">Questi comandi hanno effetto solo per la finestra corrente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-175">These commands only take effect for the current window.</span></span> <span data-ttu-id="f2a9b-176">Quando la finestra viene chiusa, l'impostazione `ASPNETCORE_ENVIRONMENT` viene ripristinata sull'impostazione predefinita o sul valore del computer.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-176">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="f2a9b-177">Per impostare il valore a livello globale in Windows, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-177">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="f2a9b-178">Aprire il **Pannello di controllo** > **sistema** > **impostazioni di sistema avanzate** e aggiungere o modificare il valore di `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-178">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Proprietà di sistema avanzate](environments/_static/systemsetting_environment.png)

  ![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="f2a9b-181">Aprire un prompt dei comandi di amministrazione e usare il comando `setx` o aprire un prompt dei comandi di PowerShell di amministrazione e usare `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-181">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="f2a9b-182">**Prompt dei comandi**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-182">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="f2a9b-183">L'opzione `/M` indica di impostare la variabile di ambiente a livello del sistema.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-183">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="f2a9b-184">Se non viene usata l'opzione `/M`, la variabile di ambiente viene impostata per l'account utente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-184">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="f2a9b-185">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-185">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="f2a9b-186">Il valore dell'opzione `Machine` indica di impostare la variabile di ambiente a livello del sistema.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-186">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="f2a9b-187">Se non viene usata l'opzione `User`, la variabile di ambiente viene impostata per l'account utente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-187">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="f2a9b-188">Quando la variabile di ambiente `ASPNETCORE_ENVIRONMENT` è impostata a livello globale, viene applicata per `dotnet run` in tutte le finestre di comando aperte dopo l'impostazione del valore.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-188">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="f2a9b-189">**web.config**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-189">**web.config**</span></span>

<span data-ttu-id="f2a9b-190">Per impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` con *web.config*, vedere la sezione *Impostazione delle variabili di ambiente* di <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-190">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="f2a9b-191">**File di progetto o profilo di pubblicazione**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-191">**Project file or publish profile**</span></span>

<span data-ttu-id="f2a9b-192">**Per le distribuzioni di Windows IIS:** Includere la proprietà `<EnvironmentName>` nel [profilo di pubblicazione (con estensione pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) o nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-192">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="f2a9b-193">Questo approccio imposta l'ambiente in *web.config* quando viene pubblicato il progetto:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-193">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="f2a9b-194">**Pool di applicazioni IIS singoli**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-194">**Per IIS Application Pool**</span></span>

<span data-ttu-id="f2a9b-195">Per impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` per un'app in esecuzione in un pool di applicazioni isolato (supportato in IIS 10.0 o versioni successive), vedere la sezione *AppCmd.exe* dell'argomento [Variabili di ambiente &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-195">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="f2a9b-196">Quando la variabile di ambiente `ASPNETCORE_ENVIRONMENT` viene impostata per un pool di app, il suo valore esegue l'override di un'impostazione a livello di sistema.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-196">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2a9b-197">Durante l'hosting di un'app in IIS, quando si aggiunge o si modifica la variabile di ambiente `ASPNETCORE_ENVIRONMENT`, usare uno degli approcci seguenti per fare in modo che il nuovo valore venga selezionato dalle app:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-197">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="f2a9b-198">Eseguire `net stop was /y` seguito da `net start w3svc` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-198">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="f2a9b-199">Riavviare il server.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-199">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="f2a9b-200">macOS</span><span class="sxs-lookup"><span data-stu-id="f2a9b-200">macOS</span></span>

<span data-ttu-id="f2a9b-201">L'impostazione dell'ambiente corrente per macOS può essere eseguita in linea durante l'esecuzione dell'app:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-201">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="f2a9b-202">In alternativa, impostare l'ambiente tramite `export` prima di eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-202">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="f2a9b-203">Le variabili di ambiente a livello computer sono impostate nel file con estensione *bashrc* o *bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-203">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="f2a9b-204">Modificare il file usando un qualsiasi editor di testo.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-204">Edit the file using any text editor.</span></span> <span data-ttu-id="f2a9b-205">Aggiungere l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-205">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="f2a9b-206">Linux</span><span class="sxs-lookup"><span data-stu-id="f2a9b-206">Linux</span></span>

<span data-ttu-id="f2a9b-207">Per le distribuzioni di Linux, usare il comando `export` al prompt dei comandi per le impostazioni delle variabili basate sulla sessione e il file *bash_profile* per le impostazioni di ambiente a livello di computer.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-207">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="f2a9b-208">Impostazione dell'ambiente nel codice</span><span class="sxs-lookup"><span data-stu-id="f2a9b-208">Set the environment in code</span></span>

<span data-ttu-id="f2a9b-209">Chiamare <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> durante la compilazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-209">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="f2a9b-210">Vedere <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-210">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="f2a9b-211">Configurazione per ambiente</span><span class="sxs-lookup"><span data-stu-id="f2a9b-211">Configuration by environment</span></span>

<span data-ttu-id="f2a9b-212">Per caricare la configurazione dall'ambiente, sono consigliabili:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-212">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="f2a9b-213">file *appSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-213">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="f2a9b-214">Vedere <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-214">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="f2a9b-215">Variabili di ambiente (impostate in ogni sistema in cui è ospitata l'app).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-215">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="f2a9b-216">Vedere <xref:fundamentals/host/generic-host#environmentname>e <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-216">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="f2a9b-217">Secret Manager (solo nell'ambiente di sviluppo).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-217">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="f2a9b-218">Vedere <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-218">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="f2a9b-219">Classe Startup e metodi basati sull'ambiente</span><span class="sxs-lookup"><span data-stu-id="f2a9b-219">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="f2a9b-220">Inserire IWebHostEnvironment in startup. Configure</span><span class="sxs-lookup"><span data-stu-id="f2a9b-220">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="f2a9b-221">Inserire <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-221">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="f2a9b-222">Questo approccio è utile quando l'app richiede solo la regolazione `Startup.Configure` per alcuni ambienti con differenze minime di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-222">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="f2a9b-223">Inserire IWebHostEnvironment nella classe Startup</span><span class="sxs-lookup"><span data-stu-id="f2a9b-223">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="f2a9b-224">Inserire <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> nel costruttore di `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-224">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="f2a9b-225">Questo approccio è utile quando l'app richiede la configurazione di `Startup` solo per alcuni ambienti con differenze minime di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-225">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="f2a9b-226">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-226">In the following example:</span></span>

* <span data-ttu-id="f2a9b-227">L'ambiente viene mantenuto nel campo `_env`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-227">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="f2a9b-228">`_env` viene usato in `ConfigureServices` e `Configure` per applicare la configurazione di avvio in base all'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-228">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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
### <a name="startup-class-conventions"></a><span data-ttu-id="f2a9b-229">Convenzioni delle classi di avvio</span><span class="sxs-lookup"><span data-stu-id="f2a9b-229">Startup class conventions</span></span>

<span data-ttu-id="f2a9b-230">Quando viene avviata un'app ASP.NET Core, la [classe Startup](xref:fundamentals/startup) avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-230">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="f2a9b-231">L'app può definire classi di `Startup` separate per ambienti diversi, ad esempio `StartupDevelopment`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-231">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="f2a9b-232">La classe `Startup` appropriata viene selezionata in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-232">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="f2a9b-233">La classe il cui suffisso di nome corrisponde all'ambiente corrente ha la priorità.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-233">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="f2a9b-234">Se non viene trovata una classe `Startup{EnvironmentName}` corrispondente, viene usata la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-234">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="f2a9b-235">Questo approccio è utile quando l'app richiede la configurazione dell'avvio per diversi ambienti con molte differenze di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-235">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="f2a9b-236">Per implementare le classi `Startup` basate sull'ambiente, creare una classe `Startup{EnvironmentName}` per ogni ambiente in uso e una classe `Startup` di fallback:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-236">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="f2a9b-237">Usare l'overload [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) che accetta un nome di assembly:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-237">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="f2a9b-238">Convenzioni dei metodi di avvio</span><span class="sxs-lookup"><span data-stu-id="f2a9b-238">Startup method conventions</span></span>

<span data-ttu-id="f2a9b-239">[Configurare](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) supportare versioni specifiche dell'ambiente del modulo `Configure<EnvironmentName>` e `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-239">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="f2a9b-240">Questo approccio è utile quando l'app richiede la configurazione dell'avvio per diversi ambienti con molte differenze di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-240">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="f2a9b-241">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f2a9b-241">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f2a9b-242">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f2a9b-242">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f2a9b-243">ASP.NET Core configura il comportamento dell'app in base all'ambiente di runtime e tramite una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-243">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="f2a9b-244">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f2a9b-244">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="f2a9b-245">Ambienti</span><span class="sxs-lookup"><span data-stu-id="f2a9b-245">Environments</span></span>

<span data-ttu-id="f2a9b-246">ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'app e ne archivia il valore in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-246">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="f2a9b-247">`ASPNETCORE_ENVIRONMENT` può essere impostato su qualsiasi valore, ma vengono forniti tre valori dal Framework:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-247">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="f2a9b-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="f2a9b-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="f2a9b-249">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-249">The preceding code:</span></span>

* <span data-ttu-id="f2a9b-250">Chiama [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) quando `ASPNETCORE_ENVIRONMENT` è impostato su `Development`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-250">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="f2a9b-251">Chiama [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quando il valore di `ASPNETCORE_ENVIRONMENT` è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-251">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="f2a9b-252">L'[helper per tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere il markup nell'elemento:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-252">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="f2a9b-253">In Windows e macOS le variabili di ambiente e i relativi valori non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-253">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="f2a9b-254">In Linux le variabili di ambiente e i relativi valori **applicano la distinzione tra maiuscole e minuscole** per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-254">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="f2a9b-255">Sviluppo</span><span class="sxs-lookup"><span data-stu-id="f2a9b-255">Development</span></span>

<span data-ttu-id="f2a9b-256">L'ambiente di sviluppo può abilitare funzionalità che non devono essere esposte nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-256">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="f2a9b-257">Ad esempio i modelli ASP.NET Core abilitano la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling#developer-exception-page) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-257">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="f2a9b-258">L'ambiente di sviluppo computer locale può essere impostato nel file *Properties\launchSettings.json* del progetto.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-258">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="f2a9b-259">I valori di ambiente in *launchSettings.json* sostituiscono i valori impostati nell'ambiente di sistema.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-259">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="f2a9b-260">Il codice JSON seguente visualizza tre profili di un file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-260">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="f2a9b-261">La proprietà `applicationUrl` in *launchSettings.json* può specificare un elenco di URL di server.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-261">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="f2a9b-262">Usare un punto e virgola tra gli URL dell'elenco:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-262">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="f2a9b-263">Quando l'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run) viene usato il primo profilo con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-263">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="f2a9b-264">Il valore di `commandName` specifica il server Web da avviare.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-264">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="f2a9b-265">`commandName` può avere uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-265">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="f2a9b-266">`Project` (che avvia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="f2a9b-266">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="f2a9b-267">Quando un'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="f2a9b-267">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="f2a9b-268">*launchSettings.json*, se disponibile, viene letto.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-268">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="f2a9b-269">Le impostazioni `environmentVariables` in *launchSettings.json* sostituiscono le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-269">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="f2a9b-270">Viene visualizzato l'ambiente host.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-270">The hosting environment is displayed.</span></span>

<span data-ttu-id="f2a9b-271">L'output seguente visualizza un'app avviata con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="f2a9b-271">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="f2a9b-272">La scheda **Debug** nelle proprietà di progetto di Visual Studio visualizza una GUI per la modifica del file *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-272">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proprietà progetto - Impostazione delle variabili di ambiente](environments/_static/project-properties-debug.png)

<span data-ttu-id="f2a9b-274">È possibile che le modifiche apportate ai profili di progetto abbiano effetto solo dopo il riavvio del server Web.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-274">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="f2a9b-275">Perché Kestrel rilevi le modifiche apportate al suo ambiente, deve essere riavviato.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-275">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="f2a9b-276">Evitare di archiviare segreti in *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-276">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="f2a9b-277">Per l'archiviazione di segreti per lo sviluppo locale, usare lo [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-277">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="f2a9b-278">Quando si usa [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-278">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="f2a9b-279">Nell'esempio seguente l'ambiente viene impostato su `Development`:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-279">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="f2a9b-280">Un file *.vscode/launch.json* del progetto non viene letto quando si avvia l'app con `dotnet run` come accade per il file *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-280">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="f2a9b-281">Quando si avvia un'app in un ambiente di sviluppo che non ha un file *launchSettings.json*, impostare una variabile di ambiente o un argomento della riga di comando sul comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-281">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="f2a9b-282">Produzione</span><span class="sxs-lookup"><span data-stu-id="f2a9b-282">Production</span></span>

<span data-ttu-id="f2a9b-283">È necessario che l'ambiente di produzione sia configurato per ottimizzare la sicurezza, le prestazioni e l'affidabilità dell'app.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-283">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="f2a9b-284">Alcune impostazioni comuni che differiscono dallo sviluppo includono:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-284">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="f2a9b-285">Memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-285">Caching.</span></span>
* <span data-ttu-id="f2a9b-286">Risorse lato client in bundle, minimizzate e potenzialmente offerte da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-286">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="f2a9b-287">Pagine di errore di diagnostica disabilitate.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-287">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="f2a9b-288">Pagine di errore descrittive abilitate.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-288">Friendly error pages enabled.</span></span>
* <span data-ttu-id="f2a9b-289">Registrazione e monitoraggio della produzione abilitati.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-289">Production logging and monitoring enabled.</span></span> <span data-ttu-id="f2a9b-290">Ad esempio [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-290">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="f2a9b-291">Impostare l'ambiente</span><span class="sxs-lookup"><span data-stu-id="f2a9b-291">Set the environment</span></span>

<span data-ttu-id="f2a9b-292">Spesso è utile impostare un ambiente specifico per il test con una variabile di ambiente o un'impostazione della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-292">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="f2a9b-293">Se l'ambiente non viene impostato, il valore predefinito è `Production`, che disabilita la maggior parte delle funzionalità di debug.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-293">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="f2a9b-294">Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-294">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="f2a9b-295">Quando viene compilato l'host, l'ultima impostazione dell'ambiente letta dall'app determina l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-295">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="f2a9b-296">L'ambiente dell'app non può essere modificato mentre l'app è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-296">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="f2a9b-297">Impostazione della piattaforma o della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="f2a9b-297">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="f2a9b-298">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="f2a9b-298">Azure App Service</span></span>

<span data-ttu-id="f2a9b-299">Per impostare l'ambiente in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-299">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="f2a9b-300">Selezionare l'app dal pannello **Servizi app**.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-300">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="f2a9b-301">Nel gruppo **IMPOSTAZIONI** selezionare il pannello **Impostazioni dell'applicazione**.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-301">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="f2a9b-302">Nell'area **Impostazioni dell'applicazione** selezionare **Aggiungi nuova impostazione**.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-302">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="f2a9b-303">In **Immettere un nome** specificare `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-303">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="f2a9b-304">In **Immettere un valore** specificare l'ambiente (ad esempio, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-304">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="f2a9b-305">Selezionare la casella di controllo **Impostazione slot** per lasciare invariata l'impostazione dell'ambiente sullo slot corrente quando gli slot di distribuzione vengono scambiati.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-305">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="f2a9b-306">Per altre informazioni, vedere [Documentazione di Azure: Impostazioni incluse nello scambio](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-306">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="f2a9b-307">Selezionare **Salva** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-307">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="f2a9b-308">Servizio app di Azure riavvia automaticamente l'app quando un'impostazione dell'app (una variabile di ambiente) viene aggiunta, modificata o eliminata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-308">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="f2a9b-309">Portale di</span><span class="sxs-lookup"><span data-stu-id="f2a9b-309">Windows</span></span>

<span data-ttu-id="f2a9b-310">Per impostare la variabile `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite [dotnet run](/dotnet/core/tools/dotnet-run), vengono usati i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-310">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="f2a9b-311">**Prompt dei comandi**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-311">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="f2a9b-312">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-312">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="f2a9b-313">Questi comandi hanno effetto solo per la finestra corrente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-313">These commands only take effect for the current window.</span></span> <span data-ttu-id="f2a9b-314">Quando la finestra viene chiusa, l'impostazione `ASPNETCORE_ENVIRONMENT` viene ripristinata sull'impostazione predefinita o sul valore del computer.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-314">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="f2a9b-315">Per impostare il valore a livello globale in Windows, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-315">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="f2a9b-316">Aprire il **Pannello di controllo** > **sistema** > **impostazioni di sistema avanzate** e aggiungere o modificare il valore di `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-316">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Proprietà di sistema avanzate](environments/_static/systemsetting_environment.png)

  ![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="f2a9b-319">Aprire un prompt dei comandi di amministrazione e usare il comando `setx` o aprire un prompt dei comandi di PowerShell di amministrazione e usare `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-319">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="f2a9b-320">**Prompt dei comandi**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-320">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="f2a9b-321">L'opzione `/M` indica di impostare la variabile di ambiente a livello del sistema.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-321">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="f2a9b-322">Se non viene usata l'opzione `/M`, la variabile di ambiente viene impostata per l'account utente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-322">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="f2a9b-323">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-323">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="f2a9b-324">Il valore dell'opzione `Machine` indica di impostare la variabile di ambiente a livello del sistema.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-324">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="f2a9b-325">Se non viene usata l'opzione `User`, la variabile di ambiente viene impostata per l'account utente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-325">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="f2a9b-326">Quando la variabile di ambiente `ASPNETCORE_ENVIRONMENT` è impostata a livello globale, viene applicata per `dotnet run` in tutte le finestre di comando aperte dopo l'impostazione del valore.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-326">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="f2a9b-327">**web.config**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-327">**web.config**</span></span>

<span data-ttu-id="f2a9b-328">Per impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` con *web.config*, vedere la sezione *Impostazione delle variabili di ambiente* di <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-328">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="f2a9b-329">**File di progetto o profilo di pubblicazione**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-329">**Project file or publish profile**</span></span>

<span data-ttu-id="f2a9b-330">**Per le distribuzioni di Windows IIS:** Includere la proprietà `<EnvironmentName>` nel [profilo di pubblicazione (con estensione pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) o nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-330">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="f2a9b-331">Questo approccio imposta l'ambiente in *web.config* quando viene pubblicato il progetto:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-331">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="f2a9b-332">**Pool di applicazioni IIS singoli**</span><span class="sxs-lookup"><span data-stu-id="f2a9b-332">**Per IIS Application Pool**</span></span>

<span data-ttu-id="f2a9b-333">Per impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` per un'app in esecuzione in un pool di applicazioni isolato (supportato in IIS 10.0 o versioni successive), vedere la sezione *AppCmd.exe* dell'argomento [Variabili di ambiente &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-333">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="f2a9b-334">Quando la variabile di ambiente `ASPNETCORE_ENVIRONMENT` viene impostata per un pool di app, il suo valore esegue l'override di un'impostazione a livello di sistema.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-334">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2a9b-335">Durante l'hosting di un'app in IIS, quando si aggiunge o si modifica la variabile di ambiente `ASPNETCORE_ENVIRONMENT`, usare uno degli approcci seguenti per fare in modo che il nuovo valore venga selezionato dalle app:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-335">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="f2a9b-336">Eseguire `net stop was /y` seguito da `net start w3svc` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-336">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="f2a9b-337">Riavviare il server.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-337">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="f2a9b-338">macOS</span><span class="sxs-lookup"><span data-stu-id="f2a9b-338">macOS</span></span>

<span data-ttu-id="f2a9b-339">L'impostazione dell'ambiente corrente per macOS può essere eseguita in linea durante l'esecuzione dell'app:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-339">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="f2a9b-340">In alternativa, impostare l'ambiente tramite `export` prima di eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-340">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="f2a9b-341">Le variabili di ambiente a livello computer sono impostate nel file con estensione *bashrc* o *bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-341">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="f2a9b-342">Modificare il file usando un qualsiasi editor di testo.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-342">Edit the file using any text editor.</span></span> <span data-ttu-id="f2a9b-343">Aggiungere l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-343">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="f2a9b-344">Linux</span><span class="sxs-lookup"><span data-stu-id="f2a9b-344">Linux</span></span>

<span data-ttu-id="f2a9b-345">Per le distribuzioni di Linux, usare il comando `export` al prompt dei comandi per le impostazioni delle variabili basate sulla sessione e il file *bash_profile* per le impostazioni di ambiente a livello di computer.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-345">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="f2a9b-346">Impostazione dell'ambiente nel codice</span><span class="sxs-lookup"><span data-stu-id="f2a9b-346">Set the environment in code</span></span>

<span data-ttu-id="f2a9b-347">Chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> durante la compilazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-347">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="f2a9b-348">Vedere <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-348">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="f2a9b-349">Configurazione per ambiente</span><span class="sxs-lookup"><span data-stu-id="f2a9b-349">Configuration by environment</span></span>

<span data-ttu-id="f2a9b-350">Per caricare la configurazione dall'ambiente, sono consigliabili:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-350">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="f2a9b-351">file *appSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-351">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="f2a9b-352">Vedere <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-352">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="f2a9b-353">Variabili di ambiente (impostate in ogni sistema in cui è ospitata l'app).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-353">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="f2a9b-354">Vedere <xref:fundamentals/host/web-host#environment>e <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-354">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="f2a9b-355">Secret Manager (solo nell'ambiente di sviluppo).</span><span class="sxs-lookup"><span data-stu-id="f2a9b-355">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="f2a9b-356">Vedere <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-356">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="f2a9b-357">Classe Startup e metodi basati sull'ambiente</span><span class="sxs-lookup"><span data-stu-id="f2a9b-357">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="f2a9b-358">Inserire IHostingEnvironment in startup. Configure</span><span class="sxs-lookup"><span data-stu-id="f2a9b-358">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="f2a9b-359">Inserire <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-359">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="f2a9b-360">Questo approccio è utile quando l'app richiede solo la configurazione di `Startup.Configure` solo per alcuni ambienti con differenze minime di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-360">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="f2a9b-361">Inserire IHostingEnvironment nella classe Startup</span><span class="sxs-lookup"><span data-stu-id="f2a9b-361">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="f2a9b-362">Inserire <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> nel costruttore di `Startup` e assegnare il servizio a un campo da utilizzare in tutta la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-362">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="f2a9b-363">Questo approccio è utile quando l'app richiede la configurazione dell'avvio per solo alcuni ambienti con differenze minime di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-363">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="f2a9b-364">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-364">In the following example:</span></span>

* <span data-ttu-id="f2a9b-365">L'ambiente viene mantenuto nel campo `_env`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-365">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="f2a9b-366">`_env` viene usato in `ConfigureServices` e `Configure` per applicare la configurazione di avvio in base all'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-366">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

### <a name="startup-class-conventions"></a><span data-ttu-id="f2a9b-367">Convenzioni delle classi di avvio</span><span class="sxs-lookup"><span data-stu-id="f2a9b-367">Startup class conventions</span></span>

<span data-ttu-id="f2a9b-368">Quando viene avviata un'app ASP.NET Core, la [classe Startup](xref:fundamentals/startup) avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-368">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="f2a9b-369">L'app può definire classi di `Startup` separate per ambienti diversi, ad esempio `StartupDevelopment`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-369">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="f2a9b-370">La classe `Startup` appropriata viene selezionata in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-370">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="f2a9b-371">La classe il cui suffisso di nome corrisponde all'ambiente corrente ha la priorità.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-371">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="f2a9b-372">Se non viene trovata una classe `Startup{EnvironmentName}` corrispondente, viene usata la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-372">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="f2a9b-373">Questo approccio è utile quando l'app richiede la configurazione dell'avvio per diversi ambienti con molte differenze di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-373">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="f2a9b-374">Per implementare le classi `Startup` basate sull'ambiente, creare una classe `Startup{EnvironmentName}` per ogni ambiente in uso e una classe `Startup` di fallback:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-374">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="f2a9b-375">Usare l'overload [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) che accetta un nome di assembly:</span><span class="sxs-lookup"><span data-stu-id="f2a9b-375">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="f2a9b-376">Convenzioni dei metodi di avvio</span><span class="sxs-lookup"><span data-stu-id="f2a9b-376">Startup method conventions</span></span>

<span data-ttu-id="f2a9b-377">[Configurare](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) supportare versioni specifiche dell'ambiente del modulo `Configure<EnvironmentName>` e `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-377">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="f2a9b-378">Questo approccio è utile quando l'app richiede la configurazione dell'avvio per diversi ambienti con molte differenze di codice per ogni ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2a9b-378">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="f2a9b-379">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f2a9b-379">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end