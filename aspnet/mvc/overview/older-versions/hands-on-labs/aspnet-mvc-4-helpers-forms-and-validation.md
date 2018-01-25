---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: La convalida, moduli e gli helper di ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: "In ASP.NET MVC 4 modelli e le esercitazioni pratiche accesso ai dati, è stato durante il caricamento e visualizzazione dei dati dal database. In questo laboratorio pratico, verranno aggiunti per il..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 925d659f42496045089ba056e194ac977c37a8de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="db2b6-104">Convalida, moduli e gli helper di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="db2b6-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>
====================
<span data-ttu-id="db2b6-105">da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="db2b6-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="db2b6-106">In **accesso ai dati e i modelli di ASP.NET MVC 4** pratica, si è stati durante il caricamento e visualizzazione dei dati dal database.</span><span class="sxs-lookup"><span data-stu-id="db2b6-106">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="db2b6-107">In questo laboratorio pratico, verranno aggiunti per il **negozio** applicazione la possibilità di modificare i dati.</span><span class="sxs-lookup"><span data-stu-id="db2b6-107">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>
> 
> <span data-ttu-id="db2b6-108">Con tale scopo, si creerà innanzitutto il controller che supporterà le azioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) di album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-108">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="db2b6-109">Verrà generato un modello di visualizzazione dell'indice usufruire della funzionalità di scaffolding di ASP.NET MVC per visualizzare le proprietà degli album in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="db2b6-109">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="db2b6-110">Per migliorare la visualizzazione, si aggiungerà un helper HTML personalizzato che verrà troncato descrizioni lunghe.</span><span class="sxs-lookup"><span data-stu-id="db2b6-110">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>
> 
> <span data-ttu-id="db2b6-111">Successivamente, si aggiungerà la modifica e creazione di visualizzazioni che consente di modificare gli album nel database, con l'aiuto di elementi del form come elenchi a discesa.</span><span class="sxs-lookup"><span data-stu-id="db2b6-111">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>
> 
> <span data-ttu-id="db2b6-112">Infine, si permettono agli utenti di eliminare un album e inoltre si impedirà l'immissione di dati errati, convalidando l'input.</span><span class="sxs-lookup"><span data-stu-id="db2b6-112">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="db2b6-113">Questa pratica presuppone avere conoscenze di base **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-113">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="db2b6-114">Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **nozioni fondamentali su MVC ASP.NET** le esercitazioni pratiche.</span><span class="sxs-lookup"><span data-stu-id="db2b6-114">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>
> 
> 
> <span data-ttu-id="db2b6-115">Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza tramite l'applicazione di modifiche di lieve entità a un'applicazione Web di esempio fornita nella cartella di origine.</span><span class="sxs-lookup"><span data-stu-id="db2b6-115">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="db2b6-116">Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="db2b6-116">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="db2b6-117">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="db2b6-117">Objectives</span></span>

<span data-ttu-id="db2b6-118">In questa pratica, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="db2b6-118">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="db2b6-119">Creare un controller per supportare le operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="db2b6-119">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="db2b6-120">Generare una visualizzazione di indice per visualizzare le proprietà delle entità in una tabella HTML</span><span class="sxs-lookup"><span data-stu-id="db2b6-120">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="db2b6-121">Aggiungere un helper HTML personalizzati</span><span class="sxs-lookup"><span data-stu-id="db2b6-121">Add a custom HTML helper</span></span>
- <span data-ttu-id="db2b6-122">Creare e personalizzare una visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="db2b6-122">Create and customize an Edit View</span></span>
- <span data-ttu-id="db2b6-123">Distinguere tra i metodi di azione che reagiscono a HTTP-GET o chiamate HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="db2b6-123">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="db2b6-124">Aggiungere e personalizzare un'istruzione Create View</span><span class="sxs-lookup"><span data-stu-id="db2b6-124">Add and customize a Create View</span></span>
- <span data-ttu-id="db2b6-125">Gestire l'eliminazione di un'entità</span><span class="sxs-lookup"><span data-stu-id="db2b6-125">Handle the deletion of an entity</span></span>
- <span data-ttu-id="db2b6-126">Convalidare l'input dell'utente</span><span class="sxs-lookup"><span data-stu-id="db2b6-126">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="db2b6-127">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="db2b6-127">Prerequisites</span></span>

<span data-ttu-id="db2b6-128">È necessario che gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="db2b6-128">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="db2b6-129">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="db2b6-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="db2b6-130">Configurazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-130">Setup</span></span>

<span data-ttu-id="db2b6-131">**L'installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="db2b6-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="db2b6-132">Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db2b6-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="db2b6-133">Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="db2b6-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="db2b6-134">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: tramite i frammenti di codice](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="db2b6-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="db2b6-135">Esercizi</span><span class="sxs-lookup"><span data-stu-id="db2b6-135">Exercises</span></span>

