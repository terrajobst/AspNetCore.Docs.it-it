---
title: Distribuire le app ASP.NET Core in Servizio app di Azure
author: guardrex
description: Questo articolo contiene collegamenti a risorse di hosting e distribuzione di Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/28/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 5035a31526e0290964e0fdee05753aeaf6cb3790
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602444"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="ac2be-103">Distribuire le app ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ac2be-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="ac2be-104">Il [servizio app di Azure](https://azure.microsoft.com/services/app-service/) è un [servizio di piattaforma di cloud computing Microsoft](https://azure.microsoft.com/) per l'hosting di app Web, inclusa ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac2be-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="ac2be-105">Risorse utili</span><span class="sxs-lookup"><span data-stu-id="ac2be-105">Useful resources</span></span>

<span data-ttu-id="ac2be-106">[Documentazione del servizio app](/azure/app-service/) è la home page di documentazione, esercitazioni, esempi, guide introduttive e altre risorse per le app di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac2be-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="ac2be-107">Due importanti esercitazioni relative all'hosting di app ASP.NET Core sono:</span><span class="sxs-lookup"><span data-stu-id="ac2be-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="ac2be-108">Creare un'app Web ASP.NET Core in Azure</span><span class="sxs-lookup"><span data-stu-id="ac2be-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="ac2be-109">Usare Visual Studio per creare e distribuire un'app Web ASP.NET Core nel servizio app di Azure in Windows.</span><span class="sxs-lookup"><span data-stu-id="ac2be-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="ac2be-110">Creare un'app ASP.NET Core nel Servizio app in Linux</span><span class="sxs-lookup"><span data-stu-id="ac2be-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="ac2be-111">Usare la riga di comando per creare e distribuire un'app Web ASP.NET Core nel servizio app di Azure in Linux.</span><span class="sxs-lookup"><span data-stu-id="ac2be-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="ac2be-112">Gli articoli seguenti sono disponibili nella documentazione di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ac2be-112">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="ac2be-113">Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac2be-113">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="ac2be-114">Informazioni su come creare un'app Web ASP.NET Core tramite Visual Studio e distribuirla nel Servizio app di Azure usando Git per la distribuzione continua.</span><span class="sxs-lookup"><span data-stu-id="ac2be-114">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="ac2be-115">Creare la prima pipeline</span><span class="sxs-lookup"><span data-stu-id="ac2be-115">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="ac2be-116">Impostare una build CI per un'app ASP.NET Core e quindi creare una versione di distribuzione continua in Servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac2be-116">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

