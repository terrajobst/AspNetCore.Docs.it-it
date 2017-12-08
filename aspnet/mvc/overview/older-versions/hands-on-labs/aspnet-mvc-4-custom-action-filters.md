---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtri azione personalizzati di ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: ASP.NET MVC fornisce i filtri di azione per l'esecuzione di logica di filtro prima o dopo la chiamata di un metodo di azione. I filtri azione sono attributi personalizzati che...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 6362f0506934ca3b3cc86e1a927af75e7bc4e1d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="e9b16-104">Filtri azione personalizzati di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="e9b16-104">ASP.NET MVC 4 Custom Action Filters</span></span>
====================
<span data-ttu-id="e9b16-105">da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e9b16-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="e9b16-106">ASP.NET MVC fornisce i filtri di azione per l'esecuzione di logica di filtro prima o dopo la chiamata di un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-106">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="e9b16-107">Filtri azione sono attributi personalizzati che forniscono un modo per aggiungere un comportamento pre-azione e post-azione ai metodi di azione del controller.</span><span class="sxs-lookup"><span data-stu-id="e9b16-107">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>
> 
> <span data-ttu-id="e9b16-108">In questo laboratorio pratico si creerà un attributo di filtro azione personalizzato nella soluzione MvcMusicStore per intercettare le richieste del controller e registrare l'attività di un sito in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="e9b16-108">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="e9b16-109">Sarà in grado di aggiungere il filtro di registrazione per l'inserimento di un'azione o un controller.</span><span class="sxs-lookup"><span data-stu-id="e9b16-109">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="e9b16-110">Infine, si noterà che la visualizzazione del log che mostra l'elenco dei visitatori.</span><span class="sxs-lookup"><span data-stu-id="e9b16-110">Finally, you will see the log view that shows the list of visitors.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="e9b16-111">Questa pratica presuppone avere conoscenze di base **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-111">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="e9b16-112">Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **nozioni di base di ASP.NET MVC 4** le esercitazioni pratiche.</span><span class="sxs-lookup"><span data-stu-id="e9b16-112">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e9b16-113">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="e9b16-113">Objectives</span></span>

<span data-ttu-id="e9b16-114">In questa pratica, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="e9b16-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="e9b16-115">Creare un attributo di filtro azione personalizzata per estendere le funzionalità di filtro</span><span class="sxs-lookup"><span data-stu-id="e9b16-115">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="e9b16-116">Applicare un attributo di filtro personalizzato per l'inserimento di un livello specifico</span><span class="sxs-lookup"><span data-stu-id="e9b16-116">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="e9b16-117">Registrare un filtri azione personalizzati a livello globale</span><span class="sxs-lookup"><span data-stu-id="e9b16-117">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e9b16-118">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e9b16-118">Prerequisites</span></span>

<span data-ttu-id="e9b16-119">È necessario che gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="e9b16-119">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e9b16-120">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="e9b16-120">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e9b16-121">Configurazione</span><span class="sxs-lookup"><span data-stu-id="e9b16-121">Setup</span></span>

<span data-ttu-id="e9b16-122">**L'installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="e9b16-122">**Installing Code Snippets**</span></span>

<span data-ttu-id="e9b16-123">Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e9b16-123">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e9b16-124">Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="e9b16-124">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e9b16-125">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: con i frammenti di codice](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e9b16-125">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e9b16-126">Esercizi</span><span class="sxs-lookup"><span data-stu-id="e9b16-126">Exercises</span></span>

