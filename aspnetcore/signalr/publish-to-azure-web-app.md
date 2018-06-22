---
title: Pubblicare un Core di ASP.NET SignalR app per App Web di Azure
author: rachelappel
description: Pubblicare un Core di ASP.NET SignalR app per App Web di Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 0d98c6b24b9695c0af0170173f13902bac5f55ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271918"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="50915-103">Pubblicare un Core di ASP.NET SignalR app a un'App Web di Azure</span><span class="sxs-lookup"><span data-stu-id="50915-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="50915-104">[Azure Web App](/azure/app-service/app-service-web-overview) è un [Microsoft per il cloud computing](https://azure.microsoft.com/) servizio di piattaforma per l'hosting di applicazioni web, tra cui ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="50915-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="50915-105">In questo articolo si riferisce alla pubblicazione di un'app di ASP.NET SignalR Core da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50915-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="50915-106">Visitare [servizio SignalR per Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) per ulteriori informazioni sull'utilizzo SignalR in Azure.</span><span class="sxs-lookup"><span data-stu-id="50915-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="50915-107">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="50915-107">Publish the app</span></span>

<span data-ttu-id="50915-108">Visual Studio fornisce gli strumenti predefiniti per la pubblicazione in un'App Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="50915-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="50915-109">Visual Studio Code utente può utilizzare [CLI di Azure](/cli/azure) comandi per pubblicare le App in Azure.</span><span class="sxs-lookup"><span data-stu-id="50915-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="50915-110">Questo articolo descrive pubblicazione usando gli strumenti in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50915-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="50915-111">Per pubblicare un'app usando Azure CLI, vedere [pubblicare un'app di ASP.NET Core in Azure con gli strumenti da riga di comando](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="50915-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="50915-112">Pulsante destro del mouse sul progetto in **Esplora soluzioni** e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="50915-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="50915-113">Verificare che **Crea nuovo** viene archiviato il **selezionare una destinazione di pubblicazione** finestra di dialogo, quindi selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="50915-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Selezione destinazione di pubblicazione](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="50915-115">Immettere le informazioni seguenti nel **servizio App di creare** finestra di dialogo e selezionare **crea**.</span><span class="sxs-lookup"><span data-stu-id="50915-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="50915-116">Elemento</span><span class="sxs-lookup"><span data-stu-id="50915-116">Item</span></span> | <span data-ttu-id="50915-117">Descrizione</span><span class="sxs-lookup"><span data-stu-id="50915-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="50915-118">**Nome dell'App**</span><span class="sxs-lookup"><span data-stu-id="50915-118">**App name**</span></span> | <span data-ttu-id="50915-119">Un nome univoco dell'app.</span><span class="sxs-lookup"><span data-stu-id="50915-119">A unique name of the app.</span></span> |
| <span data-ttu-id="50915-120">**Sottoscrizione**</span><span class="sxs-lookup"><span data-stu-id="50915-120">**Subscription**</span></span> | <span data-ttu-id="50915-121">La sottoscrizione di Azure che usa l'app.</span><span class="sxs-lookup"><span data-stu-id="50915-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="50915-122">**Gruppo di risorse**</span><span class="sxs-lookup"><span data-stu-id="50915-122">**Resource Group**</span></span> | <span data-ttu-id="50915-123">Il gruppo di risorse correlati a cui appartiene l'app.</span><span class="sxs-lookup"><span data-stu-id="50915-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="50915-124">**Piano di hosting**</span><span class="sxs-lookup"><span data-stu-id="50915-124">**Hosting Plan**</span></span> | <span data-ttu-id="50915-125">Il piano dei prezzi per l'app web.</span><span class="sxs-lookup"><span data-stu-id="50915-125">The pricing plan for the web app.</span></span> |

![Creare servizio app](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="50915-127">Visual Studio consente di completare le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="50915-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="50915-128">Crea un profilo di pubblicazione che contiene le impostazioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="50915-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="50915-129">Crea o utilizza un oggetto esistente *App Web di Azure* con i dettagli forniti.</span><span class="sxs-lookup"><span data-stu-id="50915-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="50915-130">Pubblica l'app.</span><span class="sxs-lookup"><span data-stu-id="50915-130">Publishes the app.</span></span>
* <span data-ttu-id="50915-131">Avvia un browser, con l'app web pubblicata caricato.</span><span class="sxs-lookup"><span data-stu-id="50915-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="50915-132">Si noti il formato dell'URL per l'app è stata *.azurewebsites {nome app} .net*.</span><span class="sxs-lookup"><span data-stu-id="50915-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="50915-133">Ad esempio, un'app denominata `SignalRChattR` dispone di un URL simile `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="50915-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="50915-134">Se si verifica un errore HTTP 502.2, vedere [versione di anteprima di distribuire ASP.NET Core servizio App di Azure](xref:host-and-deploy/azure-apps/index) per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="50915-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="50915-135">Configura app web di SignalR</span><span class="sxs-lookup"><span data-stu-id="50915-135">Configure SignalR web app</span></span>

<span data-ttu-id="50915-136">ASP.NET SignalR Core App pubblicate come un'App Web di Azure deve includere [affinità ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) abilitato.</span><span class="sxs-lookup"><span data-stu-id="50915-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="50915-137">[I WebSocket](xref:fundamentals/websockets) deve essere abilitata, per consentire il trasporto WebSocket alla funzione.</span><span class="sxs-lookup"><span data-stu-id="50915-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="50915-138">Nel portale di Azure, passare a **le impostazioni dell'App** per l'app web.</span><span class="sxs-lookup"><span data-stu-id="50915-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="50915-139">Impostare **WebSockets** per **sul**e verificare **ARR affinità** è **su**.</span><span class="sxs-lookup"><span data-stu-id="50915-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Impostazioni di app Web di Azure nel portale di Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="50915-141">I WebSocket e degli altri trasporti [sono limitati in base al piano di servizio App](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="50915-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="50915-142">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="50915-142">Related resources</span></span>

* [<span data-ttu-id="50915-143">Pubblicare un'app di ASP.NET Core in Azure con gli strumenti da riga di comando</span><span class="sxs-lookup"><span data-stu-id="50915-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="50915-144">Pubblicare un'app di ASP.NET Core in Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50915-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="50915-145">Ospitare e distribuire le app ASP.NET Core anteprima in Azure</span><span class="sxs-lookup"><span data-stu-id="50915-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
