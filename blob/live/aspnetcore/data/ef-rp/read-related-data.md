---
title: Pagine Razor con Entity Framework Core - leggere i dati correlati - 6 di 8
author: rick-anderson
description: "In questa esercitazione si leggere e visualizzare i dati correlati, ovvero i dati di Entity Framework carica le proprietà di navigazione."
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: d0cdb5aaa4b1129c3f2404d069e9781ca16260b7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="26651-103">Lettura correlati dati - Core EF con pagine Razor (6 di 8)</span><span class="sxs-lookup"><span data-stu-id="26651-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="26651-104">Da [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="26651-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="26651-105">In questa esercitazione, i dati correlati vengano letto e visualizzati.</span><span class="sxs-lookup"><span data-stu-id="26651-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="26651-106">I dati correlati sono dati EF Core carica le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="26651-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="26651-107">Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="26651-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="26651-108">Le illustrazioni seguenti mostrano le pagine completate per questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="26651-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Pagina di indice corsi](read-related-data/_static/courses-index.png)

![Pagina di indice istruttori](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="26651-111">Caricamento eager esplicito e lazy dei dati correlati</span><span class="sxs-lookup"><span data-stu-id="26651-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="26651-112">Esistono diversi modi EF Core può caricare i dati correlati alle proprietà di navigazione di un'entità:</span><span class="sxs-lookup"><span data-stu-id="26651-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="26651-113">[Caricamento eager](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="26651-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="26651-114">Caricamento eager è quando una query per un tipo di entità anche Carica entità correlate.</span><span class="sxs-lookup"><span data-stu-id="26651-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="26651-115">Quando l'entità è di lettura, vengono recuperati i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="26651-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="26651-116">Ciò comporta in genere una query singola join che recupera tutti i dati necessari.</span><span class="sxs-lookup"><span data-stu-id="26651-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="26651-117">Core EF emetterà più query per alcuni tipi di caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="26651-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="26651-118">Può essere più efficiente rispetto a quanto nel caso di alcune query in EF6 emette più query in cui era presente una singola query.</span><span class="sxs-lookup"><span data-stu-id="26651-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="26651-119">Caricamento eager è specificato con il `Include` e `ThenInclude` metodi.</span><span class="sxs-lookup"><span data-stu-id="26651-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Esempio di caricamento eager](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="26651-121">Caricamento eager invierà più query è incluso un nvavigation raccolta:</span><span class="sxs-lookup"><span data-stu-id="26651-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="26651-122">Una query per la query principale</span><span class="sxs-lookup"><span data-stu-id="26651-122">One query for the main query</span></span> 
 * <span data-ttu-id="26651-123">Una query per ogni raccolta di "margine" della struttura del carico.</span><span class="sxs-lookup"><span data-stu-id="26651-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="26651-124">Separare le query con `Load`: I dati possono essere recuperati nelle query separata e Core EF "corregge" le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="26651-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="26651-125">"correzioni backup" significa che EF Core popola automaticamente le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="26651-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="26651-126">Separare le query con `Load` è più simile Explicit durante il caricamento di caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="26651-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Esempio di query distinte](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="26651-128">Nota: EF Core consente di correggere automaticamente le proprietà di navigazione per qualsiasi altra entità che sono stata caricata in precedenza l'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="26651-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="26651-129">Anche se i dati per una proprietà di navigazione sono *non* inclusi in modo esplicito, la proprietà comunque possibile popolare se alcune o tutte le entità correlate sono state caricate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="26651-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="26651-130">[Caricamento esplicito](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="26651-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="26651-131">Durante la lettura di entità, non recuperare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="26651-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="26651-132">Codice deve essere scritto per recuperare i dati correlati quando necessario.</span><span class="sxs-lookup"><span data-stu-id="26651-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="26651-133">Risultati di caricamento esplicito con query separate in più query inviate al database.</span><span class="sxs-lookup"><span data-stu-id="26651-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="26651-134">Con il caricamento esplicito, il codice specifica le proprietà di navigazione da caricare.</span><span class="sxs-lookup"><span data-stu-id="26651-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="26651-135">Utilizzare il `Load` metodo per eseguire il caricamento esplicito.</span><span class="sxs-lookup"><span data-stu-id="26651-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="26651-136">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="26651-136">For example:</span></span>

 ![Esempio di caricamento esplicito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="26651-138">[Caricamento lazy](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="26651-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="26651-139">[Core EF non supporta attualmente il caricamento lazy](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="26651-139">[EF Core does not currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="26651-140">Durante la lettura di entità, non recuperare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="26651-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="26651-141">La prima volta che si accede a una proprietà di navigazione, i dati necessari per la proprietà di navigazione viene recuperati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="26651-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="26651-142">Viene inviata una query al database ogni volta che una proprietà di navigazione avviene per la prima volta.</span><span class="sxs-lookup"><span data-stu-id="26651-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="26651-143">Il `Select` operatore carica solo i dati correlati necessari.</span><span class="sxs-lookup"><span data-stu-id="26651-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="26651-144">Creare una pagina di corsi che consente di visualizzare il nome di reparto</span><span class="sxs-lookup"><span data-stu-id="26651-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="26651-145">L'entità Course include una proprietà di navigazione che contiene il `Department` entità.</span><span class="sxs-lookup"><span data-stu-id="26651-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="26651-146">Il `Department` entità contiene il reparto che la linea viene assegnata a.</span><span class="sxs-lookup"><span data-stu-id="26651-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="26651-147">Per visualizzare il nome del reparto assegnato in un elenco dei corsi:</span><span class="sxs-lookup"><span data-stu-id="26651-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="26651-148">Ottenere il `Name` proprietà il `Department` entità.</span><span class="sxs-lookup"><span data-stu-id="26651-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="26651-149">Il `Department` entità provengono dal `Course.Department` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="26651-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Reparto](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="26651-151">Lo scaffolding il modello di linea</span><span class="sxs-lookup"><span data-stu-id="26651-151">Scaffold the Course model</span></span>

* <span data-ttu-id="26651-152">Uscire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26651-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="26651-153">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="26651-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="26651-154">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="26651-155">Il comando precedente delle strutture di `Course` modello.</span><span class="sxs-lookup"><span data-stu-id="26651-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="26651-156">Aprire il progetto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26651-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="26651-157">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="26651-157">Build the project.</span></span> <span data-ttu-id="26651-158">La compilazione genera errori simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="26651-159">Una modifica globale `_context.Course` a `_context.Courses` (ovvero, aggiungere una "s" `Course`).</span><span class="sxs-lookup"><span data-stu-id="26651-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="26651-160">7 occorrenze sono disponibili e aggiornate.</span><span class="sxs-lookup"><span data-stu-id="26651-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="26651-161">Aprire *Pages/Courses/Index.cshtml.cs* ed esaminare il `OnGetAsync` metodo.</span><span class="sxs-lookup"><span data-stu-id="26651-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="26651-162">Il motore di scaffolding specificato per il caricamento non differito il `Department` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="26651-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="26651-163">Il `Include` metodo specifica caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="26651-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="26651-164">Eseguire l'app e selezionare il **corsi** collegamento.</span><span class="sxs-lookup"><span data-stu-id="26651-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="26651-165">Visualizza la colonna reparto di `DepartmentID`, che non è utile.</span><span class="sxs-lookup"><span data-stu-id="26651-165">The department column displays the `DepartmentID`, which is not useful.</span></span>

<span data-ttu-id="26651-166">Aggiornare il metodo `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="26651-167">Il codice precedente aggiunge `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="26651-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="26651-168">`AsNoTracking`migliora le prestazioni perché non vengono rilevate le entità restituite.</span><span class="sxs-lookup"><span data-stu-id="26651-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="26651-169">Le entità non vengono rilevate perché non vengono aggiornate nel contesto corrente.</span><span class="sxs-lookup"><span data-stu-id="26651-169">The entities are not tracked because they are not updated in the current context.</span></span>

<span data-ttu-id="26651-170">Aggiornamento *Views/Courses/Index.cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="26651-171">Le seguenti modifiche sono state apportate al codice scaffolding:</span><span class="sxs-lookup"><span data-stu-id="26651-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="26651-172">Modificare il titolo dall'indice ai corsi.</span><span class="sxs-lookup"><span data-stu-id="26651-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="26651-173">Aggiungere un **numero** colonna che mostra il `CourseID` valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="26651-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="26651-174">Per impostazione predefinita, le chiavi primarie non sono scaffolding perché in genere sono utilizzati per gli utenti finali.</span><span class="sxs-lookup"><span data-stu-id="26651-174">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="26651-175">Tuttavia, in questo caso la chiave primaria è significativa.</span><span class="sxs-lookup"><span data-stu-id="26651-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="26651-176">Modificare il **reparto** colonna per visualizzare il nome di reparto.</span><span class="sxs-lookup"><span data-stu-id="26651-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="26651-177">Consente di visualizzare il codice il `Name` proprietà del `Department` entità che viene caricato il `Department` proprietà di navigazione:</span><span class="sxs-lookup"><span data-stu-id="26651-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="26651-178">Eseguire l'app e selezionare il **corsi** scheda per visualizzare l'elenco con i nomi di reparto.</span><span class="sxs-lookup"><span data-stu-id="26651-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Pagina di indice corsi](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="26651-180">Caricamento dati con l'istruzione Select di correlati</span><span class="sxs-lookup"><span data-stu-id="26651-180">Loading related data with Select</span></span>

<span data-ttu-id="26651-181">Il `OnGetAsync` metodo carica i dati correlati con il `Include` metodo:</span><span class="sxs-lookup"><span data-stu-id="26651-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="26651-182">Il `Select` operatore carica solo i dati correlati necessari.</span><span class="sxs-lookup"><span data-stu-id="26651-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="26651-183">Per i singoli elementi, ad esempio il `Department.Name` Usa un SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="26651-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="26651-184">Per le raccolte utilizza un altro accesso al database, ma anche il.`Include`</span><span class="sxs-lookup"><span data-stu-id="26651-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="26651-185">operatore sulle raccolte.</span><span class="sxs-lookup"><span data-stu-id="26651-185">operator on collections.</span></span>

<span data-ttu-id="26651-186">Il codice seguente carica i dati correlati con il `Select` metodo:</span><span class="sxs-lookup"><span data-stu-id="26651-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="26651-187">Il `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="26651-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="26651-188">Vedere [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) per un esempio completo.</span><span class="sxs-lookup"><span data-stu-id="26651-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="26651-189">Creare una pagina istruttori che mostra i corsi e le registrazioni</span><span class="sxs-lookup"><span data-stu-id="26651-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="26651-190">In questa sezione viene creata la pagina istruttori.</span><span class="sxs-lookup"><span data-stu-id="26651-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="26651-191">![Pagina di indice istruttori](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="26651-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="26651-192">Questa pagina legge e visualizza i dati correlati nei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="26651-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="26651-193">L'elenco di istruttori vengono visualizzati i dati correlati di `OfficeAssignment` entità (Office nella figura precedente).</span><span class="sxs-lookup"><span data-stu-id="26651-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="26651-194">Il `Instructor` e `OfficeAssignment` entità si trovano in una relazione uno-a-zero-o-uno.</span><span class="sxs-lookup"><span data-stu-id="26651-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="26651-195">Caricamento eager viene utilizzato per il `OfficeAssignment` entità.</span><span class="sxs-lookup"><span data-stu-id="26651-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="26651-196">Caricamento eager è in genere più efficiente quando i dati correlati devono essere visualizzate.</span><span class="sxs-lookup"><span data-stu-id="26651-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="26651-197">In questo caso, le assegnazioni di office per istruttori vengono visualizzate.</span><span class="sxs-lookup"><span data-stu-id="26651-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="26651-198">Quando l'utente seleziona un docente (Harui nella figura precedente), correlato `Course` vengono visualizzate le entità.</span><span class="sxs-lookup"><span data-stu-id="26651-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="26651-199">Il `Instructor` e `Course` entità si trovano in una relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="26651-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="26651-200">Eager caricamento per il `Course` entità e le relative `Department` viene utilizzata l'entità.</span><span class="sxs-lookup"><span data-stu-id="26651-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="26651-201">In questo caso, query separate potrebbero essere più efficiente perché sono necessari solo i corsi per istruttore selezionato.</span><span class="sxs-lookup"><span data-stu-id="26651-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="26651-202">In questo esempio viene illustrato come utilizzare il caricamento immediato per le proprietà di navigazione di entità nelle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="26651-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="26651-203">Quando l'utente seleziona un corso (chimica nella figura precedente), i dati da correlati di `Enrollments` entità viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="26651-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="26651-204">Nella figura precedente, vengono visualizzati grado e nome di uno studente.</span><span class="sxs-lookup"><span data-stu-id="26651-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="26651-205">Il `Course` e `Enrollment` entità si trovano in una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="26651-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="26651-206">Creare un modello di visualizzazione per la visualizzazione dell'indice Instructor</span><span class="sxs-lookup"><span data-stu-id="26651-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="26651-207">La pagina istruttori Mostra dati di tre tabelle diverse.</span><span class="sxs-lookup"><span data-stu-id="26651-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="26651-208">Viene creato un modello di visualizzazione che include tre entità che rappresenta le tre tabelle.</span><span class="sxs-lookup"><span data-stu-id="26651-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="26651-209">Nel *SchoolViewModels* cartella, creare *InstructorIndexData.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="26651-210">Lo scaffolding modello Instructor</span><span class="sxs-lookup"><span data-stu-id="26651-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="26651-211">Uscire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26651-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="26651-212">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="26651-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="26651-213">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="26651-214">Il comando precedente delle strutture di `Instructor` modello.</span><span class="sxs-lookup"><span data-stu-id="26651-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="26651-215">Aprire il progetto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26651-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="26651-216">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="26651-216">Build the project.</span></span> <span data-ttu-id="26651-217">La compilazione genera errori.</span><span class="sxs-lookup"><span data-stu-id="26651-217">The build generates errors.</span></span>

<span data-ttu-id="26651-218">Una modifica globale `_context.Instructor` a `_context.Instructors` (ovvero, aggiungere una "s" `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="26651-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="26651-219">7 occorrenze sono disponibili e aggiornate.</span><span class="sxs-lookup"><span data-stu-id="26651-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="26651-220">Eseguire l'app e passare alla pagina i docenti.</span><span class="sxs-lookup"><span data-stu-id="26651-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="26651-221">Sostituire *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="26651-222">Il `OnGetAsync` metodo accetta i dati della route facoltativo per l'ID di istruttore selezionato.</span><span class="sxs-lookup"><span data-stu-id="26651-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="26651-223">Esaminare la query nella *Pages/Instructors/Index.cshtml* pagina:</span><span class="sxs-lookup"><span data-stu-id="26651-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="26651-224">La query dispone di due include:</span><span class="sxs-lookup"><span data-stu-id="26651-224">The query has two includes:</span></span>

* <span data-ttu-id="26651-225">`OfficeAssignment`: Visualizzato nel [Visualizza i docenti](#IP).</span><span class="sxs-lookup"><span data-stu-id="26651-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="26651-226">`CourseAssignments`: Porta in corsi tenuti.</span><span class="sxs-lookup"><span data-stu-id="26651-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="26651-227">Aggiornare la pagina di indice istruttori</span><span class="sxs-lookup"><span data-stu-id="26651-227">Update the instructors Index page</span></span>

<span data-ttu-id="26651-228">Aggiornamento *Pages/Instructors/Index.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="26651-229">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="26651-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="26651-230">Gli aggiornamenti di `page` direttiva da `@page` a `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="26651-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="26651-231">`"{id:int?}"`è un modello di route.</span><span class="sxs-lookup"><span data-stu-id="26651-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="26651-232">Le stringhe di query di tipo integer nell'URL viene modificato il modello di route per i dati della route.</span><span class="sxs-lookup"><span data-stu-id="26651-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="26651-233">Ad esempio, facendo clic su di **selezionare** collegamento per un docente quando la direttiva di pagina genera un URL simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="26651-234">Quando la direttiva di pagina è `@page "{id:int?}"`, l'URL precedente è:</span><span class="sxs-lookup"><span data-stu-id="26651-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="26651-235">Titolo della pagina è **i docenti**.</span><span class="sxs-lookup"><span data-stu-id="26651-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="26651-236">Aggiungere un **Office** colonna che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è null.</span><span class="sxs-lookup"><span data-stu-id="26651-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="26651-237">Poiché si tratta di una relazione uno-a-zero-o-uno, potrebbe essere presente un'entità OfficeAssignment correlata.</span><span class="sxs-lookup"><span data-stu-id="26651-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="26651-238">Aggiungere un **corsi** invece di colonna che visualizza i corsi per ciascun insegnante.</span><span class="sxs-lookup"><span data-stu-id="26651-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="26651-239">Vedere [esplicita riga transizione con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) per altre informazioni su questa sintassi razor.</span><span class="sxs-lookup"><span data-stu-id="26651-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="26651-240">Aggiunta di codice che aggiunge dinamicamente `class="success"` per il `tr` elemento istruttore selezionato.</span><span class="sxs-lookup"><span data-stu-id="26651-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="26651-241">Consente di impostare un colore di sfondo per la riga selezionata utilizzando una classe di Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="26651-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="26651-242">Aggiungere un nuovo collegamento ipertestuale con l'etichetta **selezionare**.</span><span class="sxs-lookup"><span data-stu-id="26651-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="26651-243">Questo collegamento invia l'ID dell'istruttore selezionato per il `Index` (metodo) e imposta un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="26651-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="26651-244">Eseguire l'app e selezionare il **i docenti** scheda. Nella pagina vengono visualizzati il `Location` (office) da correlata `OfficeAssignment` entità.</span><span class="sxs-lookup"><span data-stu-id="26651-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="26651-245">Se OfficeAssignment' è null, viene visualizzata una cella vuota della tabella.</span><span class="sxs-lookup"><span data-stu-id="26651-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Pagina di indice istruttori che non selezionato](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="26651-247">Fare clic su di **selezionare** collegamento.</span><span class="sxs-lookup"><span data-stu-id="26651-247">Click on the **Select** link.</span></span> <span data-ttu-id="26651-248">Le modifiche dello stile di riga.</span><span class="sxs-lookup"><span data-stu-id="26651-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="26651-249">Aggiungere i corsi tenuti da istruttore selezionato</span><span class="sxs-lookup"><span data-stu-id="26651-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="26651-250">Aggiornamento di `OnGetAsync` metodo *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="26651-251">Esaminare la query aggiornata:</span><span class="sxs-lookup"><span data-stu-id="26651-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="26651-252">La query precedente aggiunge il `Department` entità.</span><span class="sxs-lookup"><span data-stu-id="26651-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="26651-253">Il codice seguente viene eseguita quando viene selezionato un docente (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="26651-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="26651-254">Istruttore selezionato viene recuperato dall'elenco di istruttori nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="26651-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="26651-255">Il modello di visualizzazione `Courses` proprietà viene caricata con le `Course` entità da tale istruttore `CourseAssignments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="26651-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="26651-256">Il `Where` metodo restituisce una raccolta.</span><span class="sxs-lookup"><span data-stu-id="26651-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="26651-257">Nel passaggio precedente `Where` un solo metodo `Instructor` entità viene restituita.</span><span class="sxs-lookup"><span data-stu-id="26651-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="26651-258">Il `Single` metodo converte la raccolta in un unico `Instructor` entità.</span><span class="sxs-lookup"><span data-stu-id="26651-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="26651-259">Il `Instructor` fornisce l'accesso alle entità di `CourseAssignments` proprietà.</span><span class="sxs-lookup"><span data-stu-id="26651-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="26651-260">`CourseAssignments`fornisce l'accesso a correlata `Course` entità.</span><span class="sxs-lookup"><span data-stu-id="26651-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Per corsi di istruttore m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="26651-262">Il `Single` metodo viene utilizzato in una raccolta per la raccolta contiene un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="26651-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="26651-263">Il `Single` metodo genera un'eccezione se la raccolta è vuota o se è presente più di un elemento.</span><span class="sxs-lookup"><span data-stu-id="26651-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="26651-264">In alternativa, è `SingleOrDefault`, che restituisce un valore predefinito (null in questo caso) se la raccolta è vuota.</span><span class="sxs-lookup"><span data-stu-id="26651-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="26651-265">Utilizzando `SingleOrDefault` su una raccolta vuota:</span><span class="sxs-lookup"><span data-stu-id="26651-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="26651-266">Genera un'eccezione (di tentare di trovare un `Courses` proprietà su un riferimento null).</span><span class="sxs-lookup"><span data-stu-id="26651-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="26651-267">Il messaggio di eccezione meno chiaramente indica la causa del problema.</span><span class="sxs-lookup"><span data-stu-id="26651-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="26651-268">Il codice seguente consente di popolare il modello di visualizzazione `Enrollments` proprietà quando è selezionato un corso:</span><span class="sxs-lookup"><span data-stu-id="26651-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="26651-269">Aggiungere il markup seguente alla fine del *Pages/Courses/Index.cshtml* Razor pagina:</span><span class="sxs-lookup"><span data-stu-id="26651-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="26651-270">Il markup precedente visualizza un elenco dei corsi relativi a un docente quando è selezionato un docente.</span><span class="sxs-lookup"><span data-stu-id="26651-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="26651-271">Testare l'app.</span><span class="sxs-lookup"><span data-stu-id="26651-271">Test the app.</span></span> <span data-ttu-id="26651-272">Fare clic su un **selezionare** collegamento nella pagina i docenti.</span><span class="sxs-lookup"><span data-stu-id="26651-272">Click on a **Select** link on the instructors page.</span></span>

![Istruttore pagina di indice istruttori selezionato](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="26651-274">Mostra i dati degli studenti</span><span class="sxs-lookup"><span data-stu-id="26651-274">Show student data</span></span>

<span data-ttu-id="26651-275">In questa sezione, l'app viene aggiornata per mostrare i dati di studenti per un corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="26651-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="26651-276">Aggiornare la query nella `OnGetAsync` metodo *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="26651-277">Aggiornamento *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="26651-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="26651-278">Aggiungere il markup seguente alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="26651-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="26651-279">Il markup precedente visualizza un elenco degli studenti iscritti in corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="26651-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="26651-280">Aggiornare la pagina e selezionare un docente.</span><span class="sxs-lookup"><span data-stu-id="26651-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="26651-281">Selezionare un corso per visualizzare l'elenco degli studenti iscritti e i relativi voti.</span><span class="sxs-lookup"><span data-stu-id="26651-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Insegnante di pagina di indice istruttori e corso selezionato](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="26651-283">Utilizzo singolo</span><span class="sxs-lookup"><span data-stu-id="26651-283">Using Single</span></span>

<span data-ttu-id="26651-284">Il `Single` metodo può passare il `Where` condizione anziché chiamare il `Where` metodo separatamente:</span><span class="sxs-lookup"><span data-stu-id="26651-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="26651-285">Precedente `Single` approccio non offre alcun vantaggio rispetto all'uso `Where`.</span><span class="sxs-lookup"><span data-stu-id="26651-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="26651-286">Alcuni sviluppatori preferiscono la `Single` approccio stile.</span><span class="sxs-lookup"><span data-stu-id="26651-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="26651-287">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="26651-287">Explicit loading</span></span>

<span data-ttu-id="26651-288">Il codice corrente specifica per il caricamento non differito `Enrollments` e `Students`:</span><span class="sxs-lookup"><span data-stu-id="26651-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="26651-289">Si supponga che gli utenti desiderano raramente vedere le registrazioni in un corso.</span><span class="sxs-lookup"><span data-stu-id="26651-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="26651-290">In tal caso, un'ottimizzazione sarebbe per caricare solo i dati di registrazione, se richiesto.</span><span class="sxs-lookup"><span data-stu-id="26651-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="26651-291">In questa sezione, il `OnGetAsync` viene aggiornato per utilizzare il caricamento esplicito di `Enrollments` e `Students`.</span><span class="sxs-lookup"><span data-stu-id="26651-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="26651-292">Aggiornamento di `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="26651-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="26651-293">Elimina il codice precedente il *ThenInclude* chiamate per i dati di registrazione e gli studenti.</span><span class="sxs-lookup"><span data-stu-id="26651-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="26651-294">Se si seleziona un corso, recupera il codice evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="26651-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="26651-295">Il `Enrollment` entità per il corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="26651-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="26651-296">Il `Student` entità per ogni `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="26651-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="26651-297">Si noti il precedente codice commento `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="26651-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="26651-298">Le proprietà di navigazione possono solo essere caricate in modo esplicito per le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="26651-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="26651-299">Testare l'app.</span><span class="sxs-lookup"><span data-stu-id="26651-299">Test the app.</span></span> <span data-ttu-id="26651-300">Dal punto di vista degli utenti, l'app si comporta in modo identico alla versione precedente.</span><span class="sxs-lookup"><span data-stu-id="26651-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="26651-301">L'esercitazione successiva viene illustrato come aggiornare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="26651-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="26651-302">[Precedente](xref:data/ef-rp/complex-data-model)
>[Successivo](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="26651-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