<span data-ttu-id="e9b16-127">Questa pratica include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e9b16-127">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="e9b16-128">Esercizio 1: La registrazione delle azioni</span><span class="sxs-lookup"><span data-stu-id="e9b16-128">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="e9b16-129">Esercizio 2: Gestione di più filtri azione</span><span class="sxs-lookup"><span data-stu-id="e9b16-129">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="e9b16-130">Tempo stimato per completare questa esercitazione: **30 minuti**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-130">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="e9b16-131">Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio.</span><span class="sxs-lookup"><span data-stu-id="e9b16-131">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="e9b16-132">Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="e9b16-132">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="e9b16-133">Esercizio 1: La registrazione delle azioni</span><span class="sxs-lookup"><span data-stu-id="e9b16-133">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="e9b16-134">In questo esercizio, si apprenderà come creare un filtro azione personalizzata del log tramite i provider di filtri ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e9b16-134">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="e9b16-135">A tale scopo si applicherà un filtro di registrazione al sito MusicStore che registrerà tutte le attività nel controller selezionato.</span><span class="sxs-lookup"><span data-stu-id="e9b16-135">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="e9b16-136">Il filtro verrà esteso **ActionFilterAttributeClass** ed eseguire l'override **OnActionExecuting** per intercettare ogni richiesta e quindi eseguire le azioni di registrazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-136">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="e9b16-137">Informazioni di contesto sulle richieste HTTP, l'esecuzione di metodi, i risultati e i parametri vengono fornita da ASP.NET MVC **ActionExecutingContext** classe **.**</span><span class="sxs-lookup"><span data-stu-id="e9b16-137">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="e9b16-138">ASP.NET MVC 4 ha anche il provider di filtri predefiniti è possibile utilizzare senza creare un filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="e9b16-138">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="e9b16-139">ASP.NET MVC 4 fornisce i seguenti tipi di filtri:</span><span class="sxs-lookup"><span data-stu-id="e9b16-139">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="e9b16-140">**Autorizzazione** filtrare, che prende decisioni di protezione sulla possibilità di eseguire un metodo di azione, ad esempio l'autenticazione o la convalida delle proprietà della richiesta.</span><span class="sxs-lookup"><span data-stu-id="e9b16-140">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="e9b16-141">**Azione** filtro che esegue il wrapping l'esecuzione del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-141">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="e9b16-142">Questo filtro è possibile eseguire ulteriori elaborazioni, ad esempio fornire dati aggiuntivi al metodo di azione, controllare il valore restituito o annullare l'esecuzione del metodo di azione</span><span class="sxs-lookup"><span data-stu-id="e9b16-142">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="e9b16-143">**Risultato** filtro che esegue il wrapping di esecuzione dell'oggetto ActionResult.</span><span class="sxs-lookup"><span data-stu-id="e9b16-143">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="e9b16-144">Questo filtro è possibile eseguire un'ulteriore elaborazione del risultato, ad esempio modificare la risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e9b16-144">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="e9b16-145">**Eccezione** filtro, viene eseguita se si verifica un'eccezione non gestita generata in un punto nel metodo di azione, iniziando con i filtri di autorizzazione e terminando con l'esecuzione del risultato.</span><span class="sxs-lookup"><span data-stu-id="e9b16-145">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="e9b16-146">I filtri eccezioni è utilizzabile per attività quali la registrazione o la visualizzazione di una pagina di errore.</span><span class="sxs-lookup"><span data-stu-id="e9b16-146">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="e9b16-147">Per ulteriori informazioni sui provider di filtri, visitare il sito Web MSDN: ([https://msdn.microsoft.com/en-us/library/dd410209.aspx](https://msdn.microsoft.com/en-us/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="e9b16-147">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/en-us/library/dd410209.aspx](https://msdn.microsoft.com/en-us/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="e9b16-148">Informazioni sulla funzionalità di registrazione MVC musica archivio applicazioni</span><span class="sxs-lookup"><span data-stu-id="e9b16-148">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="e9b16-149">Questa soluzione di negozio dispone di una nuova tabella di modello di dati per la registrazione del sito, **ActionLog**, con i seguenti campi: nome del controller che ha ricevuto una richiesta, l'azione chiamate, indirizzo IP del Client e timestamp.</span><span class="sxs-lookup"><span data-stu-id="e9b16-149">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="e9b16-150">![Modello di dati. Tabella ActionLog. ] (aspnet-mvc-4-custom-action-filters/_static/image1.png "Modello di dati. Tabella ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="e9b16-150">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="e9b16-151">*Modello di dati - tabella ActionLog*</span><span class="sxs-lookup"><span data-stu-id="e9b16-151">*Data model - ActionLog table*</span></span>

<span data-ttu-id="e9b16-152">La soluzione offre una visualizzazione MVC ASP.NET per il log azioni che è reperibile in **MvcMusicStores/visualizzazioni/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="e9b16-152">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="e9b16-153">![Visualizzazione del Log azioni](aspnet-mvc-4-custom-action-filters/_static/image2.png "Visualizza registro azioni")</span><span class="sxs-lookup"><span data-stu-id="e9b16-153">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="e9b16-154">*Visualizzazione del Log azioni*</span><span class="sxs-lookup"><span data-stu-id="e9b16-154">*Action Log view*</span></span>

