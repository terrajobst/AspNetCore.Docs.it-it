---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Aggiungere modelli e controller | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 31e0fbf65c74a18677588cfa412dc34f7b25f78f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806358"
---
<a name="add-models-and-controllers"></a><span data-ttu-id="29a33-102">Aggiungere modelli e controller</span><span class="sxs-lookup"><span data-stu-id="29a33-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="29a33-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="29a33-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="29a33-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="29a33-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="29a33-105">In questa sezione si aggiungerà le classi di modello che definiscono le entità del database.</span><span class="sxs-lookup"><span data-stu-id="29a33-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="29a33-106">Si aggiungeranno quindi controller API Web che eseguono operazioni CRUD su tali entità.</span><span class="sxs-lookup"><span data-stu-id="29a33-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="29a33-107">Aggiungere le classi del modello</span><span class="sxs-lookup"><span data-stu-id="29a33-107">Add Model Classes</span></span>

<span data-ttu-id="29a33-108">In questa esercitazione si creerà il database usando l'approccio "Code First" per Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="29a33-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="29a33-109">Code First, si scrivono in c# le classi che corrispondono alle tabelle di database ed Entity Framework crea il database.</span><span class="sxs-lookup"><span data-stu-id="29a33-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="29a33-110">(Per altre informazioni, vedere [approcci di sviluppo di Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="29a33-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="29a33-111">Si inizia definendo gli oggetti di dominio come oggetti poco (plain-old CLR Object).</span><span class="sxs-lookup"><span data-stu-id="29a33-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="29a33-112">Si creerà gli oggetti poco seguenti:</span><span class="sxs-lookup"><span data-stu-id="29a33-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="29a33-113">Autore</span><span class="sxs-lookup"><span data-stu-id="29a33-113">Author</span></span>
- <span data-ttu-id="29a33-114">Libro</span><span class="sxs-lookup"><span data-stu-id="29a33-114">Book</span></span>

<span data-ttu-id="29a33-115">In Esplora soluzioni, pulsante destro del mouse, fare clic sulla cartella modelli.</span><span class="sxs-lookup"><span data-stu-id="29a33-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="29a33-116">Selezionare **Add**, quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="29a33-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="29a33-117">Assegnare alla classe il nome `Author`.</span><span class="sxs-lookup"><span data-stu-id="29a33-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="29a33-118">Sostituire tutto il codice boilerplate in Author.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="29a33-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="29a33-119">Aggiungere un'altra classe denominata `Book`, con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="29a33-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="29a33-120">Entity Framework, tali modelli verranno utilizzati per creare tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="29a33-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="29a33-121">Per ogni modello, il `Id` proprietà diventerà la colonna chiave primaria della tabella di database.</span><span class="sxs-lookup"><span data-stu-id="29a33-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="29a33-122">Nella classe Book, il `AuthorId` definisce una chiave esterna nel `Author` tabella.</span><span class="sxs-lookup"><span data-stu-id="29a33-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="29a33-123">(Per motivi di semplicità, suppongo che ogni libro ha un singolo autore.) La classe book contiene inoltre una proprietà di navigazione per i relativi `Author`.</span><span class="sxs-lookup"><span data-stu-id="29a33-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="29a33-124">È possibile utilizzare la proprietà di navigazione per accedere correlato `Author` nel codice.</span><span class="sxs-lookup"><span data-stu-id="29a33-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="29a33-125">Informazioni sulle proprietà di navigazione nella parte 4, dico [gestione delle relazioni tra entità](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="29a33-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="29a33-126">Aggiungere controller API Web</span><span class="sxs-lookup"><span data-stu-id="29a33-126">Add Web API Controllers</span></span>

<span data-ttu-id="29a33-127">In questa sezione si aggiungerà il controller API Web che supportano le operazioni CRUD (creare, leggere, aggiornare ed eliminare).</span><span class="sxs-lookup"><span data-stu-id="29a33-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="29a33-128">Il controller userà Entity Framework per comunicare con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="29a33-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="29a33-129">In primo luogo, è possibile eliminare il file Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="29a33-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="29a33-130">Questo file contiene un controller API Web di esempio, ma non è necessario per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="29a33-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="29a33-131">Successivamente, compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="29a33-131">Next, build the project.</span></span> <span data-ttu-id="29a33-132">Lo scaffolding API Web Usa la reflection per trovare le classi del modello, pertanto è necessario l'assembly compilato.</span><span class="sxs-lookup"><span data-stu-id="29a33-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="29a33-133">In Esplora soluzioni fare clic sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="29a33-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="29a33-134">Selezionare **Add**, quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="29a33-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="29a33-135">Nel **Add Scaffold** finestra di dialogo, seleziona "Web API 2 Controller con azioni, che usa Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="29a33-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="29a33-136">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="29a33-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="29a33-137">Nel **Aggiungi Controller** finestra di dialogo, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="29a33-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="29a33-138">Nel **classe modello** elenco a discesa, seleziona il `Author` classe.</span><span class="sxs-lookup"><span data-stu-id="29a33-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="29a33-139">(Se non viene visualizzato nell'elenco a discesa, assicurarsi che è stato compilato il progetto.)</span><span class="sxs-lookup"><span data-stu-id="29a33-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="29a33-140">Controllare "Azioni del controller asincrono Usa".</span><span class="sxs-lookup"><span data-stu-id="29a33-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="29a33-141">Lasciare il nome controller &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="29a33-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="29a33-142">Fare clic su più (+) accanto al pulsante **alla classe contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="29a33-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="29a33-143">Nel **nuovo contesto dati** finestra di dialogo, lasciare il nome predefinito e fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="29a33-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="29a33-144">Fare clic su **Add** per completare il **Aggiungi Controller** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="29a33-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="29a33-145">La finestra di dialogo aggiunge due classi al progetto:</span><span class="sxs-lookup"><span data-stu-id="29a33-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="29a33-146">`AuthorsController` definisce un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="29a33-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="29a33-147">Il controller implementa l'API REST usata dai client per eseguire operazioni CRUD nell'elenco degli autori.</span><span class="sxs-lookup"><span data-stu-id="29a33-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="29a33-148">`BookServiceContext` gestisce oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, il rilevamento delle modifiche e rendere persistenti i dati nel database.</span><span class="sxs-lookup"><span data-stu-id="29a33-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="29a33-149">Eredita da `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="29a33-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="29a33-150">A questo punto, compilare di nuovo il progetto.</span><span class="sxs-lookup"><span data-stu-id="29a33-150">At this point, build the project again.</span></span> <span data-ttu-id="29a33-151">Passare ora tramite la stessa procedura per aggiungere un controller API per `Book` entità.</span><span class="sxs-lookup"><span data-stu-id="29a33-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="29a33-152">Selezionare questa volta `Book` per la classe di modello e selezionare esistente `BookServiceContext` classe per la classe del contesto dati.</span><span class="sxs-lookup"><span data-stu-id="29a33-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="29a33-153">(Non creare un nuovo contesto dati). Fare clic su **Add** per aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="29a33-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="29a33-154">[Precedente](part-1.md)
> [Successivo](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="29a33-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
