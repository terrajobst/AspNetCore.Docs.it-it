---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Visualizza una tabella di dati del Database (c#) | Documenti Microsoft
author: microsoft
description: In questa esercitazione illustrano due metodi per la visualizzazione di un set di record del database. Visualizzare due metodi di formattazione di un set di record del database in un elemento HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 37ea081df2ee26e186669b815a4d769e1976ae9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="794ed-104">Visualizza una tabella di dati del Database (c#)</span><span class="sxs-lookup"><span data-stu-id="794ed-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="794ed-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="794ed-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="794ed-106">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="794ed-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="794ed-107">In questa esercitazione illustrano due metodi per la visualizzazione di un set di record del database.</span><span class="sxs-lookup"><span data-stu-id="794ed-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="794ed-108">Visualizzare due metodi di formattazione di un set di record del database in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="794ed-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="794ed-109">Visualizzare in primo luogo, come è possibile formattare i record di database direttamente in una vista.</span><span class="sxs-lookup"><span data-stu-id="794ed-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="794ed-110">Successivamente, viene illustrato come è possibile sfruttare parziali durante la formattazione di record del database.</span><span class="sxs-lookup"><span data-stu-id="794ed-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="794ed-111">L'obiettivo di questa esercitazione è illustrare come è possibile visualizzare una tabella HTML di dati del database in un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="794ed-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="794ed-112">È in primo luogo, imparare a utilizzare gli strumenti di scaffolding inclusi in Visual Studio per generare una vista che consente di visualizzare automaticamente un set di record.</span><span class="sxs-lookup"><span data-stu-id="794ed-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="794ed-113">Successivamente, si imparare a usare un elemento parziale come modello quando si formatta i record del database.</span><span class="sxs-lookup"><span data-stu-id="794ed-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="794ed-114">Creare le classi modello</span><span class="sxs-lookup"><span data-stu-id="794ed-114">Create the Model Classes</span></span>

<span data-ttu-id="794ed-115">Verrà visualizzato il set di record dalla tabella di database film.</span><span class="sxs-lookup"><span data-stu-id="794ed-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="794ed-116">La tabella di database di filmati contiene le colonne seguenti:</span><span class="sxs-lookup"><span data-stu-id="794ed-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="794ed-117">**Nome colonna**</span><span class="sxs-lookup"><span data-stu-id="794ed-117">**Column Name**</span></span> | <span data-ttu-id="794ed-118">**Tipo di dati**</span><span class="sxs-lookup"><span data-stu-id="794ed-118">**Data Type**</span></span> | <span data-ttu-id="794ed-119">**Consenti valori null**</span><span class="sxs-lookup"><span data-stu-id="794ed-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="794ed-120">Id</span><span class="sxs-lookup"><span data-stu-id="794ed-120">Id</span></span> | <span data-ttu-id="794ed-121">Int</span><span class="sxs-lookup"><span data-stu-id="794ed-121">Int</span></span> | <span data-ttu-id="794ed-122">False</span><span class="sxs-lookup"><span data-stu-id="794ed-122">False</span></span> |
| <span data-ttu-id="794ed-123">Titolo</span><span class="sxs-lookup"><span data-stu-id="794ed-123">Title</span></span> | <span data-ttu-id="794ed-124">Nvarchar (200)</span><span class="sxs-lookup"><span data-stu-id="794ed-124">Nvarchar(200)</span></span> | <span data-ttu-id="794ed-125">False</span><span class="sxs-lookup"><span data-stu-id="794ed-125">False</span></span> |
| <span data-ttu-id="794ed-126">Director</span><span class="sxs-lookup"><span data-stu-id="794ed-126">Director</span></span> | <span data-ttu-id="794ed-127">Nvarchar (50)</span><span class="sxs-lookup"><span data-stu-id="794ed-127">NVarchar(50)</span></span> | <span data-ttu-id="794ed-128">False</span><span class="sxs-lookup"><span data-stu-id="794ed-128">False</span></span> |
| <span data-ttu-id="794ed-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="794ed-129">DateReleased</span></span> | <span data-ttu-id="794ed-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="794ed-130">DateTime</span></span> | <span data-ttu-id="794ed-131">False</span><span class="sxs-lookup"><span data-stu-id="794ed-131">False</span></span> |


<span data-ttu-id="794ed-132">Per rappresentare la tabella di filmati nell'applicazione ASP.NET MVC, è necessario creare una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="794ed-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="794ed-133">In questa esercitazione, verrà usato il Framework di entità di Microsoft per creare il modello di classi.</span><span class="sxs-lookup"><span data-stu-id="794ed-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="794ed-134">In questa esercitazione, verrà usato il Framework di entità di Microsoft.</span><span class="sxs-lookup"><span data-stu-id="794ed-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="794ed-135">Tuttavia, è importante comprendere che è possibile utilizzare un'ampia gamma di tecnologie diverse per interagire con un database da un'applicazione ASP.NET MVC inclusi LINQ to SQL, NHibernate o ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="794ed-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="794ed-136">Seguire questi passaggi per avviare la procedura guidata Entity Data Model:</span><span class="sxs-lookup"><span data-stu-id="794ed-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="794ed-137">Fare clic sulla cartella modelli nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo**.</span><span class="sxs-lookup"><span data-stu-id="794ed-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="794ed-138">Selezionare il **dati** categoria e selezionare il **ADO.NET Entity Data Model** modello.</span><span class="sxs-lookup"><span data-stu-id="794ed-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="794ed-139">Denominare il modello di dati *MoviesDBModel.edmx* e fare clic su di **Aggiungi** pulsante.</span><span class="sxs-lookup"><span data-stu-id="794ed-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="794ed-140">Dopo aver fatto clic sul pulsante Aggiungi viene visualizzata la procedura guidata Entity Data Model (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="794ed-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="794ed-141">Seguire questi passaggi per completare la procedura guidata:</span><span class="sxs-lookup"><span data-stu-id="794ed-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="794ed-142">Nel **Scegli contenuto Model** passaggio, seleziona il **genera da database** opzione.</span><span class="sxs-lookup"><span data-stu-id="794ed-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="794ed-143">Nel **Seleziona connessione dati** di istruzioni, utilizzare il *MoviesDB.mdf* connessione dati e il nome *MoviesDBEntities* per le impostazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="794ed-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="794ed-144">Fare clic su di **Avanti** pulsante.</span><span class="sxs-lookup"><span data-stu-id="794ed-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="794ed-145">Nel **Seleziona oggetti di Database** passaggio, espandere il nodo tabelle, selezionare la tabella di film.</span><span class="sxs-lookup"><span data-stu-id="794ed-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="794ed-146">Immettere lo spazio dei nomi *modelli* e fare clic su di **fine** pulsante.</span><span class="sxs-lookup"><span data-stu-id="794ed-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="794ed-147">[![Creazione di LINQ alle classi di SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="794ed-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="794ed-148">**Figura 01**: creazione di classi LINQ to SQL ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="794ed-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="794ed-149">Dopo aver completato la procedura guidata Entity Data Model, verrà visualizzata la finestra di progettazione Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="794ed-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="794ed-150">La finestra di progettazione deve essere visualizzato l'entità di filmati (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="794ed-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="794ed-151">[![Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="794ed-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="794ed-152">**Figura 02**: la progettazione di Entity Data Model ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="794ed-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="794ed-153">È necessario apportare una modifica prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="794ed-153">We need to make one change before we continue.</span></span> <span data-ttu-id="794ed-154">La procedura guidata Entity Data genera una classe di modello denominata *filmati* che rappresenta la tabella di database di film.</span><span class="sxs-lookup"><span data-stu-id="794ed-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="794ed-155">Perché la classe di filmati verrà usato per rappresentare un filmato specifico, è necessario modificare il nome della classe da *film* anziché *filmati* (singolare anziché plurale).</span><span class="sxs-lookup"><span data-stu-id="794ed-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="794ed-156">Fare doppio clic sul nome della classe nell'area di progettazione e modificare il nome della classe di filmati in film.</span><span class="sxs-lookup"><span data-stu-id="794ed-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="794ed-157">Dopo aver apportato questa modifica, fare clic su di **salvare** pulsante (icona del disco floppy) per generare la classe di film.</span><span class="sxs-lookup"><span data-stu-id="794ed-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="794ed-158">Creare il Controller di filmati</span><span class="sxs-lookup"><span data-stu-id="794ed-158">Create the Movies Controller</span></span>

<span data-ttu-id="794ed-159">Ora che è disponibile un modo per rappresentare i record del database, è possibile creare un controller che restituisce la raccolta di filmati.</span><span class="sxs-lookup"><span data-stu-id="794ed-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="794ed-160">All'interno della finestra Esplora soluzioni di Visual Studio, fare clic sulla cartella controller e selezionare l'opzione di menu **Aggiungi, Controller** (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="794ed-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="794ed-161">[![L'aggiunta di Menu Controller](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="794ed-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="794ed-162">**Figura 03**: Aggiungi Menu Controller ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="794ed-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="794ed-163">Quando il **Aggiungi Controller** viene visualizzata la finestra, immettere il nome del controller MovieController (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="794ed-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="794ed-164">Fare clic su di **Aggiungi** pulsante per aggiungere il nuovo controller.</span><span class="sxs-lookup"><span data-stu-id="794ed-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="794ed-165">[![La finestra di dialogo Aggiungi Controller](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="794ed-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="794ed-166">**Figura 04**: finestra di dialogo di Aggiungi Controller ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="794ed-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="794ed-167">È necessario modificare l'azione Index () esposta dal controller di film in modo che venga restituito il set di record del database.</span><span class="sxs-lookup"><span data-stu-id="794ed-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="794ed-168">Modificare il controller in modo che risulti simile controller nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="794ed-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="794ed-169">**Elenco 1 – Controllers\MovieController.cs**</span><span class="sxs-lookup"><span data-stu-id="794ed-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="794ed-170">Nel listato 1, la classe MoviesDBEntities viene utilizzata per rappresentare il database MoviesDB.</span><span class="sxs-lookup"><span data-stu-id="794ed-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="794ed-171">Per utilizzare questa classe, è necessario importare lo spazio dei nomi MvcApplication1.Models simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="794ed-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="794ed-172">utilizzando MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="794ed-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="794ed-173">L'espressione *entità. MovieSet.ToList()* restituisce il set di tutti i filmati dalla tabella di database film.</span><span class="sxs-lookup"><span data-stu-id="794ed-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="794ed-174">Creare la vista</span><span class="sxs-lookup"><span data-stu-id="794ed-174">Create the View</span></span>

<span data-ttu-id="794ed-175">Il modo più semplice per visualizzare un set di record del database in una tabella HTML è per poter sfruttare lo scaffolding fornito da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="794ed-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="794ed-176">Compilare l'applicazione selezionando l'opzione di menu **di compilazione, Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="794ed-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="794ed-177">È necessario compilare l'applicazione prima dell'apertura di **Aggiungi visualizzazione** finestra di dialogo o le classi di dati non verranno visualizzati nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="794ed-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="794ed-178">L'azione Index () e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere Figura 5).</span><span class="sxs-lookup"><span data-stu-id="794ed-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="794ed-179">[![Aggiunta di una vista](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="794ed-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="794ed-180">**Figura 05**: aggiunta di una vista ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="794ed-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="794ed-181">Nel **Aggiungi visualizzazione** finestra di dialogo, selezionare la casella di controllo con etichettata **creare una visualizzazione fortemente tipizzata**.</span><span class="sxs-lookup"><span data-stu-id="794ed-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="794ed-182">Selezionare la classe di film come il **visualizzare dati classe**.</span><span class="sxs-lookup"><span data-stu-id="794ed-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="794ed-183">Selezionare *elenco* come il **visualizzare il contenuto** (vedere Figura 6).</span><span class="sxs-lookup"><span data-stu-id="794ed-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="794ed-184">Selezionando queste opzioni genererà una visualizzazione fortemente tipizzato che consente di visualizzare un elenco di film.</span><span class="sxs-lookup"><span data-stu-id="794ed-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="794ed-185">[![La finestra di dialogo Aggiungi visualizzazione](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="794ed-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="794ed-186">**Figura 06**: finestra di dialogo di Aggiungi visualizzazione ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="794ed-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="794ed-187">Dopo aver selezionato il **Aggiungi** pulsante, la visualizzazione nel listato 2 viene generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="794ed-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="794ed-188">Questa vista contiene il codice necessario per scorrere la raccolta di filmati e visualizzare ciascuna delle proprietà di un film.</span><span class="sxs-lookup"><span data-stu-id="794ed-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="794ed-189">**Elenco di 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="794ed-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="794ed-190">È possibile eseguire l'applicazione selezionando l'opzione di menu **Debug, Avvia debug** (o premendo il tasto F5).</span><span class="sxs-lookup"><span data-stu-id="794ed-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="794ed-191">Esegue l'applicazione avvia Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="794ed-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="794ed-192">Se si passa all'URL /Movie quindi verrà visualizzata la pagina nella figura 7.</span><span class="sxs-lookup"><span data-stu-id="794ed-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="794ed-193">[![Una tabella di filmati](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="794ed-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="794ed-194">**Figura 07**: una tabella di filmati ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="794ed-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="794ed-195">Se non sono necessariamente l'aspetto della griglia di record del database nella figura 7 è semplicemente possibile modificare la visualizzazione dell'indice.</span><span class="sxs-lookup"><span data-stu-id="794ed-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="794ed-196">Ad esempio, è possibile modificare il *DateReleased* intestazione *data di rilascio* modificando la visualizzazione dell'indice.</span><span class="sxs-lookup"><span data-stu-id="794ed-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="794ed-197">Creare un modello con un elemento parziale</span><span class="sxs-lookup"><span data-stu-id="794ed-197">Create a Template with a Partial</span></span>

<span data-ttu-id="794ed-198">Quando una visualizzazione ottiene troppo complessa, è consigliabile avviare la vista di rilievo in parziali.</span><span class="sxs-lookup"><span data-stu-id="794ed-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="794ed-199">Utilizzando parziali rende più facile da comprendere e gestire le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="794ed-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="794ed-200">Si creerà un elemento parziale che è possibile utilizzare come modello per formattare ogni record database film.</span><span class="sxs-lookup"><span data-stu-id="794ed-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="794ed-201">Seguire questi passaggi per creare parziale:</span><span class="sxs-lookup"><span data-stu-id="794ed-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="794ed-202">La cartella Views\Movie e scegliere l'opzione di menu **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="794ed-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="794ed-203">Selezionare la casella di controllo con etichettata *creare una visualizzazione parziale (con estensione ascx)*.</span><span class="sxs-lookup"><span data-stu-id="794ed-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="794ed-204">Assegnare un nome parziale *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="794ed-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="794ed-205">Selezionare la casella di controllo con etichettata **creare una visualizzazione fortemente tipizzata**.</span><span class="sxs-lookup"><span data-stu-id="794ed-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="794ed-206">Selezionare come filmato il *visualizzare dati classe*.</span><span class="sxs-lookup"><span data-stu-id="794ed-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="794ed-207">Selezionare vuoto come il *visualizzare il contenuto*.</span><span class="sxs-lookup"><span data-stu-id="794ed-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="794ed-208">Fare clic su di **Aggiungi** pulsante per aggiungere al progetto parziale.</span><span class="sxs-lookup"><span data-stu-id="794ed-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="794ed-209">Dopo aver completato questi passaggi, modificare il MovieTemplate parziale come elenco di 3.</span><span class="sxs-lookup"><span data-stu-id="794ed-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="794ed-210">**Elenco di 3: Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="794ed-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="794ed-211">Parziale listato 3 contiene un modello per una singola riga di record.</span><span class="sxs-lookup"><span data-stu-id="794ed-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="794ed-212">La visualizzazione dell'indice modificata listato 4 utilizza la MovieTemplate parziale.</span><span class="sxs-lookup"><span data-stu-id="794ed-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="794ed-213">**Listato 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="794ed-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="794ed-214">La vista listato 4 contiene un ciclo foreach che scorre tutti i film.</span><span class="sxs-lookup"><span data-stu-id="794ed-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="794ed-215">Per ogni film, il MovieTemplate parziale viene utilizzata per formattare il film.</span><span class="sxs-lookup"><span data-stu-id="794ed-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="794ed-216">Il MovieTemplate rendering viene eseguito chiamando il metodo di supporto RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="794ed-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="794ed-217">La visualizzazione dell'indice modificata esegue il rendering molto stessa tabella HTML di record del database.</span><span class="sxs-lookup"><span data-stu-id="794ed-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="794ed-218">Tuttavia, la vista è stata notevolmente semplificata.</span><span class="sxs-lookup"><span data-stu-id="794ed-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="794ed-219">Il metodo RenderPartial() è diverso rispetto alla maggior parte dei metodi di supporto perché non restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="794ed-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="794ed-220">Pertanto, è necessario chiamare il metodo RenderPartial() usando &lt;Html.RenderPartial() %; %&gt; anziché &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="794ed-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="794ed-221">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="794ed-221">Summary</span></span>

<span data-ttu-id="794ed-222">L'obiettivo di questa esercitazione è illustrare come è possibile visualizzare un set di record del database in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="794ed-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="794ed-223">In primo luogo, è stato descritto come restituire un set di record del database da un'azione del controller per sfruttare i vantaggi di Entity Framework Microsoft.</span><span class="sxs-lookup"><span data-stu-id="794ed-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="794ed-224">Successivamente, è stato descritto come utilizzare lo scaffolding di Visual Studio per generare una visualizzazione contenente una raccolta di elementi automaticamente.</span><span class="sxs-lookup"><span data-stu-id="794ed-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="794ed-225">Infine, è stato descritto come semplificare la visualizzazione sfruttando un parziale.</span><span class="sxs-lookup"><span data-stu-id="794ed-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="794ed-226">È stato descritto come utilizzare un elemento parziale come modello in modo che è possibile formattare ogni record di database.</span><span class="sxs-lookup"><span data-stu-id="794ed-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="794ed-227">[Precedente](creating-model-classes-with-linq-to-sql-cs.md)
[Successivo](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="794ed-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
