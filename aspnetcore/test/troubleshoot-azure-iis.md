---
title: Risolvere i problemi relativi a ASP.NET Core in app Azure servizio e IIS
author: guardrex
description: Informazioni su come diagnosticare i problemi relativi alle distribuzioni del servizio app Azure e Internet Information Services (IIS) delle app di ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2020
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 071dba9e936351e201b7582b3d0667cd6fac54bb
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294623"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="832eb-103">Risolvere i problemi relativi a ASP.NET Core in app Azure servizio e IIS</span><span class="sxs-lookup"><span data-stu-id="832eb-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="832eb-104">Di [Luke Latham](https://github.com/guardrex) e [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="832eb-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="832eb-105">Questo articolo fornisce informazioni sugli errori comuni di avvio delle app e istruzioni su come diagnosticare gli errori quando un'app viene distribuita in app Azure servizio o IIS:</span><span class="sxs-lookup"><span data-stu-id="832eb-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="832eb-106">Errori di avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="832eb-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="832eb-107">Vengono illustrati gli scenari di codice di stato HTTP di avvio comuni.</span><span class="sxs-lookup"><span data-stu-id="832eb-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="832eb-108">Risolvere i problemi relativi al servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="832eb-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="832eb-109">Fornisce consigli per la risoluzione dei problemi per le app distribuite nel servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="832eb-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="832eb-110">Risolvere i problemi in IIS</span><span class="sxs-lookup"><span data-stu-id="832eb-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="832eb-111">Fornisce consigli per la risoluzione dei problemi per le app distribuite in IIS o in esecuzione in IIS Express localmente.</span><span class="sxs-lookup"><span data-stu-id="832eb-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="832eb-112">Le linee guida sono valide per le distribuzioni di Windows Server e Windows desktop.</span><span class="sxs-lookup"><span data-stu-id="832eb-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="832eb-113">Cancella cache di pacchetti</span><span class="sxs-lookup"><span data-stu-id="832eb-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="832eb-114">Spiega cosa fare quando i pacchetti incoerenti interrompono un'app durante l'esecuzione di aggiornamenti principali o la modifica delle versioni del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="832eb-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="832eb-115">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="832eb-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="832eb-116">Elenca ulteriori argomenti sulla risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="832eb-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="832eb-117">Errori di avvio delle app</span><span class="sxs-lookup"><span data-stu-id="832eb-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="832eb-118">In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug.</span><span class="sxs-lookup"><span data-stu-id="832eb-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="832eb-119">Un errore *502,5-Process* o un *errore di avvio 500,30* che si verifica quando il debug locale può essere diagnosticato utilizzando i consigli in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="832eb-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="832eb-120">In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug.</span><span class="sxs-lookup"><span data-stu-id="832eb-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="832eb-121">Un *errore del processo 502,5* che si verifica quando è possibile diagnosticare localmente il debug usando i consigli in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="832eb-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="40314-forbidden"></a><span data-ttu-id="832eb-122">403,14 non consentito</span><span class="sxs-lookup"><span data-stu-id="832eb-122">403.14 Forbidden</span></span>

<span data-ttu-id="832eb-123">Non è possibile avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-123">The app fails to start.</span></span> <span data-ttu-id="832eb-124">Viene registrato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="832eb-124">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="832eb-125">L'errore è in genere causato da una distribuzione interruppe sul sistema host, che include uno degli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="832eb-125">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="832eb-126">L'app viene distribuita nella cartella errata del sistema di hosting.</span><span class="sxs-lookup"><span data-stu-id="832eb-126">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="832eb-127">Il processo di distribuzione non è riuscito a spostare tutti i file e le cartelle dell'app nella cartella di distribuzione nel sistema di hosting.</span><span class="sxs-lookup"><span data-stu-id="832eb-127">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="832eb-128">Il file *Web. config* non è presente nella distribuzione oppure il contenuto del file *Web. config* non è valido.</span><span class="sxs-lookup"><span data-stu-id="832eb-128">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="832eb-129">Eseguire la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="832eb-129">Perform the following steps:</span></span>

1. <span data-ttu-id="832eb-130">Eliminare tutti i file e le cartelle dalla cartella di distribuzione nel sistema di hosting.</span><span class="sxs-lookup"><span data-stu-id="832eb-130">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="832eb-131">Ridistribuire il contenuto della cartella di *pubblicazione* dell'app nel sistema host usando il normale metodo di distribuzione, ad esempio Visual Studio, PowerShell o la distribuzione manuale:</span><span class="sxs-lookup"><span data-stu-id="832eb-131">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="832eb-132">Verificare che il file *Web. config* sia presente nella distribuzione e che il contenuto sia corretto.</span><span class="sxs-lookup"><span data-stu-id="832eb-132">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="832eb-133">Quando si esegue l'hosting in app Azure servizio, verificare che l'app venga distribuita nella cartella `D:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="832eb-133">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="832eb-134">Quando l'app è ospitata da IIS, verificare che l'app venga distribuita nel **percorso fisico** IIS visualizzato nelle impostazioni di **base**di **Gestione IIS**.</span><span class="sxs-lookup"><span data-stu-id="832eb-134">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="832eb-135">Verificare che tutti i file e le cartelle dell'app vengano distribuiti confrontando la distribuzione nel sistema di hosting con il contenuto della cartella di *pubblicazione* del progetto.</span><span class="sxs-lookup"><span data-stu-id="832eb-135">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="832eb-136">Per ulteriori informazioni sul layout di un'app ASP.NET Core pubblicata, vedere <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="832eb-136">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="832eb-137">Per ulteriori informazioni sul file *Web. config* , vedere <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="832eb-137">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="832eb-138">500 Errore interno del server</span><span class="sxs-lookup"><span data-stu-id="832eb-138">500 Internal Server Error</span></span>

<span data-ttu-id="832eb-139">L'app viene avviata, ma un errore impedisce al server di soddisfare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="832eb-139">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="832eb-140">Questo errore si verifica all'interno del codice dell'app durante l'avvio o durante la creazione di una risposta.</span><span class="sxs-lookup"><span data-stu-id="832eb-140">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="832eb-141">La risposta potrebbe non avere alcun contenuto o essere visualizzata nel browser come un codice *500 Errore interno del server*.</span><span class="sxs-lookup"><span data-stu-id="832eb-141">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="832eb-142">Il log eventi dell'applicazione in genere indica che l'app è stata avviata normalmente.</span><span class="sxs-lookup"><span data-stu-id="832eb-142">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="832eb-143">Dal punto di vista del server, questo è corretto.</span><span class="sxs-lookup"><span data-stu-id="832eb-143">From the server's perspective, that's correct.</span></span> <span data-ttu-id="832eb-144">L'app è stata effettivamente avviata, ma non può generare una risposta valida.</span><span class="sxs-lookup"><span data-stu-id="832eb-144">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="832eb-145">Eseguire l'app al prompt dei comandi nel server o abilitare il log stdout del modulo ASP.NET Core per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="832eb-145">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="832eb-146">500.0 Errore di caricamento del gestore in-process</span><span class="sxs-lookup"><span data-stu-id="832eb-146">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="832eb-147">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="832eb-147">The worker process fails.</span></span> <span data-ttu-id="832eb-148">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="832eb-148">The app doesn't start.</span></span>

<span data-ttu-id="832eb-149">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) non riesce a trovare il CLR .NET Core e a trovare il gestore della richiesta in-process (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="832eb-149">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="832eb-150">Verificare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="832eb-150">Check that:</span></span>

* <span data-ttu-id="832eb-151">L'app specifichi come destinazione il pacchetto NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) o il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="832eb-151">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="832eb-152">Le versione del framework condiviso ASP.NET Core specificata come destinazione dall'app sia installata nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-152">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="832eb-153">500.0 Errore di caricamento del gestore out-of-process</span><span class="sxs-lookup"><span data-stu-id="832eb-153">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="832eb-154">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="832eb-154">The worker process fails.</span></span> <span data-ttu-id="832eb-155">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="832eb-155">The app doesn't start.</span></span>

<span data-ttu-id="832eb-156">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) non riesce a trovare il gestore della richiesta di hosting out-of-process.</span><span class="sxs-lookup"><span data-stu-id="832eb-156">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="832eb-157">Verificare che *aspnetcorev2_outofprocess.dll* sia presente in una sottocartella accanto ad *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="832eb-157">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="832eb-158">500.0 Errore di caricamento del gestore in-process</span><span class="sxs-lookup"><span data-stu-id="832eb-158">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="832eb-159">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="832eb-159">The worker process fails.</span></span> <span data-ttu-id="832eb-160">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="832eb-160">The app doesn't start.</span></span>

