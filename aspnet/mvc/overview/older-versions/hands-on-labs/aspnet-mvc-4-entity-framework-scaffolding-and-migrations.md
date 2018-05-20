---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Lo Scaffolding di ASP.NET MVC 4 Entity Framework e migrazioni | Documenti Microsoft
author: rick-anderson
description: Se si ha familiarità con i metodi di ASP.NET MVC 4 controller o aver completato il &quot;helper, form e convalida&quot; pratica, è necessario essere consapevoli...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 42a12ee39223a06054382dbe9b4784196a706216
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/18/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="5b168-103">Le migrazioni e lo Scaffolding di ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5b168-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="5b168-104">Da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5b168-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="5b168-105">Download Web categorie Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="5b168-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="5b168-106">Se si ha familiarità con i metodi di ASP.NET MVC 4 controller o aver completato il &quot;helper, form e convalida&quot; pratica, è necessario essere consapevoli che molte della logica per creare, aggiornare, elencare e rimuovere qualsiasi entità di dati viene ripetuta tra l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b168-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="5b168-107">Non dimenticare che, se il modello contiene diverse classi di modificare, sarà probabilmente di un tempo considerevole scrivono i metodi di azione GET e POST per ogni operazione di entità, nonché a ognuna delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="5b168-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="5b168-108">In questo laboratorio si apprenderà come usare lo scaffolding di ASP.NET MVC 4 per generare automaticamente la linea di base del CRUD applicazione (Create, Read, Update e Delete).</span><span class="sxs-lookup"><span data-stu-id="5b168-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="5b168-109">A partire da una classe di modello semplice e, senza scrivere una singola riga di codice, si creerà un controller che conterrà tutte le operazioni CRUD, nonché di tutte le necessarie visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="5b168-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="5b168-110">Dopo aver compilato e l'esecuzione della soluzione semplice, sarà necessario il database dell'applicazione generato, insieme alla logica MVC e viste per la manipolazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="5b168-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="5b168-111">Inoltre, si apprenderà come è facile da utilizzare le migrazioni di Entity Framework per eseguire aggiornamenti di modelli in tutta l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b168-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="5b168-112">Migrazioni di Entity Framework consente di modificare il database dopo il modello è stato modificato con semplici passaggi.</span><span class="sxs-lookup"><span data-stu-id="5b168-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="5b168-113">Tutte queste operazioni in considerazione, sarà in grado di creare e gestire applicazioni web in modo più efficiente, sfruttando le funzionalità più recenti di ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5b168-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="5b168-114">Tutto il codice di esempio e i frammenti di codice inclusi in Web categorie Training Kit, disponibile in [versioni Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="5b168-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="5b168-115">Il progetto specifico per questa esercitazione è disponibile all'indirizzo [Scaffolding di ASP.NET MVC 4 Entity Framework e migrazioni](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="5b168-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5b168-116">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="5b168-116">Objectives</span></span>

<span data-ttu-id="5b168-117">In questa pratica, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="5b168-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="5b168-118">Usare lo scaffolding di ASP.NET per le operazioni CRUD nel controller.</span><span class="sxs-lookup"><span data-stu-id="5b168-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="5b168-119">Modificare il modello di database utilizzando le migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b168-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5b168-120">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5b168-120">Prerequisites</span></span>

<span data-ttu-id="5b168-121">È necessario che gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="5b168-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="5b168-122">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="5b168-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5b168-123">Configurazione</span><span class="sxs-lookup"><span data-stu-id="5b168-123">Setup</span></span>

<span data-ttu-id="5b168-124">**L'installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="5b168-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="5b168-125">Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b168-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="5b168-126">Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="5b168-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="5b168-127">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: tramite i frammenti di codice](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="5b168-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5b168-128">Esercizi</span><span class="sxs-lookup"><span data-stu-id="5b168-128">Exercises</span></span>

<span data-ttu-id="5b168-129">Nell'esercizio seguente che costituiscono questa pratica:</span><span class="sxs-lookup"><span data-stu-id="5b168-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="5b168-130">Utilizzare lo Scaffolding di ASP.NET MVC 4 con le migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5b168-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="5b168-131">Questo esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="5b168-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="5b168-132">Se è necessario aggiuntive sull'utilizzo nell'esercizio, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="5b168-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="5b168-133">Tempo stimato per completare questa esercitazione: **30 minuti**</span><span class="sxs-lookup"><span data-stu-id="5b168-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="5b168-134">Esercizio 1: Utilizzare lo Scaffolding di ASP.NET MVC 4 con le migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5b168-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="5b168-135">Lo scaffolding di ASP.NET MVC fornisce un modo rapido per generare le operazioni CRUD in un modo standardizzato, creare la logica necessaria che consente all'applicazione di interagire con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="5b168-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="5b168-136">In questo esercizio, si apprenderà come usare lo scaffolding di ASP.NET MVC 4 con il codice per creare i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="5b168-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="5b168-137">Si apprenderà quindi come aggiornare il modello di applicazione delle modifiche nel database utilizzando le migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b168-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="5b168-138">Attività 1-Creazione di un nuovo ASP.NET MVC 4 progetto mediante Scaffolding</span><span class="sxs-lookup"><span data-stu-id="5b168-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="5b168-139">Se non è già aperta, avviare **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="5b168-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="5b168-140">Selezionare **File | Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="5b168-140">Select **File | New Project**.</span></span> <span data-ttu-id="5b168-141">Nel nuovo progetto nella finestra di dialogo, il **Visual c# | Web** selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="5b168-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="5b168-142">Denominare il progetto per **MVC4andEFMigrations** e impostare il percorso su **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** cartella di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5b168-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="5b168-143">Impostare il **Nome soluzione** a **iniziare** e verificare **Crea directory per soluzione** è selezionata.</span><span class="sxs-lookup"><span data-stu-id="5b168-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="5b168-144">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b168-144">Click **OK**.</span></span>

    <span data-ttu-id="5b168-145">![Finestra di dialogo Nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "finestra di dialogo Nuovo progetto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="5b168-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="5b168-146">*Finestra di dialogo Nuovo progetto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="5b168-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="5b168-147">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo selezionare il **applicazione Internet** modello e assicurarsi che **Razor** è selezionato **motoredivisualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="5b168-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="5b168-148">Fare clic su **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="5b168-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="5b168-149">![Nuova applicazione Internet di ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nuova applicazione Internet di ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="5b168-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="5b168-150">*Nuova applicazione Internet di ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="5b168-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="5b168-151">In Esplora soluzioni fare doppio clic su **modelli** e selezionare **Aggiungi | Classe** per creare un utente di un classe semplice (POCO).</span><span class="sxs-lookup"><span data-stu-id="5b168-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="5b168-152">Il nome **persona** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b168-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="5b168-153">Aprire la classe di persona e inserire le proprietà seguenti.</span><span class="sxs-lookup"><span data-stu-id="5b168-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="5b168-154">(- Frammento di codice *ASP.NET MVC 4 e le migrazioni di Entity Framework - proprietà persona Ex1*)</span><span class="sxs-lookup"><span data-stu-id="5b168-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="5b168-155">Fare clic su **compilare | Compila soluzione** per salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="5b168-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="5b168-156">![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "compilazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="5b168-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="5b168-157">*Compilazione dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="5b168-157">*Building the Application*</span></span>
7. <span data-ttu-id="5b168-158">In Esplora soluzioni, fare clic sulla cartella controller e selezionare **Aggiungi | Controller**.</span><span class="sxs-lookup"><span data-stu-id="5b168-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="5b168-159">Denominare il controller *PersonController* e completare il **opzioni Scaffolding** con i valori seguenti.</span><span class="sxs-lookup"><span data-stu-id="5b168-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="5b168-160">Nel **modello** elenco a discesa, seleziona il **controller MVC con azioni di lettura/scrittura e viste, mediante Entity Framework** opzione.</span><span class="sxs-lookup"><span data-stu-id="5b168-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="5b168-161">Nel **classe modello** elenco a discesa, seleziona il **persona** classe.</span><span class="sxs-lookup"><span data-stu-id="5b168-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="5b168-162">Nel **classe contesto dati** elenco, selezionare  **&lt;nuovo contesto dati... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="5b168-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="5b168-163">Scegliere qualsiasi nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b168-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="5b168-164">Nel **viste** elenco a discesa elenco, verificare che l'opzione **Razor** è selezionata.</span><span class="sxs-lookup"><span data-stu-id="5b168-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="5b168-165">![Aggiunta del controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "aggiunta del controller di persona con scaffolding")</span><span class="sxs-lookup"><span data-stu-id="5b168-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="5b168-166">*Aggiunta del controller di persona con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5b168-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="5b168-167">Fare clic su **Aggiungi** per creare il nuovo controller di persona con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="5b168-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="5b168-168">Le azioni del controller, nonché le visualizzazioni sono stati generati.</span><span class="sxs-lookup"><span data-stu-id="5b168-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="5b168-169">![Dopo aver creato il controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "dopo aver creato il controller di persona con scaffolding")</span><span class="sxs-lookup"><span data-stu-id="5b168-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="5b168-170">*Dopo aver creato il controller di persona con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5b168-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="5b168-171">Aprire **PersonController** classe.</span><span class="sxs-lookup"><span data-stu-id="5b168-171">Open **PersonController** class.</span></span> <span data-ttu-id="5b168-172">Si noti che i metodi di azione CRUD completi siano stati generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="5b168-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="5b168-173">![All'interno del controller di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controller all'interno di persona")</span><span class="sxs-lookup"><span data-stu-id="5b168-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="5b168-174">*All'interno del controller di persona*</span><span class="sxs-lookup"><span data-stu-id="5b168-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="5b168-175">Attività 2-esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5b168-175">Task 2- Running the application</span></span>

<span data-ttu-id="5b168-176">A questo punto, il database non è ancora creato.</span><span class="sxs-lookup"><span data-stu-id="5b168-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="5b168-177">In questa attività, eseguire l'applicazione per la prima volta e i test verranno le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="5b168-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="5b168-178">Il database verrà creato in modo immediato con Code First.</span><span class="sxs-lookup"><span data-stu-id="5b168-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="5b168-179">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b168-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5b168-180">Nel browser, aggiungere **/Person** all'URL per aprire la pagina di persona.</span><span class="sxs-lookup"><span data-stu-id="5b168-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="5b168-181">![Prima esecuzione applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "prima esecuzione applicazione")</span><span class="sxs-lookup"><span data-stu-id="5b168-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="5b168-182">*Dell'applicazione: eseguire prima*</span><span class="sxs-lookup"><span data-stu-id="5b168-182">*Application: first run*</span></span>
3. <span data-ttu-id="5b168-183">Verrà ora esplorare le pagine di persona e testare le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="5b168-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="5b168-184">Fare clic su **Crea nuovo** per aggiungere una nuova persona.</span><span class="sxs-lookup"><span data-stu-id="5b168-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="5b168-185">Immettere un nome e un cognome e fare clic su **crea**.</span><span class="sxs-lookup"><span data-stu-id="5b168-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="5b168-186">![Aggiunta di una nuova persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "aggiunta una nuova persona")</span><span class="sxs-lookup"><span data-stu-id="5b168-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="5b168-187">*Aggiunta di una nuova persona*</span><span class="sxs-lookup"><span data-stu-id="5b168-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="5b168-188">Nell'elenco della persona, è possibile eliminare, modificare o aggiungere elementi.</span><span class="sxs-lookup"><span data-stu-id="5b168-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="5b168-189">![elenco di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "elenco persona")</span><span class="sxs-lookup"><span data-stu-id="5b168-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="5b168-190">*Elenco di persona*</span><span class="sxs-lookup"><span data-stu-id="5b168-190">*Person list*</span></span>
    3. <span data-ttu-id="5b168-191">Fare clic su **dettagli** per aprire i dettagli della persona.</span><span class="sxs-lookup"><span data-stu-id="5b168-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="5b168-192">![Dettagli della persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "i dettagli dell'utente")</span><span class="sxs-lookup"><span data-stu-id="5b168-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="5b168-193">*Dettagli dell'utente*</span><span class="sxs-lookup"><span data-stu-id="5b168-193">*Person's details*</span></span>
4. <span data-ttu-id="5b168-194">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b168-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="5b168-195">Si noti che sia stato creato l'intero CRUD per l'entità person in tutta l'applicazione - dal modello per le viste di - senza dover scrivere una singola riga di codice.</span><span class="sxs-lookup"><span data-stu-id="5b168-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="5b168-196">Attività 3-aggiornamento il database utilizzando le migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5b168-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="5b168-197">In questa attività si aggiornerà il database utilizzando le migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b168-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="5b168-198">Si noterà quanto sia facile per modificare il modello di applicazione delle modifiche nei database utilizzando la funzionalità di Entity Framework migrazioni.</span><span class="sxs-lookup"><span data-stu-id="5b168-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="5b168-199">Aprire la Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="5b168-199">Open the Package Manager Console.</span></span> <span data-ttu-id="5b168-200">Selezionare **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="5b168-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="5b168-201">Nella Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="5b168-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="5b168-202">PMC</span><span class="sxs-lookup"><span data-stu-id="5b168-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="5b168-203">![Abilitare le migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "abilitazione migrazioni")</span><span class="sxs-lookup"><span data-stu-id="5b168-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="5b168-204">*L'abilitazione delle migrazioni*</span><span class="sxs-lookup"><span data-stu-id="5b168-204">*Enabling migrations*</span></span>

    <span data-ttu-id="5b168-205">Il comando Enable-Migration crea il **migrazioni** cartella che contiene uno script per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="5b168-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="5b168-206">![Cartella Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "cartella Migrations")</span><span class="sxs-lookup"><span data-stu-id="5b168-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="5b168-207">*Cartella Migrations*</span><span class="sxs-lookup"><span data-stu-id="5b168-207">*Migrations folder*</span></span>
3. <span data-ttu-id="5b168-208">Aprire il **Configuration.cs** file nella cartella migrazioni.</span><span class="sxs-lookup"><span data-stu-id="5b168-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="5b168-209">Individuare il costruttore della classe e modificare il **AutomaticMigrationsEnabled** valore *true*.</span><span class="sxs-lookup"><span data-stu-id="5b168-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="5b168-210">Aprire la classe di persona e aggiungere un attributo per il nome della persona intermedio.</span><span class="sxs-lookup"><span data-stu-id="5b168-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="5b168-211">Con questo nuovo attributo, si modifica il modello.</span><span class="sxs-lookup"><span data-stu-id="5b168-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="5b168-212">Selezionare **compilare | Compila soluzione** del menu per compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b168-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="5b168-213">![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "compilazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="5b168-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="5b168-214">*Compilazione dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="5b168-214">*Building the application*</span></span>
6. <span data-ttu-id="5b168-215">Nella Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="5b168-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="5b168-216">PMC</span><span class="sxs-lookup"><span data-stu-id="5b168-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="5b168-217">Questo comando avrà un aspetto per le modifiche negli oggetti dati e quindi, aggiunge i comandi necessari per modificare di conseguenza il database.</span><span class="sxs-lookup"><span data-stu-id="5b168-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="5b168-218">![Aggiunta di un secondo nome](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "aggiunta di un secondo nome")</span><span class="sxs-lookup"><span data-stu-id="5b168-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="5b168-219">*Aggiunta di un secondo nome*</span><span class="sxs-lookup"><span data-stu-id="5b168-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="5b168-220">(Facoltativo) È possibile eseguire il comando seguente per generare uno script SQL con l'aggiornamento differenziale.</span><span class="sxs-lookup"><span data-stu-id="5b168-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="5b168-221">Ciò consente di aggiornare manualmente il database (In questo caso non è necessario), o applicare le modifiche in altri database:</span><span class="sxs-lookup"><span data-stu-id="5b168-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="5b168-222">PMC</span><span class="sxs-lookup"><span data-stu-id="5b168-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="5b168-223">![Generazione di uno script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "la generazione di uno script SQL")</span><span class="sxs-lookup"><span data-stu-id="5b168-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="5b168-224">*Generazione di uno script SQL*</span><span class="sxs-lookup"><span data-stu-id="5b168-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="5b168-225">![Aggiornamento di Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aggiornamento Script SQL")</span><span class="sxs-lookup"><span data-stu-id="5b168-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="5b168-226">*Aggiornamento di Script SQL*</span><span class="sxs-lookup"><span data-stu-id="5b168-226">*SQL Script update*</span></span>
8. <span data-ttu-id="5b168-227">Nella Console di gestione pacchetti, immettere il comando seguente per aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="5b168-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="5b168-228">PMC</span><span class="sxs-lookup"><span data-stu-id="5b168-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="5b168-229">![L'aggiornamento del Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "l'aggiornamento del Database")</span><span class="sxs-lookup"><span data-stu-id="5b168-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="5b168-230">*L'aggiornamento del Database*</span><span class="sxs-lookup"><span data-stu-id="5b168-230">*Updating the Database*</span></span>

    <span data-ttu-id="5b168-231">Verrà aggiunta la **MiddleName** colonna il **persone** tabella corrisponde alla definizione corrente del **persona** classe.</span><span class="sxs-lookup"><span data-stu-id="5b168-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="5b168-232">Una volta che il database viene aggiornato, la cartella del Controller e scegliere **Aggiungi | Controller** per aggiungere nuovamente il controller persona (completamento con gli stessi valori).</span><span class="sxs-lookup"><span data-stu-id="5b168-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="5b168-233">Verranno aggiornate le visualizzazioni aggiungere il nuovo attributo e i metodi esistenti.</span><span class="sxs-lookup"><span data-stu-id="5b168-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="5b168-234">![Aggiunta di un aggiornamento del controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "aggiunta di un aggiornamento del controller")</span><span class="sxs-lookup"><span data-stu-id="5b168-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="5b168-235">*L'aggiornamento del controller*</span><span class="sxs-lookup"><span data-stu-id="5b168-235">*Updating the controller*</span></span>
10. <span data-ttu-id="5b168-236">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5b168-236">Click **Add**.</span></span> <span data-ttu-id="5b168-237">Selezionare quindi i valori **sovrascrivere PersonController.cs** e **sovrascrittura visualizzazioni associate** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b168-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Aggiunta di una sovrascrittura controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="5b168-239">*L'aggiornamento del controller*</span><span class="sxs-lookup"><span data-stu-id="5b168-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="5b168-240">Task4 - esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5b168-240">Task4- Running the application</span></span>

1. <span data-ttu-id="5b168-241">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b168-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5b168-242">Aprire **/Person**.</span><span class="sxs-lookup"><span data-stu-id="5b168-242">Open **/Person**.</span></span> <span data-ttu-id="5b168-243">Si noti che i dati è stati mantenuti, mentre il secondo nome colonna è stata aggiunta.</span><span class="sxs-lookup"><span data-stu-id="5b168-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="5b168-244">![Secondo nome aggiunto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "secondo nome aggiunto")</span><span class="sxs-lookup"><span data-stu-id="5b168-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="5b168-245">*Secondo nome aggiunto*</span><span class="sxs-lookup"><span data-stu-id="5b168-245">*Middle Name added*</span></span>
3. <span data-ttu-id="5b168-246">Se si fa clic **modifica**, sarà possibile aggiungere un secondo nome dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="5b168-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="5b168-247">![Secondo nome edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "secondo nome edition")</span><span class="sxs-lookup"><span data-stu-id="5b168-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5b168-248">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="5b168-248">Summary</span></span>

<span data-ttu-id="5b168-249">In questa esercitazione pratica, si sono appreso semplici passaggi per creare operazioni CRUD con ASP.NET MVC 4 Scaffolding usando qualsiasi classe di modello.</span><span class="sxs-lookup"><span data-stu-id="5b168-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="5b168-250">Quindi, si è appreso come eseguire un aggiornamento end-to-end dell'applicazione - dal database per le viste, utilizzando le migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b168-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="5b168-251">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="5b168-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="5b168-252">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="5b168-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="5b168-253">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="5b168-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="5b168-254">Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="5b168-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="5b168-255">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="5b168-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="5b168-256">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="5b168-256">Click on **Install Now**.</span></span> <span data-ttu-id="5b168-257">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="5b168-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="5b168-258">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="5b168-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="5b168-259">![Installa Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="5b168-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="5b168-260">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="5b168-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="5b168-261">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="5b168-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="5b168-263">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="5b168-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="5b168-264">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="5b168-264">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="5b168-266">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="5b168-266">*Installation progress*</span></span>
6. <span data-ttu-id="5b168-267">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="5b168-267">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="5b168-269">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="5b168-269">*Installation completed*</span></span>
7. <span data-ttu-id="5b168-270">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="5b168-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="5b168-271">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="5b168-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="5b168-273">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="5b168-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="5b168-274">Appendice b: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="5b168-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="5b168-275">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="5b168-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="5b168-276">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="5b168-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="5b168-277">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="5b168-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="5b168-278">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="5b168-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="5b168-279">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="5b168-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="5b168-280">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="5b168-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="5b168-281">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="5b168-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="5b168-282">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="5b168-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="5b168-283">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="5b168-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="5b168-284">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="5b168-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="5b168-285">![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="5b168-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="5b168-286">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="5b168-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="5b168-287">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="5b168-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="5b168-288">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="5b168-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="5b168-289">![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="5b168-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="5b168-290">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="5b168-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="5b168-291">***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="5b168-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="5b168-292">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="5b168-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="5b168-293">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="5b168-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="5b168-294">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="5b168-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="5b168-295">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="5b168-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="5b168-296">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="5b168-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="5b168-297">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="5b168-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="5b168-298">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="5b168-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
