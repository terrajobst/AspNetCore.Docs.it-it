---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Lo Scaffolding di ASP.NET MVC 4 Entity Framework e migrazioni | Documenti Microsoft
author: rick-anderson
description: "Se si ha familiarità con i metodi di ASP.NET MVC 4 controller o aver completato il &quot;helper, form e convalida&quot; pratica, è necessario essere consapevoli..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 15db1589eb90739458b430c35cea38e93e3dec5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="ffab4-103">Le migrazioni e lo Scaffolding di ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ffab4-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>
====================
<span data-ttu-id="ffab4-104">da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="ffab4-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="ffab4-105">Se si ha familiarità con i metodi di ASP.NET MVC 4 controller o aver completato il &quot;helper, form e convalida&quot; pratica, è necessario essere consapevoli che molte della logica per creare, aggiornare, elencare e rimuovere qualsiasi entità di dati viene ripetuta tra l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ffab4-105">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="ffab4-106">Non dimenticare che, se il modello contiene diverse classi di modificare, sarà probabilmente di un tempo considerevole scrivono i metodi di azione GET e POST per ogni operazione di entità, nonché a ognuna delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="ffab4-106">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>
> 
> <span data-ttu-id="ffab4-107">In questo laboratorio si apprenderà come usare lo scaffolding di ASP.NET MVC 4 per generare automaticamente la linea di base del CRUD applicazione (Create, Read, Update e Delete).</span><span class="sxs-lookup"><span data-stu-id="ffab4-107">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="ffab4-108">A partire da una classe di modello semplice e, senza scrivere una singola riga di codice, si creerà un controller che conterrà tutte le operazioni CRUD, nonché di tutte le necessarie visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="ffab4-108">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="ffab4-109">Dopo aver compilato e l'esecuzione della soluzione semplice, sarà necessario il database dell'applicazione generato, insieme alla logica MVC e viste per la manipolazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="ffab4-109">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>
> 
> <span data-ttu-id="ffab4-110">Inoltre, si apprenderà come è facile da utilizzare le migrazioni di Entity Framework per eseguire aggiornamenti di modelli in tutta l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="ffab4-110">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="ffab4-111">Migrazioni di Entity Framework consente di modificare il database dopo il modello è stato modificato con semplici passaggi.</span><span class="sxs-lookup"><span data-stu-id="ffab4-111">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="ffab4-112">Tutte queste operazioni in considerazione, sarà in grado di creare e gestire applicazioni web in modo più efficiente, sfruttando le funzionalità più recenti di ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ffab4-112">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="ffab4-113">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="ffab4-113">Objectives</span></span>

<span data-ttu-id="ffab4-114">In questa pratica, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="ffab4-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="ffab4-115">Usare lo scaffolding di ASP.NET per le operazioni CRUD nel controller.</span><span class="sxs-lookup"><span data-stu-id="ffab4-115">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="ffab4-116">Modificare il modello di database utilizzando le migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ffab4-116">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="ffab4-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ffab4-117">Prerequisites</span></span>

<span data-ttu-id="ffab4-118">È necessario che gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="ffab4-118">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="ffab4-119">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="ffab4-119">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="ffab4-120">Configurazione</span><span class="sxs-lookup"><span data-stu-id="ffab4-120">Setup</span></span>

<span data-ttu-id="ffab4-121">**L'installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="ffab4-121">**Installing Code Snippets**</span></span>

<span data-ttu-id="ffab4-122">Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ffab4-122">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="ffab4-123">Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="ffab4-123">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="ffab4-124">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: tramite i frammenti di codice](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="ffab4-124">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="ffab4-125">Esercizi</span><span class="sxs-lookup"><span data-stu-id="ffab4-125">Exercises</span></span>

