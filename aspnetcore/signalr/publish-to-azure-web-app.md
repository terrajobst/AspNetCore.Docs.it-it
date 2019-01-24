---
title: Pubblicare un ASP.NET Core SignalR app all'App Web di Azure
author: bradygaster
description: Pubblicare un ASP.NET Core SignalR app all'App Web di Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 66fa855590c49c4284e4b42cae57f3d4d81dd0fc
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837676"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="f4ca2-103">Pubblicare un ASP.NET Core SignalR app a un'App Web di Azure</span><span class="sxs-lookup"><span data-stu-id="f4ca2-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="f4ca2-104">[App Web di Azure](/azure/app-service/app-service-web-overview) è un [Microsoft il cloud computing](https://azure.microsoft.com/) servizio della piattaforma per l'hosting di App web, tra cui ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="f4ca2-105">Questo articolo si riferisce alla pubblicazione di un'app ASP.NET Core SignalR da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="f4ca2-106">Visita [servizio SignalR per Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) per altre informazioni sull'uso di SignalR in Azure.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="f4ca2-107">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="f4ca2-107">Publish the app</span></span>

<span data-ttu-id="f4ca2-108">Visual Studio offre strumenti incorporati per la pubblicazione in un'App Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="f4ca2-109">Visual Studio Code utente può utilizzare [CLI Azure](/cli/azure) comandi per pubblicare le App in Azure.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="f4ca2-110">Questo articolo illustra pubblicazione usando gli strumenti in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="f4ca2-111">Per pubblicare un'app usando Azure CLI, vedere [pubblicare un'app ASP.NET Core in Azure con gli strumenti da riga di comando](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="f4ca2-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="f4ca2-112">Pulsante destro del mouse sul progetto in **Esplora soluzioni** e selezionare **Publish**.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="f4ca2-113">Verificare che **Crea nuovo** viene archiviato il **selezionare una destinazione di pubblicazione** finestra di dialogo e selezionare **Publish**.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Selezione destinazione di pubblicazione](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="f4ca2-115">Immettere le informazioni seguenti nel **Crea servizio App** finestra di dialogo e selezionare **crea**.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="f4ca2-116">Elemento</span><span class="sxs-lookup"><span data-stu-id="f4ca2-116">Item</span></span> | <span data-ttu-id="f4ca2-117">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f4ca2-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="f4ca2-118">**Nome dell'App**</span><span class="sxs-lookup"><span data-stu-id="f4ca2-118">**App name**</span></span> | <span data-ttu-id="f4ca2-119">Un nome univoco dell'app.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-119">A unique name of the app.</span></span> |
| <span data-ttu-id="f4ca2-120">**Sottoscrizione**</span><span class="sxs-lookup"><span data-stu-id="f4ca2-120">**Subscription**</span></span> | <span data-ttu-id="f4ca2-121">Sottoscrizione di Azure usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="f4ca2-122">**Gruppo di risorse**</span><span class="sxs-lookup"><span data-stu-id="f4ca2-122">**Resource Group**</span></span> | <span data-ttu-id="f4ca2-123">Il gruppo di risorse correlate a cui appartiene l'app.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="f4ca2-124">**Piano di hosting**</span><span class="sxs-lookup"><span data-stu-id="f4ca2-124">**Hosting Plan**</span></span> | <span data-ttu-id="f4ca2-125">Il piano tariffario per l'app web.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-125">The pricing plan for the web app.</span></span> |

![Crea servizio app](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="f4ca2-127">Visual Studio completa le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="f4ca2-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="f4ca2-128">Crea un profilo di pubblicazione che contiene le impostazioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="f4ca2-129">Crea o Usa esistente *App Web di Azure* con i dettagli forniti.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="f4ca2-130">Pubblica l'app.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-130">Publishes the app.</span></span>
* <span data-ttu-id="f4ca2-131">Avvia un browser, con l'app web pubblicata caricato.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="f4ca2-132">Si noti che il formato dell'URL per l'app è *amp;#42;.azurewebsites.NET {nome dell'app} .net*.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="f4ca2-133">Ad esempio, un'app denominata `SignalRChattR` dispone di un URL simile a `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="f4ca2-134">Se si verifica un errore HTTP 502.2, vedere [versione di anteprima di distribuzione di ASP.NET Core nel servizio App di Azure di](xref:host-and-deploy/azure-apps/index) per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="f4ca2-135">Configurare l'app web di SignalR</span><span class="sxs-lookup"><span data-stu-id="f4ca2-135">Configure SignalR web app</span></span>

<span data-ttu-id="f4ca2-136">ASP.NET Core SignalR App che vengono pubblicate come un'App Web di Azure deve disporre [affinità ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) abilitata.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="f4ca2-137">[WebSockets](xref:fundamentals/websockets) deve essere abilitata, per consentire il trasporto WebSocket alla funzione.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="f4ca2-138">Nel portale di Azure, passare a **le impostazioni dell'App** per l'app web.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="f4ca2-139">Impostare **WebSockets** al **sul**e verificare **affinità ARR** è **su**.</span><span class="sxs-lookup"><span data-stu-id="f4ca2-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Impostazioni dell'app Web Azure nel portale di Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="f4ca2-141">WebSockets e altri trasporti [sono limitati in base al piano di servizio App](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="f4ca2-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="f4ca2-142">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="f4ca2-142">Related resources</span></span>

* [<span data-ttu-id="f4ca2-143">Pubblicare un'app ASP.NET Core in Azure con gli strumenti da riga di comando</span><span class="sxs-lookup"><span data-stu-id="f4ca2-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="f4ca2-144">Pubblicare un'app ASP.NET Core in Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4ca2-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="f4ca2-145">Ospitare e distribuire le App in anteprima di ASP.NET Core in Azure</span><span class="sxs-lookup"><span data-stu-id="f4ca2-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
