---
title: Ospitare ASP.NET Core in Servizio app di Azure
author: guardrex
description: Informazioni su come ospitare le app ASP.NET Core nel servizio app di Azure con collegamenti a risorse utili.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="9d8c0-103">Ospitare ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="9d8c0-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="9d8c0-104">Il [servizio app di Azure](https://azure.microsoft.com/services/app-service/) è un [servizio di piattaforma di cloud computing Microsoft](https://azure.microsoft.com/) per l'hosting di app Web, inclusa ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="9d8c0-105">Risorse utili</span><span class="sxs-lookup"><span data-stu-id="9d8c0-105">Useful resources</span></span>

<span data-ttu-id="9d8c0-106">[Documentazione di App Web](/azure/app-service/) di Azure è la home page della documentazione, delle esercitazioni, degli esempi, delle guide introduttive e di altre risorse per le app di Azure.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="9d8c0-107">Due importanti esercitazioni relative all'hosting di app ASP.NET Core sono:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="9d8c0-108">Guida introduttiva: Creare un'app Web ASP.NET Core in Azure</span><span class="sxs-lookup"><span data-stu-id="9d8c0-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="9d8c0-109">Usare Visual Studio per creare e distribuire un'app Web ASP.NET Core nel servizio app di Azure in Windows.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="9d8c0-110">Guida introduttiva: Creare un'app Web .NET Core nel Servizio app in Linux</span><span class="sxs-lookup"><span data-stu-id="9d8c0-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="9d8c0-111">Usare la riga di comando per creare e distribuire un'app Web ASP.NET Core nel servizio app di Azure in Linux.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="9d8c0-112">Gli articoli seguenti sono disponibili nella documentazione di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="9d8c0-113">Pubblicare in Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d8c0-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="9d8c0-114">Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="9d8c0-115">Pubblicare in Azure con gli strumenti dell'interfaccia della riga di comando</span><span class="sxs-lookup"><span data-stu-id="9d8c0-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="9d8c0-116">Informazioni su come pubblicare un'app ASP.NET Core nel servizio app di Azure con il client della riga di comando Git.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="9d8c0-117">Distribuzione continua in Azure con Visual Studio e Git</span><span class="sxs-lookup"><span data-stu-id="9d8c0-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="9d8c0-118">Informazioni su come creare un'app Web ASP.NET Core tramite Visual Studio e distribuirla nel Servizio app di Azure usando Git per la distribuzione continua.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="9d8c0-119">Distribuzione continua in Azure con VSTS</span><span class="sxs-lookup"><span data-stu-id="9d8c0-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="9d8c0-120">Impostare una build CI per un'app ASP.NET Core e quindi creare una versione di distribuzione continua in Servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

