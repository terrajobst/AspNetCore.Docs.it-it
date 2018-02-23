---
title: Razor Pages con EF Core - Leggere dati correlati - 6 di 8
author: rick-anderson
description: "In questa esercitazione verranno letti e visualizzati dati correlati, ovvero dati che Entity Framework carica all'interno delle proprietà di navigazione."
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ccb1e95ae2b43fd0a4c4b1ac9ed58a4d474ab3b6
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="a2f80-103">Lettura di dati correlati - Razor Pages con EF Core (6 di 8)</span><span class="sxs-lookup"><span data-stu-id="a2f80-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="a2f80-104">Di [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a2f80-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="a2f80-105">In questa esercitazione vengono letti e visualizzati dati correlati.</span><span class="sxs-lookup"><span data-stu-id="a2f80-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="a2f80-106">I dati correlati sono dati che EF Core carica all'interno delle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a2f80-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="a2f80-107">Se si verificano problemi che non si è in grado di risolvere, scaricare l'[app completa per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="a2f80-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="a2f80-108">Le figure seguenti mostrano le pagine completate per questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="a2f80-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Pagina di indice dei corsi](read-related-data/_static/courses-index.png)

![Pagina di indice degli insegnanti](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="a2f80-111">Caricamento eager, esplicito e lazy dei dati correlati</span><span class="sxs-lookup"><span data-stu-id="a2f80-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="a2f80-112">Esistono diversi modi con cui EF Core può caricare i dati correlati nelle proprietà di navigazione di un'entità:</span><span class="sxs-lookup"><span data-stu-id="a2f80-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="a2f80-113">[Caricamento eager](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="a2f80-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="a2f80-114">Il caricamento eager si verifica quando una query per un solo tipo di entità carica anche entità correlate.</span><span class="sxs-lookup"><span data-stu-id="a2f80-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="a2f80-115">Quando l'entità viene letta, vengono recuperati i dati correlati corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="a2f80-116">Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari.</span><span class="sxs-lookup"><span data-stu-id="a2f80-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="a2f80-117">Per alcuni tipi di caricamento eager, EF Core genera più query.</span><span class="sxs-lookup"><span data-stu-id="a2f80-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="a2f80-118">La generazione di più query può essere più efficiente rispetto al caso di alcune query in EF6, in cui era presente una singola query.</span><span class="sxs-lookup"><span data-stu-id="a2f80-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="a2f80-119">Il caricamento eager viene specificato con i metodi `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Esempio di caricamento eager](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="a2f80-121">Il caricamento eager invia più query quando è inclusa una navigazione di raccolte:</span><span class="sxs-lookup"><span data-stu-id="a2f80-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="a2f80-122">Una query per la query principale</span><span class="sxs-lookup"><span data-stu-id="a2f80-122">One query for the main query</span></span> 
 * <span data-ttu-id="a2f80-123">Una query per ogni raccolta "perimetrale" nell'albero del caricamento.</span><span class="sxs-lookup"><span data-stu-id="a2f80-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="a2f80-124">Query separate con `Load`: i dati possono essere recuperati in query separate ed EF Core "corregge" le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a2f80-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="a2f80-125">"Corregge" significa che EF Core popola automaticamente le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a2f80-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="a2f80-126">Le query separate con `Load` sono più simili a un caricamento esplicito che a un caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="a2f80-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Esempio di query separate](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="a2f80-128">Nota: EF Core corregge automaticamente le proprietà di navigazione per qualsiasi altra entità caricata in precedenza nell'istanza contesto.</span><span class="sxs-lookup"><span data-stu-id="a2f80-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="a2f80-129">Anche se i dati per una proprietà di navigazione *non* sono inclusi in modo esplicito, la proprietà può comunque essere popolata se alcune o tutte le entità correlate sono state caricate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="a2f80-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="a2f80-130">[Caricamento esplicito](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading)</span><span class="sxs-lookup"><span data-stu-id="a2f80-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="a2f80-131">Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati.</span><span class="sxs-lookup"><span data-stu-id="a2f80-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="a2f80-132">Per recuperare i dati correlati quando necessario, è necessario scrivere codice.</span><span class="sxs-lookup"><span data-stu-id="a2f80-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="a2f80-133">Il caricamento esplicito con query separate ha come risultato l'invio di più query al database.</span><span class="sxs-lookup"><span data-stu-id="a2f80-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="a2f80-134">Con il caricamento esplicito, il codice specifica le proprietà di navigazione da caricare.</span><span class="sxs-lookup"><span data-stu-id="a2f80-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="a2f80-135">Per eseguire il caricamento esplicito, usare il metodo `Load`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="a2f80-136">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2f80-136">For example:</span></span>

 ![Esempio di caricamento esplicito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="a2f80-138">[Caricamento lazy](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="a2f80-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="a2f80-139">[EF Core attualmente non supporta il caricamento lazy](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="a2f80-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="a2f80-140">Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati.</span><span class="sxs-lookup"><span data-stu-id="a2f80-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="a2f80-141">La prima volta che si accede a una proprietà di navigazione, i dati necessari per quest'ultima vengono recuperati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="a2f80-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="a2f80-142">Ogni volta che si accede a una proprietà di navigazione per la prima volta, viene inviata una query al database.</span><span class="sxs-lookup"><span data-stu-id="a2f80-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="a2f80-143">L'operatore `Select` carica solo i dati correlati necessari.</span><span class="sxs-lookup"><span data-stu-id="a2f80-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="a2f80-144">Creare una pagina Courses (Corsi) che visualizza il nome dei dipartimenti</span><span class="sxs-lookup"><span data-stu-id="a2f80-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="a2f80-145">L'entità Course include una proprietà di navigazione che contiene l'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="a2f80-146">L'entità `Department` contiene il dipartimento a cui il corso è assegnato.</span><span class="sxs-lookup"><span data-stu-id="a2f80-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="a2f80-147">Per visualizzare il nome del dipartimento assegnato in un elenco dei corsi:</span><span class="sxs-lookup"><span data-stu-id="a2f80-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="a2f80-148">Ottenere la proprietà `Name` dall'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="a2f80-149">L'entità `Department` proviene dalla proprietà di navigazione `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="a2f80-151">Scaffolding del modello Course</span><span class="sxs-lookup"><span data-stu-id="a2f80-151">Scaffold the Course model</span></span>

* <span data-ttu-id="a2f80-152">Uscire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2f80-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="a2f80-153">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="a2f80-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a2f80-154">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="a2f80-155">Il comando precedente esegue lo scaffolding del modello `Course`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="a2f80-156">Aprire il progetto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2f80-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="a2f80-157">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="a2f80-157">Build the project.</span></span> <span data-ttu-id="a2f80-158">La compilazione genera errori simili al seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="a2f80-159">Convertire globalmente `_context.Course` in `_context.Courses` (ovvero aggiungere una "s" a `Course`).</span><span class="sxs-lookup"><span data-stu-id="a2f80-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="a2f80-160">Vengono trovate e aggiornate sette occorrenze.</span><span class="sxs-lookup"><span data-stu-id="a2f80-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="a2f80-161">Aprire *Pages/Courses/Index.cshtml.cs* ed esaminare il metodo `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="a2f80-162">Il motore di scaffolding ha specificato il caricamento eager per la proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="a2f80-163">Il metodo `Include` specifica il caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="a2f80-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="a2f80-164">Eseguire l'app e selezionare il collegamento **Courses** (Corsi).</span><span class="sxs-lookup"><span data-stu-id="a2f80-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="a2f80-165">La colonna dei dipartimenti visualizza il `DepartmentID`, che non è utile.</span><span class="sxs-lookup"><span data-stu-id="a2f80-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="a2f80-166">Aggiornare il metodo `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="a2f80-167">Il codice precedente aggiunge `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="a2f80-168">`AsNoTracking` migliora le prestazioni, perché le entità restituite non vengono registrate,</span><span class="sxs-lookup"><span data-stu-id="a2f80-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="a2f80-169">dato che non vengono aggiornate nel contesto corrente.</span><span class="sxs-lookup"><span data-stu-id="a2f80-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="a2f80-170">Aggiornare *Views/Courses/Index.cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="a2f80-171">Al codice con scaffolding sono state apportate le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="a2f80-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="a2f80-172">Il titolo è stato modificato da Index (Indice) a Courses (Corsi).</span><span class="sxs-lookup"><span data-stu-id="a2f80-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="a2f80-173">È stata aggiunta la colonna **Number** (Numero) con il valore della proprietà `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="a2f80-174">Per impostazione predefinita, non viene eseguito lo scaffolding delle chiavi primarie, perché in genere non sono significative per gli utenti finali.</span><span class="sxs-lookup"><span data-stu-id="a2f80-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="a2f80-175">In questo caso, tuttavia, la chiave primaria è significativa.</span><span class="sxs-lookup"><span data-stu-id="a2f80-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="a2f80-176">Modificare la colonna **Department** (Dipartimento) per visualizzare il nome del dipartimento.</span><span class="sxs-lookup"><span data-stu-id="a2f80-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="a2f80-177">Il codice visualizza la proprietà `Name` dell'entità `Department` che viene caricata nella proprietà di navigazione `Department`:</span><span class="sxs-lookup"><span data-stu-id="a2f80-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="a2f80-178">Eseguire l'app e selezionare la scheda **Courses** (Corsi) per visualizzare l'elenco con i nomi dei dipartimenti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Pagina di indice dei corsi](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="a2f80-180">Caricamento di dati correlati con Select</span><span class="sxs-lookup"><span data-stu-id="a2f80-180">Loading related data with Select</span></span>

<span data-ttu-id="a2f80-181">Il metodo `OnGetAsync` carica i dati correlati con il metodo `Include`:</span><span class="sxs-lookup"><span data-stu-id="a2f80-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="a2f80-182">L'operatore `Select` carica solo i dati correlati necessari.</span><span class="sxs-lookup"><span data-stu-id="a2f80-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="a2f80-183">Per singoli elementi, ad esempio per `Department.Name` usa un INNER JOIN SQL.</span><span class="sxs-lookup"><span data-stu-id="a2f80-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="a2f80-184">Per le raccolte, usa un altro accesso al database, ma anche l'operatore `Include`</span><span class="sxs-lookup"><span data-stu-id="a2f80-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="a2f80-185">esegue la stessa operazione sulle raccolte.</span><span class="sxs-lookup"><span data-stu-id="a2f80-185">operator on collections.</span></span>

<span data-ttu-id="a2f80-186">Il codice seguente carica dati correlati con il metodo `Select`:</span><span class="sxs-lookup"><span data-stu-id="a2f80-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="a2f80-187">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="a2f80-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="a2f80-188">Per un esempio completo, vedere [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="a2f80-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="a2f80-189">Creare una pagina Instructors (Insegnanti) che mostri i corsi e le iscrizioni</span><span class="sxs-lookup"><span data-stu-id="a2f80-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="a2f80-190">In questa sezione viene creata la pagina Instructors (Insegnanti).</span><span class="sxs-lookup"><span data-stu-id="a2f80-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="a2f80-191">![Pagina di indice degli insegnanti](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="a2f80-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="a2f80-192">Questa pagina legge e visualizza dati correlati nei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a2f80-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="a2f80-193">L'elenco di insegnanti visualizza dati correlati provenienti dall'entità `OfficeAssignment`, Office (Ufficio) nella figura precedente.</span><span class="sxs-lookup"><span data-stu-id="a2f80-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="a2f80-194">Tra le entità `Instructor` e `OfficeAssignment` c'è una relazione uno-a-zero-o-uno.</span><span class="sxs-lookup"><span data-stu-id="a2f80-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="a2f80-195">Per le entità `OfficeAssignment` viene usato il caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="a2f80-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="a2f80-196">Il caricamento eager è in genere più efficiente quando i dati correlati devono essere visualizzati.</span><span class="sxs-lookup"><span data-stu-id="a2f80-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="a2f80-197">In questo caso, devono essere visualizzate le assegnazioni di ufficio degli insegnanti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="a2f80-198">Quando l'utente seleziona un insegnante (Harui nella figura precedente), vengono visualizzate le entità `Course` correlate.</span><span class="sxs-lookup"><span data-stu-id="a2f80-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="a2f80-199">Tra le entità `Instructor` e `Course` esiste una relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="a2f80-200">Per le entità `Course` e le entità `Department` corrispondenti viene usato il caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="a2f80-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="a2f80-201">In questo caso, query separate potrebbero essere più efficienti, perché sono necessari solo i corsi per l'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="a2f80-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="a2f80-202">Questo esempio illustra come usare il caricamento eager per le proprietà di navigazione di entità all'interno di proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a2f80-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="a2f80-203">Quando l'utente seleziona un corso (Chemistry (Chimica) nella figura precedente), vengono visualizzati i dati correlati dell'entità `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="a2f80-204">Nella figura precedente, vengono visualizzati il voto e il nome degli studenti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="a2f80-205">Tra le entità `Course` e `Enrollment` esiste una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="a2f80-206">Creare un modello per la visualizzazione dell'indice degli insegnanti</span><span class="sxs-lookup"><span data-stu-id="a2f80-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="a2f80-207">La pagina Instructors (Insegnanti) mostra i dati di tre tabelle diverse.</span><span class="sxs-lookup"><span data-stu-id="a2f80-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="a2f80-208">Viene creato un modello di visualizzazione che include le tre entità che rappresentano le tre tabelle.</span><span class="sxs-lookup"><span data-stu-id="a2f80-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="a2f80-209">Nella cartella *SchoolViewModels* creare *InstructorIndexData.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="a2f80-210">Scaffolding del modello Instructor</span><span class="sxs-lookup"><span data-stu-id="a2f80-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="a2f80-211">Uscire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2f80-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="a2f80-212">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="a2f80-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a2f80-213">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="a2f80-214">Il comando precedente esegue lo scaffolding del modello `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="a2f80-215">Aprire il progetto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2f80-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="a2f80-216">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="a2f80-216">Build the project.</span></span> <span data-ttu-id="a2f80-217">La compilazione genera errori.</span><span class="sxs-lookup"><span data-stu-id="a2f80-217">The build generates errors.</span></span>

<span data-ttu-id="a2f80-218">Convertire globalmente `_context.Instructor` in `_context.Instructors` (ovvero aggiungere una "s" a `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="a2f80-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="a2f80-219">Vengono trovate e aggiornate sette occorrenze.</span><span class="sxs-lookup"><span data-stu-id="a2f80-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="a2f80-220">Eseguire l'app e passare alla pagina Instructors (Insegnanti).</span><span class="sxs-lookup"><span data-stu-id="a2f80-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="a2f80-221">Sostituire *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="a2f80-222">Il metodo `OnGetAsync` accetta i dati di route facoltativi per l'ID dell'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="a2f80-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="a2f80-223">Esaminare la query nella pagina *Pages/Instructors/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a2f80-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="a2f80-224">La query ha due istruzioni Include, che riguardano:</span><span class="sxs-lookup"><span data-stu-id="a2f80-224">The query has two includes:</span></span>

* <span data-ttu-id="a2f80-225">`OfficeAssignment`: visualizzato nella [visualizzazione degli insegnanti](#IP).</span><span class="sxs-lookup"><span data-stu-id="a2f80-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="a2f80-226">`CourseAssignments`: visualizza i corsi tenuti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="a2f80-227">Aggiornare la pagina di indice degli insegnanti</span><span class="sxs-lookup"><span data-stu-id="a2f80-227">Update the instructors Index page</span></span>

<span data-ttu-id="a2f80-228">Aggiornare *Pages/Instructors/Index.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="a2f80-229">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="a2f80-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="a2f80-230">Aggiorna la direttiva `page` da `@page` a `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="a2f80-231">`"{id:int?}"` è un modello di route.</span><span class="sxs-lookup"><span data-stu-id="a2f80-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="a2f80-232">Il modello di route cambia le stringhe di query di tipo integer nell'URL in dati di route.</span><span class="sxs-lookup"><span data-stu-id="a2f80-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="a2f80-233">Se ad esempio si fa clic su sul collegamento **Select** (Seleziona) per un insegnante, la direttiva della pagina genera un URL come il seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="a2f80-234">Quando la direttiva della pagina è `@page "{id:int?}"`, l'URL precedente è:</span><span class="sxs-lookup"><span data-stu-id="a2f80-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="a2f80-235">Il titolo pagina è **Instructors** (Insegnanti).</span><span class="sxs-lookup"><span data-stu-id="a2f80-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="a2f80-236">È stata aggiunta la colonna **Office** (Ufficio) che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è Null.</span><span class="sxs-lookup"><span data-stu-id="a2f80-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="a2f80-237">Poiché questa è una relazione uno-a-zero-o-uno, potrebbe non esserci un'entità OfficeAssignment correlata.</span><span class="sxs-lookup"><span data-stu-id="a2f80-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="a2f80-238">È stata aggiunta la colonna **Courses** (Corsi) che visualizza i corsi tenuti da ogni insegnante.</span><span class="sxs-lookup"><span data-stu-id="a2f80-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="a2f80-239">Per altre informazioni su questa sintassi Razor, vedere [Transizione riga esplicita con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) .</span><span class="sxs-lookup"><span data-stu-id="a2f80-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="a2f80-240">È stato aggiunto codice che aggiunge `class="success"` in modo dinamico all'elemento `tr` dell'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="a2f80-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="a2f80-241">In questo modo viene impostato un colore di sfondo per la riga selezionata tramite una classe Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="a2f80-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="a2f80-242">È stato aggiunto un nuovo collegamento ipertestuale con etichetta **Select** (Seleziona).</span><span class="sxs-lookup"><span data-stu-id="a2f80-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="a2f80-243">Questo collegamento invia l'ID dell'insegnante selezionato al metodo `Index` e imposta un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="a2f80-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="a2f80-244">Eseguire l'app e selezionare la scheda **Instructors** (Insegnanti). La pagina visualizza `Location` (Ufficio) dall'entità `OfficeAssignment` correlata.</span><span class="sxs-lookup"><span data-stu-id="a2f80-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="a2f80-245">Se OfficeAssignment è Null, nella tabella viene visualizzata una cella vuota.</span><span class="sxs-lookup"><span data-stu-id="a2f80-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Pagina di indice degli insegnanti con nessuna selezione](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="a2f80-247">Fare clic sul collegamento **Select** (Seleziona).</span><span class="sxs-lookup"><span data-stu-id="a2f80-247">Click on the **Select** link.</span></span> <span data-ttu-id="a2f80-248">Lo stile delle righe cambia.</span><span class="sxs-lookup"><span data-stu-id="a2f80-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="a2f80-249">Aggiungere corsi tenuti dall'insegnante selezionato</span><span class="sxs-lookup"><span data-stu-id="a2f80-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="a2f80-250">Aggiornare il metodo `OnGetAsync` in *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="a2f80-251">Esaminare la query aggiornata:</span><span class="sxs-lookup"><span data-stu-id="a2f80-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="a2f80-252">La query precedente aggiunge l'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="a2f80-253">Il codice seguente viene eseguito quando viene selezionato un insegnante (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="a2f80-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="a2f80-254">L'insegnante selezionato viene recuperato dall'elenco di insegnanti nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="a2f80-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="a2f80-255">La proprietà `Courses` del modello di visualizzazione viene caricata con le entità `Course` dalla proprietà di navigazione `CourseAssignments` di tale insegnante.</span><span class="sxs-lookup"><span data-stu-id="a2f80-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="a2f80-256">Il metodo `Where` restituisce una raccolta.</span><span class="sxs-lookup"><span data-stu-id="a2f80-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="a2f80-257">Nel metodo `Where` precedente viene restituita una sola entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="a2f80-258">Il metodo `Single` converte la raccolta in un'unica entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="a2f80-259">L'entità `Instructor` consente l'accesso alla proprietà `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="a2f80-260">`CourseAssignments` consente l'accesso alle entità `Course` correlate.</span><span class="sxs-lookup"><span data-stu-id="a2f80-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Relazione m:M tra insegnante e corsi](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="a2f80-262">Il metodo `Single` viene usato in una raccolta quando quest'ultima ha un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="a2f80-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="a2f80-263">Il metodo `Single` genera un'eccezione se la raccolta è vuota o se contiene più elementi.</span><span class="sxs-lookup"><span data-stu-id="a2f80-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="a2f80-264">In alternativa, è possibile usare `SingleOrDefault`, che restituisce un valore predefinito (Null in questo caso) se la raccolta è vuota.</span><span class="sxs-lookup"><span data-stu-id="a2f80-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="a2f80-265">L'uso di `SingleOrDefault` per una raccolta vuota:</span><span class="sxs-lookup"><span data-stu-id="a2f80-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="a2f80-266">Genera un'eccezione, a causa del tentativo di cercare la proprietà `Courses` in un riferimento Null.</span><span class="sxs-lookup"><span data-stu-id="a2f80-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="a2f80-267">Il messaggio di eccezione indica meno chiaramente la causa del problema.</span><span class="sxs-lookup"><span data-stu-id="a2f80-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="a2f80-268">Il codice seguente popola la proprietà `Enrollments` del modello di visualizzazione quando è selezionato un corso:</span><span class="sxs-lookup"><span data-stu-id="a2f80-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="a2f80-269">Aggiungere il markup seguente alla fine della pagina Razor *Pages/Courses/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a2f80-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="a2f80-270">Quando è selezionato un insegnante, il markup precedente visualizza un elenco dei corsi correlati all'insegnante stesso.</span><span class="sxs-lookup"><span data-stu-id="a2f80-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="a2f80-271">Eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="a2f80-271">Test the app.</span></span> <span data-ttu-id="a2f80-272">Fare clic su un collegamento **Select** (Seleziona) nella pagina Instructors (Insegnanti).</span><span class="sxs-lookup"><span data-stu-id="a2f80-272">Click on a **Select** link on the instructors page.</span></span>

![Pagina di indice degli insegnanti con un insegnante selezionato](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="a2f80-274">Visualizzare i dati degli studenti</span><span class="sxs-lookup"><span data-stu-id="a2f80-274">Show student data</span></span>

<span data-ttu-id="a2f80-275">In questa sezione, l'app viene aggiornata in modo da visualizzare i dati degli studenti per il corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="a2f80-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="a2f80-276">Aggiornare la query nel metodo `OnGetAsync` in *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="a2f80-277">Aggiornare *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a2f80-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="a2f80-278">Aggiungere il markup seguente alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="a2f80-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="a2f80-279">Il markup precedente visualizza l'elenco degli studenti iscritti al corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="a2f80-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="a2f80-280">Aggiornare la pagina e selezionare un insegnante.</span><span class="sxs-lookup"><span data-stu-id="a2f80-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="a2f80-281">Selezionare un corso per visualizzare l'elenco degli studenti iscritti e i voti corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Pagina di indice degli insegnanti con un corso selezionato](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="a2f80-283">Usare Single</span><span class="sxs-lookup"><span data-stu-id="a2f80-283">Using Single</span></span>

<span data-ttu-id="a2f80-284">Il metodo `Single` può passare la condizione `Where` anziché chiamare il metodo `Where` separatamente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="a2f80-285">L'approccio `Single` precedente non offre alcun vantaggio rispetto all'uso di `Where`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="a2f80-286">Alcuni sviluppatori preferiscono lo stile dell'approccio `Single`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="a2f80-287">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="a2f80-287">Explicit loading</span></span>

<span data-ttu-id="a2f80-288">Il codice corrente specifica il caricamento eager per `Enrollments` e `Students`:</span><span class="sxs-lookup"><span data-stu-id="a2f80-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="a2f80-289">Si supponga che gli utenti richiedano raramente la visualizzazione delle iscrizioni a un corso.</span><span class="sxs-lookup"><span data-stu-id="a2f80-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="a2f80-290">In questo caso, per ottimizzare il funzionamento dell'app è utile caricare i dati delle iscrizioni solo se vengono richiesti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="a2f80-291">In questa sezione, viene eseguito l'aggiornamento di `OnGetAsync` in modo da usare il caricamento esplicito di `Enrollments` e `Students`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="a2f80-292">Aggiornare `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a2f80-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="a2f80-293">Il codice precedente rilascia le chiamate del metodo *ThenInclude* per i dati delle iscrizioni e degli studenti.</span><span class="sxs-lookup"><span data-stu-id="a2f80-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="a2f80-294">Se è selezionato un corso, il codice evidenziato recupera:</span><span class="sxs-lookup"><span data-stu-id="a2f80-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="a2f80-295">Le entità `Enrollment` per il corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="a2f80-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="a2f80-296">Le entità `Student` per ogni entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="a2f80-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="a2f80-297">Si noti che nel codice precedente `.AsNoTracking()` è commentato.</span><span class="sxs-lookup"><span data-stu-id="a2f80-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="a2f80-298">Le proprietà di navigazione possono solo essere caricate in modo esplicito per le entità registrate.</span><span class="sxs-lookup"><span data-stu-id="a2f80-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="a2f80-299">Eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="a2f80-299">Test the app.</span></span> <span data-ttu-id="a2f80-300">Dal punto di vista degli utenti, l'app si comporta in modo identico alla versione precedente.</span><span class="sxs-lookup"><span data-stu-id="a2f80-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="a2f80-301">La prossima esercitazione illustra come aggiornare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="a2f80-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="a2f80-302">[Precedente](xref:data/ef-rp/complex-data-model)
>[Successivo](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="a2f80-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
