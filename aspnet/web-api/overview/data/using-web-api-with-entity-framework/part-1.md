---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Using Web API 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione insegnerà le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end. L'esercitazione Usa Entity Framework 6 per il layout dei dati...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236523"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="31ed5-104">Using Web API 2 con Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="31ed5-104">Using Web API 2 with Entity Framework 6</span></span>
====================

[<span data-ttu-id="31ed5-105">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="31ed5-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="31ed5-106">Questa esercitazione illustra le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end.</span><span class="sxs-lookup"><span data-stu-id="31ed5-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="31ed5-107">L'esercitazione Usa Entity Framework 6 per il livello di dati e Knockout. js per l'applicazione JavaScript lato client.</span><span class="sxs-lookup"><span data-stu-id="31ed5-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="31ed5-108">L'esercitazione illustra anche come distribuire l'app in App Web di servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="31ed5-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="31ed5-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="31ed5-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="31ed5-110">API Web 2.1</span><span class="sxs-lookup"><span data-stu-id="31ed5-110">Web API 2.1</span></span>
> - <span data-ttu-id="31ed5-111">Visual Studio 2017 (download di Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="31ed5-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="31ed5-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="31ed5-112">Entity Framework 6</span></span>
> - <span data-ttu-id="31ed5-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="31ed5-113">.NET 4.7</span></span>
> - <span data-ttu-id="31ed5-114">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="31ed5-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="31ed5-115">Questa esercitazione Usa l'API Web ASP.NET 2 con Entity Framework 6 per creare un'applicazione web che consente di modificare un database back-end.</span><span class="sxs-lookup"><span data-stu-id="31ed5-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="31ed5-116">Di seguito è riportata una schermata dell'applicazione che verrà creato.</span><span class="sxs-lookup"><span data-stu-id="31ed5-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="31ed5-117">L'app Usa un progetto di applicazione a singola pagina (SPA).</span><span class="sxs-lookup"><span data-stu-id="31ed5-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="31ed5-118">"Applicazione a singola pagina" è il termine generale per un'applicazione web che carica una singola pagina HTML, quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine.</span><span class="sxs-lookup"><span data-stu-id="31ed5-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="31ed5-119">Dopo il caricamento della pagina iniziale, l'app comunica con il server tramite le richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="31ed5-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="31ed5-120">Il AJAX richiede i dati JSON restituiti, l'app Usa per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="31ed5-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="31ed5-121">AJAX non è una novità, ma oggi esistono Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="31ed5-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="31ed5-122">Questa esercitazione viene usato [Knockout. js](http://knockoutjs.com/), ma è possibile usare qualsiasi framework JavaScript sul client.</span><span class="sxs-lookup"><span data-stu-id="31ed5-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="31ed5-123">Ecco gli elementi fondamentali per questa app:</span><span class="sxs-lookup"><span data-stu-id="31ed5-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="31ed5-124">ASP.NET MVC consente di creare la pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="31ed5-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="31ed5-125">API Web ASP.NET gestisce le richieste AJAX e restituisce i dati JSON.</span><span class="sxs-lookup"><span data-stu-id="31ed5-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="31ed5-126">Knockout. js Associa dati gli elementi HTML per i dati JSON.</span><span class="sxs-lookup"><span data-stu-id="31ed5-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="31ed5-127">Entity Framework comunica con il database.</span><span class="sxs-lookup"><span data-stu-id="31ed5-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="31ed5-128">Vedere questa app in esecuzione in Azure</span><span class="sxs-lookup"><span data-stu-id="31ed5-128">See this app running on Azure</span></span>

<span data-ttu-id="31ed5-129">Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale?</span><span class="sxs-lookup"><span data-stu-id="31ed5-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="31ed5-130">È possibile distribuire una versione completa dell'app all'account di Azure selezionando il pulsante seguente.</span><span class="sxs-lookup"><span data-stu-id="31ed5-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="31ed5-131">È necessario un account di Azure per distribuire questa soluzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="31ed5-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="31ed5-132">Se non hai già un account, sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="31ed5-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="31ed5-133">[Aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.</span><span class="sxs-lookup"><span data-stu-id="31ed5-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="31ed5-134">[Attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="31ed5-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="31ed5-135">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="31ed5-135">Create the project</span></span>

<span data-ttu-id="31ed5-136">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31ed5-136">Open Visual Studio.</span></span> <span data-ttu-id="31ed5-137">Dal **File** dal menu **New**, quindi selezionare **progetto**.</span><span class="sxs-lookup"><span data-stu-id="31ed5-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="31ed5-138">(O selezionare **nuovo progetto** nella paginainiziale.)</span><span class="sxs-lookup"><span data-stu-id="31ed5-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="31ed5-139">Nel **nuovo progetto** finestra di dialogo, seleziona **Web** nel riquadro sinistro e **applicazione Web ASP.NET (.NET Framework)** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="31ed5-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="31ed5-140">Denominare il progetto **BookService** e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="31ed5-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="31ed5-141">Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **API Web** modello.</span><span class="sxs-lookup"><span data-stu-id="31ed5-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


<span data-ttu-id="31ed5-142">Selezionare **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="31ed5-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="31ed5-143">Configurare le impostazioni di Azure (facoltative)</span><span class="sxs-lookup"><span data-stu-id="31ed5-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="31ed5-144">Dopo aver creato il progetto, è possibile distribuire in App Web di servizio App di Azure in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="31ed5-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="31ed5-145">In Esplora soluzioni fare doppio clic sul progetto, quindi scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="31ed5-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="31ed5-146">Nella finestra visualizzata, selezionare **avviare**.</span><span class="sxs-lookup"><span data-stu-id="31ed5-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="31ed5-147">Il **selezionare una destinazione di pubblicazione** verrà visualizzata la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="31ed5-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="31ed5-148">Selezionare **Crea profilo**.</span><span class="sxs-lookup"><span data-stu-id="31ed5-148">Select **Create Profile**.</span></span> <span data-ttu-id="31ed5-149">Viene visualizzata la finestra di dialogo **Crea servizio app**.</span><span class="sxs-lookup"><span data-stu-id="31ed5-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="31ed5-150">Accettare le impostazioni predefinite oppure immettere valori diversi per il nome dell'applicazione, gruppo di risorse, area geografica, sottoscrizione di Azure e piano di hosting.</span><span class="sxs-lookup"><span data-stu-id="31ed5-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="31ed5-151">Selezionare **creare un database SQL**.</span><span class="sxs-lookup"><span data-stu-id="31ed5-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="31ed5-152">Il **configurare SQL Server** verrà visualizzata la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="31ed5-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="31ed5-153">Accettare i valori predefiniti o immettere valori diversi.</span><span class="sxs-lookup"><span data-stu-id="31ed5-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="31ed5-154">Immettere un **Adminstrator Username** e **Password dell'amministratore** per il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="31ed5-154">Enter an **Adminstrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="31ed5-155">Selezionare **OK** dopo aver completato.</span><span class="sxs-lookup"><span data-stu-id="31ed5-155">Select **OK** when you're done.</span></span> <span data-ttu-id="31ed5-156">Il **Crea servizio App** pagina viene visualizzata di nuovo.</span><span class="sxs-lookup"><span data-stu-id="31ed5-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="31ed5-157">Selezionare **Create** per creare il profilo.</span><span class="sxs-lookup"><span data-stu-id="31ed5-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="31ed5-158">Viene visualizzato un messaggio nell'angolo in basso a destra che indica che la distribuzione è in corso.</span><span class="sxs-lookup"><span data-stu-id="31ed5-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="31ed5-159">Dopo un breve periodo di tempo, il **pubblica** viene visualizzata la finestra.</span><span class="sxs-lookup"><span data-stu-id="31ed5-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="31ed5-160">Il profilo creato per distribuire l'app è ora disponibile.</span><span class="sxs-lookup"><span data-stu-id="31ed5-160">The profile you created to deploy the app is now available.</span></span> 


> [!div class="step-by-step"]
> [<span data-ttu-id="31ed5-161">avanti</span><span class="sxs-lookup"><span data-stu-id="31ed5-161">Next</span></span>](part-2.md)