<span data-ttu-id="9d8c0-121">[Azure Web App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Sandbox per app Web di Azure)</span><span class="sxs-lookup"><span data-stu-id="9d8c0-121">[Azure Web App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>  
<span data-ttu-id="9d8c0-122">Individuare le limitazioni di esecuzione di runtime di Servizio app di Azure applicate dalla piattaforma per le app Azure.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="9d8c0-123">Configurazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="9d8c0-123">Application configuration</span></span>

<span data-ttu-id="9d8c0-124">Con ASP.NET Core 2.0 e versioni successive, tre pacchetti del [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage) offrono funzionalità di registrazione automatica per le app distribuite nel servizio app di Azure:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="9d8c0-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) per fornire l'integrazione di ASP.NET Core con il servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="9d8c0-126">La funzionalità di registrazione aggiunte vengono fornite dal pacchetto `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="9d8c0-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) esegue [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) per aggiungere i provider di registrazione diagnostica del servizio app di Azure nel pacchetto `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="9d8c0-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) offre implementazioni di logger per il supporto dei registri di diagnostica del servizio app di Azure e delle funzionalità del flusso di registrazione.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="9d8c0-129">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="9d8c0-129">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="9d8c0-130">Il middleware di integrazione di IIS, che consente di configurare il middleware delle intestazioni inoltrate, e il modulo ASP.NET Core sono configurati per inoltrare lo schema (HTTP/HTTPS) e l'indirizzo IP remoto di origine della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-130">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="9d8c0-131">Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro ulteriori server proxy e servizi di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-131">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="9d8c0-132">Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="9d8c0-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="9d8c0-133">Monitoraggio e registrazione</span><span class="sxs-lookup"><span data-stu-id="9d8c0-133">Monitoring and logging</span></span>

<span data-ttu-id="9d8c0-134">Per informazioni sul monitoraggio, la registrazione e la risoluzione dei problemi, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-134">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="9d8c0-135">Procedura: Eseguire il monitoraggio delle app nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="9d8c0-135">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="9d8c0-136">Informazioni su come esaminare le quote e le metriche per le app e i piani del servizio app.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-136">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="9d8c0-137">Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="9d8c0-137">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="9d8c0-138">Informazioni su come abilitare e accedere alla registrazione diagnostica per i codici di stato HTTP, le richieste non riuscite e l'attività del server Web.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-138">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="9d8c0-139">Introduzione alla gestione degli errori in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d8c0-139">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="9d8c0-140">Informazioni sugli approcci comuni di gestione degli errori nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-140">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="9d8c0-141">Risolvere i problemi di ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="9d8c0-141">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="9d8c0-142">Informazioni su come diagnosticare i problemi delle distribuzioni del servizio app di Azure con le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-142">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="9d8c0-143">Errori comuni di Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d8c0-143">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="9d8c0-144">Informazioni sugli errori comuni di configurazione della distribuzione per le app ospitate dal servizio app di Azure o da IIS con suggerimenti per la risoluzione.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-144">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="9d8c0-145">KeyRing di protezione dati e slot di distribuzione</span><span class="sxs-lookup"><span data-stu-id="9d8c0-145">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="9d8c0-146">Le [chiavi di protezione dati](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sono salvate in modo permanente nella cartella *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-146">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="9d8c0-147">La cartella è associata all'archiviazione di rete e sincronizzata in tutti i computer che ospitano l'app.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-147">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="9d8c0-148">Le chiavi non vengono protette quando sono inattive.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-148">Keys aren't protected at rest.</span></span> <span data-ttu-id="9d8c0-149">La cartella offre il KeyRing a tutte le istanze di un'app in un singolo slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-149">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="9d8c0-150">Gli slot di distribuzione separati, ad esempio gli slot di gestione temporanea e di produzione, non condividono un KeyRing.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-150">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="9d8c0-151">Nel passaggio da uno slot di distribuzione all'altro, tutti i sistemi che usano la protezione dati non saranno in grado di decrittografare i dati archiviati usando il KeyRing all'interno dello slot precedente.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-151">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="9d8c0-152">Il middleware dei cookie di ASP.NET usa la protezione dati per proteggere i cookie.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-152">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="9d8c0-153">Di conseguenza, gli utenti vengono disconnessi da un'app che usa il middleware dei cookie di ASP.NET standard.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-153">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="9d8c0-154">Per una soluzione di KeyRing indipendente dallo slot, usare un provider di KeyRing esterno, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-154">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="9d8c0-155">Archiviazione BLOB di Azure</span><span class="sxs-lookup"><span data-stu-id="9d8c0-155">Azure Blob Storage</span></span>
* <span data-ttu-id="9d8c0-156">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="9d8c0-156">Azure Key Vault</span></span>
* <span data-ttu-id="9d8c0-157">Archivio SQL</span><span class="sxs-lookup"><span data-stu-id="9d8c0-157">SQL store</span></span>
* <span data-ttu-id="9d8c0-158">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="9d8c0-158">Redis cache</span></span>

<span data-ttu-id="9d8c0-159">Per altre informazioni, vedere [Provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="9d8c0-159">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="9d8c0-160">Distribuire la versione di anteprima di ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="9d8c0-160">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="9d8c0-161">Le app in anteprima di ASP.NET Core possono essere distribuite in Servizio app di Azure con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-161">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="9d8c0-162">Installare l'estensione del sito di anteprima</span><span class="sxs-lookup"><span data-stu-id="9d8c0-162">Install the preview site extention</span></span>](#site-x)
* [<span data-ttu-id="9d8c0-163">Distribuire l'app autonoma</span><span class="sxs-lookup"><span data-stu-id="9d8c0-163">Deploy the app self contained</span></span>](#self)
* [<span data-ttu-id="9d8c0-164">Usare Docker con app Web per contenitori</span><span class="sxs-lookup"><span data-stu-id="9d8c0-164">Use Docker with Web Apps for containers</span></span>](#docker)

<span data-ttu-id="9d8c0-165">In caso di problemi con l'estensione del sito di anteprima, aprire un problema in [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="9d8c0-165">If you have a problem using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a><span data-ttu-id="9d8c0-166">Installare l'estensione del sito di anteprima</span><span class="sxs-lookup"><span data-stu-id="9d8c0-166">Install the preview site extention</span></span>

* <span data-ttu-id="9d8c0-167">Dal portale di Azure passare al pannello Servizi App.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-167">From the Azure portal, navigate to the App Service blade.</span></span>
* <span data-ttu-id="9d8c0-168">Immettere "est" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-168">Enter "ex" in the search box.</span></span>
* <span data-ttu-id="9d8c0-169">Selezionare **Estensioni**.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-169">Select **Extensions**.</span></span>
* <span data-ttu-id="9d8c0-170">Seleziona "Aggiungi".</span><span class="sxs-lookup"><span data-stu-id="9d8c0-170">Select "Add".</span></span>

![Pannello dell'app Azure con i passaggi precedenti](index/_static/x1.png)

* <span data-ttu-id="9d8c0-172">Selezionare **ASP.NET Core Runtime Extensions** (Estensioni runtime di ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="9d8c0-172">Select **ASP.NET Core Runtime Extensions**.</span></span>
* <span data-ttu-id="9d8c0-173">Selezionare **OK** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-173">Select **OK** > **OK**.</span></span>

<span data-ttu-id="9d8c0-174">Al termine delle operazioni di aggiunta, viene installata l'anteprima più recente di .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-174">When the add operations completes, the latest .NET Core 2.1 preview is installed.</span></span> <span data-ttu-id="9d8c0-175">È possibile verificare l'installazione eseguendo `dotnet --info` nella console.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-175">You can verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="9d8c0-176">Dal pannello Servizi app:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-176">From the App Service blade:</span></span>

* <span data-ttu-id="9d8c0-177">Immettere "con" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-177">Enter "con" in the search box.</span></span>
* <span data-ttu-id="9d8c0-178">Selezionare **Console**.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-178">Select **Console**.</span></span>
* <span data-ttu-id="9d8c0-179">Immettere `dotnet --info` nella console.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-179">Enter `dotnet --info` in the console.</span></span>

![Pannello dell'app Azure con i passaggi precedenti](index/_static/cons.png)

<span data-ttu-id="9d8c0-181">L'immagine precedente era aggiornata al momento della redazione di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-181">The preceding image was current at the time this was written.</span></span> <span data-ttu-id="9d8c0-182">Potrebbe essere visualizzata una versione diversa.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-182">You may see a different version.</span></span>

<span data-ttu-id="9d8c0-183">`dotnet --info` consente di visualizzare il percorso dell'estensione del sito in cui è stata installata l'anteprima.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-183">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="9d8c0-184">Mostra che l'app è in esecuzione dall'estensione del sito invece che dal percorso predefinito *ProgramFiles*.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-184">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="9d8c0-185">Se viene visualizzato *ProgramFiles*, riavviare il sito ed eseguire `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-185">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a><span data-ttu-id="9d8c0-186">Usare l'estensione del sito di anteprima con un modello ARM</span><span class="sxs-lookup"><span data-stu-id="9d8c0-186">Use the preview site extention with an ARM template</span></span>