<span data-ttu-id="e9b16-155">Con questa struttura, tutto il lavoro verterà sulla interrompere una richiesta del controller e l'esecuzione della registrazione tramite il filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="e9b16-155">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="e9b16-156">Attività 1 - Creazione di un filtro personalizzato per rilevare la richiesta di un Controller</span><span class="sxs-lookup"><span data-stu-id="e9b16-156">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="e9b16-157">In questa attività si creerà una classe di attributi di filtro personalizzato che contiene la logica di registrazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-157">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="e9b16-158">A tale scopo si estenderà ASP.NET MVC **ActionFilterAttribute** classe e implementare l'interfaccia **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-158">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="e9b16-159">Il **ActionFilterAttribute** è la classe base per tutti i filtri di attributo.</span><span class="sxs-lookup"><span data-stu-id="e9b16-159">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="e9b16-160">Fornisce i metodi seguenti per eseguire una logica specifica dopo e prima dell'esecuzione dell'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="e9b16-160">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="e9b16-161">**OnActionExecuting**(filterContext ActionExecutingContext): solo prima dell'azione metodo viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="e9b16-161">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="e9b16-162">**OnActionExecuted**(filterContext ActionExecutedContext): viene chiamato il metodo di azione e prima che il risultato viene eseguito (prima di eseguire il rendering di visualizzazione).</span><span class="sxs-lookup"><span data-stu-id="e9b16-162">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="e9b16-163">**OnResultExecuting**(filterContext ResultExecutingContext): solo prima il risultato dell'esecuzione (prima di eseguire il rendering di visualizzazione).</span><span class="sxs-lookup"><span data-stu-id="e9b16-163">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="e9b16-164">**OnResultExecuted**(filterContext ResultExecutedContext): dopo l'esecuzione del risultato (dopo il rendering).</span><span class="sxs-lookup"><span data-stu-id="e9b16-164">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="e9b16-165">Eseguendo l'override di uno di questi metodi in una classe derivata, è possibile eseguire il proprio codice di filtro.</span><span class="sxs-lookup"><span data-stu-id="e9b16-165">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="e9b16-166">Aprire il **iniziare** soluzione disponibile all'indirizzo **\Source\Ex01-LoggingActions\Begin** cartella.</span><span class="sxs-lookup"><span data-stu-id="e9b16-166">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

    1. <span data-ttu-id="e9b16-167">È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="e9b16-167">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="e9b16-168">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-168">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="e9b16-169">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="e9b16-169">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="e9b16-170">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-170">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e9b16-171">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="e9b16-171">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e9b16-172">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="e9b16-172">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e9b16-173">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-173">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="e9b16-174">Per ulteriori informazioni, vedere l'articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e9b16-174">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e9b16-175">Aggiungere una nuova classe c# nel **filtri** cartella e denominarla *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="e9b16-175">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="e9b16-176">Questa cartella archivia tutti i filtri personalizzati.</span><span class="sxs-lookup"><span data-stu-id="e9b16-176">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="e9b16-177">Aprire **CustomActionFilter.cs** e aggiungere un riferimento a **System** e **MvcMusicStore.Models** gli spazi dei nomi:</span><span class="sxs-lookup"><span data-stu-id="e9b16-177">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="e9b16-178">(- Frammento di codice *filtri azione personalizzati di ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="e9b16-178">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="e9b16-179">Ereditare il **CustomActionFilter** classe **ActionFilterAttribute** e quindi rendere **CustomActionFilter** classe implementare **IActionFilter** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="e9b16-179">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="e9b16-180">Rendere **CustomActionFilter** classe eseguire l'override del metodo **OnActionExecuting** e aggiungere la logica necessaria per registrare l'esecuzione del filtro.</span><span class="sxs-lookup"><span data-stu-id="e9b16-180">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="e9b16-181">A tale scopo, aggiungere il seguente codice evidenziato all'interno di **CustomActionFilter** classe.</span><span class="sxs-lookup"><span data-stu-id="e9b16-181">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="e9b16-182">(- Frammento di codice *filtri azione personalizzati di ASP.NET MVC 4 - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="e9b16-182">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="e9b16-183">**OnActionExecuting** Usa metodo **Entity Framework** per aggiungere un nuovo registro ActionLog.</span><span class="sxs-lookup"><span data-stu-id="e9b16-183">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="e9b16-184">Crea e inserisce una nuova istanza di entità con le informazioni di contesto da **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-184">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="e9b16-185">Ulteriori informazioni su **ControllerContext** classe in [msdn](https://msdn.microsoft.com/en-us/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="e9b16-185">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/en-us/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="e9b16-186">Attività 2: inserimento di un intercettore di codice in una classe Controller dell'archivio</span><span class="sxs-lookup"><span data-stu-id="e9b16-186">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="e9b16-187">In questa attività si aggiungerà il filtro personalizzato per inserirlo a tutte le classi controller e azioni del controller che verranno registrate.</span><span class="sxs-lookup"><span data-stu-id="e9b16-187">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="e9b16-188">Allo scopo di questo esercizio, la classe di Controller di archiviazione avrà un log.</span><span class="sxs-lookup"><span data-stu-id="e9b16-188">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="e9b16-189">Il metodo **OnActionExecuting** da **ActionLogFilterAttribute** filtro personalizzato viene eseguito quando viene chiamato un elemento inserito.</span><span class="sxs-lookup"><span data-stu-id="e9b16-189">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="e9b16-190">È anche possibile intercettare un metodo del controller specifico.</span><span class="sxs-lookup"><span data-stu-id="e9b16-190">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="e9b16-191">Aprire il **StoreController** in **MvcMusicStore\Controllers** e aggiungere un riferimento al **filtri** dello spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="e9b16-191">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="e9b16-192">Inserire il filtro personalizzato **CustomActionFilter** in **StoreController** classe aggiungendo **[CustomActionFilter]** attributo prima della dichiarazione di classe.</span><span class="sxs-lookup"><span data-stu-id="e9b16-192">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > <span data-ttu-id="e9b16-193">Quando un filtro viene inserito in una classe controller, vengono inseriti anche tutte le relative azioni.</span><span class="sxs-lookup"><span data-stu-id="e9b16-193">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="e9b16-194">Se si desidera applicare il filtro solo per un set di azioni, è necessario inserire **[CustomActionFilter]** a ciascuno di essi:</span><span class="sxs-lookup"><span data-stu-id="e9b16-194">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="e9b16-195">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e9b16-195">Task 3 - Running the Application</span></span>

<span data-ttu-id="e9b16-196">In questa attività, si verificherà che il filtro di registrazione funziona.</span><span class="sxs-lookup"><span data-stu-id="e9b16-196">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="e9b16-197">Si avvia l'applicazione e visitare lo store e quindi si controllerà le attività registrate.</span><span class="sxs-lookup"><span data-stu-id="e9b16-197">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="e9b16-198">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-198">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e9b16-199">Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione log:</span><span class="sxs-lookup"><span data-stu-id="e9b16-199">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="e9b16-200">![Registra lo stato di arresto prima attività pagina](aspnet-mvc-4-custom-action-filters/_static/image3.png "registrare lo stato di arresto prima dell'attività di pagina")</span><span class="sxs-lookup"><span data-stu-id="e9b16-200">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="e9b16-201">*Stato di arresto di log prima dell'attività di pagina*</span><span class="sxs-lookup"><span data-stu-id="e9b16-201">*Log tracker status before page activity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e9b16-202">Per impostazione predefinita, verrà sempre indicato un unico elemento che viene generato durante il recupero di generi esistente per il menu.</span><span class="sxs-lookup"><span data-stu-id="e9b16-202">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
    > 
    > <span data-ttu-id="e9b16-203">Per motivi di semplicità ci stiamo pulizia di **ActionLog** tabella ogni volta che l'applicazione viene eseguita in modo visualizzerà solo i registri di verifica dell'attività ogni particolare.</span><span class="sxs-lookup"><span data-stu-id="e9b16-203">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
    > 
    > <span data-ttu-id="e9b16-204">Potrebbe essere necessario rimuovere il codice seguente dal **sessione\_avviare** metodo (nel **Global. asax** classe), per salvare un log cronologico per tutte le azioni eseguite all'interno dell'archivio Controller.</span><span class="sxs-lookup"><span data-stu-id="e9b16-204">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="e9b16-205">Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.</span><span class="sxs-lookup"><span data-stu-id="e9b16-205">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="e9b16-206">Passare a **/ActionLog** e se il log è vuoto premere **F5** per aggiornare la pagina.</span><span class="sxs-lookup"><span data-stu-id="e9b16-206">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="e9b16-207">Verificare che sono state rilevate le visite:</span><span class="sxs-lookup"><span data-stu-id="e9b16-207">Check that your visits were tracked:</span></span>

    <span data-ttu-id="e9b16-208">![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image4.png "log azioni con attività di registrazione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-208">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e9b16-209">*Log azioni con attività di registrazione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-209">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="e9b16-210">Esercizio 2: Gestione di più filtri azione</span><span class="sxs-lookup"><span data-stu-id="e9b16-210">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="e9b16-211">In questo esercizio si verrà aggiunto un secondo filtro azioni personalizzato alla classe StoreController e definire l'ordine specifico in cui verranno eseguiti entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="e9b16-211">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="e9b16-212">Quindi si aggiornerà il codice per registrare il filtro a livello globale.</span><span class="sxs-lookup"><span data-stu-id="e9b16-212">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="e9b16-213">Sono disponibili diverse opzioni per prendere in considerazione quando si definisce l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="e9b16-213">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="e9b16-214">Ad esempio, la proprietà di ordine e ambito dei filtri:</span><span class="sxs-lookup"><span data-stu-id="e9b16-214">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="e9b16-215">È possibile definire un **ambito** per ognuno dei filtri, ad esempio, è possibile definire l'ambito tutti i filtri dell'azione da eseguire all'interno di **Controller ambito**e tutti i filtri di autorizzazione per l'esecuzione in **ambito globale** .</span><span class="sxs-lookup"><span data-stu-id="e9b16-215">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="e9b16-216">Gli ambiti di dispongono di un ordine di esecuzione definito.</span><span class="sxs-lookup"><span data-stu-id="e9b16-216">The scopes have a defined execution order.</span></span>

<span data-ttu-id="e9b16-217">Inoltre, ogni filtro azioni presenta una proprietà di ordine che viene utilizzata per determinare l'ordine di esecuzione nell'ambito del filtro.</span><span class="sxs-lookup"><span data-stu-id="e9b16-217">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="e9b16-218">Per ulteriori informazioni sull'ordine di esecuzione filtri azione personalizzati, visitare questo articolo MSDN: ([https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="e9b16-218">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="e9b16-219">Attività 1: Creazione di un nuovo filtro azione personalizzato</span><span class="sxs-lookup"><span data-stu-id="e9b16-219">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="e9b16-220">In questa attività si creerà un nuovo filtro azione personalizzato per inserire la classe StoreController, imparare a gestire l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="e9b16-220">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="e9b16-221">Aprire il **iniziare** soluzione disponibile all'indirizzo **\Source\Ex02-ManagingMultipleActionFilters\Begin** cartella.</span><span class="sxs-lookup"><span data-stu-id="e9b16-221">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="e9b16-222">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="e9b16-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="e9b16-223">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="e9b16-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e9b16-224">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="e9b16-225">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="e9b16-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="e9b16-226">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="e9b16-227">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="e9b16-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e9b16-228">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="e9b16-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e9b16-229">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="e9b16-230">Per ulteriori informazioni, vedere l'articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e9b16-230">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e9b16-231">Aggiungere una nuova classe c# nel **filtri** cartella e denominarla *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="e9b16-231">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="e9b16-232">Aprire **MyNewCustomActionFilter.cs** e aggiungere un riferimento a **System** e **MvcMusicStore.Models** dello spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="e9b16-232">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="e9b16-233">(- Frammento di codice *filtri azione personalizzati di ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="e9b16-233">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="e9b16-234">Sostituire la dichiarazione di classe predefinita con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="e9b16-234">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="e9b16-235">(- Frammento di codice *filtri azione personalizzati di ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="e9b16-235">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="e9b16-236">Questo filtro azioni personalizzato è pressoché lo stesso di quello creato nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="e9b16-236">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="e9b16-237">La differenza principale è che è il  *&quot;registrati da&quot;*  attributo aggiornato con il nome di questa nuova classe per identificare il filtro di cui è registrato nel registro.</span><span class="sxs-lookup"><span data-stu-id="e9b16-237">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="e9b16-238">Attività 2: Inserire un nuovo intercettore di codice nella classe StoreController</span><span class="sxs-lookup"><span data-stu-id="e9b16-238">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="e9b16-239">In questa attività, si verrà aggiungere un nuovo filtro personalizzato nella classe StoreController ed eseguire la soluzione per verificare come entrambi i filtri usati insieme.</span><span class="sxs-lookup"><span data-stu-id="e9b16-239">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="e9b16-240">Aprire il **StoreController** classe si trova in **MvcMusicStore\Controllers** e inserire il nuovo filtro personalizzato **MyNewCustomActionFilter** in  **StoreController** classe come è illustrato nel codice seguente.</span><span class="sxs-lookup"><span data-stu-id="e9b16-240">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="e9b16-241">A questo punto, eseguire l'applicazione per vedere il funzionamento di questi due filtri azione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="e9b16-241">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="e9b16-242">A tale scopo, premere **F5** e attendere l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-242">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="e9b16-243">Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione di log.</span><span class="sxs-lookup"><span data-stu-id="e9b16-243">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="e9b16-244">![Registra lo stato di arresto prima attività pagina](aspnet-mvc-4-custom-action-filters/_static/image5.png "registrare lo stato di arresto prima dell'attività di pagina")</span><span class="sxs-lookup"><span data-stu-id="e9b16-244">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="e9b16-245">*Stato di arresto di log prima dell'attività di pagina*</span><span class="sxs-lookup"><span data-stu-id="e9b16-245">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="e9b16-246">Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.</span><span class="sxs-lookup"><span data-stu-id="e9b16-246">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="e9b16-247">Verificare che questa fase. le visite è sono rilevate due volte: una volta per ognuno dei filtri azione personalizzati aggiunti nel **che controller di archiviazione** classe.</span><span class="sxs-lookup"><span data-stu-id="e9b16-247">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="e9b16-248">![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image6.png "log azioni con attività di registrazione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-248">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e9b16-249">*Log azioni con attività di registrazione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-249">*Action log with activity logged*</span></span>
6. <span data-ttu-id="e9b16-250">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="e9b16-250">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="e9b16-251">Attività 3: Gestione di ordinamento di filtro</span><span class="sxs-lookup"><span data-stu-id="e9b16-251">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="e9b16-252">In questa attività si apprenderà come gestire l'ordine di esecuzione dei filtri utilizzando la proprietà di ordine.</span><span class="sxs-lookup"><span data-stu-id="e9b16-252">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="e9b16-253">Aprire il **StoreController** classe si trova in **MvcMusicStore\Controllers** e specificare il **ordine** in entrambi i filtri proprietà come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e9b16-253">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="e9b16-254">A questo punto, verificare come vengono eseguiti i filtri in base al valore della proprietà relativo ordine.</span><span class="sxs-lookup"><span data-stu-id="e9b16-254">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="e9b16-255">Si noterà che il filtro con il valore più piccolo dell'ordine (**CustomActionFilter**) è il primo che viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="e9b16-255">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="e9b16-256">Premere **F5** e attendere l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-256">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="e9b16-257">Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione di log.</span><span class="sxs-lookup"><span data-stu-id="e9b16-257">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="e9b16-258">![Registra lo stato di arresto prima attività pagina](aspnet-mvc-4-custom-action-filters/_static/image7.png "registrare lo stato di arresto prima dell'attività di pagina")</span><span class="sxs-lookup"><span data-stu-id="e9b16-258">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="e9b16-259">*Stato di arresto di log prima dell'attività di pagina*</span><span class="sxs-lookup"><span data-stu-id="e9b16-259">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="e9b16-260">Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.</span><span class="sxs-lookup"><span data-stu-id="e9b16-260">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="e9b16-261">Verificare che questa volta, sono state rilevate le visite ordinati in base al valore dell'ordine dei filtri: **CustomActionFilter** registri prima.</span><span class="sxs-lookup"><span data-stu-id="e9b16-261">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="e9b16-262">![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image8.png "log azioni con attività di registrazione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-262">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e9b16-263">*Log azioni con attività di registrazione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-263">*Action log with activity logged*</span></span>
6. <span data-ttu-id="e9b16-264">A questo punto, si aggiorna il valore di ordine dei filtri e verificare come viene modificato l'ordine di registrazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-264">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="e9b16-265">Nel **StoreController** classe, aggiornare il valore dell'ordine dei filtri come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e9b16-265">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="e9b16-266">Eseguire nuovamente l'applicazione premendo **F5**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-266">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="e9b16-267">Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.</span><span class="sxs-lookup"><span data-stu-id="e9b16-267">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="e9b16-268">Verificare che questo periodo, i log creati **MyNewCustomActionFilter** filtro viene visualizzata per primo.</span><span class="sxs-lookup"><span data-stu-id="e9b16-268">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="e9b16-269">![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image9.png "log azioni con attività di registrazione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-269">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e9b16-270">*Log azioni con attività di registrazione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-270">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="e9b16-271">Attività 4: Registrazione di filtri a livello globale</span><span class="sxs-lookup"><span data-stu-id="e9b16-271">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="e9b16-272">In questa attività si aggiornerà la soluzione per registrare il nuovo filtro (**MyNewCustomActionFilter**) come filtro globale.</span><span class="sxs-lookup"><span data-stu-id="e9b16-272">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="e9b16-273">In questo modo verrà attivato per tutti i temporanei azioni nell'applicazione e non solo di quelli StoreController come l'attività precedente.</span><span class="sxs-lookup"><span data-stu-id="e9b16-273">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="e9b16-274">In **StoreController** classe, rimuovere **[MyNewCustomActionFilter]** attributo e la proprietà order **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-274">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="e9b16-275">Dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e9b16-275">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="e9b16-276">Aprire **Global. asax** file e individuare il **applicazione\_avviare** metodo.</span><span class="sxs-lookup"><span data-stu-id="e9b16-276">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="e9b16-277">Si noti che ogni thime l'avvio dell'applicazione sta registrando i filtri globali chiamando **RegisterGlobalFilters** metodo all'interno di **FilterConfig** classe.</span><span class="sxs-lookup"><span data-stu-id="e9b16-277">Notice that each thime the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="e9b16-278">![Registrazione di filtri globali in Global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "la registrazione di filtri globali in Global. asax")</span><span class="sxs-lookup"><span data-stu-id="e9b16-278">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="e9b16-279">*Registrazione di filtri globali in Global. asax*</span><span class="sxs-lookup"><span data-stu-id="e9b16-279">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="e9b16-280">Aprire **FilterConfig.cs** file all'interno di **App\_avviare** cartella.</span><span class="sxs-lookup"><span data-stu-id="e9b16-280">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="e9b16-281">Aggiungere un riferimento mediante System; utilizzando MvcMusicStore.Filters; spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="e9b16-281">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="e9b16-282">Aggiornamento **RegisterGlobalFilters** metodo aggiunta del filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="e9b16-282">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="e9b16-283">A tale scopo, aggiungere il codice evidenziato:</span><span class="sxs-lookup"><span data-stu-id="e9b16-283">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="e9b16-284">Eseguire l'applicazione premendo **F5**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-284">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="e9b16-285">Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.</span><span class="sxs-lookup"><span data-stu-id="e9b16-285">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="e9b16-286">Verificare che ora **[MyNewCustomActionFilter]** viene viene inserito in HomeController e ActionLogController troppo.</span><span class="sxs-lookup"><span data-stu-id="e9b16-286">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="e9b16-287">![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image11.png "log azioni con attività di registrazione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-287">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e9b16-288">*Log azioni con attività globale registrato*</span><span class="sxs-lookup"><span data-stu-id="e9b16-288">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="e9b16-289">Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="e9b16-289">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e9b16-290">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e9b16-290">Summary</span></span>

<span data-ttu-id="e9b16-291">Completando questa pratica si è appreso come estendere un filtro di azione per eseguire azioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="e9b16-291">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="e9b16-292">Inoltre, si è appreso come inserire qualsiasi filtro ai controller di pagina.</span><span class="sxs-lookup"><span data-stu-id="e9b16-292">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="e9b16-293">Sono stati utilizzati i seguenti concetti:</span><span class="sxs-lookup"><span data-stu-id="e9b16-293">The following concepts were used:</span></span>

- <span data-ttu-id="e9b16-294">Come creare filtri azione personalizzati con la classe ActionFilterAttribute MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e9b16-294">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="e9b16-295">Come inserire i filtri in ASP.NET MVC controller</span><span class="sxs-lookup"><span data-stu-id="e9b16-295">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="e9b16-296">Come gestire l'ordinamento di filtro utilizzando la proprietà Order</span><span class="sxs-lookup"><span data-stu-id="e9b16-296">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="e9b16-297">Come registrare i filtri a livello globale</span><span class="sxs-lookup"><span data-stu-id="e9b16-297">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e9b16-298">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="e9b16-298">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e9b16-299">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="e9b16-299">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e9b16-300">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="e9b16-300">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e9b16-301">Passare a [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e9b16-301">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e9b16-302">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="e9b16-302">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="e9b16-303">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-303">Click on **Install Now**.</span></span> <span data-ttu-id="e9b16-304">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="e9b16-304">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e9b16-305">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-305">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e9b16-306">![Installa Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e9b16-306">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e9b16-307">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e9b16-307">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e9b16-308">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="e9b16-308">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="e9b16-310">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="e9b16-310">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e9b16-311">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-311">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="e9b16-313">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-313">*Installation progress*</span></span>
6. <span data-ttu-id="e9b16-314">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-314">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="e9b16-316">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="e9b16-316">*Installation completed*</span></span>
7. <span data-ttu-id="e9b16-317">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="e9b16-317">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e9b16-318">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="e9b16-318">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="e9b16-320">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="e9b16-320">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e9b16-321">Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="e9b16-321">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="e9b16-322">Questa appendice mostra come creare un nuovo sito web dal portale di gestione di Windows Azure e pubblicare l'applicazione, ottenute seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e9b16-322">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="e9b16-323">Attività 1 - Creazione di un nuovo sito Web da Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e9b16-323">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="e9b16-324">Passare al [il portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-324">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e9b16-325">Con Windows Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico.</span><span class="sxs-lookup"><span data-stu-id="e9b16-325">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="e9b16-326">È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="e9b16-326">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="e9b16-327">![Accedere al portale Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "accedere al portale Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="e9b16-327">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="e9b16-328">*Accedere al portale di gestione di Azure*</span><span class="sxs-lookup"><span data-stu-id="e9b16-328">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="e9b16-329">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="e9b16-329">Click **New** on the command bar.</span></span>

    <span data-ttu-id="e9b16-330">![Creazione di un nuovo sito Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="e9b16-330">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="e9b16-331">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="e9b16-331">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="e9b16-332">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-332">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="e9b16-333">Selezionare quindi **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-333">Then select **Quick Create** option.</span></span> <span data-ttu-id="e9b16-334">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-334">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e9b16-335">Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="e9b16-335">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="e9b16-336">L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale.</span><span class="sxs-lookup"><span data-stu-id="e9b16-336">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="e9b16-337">Non include i passaggi per configurare un database.</span><span class="sxs-lookup"><span data-stu-id="e9b16-337">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="e9b16-338">![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-custom-action-filters/_static/image19.png "creando un nuovo sito Web utilizzando Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="e9b16-338">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="e9b16-339">*Creazione di un nuovo sito Web utilizzando Creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="e9b16-339">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="e9b16-340">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="e9b16-340">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="e9b16-341">Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="e9b16-341">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="e9b16-342">Verificare il funzionamento del nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="e9b16-342">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="e9b16-343">![Esplorazione per il nuovo sito web](aspnet-mvc-4-custom-action-filters/_static/image20.png "esplorazione per il nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="e9b16-343">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="e9b16-344">*Esplorazione per il nuovo sito web*</span><span class="sxs-lookup"><span data-stu-id="e9b16-344">*Browsing to the new web site*</span></span>

    <span data-ttu-id="e9b16-345">![Sito Web in esecuzione](aspnet-mvc-4-custom-action-filters/_static/image21.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-345">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="e9b16-346">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-346">*Web site running*</span></span>
6. <span data-ttu-id="e9b16-347">Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-347">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="e9b16-348">![Aprire le pagine di gestione del sito web](aspnet-mvc-4-custom-action-filters/_static/image22.png "apertura delle pagine di gestione del sito web")</span><span class="sxs-lookup"><span data-stu-id="e9b16-348">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="e9b16-349">*Aprire le pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="e9b16-349">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="e9b16-350">Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="e9b16-350">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e9b16-351">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="e9b16-351">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="e9b16-352">Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-352">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="e9b16-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="e9b16-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="e9b16-354">![Profilo di pubblicazione del sito web di download](aspnet-mvc-4-custom-action-filters/_static/image23.png "profilo di pubblicazione del sito web di download")</span><span class="sxs-lookup"><span data-stu-id="e9b16-354">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="e9b16-355">*Profilo di pubblicazione del sito Web di download*</span><span class="sxs-lookup"><span data-stu-id="e9b16-355">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="e9b16-356">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="e9b16-356">Download the publish profile file to a known location.</span></span> <span data-ttu-id="e9b16-357">In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e9b16-357">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="e9b16-358">![Salvare il file del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image24.png "il salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-358">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="e9b16-359">*Salvare il file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-359">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="e9b16-360">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="e9b16-360">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="e9b16-361">Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL.</span><span class="sxs-lookup"><span data-stu-id="e9b16-361">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="e9b16-362">Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="e9b16-362">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="e9b16-363">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e9b16-363">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="e9b16-364">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-364">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="e9b16-365">Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="e9b16-365">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="e9b16-366">Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive.</span><span class="sxs-lookup"><span data-stu-id="e9b16-366">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="e9b16-367">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="e9b16-367">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="e9b16-368">![Dashboard del Server Database SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "Dashboard del Server Database SQL")</span><span class="sxs-lookup"><span data-stu-id="e9b16-368">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="e9b16-369">*Dashboard del Server Database SQL*</span><span class="sxs-lookup"><span data-stu-id="e9b16-369">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="e9b16-370">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-370">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="e9b16-371">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="e9b16-371">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="e9b16-373">*Aggiunta indirizzo IP del Client*</span><span class="sxs-lookup"><span data-stu-id="e9b16-373">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="e9b16-374">Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e9b16-374">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="e9b16-376">*Confermare le modifiche*</span><span class="sxs-lookup"><span data-stu-id="e9b16-376">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e9b16-377">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="e9b16-377">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="e9b16-378">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e9b16-378">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="e9b16-379">Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-379">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="e9b16-380">![La pubblicazione dell'applicazione](aspnet-mvc-4-custom-action-filters/_static/image29.png "la pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-380">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="e9b16-381">*Pubblicazione del sito web*</span><span class="sxs-lookup"><span data-stu-id="e9b16-381">*Publishing the web site*</span></span>
2. <span data-ttu-id="e9b16-382">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="e9b16-382">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="e9b16-383">![L'importazione del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image30.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-383">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="e9b16-384">*L'importazione di profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-384">*Importing publish profile*</span></span>
3. <span data-ttu-id="e9b16-385">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-385">Click **Validate Connection**.</span></span> <span data-ttu-id="e9b16-386">Una volta completata la convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-386">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e9b16-387">Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.</span><span class="sxs-lookup"><span data-stu-id="e9b16-387">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="e9b16-388">![Convalida della connessione](aspnet-mvc-4-custom-action-filters/_static/image31.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-388">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="e9b16-389">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-389">*Validating connection*</span></span>
4. <span data-ttu-id="e9b16-390">Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="e9b16-390">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="e9b16-391">![Configurazione della distribuzione Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="e9b16-391">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="e9b16-392">*Configurazione della distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="e9b16-392">*Web deploy configuration*</span></span>
5. <span data-ttu-id="e9b16-393">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="e9b16-393">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="e9b16-394">Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="e9b16-394">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="e9b16-395">In **nome utente** digitare il nome di accesso di amministratore di server.</span><span class="sxs-lookup"><span data-stu-id="e9b16-395">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="e9b16-396">In **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="e9b16-396">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="e9b16-397">Digitare un nuovo nome di database.</span><span class="sxs-lookup"><span data-stu-id="e9b16-397">Type a new database name.</span></span>

    <span data-ttu-id="e9b16-398">![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-custom-action-filters/_static/image33.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="e9b16-398">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="e9b16-399">*Configurazione di stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="e9b16-399">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="e9b16-400">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-400">Then click **OK**.</span></span> <span data-ttu-id="e9b16-401">Quando viene richiesto di creare il database fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-401">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="e9b16-402">![Creazione del database](aspnet-mvc-4-custom-action-filters/_static/image34.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="e9b16-402">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="e9b16-403">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="e9b16-403">*Creating the database*</span></span>
7. <span data-ttu-id="e9b16-404">La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito.</span><span class="sxs-lookup"><span data-stu-id="e9b16-404">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="e9b16-405">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-405">Then click **Next**.</span></span>

    <span data-ttu-id="e9b16-406">![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="e9b16-406">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="e9b16-407">*Stringa di connessione che punta al Database SQL*</span><span class="sxs-lookup"><span data-stu-id="e9b16-407">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="e9b16-408">Nel **anteprima** pagina, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-408">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="e9b16-409">![Pubblicare l'applicazione web](aspnet-mvc-4-custom-action-filters/_static/image36.png "pubblicare l'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="e9b16-409">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="e9b16-410">*Pubblicare l'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="e9b16-410">*Publishing the web application*</span></span>
9. <span data-ttu-id="e9b16-411">Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="e9b16-411">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="e9b16-412">Appendice c: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="e9b16-412">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="e9b16-413">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="e9b16-413">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e9b16-414">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="e9b16-414">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e9b16-415">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-custom-action-filters/_static/image37.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="e9b16-415">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e9b16-416">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="e9b16-416">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e9b16-417">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="e9b16-417">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e9b16-418">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="e9b16-418">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e9b16-419">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="e9b16-419">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e9b16-420">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="e9b16-420">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e9b16-421">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="e9b16-421">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e9b16-422">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="e9b16-422">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e9b16-423">![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-custom-action-filters/_static/image38.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="e9b16-423">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e9b16-424">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="e9b16-424">*Start typing the snippet name*</span></span>

<span data-ttu-id="e9b16-425">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-custom-action-filters/_static/image39.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="e9b16-425">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e9b16-426">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="e9b16-426">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e9b16-427">![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-custom-action-filters/_static/image40.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="e9b16-427">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e9b16-428">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="e9b16-428">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e9b16-429">***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="e9b16-429">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e9b16-430">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="e9b16-430">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e9b16-431">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="e9b16-431">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e9b16-432">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="e9b16-432">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e9b16-433">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-custom-action-filters/_static/image41.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="e9b16-433">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e9b16-434">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="e9b16-434">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e9b16-435">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-custom-action-filters/_static/image42.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="e9b16-435">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e9b16-436">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="e9b16-436">*Pick the relevant snippet from the list, by clicking on it*</span></span>
