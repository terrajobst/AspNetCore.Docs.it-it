---
title: Distribuire le app ASP.NET Core in Servizio app di Azure
author: guardrex
description: Questo articolo contiene collegamenti a risorse di hosting e distribuzione di Azure.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: f2de81af4bd2992aec76a287484d0057021231d8
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860966"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="ea129-103">Distribuire le app ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ea129-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="ea129-104">Il [servizio app di Azure](https://azure.microsoft.com/services/app-service/) è un [servizio di piattaforma di cloud computing Microsoft](https://azure.microsoft.com/) per l'hosting di app Web, inclusa ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea129-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="ea129-105">Risorse utili</span><span class="sxs-lookup"><span data-stu-id="ea129-105">Useful resources</span></span>

<span data-ttu-id="ea129-106">[Documentazione di App Web](/azure/app-service/) di Azure è la home page della documentazione, delle esercitazioni, degli esempi, delle guide introduttive e di altre risorse per le app di Azure.</span><span class="sxs-lookup"><span data-stu-id="ea129-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="ea129-107">Due importanti esercitazioni relative all'hosting di app ASP.NET Core sono:</span><span class="sxs-lookup"><span data-stu-id="ea129-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="ea129-108">Guida introduttiva: Creare un'app Web ASP.NET Core in Azure</span><span class="sxs-lookup"><span data-stu-id="ea129-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="ea129-109">Usare Visual Studio per creare e distribuire un'app Web ASP.NET Core nel servizio app di Azure in Windows.</span><span class="sxs-lookup"><span data-stu-id="ea129-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="ea129-110">Guida introduttiva: Creare un'app Web .NET Core nel Servizio app in Linux</span><span class="sxs-lookup"><span data-stu-id="ea129-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="ea129-111">Usare la riga di comando per creare e distribuire un'app Web ASP.NET Core nel servizio app di Azure in Linux.</span><span class="sxs-lookup"><span data-stu-id="ea129-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="ea129-112">Gli articoli seguenti sono disponibili nella documentazione di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ea129-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="ea129-113">Pubblicare in Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea129-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="ea129-114">Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea129-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="ea129-115">Distribuzione continua in Azure con Visual Studio e Git</span><span class="sxs-lookup"><span data-stu-id="ea129-115">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="ea129-116">Informazioni su come creare un'app Web ASP.NET Core tramite Visual Studio e distribuirla nel Servizio app di Azure usando Git per la distribuzione continua.</span><span class="sxs-lookup"><span data-stu-id="ea129-116">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="ea129-117">Creare la prima pipeline con Azure Pipelines</span><span class="sxs-lookup"><span data-stu-id="ea129-117">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="ea129-118">Impostare una build CI per un'app ASP.NET Core e quindi creare una versione di distribuzione continua in Servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="ea129-118">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

