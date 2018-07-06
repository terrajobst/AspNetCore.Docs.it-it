---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Scaffolding e migrazioni | Microsoft Docs
author: rick-anderson
description: Se ha familiarità con i metodi del controller ASP.NET MVC 4 oppure ha completato il &quot;helper, moduli e convalida&quot; laboratorio pratico, è necessario essere consapevoli...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814122"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="e8c58-103">Le migrazioni e lo Scaffolding di ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e8c58-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="e8c58-104">da [Camp Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e8c58-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e8c58-105">Download Web Camp Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="e8c58-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="e8c58-106">Se ha familiarità con i metodi del controller ASP.NET MVC 4 oppure ha completato il &quot;helper, moduli e convalida&quot; laboratorio pratico, è necessario essere consapevoli che molti della logica per creare, aggiornare, elencare e rimuovere qualsiasi entità di dati viene ripetuta tra l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="e8c58-107">Senza dimenticare che, se il modello contiene diverse classi per modificare, è probabile che un certo tempo a scrivere i metodi di azione POST e GET per ogni operazione di entità, nonché a ogni visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="e8c58-108">In questo laboratorio si apprenderà come usare lo scaffolding di ASP.NET MVC 4 per generare automaticamente la linea di base di CRUD dell'applicazione (Create, Read, Update e Delete).</span><span class="sxs-lookup"><span data-stu-id="e8c58-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="e8c58-109">A partire da una classe di modello semplice e, senza scrivere una singola riga di codice, si creerà un controller che conterrà tutte le operazioni CRUD, nonché di tutte le necessarie visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="e8c58-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="e8c58-110">Dopo aver compilato ed eseguito la soluzione più semplice, si disporrà del database dell'applicazione generato, insieme alla logica di MVC e le viste per la manipolazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="e8c58-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="e8c58-111">Inoltre, si apprenderà come è facile usare migrazioni di Entity Framework per eseguire aggiornamenti di modelli in tutta l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="e8c58-112">Migrazioni di Entity Framework consente di modificare il database dopo il modello è stato modificato con semplici passaggi.</span><span class="sxs-lookup"><span data-stu-id="e8c58-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="e8c58-113">Con tutti questi aspetti, sarà possibile creare e gestire le applicazioni web in modo più efficiente, sfruttando i vantaggi delle funzionalità più recenti di ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e8c58-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="e8c58-114">Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile in [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e8c58-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="e8c58-115">Il progetto specifico per questo lab è disponibile all'indirizzo [Scaffolding di ASP.NET MVC 4 Entity Framework e migrazioni](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="e8c58-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e8c58-116">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="e8c58-116">Objectives</span></span>

<span data-ttu-id="e8c58-117">In questo laboratorio pratico, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="e8c58-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="e8c58-118">Usare lo scaffolding di ASP.NET per le operazioni CRUD nei controller.</span><span class="sxs-lookup"><span data-stu-id="e8c58-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="e8c58-119">Modificare il modello di database tramite migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8c58-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e8c58-120">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e8c58-120">Prerequisites</span></span>

