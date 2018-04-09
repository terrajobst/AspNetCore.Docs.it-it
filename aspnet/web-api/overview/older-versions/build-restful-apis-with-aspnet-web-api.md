---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Compilare le API REST con ASP.NET Web API | Documenti Microsoft
author: rick-anderson
description: Negli ultimi anni, è diventato evidente che HTTP non è presente solo per mettere a disposizione le pagine HTML. È anche una potente piattaforma per la compilazione di API Web, utilizzando un numero limitato di o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 320409cd395384a608a07307a56d18105d45de14
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="9b368-104">Compilare le API REST con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="9b368-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="9b368-105">Da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="9b368-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="9b368-106">Negli ultimi anni, è diventato evidente che HTTP non è presente solo per mettere a disposizione le pagine HTML.</span><span class="sxs-lookup"><span data-stu-id="9b368-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="9b368-107">È inoltre una potente piattaforma per la compilazione di API Web, utilizzando un numero limitato di verbi (GET, POST e così via) e alcuni concetti semplici, ad esempio *URI* e *intestazioni*.</span><span class="sxs-lookup"><span data-stu-id="9b368-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="9b368-108">API Web ASP.NET è un set di componenti che semplificano la programmazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b368-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="9b368-109">Poiché è basato sul runtime di ASP.NET MVC, API Web gestisce automaticamente i dettagli di basso livello di trasporto HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b368-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="9b368-110">Allo stesso tempo, API Web espone naturalmente il modello di programmazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b368-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="9b368-111">In effetti, è uno degli obiettivi di API Web *non* astrarre la realtà di HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b368-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="9b368-112">Di conseguenza, l'API Web è flessibile e di facile estendere.</span><span class="sxs-lookup"><span data-stu-id="9b368-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="9b368-113">In questa esercitazione pratica, si utilizzerà API Web per compilare una semplice API REST per un'applicazione Gestione contatti.</span><span class="sxs-lookup"><span data-stu-id="9b368-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="9b368-114">Si creerà anche un client per utilizzare l'API.</span><span class="sxs-lookup"><span data-stu-id="9b368-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="9b368-115">Lo stile dell'architettura REST ha dimostrato di essere un metodo efficace per sfruttare HTTP, anche se non è certamente l'approccio valido solo per HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b368-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="9b368-116">Gestione contatti esporrà il RESTful per elencare, aggiunta ed eliminazione di contatti, tra gli altri.</span><span class="sxs-lookup"><span data-stu-id="9b368-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="9b368-117">Questa esercitazione richiede una conoscenza di base HTTP, REST e si suppone una conoscenza di base di HTML, JavaScript e jQuery.</span><span class="sxs-lookup"><span data-stu-id="9b368-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="9b368-118">Il sito Web ASP.NET dispone di un'area dedicata per il framework di ASP.NET Web API nel [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="9b368-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="9b368-119">Questo sito continuerà a fornire le informazioni più recenti, esempi e le notizie relative all'API Web, quindi archiviarlo spesso se si desidera approfondire la conoscenza arte della creazione di API Web personalizzate disponibili per qualsiasi framework di sviluppo o dispositivo.</span><span class="sxs-lookup"><span data-stu-id="9b368-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="9b368-120">ASP.NET Web API, simile a ASP.NET MVC 4, ha una grande flessibilità in termini di separare il livello di servizio dai controller di che consente di utilizzare alcuni dei framework Dependency Injection disponibili piuttosto semplice.</span><span class="sxs-lookup"><span data-stu-id="9b368-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="9b368-121">È un buon esempio in MSDN che illustra come utilizzare Ninject per l'inserimento di dipendenze in un progetto di API Web ASP.NET che è possibile scaricarlo da [qui](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="9b368-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="9b368-122">Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="9b368-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="9b368-123">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="9b368-123">Objectives</span></span>

<span data-ttu-id="9b368-124">In questa esercitazione pratica, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="9b368-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="9b368-125">Implementare un'API Web RESTful</span><span class="sxs-lookup"><span data-stu-id="9b368-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="9b368-126">Chiamare l'API da un client HTML</span><span class="sxs-lookup"><span data-stu-id="9b368-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="9b368-127">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9b368-127">Prerequisites</span></span>

<span data-ttu-id="9b368-128">Di seguito è necessario per completare questa esercitazione pratica:</span><span class="sxs-lookup"><span data-stu-id="9b368-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="9b368-129">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice B](#AppendixB) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="9b368-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="9b368-130">Configurazione</span><span class="sxs-lookup"><span data-stu-id="9b368-130">Setup</span></span>

<span data-ttu-id="9b368-131">**L'installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="9b368-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="9b368-132">Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b368-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="9b368-133">Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="9b368-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="9b368-134">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice a: con i frammenti di codice](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="9b368-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="9b368-135">Esercizi</span><span class="sxs-lookup"><span data-stu-id="9b368-135">Exercises</span></span>