<span data-ttu-id="9d8c0-187">Se si usa un modello ARM per creare e distribuire le applicazioni, è possibile usare il tipo di risorsa `siteextensions` per aggiungere l'estensione del sito a un'app Web.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-187">If you are using an ARM template to create and deploy applications you can use the `siteextensions` resource type to add the site extension to a Web App.</span></span> <span data-ttu-id="9d8c0-188">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-188">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="9d8c0-189">Distribuire l'app autonoma</span><span class="sxs-lookup"><span data-stu-id="9d8c0-189">Deploy the app self contained</span></span>

<span data-ttu-id="9d8c0-190">È possibile distribuire un'[app autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) che include il runtime di anteprima quando viene distribuita.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-190">You can deploy a [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) that carries the preview runtime with it when being deployed.</span></span> <span data-ttu-id="9d8c0-191">Per la distribuzione di un'app autonoma:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-191">When deploying a self contained app:</span></span>

* <span data-ttu-id="9d8c0-192">Non è necessario preparare il sito.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-192">You don’t need to prepare your site.</span></span>
* <span data-ttu-id="9d8c0-193">È necessario pubblicare l'applicazione in modo diverso rispetto a quanto richiesto per la distribuzione di un'app quando l'SDK è installato nel server.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-193">Requires you to publish your application differently than you would when deploying an app once the SDK is installed on the server.</span></span>

