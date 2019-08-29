---
title: Razor Pages con EF Core in ASP.NET Core - Leggere dati correlati - 6 di 8
author: tdykstra
description: In questa esercitazione verranno letti e visualizzati dati correlati, ovvero dati che Entity Framework carica all'interno delle proprietà di navigazione.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 62224312aa9b7f3e0164b5300e491f59b0832acd
ms.sourcegitcommit: 776598f71da0d1e4c9e923b3b395d3c3b5825796
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/26/2019
ms.locfileid: "70024727"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="9640e-103">Razor Pages con EF Core in ASP.NET Core - Leggere dati correlati - 6 di 8</span><span class="sxs-lookup"><span data-stu-id="9640e-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="9640e-104">Di [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9640e-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9640e-105">Questa esercitazione illustra come leggere e visualizzare dati correlati.</span><span class="sxs-lookup"><span data-stu-id="9640e-105">This tutorial shows how to read and display related data.</span></span> <span data-ttu-id="9640e-106">I dati correlati sono dati che EF Core carica all'interno delle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="9640e-107">Le figure seguenti mostrano le pagine completate per questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="9640e-107">The following illustrations show the completed pages for this tutorial:</span></span>

![Pagina di indice dei corsi](read-related-data/_static/courses-index30.png)

![Pagina di indice degli insegnanti](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a><span data-ttu-id="9640e-110">Caricamento eager, esplicito e lazy</span><span class="sxs-lookup"><span data-stu-id="9640e-110">Eager, explicit, and lazy loading</span></span>

<span data-ttu-id="9640e-111">Esistono diversi modi con cui EF Core può caricare i dati correlati nelle proprietà di navigazione di un'entità:</span><span class="sxs-lookup"><span data-stu-id="9640e-111">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="9640e-112">[Caricamento eager](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="9640e-112">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="9640e-113">Il caricamento eager si verifica quando una query per un solo tipo di entità carica anche entità correlate.</span><span class="sxs-lookup"><span data-stu-id="9640e-113">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="9640e-114">Quando viene letta un'entità, vengono recuperati i dati correlati corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-114">When an entity is read, its related data is retrieved.</span></span> <span data-ttu-id="9640e-115">Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari.</span><span class="sxs-lookup"><span data-stu-id="9640e-115">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="9640e-116">Per alcuni tipi di caricamento eager, EF Core genera più query.</span><span class="sxs-lookup"><span data-stu-id="9640e-116">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="9640e-117">L'esecuzione di più query può essere più efficiente rispetto a una singola query gigante.</span><span class="sxs-lookup"><span data-stu-id="9640e-117">Issuing multiple queries can be more efficient than a giant single query.</span></span> <span data-ttu-id="9640e-118">Il caricamento eager viene specificato con i metodi `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="9640e-118">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Esempio di caricamento eager](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="9640e-120">Il caricamento eager invia più query quando è inclusa una navigazione di raccolte:</span><span class="sxs-lookup"><span data-stu-id="9640e-120">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="9640e-121">Una query per la query principale</span><span class="sxs-lookup"><span data-stu-id="9640e-121">One query for the main query</span></span> 
  * <span data-ttu-id="9640e-122">Una query per ogni raccolta "perimetrale" nell'albero del caricamento.</span><span class="sxs-lookup"><span data-stu-id="9640e-122">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="9640e-123">Query separate con `Load`: i dati possono essere recuperati in query separate ed EF Core "corregge" le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-123">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="9640e-124">"Corregge" significa che EF Core popola automaticamente le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-124">"Fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="9640e-125">Le query separate con `Load` sono più simili a un caricamento esplicito che a un caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="9640e-125">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Esempio di query separate](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="9640e-127">Nota: EF Core corregge automaticamente le proprietà di navigazione per qualsiasi altra entità caricata in precedenza nell'istanza di contesto.</span><span class="sxs-lookup"><span data-stu-id="9640e-127">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="9640e-128">Anche se i dati per una proprietà di navigazione *non* sono inclusi in modo esplicito, la proprietà può comunque essere popolata se alcune o tutte le entità correlate sono state caricate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9640e-128">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="9640e-129">[Caricamento esplicito](/ef/core/querying/related-data#explicit-loading)</span><span class="sxs-lookup"><span data-stu-id="9640e-129">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="9640e-130">Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati.</span><span class="sxs-lookup"><span data-stu-id="9640e-130">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="9640e-131">Per recuperare i dati correlati quando necessario, è necessario scrivere codice.</span><span class="sxs-lookup"><span data-stu-id="9640e-131">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="9640e-132">Il caricamento esplicito con query separate ha come risultato l'invio di più query al database.</span><span class="sxs-lookup"><span data-stu-id="9640e-132">Explicit loading with separate queries results in multiple queries sent to the database.</span></span> <span data-ttu-id="9640e-133">Con il caricamento esplicito, il codice specifica le proprietà di navigazione da caricare.</span><span class="sxs-lookup"><span data-stu-id="9640e-133">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="9640e-134">Per eseguire il caricamento esplicito, usare il metodo `Load`.</span><span class="sxs-lookup"><span data-stu-id="9640e-134">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="9640e-135">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9640e-135">For example:</span></span>

  ![Esempio di caricamento esplicito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="9640e-137">[Caricamento lazy](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="9640e-137">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="9640e-138">[Il caricamento lazy è stato aggiunto a EF Core nella versione 2.1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="9640e-138">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="9640e-139">Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati.</span><span class="sxs-lookup"><span data-stu-id="9640e-139">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="9640e-140">La prima volta che si accede a una proprietà di navigazione, i dati necessari per quest'ultima vengono recuperati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9640e-140">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="9640e-141">Ogni volta che si accede a una proprietà di navigazione per la prima volta, viene inviata una query al database.</span><span class="sxs-lookup"><span data-stu-id="9640e-141">A query is sent to the database each time a navigation property is accessed for the first time.</span></span>

## <a name="create-course-pages"></a><span data-ttu-id="9640e-142">Creare pagine Course</span><span class="sxs-lookup"><span data-stu-id="9640e-142">Create Course pages</span></span>

<span data-ttu-id="9640e-143">L'entità `Course` include una proprietà di navigazione che contiene l'entità `Department` correlata.</span><span class="sxs-lookup"><span data-stu-id="9640e-143">The `Course` entity includes a navigation property that contains the related `Department` entity.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<span data-ttu-id="9640e-145">Per visualizzare il nome del dipartimento assegnato per un corso:</span><span class="sxs-lookup"><span data-stu-id="9640e-145">To display the name of the assigned department for a course:</span></span>

* <span data-ttu-id="9640e-146">Caricare l'entità `Department` correlata nella proprietà di navigazione `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="9640e-146">Load the related `Department` entity into the `Course.Department` navigation property.</span></span>
* <span data-ttu-id="9640e-147">Ottenere il nome dalla proprietà `Name` dell'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="9640e-147">Get the name from the `Department` entity's `Name` property.</span></span>

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a><span data-ttu-id="9640e-148">Scaffolding delle pagine Course</span><span class="sxs-lookup"><span data-stu-id="9640e-148">Scaffold Course pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9640e-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9640e-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9640e-150">Seguire le istruzioni in [Scaffolding delle pagine Student](xref:data/ef-rp/intro#scaffold-student-pages) con le eccezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9640e-150">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="9640e-151">Creare una cartella *Pages/Courses*.</span><span class="sxs-lookup"><span data-stu-id="9640e-151">Create a *Pages/Courses* folder.</span></span>
  * <span data-ttu-id="9640e-152">Usare `Course` per la classe del modello.</span><span class="sxs-lookup"><span data-stu-id="9640e-152">Use `Course` for the model class.</span></span>
  * <span data-ttu-id="9640e-153">Usare la classe di contesto esistente anziché crearne una nuova.</span><span class="sxs-lookup"><span data-stu-id="9640e-153">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9640e-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9640e-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9640e-155">Creare una cartella *Pages/Courses*.</span><span class="sxs-lookup"><span data-stu-id="9640e-155">Create a *Pages/Courses* folder.</span></span>

* <span data-ttu-id="9640e-156">Eseguire il comando seguente per eseguire lo scaffolding delle pagine Course.</span><span class="sxs-lookup"><span data-stu-id="9640e-156">Run the following command to scaffold the Course pages.</span></span>

  <span data-ttu-id="9640e-157">**In Windows:**</span><span class="sxs-lookup"><span data-stu-id="9640e-157">**On Windows:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  <span data-ttu-id="9640e-158">**In Linux o macOS:**</span><span class="sxs-lookup"><span data-stu-id="9640e-158">**On Linux or macOS:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* <span data-ttu-id="9640e-159">Aprire *Pages/Courses/Index.cshtml.cs* ed esaminare il metodo `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="9640e-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="9640e-160">Il motore di scaffolding ha specificato il caricamento eager per la proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="9640e-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="9640e-161">Il metodo `Include` specifica il caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="9640e-161">The `Include` method specifies eager loading.</span></span>

* <span data-ttu-id="9640e-162">Eseguire l'app e selezionare il collegamento **Courses** (Corsi).</span><span class="sxs-lookup"><span data-stu-id="9640e-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="9640e-163">La colonna dei dipartimenti visualizza il `DepartmentID`, che non è utile.</span><span class="sxs-lookup"><span data-stu-id="9640e-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

### <a name="display-the-department-name"></a><span data-ttu-id="9640e-164">Visualizzare il nome del dipartimento</span><span class="sxs-lookup"><span data-stu-id="9640e-164">Display the department name</span></span>

<span data-ttu-id="9640e-165">Aggiornare Pages/Courses/Index.cshtml.cs con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-165">Update Pages/Courses/Index.cshtml.cs with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

<span data-ttu-id="9640e-166">Il codice precedente modifica la proprietà `Course` in `Courses` e aggiunge `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="9640e-166">The preceding code changes the `Course` property to `Courses` and adds `AsNoTracking`.</span></span> <span data-ttu-id="9640e-167">`AsNoTracking` migliora le prestazioni, perché le entità restituite non vengono registrate,</span><span class="sxs-lookup"><span data-stu-id="9640e-167">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="9640e-168">dato che non vengono aggiornate nel contesto corrente.</span><span class="sxs-lookup"><span data-stu-id="9640e-168">The entities don't need to be tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="9640e-169">Aggiornare *Pages/Courses/Index.cshtml* con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9640e-169">Update *Pages/Courses/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

<span data-ttu-id="9640e-170">Al codice con scaffolding sono state apportate le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="9640e-170">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="9640e-171">Il nome della proprietà `Course` è stato modificato in `Courses`.</span><span class="sxs-lookup"><span data-stu-id="9640e-171">Changed the `Course` property name to `Courses`.</span></span>
* <span data-ttu-id="9640e-172">È stata aggiunta la colonna **Number** (Numero) con il valore della proprietà `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="9640e-172">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="9640e-173">Per impostazione predefinita, non viene eseguito lo scaffolding delle chiavi primarie, perché in genere non sono significative per gli utenti finali.</span><span class="sxs-lookup"><span data-stu-id="9640e-173">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="9640e-174">In questo caso, tuttavia, la chiave primaria è significativa.</span><span class="sxs-lookup"><span data-stu-id="9640e-174">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="9640e-175">Modificare la colonna **Department** (Dipartimento) per visualizzare il nome del dipartimento.</span><span class="sxs-lookup"><span data-stu-id="9640e-175">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="9640e-176">Il codice visualizza la proprietà `Name` dell'entità `Department` che viene caricata nella proprietà di navigazione `Department`:</span><span class="sxs-lookup"><span data-stu-id="9640e-176">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="9640e-177">Eseguire l'app e selezionare la scheda **Courses** (Corsi) per visualizzare l'elenco con i nomi dei dipartimenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-177">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Pagina di indice dei corsi](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="9640e-179">Caricamento di dati correlati con Select</span><span class="sxs-lookup"><span data-stu-id="9640e-179">Loading related data with Select</span></span>

<span data-ttu-id="9640e-180">Il metodo `OnGetAsync` carica i dati correlati con il metodo `Include`.</span><span class="sxs-lookup"><span data-stu-id="9640e-180">The `OnGetAsync` method loads related data with the `Include` method.</span></span> <span data-ttu-id="9640e-181">Il metodo `Select` è un'alternativa che carica solo i dati correlati necessari.</span><span class="sxs-lookup"><span data-stu-id="9640e-181">The `Select` method is an alternative that loads only the related data needed.</span></span> <span data-ttu-id="9640e-182">Per singoli elementi, ad esempio per `Department.Name` usa un INNER JOIN SQL.</span><span class="sxs-lookup"><span data-stu-id="9640e-182">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="9640e-183">Per le raccolte usa un altro accesso al database, ma anche l'operatore `Include` fa lo stesso sulle raccolte.</span><span class="sxs-lookup"><span data-stu-id="9640e-183">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="9640e-184">Il codice seguente carica dati correlati con il metodo `Select`:</span><span class="sxs-lookup"><span data-stu-id="9640e-184">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

<span data-ttu-id="9640e-185">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="9640e-185">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="9640e-186">Per un esempio completo, vedere [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="9640e-186">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-instructor-pages"></a><span data-ttu-id="9640e-187">Creare pagine Instructor</span><span class="sxs-lookup"><span data-stu-id="9640e-187">Create Instructor pages</span></span>

<span data-ttu-id="9640e-188">Questa sezione descrive come eseguire lo scaffolding delle pagine Instructor e come aggiungere corsi e iscrizioni correlati alla pagina di indice degli insegnanti.</span><span class="sxs-lookup"><span data-stu-id="9640e-188">This section scaffolds Instructor pages and adds related Courses and Enrollments to the Instructors Index page.</span></span>

<a name="IP"></a>
<span data-ttu-id="9640e-189">![Pagina di indice degli insegnanti](read-related-data/_static/instructors-index30.png)</span><span class="sxs-lookup"><span data-stu-id="9640e-189">![Instructors Index page](read-related-data/_static/instructors-index30.png)</span></span>

<span data-ttu-id="9640e-190">Questa pagina legge e visualizza dati correlati nei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9640e-190">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="9640e-191">L'elenco di insegnanti visualizza dati correlati provenienti dall'entità `OfficeAssignment`, Office (Ufficio) nella figura precedente.</span><span class="sxs-lookup"><span data-stu-id="9640e-191">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="9640e-192">Tra le entità `Instructor` e `OfficeAssignment` c'è una relazione uno-a-zero-o-uno.</span><span class="sxs-lookup"><span data-stu-id="9640e-192">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="9640e-193">Per le entità `OfficeAssignment` viene usato il caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="9640e-193">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="9640e-194">Il caricamento eager è in genere più efficiente quando i dati correlati devono essere visualizzati.</span><span class="sxs-lookup"><span data-stu-id="9640e-194">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="9640e-195">In questo caso, devono essere visualizzate le assegnazioni di ufficio degli insegnanti.</span><span class="sxs-lookup"><span data-stu-id="9640e-195">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="9640e-196">Quando l'utente seleziona un insegnante, vengono visualizzate le entità `Course` correlate.</span><span class="sxs-lookup"><span data-stu-id="9640e-196">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="9640e-197">Tra le entità `Instructor` e `Course` esiste una relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="9640e-197">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="9640e-198">Il caricamento eager viene usato per le entità `Course` e le entità `Department` correlate.</span><span class="sxs-lookup"><span data-stu-id="9640e-198">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="9640e-199">In questo caso, query separate potrebbero essere più efficienti, perché sono necessari solo i corsi per l'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-199">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="9640e-200">Questo esempio illustra come usare il caricamento eager per le proprietà di navigazione di entità all'interno di proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-200">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="9640e-201">Quando l'utente seleziona un corso, vengono visualizzati i dati correlati dall'entità `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="9640e-201">When the user selects a course, related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="9640e-202">Nella figura precedente, vengono visualizzati il voto e il nome degli studenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-202">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="9640e-203">Tra le entità `Course` e `Enrollment` esiste una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="9640e-203">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model"></a><span data-ttu-id="9640e-204">Creare un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="9640e-204">Create a view model</span></span>

<span data-ttu-id="9640e-205">La pagina Instructors (Insegnanti) mostra i dati di tre tabelle diverse.</span><span class="sxs-lookup"><span data-stu-id="9640e-205">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="9640e-206">È necessario un modello di visualizzazione che includa tre proprietà che rappresentano le tre tabelle.</span><span class="sxs-lookup"><span data-stu-id="9640e-206">A view model is needed that includes three properties representing the three tables.</span></span>

<span data-ttu-id="9640e-207">Creare *SchoolViewModels/InstructorIndexData.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-207">Create *SchoolViewModels/InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a><span data-ttu-id="9640e-208">Scaffolding delle pagine Instructor</span><span class="sxs-lookup"><span data-stu-id="9640e-208">Scaffold Instructor pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9640e-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9640e-209">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9640e-210">Seguire le istruzioni in [Scaffolding delle pagine Student](xref:data/ef-rp/intro#scaffold-student-pages) con le eccezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9640e-210">Follow the instructions in [Scaffold the student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="9640e-211">Creare una cartella *Pages/Instructors*.</span><span class="sxs-lookup"><span data-stu-id="9640e-211">Create a *Pages/Instructors* folder.</span></span>
  * <span data-ttu-id="9640e-212">Usare `Instructor` per la classe del modello.</span><span class="sxs-lookup"><span data-stu-id="9640e-212">Use `Instructor` for the model class.</span></span>
  * <span data-ttu-id="9640e-213">Usare la classe di contesto esistente anziché crearne una nuova.</span><span class="sxs-lookup"><span data-stu-id="9640e-213">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9640e-214">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9640e-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9640e-215">Creare una cartella *Pages/Instructors*.</span><span class="sxs-lookup"><span data-stu-id="9640e-215">Create a *Pages/Instructors* folder.</span></span>

* <span data-ttu-id="9640e-216">Eseguire il comando seguente per eseguire lo scaffolding delle pagine Instructor.</span><span class="sxs-lookup"><span data-stu-id="9640e-216">Run the following command to scaffold the Instructor pages.</span></span>

  <span data-ttu-id="9640e-217">**In Windows:**</span><span class="sxs-lookup"><span data-stu-id="9640e-217">**On Windows:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  <span data-ttu-id="9640e-218">**In Linux o macOS:**</span><span class="sxs-lookup"><span data-stu-id="9640e-218">**On Linux or macOS:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="9640e-219">Per visualizzare l'aspetto della pagina con scaffolding prima di aggiornarla, eseguire l'app e passare alla pagina Instructors.</span><span class="sxs-lookup"><span data-stu-id="9640e-219">To see what the scaffolded page looks like before you update it, run the app and navigate to the Instructors page.</span></span>

<span data-ttu-id="9640e-220">Sostituire *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-220">Update *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

<span data-ttu-id="9640e-221">Il metodo `OnGetAsync` accetta i dati di route facoltativi per l'ID dell'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="9640e-222">Esaminare la query nel file *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9640e-222">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

<span data-ttu-id="9640e-223">Il codice specifica il caricamento eager delle proprietà di navigazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="9640e-223">The code specifies eager loading for the following navigation properties:</span></span>

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

<span data-ttu-id="9640e-224">Si noti la ripetizione dei metodi `Include` e `ThenInclude` per `CourseAssignments` e `Course`.</span><span class="sxs-lookup"><span data-stu-id="9640e-224">Notice the repetition of `Include` and `ThenInclude` methods for `CourseAssignments` and `Course`.</span></span> <span data-ttu-id="9640e-225">Questa ripetizione è necessaria per specificare il caricamento eager per due proprietà di navigazione dell'entità `Course`.</span><span class="sxs-lookup"><span data-stu-id="9640e-225">This repetition is necessary to specify eager loading for two navigation properties of the `Course` entity.</span></span>

<span data-ttu-id="9640e-226">Il codice seguente viene eseguito quando viene selezionato un insegnante (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="9640e-226">The following code executes when an instructor is selected (`id != null`).</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

<span data-ttu-id="9640e-227">L'insegnante selezionato viene recuperato dall'elenco di insegnanti nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-227">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="9640e-228">La proprietà `Courses` del modello di visualizzazione viene caricata con le entità `Course` dalla proprietà di navigazione `CourseAssignments` di tale insegnante.</span><span class="sxs-lookup"><span data-stu-id="9640e-228">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="9640e-229">Il metodo `Where` restituisce una raccolta.</span><span class="sxs-lookup"><span data-stu-id="9640e-229">The `Where` method returns a collection.</span></span> <span data-ttu-id="9640e-230">Tuttavia, in questo caso, il filtro selezionerà una singola entità.</span><span class="sxs-lookup"><span data-stu-id="9640e-230">But in this case, the filter will select a single entity.</span></span> <span data-ttu-id="9640e-231">Il metodo `Single` viene quindi chiamato per convertire la raccolta in una singola entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="9640e-231">so the `Single` method is called to convert the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="9640e-232">L'entità `Instructor` consente l'accesso alla proprietà `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="9640e-232">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="9640e-233">`CourseAssignments` consente l'accesso alle entità `Course` correlate.</span><span class="sxs-lookup"><span data-stu-id="9640e-233">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Relazione m:M tra insegnante e corsi](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="9640e-235">Il metodo `Single` viene usato in una raccolta quando quest'ultima ha un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="9640e-235">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="9640e-236">Il metodo `Single` genera un'eccezione se la raccolta è vuota o se contiene più elementi.</span><span class="sxs-lookup"><span data-stu-id="9640e-236">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="9640e-237">In alternativa, è possibile usare `SingleOrDefault`, che restituisce un valore predefinito (Null in questo caso) se la raccolta è vuota.</span><span class="sxs-lookup"><span data-stu-id="9640e-237">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span>

<span data-ttu-id="9640e-238">Il codice seguente popola la proprietà `Enrollments` del modello di visualizzazione quando è selezionato un corso:</span><span class="sxs-lookup"><span data-stu-id="9640e-238">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="9640e-239">Aggiornare la pagina di indice degli insegnanti</span><span class="sxs-lookup"><span data-stu-id="9640e-239">Update the instructors Index page</span></span>

<span data-ttu-id="9640e-240">Aggiornare *Pages/Instructors/Index.cshtml* con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9640e-240">Update *Pages/Instructors/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

<span data-ttu-id="9640e-241">Il codice precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="9640e-241">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="9640e-242">Aggiorna la direttiva `page` da `@page` a `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="9640e-242">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="9640e-243">`"{id:int?}"` è un modello di route.</span><span class="sxs-lookup"><span data-stu-id="9640e-243">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="9640e-244">Il modello di route cambia le stringhe di query di tipo integer nell'URL in dati di route.</span><span class="sxs-lookup"><span data-stu-id="9640e-244">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="9640e-245">Se ad esempio si fa clic su sul collegamento **Select** (Seleziona) per un insegnante con la sola direttiva `@page`, viene generato un URL come il seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-245">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `https://localhost:5001/Instructors?id=2`

  <span data-ttu-id="9640e-246">Quando la direttiva della pagina è `@page "{id:int?}"`, l'URL è:</span><span class="sxs-lookup"><span data-stu-id="9640e-246">When the page directive is `@page "{id:int?}"`, the URL is:</span></span>

  `https://localhost:5001/Instructors/2`

* <span data-ttu-id="9640e-247">Aggiunge una colonna **Office** che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è Null.</span><span class="sxs-lookup"><span data-stu-id="9640e-247">Adds an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="9640e-248">Poiché questa è una relazione uno-a-zero-o-uno, potrebbe non esserci un'entità OfficeAssignment correlata.</span><span class="sxs-lookup"><span data-stu-id="9640e-248">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="9640e-249">Aggiunge una colonna **Courses** che visualizza i corsi tenuti da ogni insegnante.</span><span class="sxs-lookup"><span data-stu-id="9640e-249">Adds a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="9640e-250">Per altre informazioni su questa sintassi Razor, vedere [Transizione riga esplicita con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) .</span><span class="sxs-lookup"><span data-stu-id="9640e-250">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="9640e-251">Aggiunge codice che aggiunge `class="success"` in modo dinamico all'elemento `tr` dell'insegnante e del corso selezionati.</span><span class="sxs-lookup"><span data-stu-id="9640e-251">Adds code that dynamically adds `class="success"` to the `tr` element of the selected instructor and course.</span></span> <span data-ttu-id="9640e-252">In questo modo viene impostato un colore di sfondo per la riga selezionata tramite una classe Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="9640e-252">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="9640e-253">Aggiunge un nuovo collegamento ipertestuale con etichetta **Select**.</span><span class="sxs-lookup"><span data-stu-id="9640e-253">Adds a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="9640e-254">Questo collegamento invia l'ID dell'insegnante selezionato al metodo `Index` e imposta un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="9640e-254">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* <span data-ttu-id="9640e-255">Aggiunge una tabella di corsi per l'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-255">Adds a table of courses for the selected Instructor.</span></span>

* <span data-ttu-id="9640e-256">Aggiunge una tabella di iscrizioni degli studenti per il corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-256">Adds a table of student enrollments for the selected course.</span></span>

<span data-ttu-id="9640e-257">Eseguire l'app e selezionare la scheda **Instructors** (Insegnanti). La pagina visualizza `Location` (Ufficio) dall'entità `OfficeAssignment` correlata.</span><span class="sxs-lookup"><span data-stu-id="9640e-257">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="9640e-258">Se `OfficeAssignment` è null, viene visualizzata una cella di tabella vuota.</span><span class="sxs-lookup"><span data-stu-id="9640e-258">If `OfficeAssignment` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="9640e-259">Fare clic sul collegamento **Select** per un insegnante.</span><span class="sxs-lookup"><span data-stu-id="9640e-259">Click on the **Select** link for an instructor.</span></span> <span data-ttu-id="9640e-260">Verranno visualizzate le modifiche dello stile delle righe e i corsi assegnati a tale insegnante.</span><span class="sxs-lookup"><span data-stu-id="9640e-260">The row style changes and courses assigned to that instructor are displayed.</span></span>

<span data-ttu-id="9640e-261">Selezionare un corso per visualizzare l'elenco degli studenti iscritti e i voti corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-261">Select a course to see the list of enrolled students and their grades.</span></span>

![Pagina di indice degli insegnanti con un corso selezionato](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a><span data-ttu-id="9640e-263">Usare Single</span><span class="sxs-lookup"><span data-stu-id="9640e-263">Using Single</span></span>

<span data-ttu-id="9640e-264">Il metodo `Single` può passare la condizione `Where` anziché chiamare il metodo `Where` separatamente:</span><span class="sxs-lookup"><span data-stu-id="9640e-264">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="9640e-265">L'uso di `Single` con una condizione Where è una questione di preferenza personale.</span><span class="sxs-lookup"><span data-stu-id="9640e-265">Use of `Single` with a Where condition is a matter of personal preference.</span></span> <span data-ttu-id="9640e-266">Non offre alcun vantaggio rispetto all'uso del metodo `Where`.</span><span class="sxs-lookup"><span data-stu-id="9640e-266">It provides no benefits over using the `Where` method.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="9640e-267">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="9640e-267">Explicit loading</span></span>

<span data-ttu-id="9640e-268">Il codice corrente specifica il caricamento eager per `Enrollments` e `Students`:</span><span class="sxs-lookup"><span data-stu-id="9640e-268">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

<span data-ttu-id="9640e-269">Si supponga che gli utenti richiedano raramente la visualizzazione delle iscrizioni a un corso.</span><span class="sxs-lookup"><span data-stu-id="9640e-269">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="9640e-270">In questo caso, per ottimizzare il funzionamento dell'app è utile caricare i dati delle iscrizioni solo se vengono richiesti.</span><span class="sxs-lookup"><span data-stu-id="9640e-270">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="9640e-271">In questa sezione, viene eseguito l'aggiornamento di `OnGetAsync` in modo da usare il caricamento esplicito di `Enrollments` e `Students`.</span><span class="sxs-lookup"><span data-stu-id="9640e-271">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="9640e-272">Aggiornare *Pages/Instructors/Index.cshtml.cs* con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9640e-272">Update *Pages/Instructors/Index.cshtml.cs* with the following code.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

<span data-ttu-id="9640e-273">Il codice precedente rilascia le chiamate del metodo *ThenInclude* per i dati delle iscrizioni e degli studenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-273">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="9640e-274">Se è selezionato un corso, il codice di caricamento esplicito recupera:</span><span class="sxs-lookup"><span data-stu-id="9640e-274">If a course is selected, the explicit loading code retrieves:</span></span>

* <span data-ttu-id="9640e-275">Le entità `Enrollment` per il corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-275">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="9640e-276">Le entità `Student` per ogni entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="9640e-276">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="9640e-277">Si noti che nel codice precedente `.AsNoTracking()` è impostato come commento.</span><span class="sxs-lookup"><span data-stu-id="9640e-277">Notice that the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="9640e-278">Le proprietà di navigazione possono solo essere caricate in modo esplicito per le entità registrate.</span><span class="sxs-lookup"><span data-stu-id="9640e-278">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="9640e-279">Eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="9640e-279">Test the app.</span></span> <span data-ttu-id="9640e-280">Dal punto di vista dell'utente, l'app si comporta in modo identico alla versione precedente.</span><span class="sxs-lookup"><span data-stu-id="9640e-280">From a user's perspective, the app behaves identically to the previous version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9640e-281">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="9640e-281">Next steps</span></span>

<span data-ttu-id="9640e-282">La prossima esercitazione illustra come aggiornare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="9640e-282">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="9640e-283">[Esercitazione precedente](xref:data/ef-rp/complex-data-model)
>[Esercitazione successiva](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="9640e-283">[Previous tutorial](xref:data/ef-rp/complex-data-model)
[Next tutorial](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9640e-284">In questa esercitazione vengono letti e visualizzati dati correlati.</span><span class="sxs-lookup"><span data-stu-id="9640e-284">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="9640e-285">I dati correlati sono dati che EF Core carica all'interno delle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-285">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="9640e-286">Se si verificano problemi che non si è in grado di risolvere, [scaricare o visualizzare l'app completa](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="9640e-286">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="9640e-287">[Istruzioni per il download](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="9640e-287">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="9640e-288">Le figure seguenti mostrano le pagine completate per questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="9640e-288">The following illustrations show the completed pages for this tutorial:</span></span>

![Pagina di indice dei corsi](read-related-data/_static/courses-index.png)

![Pagina di indice degli insegnanti](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="9640e-291">Caricamento eager, esplicito e lazy dei dati correlati</span><span class="sxs-lookup"><span data-stu-id="9640e-291">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="9640e-292">Esistono diversi modi con cui EF Core può caricare i dati correlati nelle proprietà di navigazione di un'entità:</span><span class="sxs-lookup"><span data-stu-id="9640e-292">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="9640e-293">[Caricamento eager](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="9640e-293">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="9640e-294">Il caricamento eager si verifica quando una query per un solo tipo di entità carica anche entità correlate.</span><span class="sxs-lookup"><span data-stu-id="9640e-294">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="9640e-295">Quando l'entità viene letta, vengono recuperati i dati correlati corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-295">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="9640e-296">Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari.</span><span class="sxs-lookup"><span data-stu-id="9640e-296">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="9640e-297">Per alcuni tipi di caricamento eager, EF Core genera più query.</span><span class="sxs-lookup"><span data-stu-id="9640e-297">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="9640e-298">La generazione di più query può essere più efficiente rispetto al caso di alcune query in EF6, in cui era presente una singola query.</span><span class="sxs-lookup"><span data-stu-id="9640e-298">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="9640e-299">Il caricamento eager viene specificato con i metodi `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="9640e-299">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Esempio di caricamento eager](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="9640e-301">Il caricamento eager invia più query quando è inclusa una navigazione di raccolte:</span><span class="sxs-lookup"><span data-stu-id="9640e-301">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="9640e-302">Una query per la query principale</span><span class="sxs-lookup"><span data-stu-id="9640e-302">One query for the main query</span></span> 
  * <span data-ttu-id="9640e-303">Una query per ogni raccolta "perimetrale" nell'albero del caricamento.</span><span class="sxs-lookup"><span data-stu-id="9640e-303">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="9640e-304">Query separate con `Load`: i dati possono essere recuperati in query separate ed EF Core "corregge" le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-304">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="9640e-305">"Corregge" significa che EF Core popola automaticamente le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-305">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="9640e-306">Le query separate con `Load` sono più simili a un caricamento esplicito che a un caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="9640e-306">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Esempio di query separate](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="9640e-308">Nota: EF Core corregge automaticamente le proprietà di navigazione per qualsiasi altra entità caricata in precedenza nell'istanza di contesto.</span><span class="sxs-lookup"><span data-stu-id="9640e-308">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="9640e-309">Anche se i dati per una proprietà di navigazione *non* sono inclusi in modo esplicito, la proprietà può comunque essere popolata se alcune o tutte le entità correlate sono state caricate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9640e-309">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="9640e-310">[Caricamento esplicito](/ef/core/querying/related-data#explicit-loading)</span><span class="sxs-lookup"><span data-stu-id="9640e-310">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="9640e-311">Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati.</span><span class="sxs-lookup"><span data-stu-id="9640e-311">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="9640e-312">Per recuperare i dati correlati quando necessario, è necessario scrivere codice.</span><span class="sxs-lookup"><span data-stu-id="9640e-312">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="9640e-313">Il caricamento esplicito con query separate ha come risultato l'invio di più query al database.</span><span class="sxs-lookup"><span data-stu-id="9640e-313">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="9640e-314">Con il caricamento esplicito, il codice specifica le proprietà di navigazione da caricare.</span><span class="sxs-lookup"><span data-stu-id="9640e-314">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="9640e-315">Per eseguire il caricamento esplicito, usare il metodo `Load`.</span><span class="sxs-lookup"><span data-stu-id="9640e-315">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="9640e-316">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9640e-316">For example:</span></span>

  ![Esempio di caricamento esplicito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="9640e-318">[Caricamento lazy](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="9640e-318">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="9640e-319">[Il caricamento lazy è stato aggiunto a EF Core nella versione 2.1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="9640e-319">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="9640e-320">Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati.</span><span class="sxs-lookup"><span data-stu-id="9640e-320">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="9640e-321">La prima volta che si accede a una proprietà di navigazione, i dati necessari per quest'ultima vengono recuperati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9640e-321">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="9640e-322">Ogni volta che si accede a una proprietà di navigazione per la prima volta, viene inviata una query al database.</span><span class="sxs-lookup"><span data-stu-id="9640e-322">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="9640e-323">L'operatore `Select` carica solo i dati correlati necessari.</span><span class="sxs-lookup"><span data-stu-id="9640e-323">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="9640e-324">Creare una pagina Course che visualizza il nome dei dipartimenti</span><span class="sxs-lookup"><span data-stu-id="9640e-324">Create a Course page that displays department name</span></span>

<span data-ttu-id="9640e-325">L'entità Course include una proprietà di navigazione che contiene l'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="9640e-325">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="9640e-326">L'entità `Department` contiene il dipartimento a cui il corso è assegnato.</span><span class="sxs-lookup"><span data-stu-id="9640e-326">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="9640e-327">Per visualizzare il nome del dipartimento assegnato in un elenco dei corsi:</span><span class="sxs-lookup"><span data-stu-id="9640e-327">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="9640e-328">Ottenere la proprietà `Name` dall'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="9640e-328">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="9640e-329">L'entità `Department` proviene dalla proprietà di navigazione `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="9640e-329">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="9640e-331">Scaffolding del modello Course</span><span class="sxs-lookup"><span data-stu-id="9640e-331">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9640e-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9640e-332">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="9640e-333">Seguire le istruzioni in [Eseguire lo scaffolding del modello Student (Studente)](xref:data/ef-rp/intro#scaffold-the-student-model) e usare `Course` per la classe modello.</span><span class="sxs-lookup"><span data-stu-id="9640e-333">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9640e-334">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9640e-334">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="9640e-335">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-335">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="9640e-336">Il comando precedente esegue lo scaffolding del modello `Course`.</span><span class="sxs-lookup"><span data-stu-id="9640e-336">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="9640e-337">Aprire il progetto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9640e-337">Open the project in Visual Studio.</span></span>

<span data-ttu-id="9640e-338">Aprire *Pages/Courses/Index.cshtml.cs* ed esaminare il metodo `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="9640e-338">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="9640e-339">Il motore di scaffolding ha specificato il caricamento eager per la proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="9640e-339">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="9640e-340">Il metodo `Include` specifica il caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="9640e-340">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="9640e-341">Eseguire l'app e selezionare il collegamento **Courses** (Corsi).</span><span class="sxs-lookup"><span data-stu-id="9640e-341">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="9640e-342">La colonna dei dipartimenti visualizza il `DepartmentID`, che non è utile.</span><span class="sxs-lookup"><span data-stu-id="9640e-342">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="9640e-343">Aggiornare il metodo `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-343">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="9640e-344">Il codice precedente aggiunge `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="9640e-344">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="9640e-345">`AsNoTracking` migliora le prestazioni, perché le entità restituite non vengono registrate,</span><span class="sxs-lookup"><span data-stu-id="9640e-345">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="9640e-346">dato che non vengono aggiornate nel contesto corrente.</span><span class="sxs-lookup"><span data-stu-id="9640e-346">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="9640e-347">Aggiornare *Pages/Courses/Index.cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-347">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="9640e-348">Al codice con scaffolding sono state apportate le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="9640e-348">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="9640e-349">Il titolo è stato modificato da Index (Indice) a Courses (Corsi).</span><span class="sxs-lookup"><span data-stu-id="9640e-349">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="9640e-350">È stata aggiunta la colonna **Number** (Numero) con il valore della proprietà `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="9640e-350">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="9640e-351">Per impostazione predefinita, non viene eseguito lo scaffolding delle chiavi primarie, perché in genere non sono significative per gli utenti finali.</span><span class="sxs-lookup"><span data-stu-id="9640e-351">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="9640e-352">In questo caso, tuttavia, la chiave primaria è significativa.</span><span class="sxs-lookup"><span data-stu-id="9640e-352">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="9640e-353">Modificare la colonna **Department** (Dipartimento) per visualizzare il nome del dipartimento.</span><span class="sxs-lookup"><span data-stu-id="9640e-353">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="9640e-354">Il codice visualizza la proprietà `Name` dell'entità `Department` che viene caricata nella proprietà di navigazione `Department`:</span><span class="sxs-lookup"><span data-stu-id="9640e-354">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="9640e-355">Eseguire l'app e selezionare la scheda **Courses** (Corsi) per visualizzare l'elenco con i nomi dei dipartimenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-355">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Pagina di indice dei corsi](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="9640e-357">Caricamento di dati correlati con Select</span><span class="sxs-lookup"><span data-stu-id="9640e-357">Loading related data with Select</span></span>

<span data-ttu-id="9640e-358">Il metodo `OnGetAsync` carica i dati correlati con il metodo `Include`:</span><span class="sxs-lookup"><span data-stu-id="9640e-358">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="9640e-359">L'operatore `Select` carica solo i dati correlati necessari.</span><span class="sxs-lookup"><span data-stu-id="9640e-359">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="9640e-360">Per singoli elementi, ad esempio per `Department.Name` usa un INNER JOIN SQL.</span><span class="sxs-lookup"><span data-stu-id="9640e-360">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="9640e-361">Per le raccolte usa un altro accesso al database, ma anche l'operatore `Include` fa lo stesso sulle raccolte.</span><span class="sxs-lookup"><span data-stu-id="9640e-361">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="9640e-362">Il codice seguente carica dati correlati con il metodo `Select`:</span><span class="sxs-lookup"><span data-stu-id="9640e-362">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="9640e-363">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="9640e-363">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="9640e-364">Per un esempio completo, vedere [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="9640e-364">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="9640e-365">Creare una pagina Instructors (Insegnanti) che mostri i corsi e le iscrizioni</span><span class="sxs-lookup"><span data-stu-id="9640e-365">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="9640e-366">In questa sezione viene creata la pagina Instructors (Insegnanti).</span><span class="sxs-lookup"><span data-stu-id="9640e-366">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="9640e-367">![Pagina di indice degli insegnanti](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="9640e-367">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="9640e-368">Questa pagina legge e visualizza dati correlati nei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9640e-368">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="9640e-369">L'elenco di insegnanti visualizza dati correlati provenienti dall'entità `OfficeAssignment`, Office (Ufficio) nella figura precedente.</span><span class="sxs-lookup"><span data-stu-id="9640e-369">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="9640e-370">Tra le entità `Instructor` e `OfficeAssignment` c'è una relazione uno-a-zero-o-uno.</span><span class="sxs-lookup"><span data-stu-id="9640e-370">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="9640e-371">Per le entità `OfficeAssignment` viene usato il caricamento eager.</span><span class="sxs-lookup"><span data-stu-id="9640e-371">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="9640e-372">Il caricamento eager è in genere più efficiente quando i dati correlati devono essere visualizzati.</span><span class="sxs-lookup"><span data-stu-id="9640e-372">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="9640e-373">In questo caso, devono essere visualizzate le assegnazioni di ufficio degli insegnanti.</span><span class="sxs-lookup"><span data-stu-id="9640e-373">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="9640e-374">Quando l'utente seleziona un insegnante (Harui nella figura precedente), vengono visualizzate le entità `Course` correlate.</span><span class="sxs-lookup"><span data-stu-id="9640e-374">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="9640e-375">Tra le entità `Instructor` e `Course` esiste una relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="9640e-375">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="9640e-376">Il caricamento eager viene usato per le entità `Course` e le entità `Department` correlate.</span><span class="sxs-lookup"><span data-stu-id="9640e-376">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="9640e-377">In questo caso, query separate potrebbero essere più efficienti, perché sono necessari solo i corsi per l'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-377">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="9640e-378">Questo esempio illustra come usare il caricamento eager per le proprietà di navigazione di entità all'interno di proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-378">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="9640e-379">Quando l'utente seleziona un corso (Chemistry (Chimica) nella figura precedente), vengono visualizzati i dati correlati dell'entità `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="9640e-379">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="9640e-380">Nella figura precedente, vengono visualizzati il voto e il nome degli studenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-380">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="9640e-381">Tra le entità `Course` e `Enrollment` esiste una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="9640e-381">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="9640e-382">Creare un modello per la visualizzazione dell'indice degli insegnanti</span><span class="sxs-lookup"><span data-stu-id="9640e-382">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="9640e-383">La pagina Instructors (Insegnanti) mostra i dati di tre tabelle diverse.</span><span class="sxs-lookup"><span data-stu-id="9640e-383">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="9640e-384">Viene creato un modello di visualizzazione che include le tre entità che rappresentano le tre tabelle.</span><span class="sxs-lookup"><span data-stu-id="9640e-384">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="9640e-385">Nella cartella *SchoolViewModels* creare *InstructorIndexData.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-385">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="9640e-386">Scaffolding del modello Instructor</span><span class="sxs-lookup"><span data-stu-id="9640e-386">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9640e-387">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9640e-387">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="9640e-388">Seguire le istruzioni in [Eseguire lo scaffolding del modello Student (Studente)](xref:data/ef-rp/intro#scaffold-the-student-model) e usare `Instructor` per la classe modello.</span><span class="sxs-lookup"><span data-stu-id="9640e-388">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9640e-389">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9640e-389">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="9640e-390">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-390">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="9640e-391">Il comando precedente esegue lo scaffolding del modello `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="9640e-391">The preceding command scaffolds the `Instructor` model.</span></span> 
<span data-ttu-id="9640e-392">Eseguire l'app e passare alla pagina Instructors (Insegnanti).</span><span class="sxs-lookup"><span data-stu-id="9640e-392">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="9640e-393">Sostituire *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-393">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="9640e-394">Il metodo `OnGetAsync` accetta i dati di route facoltativi per l'ID dell'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-394">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="9640e-395">Esaminare la query nel file *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9640e-395">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="9640e-396">La query ha due istruzioni Include, che riguardano:</span><span class="sxs-lookup"><span data-stu-id="9640e-396">The query has two includes:</span></span>

* <span data-ttu-id="9640e-397">`OfficeAssignment`: visualizzato nella [visualizzazione degli insegnanti](#IP).</span><span class="sxs-lookup"><span data-stu-id="9640e-397">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="9640e-398">`CourseAssignments`: visualizza i corsi tenuti.</span><span class="sxs-lookup"><span data-stu-id="9640e-398">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="9640e-399">Aggiornare la pagina di indice degli insegnanti</span><span class="sxs-lookup"><span data-stu-id="9640e-399">Update the instructors Index page</span></span>

<span data-ttu-id="9640e-400">Aggiornare *Pages/Instructors/Index.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-400">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="9640e-401">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="9640e-401">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="9640e-402">Aggiorna la direttiva `page` da `@page` a `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="9640e-402">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="9640e-403">`"{id:int?}"` è un modello di route.</span><span class="sxs-lookup"><span data-stu-id="9640e-403">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="9640e-404">Il modello di route cambia le stringhe di query di tipo integer nell'URL in dati di route.</span><span class="sxs-lookup"><span data-stu-id="9640e-404">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="9640e-405">Se ad esempio si fa clic su sul collegamento **Select** (Seleziona) per un insegnante con la sola direttiva `@page`, viene generato un URL come il seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-405">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="9640e-406">Quando la direttiva della pagina è `@page "{id:int?}"`, l'URL precedente è:</span><span class="sxs-lookup"><span data-stu-id="9640e-406">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="9640e-407">Il titolo pagina è **Instructors** (Insegnanti).</span><span class="sxs-lookup"><span data-stu-id="9640e-407">Page title is **Instructors**.</span></span>
* <span data-ttu-id="9640e-408">È stata aggiunta la colonna **Office** (Ufficio) che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è Null.</span><span class="sxs-lookup"><span data-stu-id="9640e-408">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="9640e-409">Poiché questa è una relazione uno-a-zero-o-uno, potrebbe non esserci un'entità OfficeAssignment correlata.</span><span class="sxs-lookup"><span data-stu-id="9640e-409">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="9640e-410">È stata aggiunta la colonna **Courses** (Corsi) che visualizza i corsi tenuti da ogni insegnante.</span><span class="sxs-lookup"><span data-stu-id="9640e-410">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="9640e-411">Per altre informazioni su questa sintassi Razor, vedere [Transizione riga esplicita con `@:`](xref:mvc/views/razor#explicit-line-transition-with-).</span><span class="sxs-lookup"><span data-stu-id="9640e-411">See [Explicit line transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="9640e-412">È stato aggiunto codice che aggiunge `class="success"` in modo dinamico all'elemento `tr` dell'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-412">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="9640e-413">In questo modo viene impostato un colore di sfondo per la riga selezionata tramite una classe Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="9640e-413">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="9640e-414">È stato aggiunto un nuovo collegamento ipertestuale con etichetta **Select** (Seleziona).</span><span class="sxs-lookup"><span data-stu-id="9640e-414">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="9640e-415">Questo collegamento invia l'ID dell'insegnante selezionato al metodo `Index` e imposta un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="9640e-415">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="9640e-416">Eseguire l'app e selezionare la scheda **Instructors** (Insegnanti). La pagina visualizza `Location` (Ufficio) dall'entità `OfficeAssignment` correlata.</span><span class="sxs-lookup"><span data-stu-id="9640e-416">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="9640e-417">Se OfficeAssignment è Null, nella tabella viene visualizzata una cella vuota.</span><span class="sxs-lookup"><span data-stu-id="9640e-417">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="9640e-418">Fare clic sul collegamento **Select** (Seleziona).</span><span class="sxs-lookup"><span data-stu-id="9640e-418">Click on the **Select** link.</span></span> <span data-ttu-id="9640e-419">Lo stile delle righe cambia.</span><span class="sxs-lookup"><span data-stu-id="9640e-419">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="9640e-420">Aggiungere corsi tenuti dall'insegnante selezionato</span><span class="sxs-lookup"><span data-stu-id="9640e-420">Add courses taught by selected instructor</span></span>

<span data-ttu-id="9640e-421">Aggiornare il metodo `OnGetAsync` in *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-421">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="9640e-422">Aggiungere `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="9640e-422">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="9640e-423">Esaminare la query aggiornata:</span><span class="sxs-lookup"><span data-stu-id="9640e-423">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="9640e-424">La query precedente aggiunge l'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="9640e-424">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="9640e-425">Il codice seguente viene eseguito quando viene selezionato un insegnante (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="9640e-425">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="9640e-426">L'insegnante selezionato viene recuperato dall'elenco di insegnanti nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9640e-426">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="9640e-427">La proprietà `Courses` del modello di visualizzazione viene caricata con le entità `Course` dalla proprietà di navigazione `CourseAssignments` di tale insegnante.</span><span class="sxs-lookup"><span data-stu-id="9640e-427">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="9640e-428">Il metodo `Where` restituisce una raccolta.</span><span class="sxs-lookup"><span data-stu-id="9640e-428">The `Where` method returns a collection.</span></span> <span data-ttu-id="9640e-429">Nel metodo `Where` precedente viene restituita una sola entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="9640e-429">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="9640e-430">Il metodo `Single` converte la raccolta in un'unica entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="9640e-430">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="9640e-431">L'entità `Instructor` consente l'accesso alla proprietà `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="9640e-431">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="9640e-432">`CourseAssignments` consente l'accesso alle entità `Course` correlate.</span><span class="sxs-lookup"><span data-stu-id="9640e-432">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Relazione m:M tra insegnante e corsi](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="9640e-434">Il metodo `Single` viene usato in una raccolta quando quest'ultima ha un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="9640e-434">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="9640e-435">Il metodo `Single` genera un'eccezione se la raccolta è vuota o se contiene più elementi.</span><span class="sxs-lookup"><span data-stu-id="9640e-435">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="9640e-436">In alternativa, è possibile usare `SingleOrDefault`, che restituisce un valore predefinito (Null in questo caso) se la raccolta è vuota.</span><span class="sxs-lookup"><span data-stu-id="9640e-436">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="9640e-437">L'uso di `SingleOrDefault` per una raccolta vuota:</span><span class="sxs-lookup"><span data-stu-id="9640e-437">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="9640e-438">Genera un'eccezione, a causa del tentativo di cercare la proprietà `Courses` in un riferimento Null.</span><span class="sxs-lookup"><span data-stu-id="9640e-438">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="9640e-439">Il messaggio di eccezione indica meno chiaramente la causa del problema.</span><span class="sxs-lookup"><span data-stu-id="9640e-439">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="9640e-440">Il codice seguente popola la proprietà `Enrollments` del modello di visualizzazione quando è selezionato un corso:</span><span class="sxs-lookup"><span data-stu-id="9640e-440">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="9640e-441">Aggiungere il markup seguente alla fine della pagina Razor *Pages/Instructors/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9640e-441">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="9640e-442">Quando è selezionato un insegnante, il markup precedente visualizza un elenco dei corsi correlati all'insegnante stesso.</span><span class="sxs-lookup"><span data-stu-id="9640e-442">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="9640e-443">Eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="9640e-443">Test the app.</span></span> <span data-ttu-id="9640e-444">Fare clic su un collegamento **Select** (Seleziona) nella pagina Instructors (Insegnanti).</span><span class="sxs-lookup"><span data-stu-id="9640e-444">Click on a **Select** link on the instructors page.</span></span>

### <a name="show-student-data"></a><span data-ttu-id="9640e-445">Visualizzare i dati degli studenti</span><span class="sxs-lookup"><span data-stu-id="9640e-445">Show student data</span></span>

<span data-ttu-id="9640e-446">In questa sezione, l'app viene aggiornata in modo da visualizzare i dati degli studenti per il corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-446">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="9640e-447">Aggiornare la query nel metodo `OnGetAsync` in *Pages/Instructors/Index.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-447">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="9640e-448">Aggiornare *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9640e-448">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="9640e-449">Aggiungere il markup seguente alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="9640e-449">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="9640e-450">Il markup precedente visualizza l'elenco degli studenti iscritti al corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-450">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="9640e-451">Aggiornare la pagina e selezionare un insegnante.</span><span class="sxs-lookup"><span data-stu-id="9640e-451">Refresh the page and select an instructor.</span></span> <span data-ttu-id="9640e-452">Selezionare un corso per visualizzare l'elenco degli studenti iscritti e i voti corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-452">Select a course to see the list of enrolled students and their grades.</span></span>

![Pagina di indice degli insegnanti con un corso selezionato](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="9640e-454">Usare Single</span><span class="sxs-lookup"><span data-stu-id="9640e-454">Using Single</span></span>

<span data-ttu-id="9640e-455">Il metodo `Single` può passare la condizione `Where` anziché chiamare il metodo `Where` separatamente:</span><span class="sxs-lookup"><span data-stu-id="9640e-455">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="9640e-456">L'approccio `Single` precedente non offre alcun vantaggio rispetto all'uso di `Where`.</span><span class="sxs-lookup"><span data-stu-id="9640e-456">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="9640e-457">Alcuni sviluppatori preferiscono lo stile dell'approccio `Single`.</span><span class="sxs-lookup"><span data-stu-id="9640e-457">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="9640e-458">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="9640e-458">Explicit loading</span></span>

<span data-ttu-id="9640e-459">Il codice corrente specifica il caricamento eager per `Enrollments` e `Students`:</span><span class="sxs-lookup"><span data-stu-id="9640e-459">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="9640e-460">Si supponga che gli utenti richiedano raramente la visualizzazione delle iscrizioni a un corso.</span><span class="sxs-lookup"><span data-stu-id="9640e-460">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="9640e-461">In questo caso, per ottimizzare il funzionamento dell'app è utile caricare i dati delle iscrizioni solo se vengono richiesti.</span><span class="sxs-lookup"><span data-stu-id="9640e-461">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="9640e-462">In questa sezione, viene eseguito l'aggiornamento di `OnGetAsync` in modo da usare il caricamento esplicito di `Enrollments` e `Students`.</span><span class="sxs-lookup"><span data-stu-id="9640e-462">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="9640e-463">Aggiornare `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9640e-463">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="9640e-464">Il codice precedente rilascia le chiamate del metodo *ThenInclude* per i dati delle iscrizioni e degli studenti.</span><span class="sxs-lookup"><span data-stu-id="9640e-464">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="9640e-465">Se è selezionato un corso, il codice evidenziato recupera:</span><span class="sxs-lookup"><span data-stu-id="9640e-465">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="9640e-466">Le entità `Enrollment` per il corso selezionato.</span><span class="sxs-lookup"><span data-stu-id="9640e-466">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="9640e-467">Le entità `Student` per ogni entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="9640e-467">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="9640e-468">Si noti che nel codice precedente `.AsNoTracking()` è commentato.</span><span class="sxs-lookup"><span data-stu-id="9640e-468">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="9640e-469">Le proprietà di navigazione possono solo essere caricate in modo esplicito per le entità registrate.</span><span class="sxs-lookup"><span data-stu-id="9640e-469">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="9640e-470">Eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="9640e-470">Test the app.</span></span> <span data-ttu-id="9640e-471">Dal punto di vista degli utenti, l'app si comporta in modo identico alla versione precedente.</span><span class="sxs-lookup"><span data-stu-id="9640e-471">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="9640e-472">La prossima esercitazione illustra come aggiornare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="9640e-472">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9640e-473">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9640e-473">Additional resources</span></span>

* [<span data-ttu-id="9640e-474">Versione YouTube dell'esercitazione (parte 1)</span><span class="sxs-lookup"><span data-stu-id="9640e-474">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="9640e-475">Versione YouTube dell'esercitazione (parte 2)</span><span class="sxs-lookup"><span data-stu-id="9640e-475">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="9640e-476">[Precedente](xref:data/ef-rp/complex-data-model)
>[Successivo](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="9640e-476">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end