<span data-ttu-id="9b368-136">Questa esercitazione include le procedure descritte di seguito:</span><span class="sxs-lookup"><span data-stu-id="9b368-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="9b368-137">Esercizio 1: Creare una sola lettura Web API</span><span class="sxs-lookup"><span data-stu-id="9b368-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="9b368-138">Esercizio 2: Creare un'API Web di lettura/scrittura</span><span class="sxs-lookup"><span data-stu-id="9b368-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="9b368-139">Esercizio 3: Utilizzare l'API Web da un Client HTML</span><span class="sxs-lookup"><span data-stu-id="9b368-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="9b368-140">Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio.</span><span class="sxs-lookup"><span data-stu-id="9b368-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="9b368-141">Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="9b368-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="9b368-142">Tempo stimato per completare questa esercitazione: **60 minuti**.</span><span class="sxs-lookup"><span data-stu-id="9b368-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="9b368-143">Esercizio 1: Creare una sola lettura Web API</span><span class="sxs-lookup"><span data-stu-id="9b368-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="9b368-144">In questo esercizio si implementerà i metodi GET di sola lettura per la gestione di contatto.</span><span class="sxs-lookup"><span data-stu-id="9b368-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="9b368-145">Attività 1 - Creazione del progetto di API</span><span class="sxs-lookup"><span data-stu-id="9b368-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="9b368-146">In questa attività, utilizzare i nuovi modelli di progetto web ASP.NET per creare un'applicazione web API Web.</span><span class="sxs-lookup"><span data-stu-id="9b368-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="9b368-147">Eseguire **Visual Studio Express 2012 per Web**, a tale scopo passare alla **avviare** e tipo **Visual Studio Express per Web** premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="9b368-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="9b368-148">Dal **File** dal menu **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="9b368-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="9b368-149">Selezionare il **Visual c# | Web** tipo nella visualizzazione ad albero di progetto tipo di progetto, quindi selezionare il **applicazione Web ASP.NET MVC 4** tipo di progetto.</span><span class="sxs-lookup"><span data-stu-id="9b368-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="9b368-150">Impostare il progetto **nome** a *ContactManager* e **Nome soluzione** a *iniziare*, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b368-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="9b368-151">![Creazione di un nuovo progetto ASP.NET MVC 4.0 Web Application](build-restful-apis-with-aspnet-web-api/_static/image1.png "la creazione di un nuovo progetto ASP.NET MVC 4.0 Web dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="9b368-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="9b368-152">*Creazione di un nuovo progetto ASP.NET MVC 4.0 Web dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="9b368-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="9b368-153">Nella finestra di tipo di progetto ASP.NET MVC 4, selezionare il **API Web** tipo di progetto.</span><span class="sxs-lookup"><span data-stu-id="9b368-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="9b368-154">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b368-154">Click **OK**.</span></span>

    <span data-ttu-id="9b368-155">![Che specifica il tipo di progetto di Web API](build-restful-apis-with-aspnet-web-api/_static/image2.png "che specifica il tipo di progetto API Web")</span><span class="sxs-lookup"><span data-stu-id="9b368-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="9b368-156">*Che specifica il tipo di progetto API Web*</span><span class="sxs-lookup"><span data-stu-id="9b368-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="9b368-157">Attività 2 - Creazione i controller API Contact Manager</span><span class="sxs-lookup"><span data-stu-id="9b368-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="9b368-158">In questa attività si creerà le classi controller in cui risiederà metodi dell'API.</span><span class="sxs-lookup"><span data-stu-id="9b368-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="9b368-159">Eliminare il file denominato **ValuesController.cs** all'interno di **controller** cartella dal progetto.</span><span class="sxs-lookup"><span data-stu-id="9b368-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="9b368-160">Fare doppio clic su di **controller** cartella nel progetto e selezionare **Aggiungi | Controller** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="9b368-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="9b368-161">![Aggiunge un nuovo controller al progetto](build-restful-apis-with-aspnet-web-api/_static/image3.png "aggiunge un nuovo controller al progetto")</span><span class="sxs-lookup"><span data-stu-id="9b368-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="9b368-162">*Aggiunge un nuovo controller al progetto*</span><span class="sxs-lookup"><span data-stu-id="9b368-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="9b368-163">Nel **Aggiungi Controller** finestra di dialogo visualizzata, selezionare **Controller API vuoto** dal menu modello.</span><span class="sxs-lookup"><span data-stu-id="9b368-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="9b368-164">Nome della classe controller **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="9b368-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="9b368-165">Quindi, fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="9b368-165">Then, click **Add.**</span></span>

    <span data-ttu-id="9b368-166">![Utilizzando la finestra di dialogo Aggiungi Controller per creare un nuovo controller Web API](build-restful-apis-with-aspnet-web-api/_static/image4.png "utilizzando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web")</span><span class="sxs-lookup"><span data-stu-id="9b368-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="9b368-167">*Utilizzando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web*</span><span class="sxs-lookup"><span data-stu-id="9b368-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="9b368-168">Aggiungere il codice seguente per il **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="9b368-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="9b368-169">(- Frammento di codice *Web API Lab - Ex01 - ottenere il metodo API*)</span><span class="sxs-lookup"><span data-stu-id="9b368-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="9b368-170">Premere **F5** il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="9b368-171">La home page predefinita per un progetto di API Web compariranno.</span><span class="sxs-lookup"><span data-stu-id="9b368-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="9b368-172">![La home page predefinita di un'applicazione ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "la home page predefinita di un'applicazione ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="9b368-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="9b368-173">*La home page predefinita di un'applicazione ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="9b368-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="9b368-174">Nella finestra di Internet Explorer, premere il **F12** tasto per aprire la **gli strumenti di sviluppo** finestra.</span><span class="sxs-lookup"><span data-stu-id="9b368-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="9b368-175">Fare clic sul **rete** scheda e quindi scegliere il **Avvia acquisizione** pulsante per avviare l'acquisizione del traffico di rete nella finestra.</span><span class="sxs-lookup"><span data-stu-id="9b368-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="9b368-176">![Acquisizione di rete aprire la scheda di rete e di avvio](build-restful-apis-with-aspnet-web-api/_static/image6.png "all'apertura della scheda di rete e di avvio di acquisizione di rete")</span><span class="sxs-lookup"><span data-stu-id="9b368-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="9b368-177">*Aprire la scheda di rete e avviare l'acquisizione di rete*</span><span class="sxs-lookup"><span data-stu-id="9b368-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="9b368-178">Aggiungere l'URL nella barra degli indirizzi del browser con **/api/contatto** e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="9b368-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="9b368-179">I dettagli di trasmissione verranno visualizzata la finestra di acquisizione di rete.</span><span class="sxs-lookup"><span data-stu-id="9b368-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="9b368-180">Si noti che è di tipo MIME della risposta **application/json**.</span><span class="sxs-lookup"><span data-stu-id="9b368-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="9b368-181">Ciò dimostra come formato di output predefinito è JSON.</span><span class="sxs-lookup"><span data-stu-id="9b368-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="9b368-182">![Visualizzazione dell'output della richiesta API Web nella visualizzazione rete](build-restful-apis-with-aspnet-web-api/_static/image7.png "Visualizza l'output della richiesta API Web nella visualizzazione rete")</span><span class="sxs-lookup"><span data-stu-id="9b368-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="9b368-183">*Visualizzazione dell'output della richiesta API Web nella visualizzazione rete*</span><span class="sxs-lookup"><span data-stu-id="9b368-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9b368-184">Comportamento predefinito di Internet Explorer 10 a questo punto è necessario chiedere se si desidera salvare o aprire il flusso risultante dalla chiamata all'API Web.</span><span class="sxs-lookup"><span data-stu-id="9b368-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="9b368-185">L'output sarà un file di testo contenente il risultato JSON della chiamata di URL di Web API.</span><span class="sxs-lookup"><span data-stu-id="9b368-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="9b368-186">Non annullare la finestra di dialogo per poter visualizzare il contenuto della risposta tramite una finestra degli strumenti sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="9b368-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="9b368-187">Fare clic su di **passare alla visualizzazione dettagliata** pulsante per visualizzare ulteriori dettagli sulla risposta di questa chiamata API.</span><span class="sxs-lookup"><span data-stu-id="9b368-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="9b368-188">![Passare alla visualizzazione dettagliata](build-restful-apis-with-aspnet-web-api/_static/image8.png "passare alla visualizzazione dettagli")</span><span class="sxs-lookup"><span data-stu-id="9b368-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="9b368-189">*Passare alla visualizzazione dettagliata*</span><span class="sxs-lookup"><span data-stu-id="9b368-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="9b368-190">Fare clic su di **corpo della risposta** scheda per visualizzare il testo di risposta JSON effettivo.</span><span class="sxs-lookup"><span data-stu-id="9b368-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="9b368-191">![Visualizzazione JSON output di testo in network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "visualizzazione JSON output di testo in network monitor")</span><span class="sxs-lookup"><span data-stu-id="9b368-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="9b368-192">*Visualizzazione del testo di output JSON in network monitor*</span><span class="sxs-lookup"><span data-stu-id="9b368-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="9b368-193">Attività 3: creazione di modelli di contatto e aumentano il Controller di contatto</span><span class="sxs-lookup"><span data-stu-id="9b368-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="9b368-194">In questa attività si creerà le classi controller in cui risiederà metodi dell'API.</span><span class="sxs-lookup"><span data-stu-id="9b368-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="9b368-195">Fare doppio clic su di **modelli** cartella e selezionare **Aggiungi | Classe...**  dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="9b368-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="9b368-196">![Aggiunta di un nuovo modello per l'applicazione web](build-restful-apis-with-aspnet-web-api/_static/image10.png "aggiunta di un nuovo modello per l'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="9b368-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="9b368-197">*Aggiunta di un nuovo modello per l'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="9b368-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="9b368-198">Nel **Aggiungi nuovo elemento** finestra di dialogo Nome nuovo file **Contact.cs** e fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="9b368-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="9b368-199">![Creazione del nuovo file di classe contatto](build-restful-apis-with-aspnet-web-api/_static/image11.png "creazione del nuovo file di classe di contatto")</span><span class="sxs-lookup"><span data-stu-id="9b368-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="9b368-200">*Creazione del nuovo file di classe di contatto*</span><span class="sxs-lookup"><span data-stu-id="9b368-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="9b368-201">Aggiungere il codice evidenziato di seguito per il **contatto** classe.</span><span class="sxs-lookup"><span data-stu-id="9b368-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="9b368-202">(- Frammento di codice *Web API Lab - Ex01 - contatto classe*)</span><span class="sxs-lookup"><span data-stu-id="9b368-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
~~~
4. <span data-ttu-id="9b368-203">Nel **ContactController** classe, selezionare la parola **stringa** nella definizione di metodo del **ottenere** (metodo) e digitare la parola *contatto*.</span><span class="sxs-lookup"><span data-stu-id="9b368-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="9b368-204">Dopo aver digitata la parola, un indicatore verrà visualizzato all'inizio della parola **contatto**.</span><span class="sxs-lookup"><span data-stu-id="9b368-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="9b368-205">Uno tenendo premuto il **Ctrl** chiave e premere il tasto punto (.) o fare clic sull'icona con il mouse per aprire la finestra di dialogo assistenza nell'editor di codice, compilare automaticamente il **utilizzando** direttiva per i modelli spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="9b368-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Con supporto di Intellisense per le dichiarazioni dello spazio dei nomi](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="9b368-207">*Con supporto di Intellisense per le dichiarazioni dello spazio dei nomi*</span><span class="sxs-lookup"><span data-stu-id="9b368-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="9b368-208">Modificare il codice per il **ottenere** metodo in modo che restituisca una matrice di istanze di modello di contatto.</span><span class="sxs-lookup"><span data-stu-id="9b368-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="9b368-209">(- Frammento di codice *Web API Lab - Ex01 - restituzione di un elenco di contatti*)</span><span class="sxs-lookup"><span data-stu-id="9b368-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="9b368-210">Premere **F5** il debug dell'applicazione web nel browser.</span><span class="sxs-lookup"><span data-stu-id="9b368-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="9b368-211">Per visualizzare le modifiche apportate all'output di risposta dell'API, eseguire la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="9b368-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="9b368-212">Una volta aperto il browser, premere **F12** se gli strumenti di sviluppo non sono ancora aperti.</span><span class="sxs-lookup"><span data-stu-id="9b368-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="9b368-213">Fare clic su di **rete** scheda.</span><span class="sxs-lookup"><span data-stu-id="9b368-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="9b368-214">Premere il **Avvia acquisizione** pulsante.</span><span class="sxs-lookup"><span data-stu-id="9b368-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="9b368-215">Aggiungere il suffisso URL **/api/contatto** per l'URL nella barra degli indirizzi e premere il **invio** chiave.</span><span class="sxs-lookup"><span data-stu-id="9b368-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="9b368-216">Premere il **passare alla visualizzazione dettagliata** pulsante.</span><span class="sxs-lookup"><span data-stu-id="9b368-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="9b368-217">Selezionare il **corpo della risposta** scheda. Dovrebbe essere una stringa JSON che rappresenta il formato serializzato di una matrice di istanze di contatto.</span><span class="sxs-lookup"><span data-stu-id="9b368-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="9b368-218">![JSON serializzato output di una chiamata al metodo API Web complessa](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializzato output di una chiamata al metodo API Web complessa")</span><span class="sxs-lookup"><span data-stu-id="9b368-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="9b368-219">*Output JSON serializzato di una chiamata al metodo API Web complessa*</span><span class="sxs-lookup"><span data-stu-id="9b368-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="9b368-220">Attività 4 - funzionalità di estrazione in un livello di servizio</span><span class="sxs-lookup"><span data-stu-id="9b368-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="9b368-221">Questa attività verrà illustrato come estrarre funzionalità in un livello di servizio per semplificare per gli sviluppatori separare le funzionalità del servizio dal livello dei controller, consentendo la possibilità di riutilizzo dei servizi che effettivamente svolgono il lavoro.</span><span class="sxs-lookup"><span data-stu-id="9b368-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="9b368-222">Creare una nuova cartella nella radice della soluzione e denominarlo **servizi**.</span><span class="sxs-lookup"><span data-stu-id="9b368-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="9b368-223">A tale scopo, fare doppio clic su **ContactManager** progetto, selezionare **Aggiungi** | **nuova cartella**, denominarla *servizi*.</span><span class="sxs-lookup"><span data-stu-id="9b368-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="9b368-224">![Cartella servizi di creazione](build-restful-apis-with-aspnet-web-api/_static/image14.png "cartella creazione di servizi")</span><span class="sxs-lookup"><span data-stu-id="9b368-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="9b368-225">*La creazione della cartella di servizi*</span><span class="sxs-lookup"><span data-stu-id="9b368-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="9b368-226">Fare doppio clic su di **servizi** cartella e selezionare **Aggiungi | Classe...**  dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="9b368-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="9b368-227">![Aggiunta di una nuova classe per la cartella servizi](build-restful-apis-with-aspnet-web-api/_static/image15.png "aggiunta una nuova classe nella cartella Services")</span><span class="sxs-lookup"><span data-stu-id="9b368-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="9b368-228">*Aggiunta di una nuova classe nella cartella Services*</span><span class="sxs-lookup"><span data-stu-id="9b368-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="9b368-229">Quando il **Aggiungi nuovo elemento** viene visualizzata la finestra, denominare la nuova classe **ContactRepository** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="9b368-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="9b368-230">![Creazione di un file di classe per contenere il codice per il livello di servizio Repository contatto](build-restful-apis-with-aspnet-web-api/_static/image16.png "creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatti")</span><span class="sxs-lookup"><span data-stu-id="9b368-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="9b368-231">*Creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatti*</span><span class="sxs-lookup"><span data-stu-id="9b368-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="9b368-232">Aggiungere un tramite la direttiva per il **ContactRepository.cs** file da includere lo spazio dei nomi modelli.</span><span class="sxs-lookup"><span data-stu-id="9b368-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
~~~
5. <span data-ttu-id="9b368-233">Aggiungere il codice evidenziato di seguito per il **ContactRepository.cs** file per implementare il metodo GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="9b368-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="9b368-234">(- Frammento di codice *Web API Lab - Ex01 - Repository di contatti*)</span><span class="sxs-lookup"><span data-stu-id="9b368-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="9b368-235">Aprire il **ContactController.cs** file se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="9b368-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="9b368-236">Aggiungere la seguente istruzione using alla sezione della dichiarazione dello spazio dei nomi del file.</span><span class="sxs-lookup"><span data-stu-id="9b368-236">Add the following using statement to the namespace declaration section of the file.</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
~~~
8. <span data-ttu-id="9b368-237">Aggiungere il codice evidenziato di seguito per il **ContactController.cs** classe per aggiungere un campo privato per rappresentare l'istanza dell'archivio, in modo che il resto della classe i membri possono eseguire usare dell'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="9b368-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="9b368-238">(- Frammento di codice *API Lab - Ex01 - contatto Controller Web*)</span><span class="sxs-lookup"><span data-stu-id="9b368-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="9b368-239">Modifica il **ottenere** metodo in modo che rende utilizzare il servizio di repository di contatti.</span><span class="sxs-lookup"><span data-stu-id="9b368-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="9b368-240">(- Frammento di codice *restituzione di un elenco di contatti tramite l'archivio Web API Lab - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="9b368-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="9b368-241">Inserire un punto di interruzione di **ContactController**del **ottenere** la definizione di metodo.</span><span class="sxs-lookup"><span data-stu-id="9b368-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="9b368-242">![Aggiunta di punti di interruzione al controller di contatto](build-restful-apis-with-aspnet-web-api/_static/image17.png "aggiunta di punti di interruzione al controller di contatto")</span><span class="sxs-lookup"><span data-stu-id="9b368-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="9b368-243">*Aggiunta di punti di interruzione al controller di contatto*</span><span class="sxs-lookup"><span data-stu-id="9b368-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="9b368-244">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="9b368-245">Quando si apre il browser, premere **F12** per aprire gli strumenti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9b368-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="9b368-246">Fare clic su di **rete** scheda.</span><span class="sxs-lookup"><span data-stu-id="9b368-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="9b368-247">Fare clic su di **Avvia acquisizione** pulsante.</span><span class="sxs-lookup"><span data-stu-id="9b368-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="9b368-248">Aggiungere l'URL nella barra degli indirizzi con il suffisso **/api/contatto** e premere **invio** per caricare il controller API.</span><span class="sxs-lookup"><span data-stu-id="9b368-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="9b368-249">Visual Studio 2012 è presente un'interruzione di una volta **ottenere** metodo inizia l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9b368-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="9b368-250">![All'interno del metodo Get di rilievo](build-restful-apis-with-aspnet-web-api/_static/image18.png "interruzione all'interno del metodo Get")</span><span class="sxs-lookup"><span data-stu-id="9b368-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="9b368-251">*Interruzione all'interno del metodo Get*</span><span class="sxs-lookup"><span data-stu-id="9b368-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="9b368-252">Premere **F5** per continuare.</span><span class="sxs-lookup"><span data-stu-id="9b368-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="9b368-253">Tornare a Internet Explorer se non è già attivo.</span><span class="sxs-lookup"><span data-stu-id="9b368-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="9b368-254">Si noti la finestra di acquisizione di rete.</span><span class="sxs-lookup"><span data-stu-id="9b368-254">Note the network capture window.</span></span>

    <span data-ttu-id="9b368-255">![Rete di visualizzazione che mostra i risultati della chiamata API Web di Internet Explorer](build-restful-apis-with-aspnet-web-api/_static/image19.png "vista in Internet Explorer che mostra i risultati della chiamata API Web di rete")</span><span class="sxs-lookup"><span data-stu-id="9b368-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="9b368-256">*Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata API Web*</span><span class="sxs-lookup"><span data-stu-id="9b368-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="9b368-257">Fare clic su di **passare alla visualizzazione dettagliata** pulsante.</span><span class="sxs-lookup"><span data-stu-id="9b368-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="9b368-258">Fare clic su di **corpo della risposta** scheda. Si noti l'output JSON della chiamata API, e come rappresenta due contatti recuperati tramite il livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="9b368-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="9b368-259">![Visualizza l'output JSON dall'API Web nella finestra di strumenti per sviluppatori](build-restful-apis-with-aspnet-web-api/_static/image20.png "visualizzando l'output JSON dalle API Web nella finestra di strumenti per sviluppatori")</span><span class="sxs-lookup"><span data-stu-id="9b368-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="9b368-260">*Visualizza l'output JSON dall'API Web nella finestra di strumenti per sviluppatori*</span><span class="sxs-lookup"><span data-stu-id="9b368-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="9b368-261">Esercizio 2: Creare un'API Web di lettura/scrittura</span><span class="sxs-lookup"><span data-stu-id="9b368-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="9b368-262">In questo esercizio verrà implementata POST e PUT metodi per la gestione di contatto abilitare la funzionalità con le funzionalità di modifica dei dati.</span><span class="sxs-lookup"><span data-stu-id="9b368-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="9b368-263">Attività 1: aprire il progetto API Web</span><span class="sxs-lookup"><span data-stu-id="9b368-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="9b368-264">In questa attività si prepara migliorare il progetto API Web creato nell'esercizio 1, in modo che possa accettare l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="9b368-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="9b368-265">Eseguire **Visual Studio Express 2012 per Web**, a tale scopo passare alla **avviare** e tipo **Visual Studio Express per Web** premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="9b368-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="9b368-266">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex02-ReadWriteWebAPI/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="9b368-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="9b368-267">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="9b368-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9b368-268">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="9b368-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9b368-269">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9b368-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9b368-270">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="9b368-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9b368-271">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="9b368-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9b368-272">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="9b368-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9b368-273">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="9b368-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9b368-274">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="9b368-275">Aprire il **Services/ContactRepository.cs** file.</span><span class="sxs-lookup"><span data-stu-id="9b368-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="9b368-276">Attività 2: aggiunta di funzionalità di persistenza dei dati per l'implementazione del Repository di contatti</span><span class="sxs-lookup"><span data-stu-id="9b368-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="9b368-277">In questa attività, si verrà aumentano la classe ContactRepository del progetto API Web creato nell'esercizio 1 in modo che possa rendere persistente e accettare l'input dell'utente e le nuove istanze di contatto.</span><span class="sxs-lookup"><span data-stu-id="9b368-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="9b368-278">La costante seguente per aggiungere il **ContactRepository** classe per rappresentare il nome del nome del server web cache elemento chiave più avanti in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="9b368-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="9b368-279">Aggiungere un costruttore per il **ContactRepository** contenente il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9b368-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="9b368-280">(- Frammento di codice *Web API Lab - Ex02 - Repository di contatti costruttore*)</span><span class="sxs-lookup"><span data-stu-id="9b368-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="9b368-281">Modificare il codice per il **GetAllContacts** metodo come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9b368-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="9b368-282">(- Frammento di codice *Web API Lab - Ex02 - ottenere tutti i contatti*)</span><span class="sxs-lookup"><span data-stu-id="9b368-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="9b368-283">Questo esempio è a scopo dimostrativo e utilizza la cache del server web come un supporto di archiviazione, in modo che i valori verranno messe a disposizione più client contemporaneamente, anziché utilizzare un meccanismo di archiviazione di sessione o una durata di archiviazione richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b368-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="9b368-284">Una possibile utilizzare Entity Framework, l'archiviazione XML o qualsiasi altra varietà al posto di cache del server web.</span><span class="sxs-lookup"><span data-stu-id="9b368-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="9b368-285">Implementare un nuovo metodo denominato **SaveContact** per il **ContactRepository** classe per le operazioni di salvataggio di un contatto.</span><span class="sxs-lookup"><span data-stu-id="9b368-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="9b368-286">Il **SaveContact** metodo deve accettare un singolo **contatto** parametro e restituire un valore booleano valore indicante l'esito positivo o negativo.</span><span class="sxs-lookup"><span data-stu-id="9b368-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="9b368-287">(- Frammento di codice *Web API Lab - Ex02 - implementazione del metodo SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="9b368-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="9b368-288">Esercizio 3: Utilizzare l'API Web da un Client HTML</span><span class="sxs-lookup"><span data-stu-id="9b368-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="9b368-289">In questo esercizio si creerà un client HTML per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="9b368-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="9b368-290">Questo client faciliterà scambio di dati con l'API Web con JavaScript e i risultati verrà visualizzati in un web browser utilizzando il markup HTML.</span><span class="sxs-lookup"><span data-stu-id="9b368-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="9b368-291">Attività 1: modificare la visualizzazione dell'indice per fornire un'interfaccia utente grafica per la visualizzazione dei contatti</span><span class="sxs-lookup"><span data-stu-id="9b368-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="9b368-292">In questa attività si modificherà la visualizzazione dell'indice predefinito dell'applicazione web per supportare il requisito di visualizzazione dell'elenco di contatti esistenti in un browser HTML.</span><span class="sxs-lookup"><span data-stu-id="9b368-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="9b368-293">Aprire **Visual Studio Express 2012 per Web** se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="9b368-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="9b368-294">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex03-ConsumingWebAPI/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="9b368-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="9b368-295">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="9b368-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9b368-296">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="9b368-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9b368-297">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9b368-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9b368-298">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="9b368-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9b368-299">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="9b368-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9b368-300">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="9b368-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9b368-301">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="9b368-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9b368-302">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="9b368-303">Aprire il **cshtml** file si trova in **Views/Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="9b368-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="9b368-304">Sostituire il codice HTML all'interno dell'elemento div con id **corpo** in modo che risulti simile al codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9b368-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
~~~
5. <span data-ttu-id="9b368-305">Aggiungere il codice Javascript seguente nella parte inferiore del file per eseguire la richiesta HTTP nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="9b368-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>


~~~
[!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
~~~
6. <span data-ttu-id="9b368-306">Aprire il **ContactController.cs** file se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="9b368-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="9b368-307">Inserire un punto di interruzione di **ottenere** metodo il **ContactController** classe.</span><span class="sxs-lookup"><span data-stu-id="9b368-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="9b368-308">![Inserendo un punto di interruzione nel metodo Get del controller API](build-restful-apis-with-aspnet-web-api/_static/image21.png "inserendo un punto di interruzione nel metodo Get del controller API")</span><span class="sxs-lookup"><span data-stu-id="9b368-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="9b368-309">*Inserendo un punto di interruzione nel metodo Get del controller API*</span><span class="sxs-lookup"><span data-stu-id="9b368-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="9b368-310">Premere **F5** per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="9b368-310">Press **F5** to run the project.</span></span> <span data-ttu-id="9b368-311">Il browser verrà caricato il documento HTML.</span><span class="sxs-lookup"><span data-stu-id="9b368-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9b368-312">Verificare che si sta esplorando l'URL radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="9b368-313">Dopo il caricamento della pagina e viene eseguito il codice JavaScript, il punto di interruzione verrà raggiunto e sospende l'esecuzione del codice nel controller.</span><span class="sxs-lookup"><span data-stu-id="9b368-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="9b368-314">![Debug di chiamate API Web usando Visual Studio Express per Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "debug chiamate API Web usando Visual Studio Express per Web")</span><span class="sxs-lookup"><span data-stu-id="9b368-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="9b368-315">*Debug di una chiamata API Web usando Visual Studio Express 2012 per Web*</span><span class="sxs-lookup"><span data-stu-id="9b368-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="9b368-316">Rimuovere il punto di interruzione e premere **F5** o sulla barra degli strumenti Debug **continua** pulsante per continuare il caricamento della visualizzazione nel browser.</span><span class="sxs-lookup"><span data-stu-id="9b368-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="9b368-317">Una volta completata la chiamata all'API Web dovrebbe essere i contatti restituiti dall'API Web chiamare visualizzati come elementi di elenco nel browser.</span><span class="sxs-lookup"><span data-stu-id="9b368-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="9b368-318">![I risultati della chiamata API visualizzata nel browser come elementi di elenco](build-restful-apis-with-aspnet-web-api/_static/image23.png "i risultati della chiamata API visualizzata nel browser come elementi di elenco")</span><span class="sxs-lookup"><span data-stu-id="9b368-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="9b368-319">*Risultati della chiamata API visualizzata nel browser come elementi di elenco*</span><span class="sxs-lookup"><span data-stu-id="9b368-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="9b368-320">Terminare il debug.</span><span class="sxs-lookup"><span data-stu-id="9b368-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="9b368-321">Attività 2: modifica della visualizzazione dell'indice per fornire un'interfaccia utente grafica per la creazione di contatti</span><span class="sxs-lookup"><span data-stu-id="9b368-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="9b368-322">In questa attività, si continuerà a modificare la visualizzazione dell'indice dell'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="9b368-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="9b368-323">Un modulo verrà aggiunto alla pagina HTML che l'input dell'utente di acquisire e inviarlo all'API Web per creare un nuovo contatto e verrà creato un nuovo metodo controller API Web per raccogliere data dalla GUI.</span><span class="sxs-lookup"><span data-stu-id="9b368-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="9b368-324">Aprire il **ContactController.cs** file.</span><span class="sxs-lookup"><span data-stu-id="9b368-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="9b368-325">Aggiungere un nuovo metodo alla classe controller denominata **Post** come illustrato nel codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9b368-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="9b368-326">(- Frammento di codice *API Lab - Ex03 - Post metodo Web*)</span><span class="sxs-lookup"><span data-stu-id="9b368-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
~~~
3. <span data-ttu-id="9b368-327">Aprire il **cshtml** file in Visual Studio, se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="9b368-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="9b368-328">Aggiungere il codice HTML seguente subito dopo l'elenco non ordinato che nell'attività precedente è stato aggiunto il file.</span><span class="sxs-lookup"><span data-stu-id="9b368-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
~~~
5. <span data-ttu-id="9b368-329">All'interno dell'elemento di script nella parte inferiore del documento, aggiungere il codice evidenziato di seguito per gestire gli eventi clic sul pulsante, che verranno registrati i dati per l'API Web mediante una chiamata HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="9b368-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="9b368-330">In **ContactController.cs**, inserire un punto di interruzione di **Post** metodo.</span><span class="sxs-lookup"><span data-stu-id="9b368-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="9b368-331">Premere **F5** per eseguire l'applicazione nel browser.</span><span class="sxs-lookup"><span data-stu-id="9b368-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="9b368-332">Una volta la pagina viene caricata nel browser, digitare un nuovo nome di contatto e l'Id e fare clic su di **salvare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="9b368-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="9b368-333">![Caricare il documento client HTML nel browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "caricare il documento client HTML nel browser")</span><span class="sxs-lookup"><span data-stu-id="9b368-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="9b368-334">*Il documento HTML client caricata nel browser*</span><span class="sxs-lookup"><span data-stu-id="9b368-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="9b368-335">Quando la finestra del debugger si interrompe **Post** metodo, esaminare le proprietà di prendere il **contattare** parametro.</span><span class="sxs-lookup"><span data-stu-id="9b368-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="9b368-336">I valori devono corrispondere i dati che immessi nel modulo.</span><span class="sxs-lookup"><span data-stu-id="9b368-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="9b368-337">![L'oggetto contatto viene inviato all'API Web dal client](build-restful-apis-with-aspnet-web-api/_static/image25.png "oggetto contatto di inviati dal client per l'API Web")</span><span class="sxs-lookup"><span data-stu-id="9b368-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="9b368-338">*L'oggetto contatto viene inviato all'API Web dal client*</span><span class="sxs-lookup"><span data-stu-id="9b368-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="9b368-339">Passaggio tramite il metodo nel debugger fino a quando il **risposta** variabile è stata creata.</span><span class="sxs-lookup"><span data-stu-id="9b368-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="9b368-340">Al momento di ispezione nel **variabili locali** finestra nel debugger, si noterà che tutte le proprietà sono state impostate.</span><span class="sxs-lookup"><span data-stu-id="9b368-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="9b368-341">![La risposta dopo la creazione nel debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "la risposta dopo la creazione nel debugger")</span><span class="sxs-lookup"><span data-stu-id="9b368-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="9b368-342">*La risposta dopo la creazione nel debugger*</span><span class="sxs-lookup"><span data-stu-id="9b368-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="9b368-343">Se si preme **F5** oppure fare clic su **continua** nel debugger verrà completata la richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b368-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="9b368-344">Quando si passa al browser, il nuovo contatto è stato aggiunto all'elenco di contatti archiviate dal **ContactRepository** implementazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="9b368-345">![Il browser riflette alla creazione della nuova istanza contatta](build-restful-apis-with-aspnet-web-api/_static/image27.png "browser riflette alla creazione della nuova istanza di contatto")</span><span class="sxs-lookup"><span data-stu-id="9b368-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="9b368-346">*Il browser riflette alla creazione della nuova istanza di contatto*</span><span class="sxs-lookup"><span data-stu-id="9b368-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="9b368-347">Inoltre, è possibile distribuire l'applicazione Azure seguente [appendice c: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="9b368-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="9b368-348">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="9b368-348">Summary</span></span>

<span data-ttu-id="9b368-349">Questa esercitazione ha introdotto il nuovo framework di ASP.NET Web API e l'implementazione di RESTful API Web usando il framework.</span><span class="sxs-lookup"><span data-stu-id="9b368-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="9b368-350">Da qui è possibile creare un nuovo repository che facilita la persistenza dei dati utilizzando un numero qualsiasi di meccanismi e associare tale servizio anziché quello semplice fornito in questa esercitazione, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="9b368-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="9b368-351">API Web supporta una serie di funzionalità aggiuntive, ad esempio l'abilitazione delle comunicazioni da client non HTML scritti in qualsiasi linguaggio che supporti HTTP e JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="9b368-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="9b368-352">La possibilità di ospitare un'API Web all'esterno di un'applicazione web tipica è anche possibile, nonché è la possibilità di creare formati di serializzazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="9b368-353">Il sito Web ASP.NET dispone di un'area dedicata per il framework di ASP.NET Web API nel [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="9b368-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="9b368-354">Questo sito continuerà a fornire le informazioni più recenti, esempi e le notizie relative all'API Web, quindi archiviarlo spesso se si desidera approfondire la conoscenza arte della creazione di API Web personalizzate disponibili per qualsiasi framework di sviluppo o dispositivo.</span><span class="sxs-lookup"><span data-stu-id="9b368-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="9b368-355">Appendice a: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="9b368-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="9b368-356">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="9b368-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="9b368-357">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="9b368-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="9b368-358">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](build-restful-apis-with-aspnet-web-api/_static/image28.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="9b368-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="9b368-359">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="9b368-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="9b368-360">Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)</span><span class="sxs-lookup"><span data-stu-id="9b368-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="9b368-361">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="9b368-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="9b368-362">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="9b368-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="9b368-363">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="9b368-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="9b368-364">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="9b368-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="9b368-365">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="9b368-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="9b368-366">![Iniziare a digitare il nome del frammento di codice](build-restful-apis-with-aspnet-web-api/_static/image29.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="9b368-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="9b368-367">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="9b368-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="9b368-368">![Premere Tab per selezionare il frammento di codice evidenziata](build-restful-apis-with-aspnet-web-api/_static/image30.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="9b368-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="9b368-369">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="9b368-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="9b368-370">![Premere Tab nuovamente e il frammento di codice verranno espansi](build-restful-apis-with-aspnet-web-api/_static/image31.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="9b368-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="9b368-371">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="9b368-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="9b368-372">Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)</span><span class="sxs-lookup"><span data-stu-id="9b368-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="9b368-373">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="9b368-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="9b368-374">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="9b368-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="9b368-375">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="9b368-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="9b368-376">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](build-restful-apis-with-aspnet-web-api/_static/image32.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="9b368-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="9b368-377">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="9b368-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="9b368-378">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](build-restful-apis-with-aspnet-web-api/_static/image33.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="9b368-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="9b368-379">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="9b368-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="9b368-380">Appendice b: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="9b368-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="9b368-381">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="9b368-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="9b368-382">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="9b368-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="9b368-383">Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="9b368-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9b368-384">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="9b368-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="9b368-385">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="9b368-385">Click on **Install Now**.</span></span> <span data-ttu-id="9b368-386">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="9b368-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9b368-387">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9b368-388">![Installa Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="9b368-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="9b368-389">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="9b368-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="9b368-390">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="9b368-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="9b368-392">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="9b368-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9b368-393">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-393">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="9b368-395">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="9b368-395">*Installation progress*</span></span>
6. <span data-ttu-id="9b368-396">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="9b368-396">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="9b368-398">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="9b368-398">*Installation completed*</span></span>
7. <span data-ttu-id="9b368-399">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="9b368-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="9b368-400">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="9b368-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="9b368-402">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="9b368-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9b368-403">Appendice c: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="9b368-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="9b368-404">Questa appendice mostra come creare un nuovo sito web dal portale di Azure e pubblicare l'applicazione che è stato acquistato seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Azure.</span><span class="sxs-lookup"><span data-stu-id="9b368-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="9b368-405">Attività 1 - Creazione di un nuovo sito Web dal portale di Azure</span><span class="sxs-lookup"><span data-stu-id="9b368-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="9b368-406">Passare al [il portale di gestione di Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="9b368-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9b368-407">Con Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico.</span><span class="sxs-lookup"><span data-stu-id="9b368-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="9b368-408">È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="9b368-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="9b368-409">![Accedere al portale Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "accedere al portale Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="9b368-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="9b368-410">*Accedere al portale*</span><span class="sxs-lookup"><span data-stu-id="9b368-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="9b368-411">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="9b368-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="9b368-412">![Creazione di un nuovo sito Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="9b368-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="9b368-413">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="9b368-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="9b368-414">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="9b368-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="9b368-415">Selezionare quindi **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="9b368-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="9b368-416">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="9b368-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9b368-417">Azure è l'host di un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="9b368-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="9b368-418">L'opzione Creazione rapida consente di distribuire un'applicazione web completa in Azure dall'esterno al portale.</span><span class="sxs-lookup"><span data-stu-id="9b368-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="9b368-419">Non include i passaggi per configurare un database.</span><span class="sxs-lookup"><span data-stu-id="9b368-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="9b368-420">![Creazione di un nuovo sito Web utilizzando Creazione rapida](build-restful-apis-with-aspnet-web-api/_static/image41.png "creando un nuovo sito Web utilizzando Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="9b368-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="9b368-421">*Creazione di un nuovo sito Web utilizzando Creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="9b368-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="9b368-422">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="9b368-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="9b368-423">Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="9b368-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="9b368-424">Verificare il funzionamento del nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="9b368-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="9b368-425">![Esplorazione per il nuovo sito web](build-restful-apis-with-aspnet-web-api/_static/image42.png "esplorazione per il nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="9b368-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="9b368-426">*Esplorazione per il nuovo sito web*</span><span class="sxs-lookup"><span data-stu-id="9b368-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="9b368-427">![Sito Web in esecuzione](build-restful-apis-with-aspnet-web-api/_static/image43.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="9b368-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="9b368-428">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="9b368-428">*Web site running*</span></span>
6. <span data-ttu-id="9b368-429">Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="9b368-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="9b368-430">![Aprire le pagine di gestione del sito web](build-restful-apis-with-aspnet-web-api/_static/image44.png "apertura delle pagine di gestione del sito web")</span><span class="sxs-lookup"><span data-stu-id="9b368-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="9b368-431">*Aprire le pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="9b368-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="9b368-432">Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="9b368-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9b368-433">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="9b368-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="9b368-434">Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="9b368-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in Azure.</span><span class="sxs-lookup"><span data-stu-id="9b368-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="9b368-436">![Profilo di pubblicazione del sito web di download](build-restful-apis-with-aspnet-web-api/_static/image45.png "profilo di pubblicazione del sito web di download")</span><span class="sxs-lookup"><span data-stu-id="9b368-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="9b368-437">*Profilo di pubblicazione del sito Web di download*</span><span class="sxs-lookup"><span data-stu-id="9b368-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="9b368-438">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="9b368-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="9b368-439">In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web in Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b368-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="9b368-440">![Salvare il file del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image46.png "il salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="9b368-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="9b368-441">*Salvare il file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="9b368-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="9b368-442">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="9b368-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="9b368-443">Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL.</span><span class="sxs-lookup"><span data-stu-id="9b368-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="9b368-444">Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="9b368-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="9b368-445">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9b368-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="9b368-446">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Azure in **database Sql** | **server** | **Dashboard del Server**.</span><span class="sxs-lookup"><span data-stu-id="9b368-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="9b368-447">Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="9b368-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="9b368-448">Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive.</span><span class="sxs-lookup"><span data-stu-id="9b368-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="9b368-449">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="9b368-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="9b368-450">![Dashboard del Server Database SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard del Server Database SQL")</span><span class="sxs-lookup"><span data-stu-id="9b368-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="9b368-451">*Dashboard del Server Database SQL*</span><span class="sxs-lookup"><span data-stu-id="9b368-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="9b368-452">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="9b368-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="9b368-453">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="9b368-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="9b368-455">*Aggiunta indirizzo IP del Client*</span><span class="sxs-lookup"><span data-stu-id="9b368-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="9b368-456">Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="9b368-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="9b368-458">*Confermare le modifiche*</span><span class="sxs-lookup"><span data-stu-id="9b368-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9b368-459">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="9b368-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="9b368-460">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="9b368-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="9b368-461">Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="9b368-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="9b368-462">![La pubblicazione dell'applicazione](build-restful-apis-with-aspnet-web-api/_static/image51.png "la pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="9b368-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="9b368-463">*Pubblicazione del sito web*</span><span class="sxs-lookup"><span data-stu-id="9b368-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="9b368-464">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="9b368-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="9b368-465">![L'importazione del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image52.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="9b368-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="9b368-466">*L'importazione di profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="9b368-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="9b368-467">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="9b368-467">Click **Validate Connection**.</span></span> <span data-ttu-id="9b368-468">Una volta completata la convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="9b368-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9b368-469">Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.</span><span class="sxs-lookup"><span data-stu-id="9b368-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="9b368-470">![Convalida della connessione](build-restful-apis-with-aspnet-web-api/_static/image53.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="9b368-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="9b368-471">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="9b368-471">*Validating connection*</span></span>
4. <span data-ttu-id="9b368-472">Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="9b368-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="9b368-473">![Configurazione della distribuzione Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="9b368-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="9b368-474">*Configurazione della distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="9b368-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="9b368-475">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="9b368-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="9b368-476">Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b368-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="9b368-477">In **nome utente** digitare il nome di accesso di amministratore di server.</span><span class="sxs-lookup"><span data-stu-id="9b368-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="9b368-478">In **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="9b368-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="9b368-479">Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="9b368-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="9b368-480">![Configurazione di stringa di connessione di destinazione](build-restful-apis-with-aspnet-web-api/_static/image55.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="9b368-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="9b368-481">*Configurazione di stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="9b368-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="9b368-482">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b368-482">Then click **OK**.</span></span> <span data-ttu-id="9b368-483">Quando viene richiesto di creare il database fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="9b368-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="9b368-484">![Creazione del database](build-restful-apis-with-aspnet-web-api/_static/image56.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="9b368-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="9b368-485">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="9b368-485">*Creating the database*</span></span>
7. <span data-ttu-id="9b368-486">La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito.</span><span class="sxs-lookup"><span data-stu-id="9b368-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="9b368-487">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="9b368-487">Then click **Next**.</span></span>

    <span data-ttu-id="9b368-488">![Stringa di connessione che punta al Database SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="9b368-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="9b368-489">*Stringa di connessione che punta al Database SQL*</span><span class="sxs-lookup"><span data-stu-id="9b368-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="9b368-490">Nel **anteprima** pagina, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="9b368-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="9b368-491">![Pubblicare l'applicazione web](build-restful-apis-with-aspnet-web-api/_static/image58.png "pubblicare l'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="9b368-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="9b368-492">*Pubblicare l'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="9b368-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="9b368-493">Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="9b368-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="9b368-494">![Applicazione pubblicata in Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "applicazione pubblicata in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="9b368-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="9b368-495">*Applicazione pubblicata in Azure*</span><span class="sxs-lookup"><span data-stu-id="9b368-495">*Application published to Azure*</span></span>