<span data-ttu-id="ea129-119">[Azure Web App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Sandbox per app Web di Azure)</span><span class="sxs-lookup"><span data-stu-id="ea129-119">[Azure Web App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>  
<span data-ttu-id="ea129-120">Individuare le limitazioni di esecuzione di runtime di Servizio app di Azure applicate dalla piattaforma per le app Azure.</span><span class="sxs-lookup"><span data-stu-id="ea129-120">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="ea129-121">Configurazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="ea129-121">Application configuration</span></span>

<span data-ttu-id="ea129-122">In ASP.NET Core 2.0 o versioni successive, i pacchetti NuGet seguenti offrono funzionalità di registrazione automatica per le app distribuite nel Servizio app di Azure:</span><span class="sxs-lookup"><span data-stu-id="ea129-122">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="ea129-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) per fornire l'integrazione di ASP.NET Core con il Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="ea129-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="ea129-124">La funzionalità di registrazione aggiunte vengono fornite dal pacchetto `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="ea129-124">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="ea129-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) esegue [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) per aggiungere i provider di registrazione diagnostica del servizio app di Azure nel pacchetto `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="ea129-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="ea129-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) offre implementazioni di logger per il supporto dei registri di diagnostica del servizio app di Azure e delle funzionalità del flusso di registrazione.</span><span class="sxs-lookup"><span data-stu-id="ea129-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="ea129-127">Se destinati a .NET Core e aventi come riferimento il [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage), i pacchetti sono già inclusi.</span><span class="sxs-lookup"><span data-stu-id="ea129-127">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="ea129-128">I pacchetti non sono presenti nel nuovo [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ea129-128">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="ea129-129">Se destinati a .NET Framework o aventi come riferimento il metapacchetto `Microsoft.AspNetCore.App`, fare riferimento ai singoli pacchetti di registrazione.</span><span class="sxs-lookup"><span data-stu-id="ea129-129">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="ea129-130">Eseguire l'override della configurazione delle app usando il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="ea129-130">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="ea129-131">L'area **Impostazioni app** del pannello **Impostazioni applicazione** consente di impostare le variabili di ambiente per l'app.</span><span class="sxs-lookup"><span data-stu-id="ea129-131">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="ea129-132">Le variabili di ambiente possono essere utilizzate dal [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ea129-132">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="ea129-133">Quando un'app usa l'[host Web](xref:fundamentals/host/web-host) e compila l'host con [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), le variabili di ambiente che consentono di configurare l'host usano il prefisso `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="ea129-133">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="ea129-134">Per altre informazioni, vedere <xref:fundamentals/host/web-host> e il [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ea129-134">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="ea129-135">Quando un'app usa l'[host generico](xref:fundamentals/host/generic-host), le variabili di ambiente non vengono caricate nella configurazione di un'app per impostazione predefinita e il provider di configurazione deve essere aggiunto dallo sviluppatore.</span><span class="sxs-lookup"><span data-stu-id="ea129-135">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="ea129-136">Lo sviluppatore determina il prefisso delle variabili di ambiente quando viene aggiunto il provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="ea129-136">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="ea129-137">Per altre informazioni, vedere <xref:fundamentals/host/generic-host> e il [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ea129-137">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ea129-138">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="ea129-138">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ea129-139">Il middleware di integrazione di IIS, che consente di configurare il middleware delle intestazioni inoltrate, e il modulo ASP.NET Core sono configurati per inoltrare lo schema (HTTP/HTTPS) e l'indirizzo IP remoto di origine della richiesta.</span><span class="sxs-lookup"><span data-stu-id="ea129-139">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="ea129-140">Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro ulteriori server proxy e servizi di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="ea129-140">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="ea129-141">Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="ea129-141">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="ea129-142">Monitoraggio e registrazione</span><span class="sxs-lookup"><span data-stu-id="ea129-142">Monitoring and logging</span></span>

<span data-ttu-id="ea129-143">Per informazioni sul monitoraggio, la registrazione e la risoluzione dei problemi, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="ea129-143">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="ea129-144">Procedura: Eseguire il monitoraggio delle app nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ea129-144">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="ea129-145">Informazioni su come esaminare le quote e le metriche per le app e i piani del servizio app.</span><span class="sxs-lookup"><span data-stu-id="ea129-145">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="ea129-146">Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ea129-146">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="ea129-147">Informazioni su come abilitare e accedere alla registrazione diagnostica per i codici di stato HTTP, le richieste non riuscite e l'attività del server Web.</span><span class="sxs-lookup"><span data-stu-id="ea129-147">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="ea129-148">Introduzione alla gestione degli errori in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea129-148">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="ea129-149">Riconoscimento degli approcci comuni di gestione degli errori nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea129-149">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="ea129-150">Risolvere i problemi di ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ea129-150">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="ea129-151">Informazioni su come diagnosticare i problemi delle distribuzioni del servizio app di Azure con le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea129-151">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="ea129-152">Errori comuni di Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea129-152">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="ea129-153">Informazioni sugli errori comuni di configurazione della distribuzione per le app ospitate dal servizio app di Azure o da IIS con suggerimenti per la risoluzione.</span><span class="sxs-lookup"><span data-stu-id="ea129-153">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="ea129-154">KeyRing di protezione dati e slot di distribuzione</span><span class="sxs-lookup"><span data-stu-id="ea129-154">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="ea129-155">Le [chiavi di protezione dati](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sono salvate in modo permanente nella cartella *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="ea129-155">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="ea129-156">La cartella è associata all'archiviazione di rete e sincronizzata in tutti i computer che ospitano l'app.</span><span class="sxs-lookup"><span data-stu-id="ea129-156">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="ea129-157">Le chiavi non vengono protette quando sono inattive.</span><span class="sxs-lookup"><span data-stu-id="ea129-157">Keys aren't protected at rest.</span></span> <span data-ttu-id="ea129-158">La cartella offre il KeyRing a tutte le istanze di un'app in un singolo slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="ea129-158">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="ea129-159">Gli slot di distribuzione separati, ad esempio gli slot di gestione temporanea e di produzione, non condividono un KeyRing.</span><span class="sxs-lookup"><span data-stu-id="ea129-159">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="ea129-160">Nel passaggio da uno slot di distribuzione all'altro, tutti i sistemi che usano la protezione dati non saranno in grado di decrittografare i dati archiviati usando il KeyRing all'interno dello slot precedente.</span><span class="sxs-lookup"><span data-stu-id="ea129-160">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="ea129-161">Il middleware dei cookie di ASP.NET usa la protezione dati per proteggere i cookie.</span><span class="sxs-lookup"><span data-stu-id="ea129-161">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="ea129-162">Di conseguenza, gli utenti vengono disconnessi da un'app che usa il middleware dei cookie di ASP.NET standard.</span><span class="sxs-lookup"><span data-stu-id="ea129-162">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="ea129-163">Per una soluzione di KeyRing indipendente dallo slot, usare un provider di KeyRing esterno, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ea129-163">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="ea129-164">Archiviazione BLOB di Azure</span><span class="sxs-lookup"><span data-stu-id="ea129-164">Azure Blob Storage</span></span>
* <span data-ttu-id="ea129-165">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ea129-165">Azure Key Vault</span></span>
* <span data-ttu-id="ea129-166">Archivio SQL</span><span class="sxs-lookup"><span data-stu-id="ea129-166">SQL store</span></span>
* <span data-ttu-id="ea129-167">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="ea129-167">Redis cache</span></span>

<span data-ttu-id="ea129-168">Per altre informazioni, vedere [Provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="ea129-168">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="ea129-169">Distribuire la versione di anteprima di ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ea129-169">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="ea129-170">Le app in anteprima di ASP.NET Core possono essere distribuite in Servizio app di Azure con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="ea129-170">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="ea129-171">[Installare l'estensione del sito di anteprima](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="ea129-171">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="ea129-172">Usare Docker con app Web per contenitori</span><span class="sxs-lookup"><span data-stu-id="ea129-172">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="ea129-173">Installare l'estensione del sito di anteprima</span><span class="sxs-lookup"><span data-stu-id="ea129-173">Install the preview site extension</span></span>

<span data-ttu-id="ea129-174">Se si verifica un problema con l'estensione del sito di anteprima, aprire un problema in [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="ea129-174">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="ea129-175">Dal portale di Azure passare al pannello Servizi App.</span><span class="sxs-lookup"><span data-stu-id="ea129-175">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="ea129-176">Selezionare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="ea129-176">Select the web app.</span></span>
1. <span data-ttu-id="ea129-177">Digitare "ex" nella casella di ricerca o scorrere verso il basso l'elenco dei riquadri di gestione fino a **STRUMENTI DI LAVORO**.</span><span class="sxs-lookup"><span data-stu-id="ea129-177">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="ea129-178">Selezionare **STRUMENTI DI LAVORO** > **Extensions** (Estensioni).</span><span class="sxs-lookup"><span data-stu-id="ea129-178">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="ea129-179">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ea129-179">Select **Add**.</span></span>
1. <span data-ttu-id="ea129-180">Selezionare l'estensione **ASP.NET Core &lt;x.y&gt; (x86) Runtime** dall'elenco, dove `<x.y>` è la versione di anteprima di ASP.NET Core (ad esempio, **ASP.NET Core 2.2 (x86) Runtime**).</span><span class="sxs-lookup"><span data-stu-id="ea129-180">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="ea129-181">Il runtime x86 è consigliato per le [distribuzioni dipendenti dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) che si basano sull'hosting out-of-process dal modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea129-181">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="ea129-182">Selezionare **OK** per accettare le condizioni legali.</span><span class="sxs-lookup"><span data-stu-id="ea129-182">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="ea129-183">Per installare l'estensione, selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea129-183">Select **OK** to install the extension.</span></span>

<span data-ttu-id="ea129-184">Al termine dell'operazione, viene installata l'anteprima più recente di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea129-184">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="ea129-185">Verificare l'installazione:</span><span class="sxs-lookup"><span data-stu-id="ea129-185">Verify the installation:</span></span>

1. <span data-ttu-id="ea129-186">Selezionare **Strumenti avanzati** in **STRUMENTI DI LAVORO**.</span><span class="sxs-lookup"><span data-stu-id="ea129-186">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="ea129-187">Selezionare **Vai** nel pannello **Strumenti avanzati**.</span><span class="sxs-lookup"><span data-stu-id="ea129-187">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="ea129-188">Selezionare l'elemento di menu **Console di debug** > **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="ea129-188">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="ea129-189">Eseguire il comando seguente dal prompt di PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ea129-189">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="ea129-190">Sostituire la versione di runtime di ASP.NET Core per `<x.y>` nel comando:</span><span class="sxs-lookup"><span data-stu-id="ea129-190">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="ea129-191">Se il runtime dell'anteprima installata si applica ad ASP.NET Core 2.2, il comando è:</span><span class="sxs-lookup"><span data-stu-id="ea129-191">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="ea129-192">Il comando restituisce `True` quando è installato il runtime di anteprima x64.</span><span class="sxs-lookup"><span data-stu-id="ea129-192">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="ea129-193">L'architettura della piattaforma (x86 o x64) di un'app di Servizi app è impostata nel pannello **Impostazioni applicazione** in **Impostazioni generali** per le app ospitate in un livello di hosting con calcolo di serie A o migliore.</span><span class="sxs-lookup"><span data-stu-id="ea129-193">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="ea129-194">Se l'app viene eseguita in modalità in-process e l'architettura della piattaforma è configurata per 64 bit (x64), il modulo ASP.NET Core usa il runtime dell'anteprima a 64 bit, se presente.</span><span class="sxs-lookup"><span data-stu-id="ea129-194">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="ea129-195">Installare l'estensione **ASP.NET Core &lt;x.y&gt; (x64) Runtime** (ad esempio, **ASP.NET Core 2.2 (x64) Runtime**).</span><span class="sxs-lookup"><span data-stu-id="ea129-195">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="ea129-196">Dopo aver installato il runtime di anteprima x64, eseguire il comando seguente nella finestra di comando di PowerShell di Kudu per verificare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="ea129-196">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="ea129-197">Sostituire la versione di runtime di ASP.NET Core per `<x.y>` nel comando:</span><span class="sxs-lookup"><span data-stu-id="ea129-197">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="ea129-198">Se il runtime dell'anteprima installata si applica ad ASP.NET Core 2.2, il comando è:</span><span class="sxs-lookup"><span data-stu-id="ea129-198">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="ea129-199">Il comando restituisce `True` quando è installato il runtime di anteprima x64.</span><span class="sxs-lookup"><span data-stu-id="ea129-199">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ea129-200">Le **estensioni di ASP.NET Core** abilitano funzionalità aggiuntive per ASP.NET Core nei Servizi app di Azure, ad esempio la registrazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ea129-200">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="ea129-201">L'estensione viene installata automaticamente durante la distribuzione da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea129-201">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="ea129-202">Se l'estensione non è installata, installarla per l'app.</span><span class="sxs-lookup"><span data-stu-id="ea129-202">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="ea129-203">**Usare l'estensione del sito di anteprima con un modello ARM**</span><span class="sxs-lookup"><span data-stu-id="ea129-203">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="ea129-204">Se per creare e distribuire le app si usa un modello ARM, è possibile usare il tipo di risorsa `siteextensions` per aggiungere l'estensione del sito a un'app Web.</span><span class="sxs-lookup"><span data-stu-id="ea129-204">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="ea129-205">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ea129-205">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="ea129-206">Usare Docker con app Web per contenitori</span><span class="sxs-lookup"><span data-stu-id="ea129-206">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="ea129-207">L'[hub Docker](https://hub.docker.com/r/microsoft/aspnetcore/) contiene le immagini di Docker più recenti per la versione di anteprima.</span><span class="sxs-lookup"><span data-stu-id="ea129-207">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="ea129-208">Le immagini possono essere usate come immagini di base.</span><span class="sxs-lookup"><span data-stu-id="ea129-208">The images can be used as a base image.</span></span> <span data-ttu-id="ea129-209">Usare l'immagine e distribuirla alle app Web per i contenitori normalmente.</span><span class="sxs-lookup"><span data-stu-id="ea129-209">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea129-210">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ea129-210">Additional resources</span></span>

* [<span data-ttu-id="ea129-211">Panoramica di App Web (video di 5 minuti)</span><span class="sxs-lookup"><span data-stu-id="ea129-211">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* <span data-ttu-id="ea129-212">[Azure App Service: The Best Place to Host your .NET Apps ](https://channel9.msdn.com/events/dotnetConf/2017/T222) (Servizio app di Azure: la soluzione migliore per l'hosting delle app .NET) (video di 55 minuti)</span><span class="sxs-lookup"><span data-stu-id="ea129-212">[Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)](https://channel9.msdn.com/events/dotnetConf/2017/T222)</span></span>
* <span data-ttu-id="ea129-213">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)</span><span class="sxs-lookup"><span data-stu-id="ea129-213">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)</span></span>
* [<span data-ttu-id="ea129-214">Panoramica della diagnostica del servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ea129-214">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="ea129-215">Il servizio app di Azure in Windows Server usa [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ea129-215">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="ea129-216">Gli argomenti seguenti riguardano la tecnologia IIS sottostante:</span><span class="sxs-lookup"><span data-stu-id="ea129-216">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="ea129-217">Libreria di Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="ea129-217">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