<span data-ttu-id="e8c58-121">Sono necessari gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="e8c58-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e8c58-122">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice A](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="e8c58-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e8c58-123">Configurazione</span><span class="sxs-lookup"><span data-stu-id="e8c58-123">Setup</span></span>

<span data-ttu-id="e8c58-124">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="e8c58-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="e8c58-125">Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8c58-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e8c58-126">Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="e8c58-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e8c58-127">Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: Using Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e8c58-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e8c58-128">Esercizi</span><span class="sxs-lookup"><span data-stu-id="e8c58-128">Exercises</span></span>

<span data-ttu-id="e8c58-129">L'esercizio seguente che costituiscono questo laboratorio pratico:</span><span class="sxs-lookup"><span data-stu-id="e8c58-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="e8c58-130">Usare lo Scaffolding di ASP.NET MVC 4 con migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e8c58-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="e8c58-131">Questo esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="e8c58-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="e8c58-132">Se ti serve assistenza aggiuntiva, utilizzo l'esercizio, è possibile usare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="e8c58-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="e8c58-133">Tempo stimato per completare questa esercitazione: **30 minuti**</span><span class="sxs-lookup"><span data-stu-id="e8c58-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="e8c58-134">Esercizio 1: Usare lo Scaffolding di ASP.NET MVC 4 con migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e8c58-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="e8c58-135">Scaffolding di MVC ASP.NET fornisce un modo rapido per generare le operazioni CRUD in modo standardizzato, creare la logica necessaria che consente all'applicazione di interagire con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="e8c58-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="e8c58-136">In questo esercizio, si apprenderà come usare lo scaffolding di ASP.NET MVC 4 con il codice prima di tutto per creare i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="e8c58-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="e8c58-137">Quindi, si apprenderà come aggiornare il modello di applicazione delle modifiche nel database tramite migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8c58-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="e8c58-138">Attività 1-Creazione in corso un nuovo ASP.NET MVC 4 progetto usando lo Scaffolding</span><span class="sxs-lookup"><span data-stu-id="e8c58-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="e8c58-139">Se non è già aperta, avviare **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="e8c58-140">Selezionare **File | Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-140">Select **File | New Project**.</span></span> <span data-ttu-id="e8c58-141">Nella nuova finestra di dialogo progetto, sotto il **Visual c# | Web** sezione, selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e8c58-142">Denominare il progetto di **MVC4andEFMigrations** e impostare il percorso al **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** cartella della presente esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="e8c58-143">Impostare il **Nome soluzione** a **Begin** e verificare che **Crea directory per soluzione** sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="e8c58-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="e8c58-144">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-144">Click **OK**.</span></span>

    <span data-ttu-id="e8c58-145">![Finestra di dialogo Nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "finestra di dialogo Nuovo progetto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="e8c58-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="e8c58-146">*Finestra di dialogo Nuovo progetto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="e8c58-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="e8c58-147">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo per selezionare la **applicazione Internet** modello e verificare che **Razor** è selezionato **motoredivisualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="e8c58-148">Fare clic su **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="e8c58-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="e8c58-149">![Nuova applicazione Internet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nuova applicazione Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="e8c58-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="e8c58-150">*Nuova applicazione Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="e8c58-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="e8c58-151">In Esplora soluzioni fare doppio clic **modelli** e selezionare **Add | Classe** per creare un utente di classi semplice (POCO).</span><span class="sxs-lookup"><span data-stu-id="e8c58-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="e8c58-152">Denominarlo **Person** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="e8c58-153">Aprire la classe Person e inserire le proprietà seguenti.</span><span class="sxs-lookup"><span data-stu-id="e8c58-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="e8c58-154">(Code - Snippet *ASP.NET MVC 4 e migrazioni di Entity Framework - proprietà persona Ex1*)</span><span class="sxs-lookup"><span data-stu-id="e8c58-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="e8c58-155">Fare clic su **compilare | Compila soluzione** per salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="e8c58-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="e8c58-156">![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "compilazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="e8c58-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="e8c58-157">*Compilazione dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="e8c58-157">*Building the Application*</span></span>
7. <span data-ttu-id="e8c58-158">In Esplora soluzioni, fare clic sulla cartella controller e selezionare **Add | Controller**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="e8c58-159">Denominare il controller *PersonController* e completare la **opzioni di Scaffolding** con i valori seguenti.</span><span class="sxs-lookup"><span data-stu-id="e8c58-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="e8c58-160">Nel **modello** elenco a discesa, seleziona la **controller MVC con azioni di lettura/scrittura e visualizzazioni, usando Entity Framework** opzione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="e8c58-161">Nel **classe modello** elenco a discesa, seleziona la **persona** classe.</span><span class="sxs-lookup"><span data-stu-id="e8c58-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="e8c58-162">Nel **alla classe contesto dati** elenco, selezionare  **&lt;nuovo contesto dati... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="e8c58-163">Scegliere qualsiasi nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="e8c58-164">Nel **viste** elenco a discesa elenco, assicurarsi che **Razor** sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="e8c58-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="e8c58-165">![Aggiunta di controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "aggiungendo il controller di persona con scaffolding")</span><span class="sxs-lookup"><span data-stu-id="e8c58-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="e8c58-166">*Aggiunta di controller di persona con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="e8c58-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="e8c58-167">Fare clic su **Add** per creare il nuovo controller per persona con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="e8c58-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="e8c58-168">Le azioni del controller, nonché le visualizzazioni sono stati generati.</span><span class="sxs-lookup"><span data-stu-id="e8c58-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="e8c58-169">![Dopo aver creato il controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "dopo aver creato il controller di persona con scaffolding")</span><span class="sxs-lookup"><span data-stu-id="e8c58-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="e8c58-170">*Dopo aver creato il controller di persona con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="e8c58-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="e8c58-171">Aprire **PersonController** classe.</span><span class="sxs-lookup"><span data-stu-id="e8c58-171">Open **PersonController** class.</span></span> <span data-ttu-id="e8c58-172">Si noti che i metodi di azione CRUD completi siano stati generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e8c58-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="e8c58-173">![All'interno del controller di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controller all'interno di persona")</span><span class="sxs-lookup"><span data-stu-id="e8c58-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="e8c58-174">*All'interno del controller di persona*</span><span class="sxs-lookup"><span data-stu-id="e8c58-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="e8c58-175">Attività 2-esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e8c58-175">Task 2- Running the application</span></span>

<span data-ttu-id="e8c58-176">A questo punto, il database non è ancora creato.</span><span class="sxs-lookup"><span data-stu-id="e8c58-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="e8c58-177">In questa attività, eseguire l'applicazione per la prima volta e i test verranno le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="e8c58-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="e8c58-178">Il database verrà creato in tempo reale con Code First.</span><span class="sxs-lookup"><span data-stu-id="e8c58-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="e8c58-179">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e8c58-180">Nel browser, aggiungere **/Person** all'URL per aprire la pagina di persona.</span><span class="sxs-lookup"><span data-stu-id="e8c58-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="e8c58-181">![Prima dell'esecuzione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "prima dell'esecuzione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="e8c58-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="e8c58-182">*Dell'applicazione: eseguire prima*</span><span class="sxs-lookup"><span data-stu-id="e8c58-182">*Application: first run*</span></span>
3. <span data-ttu-id="e8c58-183">Verrà ora esplorare le pagine di persona e testare le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="e8c58-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="e8c58-184">Fare clic su **Crea nuovo** per aggiungere una nuova persona.</span><span class="sxs-lookup"><span data-stu-id="e8c58-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="e8c58-185">Immettere un nome e un cognome e fare clic su **Create**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="e8c58-186">![Aggiunta di una nuova persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "aggiunta una nuova persona")</span><span class="sxs-lookup"><span data-stu-id="e8c58-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="e8c58-187">*Aggiunta di una nuova persona*</span><span class="sxs-lookup"><span data-stu-id="e8c58-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="e8c58-188">Nell'elenco di una persona, è possibile eliminare, modificare o aggiungere elementi.</span><span class="sxs-lookup"><span data-stu-id="e8c58-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="e8c58-189">![elenco di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "elenco persone")</span><span class="sxs-lookup"><span data-stu-id="e8c58-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="e8c58-190">*Elenco di persona*</span><span class="sxs-lookup"><span data-stu-id="e8c58-190">*Person list*</span></span>
    3. <span data-ttu-id="e8c58-191">Fare clic su **dettagli** per aprire i dettagli di una persona.</span><span class="sxs-lookup"><span data-stu-id="e8c58-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="e8c58-192">![I dettagli della persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "i dettagli della persona")</span><span class="sxs-lookup"><span data-stu-id="e8c58-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="e8c58-193">*Dettagli della persona*</span><span class="sxs-lookup"><span data-stu-id="e8c58-193">*Person's details*</span></span>
4. <span data-ttu-id="e8c58-194">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8c58-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="e8c58-195">Si noti che sia stato creato l'intera CRUD per l'entità person in tutta l'applicazione - dal modello per le visualizzazioni - senza dover scrivere una singola riga di codice!</span><span class="sxs-lookup"><span data-stu-id="e8c58-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="e8c58-196">Attività 3: aggiornamento di database tramite migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e8c58-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="e8c58-197">In questa attività si aggiornerà il database tramite migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8c58-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="e8c58-198">Si scoprirà quanto è facile modificare il modello e riflettere le modifiche nei database utilizzando la funzionalità migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8c58-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="e8c58-199">Aprire la Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="e8c58-199">Open the Package Manager Console.</span></span> <span data-ttu-id="e8c58-200">Selezionare **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="e8c58-201">Nella Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e8c58-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="e8c58-202">PMC</span><span class="sxs-lookup"><span data-stu-id="e8c58-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="e8c58-203">![Abilitare migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "consentendo migrazioni")</span><span class="sxs-lookup"><span data-stu-id="e8c58-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="e8c58-204">*Abilitare migrazioni*</span><span class="sxs-lookup"><span data-stu-id="e8c58-204">*Enabling migrations*</span></span>

    <span data-ttu-id="e8c58-205">Il comando Enable-migrazione crea il **migrazioni** cartella che contiene uno script per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="e8c58-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="e8c58-206">![Cartella migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "cartella migrazioni")</span><span class="sxs-lookup"><span data-stu-id="e8c58-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="e8c58-207">*Cartella migrazioni*</span><span class="sxs-lookup"><span data-stu-id="e8c58-207">*Migrations folder*</span></span>
3. <span data-ttu-id="e8c58-208">Aprire il **Configuration.cs** file nella cartella migrazioni.</span><span class="sxs-lookup"><span data-stu-id="e8c58-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="e8c58-209">Individuare il costruttore della classe e modificare il **AutomaticMigrationsEnabled** valore *true*.</span><span class="sxs-lookup"><span data-stu-id="e8c58-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="e8c58-210">Aprire la classe Person e aggiungere un attributo per il nome della persona intermedio.</span><span class="sxs-lookup"><span data-stu-id="e8c58-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="e8c58-211">Con questo nuovo attributo, si modifica il modello.</span><span class="sxs-lookup"><span data-stu-id="e8c58-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="e8c58-212">Selezionare **compilare | Compila soluzione** nel menu per compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="e8c58-213">![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "compilazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="e8c58-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="e8c58-214">*Compilazione dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="e8c58-214">*Building the application*</span></span>
6. <span data-ttu-id="e8c58-215">Nella Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e8c58-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="e8c58-216">PMC</span><span class="sxs-lookup"><span data-stu-id="e8c58-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="e8c58-217">Questo comando Cerca modifiche agli oggetti dati e quindi, aggiunge i comandi necessari per modificare di conseguenza il database.</span><span class="sxs-lookup"><span data-stu-id="e8c58-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="e8c58-218">![Aggiunta di un nome intermedio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "aggiungendo un secondo nome")</span><span class="sxs-lookup"><span data-stu-id="e8c58-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="e8c58-219">*Aggiunta di un secondo nome*</span><span class="sxs-lookup"><span data-stu-id="e8c58-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="e8c58-220">(Facoltativo) È possibile eseguire il comando seguente per generare uno script SQL con l'aggiornamento di backup differenziale.</span><span class="sxs-lookup"><span data-stu-id="e8c58-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="e8c58-221">Questo modo sarà possibile aggiornare manualmente il database (In questo caso non è necessario), o applicare le modifiche in altri database:</span><span class="sxs-lookup"><span data-stu-id="e8c58-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="e8c58-222">PMC</span><span class="sxs-lookup"><span data-stu-id="e8c58-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="e8c58-223">![Generazione di uno script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generazione di uno script SQL")</span><span class="sxs-lookup"><span data-stu-id="e8c58-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="e8c58-224">*Generazione di uno script SQL*</span><span class="sxs-lookup"><span data-stu-id="e8c58-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="e8c58-225">![Aggiornamento di Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aggiornamento Script SQL")</span><span class="sxs-lookup"><span data-stu-id="e8c58-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="e8c58-226">*Aggiornamento di Script SQL*</span><span class="sxs-lookup"><span data-stu-id="e8c58-226">*SQL Script update*</span></span>
8. <span data-ttu-id="e8c58-227">Nella Console di gestione pacchetti immettere il comando seguente per aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="e8c58-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="e8c58-228">PMC</span><span class="sxs-lookup"><span data-stu-id="e8c58-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="e8c58-229">![L'aggiornamento del Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "l'aggiornamento del Database")</span><span class="sxs-lookup"><span data-stu-id="e8c58-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="e8c58-230">*L'aggiornamento del Database*</span><span class="sxs-lookup"><span data-stu-id="e8c58-230">*Updating the Database*</span></span>

    <span data-ttu-id="e8c58-231">Verrà aggiunta la **MiddleName** colonna il **persone** tabella corrisponde alla definizione corrente del **persona** classe.</span><span class="sxs-lookup"><span data-stu-id="e8c58-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="e8c58-232">Dopo aver aggiornato il database, fare doppio clic nella cartella Controller e selezionare **Add | Controller** per aggiungere nuovamente il controller persona (completa con gli stessi valori).</span><span class="sxs-lookup"><span data-stu-id="e8c58-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="e8c58-233">Verranno aggiornate le visualizzazioni aggiunta del nuovo attributo e i metodi esistenti.</span><span class="sxs-lookup"><span data-stu-id="e8c58-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="e8c58-234">![Aggiunta di un aggiornamento del controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "aggiunta di un aggiornamento del controller")</span><span class="sxs-lookup"><span data-stu-id="e8c58-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="e8c58-235">*L'aggiornamento del controller*</span><span class="sxs-lookup"><span data-stu-id="e8c58-235">*Updating the controller*</span></span>
10. <span data-ttu-id="e8c58-236">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-236">Click **Add**.</span></span> <span data-ttu-id="e8c58-237">Quindi, selezionare i valori **sovrascrivere PersonController.cs** e il **Sovrascrivi visualizzazioni associate** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Aggiunta di una sovrascrittura controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="e8c58-239">*L'aggiornamento del controller*</span><span class="sxs-lookup"><span data-stu-id="e8c58-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="e8c58-240">Task4 - esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e8c58-240">Task4- Running the application</span></span>

1. <span data-ttu-id="e8c58-241">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e8c58-242">Aprire **/Person**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-242">Open **/Person**.</span></span> <span data-ttu-id="e8c58-243">Si noti che i dati è stati mantenuti, mentre il secondo nome colonna è stata aggiunta.</span><span class="sxs-lookup"><span data-stu-id="e8c58-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="e8c58-244">![Secondo nome aggiunti](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name aggiunto")</span><span class="sxs-lookup"><span data-stu-id="e8c58-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="e8c58-245">*Secondo nome aggiunto*</span><span class="sxs-lookup"><span data-stu-id="e8c58-245">*Middle Name added*</span></span>
3. <span data-ttu-id="e8c58-246">Se si sceglie **modifica**, sarà possibile aggiungere un secondo nome per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="e8c58-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="e8c58-247">![Secondo nome edizione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "secondo nome edizione")</span><span class="sxs-lookup"><span data-stu-id="e8c58-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e8c58-248">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e8c58-248">Summary</span></span>

<span data-ttu-id="e8c58-249">In questo laboratorio pratico, si sono appreso semplici passaggi per creare operazioni CRUD con ASP.NET MVC 4 Scaffolding usando qualsiasi classe di modello.</span><span class="sxs-lookup"><span data-stu-id="e8c58-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="e8c58-250">Quindi, si è appreso come eseguire un aggiornamento to end nell'applicazione - dal database per le viste - mediante migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8c58-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e8c58-251">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="e8c58-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e8c58-252">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e8c58-253">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="e8c58-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e8c58-254">Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e8c58-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e8c58-255">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="e8c58-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="e8c58-256">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-256">Click on **Install Now**.</span></span> <span data-ttu-id="e8c58-257">Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="e8c58-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e8c58-258">Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e8c58-259">![Installa Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e8c58-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e8c58-260">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e8c58-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e8c58-261">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="e8c58-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="e8c58-263">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="e8c58-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e8c58-264">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-264">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="e8c58-266">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="e8c58-266">*Installation progress*</span></span>
6. <span data-ttu-id="e8c58-267">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-267">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="e8c58-269">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="e8c58-269">*Installation completed*</span></span>
7. <span data-ttu-id="e8c58-270">Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="e8c58-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e8c58-271">Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="e8c58-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="e8c58-273">*Visual Studio Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="e8c58-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="e8c58-274">Appendice b: uso dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="e8c58-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="e8c58-275">Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione.</span><span class="sxs-lookup"><span data-stu-id="e8c58-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e8c58-276">Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="e8c58-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e8c58-277">![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "frammenti di codice con Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="e8c58-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e8c58-278">*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="e8c58-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e8c58-279">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="e8c58-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e8c58-280">Posizionare il cursore in cui si vuole inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="e8c58-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e8c58-281">Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="e8c58-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e8c58-282">Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="e8c58-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e8c58-283">Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="e8c58-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e8c58-284">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="e8c58-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e8c58-285">![Iniziare a digitare il nome di frammento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="e8c58-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e8c58-286">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="e8c58-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="e8c58-287">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "premere Tab per selezionare il frammento di codice evidenziata")</span><span class="sxs-lookup"><span data-stu-id="e8c58-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e8c58-288">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="e8c58-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e8c58-289">![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "si espanderà il frammento di codice e premere nuovamente Tab")</span><span class="sxs-lookup"><span data-stu-id="e8c58-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e8c58-290">*Il frammento di codice e premere nuovamente Tab espanderà*</span><span class="sxs-lookup"><span data-stu-id="e8c58-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e8c58-291">***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="e8c58-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e8c58-292">Pulsante destro del mouse in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="e8c58-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e8c58-293">Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="e8c58-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e8c58-294">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="e8c58-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e8c58-295">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="e8c58-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e8c58-296">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="e8c58-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e8c58-297">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")</span><span class="sxs-lookup"><span data-stu-id="e8c58-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e8c58-298">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*</span><span class="sxs-lookup"><span data-stu-id="e8c58-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
