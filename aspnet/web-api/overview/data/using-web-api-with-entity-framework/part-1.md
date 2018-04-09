---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Using Web API 2 con Entity Framework 6 | Documenti Microsoft
author: MikeWasson
description: In questa esercitazione verrà fornite le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end. L'esercitazione Usa Entity Framework 6 per il layout dei dati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 8e6d381509a121e3036ca3af91ea3b9bd0be33c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="6a561-104">Using Web API 2 con Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6a561-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="6a561-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6a561-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6a561-106">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="6a561-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="6a561-107">In questa esercitazione verrà fornite le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end.</span><span class="sxs-lookup"><span data-stu-id="6a561-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="6a561-108">L'esercitazione Usa Entity Framework 6 per il livello dati e Knockout.js per l'applicazione di JavaScript sul lato client.</span><span class="sxs-lookup"><span data-stu-id="6a561-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="6a561-109">Nell'esercitazione viene inoltre illustrato come distribuire l'app in App Web di servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="6a561-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6a561-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="6a561-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6a561-111">2.1 API Web</span><span class="sxs-lookup"><span data-stu-id="6a561-111">Web API 2.1</span></span>
> - [<span data-ttu-id="6a561-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="6a561-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="6a561-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6a561-113">Entity Framework 6</span></span>
> - <span data-ttu-id="6a561-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6a561-114">.NET 4.5</span></span>
> - <span data-ttu-id="6a561-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="6a561-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="6a561-116">In questa esercitazione Usa ASP.NET Web API 2 con Entity Framework 6 per creare un'applicazione web che consente di modificare un database back-end.</span><span class="sxs-lookup"><span data-stu-id="6a561-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="6a561-117">Di seguito è riportata una schermata dell'applicazione che verrà creato.</span><span class="sxs-lookup"><span data-stu-id="6a561-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="6a561-118">L'app Usa una progettazione di applicazioni a pagina singola (SPA).</span><span class="sxs-lookup"><span data-stu-id="6a561-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="6a561-119">"L'applicazione a pagina singola" è il termine generale per un'applicazione web che consente di caricare una singola pagina HTML e quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine.</span><span class="sxs-lookup"><span data-stu-id="6a561-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="6a561-120">Dopo il caricamento della pagina iniziale, l'applicazione comunica con il server tramite le richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="6a561-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="6a561-121">Il AJAX richiede dati JSON restituito, che l'app Usa per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="6a561-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="6a561-122">AJAX non è nuovo, ma esistono oggi Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="6a561-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="6a561-123">Questa esercitazione viene utilizzato [Knockout.js](http://knockoutjs.com/), ma è possibile utilizzare qualsiasi framework JavaScript di client.</span><span class="sxs-lookup"><span data-stu-id="6a561-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="6a561-124">Di seguito sono i blocchi principali per l'app:</span><span class="sxs-lookup"><span data-stu-id="6a561-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="6a561-125">ASP.NET MVC consente di creare la pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="6a561-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="6a561-126">API Web ASP.NET gestisce le richieste AJAX e restituisce i dati JSON.</span><span class="sxs-lookup"><span data-stu-id="6a561-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="6a561-127">Knockout.js Associa dati degli elementi HTML per i dati JSON.</span><span class="sxs-lookup"><span data-stu-id="6a561-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="6a561-128">Entity Framework comunica con il database.</span><span class="sxs-lookup"><span data-stu-id="6a561-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="6a561-129">Vedere l'App in esecuzione in Azure</span><span class="sxs-lookup"><span data-stu-id="6a561-129">See this App Running on Azure</span></span>

<span data-ttu-id="6a561-130">Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale?</span><span class="sxs-lookup"><span data-stu-id="6a561-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="6a561-131">È possibile distribuire una versione completa dell'app al proprio account Azure, facendo semplicemente clic sul pulsante seguente.</span><span class="sxs-lookup"><span data-stu-id="6a561-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="6a561-132">È necessario un account Azure per distribuire questa soluzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="6a561-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="6a561-133">Se si dispone già di un account, sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6a561-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="6a561-134">[Aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="6a561-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="6a561-135">[Attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="6a561-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="6a561-136">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="6a561-136">Create the Project</span></span>

<span data-ttu-id="6a561-137">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a561-137">Open Visual Studio.</span></span> <span data-ttu-id="6a561-138">Dal **File** dal menu **New**, quindi selezionare **progetto**.</span><span class="sxs-lookup"><span data-stu-id="6a561-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="6a561-139">(Oppure fare clic su **nuovo progetto** nella pagina Start.)</span><span class="sxs-lookup"><span data-stu-id="6a561-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="6a561-140">Nel **nuovo progetto** finestra di dialogo, fare clic su **Web** nel riquadro a sinistra e **applicazione Web ASP.NET** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="6a561-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="6a561-141">Denominare il progetto BookService e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a561-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="6a561-142">Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **API Web** modello.</span><span class="sxs-lookup"><span data-stu-id="6a561-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="6a561-143">Se si desidera ospitare il progetto in un servizio App di Azure, lasciare il **Host nel cloud** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="6a561-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="6a561-144">Fare clic su **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="6a561-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="6a561-145">Configurare le impostazioni di Azure (facoltative)</span><span class="sxs-lookup"><span data-stu-id="6a561-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="6a561-146">Se si lascia il **Host nel Cloud** opzione selezionata, Visual Studio verrà richiesto di accedere a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="6a561-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="6a561-147">Dopo l'accesso ad Azure, Visual Studio viene richiesto di configurare l'app web.</span><span class="sxs-lookup"><span data-stu-id="6a561-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="6a561-148">Immettere un nome per il sito, selezionare la sottoscrizione di Azure e selezionare un'area geografica.</span><span class="sxs-lookup"><span data-stu-id="6a561-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="6a561-149">In **server di Database**selezionare **Crea nuovo server**.</span><span class="sxs-lookup"><span data-stu-id="6a561-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="6a561-150">Immettere un nome utente amministratore e una password.</span><span class="sxs-lookup"><span data-stu-id="6a561-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="6a561-151">avanti</span><span class="sxs-lookup"><span data-stu-id="6a561-151">Next</span></span>](part-2.md)
