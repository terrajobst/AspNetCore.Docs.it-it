---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Pubblicare l'App Azure servizio App di Azure | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="21928-102">Pubblicare l'App Azure servizio App di Azure</span><span class="sxs-lookup"><span data-stu-id="21928-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="21928-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="21928-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="21928-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="21928-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="21928-105">Come ultimo passaggio, si pubblicherà l'applicazione in Azure.</span><span class="sxs-lookup"><span data-stu-id="21928-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="21928-106">In Esplora soluzioni, fare clic sul progetto e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="21928-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="21928-107">Fare clic su **pubblica** richiama il **pubblica sul Web** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="21928-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="21928-108">Se è stata selezionata **Host nel Cloud** quando si crea il progetto, quindi la connessione e le impostazioni sono già configurate.</span><span class="sxs-lookup"><span data-stu-id="21928-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="21928-109">In tal caso, scegliere il **impostazioni** e controllare &quot;eseguire le migrazioni Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="21928-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="21928-110">(Se non è stata selezionata **Host nel Cloud** all'inizio, quindi seguire i passaggi di [nella sezione successiva](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="21928-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="21928-111">Per distribuire l'app, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="21928-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="21928-112">È possibile visualizzare lo stato nella pubblicazione di **attività di pubblicazione Web** finestra.</span><span class="sxs-lookup"><span data-stu-id="21928-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="21928-113">(Dal **vista** dal menu **altre finestre**, quindi selezionare **attività di pubblicazione Web**.)</span><span class="sxs-lookup"><span data-stu-id="21928-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="21928-114">Al termine Visual Studio distribuisce l'app, il browser predefinito verrà aperta automaticamente per l'URL del sito Web distribuito e l'applicazione creata è in esecuzione nel cloud.</span><span class="sxs-lookup"><span data-stu-id="21928-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="21928-115">L'URL nella barra degli indirizzi del browser mostra che il sito viene caricato da Internet.</span><span class="sxs-lookup"><span data-stu-id="21928-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="21928-116">La distribuzione in un nuovo sito Web</span><span class="sxs-lookup"><span data-stu-id="21928-116">Deploying to a New Website</span></span>

<span data-ttu-id="21928-117">Se non si seleziona **Host nel Cloud** quando si crea il progetto, è possibile configurare una nuova app web.</span><span class="sxs-lookup"><span data-stu-id="21928-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="21928-118">In Esplora soluzioni, fare clic sul progetto e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="21928-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="21928-119">Selezionare il **profilo** scheda e fare clic su **siti Web di Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="21928-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="21928-120">Se non è attualmente effettuato l'accesso ad Azure, verrà richiesto di accedere.</span><span class="sxs-lookup"><span data-stu-id="21928-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="21928-121">Nel **i siti Web esistenti** finestra di dialogo, fare clic su **New**.</span><span class="sxs-lookup"><span data-stu-id="21928-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="21928-122">Immettere un nome di sito.</span><span class="sxs-lookup"><span data-stu-id="21928-122">Enter a site name.</span></span> <span data-ttu-id="21928-123">Selezionare la sottoscrizione di Azure e l'area.</span><span class="sxs-lookup"><span data-stu-id="21928-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="21928-124">In **server di Database**selezionare **creare un nuovo Server**, oppure selezionare un server esistente.</span><span class="sxs-lookup"><span data-stu-id="21928-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="21928-125">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="21928-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="21928-126">Fare clic su di **impostazioni** e controllare &quot;eseguire le migrazioni Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="21928-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="21928-127">Quindi fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="21928-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="21928-128">Precedente</span><span class="sxs-lookup"><span data-stu-id="21928-128">Previous</span></span>](part-9.md)
