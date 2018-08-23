---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtri azione personalizzati di ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fornisce i filtri di azione per l'esecuzione di logica di filtro prima o dopo che viene chiamato un metodo di azione. I filtri azione sono gli attributi personalizzati che...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 0170fda6849c1dfb53b44908ea55ba2cad0dd067
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832194"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="7915c-104">Filtri azione personalizzati di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="7915c-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="7915c-105">da [Camp Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7915c-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7915c-106">Download Web Camp Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="7915c-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="7915c-107">ASP.NET MVC fornisce i filtri di azione per l'esecuzione di logica di filtro prima o dopo che viene chiamato un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7915c-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="7915c-108">I filtri azione sono gli attributi personalizzati che forniscono un modo per aggiungere un comportamento pre-azione e post-azione ai metodi di azione del controller.</span><span class="sxs-lookup"><span data-stu-id="7915c-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="7915c-109">In questo laboratorio pratico si creerà un attributo di filtro azione personalizzato nella soluzione MvcMusicStore per intercettare le richieste del controller e registrare l'attività di un sito in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="7915c-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="7915c-110">Sarà possibile aggiungere il filtro di registrazione tramite l'inserimento di qualsiasi controller o azione.</span><span class="sxs-lookup"><span data-stu-id="7915c-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="7915c-111">Infine, si noterà che la visualizzazione di log che mostra l'elenco di visitatori.</span><span class="sxs-lookup"><span data-stu-id="7915c-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="7915c-112">Questa pratica si presuppone conoscenze di base **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="7915c-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="7915c-113">Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **concetti di base di ASP.NET MVC 4** laboratorio pratico.</span><span class="sxs-lookup"><span data-stu-id="7915c-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="7915c-114">Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile in [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="7915c-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="7915c-115">Il progetto specifico per questo lab è disponibile all'indirizzo [filtri azione personalizzati di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="7915c-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="7915c-116">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="7915c-116">Objectives</span></span>

<span data-ttu-id="7915c-117">In questo laboratorio pratico, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="7915c-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="7915c-118">Creare un attributo di filtro azione personalizzato per estendere le funzionalità di filtro</span><span class="sxs-lookup"><span data-stu-id="7915c-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="7915c-119">Applicare un attributo di filtro personalizzato tramite l'inserimento di un livello specifico</span><span class="sxs-lookup"><span data-stu-id="7915c-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="7915c-120">Registrare un filtri azione personalizzati a livello globale</span><span class="sxs-lookup"><span data-stu-id="7915c-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7915c-121">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7915c-121">Prerequisites</span></span>

<span data-ttu-id="7915c-122">Sono necessari gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="7915c-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="7915c-123">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice A](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="7915c-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7915c-124">Configurazione</span><span class="sxs-lookup"><span data-stu-id="7915c-124">Setup</span></span>

<span data-ttu-id="7915c-125">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="7915c-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="7915c-126">Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7915c-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="7915c-127">Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="7915c-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="7915c-128">Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: Using Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="7915c-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7915c-129">Esercizi</span><span class="sxs-lookup"><span data-stu-id="7915c-129">Exercises</span></span>

<span data-ttu-id="7915c-130">In questo laboratorio pratico include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7915c-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="7915c-131">Esercizio 1: La registrazione delle azioni</span><span class="sxs-lookup"><span data-stu-id="7915c-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="7915c-132">Esercizio 2: Gestione di più filtri azione</span><span class="sxs-lookup"><span data-stu-id="7915c-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="7915c-133">Tempo stimato per completare questa esercitazione: **30 minuti**.</span><span class="sxs-lookup"><span data-stu-id="7915c-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="7915c-134">Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="7915c-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="7915c-135">Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="7915c-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="7915c-136">Esercizio 1: La registrazione delle azioni</span><span class="sxs-lookup"><span data-stu-id="7915c-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="7915c-137">In questo esercizio, si apprenderà come creare un filtro azione personalizzata per il log tramite provider di filtri di ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7915c-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="7915c-138">A tale scopo si applicherà un filtro di registrazione al sito MusicStore che registrerà tutte le attività nei controller selezionato.</span><span class="sxs-lookup"><span data-stu-id="7915c-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="7915c-139">Il filtro estenderà **ActionFilterAttributeClass** ed eseguire l'override **OnActionExecuting** metodo rilevi ogni richiesta e quindi eseguire le azioni di registrazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="7915c-140">Le informazioni di contesto su richieste HTTP, l'esecuzione di metodi, i risultati e i parametri verrà fornita da ASP.NET MVC **ActionExecutingContext** classe **.**</span><span class="sxs-lookup"><span data-stu-id="7915c-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="7915c-141">ASP.NET MVC 4 include anche provider di filtri predefiniti è possibile usare senza creare un filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="7915c-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="7915c-142">ASP.NET MVC 4 fornisce i tipi di filtri seguenti:</span><span class="sxs-lookup"><span data-stu-id="7915c-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="7915c-143">**Autorizzazione** filtrare, rendendo decisioni relative alla sicurezza sulla possibilità di eseguire un metodo di azione, ad esempio l'autenticazione o la convalida delle proprietà della richiesta.</span><span class="sxs-lookup"><span data-stu-id="7915c-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="7915c-144">**Azione** filtro che esegue il wrapping dell'esecuzione del metodo azione.</span><span class="sxs-lookup"><span data-stu-id="7915c-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="7915c-145">Questo filtro può eseguire un'ulteriore elaborazione, ad esempio fornire dati aggiuntivi al metodo di azione, controllare il valore restituito o annullare l'esecuzione del metodo di azione</span><span class="sxs-lookup"><span data-stu-id="7915c-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="7915c-146">**Risultato** filtro che esegue il wrapping di esecuzione dell'oggetto ActionResult.</span><span class="sxs-lookup"><span data-stu-id="7915c-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="7915c-147">Questo filtro è possibile eseguire un'ulteriore elaborazione del risultato, ad esempio la modifica della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="7915c-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="7915c-148">**Eccezione** filtro, che viene eseguita se si verifica un'eccezione non gestita generata in un punto nel metodo di azione, inizia con i filtri di autorizzazione e termina l'esecuzione del risultato.</span><span class="sxs-lookup"><span data-stu-id="7915c-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="7915c-149">I filtri eccezioni sono utilizzabile per attività quali la registrazione o la visualizzazione di una pagina di errore.</span><span class="sxs-lookup"><span data-stu-id="7915c-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="7915c-150">Per altre informazioni sui provider di filtri, visitare il sito Web MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="7915c-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="7915c-151">Informazioni sulla funzionalità di registrazione MVC Music Store applicazioni</span><span class="sxs-lookup"><span data-stu-id="7915c-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="7915c-152">Questa soluzione di Music Store è una nuova tabella di modello di dati per la registrazione del sito **ActionLog**, con i campi seguenti: nome del controller che ha ricevuto una richiesta, azione di chiamate, indirizzo IP del Client e timestamp.</span><span class="sxs-lookup"><span data-stu-id="7915c-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="7915c-153">![Modello di dati. Tabella ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modello di dati. Tabella ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="7915c-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="7915c-154">*Modello di dati - ActionLog tabella*</span><span class="sxs-lookup"><span data-stu-id="7915c-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="7915c-155">La soluzione fornisce una visualizzazione MVC ASP.NET per il log delle azioni che può essere trovato in **MvcMusicStores/viste/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="7915c-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="7915c-156">![Visualizzazione di Log di azione](aspnet-mvc-4-custom-action-filters/_static/image2.png "Visualizza registro azioni")</span><span class="sxs-lookup"><span data-stu-id="7915c-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="7915c-157">*Visualizzazione del Log azioni*</span><span class="sxs-lookup"><span data-stu-id="7915c-157">*Action Log view*</span></span>

<span data-ttu-id="7915c-158">Con questa struttura di data, tutte le operazioni si concentrerà sulle interrompere le richieste del controller ed eseguire la registrazione tramite il filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="7915c-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="7915c-159">Attività 1: creazione di un filtro personalizzato per rilevare richieste del Controller</span><span class="sxs-lookup"><span data-stu-id="7915c-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="7915c-160">In questa attività si creerà una classe di attributo di filtro personalizzato che conterrà la logica di registrazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="7915c-161">A tale scopo verrà estesa in ASP.NET MVC **ActionFilterAttribute** classe e implementare l'interfaccia **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="7915c-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="7915c-162">Il **ActionFilterAttribute** è la classe base per tutti i filtri di attributo.</span><span class="sxs-lookup"><span data-stu-id="7915c-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="7915c-163">Fornisce i metodi seguenti per eseguire una logica specifica dopo e prima dell'esecuzione dell'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="7915c-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="7915c-164">**OnActionExecuting**(ActionExecutingContext filterContext): appena prima dell'azione di metodo viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="7915c-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="7915c-165">**OnActionExecuted**(ActionExecutingContext filterContext): dopo la chiamata del metodo di azione e prima che il risultato viene eseguito (prima della visualizzazione render).</span><span class="sxs-lookup"><span data-stu-id="7915c-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="7915c-166">**OnResultExecuting**(ResultExecutingContext filterContext): solo prima che venga eseguito il risultato (prima della visualizzazione render).</span><span class="sxs-lookup"><span data-stu-id="7915c-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="7915c-167">**OnResultExecuted**(ResultExecutedContext filterContext): dopo l'esecuzione del risultato (dopo il rendering).</span><span class="sxs-lookup"><span data-stu-id="7915c-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="7915c-168">Eseguendo l'override di uno di questi metodi in una classe derivata, è possibile eseguire il proprio codice di filtro.</span><span class="sxs-lookup"><span data-stu-id="7915c-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="7915c-169">Aprire il **Begin** soluzione disponibile all'indirizzo **\Source\Ex01-LoggingActions\Begin** cartella.</span><span class="sxs-lookup"><span data-stu-id="7915c-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="7915c-170">È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="7915c-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="7915c-171">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7915c-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7915c-172">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="7915c-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7915c-173">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="7915c-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7915c-174">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="7915c-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7915c-175">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="7915c-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7915c-176">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="7915c-177">Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="7915c-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="7915c-178">Aggiungere una nuova classe c# nel **filtri** cartella e denominarlo *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="7915c-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="7915c-179">Questa cartella archivierà tutti i filtri personalizzati.</span><span class="sxs-lookup"><span data-stu-id="7915c-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="7915c-180">Aprire **CustomActionFilter.cs** e aggiungere un riferimento a **System** e **MvcMusicStore.Models** gli spazi dei nomi:</span><span class="sxs-lookup"><span data-stu-id="7915c-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="7915c-181">(Code - Snippet *filtri azione personalizzati di ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="7915c-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="7915c-182">Ereditano le **CustomActionFilter** classe **ActionFilterAttribute** e quindi apportare **CustomActionFilter** implementano **IActionFilter** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="7915c-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="7915c-183">Rendere **CustomActionFilter** classe eseguire l'override del metodo **OnActionExecuting** e aggiungere la logica necessaria per registrare l'esecuzione del filtro.</span><span class="sxs-lookup"><span data-stu-id="7915c-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="7915c-184">A questo scopo, aggiungere il codice evidenziato seguente all'interno **CustomActionFilter** classe.</span><span class="sxs-lookup"><span data-stu-id="7915c-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="7915c-185">(Code - Snippet *filtri azione personalizzati di ASP.NET MVC 4 - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="7915c-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="7915c-186">**OnActionExecuting** metodo consiste nell'usare **Entity Framework** per aggiungere un nuovo registro ActionLog.</span><span class="sxs-lookup"><span data-stu-id="7915c-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="7915c-187">Crea e inserisce una nuova istanza di entità con le informazioni di contesto provenienti **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="7915c-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="7915c-188">Altre informazioni, vedere **ControllerContext** classe [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="7915c-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="7915c-189">Attività 2: inserimento di un intercettore di codice nella classe Controller Store</span><span class="sxs-lookup"><span data-stu-id="7915c-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="7915c-190">In questa attività si aggiungerà il filtro personalizzato inserendo, tutte le classi controller e azioni del controller che verranno registrate.</span><span class="sxs-lookup"><span data-stu-id="7915c-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="7915c-191">Ai fini di questo esercizio, la classe Controller Store avrà un log.</span><span class="sxs-lookup"><span data-stu-id="7915c-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="7915c-192">Il metodo **OnActionExecuting** dalla **ActionLogFilterAttribute** filtro personalizzato viene eseguito quando viene chiamato un elemento inserito.</span><span class="sxs-lookup"><span data-stu-id="7915c-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="7915c-193">È anche possibile intercettare un metodo del controller specifico.</span><span class="sxs-lookup"><span data-stu-id="7915c-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="7915c-194">Aprire il **StoreController** alla **MvcMusicStore\Controllers** e aggiungere un riferimento al **filtri** dello spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="7915c-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="7915c-195">Inserire il filtro personalizzato **CustomActionFilter** nelle **StoreController** classe aggiungendo **[CustomActionFilter]** attributo prima della dichiarazione di classe.</span><span class="sxs-lookup"><span data-stu-id="7915c-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="7915c-196">Quando un filtro viene inserito in una classe controller, vengono inseriti anche tutte le relative azioni.</span><span class="sxs-lookup"><span data-stu-id="7915c-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="7915c-197">Se si desidera applicare il filtro solo per un set di azioni, è necessario inserire **[CustomActionFilter]** a ciascuno di essi:</span><span class="sxs-lookup"><span data-stu-id="7915c-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="7915c-198">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="7915c-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="7915c-199">In questa attività si testerà il funzionamento di filtro di registrazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="7915c-200">Si verrà avviare l'applicazione e visitare l'archivio e quindi si controllerà le attività registrate.</span><span class="sxs-lookup"><span data-stu-id="7915c-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="7915c-201">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="7915c-202">Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione log:</span><span class="sxs-lookup"><span data-stu-id="7915c-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="7915c-203">![Registra lo stato di arresto prima dell'attività di pagina](aspnet-mvc-4-custom-action-filters/_static/image3.png "registra lo stato di arresto prima dell'attività di pagina")</span><span class="sxs-lookup"><span data-stu-id="7915c-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="7915c-204">*Stato di arresto di log prima dell'attività di pagina*</span><span class="sxs-lookup"><span data-stu-id="7915c-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="7915c-205">Per impostazione predefinita, verrà sempre visualizzato un unico elemento che viene generato quando si recuperano generi esistente per il menu di scelta.</span><span class="sxs-lookup"><span data-stu-id="7915c-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="7915c-206">Per motivi di semplicità stiamo sottoponendo a pulizia le **ActionLog** tabella ogni volta che l'applicazione viene eseguita in modo che mostrerà solo i log di verifica dell'attività ogni particolare.</span><span class="sxs-lookup"><span data-stu-id="7915c-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="7915c-207">Potrebbe essere necessario rimuovere il codice seguente dal **sessione\_avviare** metodo (nel **Global. asax** classe), in modo da salvare un log cronologico per tutte le azioni eseguite all'interno di Store Controller.</span><span class="sxs-lookup"><span data-stu-id="7915c-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="7915c-208">Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="7915c-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="7915c-209">Passare a **/ActionLog** e se il log è premere vuota **F5** per aggiornare la pagina.</span><span class="sxs-lookup"><span data-stu-id="7915c-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="7915c-210">Verificare che sono state rilevate le visite:</span><span class="sxs-lookup"><span data-stu-id="7915c-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="7915c-211">![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image4.png "log azioni con attività registrate")</span><span class="sxs-lookup"><span data-stu-id="7915c-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7915c-212">*Log delle azioni con attività registrate*</span><span class="sxs-lookup"><span data-stu-id="7915c-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="7915c-213">Esercizio 2: Gestione di più filtri azione</span><span class="sxs-lookup"><span data-stu-id="7915c-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="7915c-214">In questo esercizio verrà aggiungere un secondo filtro azioni personalizzato alla classe StoreController e definire l'ordine specifico in cui verranno eseguiti entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="7915c-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="7915c-215">Si aggiornerà quindi il codice per registrare il filtro a livello globale.</span><span class="sxs-lookup"><span data-stu-id="7915c-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="7915c-216">Sono disponibili diverse opzioni per prendere in considerazione quando si definisce l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="7915c-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="7915c-217">Ad esempio, la proprietà dell'ordine e ambito dei filtri:</span><span class="sxs-lookup"><span data-stu-id="7915c-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="7915c-218">È possibile definire un **ambito** per ognuno dei filtri, ad esempio, è possibile definire l'ambito tutti i filtri dell'azione da eseguire all'interno di **ambito Controller**e tutti i filtri di autorizzazione per l'esecuzione in **ambito globale** .</span><span class="sxs-lookup"><span data-stu-id="7915c-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="7915c-219">Gli ambiti di avere un ordine di esecuzione definito.</span><span class="sxs-lookup"><span data-stu-id="7915c-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="7915c-220">Inoltre, ogni filtro azioni presenta una proprietà dell'ordine che viene usata per determinare l'ordine di esecuzione nell'ambito del filtro.</span><span class="sxs-lookup"><span data-stu-id="7915c-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="7915c-221">Per altre informazioni sull'ordine di esecuzione filtri azione personalizzati, vedere questo articolo di MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="7915c-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="7915c-222">Attività 1: Creazione di un nuovo filtro azioni personalizzato</span><span class="sxs-lookup"><span data-stu-id="7915c-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="7915c-223">In questa attività si creerà un nuovo filtro azioni personalizzato per inserire la classe StoreController, imparare a gestire l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="7915c-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="7915c-224">Aprire il **Begin** soluzione disponibile all'indirizzo **\Source\Ex02-ManagingMultipleActionFilters\Begin** cartella.</span><span class="sxs-lookup"><span data-stu-id="7915c-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="7915c-225">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="7915c-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="7915c-226">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="7915c-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7915c-227">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7915c-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="7915c-228">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="7915c-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="7915c-229">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="7915c-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="7915c-230">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="7915c-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7915c-231">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="7915c-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7915c-232">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="7915c-233">Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="7915c-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="7915c-234">Aggiungere una nuova classe c# nel **filtri** cartella e denominarlo *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="7915c-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="7915c-235">Aprire **MyNewCustomActionFilter.cs** e aggiungere un riferimento a **System** e il **MvcMusicStore.Models** dello spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="7915c-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="7915c-236">(Code - Snippet *filtri azione personalizzati di ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="7915c-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="7915c-237">Sostituire la dichiarazione di classe predefinita con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="7915c-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="7915c-238">(Code - Snippet *filtri azione personalizzati di ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="7915c-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="7915c-239">Questo filtro azioni personalizzato è quasi uguale a quella creata nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="7915c-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="7915c-240">La differenza principale è che ha il *&quot;registrate dal&quot;* attributo aggiornato con il nome di questa nuova classe per identificare il filtro registrato nel log.</span><span class="sxs-lookup"><span data-stu-id="7915c-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="7915c-241">Attività 2: Inserire un nuovo intercettore di codice nella classe StoreController</span><span class="sxs-lookup"><span data-stu-id="7915c-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="7915c-242">In questa attività è verranno aggiunge un nuovo filtro personalizzato nella classe StoreController ed eseguire la soluzione per verificare l'interagiscono come entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="7915c-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="7915c-243">Aprire il **StoreController** classe disponibile all'indirizzo **MvcMusicStore\Controllers** e inserire il nuovo filtro personalizzato **MyNewCustomActionFilter** in  **StoreController** classe, ad esempio è illustrata nel codice seguente.</span><span class="sxs-lookup"><span data-stu-id="7915c-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="7915c-244">A questo punto, eseguire l'applicazione per vedere come funzionano queste due filtri azione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="7915c-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="7915c-245">A tale scopo, premere **F5** e attendere l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="7915c-246">Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione di log.</span><span class="sxs-lookup"><span data-stu-id="7915c-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="7915c-247">![Registra lo stato di arresto prima dell'attività di pagina](aspnet-mvc-4-custom-action-filters/_static/image5.png "registra lo stato di arresto prima dell'attività di pagina")</span><span class="sxs-lookup"><span data-stu-id="7915c-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="7915c-248">*Stato di arresto di log prima dell'attività di pagina*</span><span class="sxs-lookup"><span data-stu-id="7915c-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="7915c-249">Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="7915c-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="7915c-250">Verificare che questa fase. le visite è sono rilevate due volte: una volta per ciascuno dei filtri delle azioni personalizzato aggiunto nel **controller di archiviazione** classe.</span><span class="sxs-lookup"><span data-stu-id="7915c-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="7915c-251">![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image6.png "log azioni con attività registrate")</span><span class="sxs-lookup"><span data-stu-id="7915c-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7915c-252">*Log delle azioni con attività registrate*</span><span class="sxs-lookup"><span data-stu-id="7915c-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="7915c-253">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="7915c-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="7915c-254">Attività 3: Gestione di ordinamento di filtro</span><span class="sxs-lookup"><span data-stu-id="7915c-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="7915c-255">In questa attività si apprenderà come gestire l'ordine di esecuzione dei filtri utilizzando la proprietà dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="7915c-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="7915c-256">Aprire il **StoreController** classe disponibile all'indirizzo **MvcMusicStore\Controllers** e specificare il **ordine** proprietà in entrambi i filtri, ad esempio illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7915c-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="7915c-257">A questo punto, verificare come vengono eseguiti i filtri a seconda del valore della proprietà relativa dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="7915c-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="7915c-258">Si noterà che il filtro con il valore dell'ordine più piccolo (**CustomActionFilter**) è il primo che viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="7915c-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="7915c-259">Premere **F5** e attendere l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="7915c-260">Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione di log.</span><span class="sxs-lookup"><span data-stu-id="7915c-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="7915c-261">![Registra lo stato di arresto prima dell'attività di pagina](aspnet-mvc-4-custom-action-filters/_static/image7.png "registra lo stato di arresto prima dell'attività di pagina")</span><span class="sxs-lookup"><span data-stu-id="7915c-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="7915c-262">*Stato di arresto di log prima dell'attività di pagina*</span><span class="sxs-lookup"><span data-stu-id="7915c-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="7915c-263">Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="7915c-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="7915c-264">Verificare che questa volta, sono state rilevate le visite ordinati in base al valore dell'ordine dei filtri: **CustomActionFilter** registri prima.</span><span class="sxs-lookup"><span data-stu-id="7915c-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="7915c-265">![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image8.png "log azioni con attività registrate")</span><span class="sxs-lookup"><span data-stu-id="7915c-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7915c-266">*Log delle azioni con attività registrate*</span><span class="sxs-lookup"><span data-stu-id="7915c-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="7915c-267">A questo punto, aggiornare il valore dell'ordine dei filtri e verificare come cambia l'ordine di registrazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="7915c-268">Nel **StoreController** classe, aggiornare il valore di ordine dei filtri, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7915c-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="7915c-269">Eseguire nuovamente l'applicazione premendo **F5**.</span><span class="sxs-lookup"><span data-stu-id="7915c-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="7915c-270">Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="7915c-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="7915c-271">Verificare che questo periodo, i log creati da **MyNewCustomActionFilter** filtro viene visualizzato per primo.</span><span class="sxs-lookup"><span data-stu-id="7915c-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="7915c-272">![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image9.png "log azioni con attività registrate")</span><span class="sxs-lookup"><span data-stu-id="7915c-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7915c-273">*Log delle azioni con attività registrate*</span><span class="sxs-lookup"><span data-stu-id="7915c-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="7915c-274">Attività 4: La registrazione di filtri a livello globale</span><span class="sxs-lookup"><span data-stu-id="7915c-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="7915c-275">In questa attività si aggiornerà la soluzione per registrare il nuovo filtro (**MyNewCustomActionFilter**) come filtro globale.</span><span class="sxs-lookup"><span data-stu-id="7915c-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="7915c-276">In questo modo, si verrà attivato per tutte le eseguite le azioni nell'applicazione e non solo di quelli StoreController come nell'attività precedente.</span><span class="sxs-lookup"><span data-stu-id="7915c-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="7915c-277">Nelle **StoreController** classe, rimuovere **[MyNewCustomActionFilter]** attributo e la proprietà dell'ordine dalla **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="7915c-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="7915c-278">Dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7915c-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="7915c-279">Open **Global. asax** del file e individuare il **Application\_avviare** (metodo).</span><span class="sxs-lookup"><span data-stu-id="7915c-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="7915c-280">Si noti che ogni volta che viene avviata l'applicazione registra i filtri globali chiamando **RegisterGlobalFilters** metodo interno **FilterConfig** classe.</span><span class="sxs-lookup"><span data-stu-id="7915c-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="7915c-281">![La registrazione di filtri globali in Global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "la registrazione di filtri globali in Global. asax")</span><span class="sxs-lookup"><span data-stu-id="7915c-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="7915c-282">*La registrazione di filtri globali in Global. asax*</span><span class="sxs-lookup"><span data-stu-id="7915c-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="7915c-283">Aprire **FilterConfig.cs** all'interno del file **App\_avviare** cartella.</span><span class="sxs-lookup"><span data-stu-id="7915c-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="7915c-284">Aggiungere un riferimento all'uso di System; usando MvcMusicStore.Filters; spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="7915c-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="7915c-285">Update **RegisterGlobalFilters** metodo aggiungendo il filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="7915c-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="7915c-286">A tale scopo, aggiungere il codice evidenziato:</span><span class="sxs-lookup"><span data-stu-id="7915c-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="7915c-287">Eseguire l'applicazione premendo **F5**.</span><span class="sxs-lookup"><span data-stu-id="7915c-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="7915c-288">Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="7915c-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="7915c-289">Verificare che ora **[MyNewCustomActionFilter]** è che venga introdotto in HomeController e ActionLogController troppo.</span><span class="sxs-lookup"><span data-stu-id="7915c-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="7915c-290">![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image11.png "log azioni con attività registrate")</span><span class="sxs-lookup"><span data-stu-id="7915c-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7915c-291">*Log delle azioni con l'attività globale registrato*</span><span class="sxs-lookup"><span data-stu-id="7915c-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="7915c-292">Inoltre, è possibile distribuire questa applicazione per siti Web di Azure seguenti [appendice b: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="7915c-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7915c-293">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="7915c-293">Summary</span></span>

<span data-ttu-id="7915c-294">Completando questa pratica si è appreso come estendere un filtro azione per eseguire azioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="7915c-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="7915c-295">Si è appreso anche come inserire i filtri ai controller di pagina.</span><span class="sxs-lookup"><span data-stu-id="7915c-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="7915c-296">Sono stati usati i concetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="7915c-296">The following concepts were used:</span></span>

- <span data-ttu-id="7915c-297">Come creare filtri azione personalizzati con la classe ActionFilterAttribute MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7915c-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="7915c-298">Come inserire i filtri nel controller ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7915c-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="7915c-299">Come gestire filtro ordinamento tramite la proprietà dell'ordine</span><span class="sxs-lookup"><span data-stu-id="7915c-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="7915c-300">Come registrare i filtri a livello globale</span><span class="sxs-lookup"><span data-stu-id="7915c-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="7915c-301">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="7915c-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="7915c-302">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="7915c-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="7915c-303">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="7915c-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="7915c-304">Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="7915c-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7915c-305">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="7915c-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="7915c-306">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="7915c-306">Click on **Install Now**.</span></span> <span data-ttu-id="7915c-307">Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="7915c-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7915c-308">Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7915c-309">![Installa Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="7915c-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="7915c-310">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="7915c-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="7915c-311">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="7915c-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="7915c-313">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="7915c-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7915c-314">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-314">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="7915c-316">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="7915c-316">*Installation progress*</span></span>
6. <span data-ttu-id="7915c-317">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="7915c-317">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="7915c-319">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="7915c-319">*Installation completed*</span></span>
7. <span data-ttu-id="7915c-320">Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="7915c-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="7915c-321">Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="7915c-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="7915c-323">*Visual Studio Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="7915c-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7915c-324">Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="7915c-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="7915c-325">In questa appendice spiega come creare un nuovo sito web dal portale di gestione di Azure e pubblicare l'applicazione che è stato ottenuto seguendo il lab, sfruttando la funzionalità di pubblicazione di distribuzione Web fornita da Azure.</span><span class="sxs-lookup"><span data-stu-id="7915c-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="7915c-326">Attività 1: creazione di un nuovo sito Web di Windows portale di Azure</span><span class="sxs-lookup"><span data-stu-id="7915c-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="7915c-327">Andare alla [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere usando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="7915c-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7915c-328">Con Windows Azure Puoi ospitare gratuitamente 10 siti Web ASP.NET e la scalabilità orizzontale man mano che aumenta il traffico.</span><span class="sxs-lookup"><span data-stu-id="7915c-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="7915c-329">È possibile effettuare l'iscrizione [qui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="7915c-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="7915c-330">![Accedere al portale di Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "accedere al portale di Azure")</span><span class="sxs-lookup"><span data-stu-id="7915c-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="7915c-331">*Accedere al portale di gestione di Azure*</span><span class="sxs-lookup"><span data-stu-id="7915c-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="7915c-332">Fare clic su **New** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="7915c-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="7915c-333">![Creazione di un nuovo sito Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="7915c-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="7915c-334">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="7915c-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="7915c-335">Fare clic su **Compute** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="7915c-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="7915c-336">Quindi selezionare **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="7915c-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="7915c-337">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="7915c-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7915c-338">Un sito Web di Microsoft Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="7915c-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="7915c-339">L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure all'esterno del portale.</span><span class="sxs-lookup"><span data-stu-id="7915c-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="7915c-340">Non include i passaggi per la configurazione di un database.</span><span class="sxs-lookup"><span data-stu-id="7915c-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="7915c-341">![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-custom-action-filters/_static/image19.png "creando un nuovo sito Web mediante Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="7915c-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="7915c-342">*Creazione di un nuovo sito Web utilizzando Creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="7915c-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="7915c-343">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="7915c-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="7915c-344">Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="7915c-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="7915c-345">Verificare che il nuovo sito Web sia in funzione.</span><span class="sxs-lookup"><span data-stu-id="7915c-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="7915c-346">![Passare al nuovo sito web](aspnet-mvc-4-custom-action-filters/_static/image20.png "passare al nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="7915c-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="7915c-347">*Passare al nuovo sito web*</span><span class="sxs-lookup"><span data-stu-id="7915c-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="7915c-348">![Sito Web in esecuzione](aspnet-mvc-4-custom-action-filters/_static/image21.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="7915c-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="7915c-349">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="7915c-349">*Web site running*</span></span>
6. <span data-ttu-id="7915c-350">Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="7915c-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="7915c-351">![Aprire le pagine di gestione sito web](aspnet-mvc-4-custom-action-filters/_static/image22.png "aprire le pagine di gestione sito web")</span><span class="sxs-lookup"><span data-stu-id="7915c-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="7915c-352">*Aprire le pagine di gestione sito Web*</span><span class="sxs-lookup"><span data-stu-id="7915c-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="7915c-353">Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="7915c-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7915c-354">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="7915c-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="7915c-355">Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="7915c-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7915c-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="7915c-357">![Download del sito web di profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image23.png "scaricando il sito web di profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="7915c-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="7915c-358">*Profilo di pubblicazione scaricato il sito Web*</span><span class="sxs-lookup"><span data-stu-id="7915c-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="7915c-359">Scaricare il file di profilo di pubblicazione in una posizione nota.</span><span class="sxs-lookup"><span data-stu-id="7915c-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="7915c-360">In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7915c-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="7915c-361">![Salvare il file di profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image24.png "salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="7915c-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="7915c-362">*Salvare il file di profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="7915c-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="7915c-363">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="7915c-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="7915c-364">Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server.</span><span class="sxs-lookup"><span data-stu-id="7915c-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="7915c-365">Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="7915c-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="7915c-366">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7915c-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="7915c-367">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="7915c-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="7915c-368">Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="7915c-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="7915c-369">Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva.</span><span class="sxs-lookup"><span data-stu-id="7915c-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="7915c-370">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="7915c-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="7915c-371">![Dashboard di Server di Database SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "Dashboard di Server di Database SQL")</span><span class="sxs-lookup"><span data-stu-id="7915c-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="7915c-372">*Dashboard di Server di Database SQL*</span><span class="sxs-lookup"><span data-stu-id="7915c-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="7915c-373">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="7915c-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="7915c-374">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e scegliere il ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="7915c-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="7915c-376">*Aggiunta indirizzo IP del Client*</span><span class="sxs-lookup"><span data-stu-id="7915c-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="7915c-377">Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7915c-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="7915c-379">*Confermare le modifiche*</span><span class="sxs-lookup"><span data-stu-id="7915c-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7915c-380">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="7915c-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="7915c-381">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7915c-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="7915c-382">Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.</span><span class="sxs-lookup"><span data-stu-id="7915c-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="7915c-383">![Pubblicazione dell'applicazione](aspnet-mvc-4-custom-action-filters/_static/image29.png "pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="7915c-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="7915c-384">*Pubblicazione del sito web*</span><span class="sxs-lookup"><span data-stu-id="7915c-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="7915c-385">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="7915c-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="7915c-386">![L'importazione del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image30.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="7915c-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="7915c-387">*Importazione del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="7915c-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="7915c-388">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="7915c-388">Click **Validate Connection**.</span></span> <span data-ttu-id="7915c-389">Dopo aver completata la convalida fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="7915c-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7915c-390">La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.</span><span class="sxs-lookup"><span data-stu-id="7915c-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="7915c-391">![La convalida connessione](aspnet-mvc-4-custom-action-filters/_static/image31.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="7915c-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="7915c-392">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="7915c-392">*Validating connection*</span></span>
4. <span data-ttu-id="7915c-393">Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="7915c-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="7915c-394">![Configurazione della distribuzione Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="7915c-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="7915c-395">*Configurazione della distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="7915c-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="7915c-396">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="7915c-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="7915c-397">Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="7915c-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="7915c-398">Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="7915c-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="7915c-399">Nelle **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="7915c-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="7915c-400">Digitare un nuovo nome del database.</span><span class="sxs-lookup"><span data-stu-id="7915c-400">Type a new database name.</span></span>

     <span data-ttu-id="7915c-401">![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-custom-action-filters/_static/image33.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="7915c-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="7915c-402">*Configurazione di stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="7915c-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="7915c-403">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="7915c-403">Then click **OK**.</span></span> <span data-ttu-id="7915c-404">Quando viene richiesto di creare il database, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="7915c-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="7915c-405">![Creazione del database](aspnet-mvc-4-custom-action-filters/_static/image34.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="7915c-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="7915c-406">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="7915c-406">*Creating the database*</span></span>
7. <span data-ttu-id="7915c-407">La stringa di connessione che si userà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di connessione predefinita nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="7915c-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="7915c-408">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="7915c-408">Then click **Next**.</span></span>

    <span data-ttu-id="7915c-409">![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="7915c-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="7915c-410">*Stringa di connessione che punta al Database SQL*</span><span class="sxs-lookup"><span data-stu-id="7915c-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="7915c-411">Nel **Preview** pagina, fare clic su **Publish**.</span><span class="sxs-lookup"><span data-stu-id="7915c-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="7915c-412">![Pubblicazione dell'applicazione web](aspnet-mvc-4-custom-action-filters/_static/image36.png "pubblicazione dell'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="7915c-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="7915c-413">*Pubblicazione dell'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="7915c-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="7915c-414">Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="7915c-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="7915c-415">Appendice c: uso dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="7915c-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="7915c-416">Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione.</span><span class="sxs-lookup"><span data-stu-id="7915c-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="7915c-417">Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="7915c-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="7915c-418">![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-custom-action-filters/_static/image37.png "frammenti di codice con Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="7915c-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="7915c-419">*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="7915c-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="7915c-420">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="7915c-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="7915c-421">Posizionare il cursore in cui si vuole inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="7915c-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="7915c-422">Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="7915c-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="7915c-423">Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="7915c-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="7915c-424">Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="7915c-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="7915c-425">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="7915c-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="7915c-426">![Iniziare a digitare il nome di frammento](aspnet-mvc-4-custom-action-filters/_static/image38.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="7915c-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="7915c-427">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="7915c-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="7915c-428">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-custom-action-filters/_static/image39.png "premere Tab per selezionare il frammento di codice evidenziata")</span><span class="sxs-lookup"><span data-stu-id="7915c-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="7915c-429">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="7915c-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="7915c-430">![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-custom-action-filters/_static/image40.png "si espanderà il frammento di codice e premere nuovamente Tab")</span><span class="sxs-lookup"><span data-stu-id="7915c-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="7915c-431">*Il frammento di codice e premere nuovamente Tab espanderà*</span><span class="sxs-lookup"><span data-stu-id="7915c-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="7915c-432">***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="7915c-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="7915c-433">Pulsante destro del mouse in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="7915c-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="7915c-434">Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="7915c-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="7915c-435">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="7915c-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="7915c-436">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-custom-action-filters/_static/image41.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="7915c-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="7915c-437">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="7915c-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="7915c-438">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-custom-action-filters/_static/image42.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")</span><span class="sxs-lookup"><span data-stu-id="7915c-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="7915c-439">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*</span><span class="sxs-lookup"><span data-stu-id="7915c-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