<span data-ttu-id="832eb-161">Si è verificato un errore sconosciuto durante il caricamento dei componenti del [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="832eb-161">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="832eb-162">Eseguire una delle azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="832eb-162">Take one of the following actions:</span></span>

* <span data-ttu-id="832eb-163">Contattare il [supporto tecnico Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832). Selezionare **Strumenti di sviluppo** e quindi **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="832eb-163">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="832eb-164">Porre una domanda in Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="832eb-164">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="832eb-165">Segnalare il problema nel [repository GitHub](https://github.com/dotnet/AspNetCore) di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="832eb-165">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="832eb-166">500.30 Errore di avvio in-process</span><span class="sxs-lookup"><span data-stu-id="832eb-166">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="832eb-167">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="832eb-167">The worker process fails.</span></span> <span data-ttu-id="832eb-168">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="832eb-168">The app doesn't start.</span></span>

<span data-ttu-id="832eb-169">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tenta di avviare .NET Core CLR in-process, ma non è possibile avviarlo.</span><span class="sxs-lookup"><span data-stu-id="832eb-169">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="832eb-170">La cause di un errore di avvio del processo può in genere essere determinata dalle voci nel registro eventi dell'applicazione e dal log stdout del modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="832eb-170">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="832eb-171">Condizioni di errore comuni:</span><span class="sxs-lookup"><span data-stu-id="832eb-171">Common failure conditions:</span></span>

* <span data-ttu-id="832eb-172">L'app non è configurata correttamente perché la destinazione è una versione di ASP.NET Core Framework condiviso che non è presente.</span><span class="sxs-lookup"><span data-stu-id="832eb-172">The app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="832eb-173">Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-173">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>
* <span data-ttu-id="832eb-174">Utilizzando Azure Key Vault, non sono disponibili autorizzazioni per l'Key Vault.</span><span class="sxs-lookup"><span data-stu-id="832eb-174">Using Azure Key Vault, lack of permissions to the Key Vault.</span></span> <span data-ttu-id="832eb-175">Verificare i criteri di accesso nel Key Vault di destinazione per verificare che siano state concesse le autorizzazioni corrette.</span><span class="sxs-lookup"><span data-stu-id="832eb-175">Check the access policies in the targeted Key Vault to ensure that the correct permissions are granted.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="832eb-176">500.31 ANCM Failed to Find Native Dependencies (500.31 Il modulo ASP.NET Core non è riuscito a trovare le dipendenze native)</span><span class="sxs-lookup"><span data-stu-id="832eb-176">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="832eb-177">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="832eb-177">The worker process fails.</span></span> <span data-ttu-id="832eb-178">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="832eb-178">The app doesn't start.</span></span>

<span data-ttu-id="832eb-179">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tenta di avviare il runtime di .NET Core in-process, ma non è possibile avviarlo.</span><span class="sxs-lookup"><span data-stu-id="832eb-179">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="832eb-180">La causa più comune di questo errore di avvio è la mancata installazione del runtime di `Microsoft.NETCore.App` o di `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="832eb-180">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="832eb-181">Se l'app viene distribuita per ASP.NET Core 3.0 e tale versione non esiste nel computer, si verifica questo errore.</span><span class="sxs-lookup"><span data-stu-id="832eb-181">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="832eb-182">Ecco un esempio di messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="832eb-182">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="832eb-183">Il messaggio di errore elenca tutte le versioni di .NET Core installate e la versione richiesta dall'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-183">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="832eb-184">Per correggere l'errore, eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="832eb-184">To fix this error, either:</span></span>

* <span data-ttu-id="832eb-185">Installare la versione appropriata di .NET Core nel computer.</span><span class="sxs-lookup"><span data-stu-id="832eb-185">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="832eb-186">Modificare l'app in modo che usi una versione di .NET Core presente nel computer.</span><span class="sxs-lookup"><span data-stu-id="832eb-186">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="832eb-187">Pubblicare l'app come [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="832eb-187">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="832eb-188">Durante l'esecuzione in fase di sviluppo (con la variabile di ambiente `ASPNETCORE_ENVIRONMENT` impostata su `Development`), l'errore specifico viene scritto nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="832eb-188">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="832eb-189">La cause di un errore di avvio del processo è disponibile anche nel registro eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-189">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="832eb-190">500.32 ANCM Failed to Load dll (500.32 Il modulo ASP.NET Core non è riuscito a caricare la DLL)</span><span class="sxs-lookup"><span data-stu-id="832eb-190">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="832eb-191">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="832eb-191">The worker process fails.</span></span> <span data-ttu-id="832eb-192">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="832eb-192">The app doesn't start.</span></span>

<span data-ttu-id="832eb-193">La causa più comune di questo errore è la pubblicazione dell'app per processori con architettura non compatibile.</span><span class="sxs-lookup"><span data-stu-id="832eb-193">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="832eb-194">Se il processo di lavoro è in esecuzione come app a 32 bit e l'app è stata pubblicata per un'architettura a 64 bit, si verifica questo errore.</span><span class="sxs-lookup"><span data-stu-id="832eb-194">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="832eb-195">Per correggere l'errore, eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="832eb-195">To fix this error, either:</span></span>

* <span data-ttu-id="832eb-196">Ripubblicare l'app per processori con la stessa architettura del processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="832eb-196">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="832eb-197">Pubblicare l'app come [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="832eb-197">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="832eb-198">500.33 ANCM Request Handler Load Failure (500.33 Errore di caricamento del gestore della richiesta del modulo ASP.Net Core)</span><span class="sxs-lookup"><span data-stu-id="832eb-198">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="832eb-199">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="832eb-199">The worker process fails.</span></span> <span data-ttu-id="832eb-200">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="832eb-200">The app doesn't start.</span></span>

<span data-ttu-id="832eb-201">L'app non fa riferimento al framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="832eb-201">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="832eb-202">Solo le app destinate a `Microsoft.AspNetCore.App` Framework possono essere ospitate dal [modulo di ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="832eb-202">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="832eb-203">Per correggere questo errore, verificare che l'app sia destinata al framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="832eb-203">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="832eb-204">Controllare il framework di destinazione dell'app nel file `.runtimeconfig.json`.</span><span class="sxs-lookup"><span data-stu-id="832eb-204">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="832eb-205">500.34 ANCM Mixed Hosting Models Not Supported (500.34 Modelli di hosting misto non supportati nel modulo ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="832eb-205">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="832eb-206">Il processo di lavoro non è in grado di eseguire un'app In-Process e un'app Out-of-process nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="832eb-206">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="832eb-207">Per correggere questo errore, eseguire le app in pool di applicazioni IIS separati.</span><span class="sxs-lookup"><span data-stu-id="832eb-207">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="832eb-208">500.35 ANCM Multiple In-Process Applications in same Process (500.35 Modulo ASP.NET Core: più applicazioni In-Process nello stesso processo)</span><span class="sxs-lookup"><span data-stu-id="832eb-208">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="832eb-209">Il processo di lavoro non può eseguire più app in-process nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="832eb-209">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="832eb-210">Per correggere questo errore, eseguire le app in pool di applicazioni IIS separati.</span><span class="sxs-lookup"><span data-stu-id="832eb-210">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="832eb-211">500.36 ANCM Out-Of-Process Handler Load Failure (500.36 Modulo ASP.NET Core: Errore di caricamento del gestore Out-of-process)</span><span class="sxs-lookup"><span data-stu-id="832eb-211">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="832eb-212">Il gestore delle richieste Out-of-process, *aspnetcorev2_outofprocess.dll*, non è accanto al file *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="832eb-212">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="832eb-213">Indica un'installazione danneggiata del [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="832eb-213">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="832eb-214">Per correggere questo errore, riparare l'installazione del [bundle di hosting di .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (per IIS) o di Visual Studio (per IIS Express).</span><span class="sxs-lookup"><span data-stu-id="832eb-214">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="832eb-215">500.37 ANCM Failed to Start Within Startup Time Limit (500.37 Avvio del modulo ASP.NET Core non riuscito entro il limite di tempo stabilito)</span><span class="sxs-lookup"><span data-stu-id="832eb-215">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="832eb-216">Il modulo ASP.NET Core non è stato avviato entro il limite di tempo specificato.</span><span class="sxs-lookup"><span data-stu-id="832eb-216">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="832eb-217">Per impostazione predefinita, il timeout è di 120 secondi.</span><span class="sxs-lookup"><span data-stu-id="832eb-217">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="832eb-218">Questo errore può verificarsi quando si avvia un numero elevato di app nello stesso computer.</span><span class="sxs-lookup"><span data-stu-id="832eb-218">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="832eb-219">Controllare i picchi di utilizzo della CPU e della memoria nel server durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="832eb-219">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="832eb-220">Può essere necessario scaglionare il processo di avvio di più app.</span><span class="sxs-lookup"><span data-stu-id="832eb-220">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="832eb-221">502.5 Errore del processo</span><span class="sxs-lookup"><span data-stu-id="832eb-221">502.5 Process Failure</span></span>

<span data-ttu-id="832eb-222">Il processo di lavoro ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="832eb-222">The worker process fails.</span></span> <span data-ttu-id="832eb-223">L'app non viene avviata.</span><span class="sxs-lookup"><span data-stu-id="832eb-223">The app doesn't start.</span></span>

<span data-ttu-id="832eb-224">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tenta di avviare il processo di lavoro, ma l'avvio non riesce.</span><span class="sxs-lookup"><span data-stu-id="832eb-224">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="832eb-225">La cause di un errore di avvio del processo può in genere essere determinata dalle voci nel registro eventi dell'applicazione e dal log stdout del modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="832eb-225">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="832eb-226">Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente.</span><span class="sxs-lookup"><span data-stu-id="832eb-226">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="832eb-227">Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-227">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="832eb-228">Il *framework condiviso* è il set di assembly (file *DLL*) che vengono installati nel computer e cui fa riferimento un metapacchetto, ad esempio `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="832eb-228">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="832eb-229">Il riferimento del metapacchetto può specificare una versione minima richiesta.</span><span class="sxs-lookup"><span data-stu-id="832eb-229">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="832eb-230">Per altre informazioni, vedere [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) (Il framework condiviso).</span><span class="sxs-lookup"><span data-stu-id="832eb-230">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="832eb-231">La pagina di errore *502.5 Errore del processo* viene restituita quando il processo di lavoro ha esito negativo a causa di un errore di configurazione dell'hosting o dell'app:</span><span class="sxs-lookup"><span data-stu-id="832eb-231">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="832eb-232">Non è stato possibile avviare l'aggiornamento dell'applicazione. Errore: 0x800700c1</span><span class="sxs-lookup"><span data-stu-id="832eb-232">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="832eb-233">L'app non è stata avviata perché non è stato possibile caricarne l'assembly ( *.dll*).</span><span class="sxs-lookup"><span data-stu-id="832eb-233">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="832eb-234">Questo errore si verifica quando è presente una mancata corrispondenza del numero di bit tra l'app pubblicata e il processo w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="832eb-234">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="832eb-235">Verificare che l'impostazione del pool di app a 32 bit sia corretta:</span><span class="sxs-lookup"><span data-stu-id="832eb-235">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="832eb-236">Selezionare il pool di app in **Pool di applicazioni** di Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="832eb-236">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="832eb-237">Selezionare **Impostazioni avanzate** in **Modifica pool di applicazioni** nel pannello **Azioni**.</span><span class="sxs-lookup"><span data-stu-id="832eb-237">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="832eb-238">Impostare **Attiva applicazioni a 32 bit**:</span><span class="sxs-lookup"><span data-stu-id="832eb-238">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="832eb-239">Se si sta sviluppando un'app a 32 bit (x86), impostare il valore su `True`.</span><span class="sxs-lookup"><span data-stu-id="832eb-239">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="832eb-240">Se si sta sviluppando un'app a 64 bit (x64), impostare il valore su `False`.</span><span class="sxs-lookup"><span data-stu-id="832eb-240">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="832eb-241">Verificare che non esista un conflitto tra una `<Platform>` proprietà MSBuild nel file di progetto e il bit pubblicato dell'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-241">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="832eb-242">Reimpostazione della connessione</span><span class="sxs-lookup"><span data-stu-id="832eb-242">Connection reset</span></span>

<span data-ttu-id="832eb-243">Se si verifica un errore dopo l'invio delle intestazioni, è troppo tardi perché il server possa inviare un codice **500 Errore interno del server** quando si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="832eb-243">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="832eb-244">Questo spesso accade quando si verifica un errore durante la serializzazione di oggetti complessi per una risposta.</span><span class="sxs-lookup"><span data-stu-id="832eb-244">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="832eb-245">Questo tipo di errore viene visualizzato come un errore di *reimpostazione della connessione* nel client.</span><span class="sxs-lookup"><span data-stu-id="832eb-245">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="832eb-246">La [registrazione delle applicazioni](xref:fundamentals/logging/index) può consentire di risolvere questi tipi di errori.</span><span class="sxs-lookup"><span data-stu-id="832eb-246">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="832eb-247">Limiti di avvio predefiniti</span><span class="sxs-lookup"><span data-stu-id="832eb-247">Default startup limits</span></span>

<span data-ttu-id="832eb-248">Il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) è configurato con un valore predefinito di *startupTimeLimit* di 120 secondi.</span><span class="sxs-lookup"><span data-stu-id="832eb-248">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="832eb-249">Quando si mantiene il valore predefinito, l'avvio di un'app potrebbe richiedere fino a due minuti prima che il modulo registri un errore del processo.</span><span class="sxs-lookup"><span data-stu-id="832eb-249">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="832eb-250">Per informazioni sulla configurazione del modulo, vedere [Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="832eb-250">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="832eb-251">Risolvere i problemi relativi al servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="832eb-251">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="832eb-252">Registro eventi applicazioni (servizio app Azure)</span><span class="sxs-lookup"><span data-stu-id="832eb-252">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="832eb-253">Per accedere al log eventi dell'applicazione, usare il pannello **Diagnostica e risolvi i problemi** nel portale di Azure:</span><span class="sxs-lookup"><span data-stu-id="832eb-253">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="832eb-254">Nel portale di Azure aprire l'app in **Servizi app**.</span><span class="sxs-lookup"><span data-stu-id="832eb-254">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="832eb-255">Selezionare **Diagnostica e risolvi i problemi**.</span><span class="sxs-lookup"><span data-stu-id="832eb-255">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="832eb-256">Selezionare l'intestazione **Strumenti di diagnostica**.</span><span class="sxs-lookup"><span data-stu-id="832eb-256">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="832eb-257">In **Strumenti di supporto** selezionare il pulsante **Eventi dell'applicazione**.</span><span class="sxs-lookup"><span data-stu-id="832eb-257">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="832eb-258">Esaminare l'errore più recente dalla voce *IIS AspNetCoreModule* o *IIS AspNetCoreModule V2* nella colonna **Origine**.</span><span class="sxs-lookup"><span data-stu-id="832eb-258">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="832eb-259">Un'alternativa all'uso del pannello **Diagnostica e risolvi i problemi** consiste nell'esaminare direttamente il file del log eventi dell'applicazione usando [Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="832eb-259">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="832eb-260">Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="832eb-260">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="832eb-261">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="832eb-261">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="832eb-262">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="832eb-262">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="832eb-263">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="832eb-263">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="832eb-264">Aprire la cartella **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="832eb-264">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="832eb-265">Selezionare l'icona della matita accanto al file *eventlog.xml*.</span><span class="sxs-lookup"><span data-stu-id="832eb-265">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="832eb-266">Esaminare il log.</span><span class="sxs-lookup"><span data-stu-id="832eb-266">Examine the log.</span></span> <span data-ttu-id="832eb-267">Scorrere fino alla fine del log per visualizzare gli eventi più recenti.</span><span class="sxs-lookup"><span data-stu-id="832eb-267">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="832eb-268">Eseguire l'app nella console Kudu</span><span class="sxs-lookup"><span data-stu-id="832eb-268">Run the app in the Kudu console</span></span>

<span data-ttu-id="832eb-269">Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-269">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="832eb-270">È possibile eseguire l'app nella console di esecuzione remota [Kudu](https://github.com/projectkudu/kudu/wiki) per individuare l'errore:</span><span class="sxs-lookup"><span data-stu-id="832eb-270">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="832eb-271">Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="832eb-271">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="832eb-272">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="832eb-272">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="832eb-273">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="832eb-273">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="832eb-274">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="832eb-274">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="832eb-275">Test di un'app a 32 bit (x86)</span><span class="sxs-lookup"><span data-stu-id="832eb-275">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="832eb-276">**Versione corrente**</span><span class="sxs-lookup"><span data-stu-id="832eb-276">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="832eb-277">Eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="832eb-277">Run the app:</span></span>
   * <span data-ttu-id="832eb-278">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="832eb-278">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="832eb-279">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="832eb-279">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="832eb-280">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.</span><span class="sxs-lookup"><span data-stu-id="832eb-280">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="832eb-281">**Distribuzione dipendente dal Framework in esecuzione in una versione di anteprima**</span><span class="sxs-lookup"><span data-stu-id="832eb-281">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="832eb-282">*Richiede l'installazione dell'estensione del sito del runtime ASP.NET Core {VERSION} (x86).*</span><span class="sxs-lookup"><span data-stu-id="832eb-282">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="832eb-283">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` è la versione di runtime)</span><span class="sxs-lookup"><span data-stu-id="832eb-283">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="832eb-284">Eseguire l'app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="832eb-284">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="832eb-285">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.</span><span class="sxs-lookup"><span data-stu-id="832eb-285">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="832eb-286">Test di un'app a 64 bit (x64)</span><span class="sxs-lookup"><span data-stu-id="832eb-286">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="832eb-287">**Versione corrente**</span><span class="sxs-lookup"><span data-stu-id="832eb-287">**Current release**</span></span>

* <span data-ttu-id="832eb-288">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) a 64 bit (x64):</span><span class="sxs-lookup"><span data-stu-id="832eb-288">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="832eb-289">Eseguire l'app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="832eb-289">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="832eb-290">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="832eb-290">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="832eb-291">Eseguire l'app: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="832eb-291">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="832eb-292">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.</span><span class="sxs-lookup"><span data-stu-id="832eb-292">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="832eb-293">**Distribuzione dipendente dal Framework in esecuzione in una versione di anteprima**</span><span class="sxs-lookup"><span data-stu-id="832eb-293">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="832eb-294">*Richiede l'installazione dell'estensione del sito del runtime ASP.NET Core {VERSION} (x64).*</span><span class="sxs-lookup"><span data-stu-id="832eb-294">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="832eb-295">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` è la versione di runtime)</span><span class="sxs-lookup"><span data-stu-id="832eb-295">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="832eb-296">Eseguire l'app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="832eb-296">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="832eb-297">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.</span><span class="sxs-lookup"><span data-stu-id="832eb-297">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="832eb-298">Log stdout del modulo ASP.NET Core (servizio app Azure)</span><span class="sxs-lookup"><span data-stu-id="832eb-298">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="832eb-299">Il log stdout del modulo ASP.NET Core spesso registra utili messaggi di errore non disponibili nel log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-299">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="832eb-300">Per abilitare e visualizzare i log stdout:</span><span class="sxs-lookup"><span data-stu-id="832eb-300">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="832eb-301">Passare al pannello **Diagnostica e risolvi i problemi** nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="832eb-301">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="832eb-302">In **SELECT PROBLEM CATEGORY** (SELEZIONARE LA CATEGORIA DEL PROBLEMA) selezionare il pulsante **Web App Down** (App Web inattiva).</span><span class="sxs-lookup"><span data-stu-id="832eb-302">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="832eb-303">In **Suggested Solutions** (Soluzioni consigliate)  > **Enable Stdout Log Redirection** (Abilita il reindirizzamento del log Stdout) selezionare il pulsante **Open Kudu Console to edit Web.Config** (Apri la console Kudu per modificare Web.Config).</span><span class="sxs-lookup"><span data-stu-id="832eb-303">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="832eb-304">Nella **console diagnostica** Kudu aprire le cartelle nel percorso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="832eb-304">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="832eb-305">Scorrere verso il basso fino a visualizzare il file *web.config* in fondo all'elenco.</span><span class="sxs-lookup"><span data-stu-id="832eb-305">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="832eb-306">Fare clic sull'icona della matita accanto al file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="832eb-306">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="832eb-307">Impostare **stdoutLogEnabled** su `true` e cambiare il percorso di **stdoutLogFile** in: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="832eb-307">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="832eb-308">Selezionare **Salva** per salvare il file *web.config* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="832eb-308">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="832eb-309">Effettuare una richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-309">Make a request to the app.</span></span>
1. <span data-ttu-id="832eb-310">Tornare al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="832eb-310">Return to the Azure portal.</span></span> <span data-ttu-id="832eb-311">Selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**.</span><span class="sxs-lookup"><span data-stu-id="832eb-311">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="832eb-312">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="832eb-312">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="832eb-313">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="832eb-313">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="832eb-314">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="832eb-314">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="832eb-315">Selezionare la cartella **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="832eb-315">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="832eb-316">Controllare la colonna **Data modifica** e selezionare l'icona della matita per modificare il log stdout con la data di modifica più recente.</span><span class="sxs-lookup"><span data-stu-id="832eb-316">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="832eb-317">Quando si apre il file di log, verrà visualizzato l'errore.</span><span class="sxs-lookup"><span data-stu-id="832eb-317">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="832eb-318">Al termine della risoluzione dei problemi, disabilitare la registrazione stdout:</span><span class="sxs-lookup"><span data-stu-id="832eb-318">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="832eb-319">Nella **console diagnostica** Kudu tornare al percorso **site** > **wwwroot** per visualizzare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="832eb-319">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="832eb-320">Aprire nuovamente il file **web.config** selezionando l'icona della matita.</span><span class="sxs-lookup"><span data-stu-id="832eb-320">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="832eb-321">Impostare **stdoutLogEnabled** su `false`.</span><span class="sxs-lookup"><span data-stu-id="832eb-321">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="832eb-322">Selezionare **Salva** per salvare il file.</span><span class="sxs-lookup"><span data-stu-id="832eb-322">Select **Save** to save the file.</span></span>

<span data-ttu-id="832eb-323">Per ulteriori informazioni, vedere <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="832eb-323">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="832eb-324">La mancata disabilitazione del log stdout può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="832eb-324">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="832eb-325">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="832eb-325">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="832eb-326">Usare la registrazione stdout solo per la risoluzione dei problemi di avvio delle app.</span><span class="sxs-lookup"><span data-stu-id="832eb-326">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="832eb-327">Per la registrazione generale in un'app ASP.NET Core dopo l'avvio, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-327">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="832eb-328">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="832eb-328">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="832eb-329">Log di debug del modulo ASP.NET Core (servizio app Azure)</span><span class="sxs-lookup"><span data-stu-id="832eb-329">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="832eb-330">Il log di debug del modulo ASP.NET Core fornisce dati di registrazione aggiuntivi e più approfonditi dal modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="832eb-330">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="832eb-331">Per abilitare e visualizzare i log stdout:</span><span class="sxs-lookup"><span data-stu-id="832eb-331">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="832eb-332">Per abilitare il log di diagnostica avanzato, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="832eb-332">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="832eb-333">Seguire le istruzioni in [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) per configurare l'app per la registrazione di diagnostica avanzata.</span><span class="sxs-lookup"><span data-stu-id="832eb-333">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="832eb-334">Ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-334">Redeploy the app.</span></span>
   * <span data-ttu-id="832eb-335">Aggiungere le impostazioni `<handlerSettings>` indicate in [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) al file *web.config* dell'app distribuita, usando la console Kudu:</span><span class="sxs-lookup"><span data-stu-id="832eb-335">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="832eb-336">Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="832eb-336">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="832eb-337">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="832eb-337">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="832eb-338">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="832eb-338">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="832eb-339">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="832eb-339">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="832eb-340">Aprire le cartelle nel percorso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="832eb-340">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="832eb-341">Modificare il file *web.config* selezionando il pulsante a forma di matita.</span><span class="sxs-lookup"><span data-stu-id="832eb-341">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="832eb-342">Aggiungere la sezione `<handlerSettings>` come illustrato in [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="832eb-342">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="832eb-343">Selezionare il pulsante **Salva**.</span><span class="sxs-lookup"><span data-stu-id="832eb-343">Select the **Save** button.</span></span>
1. <span data-ttu-id="832eb-344">Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="832eb-344">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="832eb-345">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="832eb-345">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="832eb-346">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="832eb-346">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="832eb-347">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="832eb-347">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="832eb-348">Aprire le cartelle nel percorso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="832eb-348">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="832eb-349">Se non è stato specificato un percorso per il file *aspnetcore-debug.log*, il file viene visualizzato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="832eb-349">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="832eb-350">Se è stato specificato un percorso, passare al percorso del file di log.</span><span class="sxs-lookup"><span data-stu-id="832eb-350">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="832eb-351">Aprire il file di log con il pulsante a forma di matita accanto al nome del file.</span><span class="sxs-lookup"><span data-stu-id="832eb-351">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="832eb-352">Al termine della risoluzione dei problemi, disabilitare la registrazione di debug:</span><span class="sxs-lookup"><span data-stu-id="832eb-352">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="832eb-353">Per disabilitare il log di debug avanzato, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="832eb-353">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="832eb-354">Rimuovere `<handlerSettings>` dal file *web.config* in locale e ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-354">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="832eb-355">Usare la console Kudu per modificare il file *web.config* e rimuovere la sezione `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="832eb-355">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="832eb-356">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="832eb-356">Save the file.</span></span>

<span data-ttu-id="832eb-357">Per ulteriori informazioni, vedere <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="832eb-357">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="832eb-358">La mancata disabilitazione del log di debug può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="832eb-358">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="832eb-359">Non è previsto alcun limite per le dimensioni del file di log.</span><span class="sxs-lookup"><span data-stu-id="832eb-359">There's no limit on log file size.</span></span> <span data-ttu-id="832eb-360">Usare solo la registrazione di debug per la risoluzione dei problemi di avvio delle app.</span><span class="sxs-lookup"><span data-stu-id="832eb-360">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="832eb-361">Per la registrazione generale in un'app ASP.NET Core dopo l'avvio, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-361">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="832eb-362">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="832eb-362">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="832eb-363">App lenta o sospesa (servizio app Azure)</span><span class="sxs-lookup"><span data-stu-id="832eb-363">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="832eb-364">Quando un'app risponde lentamente o si blocca durante una richiesta, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="832eb-364">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* <span data-ttu-id="832eb-365">[Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) (Risoluzione dei problemi di prestazioni delle app Web lente in Servizio app di Azure)</span><span class="sxs-lookup"><span data-stu-id="832eb-365">[Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)</span></span>
* <span data-ttu-id="832eb-366">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) (Usare l'estensione del sito Crash Diagnoser per acquisire il dump per i problemi di eccezioni intermittenti o i problemi di prestazioni nell'app Web di Azure)</span><span class="sxs-lookup"><span data-stu-id="832eb-366">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="832eb-367">Pannelli di monitoraggio</span><span class="sxs-lookup"><span data-stu-id="832eb-367">Monitoring blades</span></span>

