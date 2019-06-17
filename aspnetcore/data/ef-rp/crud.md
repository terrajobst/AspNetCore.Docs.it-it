---
title: Razor Pages con EF Core in ASP.NET Core - CRUD - 2 di 8
author: rick-anderson
description: Illustra come creare, leggere, aggiornare ed eliminare con EF Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/crud
ms.openlocfilehash: ec22c82b3b70399017b5b9aa61896440db647062
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750023"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="bd069-103">Razor Pages con EF Core in ASP.NET Core - CRUD - 2 di 8</span><span class="sxs-lookup"><span data-stu-id="bd069-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

<span data-ttu-id="bd069-104">Di [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bd069-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="bd069-105">In questa esercitazione viene esaminato e personalizzato il codice CRUD (Create, Read, Update, Delete) con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="bd069-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="bd069-106">Per ridurre la complessità e mantenere queste esercitazioni incentrate su EF Core, viene usato il codice EF Core nei modelli di pagina.</span><span class="sxs-lookup"><span data-stu-id="bd069-106">To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the page models.</span></span> <span data-ttu-id="bd069-107">Alcuni sviluppatori usano un modello di servizio o di repository per creare un livello di astrazione tra l'interfaccia utente (Razor Pages) e il livello di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="bd069-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="bd069-108">In questa esercitazione vengono esaminate le pagine Razor Create (Crea), Edit (Modifica), Delete (Elimina) e Details (Dettagli) nella cartella *Students*.</span><span class="sxs-lookup"><span data-stu-id="bd069-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Students* folder are examined.</span></span>

<span data-ttu-id="bd069-109">Il codice con scaffolding usa il modello seguente per le pagine Create, Edit e Delete:</span><span class="sxs-lookup"><span data-stu-id="bd069-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="bd069-110">Ottenere e visualizzare i dati richiesti con il metodo HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="bd069-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="bd069-111">Salvare le modifiche apportate ai dati con il metodo HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="bd069-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="bd069-112">Le pagine Index e Details ottengono e visualizzano i dati richiesti con il metodo HTTP GET `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="bd069-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="singleordefaultasync-vs-firstordefaultasync"></a><span data-ttu-id="bd069-113">SingleOrDefaultAsync e FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="bd069-113">SingleOrDefaultAsync vs. FirstOrDefaultAsync</span></span>

<span data-ttu-id="bd069-114">Il codice generato usa [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), che in genere è preferibile rispetto a [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="bd069-114">The generated code uses [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), which is generally preferred over [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

 <span data-ttu-id="bd069-115">`FirstOrDefaultAsync` è più efficace di `SingleOrDefaultAsync` nel recupero di una sola entità:</span><span class="sxs-lookup"><span data-stu-id="bd069-115">`FirstOrDefaultAsync` is more efficient than `SingleOrDefaultAsync` at fetching one entity:</span></span>

* <span data-ttu-id="bd069-116">A meno che il codice non necessiti di verificare che non sia presente più di un'entità restituita dalla query.</span><span class="sxs-lookup"><span data-stu-id="bd069-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span>
* <span data-ttu-id="bd069-117">`SingleOrDefaultAsync` recupera una maggior quantità di dati ed esegue operazioni non necessarie.</span><span class="sxs-lookup"><span data-stu-id="bd069-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="bd069-118">`SingleOrDefaultAsync` genera un'eccezione se è presente più di un'entità che soddisfa la parte del filtro.</span><span class="sxs-lookup"><span data-stu-id="bd069-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
* <span data-ttu-id="bd069-119">`FirstOrDefaultAsync` non genera alcun elemento se è presente più di un'entità che soddisfa la parte del filtro.</span><span class="sxs-lookup"><span data-stu-id="bd069-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<a name="FindAsync"></a>

### <a name="findasync"></a><span data-ttu-id="bd069-120">FindAsync</span><span class="sxs-lookup"><span data-stu-id="bd069-120">FindAsync</span></span>

<span data-ttu-id="bd069-121">Nella maggior parte del codice con scaffolding è possibile usare [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) al posto di `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="bd069-121">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync`.</span></span>

<span data-ttu-id="bd069-122">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="bd069-122">`FindAsync`:</span></span>

* <span data-ttu-id="bd069-123">Individua un'entità con la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="bd069-123">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="bd069-124">Se il contesto rileva un'entità con la chiave primaria, l'entità viene restituita senza una richiesta al database.</span><span class="sxs-lookup"><span data-stu-id="bd069-124">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="bd069-125">È semplice e conciso.</span><span class="sxs-lookup"><span data-stu-id="bd069-125">Is simple and concise.</span></span>
* <span data-ttu-id="bd069-126">È ottimizzato per la ricerca di una singola entità.</span><span class="sxs-lookup"><span data-stu-id="bd069-126">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="bd069-127">Può offrire migliori prestazioni, ma raramente negli scenari Web tipici.</span><span class="sxs-lookup"><span data-stu-id="bd069-127">Can have perf benefits in some situations, but that rarely happens for typical web apps.</span></span>
* <span data-ttu-id="bd069-128">Usa in modo implicito [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) anziché [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="bd069-128">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

<span data-ttu-id="bd069-129">Se si vuole usare `Include` per includere altre entità, l'uso di `FindAsync` non è più appropriato.</span><span class="sxs-lookup"><span data-stu-id="bd069-129">But if you want to `Include` other entities, then `FindAsync` is no longer appropriate.</span></span> <span data-ttu-id="bd069-130">Ciò significa che potrebbe essere necessario abbandonare `FindAsync` e passare a una query durante la creazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="bd069-130">This means that you may need to abandon `FindAsync` and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="bd069-131">Personalizzare la pagina Details</span><span class="sxs-lookup"><span data-stu-id="bd069-131">Customize the Details page</span></span>

<span data-ttu-id="bd069-132">Passare alla pagina `Pages/Students`.</span><span class="sxs-lookup"><span data-stu-id="bd069-132">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="bd069-133">I collegamenti **Edit**, **Details** e **Delete** vengono generati dall'[helper del tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) nel file *Pages/Students/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bd069-133">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

<span data-ttu-id="bd069-134">Eseguire l'app e selezionare un collegamento **Details**.</span><span class="sxs-lookup"><span data-stu-id="bd069-134">Run the app and select a **Details** link.</span></span> <span data-ttu-id="bd069-135">L'URL è nel formato `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="bd069-135">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="bd069-136">L'ID studente viene passato tramite una stringa di query (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="bd069-136">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="bd069-137">Aggiornare le pagine Razor Edit, Details e Delete in modo da usare il modello di route `"{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="bd069-137">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="bd069-138">Modificare la direttiva page per ognuna di queste pagine da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="bd069-138">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="bd069-139">Una richiesta alla pagina con il modello di route "{id: int}" che **non** include il valore di route intero restituisce un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="bd069-139">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="bd069-140">Ad esempio, `http://localhost:5000/Students/Details` restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="bd069-140">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="bd069-141">Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:</span><span class="sxs-lookup"><span data-stu-id="bd069-141">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="bd069-142">Eseguire l'app, fare clic su un collegamento Details e verificare che l'URL passi l'ID come dati della route (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="bd069-142">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="bd069-143">Non modificare globalmente `@page` in `@page "{id:int}"` per non interrompere i collegamenti alle pagine Home e Create.</span><span class="sxs-lookup"><span data-stu-id="bd069-143">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="bd069-144">Aggiungere dati correlati</span><span class="sxs-lookup"><span data-stu-id="bd069-144">Add related data</span></span>

<span data-ttu-id="bd069-145">Il codice con scaffolding della pagina Students Index non include la proprietà `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="bd069-145">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="bd069-146">In questa sezione i contenuti della raccolta `Enrollments` sono visualizzati nella pagina Details.</span><span class="sxs-lookup"><span data-stu-id="bd069-146">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="bd069-147">Il metodo `OnGetAsync` del file *Pages/Students/Details.cshtml.cs* usa il metodo `FirstOrDefaultAsync` per recuperare una singola entità `Student`.</span><span class="sxs-lookup"><span data-stu-id="bd069-147">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="bd069-148">Aggiungere il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="bd069-148">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="bd069-149">I metodi [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) e [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) fanno in modo che il contesto carichi la proprietà di navigazione `Student.Enrollments` e la proprietà di navigazione `Enrollment.Course` all'interno di ogni iscrizione.</span><span class="sxs-lookup"><span data-stu-id="bd069-149">The [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) and [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="bd069-150">Questi metodi vengono esaminati in dettaglio nell'esercitazione sulla lettura dei dati correlati.</span><span class="sxs-lookup"><span data-stu-id="bd069-150">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="bd069-151">Il metodo [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) migliora le prestazioni negli scenari in cui le entità restituite non vengono aggiornate nel contesto corrente.</span><span class="sxs-lookup"><span data-stu-id="bd069-151">The [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="bd069-152">`AsNoTracking` è descritto più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="bd069-152">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="bd069-153">Visualizzare le registrazioni correlate nella pagina Details</span><span class="sxs-lookup"><span data-stu-id="bd069-153">Display related enrollments on the Details page</span></span>

<span data-ttu-id="bd069-154">Aprire *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bd069-154">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="bd069-155">Per visualizzare un elenco delle registrazioni, aggiungere il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="bd069-155">Add the following highlighted code to display a list of enrollments:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

<span data-ttu-id="bd069-156">Se dopo aver incollato il codice il rientro è errato, premere CTRL-K-D per correggerlo.</span><span class="sxs-lookup"><span data-stu-id="bd069-156">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="bd069-157">Il codice precedente esegue il ciclo nelle entità nella proprietà di navigazione `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="bd069-157">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="bd069-158">Per ogni registrazione, il codice visualizza il titolo del corso e il voto.</span><span class="sxs-lookup"><span data-stu-id="bd069-158">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="bd069-159">Il titolo del corso viene recuperato dall'entità Course memorizzata nella proprietà di navigazione `Course` dell'entità Enrollments.</span><span class="sxs-lookup"><span data-stu-id="bd069-159">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="bd069-160">Eseguire l'app, selezionare la scheda **Students** e fare clic sul collegamento **Details** relativo a uno studente.</span><span class="sxs-lookup"><span data-stu-id="bd069-160">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="bd069-161">Viene visualizzato l'elenco dei corsi e dei voti dello studente selezionato.</span><span class="sxs-lookup"><span data-stu-id="bd069-161">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="bd069-162">Aggiornare la pagina Create</span><span class="sxs-lookup"><span data-stu-id="bd069-162">Update the Create page</span></span>

<span data-ttu-id="bd069-163">Aggiornare il metodo `OnPostAsync` in *Pages/Students/Create.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="bd069-163">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a><span data-ttu-id="bd069-164">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="bd069-164">TryUpdateModelAsync</span></span>

<span data-ttu-id="bd069-165">Esaminare il codice [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_):</span><span class="sxs-lookup"><span data-stu-id="bd069-165">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="bd069-166">Nel codice precedente `TryUpdateModelAsync<Student>` tenta di aggiornare l'oggetto `emptyStudent` usando i valori di modulo inviati dalla proprietà [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) in [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span><span class="sxs-lookup"><span data-stu-id="bd069-166">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span></span> <span data-ttu-id="bd069-167">`TryUpdateModelAsync` aggiorna solo le proprietà elencate (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="bd069-167">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="bd069-168">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="bd069-168">In the preceding sample:</span></span>

* <span data-ttu-id="bd069-169">Il secondo argomento (`"student", // Prefix`) è il prefisso usato per cercare i valori.</span><span class="sxs-lookup"><span data-stu-id="bd069-169">The second argument (`"student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="bd069-170">Non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="bd069-170">It's not case sensitive.</span></span>
* <span data-ttu-id="bd069-171">I valori di modulo inviati sono convertiti nei tipi nel modello `Student` usando l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="bd069-171">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding).</span></span>

<a id="overpost"></a>

### <a name="overposting"></a><span data-ttu-id="bd069-172">Overposting</span><span class="sxs-lookup"><span data-stu-id="bd069-172">Overposting</span></span>

<span data-ttu-id="bd069-173">L'uso di `TryUpdateModel` per l'aggiornamento dei campi con i valori inviati è una procedura di sicurezza consigliata poiché impedisce l'overposting.</span><span class="sxs-lookup"><span data-stu-id="bd069-173">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="bd069-174">Ad esempio, si supponga che l'entità Student includa una proprietà `Secret` che la pagina Web non deve aggiornare o aggiungere:</span><span class="sxs-lookup"><span data-stu-id="bd069-174">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="bd069-175">Anche se l'app non include un campo `Secret` nella pagina Razor Create o Update, un hacker potrebbe impostare il valore `Secret` tramite overposting.</span><span class="sxs-lookup"><span data-stu-id="bd069-175">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="bd069-176">Un hacker potrebbe usare uno strumento come Fiddler oppure scrivere codice JavaScript per inviare un valore di modulo `Secret`.</span><span class="sxs-lookup"><span data-stu-id="bd069-176">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="bd069-177">Il codice originale non limita i campi usati dallo strumento di associazione di modelli durante la creazione di un'istanza di Student.</span><span class="sxs-lookup"><span data-stu-id="bd069-177">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="bd069-178">Qualsiasi valore specificato dall'hacker per il campo di modulo `Secret` viene aggiornato nel database.</span><span class="sxs-lookup"><span data-stu-id="bd069-178">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="bd069-179">L'immagine seguente illustra lo strumento Fiddler che aggiunge il campo `Secret` (con il valore "OverPost") ai valori di modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="bd069-179">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler aggiunge il campo Secret](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="bd069-181">Il valore "OverPost" è stato aggiunto alla proprietà `Secret` della riga inserita.</span><span class="sxs-lookup"><span data-stu-id="bd069-181">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="bd069-182">La progettazione app non prevedeva in alcun modo che la proprietà `Secret` venisse impostata con la pagina Create.</span><span class="sxs-lookup"><span data-stu-id="bd069-182">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>

### <a name="view-model"></a><span data-ttu-id="bd069-183">Modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="bd069-183">View model</span></span>

<span data-ttu-id="bd069-184">Un modello di visualizzazione contiene in genere un subset delle proprietà incluse nel modello usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bd069-184">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="bd069-185">Il modello di applicazione è spesso chiamato modello di dominio.</span><span class="sxs-lookup"><span data-stu-id="bd069-185">The application model is often called the domain model.</span></span> <span data-ttu-id="bd069-186">Il modello di dominio contiene in genere tutte le proprietà richieste dall'entità corrispondente nel database.</span><span class="sxs-lookup"><span data-stu-id="bd069-186">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="bd069-187">Il modello di visualizzazione contiene solo le proprietà necessarie per il livello di interfaccia utente, ad esempio la pagina Create.</span><span class="sxs-lookup"><span data-stu-id="bd069-187">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="bd069-188">Oltre al modello di visualizzazione, alcune app usano un modello di associazione o un modello di input per passare i dati dalla classe del modello di pagina di Razor Pages al browser e viceversa.</span><span class="sxs-lookup"><span data-stu-id="bd069-188">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="bd069-189">Si consideri il modello di visualizzazione `Student` seguente:</span><span class="sxs-lookup"><span data-stu-id="bd069-189">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

<span data-ttu-id="bd069-190">I modelli di visualizzazione rappresentano un altro metodo per impedire l'overposting.</span><span class="sxs-lookup"><span data-stu-id="bd069-190">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="bd069-191">Il modello di visualizzazione contiene solo le proprietà da visualizzare o aggiornare.</span><span class="sxs-lookup"><span data-stu-id="bd069-191">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="bd069-192">Il codice seguente usa il modello di visualizzazione `StudentVM` per creare un nuovo studente:</span><span class="sxs-lookup"><span data-stu-id="bd069-192">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="bd069-193">Il metodo [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) imposta i valori di questo oggetto leggendo i valori di un altro oggetto [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues).</span><span class="sxs-lookup"><span data-stu-id="bd069-193">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="bd069-194">`SetValues` usa la corrispondenza dei nomi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="bd069-194">`SetValues` uses property name matching.</span></span> <span data-ttu-id="bd069-195">Poiché il tipo di modello di visualizzazione non deve essere correlato al tipo di modello, è sufficiente che abbia proprietà corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="bd069-195">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="bd069-196">Se si usa `StudentVM` è necessario che [CreateVM.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) venga aggiornato per l'uso di `StudentVM` anziché `Student`.</span><span class="sxs-lookup"><span data-stu-id="bd069-196">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="bd069-197">In Razor Pages la classe derivata `PageModel` è il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bd069-197">In Razor Pages, the `PageModel` derived class is the view model.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="bd069-198">Aggiornare la pagina Edit (Modifica)</span><span class="sxs-lookup"><span data-stu-id="bd069-198">Update the Edit page</span></span>

<span data-ttu-id="bd069-199">Aggiornare il modello di pagina per la pagina Edit.</span><span class="sxs-lookup"><span data-stu-id="bd069-199">Update the page model for the Edit page.</span></span> <span data-ttu-id="bd069-200">Le modifiche principali sono evidenziate:</span><span class="sxs-lookup"><span data-stu-id="bd069-200">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="bd069-201">Le modifiche al codice sono simili alla pagina Create con alcune eccezioni:</span><span class="sxs-lookup"><span data-stu-id="bd069-201">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="bd069-202">`OnPostAsync` include un parametro `id` facoltativo.</span><span class="sxs-lookup"><span data-stu-id="bd069-202">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="bd069-203">Lo studente corrente viene recuperato dal database senza creare uno studente vuoto.</span><span class="sxs-lookup"><span data-stu-id="bd069-203">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="bd069-204">`FirstOrDefaultAsync` è stato sostituito con [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span><span class="sxs-lookup"><span data-stu-id="bd069-204">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span></span> <span data-ttu-id="bd069-205">`FindAsync` è una scelta ottimale quando si seleziona un'entità dalla chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="bd069-205">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="bd069-206">Per altre informazioni, vedere [FindAsync](#FindAsync).</span><span class="sxs-lookup"><span data-stu-id="bd069-206">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="bd069-207">Testare le pagine Edit e Create</span><span class="sxs-lookup"><span data-stu-id="bd069-207">Test the Edit and Create pages</span></span>

<span data-ttu-id="bd069-208">Creare e modificare alcune entità studente.</span><span class="sxs-lookup"><span data-stu-id="bd069-208">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="bd069-209">Stati di entità</span><span class="sxs-lookup"><span data-stu-id="bd069-209">Entity States</span></span>

<span data-ttu-id="bd069-210">Il contesto del database tiene traccia della sincronizzazione delle entità in memoria con le righe corrispondenti nel database.</span><span class="sxs-lookup"><span data-stu-id="bd069-210">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="bd069-211">Le informazioni di sincronizzazione del contesto del database determinano le operazioni eseguite quando viene chiamato [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="bd069-211">The DB context sync information determines what happens when [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span> <span data-ttu-id="bd069-212">Ad esempio, quando una nuova entità viene passata al metodo [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync), lo stato dell'entità viene impostato su [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span><span class="sxs-lookup"><span data-stu-id="bd069-212">For example, when a new entity is passed to the [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) method, that entity's state is set to [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span></span> <span data-ttu-id="bd069-213">Quando viene chiamato `SaveChangesAsync`, il contesto del database genera un comando SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="bd069-213">When `SaveChangesAsync` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="bd069-214">Un'entità può essere in uno dei [seguenti stati](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span><span class="sxs-lookup"><span data-stu-id="bd069-214">An entity may be in one of the [following states](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span></span>

* <span data-ttu-id="bd069-215">`Added`: l'entità non esiste ancora nel database.</span><span class="sxs-lookup"><span data-stu-id="bd069-215">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="bd069-216">Il metodo `SaveChanges` genera un'istruzione INSERT.</span><span class="sxs-lookup"><span data-stu-id="bd069-216">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="bd069-217">`Unchanged`: non è necessario salvare alcuna modifica con questa entità.</span><span class="sxs-lookup"><span data-stu-id="bd069-217">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="bd069-218">Un'entità ha questo stato quando viene letta dal database.</span><span class="sxs-lookup"><span data-stu-id="bd069-218">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="bd069-219">`Modified`: Sono stati modificati alcuni o tutti i valori di proprietà dell'entità.</span><span class="sxs-lookup"><span data-stu-id="bd069-219">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="bd069-220">Il metodo `SaveChanges` genera un'istruzione UPDATE.</span><span class="sxs-lookup"><span data-stu-id="bd069-220">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="bd069-221">`Deleted`: L'entità è stata contrassegnata per l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="bd069-221">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="bd069-222">Il metodo `SaveChanges` genera un'istruzione DELETE.</span><span class="sxs-lookup"><span data-stu-id="bd069-222">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="bd069-223">`Detached`: l'entità non viene registrata dal contesto del database.</span><span class="sxs-lookup"><span data-stu-id="bd069-223">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="bd069-224">In un'applicazione desktop le modifiche dello stato vengono in genere impostate automaticamente.</span><span class="sxs-lookup"><span data-stu-id="bd069-224">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="bd069-225">Viene letta un'entità, vengono apportate le modifiche e lo stato dell'entità viene modificato automaticamente in `Modified`.</span><span class="sxs-lookup"><span data-stu-id="bd069-225">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="bd069-226">La chiamata di `SaveChanges` genera un'istruzione SQL UPDATE che aggiorna solo le proprietà modificate.</span><span class="sxs-lookup"><span data-stu-id="bd069-226">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="bd069-227">In un'app Web il `DbContext` che legge un'entità e visualizza i dati viene eliminato dopo il rendering di una pagina.</span><span class="sxs-lookup"><span data-stu-id="bd069-227">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="bd069-228">Quando viene chiamato il metodo `OnPostAsync` di una pagina, viene effettuata una nuova richiesta Web con una nuova istanza di `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bd069-228">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="bd069-229">La rilettura dell'entità nel nuovo contesto simula l'elaborazione desktop.</span><span class="sxs-lookup"><span data-stu-id="bd069-229">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="bd069-230">Aggiornare la pagina Delete</span><span class="sxs-lookup"><span data-stu-id="bd069-230">Update the Delete page</span></span>

<span data-ttu-id="bd069-231">In questa sezione viene aggiunto un codice per implementare un messaggio di errore personalizzato quando la chiamata a `SaveChanges` ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="bd069-231">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="bd069-232">Aggiungere una stringa per i possibili messaggi di errore:</span><span class="sxs-lookup"><span data-stu-id="bd069-232">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="bd069-233">Sostituire il metodo `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="bd069-233">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="bd069-234">Il codice precedente contiene il parametro facoltativo `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="bd069-234">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="bd069-235">`saveChangesError` indica se il metodo è stato chiamato dopo un errore di eliminazione dell'oggetto Student.</span><span class="sxs-lookup"><span data-stu-id="bd069-235">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="bd069-236">L'operazione di eliminazione potrebbe non riuscire a causa di problemi di rete temporanei.</span><span class="sxs-lookup"><span data-stu-id="bd069-236">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="bd069-237">Gli errori di rete temporanei sono più probabili nel cloud.</span><span class="sxs-lookup"><span data-stu-id="bd069-237">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="bd069-238">`saveChangesError` ha valore false quando `OnGetAsync` della pagina Delete viene chiamato dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="bd069-238">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="bd069-239">Quando `OnGetAsync` viene chiamato da `OnPostAsync` (perché l'operazione di eliminazione ha avuto esito negativo), il parametro `saveChangesError` ha valore true.</span><span class="sxs-lookup"><span data-stu-id="bd069-239">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="bd069-240">Metodo OnPostAsync delle pagine Delete</span><span class="sxs-lookup"><span data-stu-id="bd069-240">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="bd069-241">Sostituire `OnPostAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="bd069-241">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="bd069-242">Il codice precedente recupera l'entità selezionata, quindi chiama il metodo [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) per impostare lo stato dell'entità su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="bd069-242">The preceding code retrieves the selected entity, then calls the [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="bd069-243">Quando viene chiamato `SaveChanges`, viene generato un comando SQL DELETE.</span><span class="sxs-lookup"><span data-stu-id="bd069-243">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="bd069-244">Se `Remove` ha esito negativo:</span><span class="sxs-lookup"><span data-stu-id="bd069-244">If `Remove` fails:</span></span>

* <span data-ttu-id="bd069-245">Viene rilevata l'eccezione di database.</span><span class="sxs-lookup"><span data-stu-id="bd069-245">The DB exception is caught.</span></span>
* <span data-ttu-id="bd069-246">Il metodo `OnGetAsync` delle pagine Delete viene chiamato con `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="bd069-246">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="bd069-247">Aggiornare la pagina Razor Delete</span><span class="sxs-lookup"><span data-stu-id="bd069-247">Update the Delete Razor Page</span></span>

<span data-ttu-id="bd069-248">Aggiungere il messaggio di errore evidenziato seguente alla pagina Razor Delete.</span><span class="sxs-lookup"><span data-stu-id="bd069-248">Add the following highlighted error message to the Delete Razor Page.</span></span>
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="bd069-249">Eseguire il test di Delete.</span><span class="sxs-lookup"><span data-stu-id="bd069-249">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="bd069-250">Errori comuni</span><span class="sxs-lookup"><span data-stu-id="bd069-250">Common errors</span></span>

<span data-ttu-id="bd069-251">Students/Index o altri collegamenti non funzionano:</span><span class="sxs-lookup"><span data-stu-id="bd069-251">Students/Index or other links don't work:</span></span>

<span data-ttu-id="bd069-252">Verificare che la pagina Razor contenga la direttiva `@page` corretta.</span><span class="sxs-lookup"><span data-stu-id="bd069-252">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="bd069-253">Ad esempio, la pagina Razor Students/Index **non** deve contenere un modello di route:</span><span class="sxs-lookup"><span data-stu-id="bd069-253">For example, The Students/Index Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="bd069-254">Ogni pagina Razor deve includere la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="bd069-254">Each Razor Page must include the `@page` directive.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="bd069-255">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="bd069-255">Additional resources</span></span>

* [<span data-ttu-id="bd069-256">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="bd069-256">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=K4X1MT2jt6o)

> [!div class="step-by-step"]
> <span data-ttu-id="bd069-257">[Precedente](xref:data/ef-rp/intro)
> [Successivo](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="bd069-257">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