<span data-ttu-id="9d8c0-194">Le app autonome sono un'opzione per tutte le applicazioni .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-194">Self-contained apps are an option for all .NET Core applications.</span></span>

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="9d8c0-195">Usare Docker con app Web per contenitori</span><span class="sxs-lookup"><span data-stu-id="9d8c0-195">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="9d8c0-196">L'[hub di Docker](https://hub.docker.com/r/microsoft/aspnetcore/) contiene le immagini di Docker più recenti per la versione di anteprima 2.1.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-196">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest 2.1 preview Docker images.</span></span> <span data-ttu-id="9d8c0-197">È possibile usarle come immagine di base e distribuirle in app Web per contenitori come di consueto.</span><span class="sxs-lookup"><span data-stu-id="9d8c0-197">You can use them as your base image and deploy to Web Apps for Containers as you normally would.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d8c0-198">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9d8c0-198">Additional resources</span></span>

* [<span data-ttu-id="9d8c0-199">Panoramica di App Web (video di 5 minuti)</span><span class="sxs-lookup"><span data-stu-id="9d8c0-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* <span data-ttu-id="9d8c0-200">[Azure App Service: The Best Place to Host your .NET Apps ](https://channel9.msdn.com/events/dotnetConf/2017/T222) (Servizio app di Azure: la soluzione migliore per l'hosting delle app .NET) (video di 55 minuti)</span><span class="sxs-lookup"><span data-stu-id="9d8c0-200">[Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)](https://channel9.msdn.com/events/dotnetConf/2017/T222)</span></span>
* <span data-ttu-id="9d8c0-201">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)</span><span class="sxs-lookup"><span data-stu-id="9d8c0-201">[Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)</span></span>
* [<span data-ttu-id="9d8c0-202">Panoramica della diagnostica del servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="9d8c0-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="9d8c0-203">Il servizio app di Azure in Windows Server usa [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="9d8c0-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="9d8c0-204">Gli argomenti seguenti riguardano la tecnologia IIS sottostante:</span><span class="sxs-lookup"><span data-stu-id="9d8c0-204">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="9d8c0-205">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="9d8c0-205">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="9d8c0-206">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d8c0-206">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="9d8c0-207">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d8c0-207">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="9d8c0-208">Moduli IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d8c0-208">IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="9d8c0-209">Libreria di Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="9d8c0-209">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