<span data-ttu-id="832eb-368">I pannelli di monitoraggio forniscono un'esperienza di risoluzione dei problemi alternativa ai metodi descritti in precedenza in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="832eb-368">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="832eb-369">È possibile usare questi pannelli per diagnosticare gli errori della serie 500.</span><span class="sxs-lookup"><span data-stu-id="832eb-369">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="832eb-370">Verificare che le estensioni di ASP.NET Core siano installate.</span><span class="sxs-lookup"><span data-stu-id="832eb-370">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="832eb-371">Se le estensioni non sono installate, installarle manualmente:</span><span class="sxs-lookup"><span data-stu-id="832eb-371">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="832eb-372">Nella sezione del pannello **STRUMENTI DI SVILUPPO** selezionare il pannello **Estensioni**.</span><span class="sxs-lookup"><span data-stu-id="832eb-372">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="832eb-373">Nell'elenco dovrebbe essere visualizzato **ASP.NET Core Extensions** (Estensioni ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="832eb-373">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="832eb-374">Se le estensioni non sono installate, selezionare il pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="832eb-374">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="832eb-375">Scegliere **ASP.NET Core Extensions** (Estensioni ASP.NET Core) dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="832eb-375">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="832eb-376">Selezionare **OK** per accettare le condizioni legali.</span><span class="sxs-lookup"><span data-stu-id="832eb-376">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="832eb-377">Selezionare **OK** nel pannello **Aggiungi estensione**.</span><span class="sxs-lookup"><span data-stu-id="832eb-377">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="832eb-378">Un messaggio popup informativo indica quando le estensioni sono state installate correttamente.</span><span class="sxs-lookup"><span data-stu-id="832eb-378">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="832eb-379">Se la registrazione stdout non è abilitata, attenersi ai passaggi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="832eb-379">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="832eb-380">Nel portale di Azure selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**.</span><span class="sxs-lookup"><span data-stu-id="832eb-380">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="832eb-381">Selezionare il pulsante **Vai&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="832eb-381">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="832eb-382">Verrà aperta la console Kudu in una nuova scheda o finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="832eb-382">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="832eb-383">Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.</span><span class="sxs-lookup"><span data-stu-id="832eb-383">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="832eb-384">Aprire le cartelle nel percorso **site** > **wwwroot** e scorrere verso il basso fino a visualizzare il file *web.config* in fondo all'elenco.</span><span class="sxs-lookup"><span data-stu-id="832eb-384">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="832eb-385">Fare clic sull'icona della matita accanto al file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="832eb-385">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="832eb-386">Impostare **stdoutLogEnabled** su `true` e cambiare il percorso di **stdoutLogFile** in: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="832eb-386">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="832eb-387">Selezionare **Salva** per salvare il file *web.config* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="832eb-387">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="832eb-388">Passare all'attivazione della registrazione diagnostica:</span><span class="sxs-lookup"><span data-stu-id="832eb-388">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="832eb-389">Nel portale di Azure selezionare il pannello **Log di diagnostica**.</span><span class="sxs-lookup"><span data-stu-id="832eb-389">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="832eb-390">Selezionare l'interruttore **Attivato** per **Registrazione applicazioni (file system)** e **Messaggi di errore dettagliati**.</span><span class="sxs-lookup"><span data-stu-id="832eb-390">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="832eb-391">Selezionare il pulsante **Salva** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="832eb-391">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="832eb-392">Per includere la traccia delle richieste non riuscite, anche nota come registrazione FREB (Failed Request Event Buffering), selezionare l'interruttore **Attivato** per **Traccia delle richieste non riuscite**.</span><span class="sxs-lookup"><span data-stu-id="832eb-392">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="832eb-393">Selezionare il pannello **Flusso di registrazione**, immediatamente sotto il pannello **Log di diagnostica** nel portale.</span><span class="sxs-lookup"><span data-stu-id="832eb-393">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="832eb-394">Effettuare una richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-394">Make a request to the app.</span></span>
1. <span data-ttu-id="832eb-395">Nei dati del flusso di registrazione viene indicata la causa dell'errore.</span><span class="sxs-lookup"><span data-stu-id="832eb-395">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="832eb-396">Al termine della risoluzione dei problemi, assicurarsi di disabilitare la registrazione stdout.</span><span class="sxs-lookup"><span data-stu-id="832eb-396">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="832eb-397">Per visualizzare i log di traccia delle richieste non riuscite (log FREB):</span><span class="sxs-lookup"><span data-stu-id="832eb-397">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="832eb-398">Passare al pannello **Diagnostica e risolvi i problemi** nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="832eb-398">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="832eb-399">Selezionare **Failed Request Tracing Logs** (Log di traccia delle richieste non riuscite) nell'area **STRUMENTI DI SUPPORTO** della barra laterale.</span><span class="sxs-lookup"><span data-stu-id="832eb-399">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="832eb-400">Per altre informazioni, vedere la [sezione relativa alla traccia delle richieste non riuscite nell'argomento Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) e [Domande frequenti sulle prestazioni delle applicazioni in App Web di Azure: Come si abilita la traccia delle richieste non riuscite?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing).</span><span class="sxs-lookup"><span data-stu-id="832eb-400">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="832eb-401">Per altre informazioni, vedere [Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="832eb-401">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="832eb-402">La mancata disabilitazione del log stdout può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="832eb-402">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="832eb-403">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="832eb-403">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="832eb-404">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-404">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="832eb-405">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="832eb-405">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="832eb-406">Risoluzione dei problemi in IIS</span><span class="sxs-lookup"><span data-stu-id="832eb-406">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="832eb-407">Registro eventi applicazioni (IIS)</span><span class="sxs-lookup"><span data-stu-id="832eb-407">Application Event Log (IIS)</span></span>

<span data-ttu-id="832eb-408">Accedere al log eventi dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="832eb-408">Access the Application Event Log:</span></span>

1. <span data-ttu-id="832eb-409">Aprire il menu Start, cercare *Visualizzatore eventi*e selezionare l'app **Visualizzatore eventi** .</span><span class="sxs-lookup"><span data-stu-id="832eb-409">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="832eb-410">In **Visualizzatore eventi** aprire il nodo **Registri di Windows**.</span><span class="sxs-lookup"><span data-stu-id="832eb-410">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="832eb-411">Selezionare **Applicazione** per aprire il log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-411">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="832eb-412">Cercare gli errori associati all'app in cui si è verificato il problema.</span><span class="sxs-lookup"><span data-stu-id="832eb-412">Search for errors associated with the failing app.</span></span> <span data-ttu-id="832eb-413">Gli errori presentano un valore *Modulo AspNetCore IIS* o *Modulo AspNetCore IIS Express* nella colonna *Origine*.</span><span class="sxs-lookup"><span data-stu-id="832eb-413">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="832eb-414">Eseguire l'app da un prompt dei comandi</span><span class="sxs-lookup"><span data-stu-id="832eb-414">Run the app at a command prompt</span></span>

<span data-ttu-id="832eb-415">Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-415">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="832eb-416">È possibile individuare la causa di alcuni errori eseguendo l'app da un prompt dei comandi nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="832eb-416">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="832eb-417">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="832eb-417">Framework-dependent deployment</span></span>

<span data-ttu-id="832eb-418">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="832eb-418">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="832eb-419">Da un prompt dei comandi passare alla cartella di distribuzione e avviare l'app eseguendo l'assembly dell'app con *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="832eb-419">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="832eb-420">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="832eb-420">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="832eb-421">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="832eb-421">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="832eb-422">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="832eb-422">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="832eb-423">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="832eb-423">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="832eb-424">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-424">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="832eb-425">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="832eb-425">Self-contained deployment</span></span>

<span data-ttu-id="832eb-426">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="832eb-426">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="832eb-427">Da un prompt dei comandi passare alla cartella di distribuzione e avviare il file eseguibile dell'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-427">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="832eb-428">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="832eb-428">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="832eb-429">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="832eb-429">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="832eb-430">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="832eb-430">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="832eb-431">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="832eb-431">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="832eb-432">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-432">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="832eb-433">Log stdout del modulo ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="832eb-433">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="832eb-434">Per abilitare e visualizzare i log stdout:</span><span class="sxs-lookup"><span data-stu-id="832eb-434">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="832eb-435">Passare alla cartella di distribuzione del sito nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="832eb-435">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="832eb-436">Se la cartella *logs* non è presente, creare la cartella.</span><span class="sxs-lookup"><span data-stu-id="832eb-436">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="832eb-437">Per istruzioni su come impostare MSBuild per la creazione automatica della cartella *logs* nella distribuzione, vedere l'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="832eb-437">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="832eb-438">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="832eb-438">Edit the *web.config* file.</span></span> <span data-ttu-id="832eb-439">Impostare **stdoutLogEnabled** su `true` e modificare il percorso di **stdoutLogFile** in modo da fare riferimento alla cartella *logs*, ad esempio `.\logs\stdout`.</span><span class="sxs-lookup"><span data-stu-id="832eb-439">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="832eb-440">`stdout` nel percorso è il prefisso del nome del file di log.</span><span class="sxs-lookup"><span data-stu-id="832eb-440">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="832eb-441">Un timestamp, l'ID del processo e l'estensione del file vengono aggiunti automaticamente al momento della creazione del log.</span><span class="sxs-lookup"><span data-stu-id="832eb-441">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="832eb-442">Usando `stdout` come prefisso del nome del file, un tipico file di log è denominato *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="832eb-442">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="832eb-443">Assicurarsi che l'identità del pool di applicazioni disponga delle autorizzazioni di scrittura per la cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="832eb-443">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="832eb-444">Salvare il file *web.config* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="832eb-444">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="832eb-445">Effettuare una richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-445">Make a request to the app.</span></span>
1. <span data-ttu-id="832eb-446">Passare alla cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="832eb-446">Navigate to the *logs* folder.</span></span> <span data-ttu-id="832eb-447">Trovare e aprire il log stdout più recente.</span><span class="sxs-lookup"><span data-stu-id="832eb-447">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="832eb-448">Esaminare il log per verificare se sono presenti errori.</span><span class="sxs-lookup"><span data-stu-id="832eb-448">Study the log for errors.</span></span>

<span data-ttu-id="832eb-449">Al termine della risoluzione dei problemi, disabilitare la registrazione stdout:</span><span class="sxs-lookup"><span data-stu-id="832eb-449">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="832eb-450">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="832eb-450">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="832eb-451">Impostare **stdoutLogEnabled** su `false`.</span><span class="sxs-lookup"><span data-stu-id="832eb-451">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="832eb-452">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="832eb-452">Save the file.</span></span>

<span data-ttu-id="832eb-453">Per ulteriori informazioni, vedere <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="832eb-453">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="832eb-454">La mancata disabilitazione del log stdout può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="832eb-454">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="832eb-455">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="832eb-455">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="832eb-456">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="832eb-456">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="832eb-457">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="832eb-457">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="832eb-458">Log di debug del modulo ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="832eb-458">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="832eb-459">Aggiungere le impostazioni del gestore seguenti al file *Web. config* dell'app per abilitare il log di debug del modulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="832eb-459">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="832eb-460">Verificare che il percorso specificato per il log esista e che l'identità del pool di applicazioni abbia le autorizzazioni di scrittura nel percorso.</span><span class="sxs-lookup"><span data-stu-id="832eb-460">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="832eb-461">Per ulteriori informazioni, vedere <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="832eb-461">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="832eb-462">Abilitare la pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="832eb-462">Enable the Developer Exception Page</span></span>

<span data-ttu-id="832eb-463">Per eseguire l'app nell'ambiente di sviluppo, [è possibile aggiungere la variabile di ambiente `ASPNETCORE_ENVIRONMENT` a Web. config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) .</span><span class="sxs-lookup"><span data-stu-id="832eb-463">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="832eb-464">Purché l'ambiente non sia sottoposto a override durante l'avvio dell'app tramite `UseEnvironment` nel generatore di host, l'impostazione della variabile di ambiente consente di visualizzare la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) quando viene eseguita l'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-464">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

<span data-ttu-id="832eb-465">L'impostazione della variabile di ambiente per `ASPNETCORE_ENVIRONMENT` è consigliata solo per l'uso in server di gestione temporanea e test non esposti a Internet.</span><span class="sxs-lookup"><span data-stu-id="832eb-465">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="832eb-466">Al termine della risoluzione dei problemi, rimuovere la variabile di ambiente dal file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="832eb-466">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="832eb-467">Per informazioni sull'impostazione delle variabili di ambiente in *web.config*, vedere [elemento figlio environmentVariables di aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="832eb-467">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="832eb-468">Ottenere dati da un'app</span><span class="sxs-lookup"><span data-stu-id="832eb-468">Obtain data from an app</span></span>

<span data-ttu-id="832eb-469">Se un'app è in grado di rispondere alle richieste, è possibile ottenere dati sulle richieste, le connessioni e altri dati dall'app tramite middleware inline di terminale.</span><span class="sxs-lookup"><span data-stu-id="832eb-469">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="832eb-470">Per altre informazioni e codice di esempio, vedere <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="832eb-470">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="832eb-471">App lenta o sospesa (IIS)</span><span class="sxs-lookup"><span data-stu-id="832eb-471">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="832eb-472">Un *dump di arresto anomalo* del sistema è uno snapshot della memoria del sistema e può contribuire a determinare la provocazione di un arresto anomalo dell'app, dell'avvio o dell'applicazione lenta.</span><span class="sxs-lookup"><span data-stu-id="832eb-472">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="832eb-473">Arresto anomalo o eccezione di un'app</span><span class="sxs-lookup"><span data-stu-id="832eb-473">App crashes or encounters an exception</span></span>

<span data-ttu-id="832eb-474">Ottenere e analizzare un dump da [Segnalazione errori Windows](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="832eb-474">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="832eb-475">Creare una cartella per i file dump di arresto anomalo del sistema in `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="832eb-475">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="832eb-476">Il pool di app deve avere accesso in scrittura alla cartella.</span><span class="sxs-lookup"><span data-stu-id="832eb-476">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="832eb-477">Eseguire lo [script di PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="832eb-477">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="832eb-478">Se l'app usa il [modello di hosting in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="832eb-478">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="832eb-479">Se l'app usa il [modello di hosting out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="832eb-479">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="832eb-480">Eseguire l'app nelle condizioni che causano l'arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="832eb-480">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="832eb-481">Dopo che si è verificato l'arresto anomalo, eseguire lo [script di PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="832eb-481">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="832eb-482">Se l'app usa il [modello di hosting in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="832eb-482">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="832eb-483">Se l'app usa il [modello di hosting out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="832eb-483">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="832eb-484">Dopo l'arresto anomalo di un'app e la raccolta dei dump, l'app può terminare normalmente.</span><span class="sxs-lookup"><span data-stu-id="832eb-484">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="832eb-485">Lo script di PowerShell configura Segnalazione errori Windows per raccogliere fino a cinque dump per ogni app.</span><span class="sxs-lookup"><span data-stu-id="832eb-485">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="832eb-486">I dump di arresto anomalo del sistema potrebbero richiedere una grande quantità di spazio su disco (fino a diversi gigabyte ognuno).</span><span class="sxs-lookup"><span data-stu-id="832eb-486">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="832eb-487">L'app si blocca, si verifica un errore durante l'avvio o viene eseguita normalmente</span><span class="sxs-lookup"><span data-stu-id="832eb-487">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="832eb-488">Quando un'app si *blocca* (smette di rispondere ma non si arresta in modo anomalo), si verifica un errore durante l'avvio o viene eseguita normalmente, vedere [file di dump in modalità utente: scegliere lo strumento migliore](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) per selezionare uno strumento appropriato per produrre il dump.</span><span class="sxs-lookup"><span data-stu-id="832eb-488">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="832eb-489">Analizzare il dump</span><span class="sxs-lookup"><span data-stu-id="832eb-489">Analyze the dump</span></span>

<span data-ttu-id="832eb-490">È possibile analizzare un dump usando diversi approcci.</span><span class="sxs-lookup"><span data-stu-id="832eb-490">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="832eb-491">Per altre informazioni, vedere [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analisi di un file dump in modalità utente).</span><span class="sxs-lookup"><span data-stu-id="832eb-491">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="832eb-492">Cancella cache di pacchetti</span><span class="sxs-lookup"><span data-stu-id="832eb-492">Clear package caches</span></span>

<span data-ttu-id="832eb-493">Un'app funzionante potrebbe non riuscire immediatamente dopo l'aggiornamento del .NET Core SDK nel computer di sviluppo o la modifica delle versioni del pacchetto all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-493">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="832eb-494">In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali.</span><span class="sxs-lookup"><span data-stu-id="832eb-494">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="832eb-495">La maggior parte di questi problemi può essere risolta attenendosi alle istruzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="832eb-495">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="832eb-496">Eliminare le cartelle *bin* e *obj*.</span><span class="sxs-lookup"><span data-stu-id="832eb-496">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="832eb-497">Cancellare le cache dei pacchetti eseguendo [le impostazioni locali di DotNet NuGet All--Clear](/dotnet/core/tools/dotnet-nuget-locals) da una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="832eb-497">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="832eb-498">La cancellazione delle cache dei pacchetti può essere eseguita anche con lo strumento [NuGet. exe](https://www.nuget.org/downloads) ed eseguendo il comando `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="832eb-498">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="832eb-499">*nuget.exe* non è un'installazione inclusa con il sistema operativo desktop Windows e deve essere ottenuta separatamente dal [sito Web NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="832eb-499">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="832eb-500">Ripristinare e ricompilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="832eb-500">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="832eb-501">Eliminare tutti i file nella cartella di distribuzione nel server prima di ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="832eb-501">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="832eb-502">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="832eb-502">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="832eb-503">Documentazione di Azure</span><span class="sxs-lookup"><span data-stu-id="832eb-503">Azure documentation</span></span>

* [<span data-ttu-id="832eb-504">Application Insights per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="832eb-504">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="832eb-505">Sezione app Web per il debug remoto di risoluzione dei problemi di un'app Web nel servizio app Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="832eb-505">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="832eb-506">Panoramica della diagnostica del servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="832eb-506">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="832eb-507">Procedura: Eseguire il monitoraggio delle app nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="832eb-507">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="832eb-508">Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio</span><span class="sxs-lookup"><span data-stu-id="832eb-508">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="832eb-509">Risolvere gli errori HTTP "502 - Gateway non valido" e "503 - Servizio non disponibile" nelle App Web di Azure</span><span class="sxs-lookup"><span data-stu-id="832eb-509">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* <span data-ttu-id="832eb-510">[Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) (Risoluzione dei problemi di prestazioni delle app Web lente in Servizio app di Azure)</span><span class="sxs-lookup"><span data-stu-id="832eb-510">[Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)</span></span>
* [<span data-ttu-id="832eb-511">Domande frequenti sulle prestazioni delle applicazioni in App Web di Azure</span><span class="sxs-lookup"><span data-stu-id="832eb-511">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="832eb-512">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Sandbox per app Web di Azure - Limitazioni di esecuzione di runtime di Servizio app di Azure)</span><span class="sxs-lookup"><span data-stu-id="832eb-512">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* <span data-ttu-id="832eb-513">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)</span><span class="sxs-lookup"><span data-stu-id="832eb-513">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)</span></span>

### <a name="visual-studio-documentation"></a><span data-ttu-id="832eb-514">Documentazione di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="832eb-514">Visual Studio documentation</span></span>

* [<span data-ttu-id="832eb-515">ASP.NET Core di debug remoto in IIS in Azure in Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="832eb-515">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="832eb-516">ASP.NET Core di debug remoto in un computer IIS remoto in Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="832eb-516">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="832eb-517">Informazioni sul debug tramite Visual Studio</span><span class="sxs-lookup"><span data-stu-id="832eb-517">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="832eb-518">Documentazione di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="832eb-518">Visual Studio Code documentation</span></span>

* <span data-ttu-id="832eb-519">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) (Debug con Visual Studio Code)</span><span class="sxs-lookup"><span data-stu-id="832eb-519">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)</span></span>
