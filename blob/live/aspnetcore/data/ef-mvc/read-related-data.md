---
title: Il Core di ASP.NET MVC con Entity Framework Core - leggere i dati correlati - 6 di 10
author: tdykstra
description: "In questa esercitazione verrà leggere e visualizzare i dati correlati, ovvero i dati di Entity Framework carica le proprietà di navigazione."
keywords: ASP.NET Core, Entity Framework Core, i dati correlati, join
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 71fec30f-8ea7-4ca8-96e3-d2e26c5be44e
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 778ef976fdbef70684ca26b0c7c548ffcc83ee00
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a><span data-ttu-id="871f3-104">Lettura correlati dati - EF Core con l'esercitazione di base di ASP.NET MVC (6 di 10)</span><span class="sxs-lookup"><span data-stu-id="871f3-104">Reading related data - EF Core with ASP.NET Core MVC tutorial (6 of 10)</span></span>

<span data-ttu-id="871f3-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="871f3-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="871f3-106">L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="871f3-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="871f3-107">Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="871f3-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="871f3-108">Nell'esercitazione precedente, è stato completato il modello di dati dell'istituto di istruzione.</span><span class="sxs-lookup"><span data-stu-id="871f3-108">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="871f3-109">In questa esercitazione verrà leggere e visualizzare i dati correlati, ovvero i dati di Entity Framework carica le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-109">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="871f3-110">Le illustrazioni seguenti mostrano le pagine che verranno utilizzate.</span><span class="sxs-lookup"><span data-stu-id="871f3-110">The following illustrations show the pages that you'll work with.</span></span>

![Pagina di indice corsi](read-related-data/_static/courses-index.png)