<span data-ttu-id="db2b6-136">Esercizi seguenti che costituiscono questa pratica:</span><span class="sxs-lookup"><span data-stu-id="db2b6-136">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="db2b6-137">Creazione di controller di gestione di Store e la visualizzazione dell'indice</span><span class="sxs-lookup"><span data-stu-id="db2b6-137">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="db2b6-138">Aggiunta di un HTML Helper</span><span class="sxs-lookup"><span data-stu-id="db2b6-138">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="db2b6-139">Creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="db2b6-139">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="db2b6-140">Aggiunta di una vista di creazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-140">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="db2b6-141">Eliminazione di gestione</span><span class="sxs-lookup"><span data-stu-id="db2b6-141">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="db2b6-142">Aggiunta della convalida</span><span class="sxs-lookup"><span data-stu-id="db2b6-142">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="db2b6-143">Utilizzo di jQuery non intrusiva sul lato Client</span><span class="sxs-lookup"><span data-stu-id="db2b6-143">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="db2b6-144">Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio.</span><span class="sxs-lookup"><span data-stu-id="db2b6-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="db2b6-145">Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="db2b6-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="db2b6-146">Tempo stimato per completare questa esercitazione: **60 minuti**</span><span class="sxs-lookup"><span data-stu-id="db2b6-146">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="db2b6-147">Esercizio 1: Creazione di controller di gestione di Store e la visualizzazione dell'indice</span><span class="sxs-lookup"><span data-stu-id="db2b6-147">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="db2b6-148">In questo esercizio, si apprenderà come creare un nuovo controller per supportare le operazioni CRUD, personalizzare il relativo metodo di azione Index per restituire un elenco di album dal database e infine la generazione di un modello di visualizzazione dell'indice sfruttando la possibilità di scaffolding di ASP.NET MVC funzionalità per visualizzare le proprietà degli album in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="db2b6-148">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="db2b6-149">Attività 1 - Creazione di StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="db2b6-149">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="db2b6-150">In questa attività si creerà un nuovo controller chiamato **StoreManagerController** per supportare le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="db2b6-150">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="db2b6-151">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex1-CreatingTheStoreManagerController/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-151">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="db2b6-152">È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="db2b6-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db2b6-153">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db2b6-154">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db2b6-155">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db2b6-156">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db2b6-157">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db2b6-158">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="db2b6-159">Aggiunge un nuovo controller.</span><span class="sxs-lookup"><span data-stu-id="db2b6-159">Add a new controller.</span></span> <span data-ttu-id="db2b6-160">A tale scopo, fare doppio clic su di **controller** cartella in Esplora soluzioni, selezionare **Aggiungi** e quindi la **Controller** comando.</span><span class="sxs-lookup"><span data-stu-id="db2b6-160">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="db2b6-161">Modifica il **Controller** **nome** a **StoreManagerController** e accertarsi che l'opzione **controller MVC con azioni di lettura/scrittura vuote**è selezionata.</span><span class="sxs-lookup"><span data-stu-id="db2b6-161">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="db2b6-162">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-162">Click **Add**.</span></span>

    <span data-ttu-id="db2b6-163">![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "finestra di dialogo Aggiungi controller")</span><span class="sxs-lookup"><span data-stu-id="db2b6-163">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="db2b6-164">*Aggiungere una finestra di dialogo Controller*</span><span class="sxs-lookup"><span data-stu-id="db2b6-164">*Add Controller Dialog*</span></span>

    <span data-ttu-id="db2b6-165">Viene generata una nuova classe Controller.</span><span class="sxs-lookup"><span data-stu-id="db2b6-165">A new Controller class is generated.</span></span> <span data-ttu-id="db2b6-166">Poiché è indicato per aggiungere le azioni di lettura/scrittura, i metodi stub per gli utenti, le azioni CRUD comuni vengono create con i commenti TODO compilati, verrà richiesto di includere la logica specifica dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-166">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="db2b6-167">Attività 2 - personalizzare l'indice StoreManager</span><span class="sxs-lookup"><span data-stu-id="db2b6-167">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="db2b6-168">In questa attività si personalizzerà il metodo di azione StoreManager indice per restituire una vista con l'elenco di album dal database.</span><span class="sxs-lookup"><span data-stu-id="db2b6-168">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="db2b6-169">Nella classe StoreManagerController, aggiungere le seguenti *utilizzando* direttive.</span><span class="sxs-lookup"><span data-stu-id="db2b6-169">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="db2b6-170">(- Frammento di codice *convalida - Ex1 e form e ASP.NET MVC 4 helper utilizzando MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-170">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="db2b6-171">Aggiungere un campo di **StoreManagerController** per contenere un'istanza di **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="db2b6-171">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="db2b6-172">(- Frammento di codice *gli helper di ASP.NET MVC 4, moduli e convalida - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="db2b6-173">Implementare l'operazione sull'indice di StoreManagerController per restituire una vista con l'elenco di album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-173">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="db2b6-174">La logica di azione del Controller sarà molto simile all'azione di indice del StoreController scritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="db2b6-174">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="db2b6-175">Utilizzare LINQ per recuperare tutti gli album, incluse le informazioni Genre e artista per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-175">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="db2b6-176">(- Frammento di codice *gli helper di ASP.NET MVC 4, moduli e convalida - Ex1 StoreManagerController indice*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-176">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="db2b6-177">Attività 3: creazione della visualizzazione dell'indice</span><span class="sxs-lookup"><span data-stu-id="db2b6-177">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="db2b6-178">In questa attività si creerà il modello di visualizzazione dell'indice per visualizzare l'elenco di album restituito dal **StoreManager** Controller.</span><span class="sxs-lookup"><span data-stu-id="db2b6-178">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="db2b6-179">Prima di creare il nuovo modello di visualizzazione, è necessario compilare il progetto in modo che il **finestra di dialogo Aggiungi visualizzazione** viene a conoscenza di **Album** classe da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="db2b6-179">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="db2b6-180">Selezionare **compilare | Compilare MvcMusicStore** per compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-180">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="db2b6-181">Fare doppio clic all'interno di **indice** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-181">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="db2b6-182">Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="db2b6-182">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="db2b6-183">![Aggiungi visualizzazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Aggiungi visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="db2b6-183">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="db2b6-184">*Aggiunta di una vista all'interno del metodo di indice*</span><span class="sxs-lookup"><span data-stu-id="db2b6-184">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="db2b6-185">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della visualizzazione è **indice**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-185">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="db2b6-186">Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="db2b6-186">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="db2b6-187">Selezionare **elenco** dal **modello di scaffolding** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="db2b6-187">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="db2b6-188">Lasciare il **motore di visualizzazione** a **Razor** e gli altri campi con valori predefiniti di valore e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-188">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="db2b6-189">![Aggiunta di una visualizzazione index](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "aggiunta di una vista di indice")</span><span class="sxs-lookup"><span data-stu-id="db2b6-189">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="db2b6-190">*Aggiunta di una vista di indice*</span><span class="sxs-lookup"><span data-stu-id="db2b6-190">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="db2b6-191">Attività 4: personalizzare la pagina della visualizzazione dell'indice</span><span class="sxs-lookup"><span data-stu-id="db2b6-191">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="db2b6-192">In questa attività, si potrà regolare il modello di visualizzazione semplice creato con funzionalità di scaffolding di ASP.NET MVC per visualizzare i campi desiderati.</span><span class="sxs-lookup"><span data-stu-id="db2b6-192">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="db2b6-193">Il **scaffolding** supporto all'interno di ASP.NET MVC genera un semplice modello di visualizzazione in cui sono elencati tutti i campi nel modello Album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-193">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="db2b6-194">**Lo scaffolding** offre un modo rapido per un'introduzione a una visualizzazione fortemente tipizzata: invece di scrivere manualmente il modello di visualizzazione, lo scaffolding rapidamente genera un modello predefinito e quindi è possibile modificare il codice generato.</span><span class="sxs-lookup"><span data-stu-id="db2b6-194">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="db2b6-195">Esaminare il codice creato.</span><span class="sxs-lookup"><span data-stu-id="db2b6-195">Review the code created.</span></span> <span data-ttu-id="db2b6-196">L'elenco di campi generato sarà parte del seguente tabella HTML **Scaffolding** utilizzata per la visualizzazione dei dati tabulari.</span><span class="sxs-lookup"><span data-stu-id="db2b6-196">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="db2b6-197">Sostituire il  **&lt;tabella&gt;**  codice con il codice seguente per visualizzare solo il **Genre**, **artista**, **titolo**, e **prezzo** campi.</span><span class="sxs-lookup"><span data-stu-id="db2b6-197">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="db2b6-198">Questo elimina la **payload** e **Album Art URL** colonne.</span><span class="sxs-lookup"><span data-stu-id="db2b6-198">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="db2b6-199">Inoltre, viene modificato GenreId e ArtistId colonne per visualizzare le proprietà di classe collegati di **Artist.Name** e **Genre.Name**e rimuove il **dettagli** collegamento.</span><span class="sxs-lookup"><span data-stu-id="db2b6-199">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="db2b6-200">Modificare le descrizioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-200">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="db2b6-201">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-201">Task 5 - Running the Application</span></span>

<span data-ttu-id="db2b6-202">In questa attività, si verificherà che il **StoreManager** **indice** modello di visualizzazione viene visualizzato un elenco di album in base alla progettazione dei passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-202">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="db2b6-203">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-203">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-204">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-204">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-205">Modificare l'URL in **/StoreManager** per verificare che venga visualizzato un elenco di album, che mostra i **titolo**, **artista** e **Genre**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-205">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="db2b6-206">![Esplorazione dell'elenco di album](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "esplorazione dell'elenco di album")</span><span class="sxs-lookup"><span data-stu-id="db2b6-206">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="db2b6-207">*Esplorazione dell'elenco di album*</span><span class="sxs-lookup"><span data-stu-id="db2b6-207">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="db2b6-208">Esercizio 2: Aggiunta di un HTML Helper</span><span class="sxs-lookup"><span data-stu-id="db2b6-208">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="db2b6-209">La pagina di indice StoreManager dispone di un potenziale problema: la proprietà Title e Name artista può essere entrambi sufficientemente lungo per lanciare la formattazione della tabella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-209">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="db2b6-210">In questo esercizio si apprenderà come aggiungere un helper HTML personalizzato per troncare il testo.</span><span class="sxs-lookup"><span data-stu-id="db2b6-210">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="db2b6-211">Nella figura seguente, è possibile visualizzare come viene modificato il formato a causa della lunghezza del testo quando si utilizza una dimensione piccola del browser.</span><span class="sxs-lookup"><span data-stu-id="db2b6-211">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="db2b6-212">![Esplorazione dell'elenco di album con di testo non troncato](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "esplorazione dell'elenco di album con di testo non troncato")</span><span class="sxs-lookup"><span data-stu-id="db2b6-212">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="db2b6-213">*Esplorazione dell'elenco di album con di testo non troncato*</span><span class="sxs-lookup"><span data-stu-id="db2b6-213">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="db2b6-214">Attività 1 - estensione l'HTML Helper</span><span class="sxs-lookup"><span data-stu-id="db2b6-214">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="db2b6-215">In questa attività si aggiungerà un nuovo metodo **Truncate** per il **HTML** oggetto esposto all'interno di viste di MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="db2b6-215">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="db2b6-216">A tale scopo, implementare un **metodo di estensione** agli incorporati **System** classe fornita da ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="db2b6-216">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="db2b6-217">Per ulteriori informazioni su **metodi di estensione**, visitare questo articolo di msdn.</span><span class="sxs-lookup"><span data-stu-id="db2b6-217">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="db2b6-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="db2b6-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="db2b6-219">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex2-AddingAnHTMLHelper/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-219">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="db2b6-220">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-220">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="db2b6-221">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="db2b6-221">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db2b6-222">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-222">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db2b6-223">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-223">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db2b6-224">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-224">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db2b6-225">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-225">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db2b6-226">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-226">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db2b6-227">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-227">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="db2b6-228">Aprire la visualizzazione di StoreManager indice.</span><span class="sxs-lookup"><span data-stu-id="db2b6-228">Open StoreManager's Index View.</span></span> <span data-ttu-id="db2b6-229">A tale scopo, in Esplora soluzioni espandere la **viste** cartella, quindi il **StoreManager** e aprire il **cshtml** file.</span><span class="sxs-lookup"><span data-stu-id="db2b6-229">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="db2b6-230">Aggiungere il codice seguente sotto il  **@model**  (direttiva) per definire il **Truncate** metodo di supporto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-230">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="db2b6-231">Attività 2 - testo troncato nella pagina</span><span class="sxs-lookup"><span data-stu-id="db2b6-231">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="db2b6-232">In questa attività, si utilizzerà il **Truncate** metodo troncare il testo nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-232">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="db2b6-233">Aprire la visualizzazione di StoreManager indice.</span><span class="sxs-lookup"><span data-stu-id="db2b6-233">Open StoreManager's Index View.</span></span> <span data-ttu-id="db2b6-234">A tale scopo, in Esplora soluzioni espandere la **viste** cartella, quindi il **StoreManager** e aprire il **cshtml** file.</span><span class="sxs-lookup"><span data-stu-id="db2b6-234">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="db2b6-235">Sostituire le righe che mostrano il **Nome artista** dell'Album e **titolo**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-235">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="db2b6-236">A tale scopo, sostituire le righe seguenti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-236">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="db2b6-237">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-237">Task 3 - Running the Application</span></span>

<span data-ttu-id="db2b6-238">In questa attività, si verificherà che il **StoreManager** **indice** modello di visualizzazione tronca titolo e il nome artista dell'Album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-238">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="db2b6-239">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-239">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-240">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-240">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-241">Modificare l'URL in **/StoreManager** per verificare che prolungata testi nel **titolo** e **artista** colonna vengono troncati.</span><span class="sxs-lookup"><span data-stu-id="db2b6-241">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="db2b6-242">![I nomi dei titoli e artisti troncato](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "troncati i nomi dei titoli e artisti")</span><span class="sxs-lookup"><span data-stu-id="db2b6-242">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="db2b6-243">*Titoli troncati e i nomi degli artisti*</span><span class="sxs-lookup"><span data-stu-id="db2b6-243">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="db2b6-244">Esercizio 3: Creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="db2b6-244">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="db2b6-245">In questo esercizio, si apprenderà come creare un form per consentire ai responsabili di archivio di modifica di un Album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-245">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="db2b6-246">Verrà esplorano il **/StoreManager/Edit/id** URL (**id** da id univoco dell'album da modificare), rendendo in tal modo una chiamata HTTP-GET al server.</span><span class="sxs-lookup"><span data-stu-id="db2b6-246">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="db2b6-247">Il metodo di azione Modifica Controller verranno recuperare Album appropriato dal database, creare un **StoreManagerViewModel** incapsularlo (insieme a un elenco degli artisti e generi) e quindi passare a un modello di visualizzazione per oggetto all'utente, eseguire il rendering della pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="db2b6-247">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="db2b6-248">Questa pagina conterrà un  **&lt;modulo&gt;**  elemento con caselle di testo ed elenchi a discesa per modificare le proprietà dell'Album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-248">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="db2b6-249">Dopo che l'utente aggiorna i valori del form Album e fa clic il **salvare** pulsante, le modifiche vengono inviate tramite un POST HTTP richiamare **/StoreManager/Edit/id**. Anche se l'URL viene mantenuta come l'ultima chiamata, ASP.NET MVC che identifica questa volta è un POST HTTP e pertanto viene eseguito un altro metodo di azione di modifica (uno decorati con **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="db2b6-249">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="db2b6-250">Attività 1: implementazione del metodo di azione di modifica HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="db2b6-250">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="db2b6-251">In questa attività verrà implementata la versione HTTP-GET del metodo di azione di modifica per recuperare Album appropriato dal database, nonché un elenco di tutti i generi e artisti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-251">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="db2b6-252">Creerà un pacchetto dati nel **StoreManagerViewModel** oggetto definito nell'ultimo passaggio, verrà quindi passato a un modello di visualizzazione per il rendering con la risposta.</span><span class="sxs-lookup"><span data-stu-id="db2b6-252">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="db2b6-253">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex3-CreatingTheEditView/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-253">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="db2b6-254">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-254">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="db2b6-255">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="db2b6-255">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db2b6-256">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-256">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db2b6-257">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-257">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db2b6-258">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-258">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db2b6-259">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-259">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db2b6-260">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-260">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db2b6-261">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-261">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="db2b6-262">Aprire il **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="db2b6-262">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="db2b6-263">A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-263">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="db2b6-264">Sostituire il **HTTP-GET modifica** il metodo di azione con il codice seguente per recuperare l'oggetto appropriato **Album** , nonché **generi** e **artisti**Elenca.</span><span class="sxs-lookup"><span data-stu-id="db2b6-264">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="db2b6-265">(- Frammento di codice *helper di ASP.NET MVC 4, moduli e convalida - Ex3 StoreManagerController HTTP-GET Modifica azione*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-265">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="db2b6-266">Si utilizza **System** **SelectList** per artisti e generi anziché il **System.Collections.Generic** elenco.</span><span class="sxs-lookup"><span data-stu-id="db2b6-266">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="db2b6-267">**SelectList** è un modo più semplice per popolare i menu a discesa HTML e gestione di elementi quali la selezione corrente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-267">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="db2b6-268">Creazione di un'istanza e la successiva configurazione di questi oggetti ViewModel nell'azione del controller rende scenario del modulo di modifica più chiara.</span><span class="sxs-lookup"><span data-stu-id="db2b6-268">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="db2b6-269">Attività 2: creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="db2b6-269">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="db2b6-270">In questa attività si creerà un modello di visualizzazione di modifica che consente di visualizzare in un secondo momento le proprietà album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-270">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="db2b6-271">Creare la visualizzazione di modifica.</span><span class="sxs-lookup"><span data-stu-id="db2b6-271">Create the Edit View.</span></span> <span data-ttu-id="db2b6-272">A tale scopo, fare doppio clic all'interno di **modifica** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-272">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="db2b6-273">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della visualizzazione è **modifica**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-273">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="db2b6-274">Controllare il **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **Album (MvcMusicStore.Models)** dal **visualizzare dati classe** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="db2b6-274">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="db2b6-275">Selezionare **modifica** dal **modello di scaffolding** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="db2b6-275">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="db2b6-276">Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-276">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="db2b6-277">![Aggiunta di una visualizzazione di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "aggiunta di una visualizzazione di modifica")</span><span class="sxs-lookup"><span data-stu-id="db2b6-277">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="db2b6-278">*Aggiunta di una visualizzazione di modifica*</span><span class="sxs-lookup"><span data-stu-id="db2b6-278">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="db2b6-279">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-279">Task 3 - Running the Application</span></span>

<span data-ttu-id="db2b6-280">In questa attività, si verificherà che il **StoreManager** **modifica** pagina di visualizzazione consente di visualizzare i valori delle proprietà dell'album passato come parametro.</span><span class="sxs-lookup"><span data-stu-id="db2b6-280">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="db2b6-281">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-281">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-282">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-282">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-283">Modificare l'URL in **/StoreManager/Edit/1** per verificare che vengano visualizzati i valori delle proprietà dell'album passato.</span><span class="sxs-lookup"><span data-stu-id="db2b6-283">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="db2b6-284">![Visualizzazione di modifica dell'Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Modifica visualizzazione Album esplorazione")</span><span class="sxs-lookup"><span data-stu-id="db2b6-284">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="db2b6-285">*Visualizzazione di modifica dell'Album.*</span><span class="sxs-lookup"><span data-stu-id="db2b6-285">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="db2b6-286">Attività 4: implementazione di elenchi a discesa sul modello di Editor Album</span><span class="sxs-lookup"><span data-stu-id="db2b6-286">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="db2b6-287">In questa attività si aggiungerà gli elenchi a discesa per il modello di visualizzazione creato nell'ultima attività, in modo che l'utente può selezionare da un elenco degli artisti e generi.</span><span class="sxs-lookup"><span data-stu-id="db2b6-287">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="db2b6-288">Sostituisci tutto il **Album** fieldset codice con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="db2b6-288">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="db2b6-289">Un **Html.DropDownList** è stato aggiunto supporto per elenchi a discesa per la scelta artisti e generi di eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="db2b6-289">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="db2b6-290">I parametri passati al **Html.DropDownList** sono:</span><span class="sxs-lookup"><span data-stu-id="db2b6-290">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="db2b6-291">Il nome del campo del form (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="db2b6-291">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="db2b6-292">Il **SelectList** dei valori di elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="db2b6-292">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="db2b6-293">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-293">Task 5 - Running the Application</span></span>

<span data-ttu-id="db2b6-294">In questa attività, si verificherà che il **StoreManager** **modifica** pagina di visualizzazione consente di visualizzare gli elenchi a discesa anziché artista e l'ID di genere campi di testo.</span><span class="sxs-lookup"><span data-stu-id="db2b6-294">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="db2b6-295">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-295">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-296">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-296">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-297">Modificare l'URL in **/StoreManager/Edit/1** per verificare che vengano visualizzati gli elenchi a discesa anziché artista e l'ID di genere campi di testo.</span><span class="sxs-lookup"><span data-stu-id="db2b6-297">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="db2b6-298">![Visualizzazione di modifica dell'Album con menu a discesa](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Modifica visualizzazione esplorazione Album con menu a discesa")</span><span class="sxs-lookup"><span data-stu-id="db2b6-298">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="db2b6-299">*Visualizzazione di modifica dell'Album, questa volta con menu a discesa*</span><span class="sxs-lookup"><span data-stu-id="db2b6-299">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="db2b6-300">Attività 6: implementazione del metodo di azione HTTP-POST modifica</span><span class="sxs-lookup"><span data-stu-id="db2b6-300">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="db2b6-301">Ora che la visualizzazione di modifica vengono visualizzate come previsto, è necessario implementare il metodo HTTP-POST Modifica azione per salvare le modifiche apportate all'Album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-301">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="db2b6-302">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db2b6-302">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="db2b6-303">Aprire **StoreManagerController** dal **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-303">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="db2b6-304">Sostituire **HTTP-POST modifica** codice metodo di azione con la seguente (si noti che il metodo che deve essere sostituito versione di overload che riceve due parametri di):</span><span class="sxs-lookup"><span data-stu-id="db2b6-304">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="db2b6-305">(- Frammento di codice *helper di ASP.NET MVC 4, moduli e convalida - Ex3 StoreManagerController HTTP-POST Modifica azione*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-305">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="db2b6-306">Questo metodo verrà eseguito quando l'utente sceglie il **salvare** pulsante della vista ed esegue un POST HTTP dei valori di form al server per renderle persistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="db2b6-306">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="db2b6-307">L'elemento decorator **[HttpPost]** indica che il metodo deve essere utilizzato per gli scenari HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="db2b6-307">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="db2b6-308">Il metodo accetta un **Album** oggetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-308">The method takes an **Album** object.</span></span> <span data-ttu-id="db2b6-309">ASP.NET MVC crea automaticamente l'oggetto di Album da registrato il &lt;modulo&gt; valori.</span><span class="sxs-lookup"><span data-stu-id="db2b6-309">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="db2b6-310">Il metodo verrà eseguire questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="db2b6-310">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="db2b6-311">Se il modello è valido:</span><span class="sxs-lookup"><span data-stu-id="db2b6-311">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="db2b6-312">Aggiornare la voce album nel contesto per contrassegnarlo come un oggetto modificato.</span><span class="sxs-lookup"><span data-stu-id="db2b6-312">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="db2b6-313">Salvare le modifiche e reindirizzare la visualizzazione dell'indice.</span><span class="sxs-lookup"><span data-stu-id="db2b6-313">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="db2b6-314">Se il modello non è valido, popola ViewBag con il **GenreId** e **ArtistId**, verrà restituito la vista con l'oggetto Album ricevuto per consentire all'utente di eseguire qualsiasi aggiornamento necessario.</span><span class="sxs-lookup"><span data-stu-id="db2b6-314">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="db2b6-315">Attività 7: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-315">Task 7 - Running the Application</span></span>

<span data-ttu-id="db2b6-316">In questa attività, si verificherà che il **StoreManager modifica** pagina Salva effettivamente i dati di Album aggiornati nel database.</span><span class="sxs-lookup"><span data-stu-id="db2b6-316">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="db2b6-317">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-317">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-318">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-318">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-319">Modificare l'URL in **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-319">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="db2b6-320">Modificare il titolo dell'Album in **carico** e fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-320">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="db2b6-321">Verificare che il titolo dell'album effettivamente modificato nell'elenco degli album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-321">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="db2b6-322">![L'aggiornamento di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "l'aggiornamento di un album")</span><span class="sxs-lookup"><span data-stu-id="db2b6-322">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="db2b6-323">*L'aggiornamento di un Album*</span><span class="sxs-lookup"><span data-stu-id="db2b6-323">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="db2b6-324">Esercizio 4: Aggiunta di una vista di creazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-324">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="db2b6-325">Ora che il **StoreManagerController** supporta il **modifica** possibilità, in questo esercizio si apprenderà come aggiungere un modello Create View per consentire di memorizzare Manager aggiungere nuovo album all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-325">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="db2b6-326">Come fatto in precedenza con la funzionalità di modifica, implementare lo scenario Create utilizzando due metodi distinti all'interno di **StoreManagerController** classe:</span><span class="sxs-lookup"><span data-stu-id="db2b6-326">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="db2b6-327">Un metodo di azione verrà visualizzato un form vuoto quando l'archivio Gestioni prima visita il **StoreManager/crea** URL.</span><span class="sxs-lookup"><span data-stu-id="db2b6-327">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="db2b6-328">Un secondo metodo di azione gestirà lo scenario in cui il gestore del Negozio seleziona il **salvare** pulsante all'interno del form e invia nuovamente i valori al **StoreManager/crea** URL come un POST HTTP.</span><span class="sxs-lookup"><span data-stu-id="db2b6-328">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="db2b6-329">Attività 1: implementazione del metodo di azione HTTP-GET creare</span><span class="sxs-lookup"><span data-stu-id="db2b6-329">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="db2b6-330">In questa attività verrà implementata la versione HTTP-GET del metodo di azione Create per recuperare un elenco di tutti i generi e artisti, creare pacchetti questi dati in un **StoreManagerViewModel** oggetto, che verrà quindi passato a un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-330">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="db2b6-331">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex4-AddingACreateView/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-331">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="db2b6-332">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-332">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="db2b6-333">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="db2b6-333">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db2b6-334">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-334">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db2b6-335">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-335">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db2b6-336">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-336">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db2b6-337">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-337">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db2b6-338">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-338">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db2b6-339">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-339">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="db2b6-340">Aprire **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="db2b6-340">Open **StoreManagerController** class.</span></span> <span data-ttu-id="db2b6-341">A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-341">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="db2b6-342">Sostituire il **crea** codice metodo di azione con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="db2b6-342">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="db2b6-343">(- Frammento di codice *convalida - Ex4 StoreManagerController HTTP-GET Create action e form e ASP.NET MVC 4 helper*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-343">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="db2b6-344">Attività 2: aggiunta della visualizzazione di creazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-344">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="db2b6-345">In questa attività si aggiungerà il modello Create View che verrà visualizzato un nuovo form Album (vuoto).</span><span class="sxs-lookup"><span data-stu-id="db2b6-345">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="db2b6-346">Fare doppio clic all'interno di **crea** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-346">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="db2b6-347">Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-347">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="db2b6-348">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della visualizzazione è **crea**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-348">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="db2b6-349">Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa e **crea** dal **modello di scaffolding** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="db2b6-349">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="db2b6-350">Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-350">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="db2b6-351">![Aggiunta di una vista crea](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "aggiunta-a-Crea-view.png")</span><span class="sxs-lookup"><span data-stu-id="db2b6-351">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="db2b6-352">*Aggiunta della visualizzazione di creazione*</span><span class="sxs-lookup"><span data-stu-id="db2b6-352">*Adding the Create View*</span></span>
3. <span data-ttu-id="db2b6-353">Aggiornamento di **GenreId** e **ArtistId** campi da utilizzare un elenco a discesa come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="db2b6-353">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="db2b6-354">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-354">Task 3 - Running the Application</span></span>

<span data-ttu-id="db2b6-355">In questa attività, si verificherà che il **StoreManager** **crea** pagina di visualizzazione consente di visualizzare un form Album vuoto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-355">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="db2b6-356">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-356">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-357">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-357">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-358">Modificare l'URL in **StoreManager/creare**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-358">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="db2b6-359">Verificare che un form vuoto viene visualizzato per la compilazione di nuove proprietà Album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-359">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="db2b6-360">![Creare una vista con un form vuoto](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View con un form vuoto")</span><span class="sxs-lookup"><span data-stu-id="db2b6-360">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="db2b6-361">*Creare una vista con un form vuoto*</span><span class="sxs-lookup"><span data-stu-id="db2b6-361">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="db2b6-362">Metodo di azione di creazione di attività 4: implementazione di HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="db2b6-362">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="db2b6-363">In questa attività verrà implementata la versione HTTP-POST del metodo di azione di creazione che verrà richiamato quando un utente fa clic il **salvare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="db2b6-363">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="db2b6-364">Il metodo deve salvare il nuovo album nel database.</span><span class="sxs-lookup"><span data-stu-id="db2b6-364">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="db2b6-365">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db2b6-365">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="db2b6-366">Aprire **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="db2b6-366">Open **StoreManagerController** class.</span></span> <span data-ttu-id="db2b6-367">A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-367">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="db2b6-368">Sostituire **creare HTTP-POST** codice metodo di azione con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="db2b6-368">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="db2b6-369">(- Frammento di codice *convalida - Ex4 StoreManagerController HTTP - POST azione Create e form e ASP.NET MVC 4 helper*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-369">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="db2b6-370">Azione di creazione è molto simile al metodo di azione di modifica precedente ma, anziché impostare l'oggetto come modificata, viene aggiunto al contesto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-370">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="db2b6-371">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-371">Task 5 - Running the Application</span></span>

<span data-ttu-id="db2b6-372">In questa attività, si verificherà che il **StoreManager creare** pagina di visualizzazione consente di creare un nuovo Album e viene quindi reindirizzata alla visualizzazione StoreManager indice.</span><span class="sxs-lookup"><span data-stu-id="db2b6-372">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="db2b6-373">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-373">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-374">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-374">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-375">Modificare l'URL in **StoreManager/creare**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-375">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="db2b6-376">Compilare tutti i campi modulo con i dati per un nuovo Album, simile a quello nella figura riportata di seguito:</span><span class="sxs-lookup"><span data-stu-id="db2b6-376">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="db2b6-377">![Creazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "la creazione di un Album")</span><span class="sxs-lookup"><span data-stu-id="db2b6-377">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="db2b6-378">*Creazione di un Album*</span><span class="sxs-lookup"><span data-stu-id="db2b6-378">*Creating an Album*</span></span>
3. <span data-ttu-id="db2b6-379">Verificare che l'utente viene reindirizzato alla visualizzazione indice StoreManager che include il nuovo Album appena creato.</span><span class="sxs-lookup"><span data-stu-id="db2b6-379">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="db2b6-380">![Nuovo Album creato](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nuovo Album creato")</span><span class="sxs-lookup"><span data-stu-id="db2b6-380">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="db2b6-381">*Nuovo Album creato*</span><span class="sxs-lookup"><span data-stu-id="db2b6-381">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="db2b6-382">Esercizio 5: Gestisce l'eliminazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-382">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="db2b6-383">La possibilità di eliminare gli album non è ancora implementata.</span><span class="sxs-lookup"><span data-stu-id="db2b6-383">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="db2b6-384">Questo è ciò che questo esercizio su.</span><span class="sxs-lookup"><span data-stu-id="db2b6-384">This is what this exercise will be about.</span></span> <span data-ttu-id="db2b6-385">Come prima, si verrà implementare lo scenario di eliminazione utilizzando due metodi distinti all'interno di **StoreManagerController** classe:</span><span class="sxs-lookup"><span data-stu-id="db2b6-385">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="db2b6-386">Un metodo di azione verrà visualizzato un modulo di conferma</span><span class="sxs-lookup"><span data-stu-id="db2b6-386">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="db2b6-387">Un secondo metodo di azione gestirà l'invio del modulo</span><span class="sxs-lookup"><span data-stu-id="db2b6-387">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="db2b6-388">Attività 1: implementazione del metodo di azione Delete HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="db2b6-388">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="db2b6-389">In questa attività si implementerà la versione HTTP-GET del metodo di azione di eliminazione per recuperare le informazioni dell'album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-389">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="db2b6-390">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex5-HandlingDeletion/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-390">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="db2b6-391">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-391">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="db2b6-392">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="db2b6-392">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db2b6-393">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-393">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db2b6-394">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-394">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db2b6-395">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-395">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db2b6-396">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-396">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db2b6-397">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-397">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db2b6-398">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-398">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="db2b6-399">Aprire **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="db2b6-399">Open **StoreManagerController** class.</span></span> <span data-ttu-id="db2b6-400">A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-400">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="db2b6-401">L'azione di eliminazione controller è esattamente come azione del controller dettagli archivio precedente: viene eseguita una query di **album** oggetto dal database utilizzando il **id** fornito l'URL e restituisce il appropriato **vista**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-401">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="db2b6-402">A tale scopo, sostituire HTTP-GET **eliminare** codice metodo di azione con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="db2b6-402">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="db2b6-403">(- Frammento di codice *helper di ASP.NET MVC 4, moduli e convalida - HTTP-GET di eliminazione gestione Ex5 Elimina azione*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-403">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="db2b6-404">Fare doppio clic all'interno di **eliminare** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-404">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="db2b6-405">Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-405">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="db2b6-406">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della visualizzazione è **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-406">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="db2b6-407">Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="db2b6-407">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="db2b6-408">Selezionare **eliminare** dal **modello di scaffolding** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="db2b6-408">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="db2b6-409">Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-409">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="db2b6-410">![Aggiunta di una vista Elimina](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "aggiunta di una visualizzazione di eliminazione")</span><span class="sxs-lookup"><span data-stu-id="db2b6-410">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="db2b6-411">*Aggiunta di una visualizzazione di eliminazione*</span><span class="sxs-lookup"><span data-stu-id="db2b6-411">*Adding a Delete View*</span></span>
6. <span data-ttu-id="db2b6-412">Il modello di eliminazione Mostra tutti i campi dal modello.</span><span class="sxs-lookup"><span data-stu-id="db2b6-412">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="db2b6-413">Mostrare solo il titolo dell'album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-413">You will show only the album's title.</span></span> <span data-ttu-id="db2b6-414">A tale scopo, sostituire il contenuto della visualizzazione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="db2b6-414">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="db2b6-415">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-415">Task 2 - Running the Application</span></span>

<span data-ttu-id="db2b6-416">In questa attività, si verificherà che il **StoreManager** **eliminare** pagina di visualizzazione consente di visualizzare un modulo di conferma l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-416">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="db2b6-417">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-417">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-418">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-418">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-419">Modificare l'URL in **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-419">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="db2b6-420">Selezionare un album da eliminare facendo **eliminare** e verificare che la nuova vista venga caricata.</span><span class="sxs-lookup"><span data-stu-id="db2b6-420">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="db2b6-421">![Eliminazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "eliminazione di un Album")</span><span class="sxs-lookup"><span data-stu-id="db2b6-421">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="db2b6-422">*Eliminazione di un Album*</span><span class="sxs-lookup"><span data-stu-id="db2b6-422">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="db2b6-423">Attività 3-implementazione del metodo di azione Delete HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="db2b6-423">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="db2b6-424">In questa attività verrà implementata la versione HTTP-POST del metodo di azione Delete che verrà richiamato quando un utente fa clic il **eliminare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="db2b6-424">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="db2b6-425">Il metodo deve eliminare album nel database.</span><span class="sxs-lookup"><span data-stu-id="db2b6-425">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="db2b6-426">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db2b6-426">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="db2b6-427">Aprire **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="db2b6-427">Open **StoreManagerController** class.</span></span> <span data-ttu-id="db2b6-428">A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-428">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="db2b6-429">Sostituire **HTTP-POST eliminare** codice metodo di azione con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="db2b6-429">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="db2b6-430">(- Frammento di codice *helper di ASP.NET MVC 4, moduli e convalida - HTTP-POST di eliminazione gestione Ex5 Elimina azione*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-430">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="db2b6-431">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-431">Task 4 - Running the Application</span></span>

<span data-ttu-id="db2b6-432">In questa attività, si verificherà che il **StoreManager Delete** pagina di visualizzazione consente di eliminare un Album e viene quindi reindirizzata alla visualizzazione StoreManager indice.</span><span class="sxs-lookup"><span data-stu-id="db2b6-432">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="db2b6-433">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-433">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-434">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-434">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-435">Modificare l'URL in **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-435">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="db2b6-436">Selezionare un album da eliminare facendo **eliminare.**</span><span class="sxs-lookup"><span data-stu-id="db2b6-436">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="db2b6-437">Confermare l'eliminazione, fare clic su **eliminare** pulsante:</span><span class="sxs-lookup"><span data-stu-id="db2b6-437">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="db2b6-438">![Eliminazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "eliminazione di un Album")</span><span class="sxs-lookup"><span data-stu-id="db2b6-438">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="db2b6-439">*Eliminazione di un Album*</span><span class="sxs-lookup"><span data-stu-id="db2b6-439">*Deleting an Album*</span></span>
3. <span data-ttu-id="db2b6-440">Verificare che album è stato eliminato poiché non viene visualizzato nel **indice** pagina.</span><span class="sxs-lookup"><span data-stu-id="db2b6-440">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="db2b6-441">Esercizio 6: Aggiunta della convalida</span><span class="sxs-lookup"><span data-stu-id="db2b6-441">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="db2b6-442">Attualmente, i moduli di creazione e modifica che sia non eseguono qualsiasi tipo di convalida.</span><span class="sxs-lookup"><span data-stu-id="db2b6-442">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="db2b6-443">Se l'utente lascia un campo obbligatorio vuoto o lettere tipo nel campo prezzo, il primo errore che verrà visualizzato sarà dal database.</span><span class="sxs-lookup"><span data-stu-id="db2b6-443">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="db2b6-444">È possibile aggiungere la convalida per l'applicazione aggiungendo le annotazioni dei dati per la classe di modello.</span><span class="sxs-lookup"><span data-stu-id="db2b6-444">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="db2b6-445">Le annotazioni dei dati consentono di descrivere le regole da applicare alle proprietà del modello e MVC ASP.NET si occuperà di applicazione e visualizzazione messaggio appropriato per gli utenti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-445">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="db2b6-446">Attività 1: aggiunta di annotazioni dati</span><span class="sxs-lookup"><span data-stu-id="db2b6-446">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="db2b6-447">In questa attività si aggiungerà le annotazioni dei dati al modello che consentono la pagina di creazione e modifica Album visualizzare messaggi di convalida quando appropriato.</span><span class="sxs-lookup"><span data-stu-id="db2b6-447">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="db2b6-448">Per una classe di modello semplice, l'aggiunta di un'annotazione di dati viene gestita solo tramite l'aggiunta di un **utilizzando** istruzione per **System.ComponentModel.DataAnnotation**, quindi inserire un **[obbligatorio]**attributo le proprietà appropriate.</span><span class="sxs-lookup"><span data-stu-id="db2b6-448">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="db2b6-449">Renderebbe l'esempio seguente il **nome** un campo obbligatorio nella visualizzazione di proprietà.</span><span class="sxs-lookup"><span data-stu-id="db2b6-449">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="db2b6-450">È leggermente più complesso in casi come questo tipo di applicazione in cui viene generato l'Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="db2b6-450">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="db2b6-451">Se si aggiungono le annotazioni dei dati direttamente a classi del modello, essi verranno sovrascritte se si aggiorna il modello dal database.</span><span class="sxs-lookup"><span data-stu-id="db2b6-451">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="db2b6-452">In alternativa, è possibile apportare le classi di utilizzo delle classi parziali di metadati che sarà disponibile per contenere le annotazioni e sono associati al modello utilizzando il **[MetadataType]** attributo.</span><span class="sxs-lookup"><span data-stu-id="db2b6-452">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="db2b6-453">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex6-AddingValidation/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-453">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="db2b6-454">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-454">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="db2b6-455">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="db2b6-455">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db2b6-456">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-456">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db2b6-457">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-457">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db2b6-458">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-458">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db2b6-459">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-459">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db2b6-460">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-460">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db2b6-461">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-461">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="db2b6-462">Aprire il **Album.cs** dal **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-462">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="db2b6-463">Sostituire **Album.cs** contenuto con il codice evidenziato di seguito, in modo che risulti simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="db2b6-463">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="db2b6-464">La riga **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica che le stringhe vuote dal modello di non essere convertite in null quando viene aggiornato il campo dei dati nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="db2b6-464">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="db2b6-465">Questa impostazione per evitare un'eccezione quando Entity Framework assegna i valori null per il modello prima che i campi di convalida l'annotazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="db2b6-465">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="db2b6-466">(- Frammento di codice *convalida - classe parziale di metadati Ex6 Album e form e ASP.NET MVC 4 helper*)</span><span class="sxs-lookup"><span data-stu-id="db2b6-466">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="db2b6-467">Questo **Album** classe parziale è un **MetadataType** attributo che fa riferimento al **AlbumMetaData** classe per le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="db2b6-467">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="db2b6-468">Questi sono alcuni degli attributi di annotazione dei dati in uso per annotare il modello di Album:</span><span class="sxs-lookup"><span data-stu-id="db2b6-468">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="db2b6-469">Richiesto: indica che la proprietà è un campo obbligatorio</span><span class="sxs-lookup"><span data-stu-id="db2b6-469">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="db2b6-470">DisplayName - definisce il testo da utilizzare per i campi del form e i messaggi di convalida</span><span class="sxs-lookup"><span data-stu-id="db2b6-470">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="db2b6-471">DisplayFormat - specifica come vengono visualizzati e formattazione dei campi dati.</span><span class="sxs-lookup"><span data-stu-id="db2b6-471">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="db2b6-472">StringLength - definisce una lunghezza massima per un campo stringa</span><span class="sxs-lookup"><span data-stu-id="db2b6-472">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="db2b6-473">Intervallo - assegna un valore minimo e massimo per un campo numerico</span><span class="sxs-lookup"><span data-stu-id="db2b6-473">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="db2b6-474">ScaffoldColumn - consente di nascondere i campi da moduli di editor</span><span class="sxs-lookup"><span data-stu-id="db2b6-474">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="db2b6-475">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-475">Task 2 - Running the Application</span></span>

<span data-ttu-id="db2b6-476">In questa attività si testerà convalidano che le pagine di creare e modificare i campi, utilizzando i nomi visualizzati scelti nell'ultima attività.</span><span class="sxs-lookup"><span data-stu-id="db2b6-476">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="db2b6-477">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-477">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db2b6-478">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-478">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-479">Modificare l'URL in **StoreManager/creare**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-479">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="db2b6-480">Verificare che i nomi visualizzati corrispondano a quelli nella classe parziale (ad esempio **Album Art URL** anziché **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="db2b6-480">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="db2b6-481">Fare clic su **crea**, senza la compilazione del form.</span><span class="sxs-lookup"><span data-stu-id="db2b6-481">Click **Create**, without filling the form.</span></span> <span data-ttu-id="db2b6-482">Verificare che i messaggi di convalida corrispondente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-482">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="db2b6-483">![Convalidare i campi nella pagina Crea](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "convalidati i campi nella pagina Crea")</span><span class="sxs-lookup"><span data-stu-id="db2b6-483">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="db2b6-484">*Campi convalidati nella pagina Crea*</span><span class="sxs-lookup"><span data-stu-id="db2b6-484">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="db2b6-485">È possibile verificare che lo stesso si verifica con la **modifica** pagina.</span><span class="sxs-lookup"><span data-stu-id="db2b6-485">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="db2b6-486">Modificare l'URL in **/StoreManager/Edit/1** e verificare che i nomi visualizzati corrispondano a quelli nella classe parziale (ad esempio **Album Art URL** anziché **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="db2b6-486">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="db2b6-487">Vuota il **titolo** e **prezzo** campi e fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-487">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="db2b6-488">Verificare che i messaggi di convalida corrispondente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-488">Verify that you get the corresponding validation messages.</span></span>

    ![Convalidato i campi della pagina di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="db2b6-490">*Convalidato i campi della pagina di modifica*</span><span class="sxs-lookup"><span data-stu-id="db2b6-490">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="db2b6-491">Esercizio 7: Utilizzo di jQuery non intrusiva sul lato Client</span><span class="sxs-lookup"><span data-stu-id="db2b6-491">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="db2b6-492">In questo esercizio, si apprenderà come abilitare la convalida jQuery non intrusiva di MVC 4 sul lato client.</span><span class="sxs-lookup"><span data-stu-id="db2b6-492">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="db2b6-493">JQuery non intrusiva utilizza prefisso dati ajax JavaScript per richiamare metodi di azione sul server anziché intrusivo creazione script client in linea.</span><span class="sxs-lookup"><span data-stu-id="db2b6-493">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="db2b6-494">Attività 1: esecuzione dell'applicazione prima di attivazione non intrusiva jQuery</span><span class="sxs-lookup"><span data-stu-id="db2b6-494">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="db2b6-495">In questa attività verrà eseguita l'applicazione prima di includere jQuery per confrontare i modelli di convalida.</span><span class="sxs-lookup"><span data-stu-id="db2b6-495">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="db2b6-496">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex7-UnobtrusivejQueryValidation/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="db2b6-496">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="db2b6-497">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-497">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="db2b6-498">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="db2b6-498">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db2b6-499">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-499">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db2b6-500">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-500">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db2b6-501">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-501">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db2b6-502">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-502">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db2b6-503">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="db2b6-503">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db2b6-504">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-504">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="db2b6-505">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-505">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="db2b6-506">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-506">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-507">Sfoglia **StoreManager/crea** e fare clic su **crea** senza compilare il modulo per verificare che i messaggi di convalida:</span><span class="sxs-lookup"><span data-stu-id="db2b6-507">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="db2b6-508">![Convalida del client disabilitata](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "la convalida del Client disabilitato")</span><span class="sxs-lookup"><span data-stu-id="db2b6-508">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="db2b6-509">*Convalida del client disabilitata*</span><span class="sxs-lookup"><span data-stu-id="db2b6-509">*Client validation disabled*</span></span>
4. <span data-ttu-id="db2b6-510">Nel browser, aprire il codice sorgente HTML:</span><span class="sxs-lookup"><span data-stu-id="db2b6-510">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="db2b6-511">Attività 2: convalida del Client non intrusiva abilitazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-511">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="db2b6-512">In questa attività si abiliterà jQuery **la convalida del client non intrusiva** da **Web. config** file, per impostazione predefinita impostato su false in tutti i nuovi progetti ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="db2b6-512">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="db2b6-513">Inoltre, si aggiungeranno che necessari script i riferimenti per rendere jQuery lavoro convalida del Client non intrusiva.</span><span class="sxs-lookup"><span data-stu-id="db2b6-513">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="db2b6-514">Aprire **Web. config** file alla radice del progetto e verificare che il **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** valori di chiavi sono impostati su **true**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-514">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="db2b6-515">È inoltre possibile abilitare la convalida del client dal codice in Global.asax.cs per ottenere gli stessi risultati:</span><span class="sxs-lookup"><span data-stu-id="db2b6-515">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="db2b6-516">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="db2b6-516">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="db2b6-517">Inoltre, è possibile assegnare l'attributo ClientValidationEnabled in qualsiasi controller di avere un comportamento personalizzato.</span><span class="sxs-lookup"><span data-stu-id="db2b6-517">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="db2b6-518">Aprire **Create.cshtml** in **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-518">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="db2b6-519">Assicurarsi che i seguenti file di script, **jquery.validate** e **jquery.validate.unobtrusive**, viene fatto riferimento in attraverso la visualizzazione di &quot; **~/bundles/jqueryval** &quot; bundle.</span><span class="sxs-lookup"><span data-stu-id="db2b6-519">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="db2b6-520">Tutte queste librerie jQuery sono incluse nei nuovi progetti MVC 4.</span><span class="sxs-lookup"><span data-stu-id="db2b6-520">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="db2b6-521">È possibile trovare altre librerie nel **/script** nella cartella del progetto è.</span><span class="sxs-lookup"><span data-stu-id="db2b6-521">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="db2b6-522">Per rendere questa convalida il funzionamento delle librerie, è necessario aggiungere un riferimento alla libreria di framework jQuery.</span><span class="sxs-lookup"><span data-stu-id="db2b6-522">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="db2b6-523">Poiché questo riferimento è già stato aggiunto nel  **\_cshtml** file, sarà necessario aggiungerlo in questa vista specifica.</span><span class="sxs-lookup"><span data-stu-id="db2b6-523">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="db2b6-524">Attività 3: in esecuzione l'applicazione utilizzando discreto jQuery convalida</span><span class="sxs-lookup"><span data-stu-id="db2b6-524">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="db2b6-525">In questa attività, si verificherà che il **StoreManager** creare vista modello esegue la convalida lato client utilizzando le librerie di jQuery quando l'utente crea un nuovo album.</span><span class="sxs-lookup"><span data-stu-id="db2b6-525">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="db2b6-526">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-526">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="db2b6-527">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="db2b6-527">The project starts in the Home page.</span></span> <span data-ttu-id="db2b6-528">Sfoglia **StoreManager/crea** e fare clic su **crea** senza compilare il modulo per verificare che i messaggi di convalida:</span><span class="sxs-lookup"><span data-stu-id="db2b6-528">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="db2b6-529">![Convalida del client con jQuery abilitato](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "la convalida del Client con jQuery abilitato")</span><span class="sxs-lookup"><span data-stu-id="db2b6-529">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="db2b6-530">*Convalida del client con jQuery abilitato*</span><span class="sxs-lookup"><span data-stu-id="db2b6-530">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="db2b6-531">Nel browser, aprire il codice sorgente per la visualizzazione di creazione:</span><span class="sxs-lookup"><span data-stu-id="db2b6-531">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="db2b6-532">Per ogni regola di convalida del client, jQuery non intrusiva aggiunge un attributo con dati-val -*rulename*=&quot;*messaggio*&quot;.</span><span class="sxs-lookup"><span data-stu-id="db2b6-532">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="db2b6-533">Di seguito è riportato un elenco di tag che Unobtrusive jQuery inserisce nel campo di input html per eseguire la convalida del client:</span><span class="sxs-lookup"><span data-stu-id="db2b6-533">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="db2b6-534">Dati val</span><span class="sxs-lookup"><span data-stu-id="db2b6-534">Data-val</span></span>
    > - <span data-ttu-id="db2b6-535">Numero di dati val</span><span class="sxs-lookup"><span data-stu-id="db2b6-535">Data-val-number</span></span>
    > - <span data-ttu-id="db2b6-536">Intervallo di dati val</span><span class="sxs-lookup"><span data-stu-id="db2b6-536">Data-val-range</span></span>
    > - <span data-ttu-id="db2b6-537">Dati-val-intervallo-min / dati-val-intervallo-max</span><span class="sxs-lookup"><span data-stu-id="db2b6-537">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="db2b6-538">Val dati obbligatori</span><span class="sxs-lookup"><span data-stu-id="db2b6-538">Data-val-required</span></span>
    > - <span data-ttu-id="db2b6-539">Val-lunghezza dei dati</span><span class="sxs-lookup"><span data-stu-id="db2b6-539">Data-val-length</span></span>
    > - <span data-ttu-id="db2b6-540">Dati-val lunghezza max / min dati-val-lunghezza</span><span class="sxs-lookup"><span data-stu-id="db2b6-540">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="db2b6-541">Tutti i valori di dati vengono riempiti con modello **annotazione dei dati**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-541">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="db2b6-542">Quindi, è possibile eseguire la logica che funziona sul lato server sul lato client.</span><span class="sxs-lookup"><span data-stu-id="db2b6-542">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="db2b6-543">Ad esempio, prezzo e presenta l'annotazione dei dati seguenti nel modello:</span><span class="sxs-lookup"><span data-stu-id="db2b6-543">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="db2b6-544">Dopo aver utilizzato jQuery non intrusiva, il codice generato è:</span><span class="sxs-lookup"><span data-stu-id="db2b6-544">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="db2b6-545">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="db2b6-545">Summary</span></span>

<span data-ttu-id="db2b6-546">Completando questa pratica si è appreso come consentire agli utenti di modificare i dati archiviati nel database con l'uso delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="db2b6-546">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="db2b6-547">Le azioni del controller come indice, creazione, modifica, eliminazione</span><span class="sxs-lookup"><span data-stu-id="db2b6-547">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="db2b6-548">Funzionalità di scaffolding di ASP.NET MVC per la visualizzazione delle proprietà in una tabella HTML</span><span class="sxs-lookup"><span data-stu-id="db2b6-548">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="db2b6-549">Esperienza di helper HTML personalizzati per migliorare l'utente</span><span class="sxs-lookup"><span data-stu-id="db2b6-549">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="db2b6-550">Metodi di azione che reagiscono a HTTP-GET o chiamate HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="db2b6-550">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="db2b6-551">Un modello condiviso editor di modelli di visualizzazione simili come creare e modificare</span><span class="sxs-lookup"><span data-stu-id="db2b6-551">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="db2b6-552">Elementi form come elenchi a discesa</span><span class="sxs-lookup"><span data-stu-id="db2b6-552">Form elements like drop-downs</span></span>
- <span data-ttu-id="db2b6-553">Annotazioni dei dati per la convalida del modello</span><span class="sxs-lookup"><span data-stu-id="db2b6-553">Data annotations for Model validation</span></span>
- <span data-ttu-id="db2b6-554">Convalida lato client tramite jQuery non intrusiva libreria</span><span class="sxs-lookup"><span data-stu-id="db2b6-554">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="db2b6-555">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="db2b6-555">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="db2b6-556">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="db2b6-556">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="db2b6-557">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="db2b6-557">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="db2b6-558">Passare a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="db2b6-558">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="db2b6-559">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="db2b6-559">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="db2b6-560">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-560">Click on **Install Now**.</span></span> <span data-ttu-id="db2b6-561">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="db2b6-561">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="db2b6-562">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-562">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="db2b6-563">![Installa Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="db2b6-563">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="db2b6-564">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="db2b6-564">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="db2b6-565">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="db2b6-565">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="db2b6-567">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="db2b6-567">*Accepting the license terms*</span></span>
5. <span data-ttu-id="db2b6-568">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="db2b6-568">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="db2b6-570">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="db2b6-570">*Installation progress*</span></span>
6. <span data-ttu-id="db2b6-571">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-571">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="db2b6-573">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="db2b6-573">*Installation completed*</span></span>
7. <span data-ttu-id="db2b6-574">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="db2b6-574">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="db2b6-575">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="db2b6-575">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="db2b6-577">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="db2b6-577">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="db2b6-578">Appendice b: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="db2b6-578">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="db2b6-579">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="db2b6-579">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="db2b6-580">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="db2b6-580">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="db2b6-581">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="db2b6-581">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="db2b6-582">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="db2b6-582">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="db2b6-583">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="db2b6-583">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="db2b6-584">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="db2b6-584">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="db2b6-585">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="db2b6-585">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="db2b6-586">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="db2b6-586">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="db2b6-587">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="db2b6-587">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="db2b6-588">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="db2b6-588">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="db2b6-589">![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="db2b6-589">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="db2b6-590">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="db2b6-590">*Start typing the snippet name*</span></span>

<span data-ttu-id="db2b6-591">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="db2b6-591">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="db2b6-592">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="db2b6-592">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="db2b6-593">![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="db2b6-593">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="db2b6-594">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="db2b6-594">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="db2b6-595">***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="db2b6-595">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="db2b6-596">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="db2b6-596">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="db2b6-597">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="db2b6-597">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="db2b6-598">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="db2b6-598">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="db2b6-599">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="db2b6-599">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="db2b6-600">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="db2b6-600">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="db2b6-601">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="db2b6-601">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="db2b6-602">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="db2b6-602">*Pick the relevant snippet from the list, by clicking on it*</span></span>
