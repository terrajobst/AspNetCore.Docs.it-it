---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Aggiungere modelli e controller | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="add-models-and-controllers"></a><span data-ttu-id="5e461-102">Aggiungere modelli e controller</span><span class="sxs-lookup"><span data-stu-id="5e461-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="5e461-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5e461-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5e461-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="5e461-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="5e461-105">In questa sezione si aggiungerà le classi di modello che definiscono le entità del database.</span><span class="sxs-lookup"><span data-stu-id="5e461-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="5e461-106">Si aggiungeranno quindi controller API Web che eseguono operazioni CRUD su tali entità.</span><span class="sxs-lookup"><span data-stu-id="5e461-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="5e461-107">Aggiungere le classi modello</span><span class="sxs-lookup"><span data-stu-id="5e461-107">Add Model Classes</span></span>

<span data-ttu-id="5e461-108">In questa esercitazione si creerà il database utilizzando l'approccio "Code First" per Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="5e461-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="5e461-109">Code First, si scrivono in c# le classi che corrispondono alle tabelle di database ed EF crea il database.</span><span class="sxs-lookup"><span data-stu-id="5e461-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="5e461-110">(Per ulteriori informazioni, vedere [approcci allo sviluppo con Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="5e461-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="5e461-111">Viene innanzitutto la definizione di oggetti di dominio come POCOs (oggetti CLR normale precedente).</span><span class="sxs-lookup"><span data-stu-id="5e461-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="5e461-112">Verrà creata la POCOs seguenti:</span><span class="sxs-lookup"><span data-stu-id="5e461-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="5e461-113">Autore</span><span class="sxs-lookup"><span data-stu-id="5e461-113">Author</span></span>
- <span data-ttu-id="5e461-114">Rubrica</span><span class="sxs-lookup"><span data-stu-id="5e461-114">Book</span></span>

<span data-ttu-id="5e461-115">In Esplora soluzioni, fare clic sulla cartella di modelli.</span><span class="sxs-lookup"><span data-stu-id="5e461-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="5e461-116">Selezionare **Aggiungi**, quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="5e461-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="5e461-117">Assegnare alla classe il nome `Author`.</span><span class="sxs-lookup"><span data-stu-id="5e461-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="5e461-118">Sostituire tutto il codice boilerplate in Author.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="5e461-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="5e461-119">Aggiungere un'altra classe denominata `Book`, con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="5e461-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="5e461-120">Questi modelli di Entity Framework verranno utilizzati per creare tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="5e461-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="5e461-121">Per ogni modello, il `Id` proprietà diventerà una colonna chiave primaria della tabella di database.</span><span class="sxs-lookup"><span data-stu-id="5e461-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="5e461-122">Nella classe Book di `AuthorId` definisce una chiave esterna nel `Author` tabella.</span><span class="sxs-lookup"><span data-stu-id="5e461-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="5e461-123">(Per motivi di semplicità, suppone che ogni libro di autore di un singolo.) Classe book contiene anche una proprietà di navigazione per correlata `Author`.</span><span class="sxs-lookup"><span data-stu-id="5e461-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="5e461-124">È possibile utilizzare la proprietà di navigazione per accedere correlata `Author` nel codice.</span><span class="sxs-lookup"><span data-stu-id="5e461-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="5e461-125">Informazioni sulle proprietà di navigazione nella parte 4, dire [gestisce le relazioni di entità](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="5e461-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="5e461-126">Aggiungere il controller API Web</span><span class="sxs-lookup"><span data-stu-id="5e461-126">Add Web API Controllers</span></span>

<span data-ttu-id="5e461-127">In questa sezione verrà aggiunto il controller API Web che supportano le operazioni CRUD (creare, leggere, aggiornare ed eliminare).</span><span class="sxs-lookup"><span data-stu-id="5e461-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="5e461-128">I controller utilizzerà Entity Framework per comunicare con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="5e461-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="5e461-129">In primo luogo, è possibile eliminare il file Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="5e461-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="5e461-130">Questo file contiene un controller API Web di esempio, ma non è necessario per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5e461-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="5e461-131">Successivamente, compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="5e461-131">Next, build the project.</span></span> <span data-ttu-id="5e461-132">Lo scaffolding di API Web utilizza la reflection per trovare le classi di modello, pertanto è necessario l'assembly compilato.</span><span class="sxs-lookup"><span data-stu-id="5e461-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="5e461-133">In Esplora soluzioni, fare clic sulla cartella controller.</span><span class="sxs-lookup"><span data-stu-id="5e461-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="5e461-134">Selezionare **Aggiungi**, quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="5e461-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="5e461-135">Nel **aggiungere lo scaffolding** finestra di dialogo, seleziona "Web API 2 Controller con azioni, mediante Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="5e461-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="5e461-136">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5e461-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="5e461-137">Nel **Aggiungi Controller** finestra di dialogo, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5e461-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="5e461-138">Nel **classe modello** elenco a discesa, seleziona la `Author` classe.</span><span class="sxs-lookup"><span data-stu-id="5e461-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="5e461-139">(Se non viene visualizzato nell'elenco a discesa, assicurarsi che il progetto è stato generato.)</span><span class="sxs-lookup"><span data-stu-id="5e461-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="5e461-140">Selezionare "Usa azioni asincrone del controller".</span><span class="sxs-lookup"><span data-stu-id="5e461-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="5e461-141">Lasciare il nome del controller come &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="5e461-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="5e461-142">Fare clic su più (+) accanto al pulsante **classe contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="5e461-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="5e461-143">Nel **nuovo contesto dati** finestra di dialogo, lasciare il nome predefinito e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5e461-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="5e461-144">Fare clic su **Aggiungi** per completare il **Aggiungi Controller** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="5e461-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="5e461-145">La finestra di dialogo aggiunge due classi al progetto:</span><span class="sxs-lookup"><span data-stu-id="5e461-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="5e461-146">`AuthorsController` definisce un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="5e461-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="5e461-147">Il controller implementa l'API REST utilizzato dai client per eseguire operazioni CRUD su nell'elenco degli autori.</span><span class="sxs-lookup"><span data-stu-id="5e461-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="5e461-148">`BookServiceContext` gestisce gli oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, il rilevamento delle modifiche e rendere persistenti i dati al database.</span><span class="sxs-lookup"><span data-stu-id="5e461-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="5e461-149">Eredita da `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5e461-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="5e461-150">A questo punto, generare di nuovo il progetto.</span><span class="sxs-lookup"><span data-stu-id="5e461-150">At this point, build the project again.</span></span> <span data-ttu-id="5e461-151">Ora accedere tramite gli stessi passaggi per aggiungere un controller di API per `Book` entità.</span><span class="sxs-lookup"><span data-stu-id="5e461-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="5e461-152">In questo momento, selezionare `Book` per la classe di modello e selezionare esistente `BookServiceContext` classe per la classe di contesto dati.</span><span class="sxs-lookup"><span data-stu-id="5e461-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="5e461-153">(Non creare un nuovo contesto dati). Fare clic su **Aggiungi** per aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="5e461-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="5e461-154">[Precedente](part-1.md)
> [Successivo](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="5e461-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
