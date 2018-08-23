---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Pubblicare l'App Azure servizio App di Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 5c1a70ceded85681046065881f62c5c95c5d8740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831264"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="796f4-102">Pubblicare l'App Azure servizio App di Azure</span><span class="sxs-lookup"><span data-stu-id="796f4-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="796f4-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="796f4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="796f4-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="796f4-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="796f4-105">Come ultimo passaggio, si pubblicherà l'applicazione in Azure.</span><span class="sxs-lookup"><span data-stu-id="796f4-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="796f4-106">In Esplora soluzioni fare clic sul progetto e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="796f4-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="796f4-107">Facendo clic **Publish** richiama le **pubblica sul Web** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="796f4-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="796f4-108">Se è stata selezionata **ospita nel Cloud** quando è stato creato prima di tutto il progetto, quindi la connessione e le impostazioni sono già configurate.</span><span class="sxs-lookup"><span data-stu-id="796f4-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="796f4-109">In tal caso, semplicemente fare clic sui **le impostazioni** scheda e selezionare &quot;Esegui migrazioni Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="796f4-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="796f4-110">(Se non è stata selezionata **ospita nel Cloud** all'inizio, quindi seguire i passaggi descritti nel [nella sezione successiva](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="796f4-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="796f4-111">Per distribuire l'app, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="796f4-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="796f4-112">È possibile visualizzare lo stato della pubblicazione nel **attività pubblicazione sul Web** finestra.</span><span class="sxs-lookup"><span data-stu-id="796f4-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="796f4-113">(Dal **View** dal menu **Other Windows**, quindi selezionare **attività pubblicazione sul Web**.)</span><span class="sxs-lookup"><span data-stu-id="796f4-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="796f4-114">Quando Visual Studio ha completato la distribuzione dell'app, il browser predefinito verrà aperto automaticamente l'URL del sito Web distribuito e l'applicazione creata è in esecuzione nel cloud.</span><span class="sxs-lookup"><span data-stu-id="796f4-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="796f4-115">L'URL nella barra degli indirizzi del browser mostra che il sito viene caricato da Internet.</span><span class="sxs-lookup"><span data-stu-id="796f4-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="796f4-116">Distribuzione in un nuovo sito Web</span><span class="sxs-lookup"><span data-stu-id="796f4-116">Deploying to a New Website</span></span>

<span data-ttu-id="796f4-117">Se non si seleziona **ospita nel Cloud** quando si crea il progetto, è possibile configurare ora una nuova app web.</span><span class="sxs-lookup"><span data-stu-id="796f4-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="796f4-118">In Esplora soluzioni fare clic sul progetto e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="796f4-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="796f4-119">Selezionare il **profilo** scheda e fare clic su **siti Web di Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="796f4-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="796f4-120">Se non sono attualmente connessi ad Azure, verrà richiesto di accedere.</span><span class="sxs-lookup"><span data-stu-id="796f4-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="796f4-121">Nel **siti Web esistenti** finestra di dialogo, fare clic su **New**.</span><span class="sxs-lookup"><span data-stu-id="796f4-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="796f4-122">Immettere un nome di sito.</span><span class="sxs-lookup"><span data-stu-id="796f4-122">Enter a site name.</span></span> <span data-ttu-id="796f4-123">Selezionare la sottoscrizione di Azure e l'area.</span><span class="sxs-lookup"><span data-stu-id="796f4-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="796f4-124">Sotto **passava**, selezionare **Crea nuovo Server**, o selezionare un server esistente.</span><span class="sxs-lookup"><span data-stu-id="796f4-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="796f4-125">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="796f4-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="796f4-126">Scegliere il **le impostazioni** scheda e selezionare &quot;Esegui migrazioni Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="796f4-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="796f4-127">Quindi fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="796f4-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="796f4-128">Precedente</span><span class="sxs-lookup"><span data-stu-id="796f4-128">Previous</span></span>](part-9.md)
