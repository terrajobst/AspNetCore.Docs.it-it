---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Gli helper di ASP.NET MVC 4, moduli e convalida | Microsoft Docs
author: rick-anderson
description: In ASP.NET MVC 4 modelli e le esercitazioni pratiche accesso dati, si sono stati il caricamento e visualizzazione dei dati dal database. In questo laboratorio pratico, verrà aggiunto ai...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 8671ae8e9408e6f05135fa27d56480477521c4ba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836504"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="03c95-104">Gli helper di ASP.NET MVC 4, moduli e convalida</span><span class="sxs-lookup"><span data-stu-id="03c95-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="03c95-105">da [Camp Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="03c95-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="03c95-106">Download Web Camp Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="03c95-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="03c95-107">Nelle **modelli di ASP.NET MVC 4 e l'accesso ai dati** Lab pratici, sono stati durante il caricamento e visualizzazione dei dati dal database.</span><span class="sxs-lookup"><span data-stu-id="03c95-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="03c95-108">In questo laboratorio pratico, verrà aggiunto per il **Music Store** applicazione la possibilità di modificare i dati.</span><span class="sxs-lookup"><span data-stu-id="03c95-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="03c95-109">Tenendo presente questo obiettivo, si creerà innanzitutto il controller che supporterà le azioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) di album.</span><span class="sxs-lookup"><span data-stu-id="03c95-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="03c95-110">Si genererà un modello di visualizzazione dell'indice sfruttando i vantaggi della funzionalità di scaffolding di ASP.NET MVC per visualizzare le proprietà degli album in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="03c95-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="03c95-111">Per migliorare tale visualizzazione, si aggiungerà un helper HTML personalizzato che verrà troncato descrizioni lunghe.</span><span class="sxs-lookup"><span data-stu-id="03c95-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="03c95-112">Successivamente, si aggiungerà la modifica e creare visualizzazioni che consentirà di modificare gli album nel database, con l'aiuto degli elementi del form, ad esempio elenchi a discesa.</span><span class="sxs-lookup"><span data-stu-id="03c95-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="03c95-113">Infine, si consentirà agli utenti di eliminare un album e inoltre si impedirà l'immissione di dati errati, convalidando l'input.</span><span class="sxs-lookup"><span data-stu-id="03c95-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="03c95-114">Questa pratica si presuppone conoscenze di base **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="03c95-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="03c95-115">Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **concetti di base di ASP.NET MVC** laboratorio pratico.</span><span class="sxs-lookup"><span data-stu-id="03c95-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="03c95-116">Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.</span><span class="sxs-lookup"><span data-stu-id="03c95-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="03c95-117">Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="03c95-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="03c95-118">Il progetto specifico per questo lab è disponibile all'indirizzo [helper di ASP.NET MVC 4, moduli e convalida](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="03c95-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="03c95-119">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="03c95-119">Objectives</span></span>

<span data-ttu-id="03c95-120">In questo laboratorio pratico, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="03c95-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="03c95-121">Creare un controller per supportare le operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="03c95-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="03c95-122">Generare una visualizzazione indice per visualizzare le proprietà dell'entità in una tabella HTML</span><span class="sxs-lookup"><span data-stu-id="03c95-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="03c95-123">Aggiungere un helper HTML personalizzato</span><span class="sxs-lookup"><span data-stu-id="03c95-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="03c95-124">Creare e personalizzare una visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="03c95-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="03c95-125">Distinguere i metodi di azione che reagiscono a HTTP-GET o le chiamate HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="03c95-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="03c95-126">Aggiungere e personalizzare un'istruzione Create View</span><span class="sxs-lookup"><span data-stu-id="03c95-126">Add and customize a Create View</span></span>
- <span data-ttu-id="03c95-127">Gestire l'eliminazione di un'entità</span><span class="sxs-lookup"><span data-stu-id="03c95-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="03c95-128">Convalida dell'input utente</span><span class="sxs-lookup"><span data-stu-id="03c95-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="03c95-129">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="03c95-129">Prerequisites</span></span>

<span data-ttu-id="03c95-130">Sono necessari gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="03c95-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="03c95-131">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice A](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="03c95-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="03c95-132">Configurazione</span><span class="sxs-lookup"><span data-stu-id="03c95-132">Setup</span></span>

<span data-ttu-id="03c95-133">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="03c95-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="03c95-134">Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03c95-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="03c95-135">Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="03c95-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="03c95-136">Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: Using Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="03c95-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="03c95-137">Esercizi</span><span class="sxs-lookup"><span data-stu-id="03c95-137">Exercises</span></span>

<span data-ttu-id="03c95-138">Gli esercizi seguenti che costituiscono questo laboratorio pratico:</span><span class="sxs-lookup"><span data-stu-id="03c95-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="03c95-139">Creazione di controller di gestione di Store e la relativa visualizzazione dell'indice</span><span class="sxs-lookup"><span data-stu-id="03c95-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="03c95-140">Aggiunta di un HTML Helper</span><span class="sxs-lookup"><span data-stu-id="03c95-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="03c95-141">Creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="03c95-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="03c95-142">Aggiunta di una visualizzazione Create</span><span class="sxs-lookup"><span data-stu-id="03c95-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="03c95-143">Gestisce l'eliminazione</span><span class="sxs-lookup"><span data-stu-id="03c95-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="03c95-144">Aggiunta della convalida</span><span class="sxs-lookup"><span data-stu-id="03c95-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="03c95-145">Uso di jQuery Unobtrusive sul lato Client</span><span class="sxs-lookup"><span data-stu-id="03c95-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="03c95-146">Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="03c95-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="03c95-147">Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="03c95-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="03c95-148">Tempo stimato per completare questa esercitazione: **60 minuti**</span><span class="sxs-lookup"><span data-stu-id="03c95-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="03c95-149">Esercizio 1: Creazione di controller di gestione di Store e la relativa visualizzazione dell'indice</span><span class="sxs-lookup"><span data-stu-id="03c95-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="03c95-150">In questo esercizio, si apprenderà come creare un nuovo controller di supportare le operazioni CRUD, personalizzare il metodo di azione Index per restituire un elenco di album dal database e infine la generazione di un modello di visualizzazione dell'indice a sfruttare lo scaffolding di ASP.NET MVC funzionalità per visualizzare le proprietà degli album in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="03c95-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="03c95-151">Attività 1: creazione di StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="03c95-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="03c95-152">In questa attività si creerà un nuovo controller denominato **StoreManagerController** per supportare le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="03c95-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="03c95-153">Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex1-CreatingTheStoreManagerController/inizio/** cartella.</span><span class="sxs-lookup"><span data-stu-id="03c95-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="03c95-154">È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="03c95-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03c95-155">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03c95-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03c95-156">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="03c95-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03c95-157">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03c95-158">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03c95-159">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03c95-160">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03c95-161">Aggiungere un nuovo controller.</span><span class="sxs-lookup"><span data-stu-id="03c95-161">Add a new controller.</span></span> <span data-ttu-id="03c95-162">A tale scopo, fare doppio clic sui **controller** cartella all'interno di Esplora soluzioni, scegliere **Add** e quindi il **Controller** comando.</span><span class="sxs-lookup"><span data-stu-id="03c95-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="03c95-163">Modifica il **Controller** **Name** al **StoreManagerController** e assicurarsi che l'opzione **controller MVC con azioni di lettura/scrittura vuote**è selezionato.</span><span class="sxs-lookup"><span data-stu-id="03c95-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="03c95-164">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="03c95-164">Click **Add**.</span></span>

    <span data-ttu-id="03c95-165">![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "finestra di dialogo Aggiungi controller")</span><span class="sxs-lookup"><span data-stu-id="03c95-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="03c95-166">*Aggiungi finestra di dialogo Controller*</span><span class="sxs-lookup"><span data-stu-id="03c95-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="03c95-167">Viene generata una nuova classe Controller.</span><span class="sxs-lookup"><span data-stu-id="03c95-167">A new Controller class is generated.</span></span> <span data-ttu-id="03c95-168">Poiché è indicato per aggiungere azioni per la lettura/scrittura, i metodi stub per tali valori, vengono create azioni CRUD comuni con i commenti TODO compilati, verrà richiesto di includere la logica specifica dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="03c95-169">Attività 2 - personalizzare l'indice StoreManager</span><span class="sxs-lookup"><span data-stu-id="03c95-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="03c95-170">In questa attività si personalizzerà il metodo di azione StoreManager Index per restituire una visualizzazione con l'elenco di album dal database.</span><span class="sxs-lookup"><span data-stu-id="03c95-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="03c95-171">Aggiungere il codice seguente nella classe StoreManagerController *usando* direttive.</span><span class="sxs-lookup"><span data-stu-id="03c95-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="03c95-172">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - Ex1 usando MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="03c95-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="03c95-173">Aggiungere un campo per il **StoreManagerController** per contenere un'istanza di **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="03c95-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="03c95-174">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - MusicStoreEntities Ex1*)</span><span class="sxs-lookup"><span data-stu-id="03c95-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="03c95-175">Implementare l'azione StoreManagerController Index per restituire una visualizzazione con l'elenco di album.</span><span class="sxs-lookup"><span data-stu-id="03c95-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="03c95-176">La logica di azione del Controller sarà molto simile all'azione Index del StoreController scritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="03c95-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="03c95-177">Usare LINQ per recuperare tutti gli album, incluse quelle relative al genere e artista per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="03c95-178">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - indice StoreManagerController Ex1*)</span><span class="sxs-lookup"><span data-stu-id="03c95-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="03c95-179">Attività 3: creare la visualizzazione dell'indice</span><span class="sxs-lookup"><span data-stu-id="03c95-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="03c95-180">In questa attività si creerà il modello di visualizzazione dell'indice per visualizzare l'elenco di album restituiti dai **StoreManager** Controller.</span><span class="sxs-lookup"><span data-stu-id="03c95-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="03c95-181">Prima di creare il nuovo modello di vista, è necessario compilare il progetto in modo che il **finestra di dialogo Aggiungi visualizzazione** conosce le **Album** classe da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="03c95-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="03c95-182">Selezionare **compilare | Compilare MvcMusicStore** per compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="03c95-183">Pulsante destro del mouse all'interno di **indice** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="03c95-184">Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="03c95-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="03c95-185">![Aggiungi visualizzazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Aggiungi visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="03c95-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="03c95-186">*Aggiunta di una vista all'interno del metodo Index*</span><span class="sxs-lookup"><span data-stu-id="03c95-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="03c95-187">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **indice**.</span><span class="sxs-lookup"><span data-stu-id="03c95-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="03c95-188">Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="03c95-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="03c95-189">Selezionare **elenco** dalle **modelli di scaffolding** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="03c95-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="03c95-190">Lasciare il **motore di visualizzazione** al **Razor** mentre gli altri campi con valori predefiniti di valore e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="03c95-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="03c95-191">![Aggiunta di una visualizzazione indice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "aggiunta di una visualizzazione indice")</span><span class="sxs-lookup"><span data-stu-id="03c95-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="03c95-192">*Aggiunta di una visualizzazione indice*</span><span class="sxs-lookup"><span data-stu-id="03c95-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="03c95-193">Attività 4: personalizzazione della visualizzazione Index scaffold</span><span class="sxs-lookup"><span data-stu-id="03c95-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="03c95-194">In questa attività si regolerà il semplice modello di vista creato con funzionalità di scaffolding di ASP.NET MVC in modo che i campi da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="03c95-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="03c95-195">Il **scaffolding** supporto all'interno di ASP.NET MVC genera un semplice modello di vista in cui sono elencati tutti i campi nel modello di Album.</span><span class="sxs-lookup"><span data-stu-id="03c95-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="03c95-196">**Lo scaffolding** offre un modo rapido per iniziare a usare in una visualizzazione fortemente tipizzata: invece di dover scrivere manualmente il modello di vista, lo scaffolding rapidamente genera un modello predefinito e quindi è possibile modificare il codice generato.</span><span class="sxs-lookup"><span data-stu-id="03c95-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="03c95-197">Esaminare il codice creato.</span><span class="sxs-lookup"><span data-stu-id="03c95-197">Review the code created.</span></span> <span data-ttu-id="03c95-198">L'elenco dei campi generato faranno parte della seguente tabella HTML **Scaffolding** utilizza per la visualizzazione di dati tabulari.</span><span class="sxs-lookup"><span data-stu-id="03c95-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="03c95-199">Sostituire il **&lt;tabella&gt;** codice con il codice seguente per visualizzare solo i **Genre**, **artista**, **titolo Album**, e **prezzo** campi.</span><span class="sxs-lookup"><span data-stu-id="03c95-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="03c95-200">Questa procedura elimina le **AlbumId** e **Album Art URL** colonne.</span><span class="sxs-lookup"><span data-stu-id="03c95-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="03c95-201">Inoltre, viene modificato GenreId e ArtistId colonne per visualizzare le relative proprietà di classe collegati di **Artist.Name** e **Genre.Name**e rimuove il **dettagli** collegamento.</span><span class="sxs-lookup"><span data-stu-id="03c95-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="03c95-202">Modificare le descrizioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="03c95-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="03c95-203">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="03c95-204">In questa attività si testerà che il **StoreManager** **indice** Visualizza modello viene visualizzato un elenco di album in base alla progettazione dei passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="03c95-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="03c95-205">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-206">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-206">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-207">Modificare l'URL **/StoreManager** per verificare che venga visualizzato un elenco di album, che mostra le **titolo**, **artista** e **Genre**.</span><span class="sxs-lookup"><span data-stu-id="03c95-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="03c95-208">![Esplorare l'elenco di album](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "esplorare l'elenco di album")</span><span class="sxs-lookup"><span data-stu-id="03c95-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="03c95-209">*Esplorare l'elenco di album*</span><span class="sxs-lookup"><span data-stu-id="03c95-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="03c95-210">Esercizio 2: Aggiunta di un HTML Helper</span><span class="sxs-lookup"><span data-stu-id="03c95-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="03c95-211">La pagina di indice StoreManager dispone di un potenziale problema: le proprietà di titolo e il nome dell'artista possono essere entrambi sufficientemente lungo per lanciare la formattazione della tabella.</span><span class="sxs-lookup"><span data-stu-id="03c95-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="03c95-212">In questa esercitazione si apprenderà come aggiungere un helper HTML personalizzato per troncare il testo.</span><span class="sxs-lookup"><span data-stu-id="03c95-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="03c95-213">Nella figura seguente, è possibile visualizzare come il formato viene modificato a causa della lunghezza del testo quando si usa la dimensione small del browser.</span><span class="sxs-lookup"><span data-stu-id="03c95-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="03c95-214">![Esplorare l'elenco di album con di testo non troncato](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "esplorare l'elenco di album con di testo non troncato")</span><span class="sxs-lookup"><span data-stu-id="03c95-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="03c95-215">*Esplorare l'elenco di album con di testo non troncato*</span><span class="sxs-lookup"><span data-stu-id="03c95-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="03c95-216">Attività 1: estendere l'HTML Helper</span><span class="sxs-lookup"><span data-stu-id="03c95-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="03c95-217">In questa attività si aggiungerà un nuovo metodo **Truncate** per il **HTML** oggetto esposto all'interno di viste di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="03c95-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="03c95-218">A tale scopo, si implementerà un **metodo di estensione** per l'oggetto incorporato **System** classe fornita da ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="03c95-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="03c95-219">Per altre informazioni, vedere **metodi di estensione**, vedere questo articolo di msdn.</span><span class="sxs-lookup"><span data-stu-id="03c95-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="03c95-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="03c95-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="03c95-221">Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex2-AddingAnHTMLHelper/inizio/** cartella.</span><span class="sxs-lookup"><span data-stu-id="03c95-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="03c95-222">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="03c95-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03c95-223">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="03c95-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03c95-224">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03c95-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03c95-225">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="03c95-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03c95-226">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03c95-227">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03c95-228">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03c95-229">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03c95-230">Aprire la visualizzazione del StoreManager indice.</span><span class="sxs-lookup"><span data-stu-id="03c95-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="03c95-231">A tale scopo, in Esplora soluzioni espandere la **viste** cartella, il **StoreManager** e aprire il **index. cshtml** file.</span><span class="sxs-lookup"><span data-stu-id="03c95-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="03c95-232">Aggiungere il codice seguente sotto il <strong>@model</strong> direttiva per definire il <strong>Truncate</strong> metodo helper.</span><span class="sxs-lookup"><span data-stu-id="03c95-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="03c95-233">Attività 2 - testo troncamento nella pagina</span><span class="sxs-lookup"><span data-stu-id="03c95-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="03c95-234">In questa attività si userà il **Truncate** metodo per troncare il testo nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="03c95-235">Aprire la visualizzazione del StoreManager indice.</span><span class="sxs-lookup"><span data-stu-id="03c95-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="03c95-236">A tale scopo, in Esplora soluzioni espandere la **viste** cartella, il **StoreManager** e aprire il **index. cshtml** file.</span><span class="sxs-lookup"><span data-stu-id="03c95-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="03c95-237">Sostituire le righe che mostrano le **nome dell'artista** e un album trae **titolo**.</span><span class="sxs-lookup"><span data-stu-id="03c95-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="03c95-238">A tale scopo, sostituire le righe seguenti.</span><span class="sxs-lookup"><span data-stu-id="03c95-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="03c95-239">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="03c95-240">In questa attività si testerà che il **StoreManager** **indice** Visualizza modello tronca titolo dell'Album e artista.</span><span class="sxs-lookup"><span data-stu-id="03c95-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="03c95-241">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-242">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-242">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-243">Modificare l'URL **/StoreManager** per verificare tale prolungata testi nel **titolo** e **artista** colonna vengono troncati.</span><span class="sxs-lookup"><span data-stu-id="03c95-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="03c95-244">![Troncare i nomi dei titoli e gli artisti](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "troncato titoli e gli artisti nomi")</span><span class="sxs-lookup"><span data-stu-id="03c95-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="03c95-245">*Troncato titoli e nomi di artista*</span><span class="sxs-lookup"><span data-stu-id="03c95-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="03c95-246">Esercizio 3: Creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="03c95-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="03c95-247">In questo esercizio, si apprenderà come creare un form per consentire ai responsabili di archivio di modifica di un Album.</span><span class="sxs-lookup"><span data-stu-id="03c95-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="03c95-248">Si sfoglierà il **/StoreManager/Edit/id** URL (**id** in corso l'id univoco dell'album da modificare), rendendo in tal modo una chiamata HTTP-GET per il server.</span><span class="sxs-lookup"><span data-stu-id="03c95-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="03c95-249">Il metodo di azione Modifica Controller verrà recuperare dell'Album appropriato dal database, creare un **StoreManagerViewModel** oggetto incapsularlo (insieme a un elenco degli artisti e generi) e quindi passarlo a un modello di vista eseguire il rendering della pagina HTML all'utente.</span><span class="sxs-lookup"><span data-stu-id="03c95-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="03c95-250">Questa pagina contiene un **&lt;form&gt;** elemento con caselle di testo ed elenchi a discesa per modificare le proprietà dell'Album.</span><span class="sxs-lookup"><span data-stu-id="03c95-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="03c95-251">Dopo che l'utente aggiorna i valori del modulo Album e fa clic il **salvare** pulsante, le modifiche vengono inviate tramite una richiesta HTTP-POST richiamare **/StoreManager/Edit/id**. Anche se l'URL resta invariato come nell'ultima chiamata, ASP.NET MVC che identifica questa volta è una richiesta HTTP-POST e pertanto viene eseguito un altro metodo di azione di modifica (uno dotato **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="03c95-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="03c95-252">Attività 1: l'implementazione del metodo di azione di modifica HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="03c95-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="03c95-253">In questa attività si implementerà la versione HTTP-GET del metodo di azione di modifica per il recupero dell'Album appropriato dal database, nonché un elenco di tutti i generi e gli artisti.</span><span class="sxs-lookup"><span data-stu-id="03c95-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="03c95-254">Verrà creato il pacchetto i dati nel **StoreManagerViewModel** definito nel passaggio precedente, che verrà quindi passato a un modello di vista per eseguire il rendering della risposta con oggetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="03c95-255">Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex3-CreatingTheEditView/inizio/** cartella.</span><span class="sxs-lookup"><span data-stu-id="03c95-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="03c95-256">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="03c95-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03c95-257">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="03c95-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03c95-258">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03c95-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03c95-259">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="03c95-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03c95-260">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03c95-261">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03c95-262">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03c95-263">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03c95-264">Aprire il **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="03c95-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="03c95-265">A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03c95-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="03c95-266">Sostituire il **HTTP-GET modificare** metodo di azione con il codice seguente per recuperare l'oggetto appropriato **Album** , nonché **generi** e **alle creazioni degli artisti**sono elencati.</span><span class="sxs-lookup"><span data-stu-id="03c95-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="03c95-267">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - Ex3 StoreManagerController HTTP-GET Modifica azione*)</span><span class="sxs-lookup"><span data-stu-id="03c95-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="03c95-268">Si usa **System** **SelectList** alle creazioni degli artisti e generi anziché il **System.Collections.Generic** elenco.</span><span class="sxs-lookup"><span data-stu-id="03c95-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="03c95-269">**SelectList** è un modo più semplice per popolare i menu a discesa HTML e gestione di elementi quali la selezione corrente.</span><span class="sxs-lookup"><span data-stu-id="03c95-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="03c95-270">Creazione di istanze e successiva configurazione di questi oggetti ViewModel nell'azione del controller renderà lo scenario di modulo di modifica più chiara.</span><span class="sxs-lookup"><span data-stu-id="03c95-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="03c95-271">Attività 2: creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="03c95-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="03c95-272">In questa attività si creerà un modello di visualizzazione di modifica in un secondo momento verrà visualizzate le proprietà di album.</span><span class="sxs-lookup"><span data-stu-id="03c95-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="03c95-273">Creare la visualizzazione di modifica.</span><span class="sxs-lookup"><span data-stu-id="03c95-273">Create the Edit View.</span></span> <span data-ttu-id="03c95-274">A tale scopo, fare doppio clic all'interno di **Edit** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="03c95-275">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **modifica**.</span><span class="sxs-lookup"><span data-stu-id="03c95-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="03c95-276">Controllare la **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **Album (MvcMusicStore.Models)** dal **visualizzare dati classe** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="03c95-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="03c95-277">Selezionare **Edit** dalle **modelli di scaffolding** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="03c95-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="03c95-278">Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="03c95-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="03c95-279">![Aggiunta di una visualizzazione di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "aggiunta di una visualizzazione di modifica")</span><span class="sxs-lookup"><span data-stu-id="03c95-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="03c95-280">*Aggiunta di una visualizzazione di modifica*</span><span class="sxs-lookup"><span data-stu-id="03c95-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="03c95-281">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="03c95-282">In questa attività si testerà che il **StoreManager** **modificare** visualizzazione pagina vengono visualizzati i valori delle proprietà dell'album passato come parametro.</span><span class="sxs-lookup"><span data-stu-id="03c95-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="03c95-283">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-284">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-284">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-285">Modificare l'URL **/StoreManager/Edit/1** per verificare che vengano visualizzati i valori delle proprietà dell'album passato.</span><span class="sxs-lookup"><span data-stu-id="03c95-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="03c95-286">![Esplorazione di visualizzazione di modifica di un album trae](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Modifica visualizzazione un album trae di esplorazione")</span><span class="sxs-lookup"><span data-stu-id="03c95-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="03c95-287">*Visualizzazione di modifica di un album trae di esplorazione*</span><span class="sxs-lookup"><span data-stu-id="03c95-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="03c95-288">Attività 4: implementazione di elenchi a discesa sul modello di Album Editor</span><span class="sxs-lookup"><span data-stu-id="03c95-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="03c95-289">In questa attività si aggiungerà elenchi a discesa per il modello di vista creato nell'ultima attività, in modo che l'utente può selezionare da un elenco degli artisti e generi.</span><span class="sxs-lookup"><span data-stu-id="03c95-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="03c95-290">Sostituire tutte le **Album** fieldset codice con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="03c95-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="03c95-291">Un' **Html.DropDownList** helper è stato aggiunto per il rendering di elenchi a discesa per la scelta alle creazioni degli artisti e generi.</span><span class="sxs-lookup"><span data-stu-id="03c95-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="03c95-292">I parametri passati al **Html.DropDownList** sono:</span><span class="sxs-lookup"><span data-stu-id="03c95-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="03c95-293">Il nome del campo del form (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="03c95-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="03c95-294">Il **SelectList** dei valori per l'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="03c95-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="03c95-295">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="03c95-296">In questa attività si testerà che il **StoreManager** **modificare** visualizzazione pagina vengono visualizzati elenchi a discesa anziché artista e dell'ID di genere campi di testo.</span><span class="sxs-lookup"><span data-stu-id="03c95-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="03c95-297">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-298">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-298">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-299">Modificare l'URL **/StoreManager/Edit/1** per verificare che vengano visualizzati elenchi a discesa anziché artista e dell'ID di genere campi di testo.</span><span class="sxs-lookup"><span data-stu-id="03c95-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="03c95-300">![Esplorazione di visualizzazione di modifica di un album trae con elenchi a discesa](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Modifica visualizzazione esplorazione di un album trae con elenchi a discesa")</span><span class="sxs-lookup"><span data-stu-id="03c95-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="03c95-301">*Visualizzazione di modifica di un album trae, questa volta con elenchi a discesa di esplorazione*</span><span class="sxs-lookup"><span data-stu-id="03c95-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="03c95-302">Attività 6: l'implementazione del metodo di azione HTTP-POST modifica</span><span class="sxs-lookup"><span data-stu-id="03c95-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="03c95-303">Ora che la visualizzazione di modifica vengono visualizzate come previsto, è necessario implementare il metodo di azione di modifica HTTP-POST per salvare le modifiche apportate all'album.</span><span class="sxs-lookup"><span data-stu-id="03c95-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="03c95-304">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03c95-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="03c95-305">Aprire **StoreManagerController** dalle **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="03c95-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="03c95-306">Sostituire **HTTP-POST modifica** codice del metodo azione con il codice seguente (si noti che il metodo che deve essere sostituito versione sottoposta a overload che riceve due parametri):</span><span class="sxs-lookup"><span data-stu-id="03c95-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="03c95-307">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - Ex3 StoreManagerController HTTP-POST Modifica azione*)</span><span class="sxs-lookup"><span data-stu-id="03c95-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="03c95-308">Questo metodo verrà eseguito quando l'utente sceglie il **salvare** pulsante della visualizzazione ed esegue una richiesta HTTP-POST dei valori di form al server per renderle persistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="03c95-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="03c95-309">L'elemento decorator **[HttpPost]** indica che il metodo deve essere utilizzato per gli scenari HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="03c95-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="03c95-310">Il metodo accetta un **Album** oggetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-310">The method takes an **Album** object.</span></span> <span data-ttu-id="03c95-311">ASP.NET MVC crea automaticamente l'oggetto di Album da registrato il &lt;form&gt; valori.</span><span class="sxs-lookup"><span data-stu-id="03c95-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="03c95-312">Il metodo verrà eseguiti questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="03c95-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="03c95-313">Se il modello è valido:</span><span class="sxs-lookup"><span data-stu-id="03c95-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="03c95-314">Aggiornare la voce di album nel contesto per contrassegnarlo come un oggetto modificato.</span><span class="sxs-lookup"><span data-stu-id="03c95-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="03c95-315">Salvare le modifiche e reindirizza alla vista index.</span><span class="sxs-lookup"><span data-stu-id="03c95-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="03c95-316">Se il modello non è valido, verrà popolato ViewBag con il **GenreId** e **ArtistId**, verrà restituito la visualizzazione con l'oggetto Album ricevuto per consentire all'utente di eseguire qualsiasi aggiornamento obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="03c95-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="03c95-317">Attività 7: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="03c95-318">In questa attività si testerà che il **StoreManager modifica** Visualizza pagina Salva effettivamente i dati di Album aggiornati nel database.</span><span class="sxs-lookup"><span data-stu-id="03c95-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="03c95-319">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-320">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-320">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-321">Modificare l'URL **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="03c95-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="03c95-322">Modificare il titolo Album **Load** e fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="03c95-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="03c95-323">Verificare che il titolo dell'album effettivamente modificato nell'insieme di album.</span><span class="sxs-lookup"><span data-stu-id="03c95-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="03c95-324">![L'aggiornamento di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "l'aggiornamento di un album")</span><span class="sxs-lookup"><span data-stu-id="03c95-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="03c95-325">*L'aggiornamento di un Album*</span><span class="sxs-lookup"><span data-stu-id="03c95-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="03c95-326">Esercizio 4: Aggiunta di una visualizzazione Create</span><span class="sxs-lookup"><span data-stu-id="03c95-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="03c95-327">Ora che il **StoreManagerController** supporta il **modificare** possibilità, in questo esercizio si apprenderà come aggiungere un modello Create View per consentono di archiviare Manager aggiungere nuovo album all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="03c95-328">Come con la funzionalità di modifica, si implementerà lo scenario di creazione usando due metodi distinti all'interno di **StoreManagerController** classe:</span><span class="sxs-lookup"><span data-stu-id="03c95-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="03c95-329">Un metodo di azione verrà visualizzato un form vuoto quando gestori store visita prima la **StoreManager/crea** URL.</span><span class="sxs-lookup"><span data-stu-id="03c95-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="03c95-330">Un secondo metodo di azione gestirà lo scenario in cui il gestore dell'archivio fa clic sul **salvare** pulsante all'interno del form e invia nuovamente i valori per il **StoreManager/crea** URL come una richiesta HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="03c95-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="03c95-331">Attività 1: l'implementazione del metodo di azione HTTP-GET creare</span><span class="sxs-lookup"><span data-stu-id="03c95-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="03c95-332">In questa attività si implementerà la versione HTTP-GET del metodo di azione Create per recuperare un elenco di tutti i generi e gli artisti, i pacchetti con questi dati un **StoreManagerViewModel** oggetto, che verrà quindi passato a un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="03c95-333">Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex4-AddingACreateView/inizio/** cartella.</span><span class="sxs-lookup"><span data-stu-id="03c95-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="03c95-334">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="03c95-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03c95-335">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="03c95-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03c95-336">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03c95-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03c95-337">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="03c95-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03c95-338">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03c95-339">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03c95-340">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03c95-341">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03c95-342">Aprire **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="03c95-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="03c95-343">A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03c95-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="03c95-344">Sostituire il **Create** codice del metodo azione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="03c95-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="03c95-345">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - azione Ex4 StoreManagerController HTTP-GET crea*)</span><span class="sxs-lookup"><span data-stu-id="03c95-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="03c95-346">Attività 2: aggiunta della visualizzazione Create</span><span class="sxs-lookup"><span data-stu-id="03c95-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="03c95-347">In questa attività si aggiungerà il modello Create View che verrà visualizzato un nuovo form Album (vuoto).</span><span class="sxs-lookup"><span data-stu-id="03c95-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="03c95-348">Pulsante destro del mouse all'interno di **Create** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="03c95-349">Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="03c95-350">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **Create**.</span><span class="sxs-lookup"><span data-stu-id="03c95-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="03c95-351">Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa e **crea** dal **modello di scaffolding** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="03c95-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="03c95-352">Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="03c95-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="03c95-353">![Aggiunta di una visualizzazione di creazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "aggiunta-a-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="03c95-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="03c95-354">*Aggiunta della visualizzazione Create*</span><span class="sxs-lookup"><span data-stu-id="03c95-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="03c95-355">Aggiorna il **GenreId** e **ArtistId** campi da usare un elenco di riepilogo a discesa come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="03c95-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="03c95-356">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="03c95-357">In questa attività si testerà che il **StoreManager** **crea** visualizzazione pagina viene visualizzato un form vuoto di Album.</span><span class="sxs-lookup"><span data-stu-id="03c95-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="03c95-358">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-359">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-359">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-360">Modificare l'URL **StoreManager/creare**.</span><span class="sxs-lookup"><span data-stu-id="03c95-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="03c95-361">Verificare che venga visualizzato un form vuoto per riempire le nuove proprietà di Album.</span><span class="sxs-lookup"><span data-stu-id="03c95-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="03c95-362">![Creare una vista con un form vuoto](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View con un form vuoto")</span><span class="sxs-lookup"><span data-stu-id="03c95-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="03c95-363">*Creare una vista con un form vuoto*</span><span class="sxs-lookup"><span data-stu-id="03c95-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="03c95-364">Attività 4: implementare il protocollo HTTP-POST Crea metodo di azione</span><span class="sxs-lookup"><span data-stu-id="03c95-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="03c95-365">In questa attività si implementerà la versione HTTP-POST del metodo di azione di creazione che verrà richiamato quando un utente sceglie il **salvare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="03c95-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="03c95-366">Il metodo deve salvare il nuovo album nel database.</span><span class="sxs-lookup"><span data-stu-id="03c95-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="03c95-367">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03c95-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="03c95-368">Aprire **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="03c95-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="03c95-369">A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03c95-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="03c95-370">Sostituire **HTTP-POST creare** codice del metodo azione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="03c95-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="03c95-371">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - Ex4 StoreManagerController HTTP - POST azione crea*)</span><span class="sxs-lookup"><span data-stu-id="03c95-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="03c95-372">L'azione di creazione è molto simile al metodo di azione di modifica precedente, ma anziché impostare l'oggetto come modificata, viene aggiunto al contesto.</span><span class="sxs-lookup"><span data-stu-id="03c95-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="03c95-373">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="03c95-374">In questa attività si testerà che il **StoreManager creare** visualizzazione pagina consente di creare un nuovo Album e viene quindi reindirizzata alla vista Index StoreManager.</span><span class="sxs-lookup"><span data-stu-id="03c95-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="03c95-375">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-376">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-376">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-377">Modificare l'URL **StoreManager/creare**.</span><span class="sxs-lookup"><span data-stu-id="03c95-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="03c95-378">Compilare tutti i campi modulo con i dati per un nuovo Album, simile a quello nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="03c95-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="03c95-379">![Creazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "la creazione di un Album")</span><span class="sxs-lookup"><span data-stu-id="03c95-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="03c95-380">*Creazione di un Album*</span><span class="sxs-lookup"><span data-stu-id="03c95-380">*Creating an Album*</span></span>
3. <span data-ttu-id="03c95-381">Verificare che l'utente viene reindirizzato alla vista Index StoreManager che include il nuovo Album appena creato.</span><span class="sxs-lookup"><span data-stu-id="03c95-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="03c95-382">![Creato Nuovo Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nuovo Album creato")</span><span class="sxs-lookup"><span data-stu-id="03c95-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="03c95-383">*Nuovo Album creato*</span><span class="sxs-lookup"><span data-stu-id="03c95-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="03c95-384">Esercizio 5: Gestisce l'eliminazione</span><span class="sxs-lookup"><span data-stu-id="03c95-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="03c95-385">La possibilità di eliminare gli album non è ancora implementata.</span><span class="sxs-lookup"><span data-stu-id="03c95-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="03c95-386">Questo è ciò che in questo esercizio sarà su.</span><span class="sxs-lookup"><span data-stu-id="03c95-386">This is what this exercise will be about.</span></span> <span data-ttu-id="03c95-387">Come in precedenza, si implementerà lo scenario di eliminazione utilizzando due metodi distinti all'interno di **StoreManagerController** classe:</span><span class="sxs-lookup"><span data-stu-id="03c95-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="03c95-388">Un metodo di azione verrà visualizzato un form di conferma</span><span class="sxs-lookup"><span data-stu-id="03c95-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="03c95-389">Un secondo metodo di azione gestirà l'invio del modulo</span><span class="sxs-lookup"><span data-stu-id="03c95-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="03c95-390">Attività 1: l'implementazione del metodo di azione Delete HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="03c95-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="03c95-391">In questa attività si implementerà la versione HTTP-GET del metodo di azione Delete per recuperare le informazioni dell'album.</span><span class="sxs-lookup"><span data-stu-id="03c95-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="03c95-392">Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex5-HandlingDeletion/inizio/** cartella.</span><span class="sxs-lookup"><span data-stu-id="03c95-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="03c95-393">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="03c95-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03c95-394">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="03c95-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03c95-395">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03c95-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03c95-396">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="03c95-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03c95-397">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03c95-398">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03c95-399">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03c95-400">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03c95-401">Aprire **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="03c95-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="03c95-402">A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03c95-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="03c95-403">L'azione del controller Delete è esattamente quello utilizzato per l'azione del controller Store dettagli precedente: viene eseguita una query il **album** oggetto dal database utilizzando il **id** forniti l'URL e restituisce il appropriato **vista**.</span><span class="sxs-lookup"><span data-stu-id="03c95-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="03c95-404">A tale scopo, sostituire l'HTTP-GET **eliminare** codice del metodo azione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="03c95-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="03c95-405">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - HTTP-GET di eliminazione gestione Ex5 Elimina azione*)</span><span class="sxs-lookup"><span data-stu-id="03c95-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="03c95-406">Pulsante destro del mouse all'interno di **eliminare** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="03c95-407">Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="03c95-408">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="03c95-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="03c95-409">Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="03c95-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="03c95-410">Selezionare **eliminare** dalle **modelli di scaffolding** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="03c95-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="03c95-411">Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="03c95-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="03c95-412">![Aggiunta di una visualizzazione di eliminazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "aggiunta di una visualizzazione di eliminazione")</span><span class="sxs-lookup"><span data-stu-id="03c95-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="03c95-413">*Aggiunta di una visualizzazione di eliminazione*</span><span class="sxs-lookup"><span data-stu-id="03c95-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="03c95-414">Il modello di eliminazione Mostra tutti i campi dal modello.</span><span class="sxs-lookup"><span data-stu-id="03c95-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="03c95-415">Si visualizzerà solo titolo dell'album.</span><span class="sxs-lookup"><span data-stu-id="03c95-415">You will show only the album's title.</span></span> <span data-ttu-id="03c95-416">A tale scopo, sostituire il contenuto della visualizzazione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="03c95-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="03c95-417">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="03c95-418">In questa attività si testerà che il **StoreManager** **eliminare** visualizzazione pagina viene visualizzato un form di conferma dell'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="03c95-419">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-420">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-420">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-421">Modificare l'URL **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="03c95-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="03c95-422">Selezionare un album eliminare facendo **Elimina** e verificare che la nuova vista venga caricata.</span><span class="sxs-lookup"><span data-stu-id="03c95-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="03c95-423">![Eliminazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "l'eliminazione di un Album")</span><span class="sxs-lookup"><span data-stu-id="03c95-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="03c95-424">*Eliminazione di un Album*</span><span class="sxs-lookup"><span data-stu-id="03c95-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="03c95-425">Attività 3: implementazione del metodo di azione Delete HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="03c95-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="03c95-426">In questa attività si implementerà la versione HTTP-POST del metodo di azione Delete che verrà richiamato quando un utente sceglie il **eliminare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="03c95-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="03c95-427">Il metodo deve l'eliminazione dell'album nel database.</span><span class="sxs-lookup"><span data-stu-id="03c95-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="03c95-428">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03c95-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="03c95-429">Aprire **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="03c95-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="03c95-430">A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03c95-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="03c95-431">Sostituire **HTTP-POST Delete** codice del metodo azione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="03c95-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="03c95-432">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - HTTP-POST di eliminazione gestione Ex5 Elimina azione*)</span><span class="sxs-lookup"><span data-stu-id="03c95-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="03c95-433">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="03c95-434">In questa attività si testerà che il **StoreManager Delete** visualizzazione pagina consente di eliminare un Album e viene quindi reindirizzata alla vista Index StoreManager.</span><span class="sxs-lookup"><span data-stu-id="03c95-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="03c95-435">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-436">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-436">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-437">Modificare l'URL **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="03c95-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="03c95-438">Selezionare un album eliminare facendo **eliminare.**</span><span class="sxs-lookup"><span data-stu-id="03c95-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="03c95-439">Confermare l'eliminazione facendo **eliminare** pulsante:</span><span class="sxs-lookup"><span data-stu-id="03c95-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="03c95-440">![Eliminazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "l'eliminazione di un Album")</span><span class="sxs-lookup"><span data-stu-id="03c95-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="03c95-441">*Eliminazione di un Album*</span><span class="sxs-lookup"><span data-stu-id="03c95-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="03c95-442">Verificare che sia stato eliminato dell'album poiché non viene visualizzato nei **indice** pagina.</span><span class="sxs-lookup"><span data-stu-id="03c95-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="03c95-443">Esercizio 6: Aggiunta della convalida</span><span class="sxs-lookup"><span data-stu-id="03c95-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="03c95-444">Attualmente, le forme di creazione e modifica che sia non eseguono qualsiasi tipo di convalida.</span><span class="sxs-lookup"><span data-stu-id="03c95-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="03c95-445">Se l'utente lascia un campo obbligatorio vuoto o lettere tipo del campo prezzo, il primo errore che verrà visualizzato sarà dal database.</span><span class="sxs-lookup"><span data-stu-id="03c95-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="03c95-446">È possibile aggiungere la convalida per l'applicazione mediante l'aggiunta di annotazioni dei dati per la classe di modello.</span><span class="sxs-lookup"><span data-stu-id="03c95-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="03c95-447">Annotazioni dei dati per consentono che descrive le regole desiderate applicate alle proprietà del modello e ASP.NET MVC eseguirà l'applicazione e visualizzazione messaggio appropriato per gli utenti.</span><span class="sxs-lookup"><span data-stu-id="03c95-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="03c95-448">Attività 1: aggiunta di annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="03c95-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="03c95-449">In questa attività si aggiungerà le annotazioni dei dati al modello di Album che renderà la pagina di creazione e modifica visualizzare i messaggi di convalida quando appropriato.</span><span class="sxs-lookup"><span data-stu-id="03c95-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="03c95-450">Per una classe di modello semplice, aggiunta di un'annotazione dei dati viene gestita solo tramite l'aggiunta di un **usando** istruzione for **System.ComponentModel.DataAnnotation**, quindi inserire un **[obbligatorio]** attributo per le proprietà appropriate.</span><span class="sxs-lookup"><span data-stu-id="03c95-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="03c95-451">Renderebbe l'esempio seguente il **nome** proprietà un campo obbligatorio nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="03c95-452">Questo è un po' più complesso in casi come questa applicazione in cui viene generato l'Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="03c95-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="03c95-453">Se è stato aggiunto le annotazioni dei dati direttamente le classi del modello, essi verranno sovrascritti se si aggiorna il modello dal database.</span><span class="sxs-lookup"><span data-stu-id="03c95-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="03c95-454">In alternativa, è possibile apportare le classi di uso delle classi parziali che esisterà per contenere le annotazioni e sono associati al modello usando il **[MetadataType]** attributo.</span><span class="sxs-lookup"><span data-stu-id="03c95-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="03c95-455">Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex6-AddingValidation/** cartella.</span><span class="sxs-lookup"><span data-stu-id="03c95-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="03c95-456">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="03c95-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03c95-457">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="03c95-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03c95-458">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03c95-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03c95-459">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="03c95-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03c95-460">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03c95-461">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03c95-462">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03c95-463">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03c95-464">Aprire il **Album.cs** dalle **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="03c95-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="03c95-465">Sostituire **Album.cs** del contenuto con il codice evidenziato di seguito, in modo che risulti simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="03c95-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="03c95-466">La riga **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica che le stringhe vuote dal modello non verranno convertite in null quando il campo dati viene aggiornato nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="03c95-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="03c95-467">Questa impostazione verrà evitare un'eccezione quando Entity Framework assegna i valori null per il modello prima di annotazione dei dati verifica i campi.</span><span class="sxs-lookup"><span data-stu-id="03c95-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="03c95-468">(Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - classe parziale i metadati di Album Ex6*)</span><span class="sxs-lookup"><span data-stu-id="03c95-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="03c95-469">Ciò **Album** classe parziale ha un' **MetadataType** attributo che fa riferimento al **AlbumMetaData** classe per le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="03c95-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="03c95-470">Questi sono alcuni degli attributi di annotazione dei dati in uso per annotare il modello di Album:</span><span class="sxs-lookup"><span data-stu-id="03c95-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="03c95-471">Richiesto: indica che la proprietà è un campo obbligatorio</span><span class="sxs-lookup"><span data-stu-id="03c95-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="03c95-472">DisplayName - definisce il testo da usare per i campi del form e i messaggi di convalida</span><span class="sxs-lookup"><span data-stu-id="03c95-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="03c95-473">DisplayFormat: specifica la modalità di visualizzazione e formattazione dei campi dati.</span><span class="sxs-lookup"><span data-stu-id="03c95-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="03c95-474">StringLength - definisce una lunghezza massima per un campo stringa</span><span class="sxs-lookup"><span data-stu-id="03c95-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="03c95-475">Intervallo: fornisce un valore minimo e massimo per un campo numerico</span><span class="sxs-lookup"><span data-stu-id="03c95-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="03c95-476">ScaffoldColumn - consente di nascondere i campi dall'editor form</span><span class="sxs-lookup"><span data-stu-id="03c95-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="03c95-477">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="03c95-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="03c95-478">In questa attività si testerà che le pagine di creazione e modifica è convalidare campi, usando i nomi visualizzati scelti nell'ultima attività.</span><span class="sxs-lookup"><span data-stu-id="03c95-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="03c95-479">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03c95-480">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-480">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-481">Modificare l'URL **StoreManager/creare**.</span><span class="sxs-lookup"><span data-stu-id="03c95-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="03c95-482">Verificare che i nomi visualizzati corrispondano a quelli nella classe parziale (ad esempio **URL Art Album** invece di **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="03c95-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="03c95-483">Fare clic su **Create**, senza la compilazione del form.</span><span class="sxs-lookup"><span data-stu-id="03c95-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="03c95-484">Verificare che i messaggi di convalida corrispondente.</span><span class="sxs-lookup"><span data-stu-id="03c95-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="03c95-485">![Convalidare i campi nella pagina di creazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "convalidati i campi nella pagina Crea")</span><span class="sxs-lookup"><span data-stu-id="03c95-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="03c95-486">*Convalidato i campi nella pagina Crea*</span><span class="sxs-lookup"><span data-stu-id="03c95-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="03c95-487">È possibile verificare che lo stesso accade con la **modifica** pagina.</span><span class="sxs-lookup"><span data-stu-id="03c95-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="03c95-488">Modificare l'URL **/StoreManager/Edit/1** e verificare che i nomi visualizzati corrispondano a quelli nella classe parziale (ad esempio **Album Art URL** invece di **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="03c95-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="03c95-489">Vuoto il **Title** e **prezzo** campi e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="03c95-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="03c95-490">Verificare che i messaggi di convalida corrispondente.</span><span class="sxs-lookup"><span data-stu-id="03c95-490">Verify that you get the corresponding validation messages.</span></span>

    ![Convalidato campi nella pagina di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="03c95-492">*Convalidato campi nella pagina di modifica*</span><span class="sxs-lookup"><span data-stu-id="03c95-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="03c95-493">Esercizio 7: Uso di jQuery Unobtrusive sul lato Client</span><span class="sxs-lookup"><span data-stu-id="03c95-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="03c95-494">In questo esercizio, si apprenderà come abilitare MVC 4 Unobtrusive validation di jQuery sul lato client.</span><span class="sxs-lookup"><span data-stu-id="03c95-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="03c95-495">Il componente aggiuntivo jQuery Unobtrusive Usa dati ajax prefisso JavaScript per richiamare i metodi di azione sul server anziché intrusivo che genera gli script client inline.</span><span class="sxs-lookup"><span data-stu-id="03c95-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="03c95-496">Attività 1: esecuzione dell'applicazione prima di jQuery Unobtrusive abilitazione</span><span class="sxs-lookup"><span data-stu-id="03c95-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="03c95-497">In questa attività si eseguirà l'applicazione prima di includere jQuery per confrontare i modelli di convalida.</span><span class="sxs-lookup"><span data-stu-id="03c95-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="03c95-498">Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex7-UnobtrusivejQueryValidation/** cartella.</span><span class="sxs-lookup"><span data-stu-id="03c95-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="03c95-499">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="03c95-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03c95-500">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="03c95-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03c95-501">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03c95-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03c95-502">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="03c95-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03c95-503">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="03c95-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03c95-504">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03c95-505">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03c95-506">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03c95-507">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="03c95-508">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-508">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-509">Esplorare **StoreManager/creare** e fare clic su **crea** senza compilare il modulo per verificare che i messaggi di convalida:</span><span class="sxs-lookup"><span data-stu-id="03c95-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="03c95-510">![Convalida del client disabilitata](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "disabilitata la convalida del Client")</span><span class="sxs-lookup"><span data-stu-id="03c95-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="03c95-511">*Disabilitata la convalida del client*</span><span class="sxs-lookup"><span data-stu-id="03c95-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="03c95-512">Nel browser, aprire il codice sorgente HTML:</span><span class="sxs-lookup"><span data-stu-id="03c95-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="03c95-513">Attività 2: abilitare la convalida del Client non intrusivi</span><span class="sxs-lookup"><span data-stu-id="03c95-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="03c95-514">In questa attività si abiliterà jQuery **la convalida del client non intrusivi** dalla **Web. config** file, che è impostato su false in tutti i nuovi progetti ASP.NET MVC 4 per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="03c95-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="03c95-515">Inoltre, si aggiungerà che la necessità di generare script i riferimenti per rendere jQuery lavoro Unobtrusive Validation di Client.</span><span class="sxs-lookup"><span data-stu-id="03c95-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="03c95-516">Aprire **Web. config** file alla radice del progetto e assicurarsi che il **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** i valori delle chiavi vengono impostati **true**.</span><span class="sxs-lookup"><span data-stu-id="03c95-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="03c95-517">È anche possibile abilitare la convalida del client dal codice in Global.asax.cs per ottenere gli stessi risultati:</span><span class="sxs-lookup"><span data-stu-id="03c95-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="03c95-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="03c95-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="03c95-519">Inoltre, è possibile assegnare attributi ClientValidationEnabled in qualsiasi controller di avere un comportamento personalizzato.</span><span class="sxs-lookup"><span data-stu-id="03c95-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="03c95-520">Aprire **create. cshtml** alla **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="03c95-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="03c95-521">Assicurarsi che i file di script seguente, **jQuery. Validate** e **jquery.validate.unobtrusive**, viene fatto riferimento in tramite la vista il &quot; **~/bundles/jqueryval** &quot; bundle.</span><span class="sxs-lookup"><span data-stu-id="03c95-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="03c95-522">Tutte queste librerie jQuery sono incluse nei nuovi progetti MVC 4.</span><span class="sxs-lookup"><span data-stu-id="03c95-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="03c95-523">È possibile trovare altre librerie nel **/Scripts** cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="03c95-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="03c95-524">Per rendere questa convalida il funzionamento delle librerie, è necessario aggiungere un riferimento alla libreria framework jQuery.</span><span class="sxs-lookup"><span data-stu-id="03c95-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="03c95-525">Poiché il riferimento è già stato aggiunto nel  **\_layout. cshtml** file, è necessario aggiungerlo in questa vista specifica.</span><span class="sxs-lookup"><span data-stu-id="03c95-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="03c95-526">Attività 3: esegue la convalida jQuery non intrusivo usando Application</span><span class="sxs-lookup"><span data-stu-id="03c95-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="03c95-527">In questa attività si testerà che il **StoreManager** creare vista modello esegue la convalida lato client usando le librerie di jQuery quando l'utente crea un nuovo album.</span><span class="sxs-lookup"><span data-stu-id="03c95-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="03c95-528">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="03c95-529">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="03c95-529">The project starts in the Home page.</span></span> <span data-ttu-id="03c95-530">Esplorare **StoreManager/creare** e fare clic su **crea** senza compilare il modulo per verificare che i messaggi di convalida:</span><span class="sxs-lookup"><span data-stu-id="03c95-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="03c95-531">![Convalida del client con jQuery abilitata](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "convalida del Client con jQuery abilitata")</span><span class="sxs-lookup"><span data-stu-id="03c95-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="03c95-532">*Convalida del client con jQuery abilitata*</span><span class="sxs-lookup"><span data-stu-id="03c95-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="03c95-533">Nel browser, aprire il codice sorgente per la visualizzazione di creazione:</span><span class="sxs-lookup"><span data-stu-id="03c95-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="03c95-534">Per ogni regola di convalida del client, jQuery Unobtrusive aggiunge un attributo con i dati-- val*rulename*=&quot;*messaggio*&quot;.</span><span class="sxs-lookup"><span data-stu-id="03c95-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="03c95-535">Ecco un elenco di tag che Unobtrusive jQuery viene inserito nel campo di input html per eseguire la convalida del client:</span><span class="sxs-lookup"><span data-stu-id="03c95-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="03c95-536">Dati val</span><span class="sxs-lookup"><span data-stu-id="03c95-536">Data-val</span></span>
   > - <span data-ttu-id="03c95-537">Numero dei dati-val</span><span class="sxs-lookup"><span data-stu-id="03c95-537">Data-val-number</span></span>
   > - <span data-ttu-id="03c95-538">Intervallo di dati val</span><span class="sxs-lookup"><span data-stu-id="03c95-538">Data-val-range</span></span>
   > - <span data-ttu-id="03c95-539">Data-val-intervallo-min / dati-val-intervallo-max</span><span class="sxs-lookup"><span data-stu-id="03c95-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="03c95-540">Dati val necessari</span><span class="sxs-lookup"><span data-stu-id="03c95-540">Data-val-required</span></span>
   > - <span data-ttu-id="03c95-541">I dati a val-lunghezza</span><span class="sxs-lookup"><span data-stu-id="03c95-541">Data-val-length</span></span>
   > - <span data-ttu-id="03c95-542">Data-val-length-max / min-Data-val-lunghezza</span><span class="sxs-lookup"><span data-stu-id="03c95-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="03c95-543">Tutti i valori di data vengono riempiti con modello **annotazione dei dati**.</span><span class="sxs-lookup"><span data-stu-id="03c95-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="03c95-544">Quindi, tutta la logica che funziona sul lato server può essere eseguita sul lato client.</span><span class="sxs-lookup"><span data-stu-id="03c95-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="03c95-545">Ad esempio, prezzo e presenta l'annotazione dei dati seguenti nel modello:</span><span class="sxs-lookup"><span data-stu-id="03c95-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="03c95-546">Dopo l'uso di jQuery Unobtrusive, il codice generato è:</span><span class="sxs-lookup"><span data-stu-id="03c95-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="03c95-547">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="03c95-547">Summary</span></span>

<span data-ttu-id="03c95-548">Completando questa pratica si è appreso come consentire agli utenti di modificare i dati archiviati nel database con l'uso delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="03c95-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="03c95-549">Le azioni del controller, ad esempio Index, Create, Edit, Delete</span><span class="sxs-lookup"><span data-stu-id="03c95-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="03c95-550">Funzionalità di scaffolding di ASP.NET MVC per visualizzare le proprietà in una tabella HTML</span><span class="sxs-lookup"><span data-stu-id="03c95-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="03c95-551">Esperienza di helper HTML personalizzati per migliorare l'utente</span><span class="sxs-lookup"><span data-stu-id="03c95-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="03c95-552">Metodi di azione che reagiscono a HTTP-GET o le chiamate HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="03c95-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="03c95-553">Un modello di editor condivisa per i modelli di vista simile, ad esempio creazione e modifica</span><span class="sxs-lookup"><span data-stu-id="03c95-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="03c95-554">Elementi del form, ad esempio elenchi a discesa</span><span class="sxs-lookup"><span data-stu-id="03c95-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="03c95-555">Annotazioni dei dati per la convalida del modello</span><span class="sxs-lookup"><span data-stu-id="03c95-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="03c95-556">Convalida lato client con jQuery Unobtrusive libreria</span><span class="sxs-lookup"><span data-stu-id="03c95-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="03c95-557">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="03c95-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="03c95-558">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="03c95-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="03c95-559">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="03c95-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="03c95-560">Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="03c95-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="03c95-561">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="03c95-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="03c95-562">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="03c95-562">Click on **Install Now**.</span></span> <span data-ttu-id="03c95-563">Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="03c95-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="03c95-564">Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="03c95-565">![Installa Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="03c95-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="03c95-566">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="03c95-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="03c95-567">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="03c95-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="03c95-569">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="03c95-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="03c95-570">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="03c95-570">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="03c95-572">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="03c95-572">*Installation progress*</span></span>
6. <span data-ttu-id="03c95-573">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="03c95-573">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="03c95-575">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="03c95-575">*Installation completed*</span></span>
7. <span data-ttu-id="03c95-576">Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="03c95-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="03c95-577">Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="03c95-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="03c95-579">*Visual Studio Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="03c95-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="03c95-580">Appendice b: uso dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="03c95-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="03c95-581">Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione.</span><span class="sxs-lookup"><span data-stu-id="03c95-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="03c95-582">Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="03c95-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="03c95-583">![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "frammenti di codice con Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="03c95-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="03c95-584">*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="03c95-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="03c95-585">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="03c95-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="03c95-586">Posizionare il cursore in cui si vuole inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="03c95-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="03c95-587">Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="03c95-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="03c95-588">Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="03c95-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="03c95-589">Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="03c95-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="03c95-590">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="03c95-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="03c95-591">![Iniziare a digitare il nome di frammento](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="03c95-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="03c95-592">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="03c95-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="03c95-593">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "premere Tab per selezionare il frammento di codice evidenziata")</span><span class="sxs-lookup"><span data-stu-id="03c95-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="03c95-594">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="03c95-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="03c95-595">![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "si espanderà il frammento di codice e premere nuovamente Tab")</span><span class="sxs-lookup"><span data-stu-id="03c95-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="03c95-596">*Il frammento di codice e premere nuovamente Tab espanderà*</span><span class="sxs-lookup"><span data-stu-id="03c95-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="03c95-597">***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="03c95-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="03c95-598">Pulsante destro del mouse in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="03c95-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="03c95-599">Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="03c95-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="03c95-600">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="03c95-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="03c95-601">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="03c95-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="03c95-602">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="03c95-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="03c95-603">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")</span><span class="sxs-lookup"><span data-stu-id="03c95-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="03c95-604">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*</span><span class="sxs-lookup"><span data-stu-id="03c95-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