<span data-ttu-id="ac2be-117">[Azure Web App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Sandbox per app Web di Azure)</span><span class="sxs-lookup"><span data-stu-id="ac2be-117">[Azure Web App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>  
<span data-ttu-id="ac2be-118">Individuare le limitazioni di esecuzione di runtime di Servizio app di Azure applicate dalla piattaforma per le app Azure.</span><span class="sxs-lookup"><span data-stu-id="ac2be-118">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

<xref:test/troubleshoot>  
<span data-ttu-id="ac2be-119">Riconoscere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac2be-119">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="ac2be-120">Configurazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="ac2be-120">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="ac2be-121">Piattaforma</span><span class="sxs-lookup"><span data-stu-id="ac2be-121">Platform</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac2be-122">I runtime per le app a 64 bit (x64) e a 32 bit (x86) sono disponibili in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac2be-122">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="ac2be-123">[.NET Core SDK](/dotnet/core/sdk) disponibile nel servizio app è a 32 bit, ma è possibile distribuire app a 64 bit compilate in locale usando la console [Kudu](https://github.com/projectkudu/kudu/wiki) o il processo di pubblicazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac2be-123">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps built locally using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or the publish process in Visual Studio.</span></span> <span data-ttu-id="ac2be-124">Per altre informazioni, vedere la sezione [Pubblicare e distribuire l'app](#publish-and-deploy-the-app).</span><span class="sxs-lookup"><span data-stu-id="ac2be-124">For more information, see the [Publish and deploy the app](#publish-and-deploy-the-app) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac2be-125">Per le app con dipendenze native, i runtime per le app a 32 bit (x86) sono disponibili in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac2be-125">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="ac2be-126">La versione di [.NET Core SDK](/dotnet/core/sdk) disponibile nel servizio app è a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="ac2be-126">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

<span data-ttu-id="ac2be-127">Per altre informazioni sui componenti e sui metodi di distribuzione del framework .NET Core, ad esempio informazioni sul runtime di .NET Core e su .NET Core SDK, vedere [Informazioni su .NET Core: Composizione](/dotnet/core/about#composition).</span><span class="sxs-lookup"><span data-stu-id="ac2be-127">For more information on .NET Core framework components and distribution methods, such as information on the .NET Core runtime and the .NET Core SDK, see [About .NET Core: Composition](/dotnet/core/about#composition).</span></span>

### <a name="packages"></a><span data-ttu-id="ac2be-128">Pacchetti</span><span class="sxs-lookup"><span data-stu-id="ac2be-128">Packages</span></span>

<span data-ttu-id="ac2be-129">Includere i pacchetti NuGet seguenti per offrire funzionalità di registrazione automatica per le app distribuite in Servizio app di Azure:</span><span class="sxs-lookup"><span data-stu-id="ac2be-129">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="ac2be-130">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) per fornire l'integrazione di ASP.NET Core con il Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac2be-130">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="ac2be-131">La funzionalità di registrazione aggiunte vengono fornite dal pacchetto `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="ac2be-131">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="ac2be-132">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) esegue [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) per aggiungere i provider di registrazione diagnostica del servizio app di Azure nel pacchetto `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="ac2be-132">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="ac2be-133">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) offre implementazioni di logger per il supporto dei registri di diagnostica del servizio app di Azure e delle funzionalità del flusso di registrazione.</span><span class="sxs-lookup"><span data-stu-id="ac2be-133">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="ac2be-134">I pacchetti precedenti non sono disponibili dal [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ac2be-134">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="ac2be-135">Le app destinate a .NET Framework o che fanno riferimento al metapacchetto `Microsoft.AspNetCore.App` in modo esplicito deve fare riferimento ai singoli pacchetti nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac2be-135">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="ac2be-136">Eseguire l'override della configurazione delle app usando il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="ac2be-136">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="ac2be-137">Le impostazioni dell'app nel portale di Azure consentono di impostare le variabili di ambiente per l'app.</span><span class="sxs-lookup"><span data-stu-id="ac2be-137">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="ac2be-138">Le variabili di ambiente possono essere utilizzate dal [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ac2be-138">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="ac2be-139">Quando nel portale di Azure viene creata o modificata un'impostazione dell'app e viene selezionato il pulsante **Salva**, l'app Azure viene riavviata.</span><span class="sxs-lookup"><span data-stu-id="ac2be-139">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="ac2be-140">La variabile di ambiente risulterà disponibile per l'app dopo il riavvio del servizio.</span><span class="sxs-lookup"><span data-stu-id="ac2be-140">The environment variable is available to the app after the service restarts.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac2be-141">Quando un'app usa l'[host generico](xref:fundamentals/host/generic-host), le variabili di ambiente non vengono caricate nella configurazione di un'app per impostazione predefinita e il provider di configurazione deve essere aggiunto dallo sviluppatore.</span><span class="sxs-lookup"><span data-stu-id="ac2be-141">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="ac2be-142">Lo sviluppatore determina il prefisso delle variabili di ambiente quando viene aggiunto il provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="ac2be-142">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="ac2be-143">Per altre informazioni, vedere <xref:fundamentals/host/generic-host> e il [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ac2be-143">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac2be-144">Quando un'app compila l'host con [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), le variabili di ambiente che configurano l'host usano il prefisso `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="ac2be-144">When an app builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="ac2be-145">Per altre informazioni, vedere <xref:fundamentals/host/web-host> e il [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ac2be-145">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ac2be-146">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="ac2be-146">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ac2be-147">Il [middleware di integrazione IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), che consente di configurare il middleware delle intestazioni inoltrate in caso di hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), e il modulo ASP.NET Core sono configurati per inoltrare lo schema (HTTP/HTTPS) e l'indirizzo IP remoto di origine della richiesta.</span><span class="sxs-lookup"><span data-stu-id="ac2be-147">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="ac2be-148">Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro ulteriori server proxy e servizi di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="ac2be-148">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="ac2be-149">Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="ac2be-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="ac2be-150">Monitoraggio e registrazione</span><span class="sxs-lookup"><span data-stu-id="ac2be-150">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac2be-151">Le app ASP.NET Core distribuite in Servizio app di Azure ricevono automaticamente l'estensione di Servizio app **ASP.NET Core Logging Integration** (Integrazione di registrazione ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="ac2be-151">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="ac2be-152">L'estensione abilita l'integrazione della registrazione per le app ASP.NET Core in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac2be-152">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac2be-153">Le app ASP.NET Core distribuite in Servizio app ricevono automaticamente un'estensione di Servizio app, ovvero le **estensioni di registrazione di ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-153">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="ac2be-154">L'estensione abilita l'integrazione della registrazione per le app ASP.NET Core in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac2be-154">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="ac2be-155">Per informazioni sul monitoraggio, la registrazione e la risoluzione dei problemi, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="ac2be-155">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="ac2be-156">Monitorare le app nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ac2be-156">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="ac2be-157">Informazioni su come esaminare le quote e le metriche per le app e i piani del servizio app.</span><span class="sxs-lookup"><span data-stu-id="ac2be-157">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="ac2be-158">Abilitare la registrazione diagnostica per le app nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ac2be-158">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="ac2be-159">Informazioni su come abilitare e accedere alla registrazione diagnostica per i codici di stato HTTP, le richieste non riuscite e l'attività del server Web.</span><span class="sxs-lookup"><span data-stu-id="ac2be-159">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="ac2be-160">Riconoscimento degli approcci comuni di gestione degli errori nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac2be-160">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:test/troubleshoot-azure-iis>  
<span data-ttu-id="ac2be-161">Informazioni su come diagnosticare i problemi delle distribuzioni del servizio app di Azure con le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac2be-161">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="ac2be-162">Informazioni sugli errori comuni di configurazione della distribuzione per le app ospitate dal servizio app di Azure o da IIS con suggerimenti per la risoluzione.</span><span class="sxs-lookup"><span data-stu-id="ac2be-162">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="ac2be-163">KeyRing di protezione dati e slot di distribuzione</span><span class="sxs-lookup"><span data-stu-id="ac2be-163">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="ac2be-164">Le [chiavi di protezione dati](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sono salvate in modo permanente nella cartella *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="ac2be-164">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="ac2be-165">La cartella è associata all'archiviazione di rete e sincronizzata in tutti i computer che ospitano l'app.</span><span class="sxs-lookup"><span data-stu-id="ac2be-165">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="ac2be-166">Le chiavi non vengono protette quando sono inattive.</span><span class="sxs-lookup"><span data-stu-id="ac2be-166">Keys aren't protected at rest.</span></span> <span data-ttu-id="ac2be-167">La cartella offre il KeyRing a tutte le istanze di un'app in un singolo slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="ac2be-167">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="ac2be-168">Gli slot di distribuzione separati, ad esempio gli slot di gestione temporanea e di produzione, non condividono un KeyRing.</span><span class="sxs-lookup"><span data-stu-id="ac2be-168">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="ac2be-169">Nel passaggio da uno slot di distribuzione all'altro, tutti i sistemi che usano la protezione dati non saranno in grado di decrittografare i dati archiviati usando il KeyRing all'interno dello slot precedente.</span><span class="sxs-lookup"><span data-stu-id="ac2be-169">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="ac2be-170">Il middleware dei cookie di ASP.NET usa la protezione dati per proteggere i cookie.</span><span class="sxs-lookup"><span data-stu-id="ac2be-170">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="ac2be-171">Di conseguenza, gli utenti vengono disconnessi da un'app che usa il middleware dei cookie di ASP.NET standard.</span><span class="sxs-lookup"><span data-stu-id="ac2be-171">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="ac2be-172">Per una soluzione di KeyRing indipendente dallo slot, usare un provider di KeyRing esterno, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ac2be-172">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="ac2be-173">Archiviazione BLOB di Azure</span><span class="sxs-lookup"><span data-stu-id="ac2be-173">Azure Blob Storage</span></span>
* <span data-ttu-id="ac2be-174">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ac2be-174">Azure Key Vault</span></span>
* <span data-ttu-id="ac2be-175">Archivio SQL</span><span class="sxs-lookup"><span data-stu-id="ac2be-175">SQL store</span></span>
* <span data-ttu-id="ac2be-176">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="ac2be-176">Redis cache</span></span>

<span data-ttu-id="ac2be-177">Per altre informazioni, vedere <xref:security/data-protection/implementation/key-storage-providers>.</span><span class="sxs-lookup"><span data-stu-id="ac2be-177">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="ac2be-178">Distribuire la versione di anteprima di ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ac2be-178">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="ac2be-179">Usare uno degli approcci seguenti se l'app si basa su una versione di anteprima di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ac2be-179">Use one of the following approaches if the app relies on a preview release of .NET Core:</span></span>

* <span data-ttu-id="ac2be-180">[Installare l'estensione del sito di anteprima](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="ac2be-180">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="ac2be-181">[Distribuire un'app di anteprima completa](#deploy-a-self-contained-preview-app).</span><span class="sxs-lookup"><span data-stu-id="ac2be-181">[Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).</span></span>
* <span data-ttu-id="ac2be-182">[Usare Docker con app Web per contenitori](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="ac2be-182">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="ac2be-183">Installare l'estensione del sito di anteprima</span><span class="sxs-lookup"><span data-stu-id="ac2be-183">Install the preview site extension</span></span>

<span data-ttu-id="ac2be-184">Se si verifica un problema con l'estensione del sito di anteprima, aprire un [problema aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/issues).</span><span class="sxs-lookup"><span data-stu-id="ac2be-184">If a problem occurs using the preview site extension, open an [aspnet/AspNetCore issue](https://github.com/aspnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="ac2be-185">Dal portale di Azure passare al servizio app.</span><span class="sxs-lookup"><span data-stu-id="ac2be-185">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="ac2be-186">Selezionare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="ac2be-186">Select the web app.</span></span>
1. <span data-ttu-id="ac2be-187">Digitare "es" nella casella di ricerca per filtrare per "Estensioni" o scorrere l'elenco degli strumenti di gestione.</span><span class="sxs-lookup"><span data-stu-id="ac2be-187">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="ac2be-188">Selezionare **Estensioni**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-188">Select **Extensions**.</span></span>
1. <span data-ttu-id="ac2be-189">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-189">Select **Add**.</span></span>
1. <span data-ttu-id="ac2be-190">Selezionare l'estensione **ASP.NET Core {X.Y} ({x64|x86}) Runtime** nell'elenco, dove `{X.Y}` è la versione di anteprima di ASP.NET Core e `{x64|x86}` specifica la piattaforma.</span><span class="sxs-lookup"><span data-stu-id="ac2be-190">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="ac2be-191">Selezionare **OK** per accettare le condizioni legali.</span><span class="sxs-lookup"><span data-stu-id="ac2be-191">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="ac2be-192">Per installare l'estensione, selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-192">Select **OK** to install the extension.</span></span>

<span data-ttu-id="ac2be-193">Al termine dell'operazione, viene installata l'anteprima più recente di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac2be-193">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="ac2be-194">Verificare l'installazione:</span><span class="sxs-lookup"><span data-stu-id="ac2be-194">Verify the installation:</span></span>

1. <span data-ttu-id="ac2be-195">Selezionare **Strumenti avanzati**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-195">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="ac2be-196">Selezionare **Vai** in **Strumenti avanzati**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-196">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="ac2be-197">Selezionare l'elemento di menu **Console di debug** > **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-197">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="ac2be-198">Eseguire il comando seguente dal prompt di PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ac2be-198">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="ac2be-199">Sostituire la versione di runtime di ASP.NET Core in `{X.Y}` e la piattaforma in `{PLATFORM}` nel comando:</span><span class="sxs-lookup"><span data-stu-id="ac2be-199">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="ac2be-200">Il comando restituisce `True` quando è installato il runtime di anteprima x64.</span><span class="sxs-lookup"><span data-stu-id="ac2be-200">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="ac2be-201">L'architettura della piattaforma (x86 o x64) di un'app di Servizi app viene impostata nelle impostazioni dell'app nel portale di Azure per le app ospitate in un livello di hosting con calcolo di serie A o migliore.</span><span class="sxs-lookup"><span data-stu-id="ac2be-201">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="ac2be-202">Se l'app viene eseguita in modalità in-process e l'architettura della piattaforma è configurata per 64 bit (x64), il modulo ASP.NET Core usa il runtime dell'anteprima a 64 bit, se presente.</span><span class="sxs-lookup"><span data-stu-id="ac2be-202">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="ac2be-203">Installare l'estensione **ASP.NET Core {X.Y} (x64) Runtime**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-203">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension.</span></span>
>
> <span data-ttu-id="ac2be-204">Dopo aver installato il runtime di anteprima x64, eseguire il comando seguente nella finestra di comando di PowerShell di Kudu per verificare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="ac2be-204">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="ac2be-205">Sostituire la versione di runtime di ASP.NET Core per `{X.Y}` nel comando:</span><span class="sxs-lookup"><span data-stu-id="ac2be-205">Substitute the ASP.NET Core runtime version for `{X.Y}` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="ac2be-206">Il comando restituisce `True` quando è installato il runtime di anteprima x64.</span><span class="sxs-lookup"><span data-stu-id="ac2be-206">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="ac2be-207">Le **estensioni di ASP.NET Core** abilitano funzionalità aggiuntive per ASP.NET Core nei Servizi app di Azure, ad esempio la registrazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac2be-207">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="ac2be-208">L'estensione viene installata automaticamente durante la distribuzione da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac2be-208">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="ac2be-209">Se l'estensione non è installata, installarla per l'app.</span><span class="sxs-lookup"><span data-stu-id="ac2be-209">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="ac2be-210">**Usare l'estensione del sito di anteprima con un modello ARM**</span><span class="sxs-lookup"><span data-stu-id="ac2be-210">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="ac2be-211">Se per creare e distribuire le app si usa un modello ARM, è possibile usare il tipo di risorsa `siteextensions` per aggiungere l'estensione del sito a un'app Web.</span><span class="sxs-lookup"><span data-stu-id="ac2be-211">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="ac2be-212">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ac2be-212">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-a-self-contained-preview-app"></a><span data-ttu-id="ac2be-213">Distribuire un'app di anteprima autonoma</span><span class="sxs-lookup"><span data-stu-id="ac2be-213">Deploy a self-contained preview app</span></span>

<span data-ttu-id="ac2be-214">Una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) che ha come destinazione un runtime di anteprima include il runtime di anteprima nella distribuzione.</span><span class="sxs-lookup"><span data-stu-id="ac2be-214">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="ac2be-215">Per la distribuzione di un'app autonoma:</span><span class="sxs-lookup"><span data-stu-id="ac2be-215">When deploying a self-contained app:</span></span>

* <span data-ttu-id="ac2be-216">Il sito nel Servizio app di Azure non richiede l'[estensione del sito di anteprima](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="ac2be-216">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="ac2be-217">L'app deve essere pubblicata seguendo un approccio diverso rispetto alla procedura di pubblicazione per una [distribuzione dipendente dal framework](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="ac2be-217">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="ac2be-218">Seguire le istruzioni riportate nella sezione [Distribuire l'app autonoma](#deploy-the-app-self-contained).</span><span class="sxs-lookup"><span data-stu-id="ac2be-218">Follow the guidance in the [Deploy the app self-contained](#deploy-the-app-self-contained) section.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="ac2be-219">Usare Docker con app Web per contenitori</span><span class="sxs-lookup"><span data-stu-id="ac2be-219">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="ac2be-220">L'[hub Docker](https://hub.docker.com/r/microsoft/aspnetcore/) contiene le immagini di Docker più recenti per la versione di anteprima.</span><span class="sxs-lookup"><span data-stu-id="ac2be-220">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="ac2be-221">Le immagini possono essere usate come immagini di base.</span><span class="sxs-lookup"><span data-stu-id="ac2be-221">The images can be used as a base image.</span></span> <span data-ttu-id="ac2be-222">Usare l'immagine e distribuirla alle app Web per i contenitori normalmente.</span><span class="sxs-lookup"><span data-stu-id="ac2be-222">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="ac2be-223">Pubblicare e distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="ac2be-223">Publish and deploy the app</span></span>

### <a name="deploy-the-app-framework-dependent"></a><span data-ttu-id="ac2be-224">Distribuire l'app in modo dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="ac2be-224">Deploy the app framework-dependent</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac2be-225">Per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) a 64-bit :</span><span class="sxs-lookup"><span data-stu-id="ac2be-225">For a 64-bit [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

* <span data-ttu-id="ac2be-226">Usare .NET Core SDK a 64 bit per compilare un'app a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="ac2be-226">Use a 64-bit .NET Core SDK to build a 64-bit app.</span></span>
* <span data-ttu-id="ac2be-227">Impostare **Piattaforma** su **64 bit** in **Configurazione** > **Impostazioni generali** nel servizio app.</span><span class="sxs-lookup"><span data-stu-id="ac2be-227">Set the **Platform** to **64 Bit** in the App Service's **Configuration** > **General settings**.</span></span> <span data-ttu-id="ac2be-228">L'app deve usare un piano di servizio Basic o superiore per abilitare la scelta del numero di bit della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="ac2be-228">The app must use a Basic or higher service plan to enable the choice of platform bitness.</span></span>

::: moniker-end

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac2be-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac2be-229">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ac2be-230">Selezionare **Compila** > **Pubblica {nome applicazione}** dalla barra degli strumenti di Visual Studio oppure fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-230">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="ac2be-231">Nella finestra di dialogo **Selezionare una destinazione di pubblicazione.** verificare che sia selezionata la voce **Servizio app**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-231">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="ac2be-232">Selezionare **Avanzate**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-232">Select **Advanced**.</span></span> <span data-ttu-id="ac2be-233">Viene visualizzata la finestra di dialogo **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-233">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="ac2be-234">Nella finestra di dialogo **Pubblica**:</span><span class="sxs-lookup"><span data-stu-id="ac2be-234">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="ac2be-235">Verificare che sia selezionata la configurazione **Rilascio**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-235">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="ac2be-236">Aprire l'elenco a discesa **Modalità di distribuzione** e selezionare **Dipendente dal framework**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-236">Open the **Deployment Mode** drop-down list and select **Framework-Dependent**.</span></span>
   * <span data-ttu-id="ac2be-237">Selezionare **Portabile** come **Runtime di destinazione**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-237">Select **Portable** as the **Target Runtime**.</span></span>
   * <span data-ttu-id="ac2be-238">Se durante la distribuzione è necessario rimuovere i file aggiuntivi, aprire **Opzioni pubblicazione file** e selezionare la casella di controllo che consente di rimuovere i file aggiuntivi nella destinazione.</span><span class="sxs-lookup"><span data-stu-id="ac2be-238">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="ac2be-239">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-239">Select **Save**.</span></span>
1. <span data-ttu-id="ac2be-240">Creare un nuovo sito o aggiornare un sito esistente seguendo le istruzioni rimanenti della pubblicazione guidata.</span><span class="sxs-lookup"><span data-stu-id="ac2be-240">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ac2be-241">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="ac2be-241">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="ac2be-242">Nel file di progetto non specificare un [Identificatore runtime](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="ac2be-242">In the project file, don't specify a [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

1. <span data-ttu-id="ac2be-243">Da una shell dei comandi pubblicare l'app nella configurazione di versione con il comando [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="ac2be-243">From a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="ac2be-244">Nell'esempio seguente l'app viene pubblicata come app dipendente dal framework:</span><span class="sxs-lookup"><span data-stu-id="ac2be-244">In the following example, the app is published as a framework-dependent app:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="ac2be-245">Spostare il contenuto della directory *bin/Release/{TARGET FRAMEWORK}/publish* nel sito nel servizio app.</span><span class="sxs-lookup"><span data-stu-id="ac2be-245">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="ac2be-246">Se il contenuto della cartella *publish* viene trascinato dal disco rigido locale o dalla condivisione di rete direttamente nel servizio app nella console [Kudu](https://github.com/projectkudu/kudu/wiki), trascinare i file nella cartella `D:\home\site\wwwroot` nella console Kudu.</span><span class="sxs-lookup"><span data-stu-id="ac2be-246">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the [Kudu](https://github.com/projectkudu/kudu/wiki) console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="ac2be-247">Distribuire l'app autonoma</span><span class="sxs-lookup"><span data-stu-id="ac2be-247">Deploy the app self-contained</span></span>

<span data-ttu-id="ac2be-248">Usare Visual Studio o gli strumenti dell'interfaccia della riga di comando (CLI) per una [distribuzione autonoma (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="ac2be-248">Use Visual Studio or the command-line interface (CLI) tools for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac2be-249">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac2be-249">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ac2be-250">Selezionare **Compila** > **Pubblica {nome applicazione}** dalla barra degli strumenti di Visual Studio oppure fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-250">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="ac2be-251">Nella finestra di dialogo **Selezionare una destinazione di pubblicazione.** verificare che sia selezionata la voce **Servizio app**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-251">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="ac2be-252">Selezionare **Avanzate**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-252">Select **Advanced**.</span></span> <span data-ttu-id="ac2be-253">Viene visualizzata la finestra di dialogo **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-253">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="ac2be-254">Nella finestra di dialogo **Pubblica**:</span><span class="sxs-lookup"><span data-stu-id="ac2be-254">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="ac2be-255">Verificare che sia selezionata la configurazione **Rilascio**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-255">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="ac2be-256">Aprire l'elenco a discesa **Modalità di distribuzione** e selezionare **Completa**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-256">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="ac2be-257">Selezionare il runtime di destinazione dall'elenco a discesa **Runtime di destinazione**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-257">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="ac2be-258">Il valore predefinito è `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="ac2be-258">The default is `win-x86`.</span></span>
   * <span data-ttu-id="ac2be-259">Se durante la distribuzione è necessario rimuovere i file aggiuntivi, aprire **Opzioni pubblicazione file** e selezionare la casella di controllo che consente di rimuovere i file aggiuntivi nella destinazione.</span><span class="sxs-lookup"><span data-stu-id="ac2be-259">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="ac2be-260">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="ac2be-260">Select **Save**.</span></span>
1. <span data-ttu-id="ac2be-261">Creare un nuovo sito o aggiornare un sito esistente seguendo le istruzioni rimanenti della pubblicazione guidata.</span><span class="sxs-lookup"><span data-stu-id="ac2be-261">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ac2be-262">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="ac2be-262">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="ac2be-263">Nel file di progetto specificare uno o più [identificatori di runtime (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="ac2be-263">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="ac2be-264">Usare `<RuntimeIdentifier>` (singolare) per un unico RID o `<RuntimeIdentifiers>` (plurale) per specificare un elenco di RID delimitato da punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="ac2be-264">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="ac2be-265">Nell'esempio seguente è specificato il RID `win-x86`:</span><span class="sxs-lookup"><span data-stu-id="ac2be-265">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="ac2be-266">Da una shell dei comandi eseguire il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) per pubblicare l'app in configurazione Rilascio per il runtime dell'host.</span><span class="sxs-lookup"><span data-stu-id="ac2be-266">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="ac2be-267">Nell'esempio seguente l'app viene pubblicata per il RID `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="ac2be-267">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="ac2be-268">Il RID fornito per l'opzione `--runtime` deve essere specificato nella proprietà `<RuntimeIdentifier>` (o `<RuntimeIdentifiers>`) del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="ac2be-268">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. <span data-ttu-id="ac2be-269">Spostare il contenuto della directory *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* nel sito del Servizio app.</span><span class="sxs-lookup"><span data-stu-id="ac2be-269">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="ac2be-270">Se il contenuto della cartella *publish* viene trascinato dal disco rigido locale o dalla condivisione di rete direttamente nel servizio app nella console Kudu, trascinare i file nella cartella `D:\home\site\wwwroot` nella console Kudu.</span><span class="sxs-lookup"><span data-stu-id="ac2be-270">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the Kudu console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

## <a name="protocol-settings-https"></a><span data-ttu-id="ac2be-271">Impostazioni del protocollo (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="ac2be-271">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="ac2be-272">Le associazioni di protocollo protette consentono di specificare un certificato da usare per rispondere alle richieste su HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ac2be-272">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="ac2be-273">L'associazione richiede un certificato privato valido (*PFX*) rilasciato per il nome host specifico.</span><span class="sxs-lookup"><span data-stu-id="ac2be-273">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="ac2be-274">Per altre informazioni, vedere [Esercitazione: Associare un certificato SSL personalizzato esistente al servizio app di Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="ac2be-274">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="ac2be-275">Trasformare web.config</span><span class="sxs-lookup"><span data-stu-id="ac2be-275">Transform web.config</span></span>

<span data-ttu-id="ac2be-276">Se è necessario trasformare *web.config* in fase di pubblicazione (ad esempio, impostare variabili di ambiente in base a configurazione, profilo o ambiente), vedere <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="ac2be-276">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ac2be-277">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ac2be-277">Additional resources</span></span>

* [<span data-ttu-id="ac2be-278">Panoramica del servizio app</span><span class="sxs-lookup"><span data-stu-id="ac2be-278">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* <span data-ttu-id="ac2be-279">[Azure App Service: The Best Place to Host your .NET Apps ](https://channel9.msdn.com/events/dotnetConf/2017/T222) (Servizio app di Azure: la soluzione migliore per l'hosting delle app .NET) (video di 55 minuti)</span><span class="sxs-lookup"><span data-stu-id="ac2be-279">[Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)](https://channel9.msdn.com/events/dotnetConf/2017/T222)</span></span>
* <span data-ttu-id="ac2be-280">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)</span><span class="sxs-lookup"><span data-stu-id="ac2be-280">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)</span></span>
* [<span data-ttu-id="ac2be-281">Panoramica della diagnostica del servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ac2be-281">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="ac2be-282">Il servizio app di Azure in Windows Server usa [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ac2be-282">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="ac2be-283">Gli argomenti seguenti riguardano la tecnologia IIS sottostante:</span><span class="sxs-lookup"><span data-stu-id="ac2be-283">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="ac2be-284">Windows Server - Contenuti per l'amministratore IT per la versione corrente e le versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="ac2be-284">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
