---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Using Web API 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione insegnerà le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end. L'esercitazione Usa Entity Framework 6 per il layout dei dati...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 4abe0e06dfd927765efd8e566584e111cf4117d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827098"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="a00d4-104">Using Web API 2 con Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a00d4-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="a00d4-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a00d4-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a00d4-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="a00d4-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="a00d4-107">Questa esercitazione insegnerà le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end.</span><span class="sxs-lookup"><span data-stu-id="a00d4-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="a00d4-108">L'esercitazione Usa Entity Framework 6 per il livello di dati e Knockout. js per l'applicazione JavaScript lato client.</span><span class="sxs-lookup"><span data-stu-id="a00d4-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="a00d4-109">L'esercitazione illustra anche come distribuire l'app in App Web di servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="a00d4-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a00d4-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="a00d4-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a00d4-111">API Web 2.1</span><span class="sxs-lookup"><span data-stu-id="a00d4-111">Web API 2.1</span></span>
> - [<span data-ttu-id="a00d4-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="a00d4-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="a00d4-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a00d4-113">Entity Framework 6</span></span>
> - <span data-ttu-id="a00d4-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a00d4-114">.NET 4.5</span></span>
> - <span data-ttu-id="a00d4-115">[Knockout. js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="a00d4-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="a00d4-116">Questa esercitazione Usa l'API Web ASP.NET 2 con Entity Framework 6 per creare un'applicazione web che consente di modificare un database back-end.</span><span class="sxs-lookup"><span data-stu-id="a00d4-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="a00d4-117">Di seguito è riportata una schermata dell'applicazione che verrà creato.</span><span class="sxs-lookup"><span data-stu-id="a00d4-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="a00d4-118">L'app Usa un progetto di applicazione a singola pagina (SPA).</span><span class="sxs-lookup"><span data-stu-id="a00d4-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="a00d4-119">"Applicazione a singola pagina" è il termine generale per un'applicazione web che carica una singola pagina HTML, quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine.</span><span class="sxs-lookup"><span data-stu-id="a00d4-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="a00d4-120">Dopo il caricamento della pagina iniziale, l'app comunica con il server tramite le richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="a00d4-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="a00d4-121">Il AJAX richiede i dati JSON restituiti, l'app Usa per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="a00d4-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="a00d4-122">AJAX non è una novità, ma oggi esistono Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="a00d4-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="a00d4-123">Questa esercitazione viene usato [Knockout. js](http://knockoutjs.com/), ma è possibile usare qualsiasi framework JavaScript sul client.</span><span class="sxs-lookup"><span data-stu-id="a00d4-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="a00d4-124">Ecco gli elementi fondamentali per questa app:</span><span class="sxs-lookup"><span data-stu-id="a00d4-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="a00d4-125">ASP.NET MVC consente di creare la pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="a00d4-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="a00d4-126">API Web ASP.NET gestisce le richieste AJAX e restituisce i dati JSON.</span><span class="sxs-lookup"><span data-stu-id="a00d4-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="a00d4-127">Knockout. js Associa dati gli elementi HTML per i dati JSON.</span><span class="sxs-lookup"><span data-stu-id="a00d4-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="a00d4-128">Entity Framework comunica con il database.</span><span class="sxs-lookup"><span data-stu-id="a00d4-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="a00d4-129">Vedere questa App in esecuzione in Azure</span><span class="sxs-lookup"><span data-stu-id="a00d4-129">See this App Running on Azure</span></span>

<span data-ttu-id="a00d4-130">Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale?</span><span class="sxs-lookup"><span data-stu-id="a00d4-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="a00d4-131">È possibile distribuire una versione completa dell'app al tuo account Azure, facendo semplicemente clic sul pulsante seguente.</span><span class="sxs-lookup"><span data-stu-id="a00d4-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="a00d4-132">È necessario un account di Azure per distribuire questa soluzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="a00d4-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="a00d4-133">Se non hai già un account, sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a00d4-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="a00d4-134">[Aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.</span><span class="sxs-lookup"><span data-stu-id="a00d4-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="a00d4-135">[Attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="a00d4-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="a00d4-136">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="a00d4-136">Create the Project</span></span>

<span data-ttu-id="a00d4-137">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a00d4-137">Open Visual Studio.</span></span> <span data-ttu-id="a00d4-138">Dal **File** dal menu **New**, quindi selezionare **progetto**.</span><span class="sxs-lookup"><span data-stu-id="a00d4-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="a00d4-139">(Oppure fare clic su **nuovo progetto** nella paginainiziale.)</span><span class="sxs-lookup"><span data-stu-id="a00d4-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="a00d4-140">Nel **nuovo progetto** finestra di dialogo, fare clic su **Web** nel riquadro sinistro e **applicazione Web ASP.NET** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="a00d4-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="a00d4-141">Denominare il progetto BookService e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a00d4-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="a00d4-142">Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **API Web** modello.</span><span class="sxs-lookup"><span data-stu-id="a00d4-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="a00d4-143">Se si vuole ospitare il progetto in un servizio App di Azure, lasciare il **ospita nel cloud** casella selezionata.</span><span class="sxs-lookup"><span data-stu-id="a00d4-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="a00d4-144">Fare clic su **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="a00d4-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="a00d4-145">Configurare le impostazioni di Azure (facoltative)</span><span class="sxs-lookup"><span data-stu-id="a00d4-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="a00d4-146">Se si lascia il **ospita nel Cloud** opzione selezionata, Visual Studio verrà richiesto di accedere a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="a00d4-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="a00d4-147">Dopo l'accesso ad Azure, Visual Studio viene richiesto di configurare l'app web.</span><span class="sxs-lookup"><span data-stu-id="a00d4-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="a00d4-148">Immettere un nome per il sito, selezionare la sottoscrizione di Azure e selezionare un'area geografica.</span><span class="sxs-lookup"><span data-stu-id="a00d4-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="a00d4-149">Sotto **passava**, selezionare **Crea nuovo server**.</span><span class="sxs-lookup"><span data-stu-id="a00d4-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="a00d4-150">Immettere un nome utente dell'amministratore e una password.</span><span class="sxs-lookup"><span data-stu-id="a00d4-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="a00d4-151">avanti</span><span class="sxs-lookup"><span data-stu-id="a00d4-151">Next</span></span>](part-2.md)