![Pagina di indice istruttori](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="871f3-113">Caricamento eager esplicito e lazy dei dati correlati</span><span class="sxs-lookup"><span data-stu-id="871f3-113">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="871f3-114">Esistono diversi modi software Mapping relazionale a oggetti (ORM), ad esempio Entity Framework è possibile caricare i dati correlati alle proprietà di navigazione di un'entità:</span><span class="sxs-lookup"><span data-stu-id="871f3-114">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="871f3-115">Caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="871f3-115">Eager loading.</span></span> <span data-ttu-id="871f3-116">Durante la lettura di entità, i dati correlati vengono recuperati con essa.</span><span class="sxs-lookup"><span data-stu-id="871f3-116">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="871f3-117">Ciò comporta in genere una query singola join che recupera tutti i dati necessari.</span><span class="sxs-lookup"><span data-stu-id="871f3-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="871f3-118">Specificare il caricamento non differito in Entity Framework Core utilizzando il `Include` e `ThenInclude` metodi.</span><span class="sxs-lookup"><span data-stu-id="871f3-118">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Esempio di caricamento eager](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="871f3-120">È possibile recuperare alcuni dei dati nella query separate ed EF "correzioni" le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-120">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="871f3-121">Vale a dire EF vengono aggiunti automaticamente le entità recuperate separatamente in cui appartengono le proprietà di navigazione di entità recuperate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="871f3-121">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="871f3-122">Per la query che recupera i dati correlati, è possibile utilizzare il `Load` metodo anziché un metodo che restituisce un elenco o un oggetto, ad esempio `ToList` o `Single`.</span><span class="sxs-lookup"><span data-stu-id="871f3-122">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Esempio di query distinte](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="871f3-124">Caricamento esplicito.</span><span class="sxs-lookup"><span data-stu-id="871f3-124">Explicit loading.</span></span> <span data-ttu-id="871f3-125">Durante la lettura di entità, non recuperare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="871f3-125">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="871f3-126">Si scrive codice che recupera i dati correlati se necessario.</span><span class="sxs-lookup"><span data-stu-id="871f3-126">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="871f3-127">Come nel caso di il caricamento eager con query separate, esplicita, il caricamento dei risultati in più query inviate al database.</span><span class="sxs-lookup"><span data-stu-id="871f3-127">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="871f3-128">La differenza è che, con il caricamento esplicito, il codice specifica le proprietà di navigazione da caricare.</span><span class="sxs-lookup"><span data-stu-id="871f3-128">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="871f3-129">In Entity Framework Core 1.1 è possibile utilizzare il `Load` metodo per eseguire il caricamento esplicito.</span><span class="sxs-lookup"><span data-stu-id="871f3-129">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="871f3-130">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="871f3-130">For example:</span></span>

  ![Esempio di caricamento esplicito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="871f3-132">Caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="871f3-132">Lazy loading.</span></span> <span data-ttu-id="871f3-133">Durante la lettura di entità, non recuperare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="871f3-133">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="871f3-134">La prima volta che si tenta di accedere a una proprietà di navigazione, viene tuttavia recuperati automaticamente i dati necessari per la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-134">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="871f3-135">Invio di una query al database ogni volta che si tenta di ottenere dati da una proprietà di navigazione per la prima volta.</span><span class="sxs-lookup"><span data-stu-id="871f3-135">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="871f3-136">Entity Framework Core 1.0 non supporta il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="871f3-136">Entity Framework Core 1.0 does not support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="871f3-137">Considerazioni sulle prestazioni</span><span class="sxs-lookup"><span data-stu-id="871f3-137">Performance considerations</span></span>

<span data-ttu-id="871f3-138">Se si conosce che i dati correlati è necessario per tutte le entità recuperate, il caricamento eager spesso offre prestazioni ottimali, perché è in genere più efficiente delle query separate per ogni entità recuperata una singola query inviate al database.</span><span class="sxs-lookup"><span data-stu-id="871f3-138">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="871f3-139">Ad esempio, si supponga che ogni reparto di dieci corsi correlati.</span><span class="sxs-lookup"><span data-stu-id="871f3-139">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="871f3-140">Caricamento eager di tutti i dati correlati comporta solo una query singola (join) e un singolo round trip al database.</span><span class="sxs-lookup"><span data-stu-id="871f3-140">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="871f3-141">Una query separata per i corsi per ogni reparto comporta undici round trip al database.</span><span class="sxs-lookup"><span data-stu-id="871f3-141">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="871f3-142">Il round trip aggiuntivi al database sono particolarmente effetti negativi sulle prestazioni quando la latenza è elevata.</span><span class="sxs-lookup"><span data-stu-id="871f3-142">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="871f3-143">D'altra parte, in alcuni scenari è più efficiente query separate.</span><span class="sxs-lookup"><span data-stu-id="871f3-143">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="871f3-144">Caricamento eager di tutti i dati correlati in una query può causare un join molto complesso da generare, quale SQL Server in grado di elaborare in modo efficiente.</span><span class="sxs-lookup"><span data-stu-id="871f3-144">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="871f3-145">O se si desidera accedere alle proprietà di navigazione di un'entità solo per un subset di un set di entità a cui che si sta elaborando, query separate potrebbero risultare migliori perché il caricamento immediato di tutti gli elementi di sarebbe recuperare più dati superflui.</span><span class="sxs-lookup"><span data-stu-id="871f3-145">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="871f3-146">Se le prestazioni sono essenziali, è consigliabile testare le prestazioni di entrambi i modi per adottare la scelta migliore.</span><span class="sxs-lookup"><span data-stu-id="871f3-146">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="871f3-147">Creare una pagina di corsi che consente di visualizzare il nome di reparto</span><span class="sxs-lookup"><span data-stu-id="871f3-147">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="871f3-148">L'entità Course include una proprietà di navigazione che contiene l'entità reparto del corso assegnato al reparto.</span><span class="sxs-lookup"><span data-stu-id="871f3-148">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="871f3-149">Per visualizzare il nome del reparto assegnato in un elenco dei corsi, è necessario ottenere la proprietà del nome dell'entità di reparto nel `Course.Department` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-149">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="871f3-150">Creare un controller denominato CoursesController per il tipo di entità Course, con le stesse opzioni per il **Controller MVC con visualizzazioni, mediante Entity Framework** scaffolder che hai usato in precedenza per il controller di studenti, come illustrato di Nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="871f3-150">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Aggiungi controller corsi](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="871f3-152">Aprire *CoursesController.cs* ed esaminare il `Index` metodo.</span><span class="sxs-lookup"><span data-stu-id="871f3-152">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="871f3-153">Lo scaffolding automatica è specificato per il caricamento non differito il `Department` proprietà di navigazione tramite il `Include` metodo.</span><span class="sxs-lookup"><span data-stu-id="871f3-153">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="871f3-154">Sostituire il `Index` (metodo) con il codice seguente che utilizza un nome più appropriato per il `IQueryable` che restituisce le entità Course (`courses` anziché `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="871f3-154">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="871f3-155">Aprire *Views/Courses/Index.cshtml* e sostituire il codice del modello con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="871f3-155">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="871f3-156">Le modifiche vengono evidenziate:</span><span class="sxs-lookup"><span data-stu-id="871f3-156">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="871f3-157">Le seguenti modifiche apportate al codice scaffolding:</span><span class="sxs-lookup"><span data-stu-id="871f3-157">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="871f3-158">Modificare il titolo dall'indice ai corsi.</span><span class="sxs-lookup"><span data-stu-id="871f3-158">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="871f3-159">Aggiungere un **numero** colonna che mostra il `CourseID` valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="871f3-159">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="871f3-160">Per impostazione predefinita, le chiavi primarie non sono scaffolding perché in genere sono utilizzati per gli utenti finali.</span><span class="sxs-lookup"><span data-stu-id="871f3-160">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="871f3-161">Tuttavia, in questo caso la chiave primaria è significativa e si desidera visualizzarlo.</span><span class="sxs-lookup"><span data-stu-id="871f3-161">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="871f3-162">Modificare il **reparto** colonna per visualizzare il nome di reparto.</span><span class="sxs-lookup"><span data-stu-id="871f3-162">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="871f3-163">Il codice visualizza il `Name` proprietà dell'entità reparto che viene caricato il `Department` proprietà di navigazione:</span><span class="sxs-lookup"><span data-stu-id="871f3-163">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="871f3-164">Eseguire l'app e selezionare il **corsi** scheda per visualizzare l'elenco con i nomi di reparto.</span><span class="sxs-lookup"><span data-stu-id="871f3-164">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Pagina di indice corsi](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="871f3-166">Creare una pagina istruttori che mostra i corsi e le registrazioni</span><span class="sxs-lookup"><span data-stu-id="871f3-166">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="871f3-167">In questa sezione si creerà un controller e una visualizzazione per l'entità Instructor per visualizzare la pagina istruttori:</span><span class="sxs-lookup"><span data-stu-id="871f3-167">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Pagina di indice istruttori](read-related-data/_static/instructors-index.png)

<span data-ttu-id="871f3-169">Questa pagina legge e visualizza i dati correlati nei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="871f3-169">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="871f3-170">L'elenco di istruttori Mostra dati correlati nell'entità OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="871f3-170">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="871f3-171">Le entità Instructor e OfficeAssignment sono in una relazione uno-a-zero-o-uno.</span><span class="sxs-lookup"><span data-stu-id="871f3-171">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="871f3-172">Caricamento eager userai per le entità OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="871f3-172">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="871f3-173">Come spiegato in precedenza, il caricamento non differito è in genere più efficiente quando i dati correlati è necessario per tutte le righe recuperate della tabella primaria.</span><span class="sxs-lookup"><span data-stu-id="871f3-173">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="871f3-174">In questo caso, si desidera visualizzare le assegnazioni di office per tutti i docenti visualizzati.</span><span class="sxs-lookup"><span data-stu-id="871f3-174">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="871f3-175">Quando l'utente seleziona un docente, vengono visualizzate le entità Course correlate.</span><span class="sxs-lookup"><span data-stu-id="871f3-175">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="871f3-176">Le entità Instructor e corso hanno una relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="871f3-176">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="871f3-177">Caricamento eager da usare per le entità Course e le relative entità correlate di reparto.</span><span class="sxs-lookup"><span data-stu-id="871f3-177">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="871f3-178">In questo caso, query separate potrebbero essere più efficiente perché è necessario corsi solo per istruttore selezionato.</span><span class="sxs-lookup"><span data-stu-id="871f3-178">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="871f3-179">Tuttavia, in questo esempio viene illustrato come utilizzare il caricamento immediato per le proprietà di navigazione all'interno delle entità che sono a loro volta nelle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-179">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="871f3-180">Quando l'utente seleziona un corso, viene visualizzati i dati correlati dal set di entità le registrazioni.</span><span class="sxs-lookup"><span data-stu-id="871f3-180">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="871f3-181">Le entità Course e registrazione sono in una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="871f3-181">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="871f3-182">Utilizzare query separate per le entità di registrazione e le relative entità Student correlati.</span><span class="sxs-lookup"><span data-stu-id="871f3-182">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="871f3-183">Creare un modello di visualizzazione per la visualizzazione dell'indice Instructor</span><span class="sxs-lookup"><span data-stu-id="871f3-183">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="871f3-184">La pagina istruttori Mostra dati di tre tabelle diverse.</span><span class="sxs-lookup"><span data-stu-id="871f3-184">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="871f3-185">Pertanto, si creerà un modello di visualizzazione che include tre proprietà, ciascuna contenente i dati per una delle tabelle.</span><span class="sxs-lookup"><span data-stu-id="871f3-185">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="871f3-186">Nel *SchoolViewModels* cartella, creare *InstructorIndexData.cs* e sostituire il codice esistente con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="871f3-186">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="871f3-187">Creare le visualizzazioni e controller Instructor</span><span class="sxs-lookup"><span data-stu-id="871f3-187">Create the Instructor controller and views</span></span>

<span data-ttu-id="871f3-188">Creare un controller istruttori con azioni di lettura/scrittura EF come illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="871f3-188">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Aggiungere i docenti controller](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="871f3-190">Aprire *InstructorsController.cs* e aggiungere un tramite l'istruzione per lo spazio dei nomi ViewModel:</span><span class="sxs-lookup"><span data-stu-id="871f3-190">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="871f3-191">Sostituire il metodo di indice con il codice seguente per eseguire il caricamento immediato di dati correlati e inserirlo nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-191">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="871f3-192">Il metodo accetta i dati della route facoltativo (`id`) e un parametro di stringa di query (`courseID`) che forniscono i valori di ID del corso selezionato e istruttore selezionato.</span><span class="sxs-lookup"><span data-stu-id="871f3-192">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="871f3-193">I parametri sono forniti dal **selezionare** della pagina.</span><span class="sxs-lookup"><span data-stu-id="871f3-193">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="871f3-194">Il codice inizia creando un'istanza del modello di visualizzazione e inserire in essa contenuti l'elenco di istruttori.</span><span class="sxs-lookup"><span data-stu-id="871f3-194">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="871f3-195">Il codice specifica per il caricamento immediato di `Instructor.OfficeAssignment` e `Instructor.CourseAssignments` le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-195">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="871f3-196">All'interno di `CourseAssignments` proprietà, il `Course` è caricato all'interno della quale, il `Enrollments` e `Department` proprietà vengono caricati e all'interno di ogni `Enrollment` entità il `Student` proprietà è caricata.</span><span class="sxs-lookup"><span data-stu-id="871f3-196">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="871f3-197">Poiché la vista richiede sempre l'entità OfficeAssignment, è più efficiente recuperare che nella stessa query.</span><span class="sxs-lookup"><span data-stu-id="871f3-197">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="871f3-198">Le entità Course sono necessari quando viene selezionato un docente nella pagina web, una singola query è migliore di più query solo se la pagina viene visualizzata più spesso con un corso selezionato rispetto senza.</span><span class="sxs-lookup"><span data-stu-id="871f3-198">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="871f3-199">Il codice ripete `CourseAssignments` e `Course` perché è necessario due proprietà da `Course`.</span><span class="sxs-lookup"><span data-stu-id="871f3-199">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="871f3-200">La prima stringa di `ThenInclude` chiama Ottiene `CourseAssignment.Course`, `Course.Enrollments`, e `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="871f3-200">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="871f3-201">A questo punto nel codice, un altro `ThenInclude` per proprietà di navigazione di `Student`, che non è necessario.</span><span class="sxs-lookup"><span data-stu-id="871f3-201">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="871f3-202">Ma la chiamata `Include` ricomincia con `Instructor` proprietà, pertanto è necessario passare attraverso la catena di nuovo, questa volta specificare `Course.Department` anziché `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="871f3-202">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="871f3-203">Il codice seguente viene eseguito quando è stato selezionato un docente.</span><span class="sxs-lookup"><span data-stu-id="871f3-203">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="871f3-204">Istruttore selezionato viene recuperato dall'elenco di istruttori nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-204">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="871f3-205">Il modello di visualizzazione `Courses` proprietà viene quindi caricata con le entità Course da tale istruttore `CourseAssignments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-205">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="871f3-206">Il `Where` metodo restituisce una raccolta, ma in questo caso i criteri di passati a tale metodo vengono restituiti solo una singola entità Instructor restituita.</span><span class="sxs-lookup"><span data-stu-id="871f3-206">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="871f3-207">Il `Single` metodo converte la raccolta in una singola entità Instructor, che consente di accedere a tale entità `CourseAssignments` proprietà.</span><span class="sxs-lookup"><span data-stu-id="871f3-207">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="871f3-208">Il `CourseAssignments` contiene proprietà `CourseAssignment` entità, da cui si desidera solo correlata `Course` entità.</span><span class="sxs-lookup"><span data-stu-id="871f3-208">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="871f3-209">Utilizzare il `Single` metodo in una raccolta quando si conosce la raccolta ha un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="871f3-209">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="871f3-210">L'unico metodo genera un'eccezione se la raccolta passata a esso è vuota o se è presente più di un elemento.</span><span class="sxs-lookup"><span data-stu-id="871f3-210">The Single method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="871f3-211">In alternativa, è `SingleOrDefault`, che restituisce un valore predefinito (null in questo caso) se la raccolta è vuota.</span><span class="sxs-lookup"><span data-stu-id="871f3-211">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="871f3-212">Tuttavia, in questo caso che ancora comporta un'eccezione (di tentare di trovare un `Courses` proprietà su un riferimento null), e il messaggio di eccezione meno chiaramente indica la causa del problema.</span><span class="sxs-lookup"><span data-stu-id="871f3-212">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="871f3-213">Quando si chiama il `Single` (metodo), è anche possibile passare in Where condizione anziché chiamare il `Where` metodo separatamente:</span><span class="sxs-lookup"><span data-stu-id="871f3-213">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="871f3-214">Invece di:</span><span class="sxs-lookup"><span data-stu-id="871f3-214">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="871f3-215">Successivamente, se è stato selezionato un corso, viene recuperato il corso selezionato dall'elenco dei corsi nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-215">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="871f3-216">Del quindi il modello di visualizzazione `Enrollments` proprietà viene caricata con le entità di registrazione da quel corso `Enrollments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-216">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="871f3-217">Modificare la visualizzazione dell'indice Instructor</span><span class="sxs-lookup"><span data-stu-id="871f3-217">Modify the Instructor Index view</span></span>

<span data-ttu-id="871f3-218">In *Views/Instructors/Index.cshtml*, sostituire il codice del modello con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="871f3-218">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="871f3-219">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="871f3-219">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="871f3-220">Le seguenti modifiche apportate al codice esistente:</span><span class="sxs-lookup"><span data-stu-id="871f3-220">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="871f3-221">Modificare la classe di modello per `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="871f3-221">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="871f3-222">Modificare il titolo della pagina da **indice** a **i docenti**.</span><span class="sxs-lookup"><span data-stu-id="871f3-222">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="871f3-223">Aggiungere un **Office** colonna che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è null.</span><span class="sxs-lookup"><span data-stu-id="871f3-223">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="871f3-224">(Poiché si tratta di una relazione uno-a-zero-o-uno, potrebbe essere presente un'entità correlata di OfficeAssignment.)</span><span class="sxs-lookup"><span data-stu-id="871f3-224">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="871f3-225">Aggiungere un **corsi** invece di colonna che visualizza i corsi per ciascun insegnante.</span><span class="sxs-lookup"><span data-stu-id="871f3-225">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="871f3-226">Vedere [esplicita riga transizione con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) per altre informazioni su questa sintassi razor.</span><span class="sxs-lookup"><span data-stu-id="871f3-226">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="871f3-227">Aggiunta di codice che aggiunge dinamicamente `class="success"` per il `tr` elemento istruttore selezionato.</span><span class="sxs-lookup"><span data-stu-id="871f3-227">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="871f3-228">Consente di impostare un colore di sfondo per la riga selezionata utilizzando una classe di Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="871f3-228">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="871f3-229">Aggiungere un nuovo collegamento ipertestuale con l'etichetta **selezionare** immediatamente prima di altri collegamenti in ogni riga, che comporta l'ID dell'istruttore selezionato da inviare al `Index` metodo.</span><span class="sxs-lookup"><span data-stu-id="871f3-229">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="871f3-230">Eseguire l'app e selezionare il **i docenti** scheda. La pagina Visualizza la proprietà Location di entità OfficeAssignment correlate e una cella vuota della tabella quando è presente alcuna entità OfficeAssignment correlati.</span><span class="sxs-lookup"><span data-stu-id="871f3-230">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Pagina di indice istruttori che non selezionato](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="871f3-232">Nel *Views/Instructors/Index.cshtml* file, dopo la chiusura tabella elemento (alla fine del file), aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="871f3-232">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="871f3-233">Questo codice visualizza un elenco dei corsi relativi a un docente quando è selezionato un docente.</span><span class="sxs-lookup"><span data-stu-id="871f3-233">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="871f3-234">Questo codice legge il `Courses` proprietà del modello di visualizzazione per visualizzare un elenco dei corsi.</span><span class="sxs-lookup"><span data-stu-id="871f3-234">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="871f3-235">Fornisce inoltre un **selezionare** collegamento ipertestuale che invia l'ID del corso selezionato per il `Index` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="871f3-235">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="871f3-236">Aggiornare la pagina e selezionare un docente.</span><span class="sxs-lookup"><span data-stu-id="871f3-236">Refresh the page and select an instructor.</span></span> <span data-ttu-id="871f3-237">È ora possibile visualizzare una griglia che visualizza i corsi assegnati a istruttore selezionato e per ogni corso vedrai il nome del reparto assegnato.</span><span class="sxs-lookup"><span data-stu-id="871f3-237">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Istruttore pagina di indice istruttori selezionato](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="871f3-239">Dopo il blocco di codice che appena aggiunto, aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="871f3-239">After the code block you just added, add the following code.</span></span> <span data-ttu-id="871f3-240">Consente di visualizzare un elenco degli studenti iscritti un corso quando quel corso è selezionata.</span><span class="sxs-lookup"><span data-stu-id="871f3-240">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="871f3-241">Questo codice legge la proprietà le registrazioni del modello di visualizzazione per visualizzare un elenco di studenti iscritti in corso.</span><span class="sxs-lookup"><span data-stu-id="871f3-241">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="871f3-242">Aggiornare di nuovo la pagina e selezionare un docente.</span><span class="sxs-lookup"><span data-stu-id="871f3-242">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="871f3-243">Selezionare quindi un corso per visualizzare l'elenco degli studenti iscritti e i relativi voti.</span><span class="sxs-lookup"><span data-stu-id="871f3-243">Then select a course to see the list of enrolled students and their grades.</span></span>

![Insegnante di pagina di indice istruttori e corso selezionato](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="871f3-245">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="871f3-245">Explicit loading</span></span>

<span data-ttu-id="871f3-246">Quando è stato recuperato l'elenco di istruttori in *InstructorsController.cs*, specificato per il caricamento non differito il `CourseAssignments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-246">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="871f3-247">Si supponga che dovrebbero agli utenti di solo raramente si desidera visualizzare le registrazioni in corso e istruttore selezionato.</span><span class="sxs-lookup"><span data-stu-id="871f3-247">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="871f3-248">In tal caso, si potrebbe voler caricare i dati di registrazione solo se richiesto.</span><span class="sxs-lookup"><span data-stu-id="871f3-248">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="871f3-249">Per visualizzare un esempio di come eseguire il caricamento esplicito, sostituire il `Index` (metodo) con il codice seguente, che rimuove il caricamento immediato per le registrazioni e carica in modo esplicito tale proprietà.</span><span class="sxs-lookup"><span data-stu-id="871f3-249">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="871f3-250">Le modifiche al codice sono evidenziati.</span><span class="sxs-lookup"><span data-stu-id="871f3-250">The code changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="871f3-251">Elimina il nuovo codice di *ThenInclude* chiamate per i dati di registrazione dal codice che consente di recuperare entità instructor.</span><span class="sxs-lookup"><span data-stu-id="871f3-251">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="871f3-252">Se vengono selezionati un docente e corso, il codice evidenziato Recupera entità di registrazione per il corso selezionato e le entità di studenti per ogni registrazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-252">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="871f3-253">Eseguire che l'app, andare alla pagina di indice istruttori ora non si vedrà alcuna differenza di ciò che viene visualizzato nella pagina anche se hai modificato la modalità di recupero dei dati.</span><span class="sxs-lookup"><span data-stu-id="871f3-253">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="871f3-254">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="871f3-254">Summary</span></span>

<span data-ttu-id="871f3-255">Ora utilizzati caricamento eager con una query e con più query per leggere i dati correlati in proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="871f3-255">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="871f3-256">Nella prossima esercitazione verrà illustrato come aggiornare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="871f3-256">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="871f3-257">[Precedente](complex-data-model.md)
>[Successivo](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="871f3-257">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