<span data-ttu-id="ffab4-126">Nell'esercizio seguente che costituiscono questa pratica:</span><span class="sxs-lookup"><span data-stu-id="ffab4-126">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="ffab4-127">Utilizzare lo Scaffolding di ASP.NET MVC 4 con le migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ffab4-127">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="ffab4-128">Questo esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="ffab4-128">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="ffab4-129">Se è necessario aggiuntive sull'utilizzo nell'esercizio, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="ffab4-129">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="ffab4-130">Tempo stimato per completare questa esercitazione: **30 minuti**</span><span class="sxs-lookup"><span data-stu-id="ffab4-130">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="ffab4-131">Esercizio 1: Utilizzare lo Scaffolding di ASP.NET MVC 4 con le migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ffab4-131">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="ffab4-132">Lo scaffolding di ASP.NET MVC fornisce un modo rapido per generare le operazioni CRUD in un modo standardizzato, creare la logica necessaria che consente all'applicazione di interagire con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="ffab4-132">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="ffab4-133">In questo esercizio, si apprenderà come usare lo scaffolding di ASP.NET MVC 4 con il codice per creare i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="ffab4-133">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="ffab4-134">Si apprenderà quindi come aggiornare il modello di applicazione delle modifiche nel database utilizzando le migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ffab4-134">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="ffab4-135">Attività 1-Creazione di un nuovo ASP.NET MVC 4 progetto mediante Scaffolding</span><span class="sxs-lookup"><span data-stu-id="ffab4-135">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="ffab4-136">Se non è già aperta, avviare **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-136">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="ffab4-137">Selezionare **File | Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-137">Select **File | New Project**.</span></span> <span data-ttu-id="ffab4-138">Nel nuovo progetto nella finestra di dialogo, il **Visual c# | Web** selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-138">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="ffab4-139">Denominare il progetto per **MVC4andEFMigrations** e impostare il percorso su **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** cartella di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ffab4-139">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="ffab4-140">Impostare il **Nome soluzione** a **iniziare** e verificare **Crea directory per soluzione** è selezionata.</span><span class="sxs-lookup"><span data-stu-id="ffab4-140">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="ffab4-141">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-141">Click **OK**.</span></span>

    <span data-ttu-id="ffab4-142">![Finestra di dialogo Nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "finestra di dialogo Nuovo progetto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="ffab4-142">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="ffab4-143">*Finestra di dialogo Nuovo progetto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="ffab4-143">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="ffab4-144">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo selezionare il **applicazione Internet** modello e assicurarsi che **Razor** è selezionato **motoredivisualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-144">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="ffab4-145">Fare clic su **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="ffab4-145">Click **OK** to create the project.</span></span>

    <span data-ttu-id="ffab4-146">![Nuova applicazione Internet di ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nuova applicazione Internet di ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="ffab4-146">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="ffab4-147">*Nuova applicazione Internet di ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="ffab4-147">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="ffab4-148">In Esplora soluzioni fare doppio clic su **modelli** e selezionare **Aggiungi | Classe** per creare un utente di un classe semplice (POCO).</span><span class="sxs-lookup"><span data-stu-id="ffab4-148">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="ffab4-149">Il nome **persona** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-149">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="ffab4-150">Aprire la classe di persona e inserire le proprietà seguenti.</span><span class="sxs-lookup"><span data-stu-id="ffab4-150">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="ffab4-151">(- Frammento di codice *ASP.NET MVC 4 e le migrazioni di Entity Framework - proprietà persona Ex1*)</span><span class="sxs-lookup"><span data-stu-id="ffab4-151">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="ffab4-152">Fare clic su **compilare | Compila soluzione** per salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="ffab4-152">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="ffab4-153">![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "compilazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="ffab4-153">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="ffab4-154">*Compilazione dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="ffab4-154">*Building the Application*</span></span>
7. <span data-ttu-id="ffab4-155">In Esplora soluzioni, fare clic sulla cartella controller e selezionare **Aggiungi | Controller**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-155">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="ffab4-156">Denominare il controller *PersonController* e completare il **opzioni Scaffolding** con i valori seguenti.</span><span class="sxs-lookup"><span data-stu-id="ffab4-156">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

    1. <span data-ttu-id="ffab4-157">Nel **modello** elenco a discesa, seleziona il **controller MVC con azioni di lettura/scrittura e viste, mediante Entity Framework** opzione.</span><span class="sxs-lookup"><span data-stu-id="ffab4-157">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
    2. <span data-ttu-id="ffab4-158">Nel **classe modello** elenco a discesa, seleziona il **persona** classe.</span><span class="sxs-lookup"><span data-stu-id="ffab4-158">In the **Model class** drop-down list, select the **Person** class.</span></span>
    3. <span data-ttu-id="ffab4-159">Nel **classe contesto dati** elenco, selezionare  **&lt;nuovo contesto dati... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-159">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="ffab4-160">Scegliere qualsiasi nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-160">Choose any name and click **OK**.</span></span>
    4. <span data-ttu-id="ffab4-161">Nel **viste** elenco a discesa elenco, verificare che l'opzione **Razor** è selezionata.</span><span class="sxs-lookup"><span data-stu-id="ffab4-161">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

    <span data-ttu-id="ffab4-162">![Aggiunta del controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "aggiunta del controller di persona con scaffolding")</span><span class="sxs-lookup"><span data-stu-id="ffab4-162">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

    <span data-ttu-id="ffab4-163">*Aggiunta del controller di persona con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="ffab4-163">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="ffab4-164">Fare clic su **Aggiungi** per creare il nuovo controller di persona con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="ffab4-164">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="ffab4-165">Le azioni del controller, nonché le visualizzazioni sono stati generati.</span><span class="sxs-lookup"><span data-stu-id="ffab4-165">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="ffab4-166">![Dopo aver creato il controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "dopo aver creato il controller di persona con scaffolding")</span><span class="sxs-lookup"><span data-stu-id="ffab4-166">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="ffab4-167">*Dopo aver creato il controller di persona con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="ffab4-167">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="ffab4-168">Aprire **PersonController** classe.</span><span class="sxs-lookup"><span data-stu-id="ffab4-168">Open **PersonController** class.</span></span> <span data-ttu-id="ffab4-169">Si noti che i metodi di azione CRUD completi siano stati generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ffab4-169">Notice that the full CRUD action methods have been generated automatically.</span></span>

    <span data-ttu-id="ffab4-170">![All'interno del controller di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controller all'interno di persona")</span><span class="sxs-lookup"><span data-stu-id="ffab4-170">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

    <span data-ttu-id="ffab4-171">*All'interno del controller di persona*</span><span class="sxs-lookup"><span data-stu-id="ffab4-171">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="ffab4-172">Attività 2-esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="ffab4-172">Task 2- Running the application</span></span>

<span data-ttu-id="ffab4-173">A questo punto, il database non è ancora creato.</span><span class="sxs-lookup"><span data-stu-id="ffab4-173">At this point, the database is not yet created.</span></span> <span data-ttu-id="ffab4-174">In questa attività, eseguire l'applicazione per la prima volta e i test verranno le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="ffab4-174">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="ffab4-175">Il database verrà creato in modo immediato con Code First.</span><span class="sxs-lookup"><span data-stu-id="ffab4-175">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="ffab4-176">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ffab4-176">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="ffab4-177">Nel browser, aggiungere **/Person** all'URL per aprire la pagina di persona.</span><span class="sxs-lookup"><span data-stu-id="ffab4-177">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="ffab4-178">![Prima esecuzione applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "prima esecuzione applicazione")</span><span class="sxs-lookup"><span data-stu-id="ffab4-178">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="ffab4-179">*Dell'applicazione: eseguire prima*</span><span class="sxs-lookup"><span data-stu-id="ffab4-179">*Application: first run*</span></span>
3. <span data-ttu-id="ffab4-180">Verrà ora esplorare le pagine di persona e testare le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="ffab4-180">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="ffab4-181">Fare clic su **Crea nuovo** per aggiungere una nuova persona.</span><span class="sxs-lookup"><span data-stu-id="ffab4-181">Click **Create New** to add a new person.</span></span> <span data-ttu-id="ffab4-182">Immettere un nome e un cognome e fare clic su **crea**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-182">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="ffab4-183">![Aggiunta di una nuova persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "aggiunta una nuova persona")</span><span class="sxs-lookup"><span data-stu-id="ffab4-183">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="ffab4-184">*Aggiunta di una nuova persona*</span><span class="sxs-lookup"><span data-stu-id="ffab4-184">*Adding a new person*</span></span>
    2. <span data-ttu-id="ffab4-185">Nell'elenco della persona, è possibile eliminare, modificare o aggiungere elementi.</span><span class="sxs-lookup"><span data-stu-id="ffab4-185">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="ffab4-186">![elenco di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "elenco persona")</span><span class="sxs-lookup"><span data-stu-id="ffab4-186">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="ffab4-187">*Elenco di persona*</span><span class="sxs-lookup"><span data-stu-id="ffab4-187">*Person list*</span></span>
    3. <span data-ttu-id="ffab4-188">Fare clic su **dettagli** per aprire i dettagli della persona.</span><span class="sxs-lookup"><span data-stu-id="ffab4-188">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="ffab4-189">![Dettagli della persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "i dettagli dell'utente")</span><span class="sxs-lookup"><span data-stu-id="ffab4-189">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="ffab4-190">*Dettagli dell'utente*</span><span class="sxs-lookup"><span data-stu-id="ffab4-190">*Person's details*</span></span>
4. <span data-ttu-id="ffab4-191">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ffab4-191">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="ffab4-192">Si noti che sia stato creato l'intero CRUD per l'entità person in tutta l'applicazione - dal modello per le viste di - senza dover scrivere una singola riga di codice.</span><span class="sxs-lookup"><span data-stu-id="ffab4-192">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="ffab4-193">Attività 3-aggiornamento il database utilizzando le migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ffab4-193">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="ffab4-194">In questa attività si aggiornerà il database utilizzando le migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ffab4-194">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="ffab4-195">Si noterà quanto sia facile per modificare il modello di applicazione delle modifiche nei database utilizzando la funzionalità di Entity Framework migrazioni.</span><span class="sxs-lookup"><span data-stu-id="ffab4-195">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="ffab4-196">Aprire la Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="ffab4-196">Open the Package Manager Console.</span></span> <span data-ttu-id="ffab4-197">Selezionare **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-197">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="ffab4-198">Nella Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ffab4-198">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="ffab4-199">PMC</span><span class="sxs-lookup"><span data-stu-id="ffab4-199">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="ffab4-200">![Abilitare le migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "abilitazione migrazioni")</span><span class="sxs-lookup"><span data-stu-id="ffab4-200">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="ffab4-201">*L'abilitazione delle migrazioni*</span><span class="sxs-lookup"><span data-stu-id="ffab4-201">*Enabling migrations*</span></span>

    <span data-ttu-id="ffab4-202">Il comando Enable-Migration crea il **migrazioni** cartella che contiene uno script per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="ffab4-202">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="ffab4-203">![Cartella Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "cartella Migrations")</span><span class="sxs-lookup"><span data-stu-id="ffab4-203">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="ffab4-204">*Cartella Migrations*</span><span class="sxs-lookup"><span data-stu-id="ffab4-204">*Migrations folder*</span></span>
3. <span data-ttu-id="ffab4-205">Aprire il **Configuration.cs** file nella cartella migrazioni.</span><span class="sxs-lookup"><span data-stu-id="ffab4-205">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="ffab4-206">Individuare il costruttore della classe e modificare il **AutomaticMigrationsEnabled** valore *true*.</span><span class="sxs-lookup"><span data-stu-id="ffab4-206">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="ffab4-207">Aprire la classe di persona e aggiungere un attributo per il nome della persona intermedio.</span><span class="sxs-lookup"><span data-stu-id="ffab4-207">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="ffab4-208">Con questo nuovo attributo, si modifica il modello.</span><span class="sxs-lookup"><span data-stu-id="ffab4-208">With this new attribute, you are changing the model.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="ffab4-209">Selezionare **compilare | Compila soluzione** del menu per compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ffab4-209">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="ffab4-210">![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "compilazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="ffab4-210">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="ffab4-211">*Compilazione dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="ffab4-211">*Building the application*</span></span>
6. <span data-ttu-id="ffab4-212">Nella Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ffab4-212">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="ffab4-213">PMC</span><span class="sxs-lookup"><span data-stu-id="ffab4-213">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="ffab4-214">Questo comando avrà un aspetto per le modifiche negli oggetti dati e quindi, aggiunge i comandi necessari per modificare di conseguenza il database.</span><span class="sxs-lookup"><span data-stu-id="ffab4-214">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="ffab4-215">![Aggiunta di un secondo nome](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "aggiunta di un secondo nome")</span><span class="sxs-lookup"><span data-stu-id="ffab4-215">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="ffab4-216">*Aggiunta di un secondo nome*</span><span class="sxs-lookup"><span data-stu-id="ffab4-216">*Adding a middle name*</span></span>
7. <span data-ttu-id="ffab4-217">(Facoltativo) È possibile eseguire il comando seguente per generare uno script SQL con l'aggiornamento differenziale.</span><span class="sxs-lookup"><span data-stu-id="ffab4-217">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="ffab4-218">Ciò consente di aggiornare manualmente il database (In questo caso non è necessario), o applicare le modifiche in altri database:</span><span class="sxs-lookup"><span data-stu-id="ffab4-218">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="ffab4-219">PMC</span><span class="sxs-lookup"><span data-stu-id="ffab4-219">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="ffab4-220">![Generazione di uno script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "la generazione di uno script SQL")</span><span class="sxs-lookup"><span data-stu-id="ffab4-220">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="ffab4-221">*Generazione di uno script SQL*</span><span class="sxs-lookup"><span data-stu-id="ffab4-221">*Generating a SQL script*</span></span>

    <span data-ttu-id="ffab4-222">![Aggiornamento di Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aggiornamento Script SQL")</span><span class="sxs-lookup"><span data-stu-id="ffab4-222">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="ffab4-223">*Aggiornamento di Script SQL*</span><span class="sxs-lookup"><span data-stu-id="ffab4-223">*SQL Script update*</span></span>
8. <span data-ttu-id="ffab4-224">Nella Console di gestione pacchetti, immettere il comando seguente per aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="ffab4-224">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="ffab4-225">PMC</span><span class="sxs-lookup"><span data-stu-id="ffab4-225">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="ffab4-226">![L'aggiornamento del Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "l'aggiornamento del Database")</span><span class="sxs-lookup"><span data-stu-id="ffab4-226">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="ffab4-227">*L'aggiornamento del Database*</span><span class="sxs-lookup"><span data-stu-id="ffab4-227">*Updating the Database*</span></span>

    <span data-ttu-id="ffab4-228">Verrà aggiunta la **MiddleName** colonna il **persone** tabella corrisponde alla definizione corrente del **persona** classe.</span><span class="sxs-lookup"><span data-stu-id="ffab4-228">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="ffab4-229">Una volta che il database viene aggiornato, la cartella del Controller e scegliere **Aggiungi | Controller** per aggiungere nuovamente il controller persona (completamento con gli stessi valori).</span><span class="sxs-lookup"><span data-stu-id="ffab4-229">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="ffab4-230">Verranno aggiornate le visualizzazioni aggiungere il nuovo attributo e i metodi esistenti.</span><span class="sxs-lookup"><span data-stu-id="ffab4-230">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="ffab4-231">![Aggiunta di un aggiornamento del controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "aggiunta di un aggiornamento del controller")</span><span class="sxs-lookup"><span data-stu-id="ffab4-231">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="ffab4-232">*L'aggiornamento del controller*</span><span class="sxs-lookup"><span data-stu-id="ffab4-232">*Updating the controller*</span></span>
10. <span data-ttu-id="ffab4-233">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-233">Click **Add**.</span></span> <span data-ttu-id="ffab4-234">Selezionare quindi i valori **sovrascrivere PersonController.cs** e **sovrascrittura visualizzazioni associate** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-234">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

    ![Aggiunta di una sovrascrittura controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    <span data-ttu-id="ffab4-236">*L'aggiornamento del controller*</span><span class="sxs-lookup"><span data-stu-id="ffab4-236">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="ffab4-237">Task4 - esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="ffab4-237">Task4- Running the application</span></span>

1. <span data-ttu-id="ffab4-238">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ffab4-238">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="ffab4-239">Aprire **/Person**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-239">Open **/Person**.</span></span> <span data-ttu-id="ffab4-240">Si noti che i dati è stati mantenuti, mentre il secondo nome colonna è stata aggiunta.</span><span class="sxs-lookup"><span data-stu-id="ffab4-240">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="ffab4-241">![Secondo nome aggiunto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "secondo nome aggiunto")</span><span class="sxs-lookup"><span data-stu-id="ffab4-241">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="ffab4-242">*Secondo nome aggiunto*</span><span class="sxs-lookup"><span data-stu-id="ffab4-242">*Middle Name added*</span></span>
3. <span data-ttu-id="ffab4-243">Se si fa clic **modifica**, sarà possibile aggiungere un secondo nome dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="ffab4-243">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="ffab4-244">![Secondo nome edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "secondo nome edition")</span><span class="sxs-lookup"><span data-stu-id="ffab4-244">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="ffab4-245">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="ffab4-245">Summary</span></span>

<span data-ttu-id="ffab4-246">In questa esercitazione pratica, si sono appreso semplici passaggi per creare operazioni CRUD con ASP.NET MVC 4 Scaffolding usando qualsiasi classe di modello.</span><span class="sxs-lookup"><span data-stu-id="ffab4-246">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="ffab4-247">Quindi, si è appreso come eseguire un aggiornamento end-to-end dell'applicazione - dal database per le viste, utilizzando le migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ffab4-247">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="ffab4-248">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="ffab4-248">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="ffab4-249">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="ffab4-249">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="ffab4-250">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="ffab4-250">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="ffab4-251">Passare a [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="ffab4-251">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="ffab4-252">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="ffab4-252">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="ffab4-253">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-253">Click on **Install Now**.</span></span> <span data-ttu-id="ffab4-254">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="ffab4-254">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="ffab4-255">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="ffab4-255">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="ffab4-256">![Installa Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="ffab4-256">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="ffab4-257">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="ffab4-257">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="ffab4-258">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="ffab4-258">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="ffab4-260">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="ffab4-260">*Accepting the license terms*</span></span>
5. <span data-ttu-id="ffab4-261">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="ffab4-261">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="ffab4-263">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="ffab4-263">*Installation progress*</span></span>
6. <span data-ttu-id="ffab4-264">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-264">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="ffab4-266">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="ffab4-266">*Installation completed*</span></span>
7. <span data-ttu-id="ffab4-267">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="ffab4-267">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="ffab4-268">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="ffab4-268">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="ffab4-270">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="ffab4-270">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="ffab4-271">Appendice b: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="ffab4-271">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="ffab4-272">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="ffab4-272">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="ffab4-273">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="ffab4-273">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="ffab4-274">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="ffab4-274">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="ffab4-275">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="ffab4-275">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="ffab4-276">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="ffab4-276">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="ffab4-277">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="ffab4-277">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="ffab4-278">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="ffab4-278">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="ffab4-279">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="ffab4-279">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="ffab4-280">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="ffab4-280">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="ffab4-281">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="ffab4-281">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="ffab4-282">![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="ffab4-282">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="ffab4-283">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="ffab4-283">*Start typing the snippet name*</span></span>

<span data-ttu-id="ffab4-284">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="ffab4-284">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="ffab4-285">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="ffab4-285">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="ffab4-286">![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="ffab4-286">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="ffab4-287">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="ffab4-287">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="ffab4-288">***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="ffab4-288">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="ffab4-289">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="ffab4-289">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="ffab4-290">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="ffab4-290">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="ffab4-291">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="ffab4-291">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="ffab4-292">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="ffab4-292">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="ffab4-293">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="ffab4-293">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="ffab4-294">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="ffab4-294">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="ffab4-295">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="ffab4-295">*Pick the relevant snippet from the list, by clicking on it*</span></span>
