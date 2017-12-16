---
title: Pagine Razor con Entity Framework Core - CRUD - 2 di 8
author: rick-anderson
description: Viene illustrato come creare, leggere, aggiornare ed eliminare con Entity Framework di base
keywords: ASP.NET Core, Entity Framework Core, CRUD, creare, leggere, aggiornare, eliminare
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 163bc35afed0bf1d9236935d5ce60e6975356594
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/15/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="d7edf-104">Creare, leggere, aggiornare ed eliminare - Core EF con pagine Razor (2 di 8)</span><span class="sxs-lookup"><span data-stu-id="d7edf-104">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="d7edf-105">Da [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d7edf-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d7edf-106">In questa esercitazione, lo scaffolding CRUD (create, leggere, aggiornare ed eliminare) codice viene esaminato e personalizzati.</span><span class="sxs-lookup"><span data-stu-id="d7edf-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="d7edf-107">Nota: Per ridurre la complessità e mantenere queste esercitazioni con stato attivo in Entity Framework Core, il codice di Entity Framework Core viene utilizzato nei file di codice Razor pagine.</span><span class="sxs-lookup"><span data-stu-id="d7edf-107">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages code-behind files.</span></span> <span data-ttu-id="d7edf-108">Alcuni sviluppatori utilizzano un modello di repository o livello di servizio in per creare un livello di astrazione tra l'interfaccia utente (pagine Razor) e il livello di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="d7edf-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="d7edf-109">In questa esercitazione, crea, modifica, eliminazione e pagine Razor dettagli di *Student* cartella vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="d7edf-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="d7edf-110">Per le pagine di creazione, modifica ed eliminazione, il codice di scaffolding utilizza il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="d7edf-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="d7edf-111">Ottenere e visualizzare i dati richiesti con il metodo HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="d7edf-112">Salvare le modifiche ai dati con il metodo HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="d7edf-113">Pagine dell'indice e i dettagli di ottenere e visualizzare i dati richiesti con il metodo HTTP GET`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="d7edf-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="d7edf-114">Sostituire SingleOrDefaultAsync con FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="d7edf-114">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="d7edf-115">Il codice generato utilizza [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) per recuperare l'entità richiesta.</span><span class="sxs-lookup"><span data-stu-id="d7edf-115">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="d7edf-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) risulta più efficiente il recupero di un'entità:</span><span class="sxs-lookup"><span data-stu-id="d7edf-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="d7edf-117">A meno che non è necessario verificare che non è più di un'entità restituita dalla query.</span><span class="sxs-lookup"><span data-stu-id="d7edf-117">Unless the code needs to verify that there is not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="d7edf-118">`SingleOrDefaultAsync`Recupera i dati più ed esegue operazioni non necessarie.</span><span class="sxs-lookup"><span data-stu-id="d7edf-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="d7edf-119">`SingleOrDefaultAsync`genera un'eccezione se è presente più di un'entità che soddisfa la parte del filtro.</span><span class="sxs-lookup"><span data-stu-id="d7edf-119">`SingleOrDefaultAsync` throws an exception if there is more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="d7edf-120">`FirstOrDefaultAsync`non genera eccezioni se è presente più di un'entità che soddisfa la parte del filtro.</span><span class="sxs-lookup"><span data-stu-id="d7edf-120">`FirstOrDefaultAsync` doesn't throw if there is more than one entity that fits the filter part.</span></span>

<span data-ttu-id="d7edf-121">Sostituire globalmente `SingleOrDefaultAsync` con `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-121">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="d7edf-122">`SingleOrDefaultAsync`viene utilizzata nelle 5 posizioni:</span><span class="sxs-lookup"><span data-stu-id="d7edf-122">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="d7edf-123">`OnGetAsync`Nella pagina dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="d7edf-123">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="d7edf-124">`OnGetAsync`e `OnPostAsync` nelle pagine di modifica e l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="d7edf-124">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="d7edf-125">FindAsync</span><span class="sxs-lookup"><span data-stu-id="d7edf-125">FindAsync</span></span>

<span data-ttu-id="d7edf-126">La maggior parte del codice scaffolding, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) può essere utilizzato al posto di `FirstOrDefaultAsync` o `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-126">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="d7edf-127">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="d7edf-127">`FindAsync`:</span></span>

* <span data-ttu-id="d7edf-128">Consente di individuare un'entità con la chiave primaria (PK).</span><span class="sxs-lookup"><span data-stu-id="d7edf-128">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="d7edf-129">Se un'entità con la chiave pubblica è stata rilevata dal contesto, viene restituito senza una richiesta al database.</span><span class="sxs-lookup"><span data-stu-id="d7edf-129">If an entity with the PK is being tracked by the context, it is returned without a request to the DB.</span></span>
* <span data-ttu-id="d7edf-130">È semplice e conciso.</span><span class="sxs-lookup"><span data-stu-id="d7edf-130">Is simple and concise.</span></span>
* <span data-ttu-id="d7edf-131">È ottimizzato per cercare una singola entità.</span><span class="sxs-lookup"><span data-stu-id="d7edf-131">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="d7edf-132">Possono avere i vantaggi di prestazioni in alcune situazioni, ma vengono raramente entrano in gioco per gli scenari web normale.</span><span class="sxs-lookup"><span data-stu-id="d7edf-132">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="d7edf-133">Utilizza in modo implicito [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) anziché [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="d7edf-133">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="d7edf-134">Se si desidera includere altre entità, allora trova non è più appropriato.</span><span class="sxs-lookup"><span data-stu-id="d7edf-134">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="d7edf-135">Ciò significa che potrebbe essere necessario abbandonare Find e spostare in una query durante l'avanzamento app.</span><span class="sxs-lookup"><span data-stu-id="d7edf-135">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="d7edf-136">Personalizzare la pagina dei dettagli</span><span class="sxs-lookup"><span data-stu-id="d7edf-136">Customize the Details page</span></span>

<span data-ttu-id="d7edf-137">Passare a `Pages/Students` pagina.</span><span class="sxs-lookup"><span data-stu-id="d7edf-137">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="d7edf-138">Il **modifica**, **dettagli**, e **eliminare** collegamenti vengono generati dal [Helper di Tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) nel *pagine e gli studenti / Cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="d7edf-138">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="d7edf-139">Selezionare un collegamento di dettagli.</span><span class="sxs-lookup"><span data-stu-id="d7edf-139">Select a Details link.</span></span> <span data-ttu-id="d7edf-140">L'URL è nel formato `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-140">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="d7edf-141">L'ID dello studente viene passato utilizzando una stringa di query (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="d7edf-141">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="d7edf-142">Aggiornare la modifica, dettagli ed Elimina pagine Razor da utilizzare il `"{id:int}"` modello di route.</span><span class="sxs-lookup"><span data-stu-id="d7edf-142">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="d7edf-143">Modificare la direttiva page per ognuna di queste pagine da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-143">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="d7edf-144">Una richiesta per la pagina con il modello di route "{id: int}" che esegue **non** includono un numero intero route valore restituisce HTTP 404 (non trovato) l'errore.</span><span class="sxs-lookup"><span data-stu-id="d7edf-144">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="d7edf-145">Ad esempio, `http://localhost:5000/Students/Details` restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="d7edf-145">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="d7edf-146">Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:</span><span class="sxs-lookup"><span data-stu-id="d7edf-146">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="d7edf-147">Eseguire l'app, fare clic su un collegamento di dettagli e verificare l'URL è passando l'ID come dati della route (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="d7edf-147">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="d7edf-148">Non modificare globalmente `@page` a `@page "{id:int}"`, in questo modo viene interrotta i collegamenti per la home page e creare pagine.</span><span class="sxs-lookup"><span data-stu-id="d7edf-148">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="d7edf-149">Aggiungere i dati correlati</span><span class="sxs-lookup"><span data-stu-id="d7edf-149">Add related data</span></span>

<span data-ttu-id="d7edf-150">Il codice per la pagina di indice di studenti scaffolding non include il `Enrollments` proprietà.</span><span class="sxs-lookup"><span data-stu-id="d7edf-150">The scaffolded code for the Students Index page does not include the `Enrollments` property.</span></span> <span data-ttu-id="d7edf-151">In questa sezione, il contenuto del `Enrollments` raccolta viene visualizzata nella pagina dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="d7edf-151">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="d7edf-152">Il `OnGetAsync` metodo *Pages/Students/Details.cshtml.cs* utilizza il `FirstOrDefaultAsync` metodo per recuperare un singolo `Student` entità.</span><span class="sxs-lookup"><span data-stu-id="d7edf-152">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="d7edf-153">Aggiungere il codice evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="d7edf-153">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="d7edf-154">Il `Include` e `ThenInclude` metodi che il contesto di caricamento di `Student.Enrollments` proprietà di navigazione e all'interno di ogni registrazione di `Enrollment.Course` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="d7edf-154">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="d7edf-155">Questi metodi sono examinied in dettaglio in questa esercitazione i dati relativi alla lettura.</span><span class="sxs-lookup"><span data-stu-id="d7edf-155">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="d7edf-156">Il `AsNoTracking` metodo migliora le prestazioni negli scenari quando le entità restituite non vengono aggiornati nel contesto corrente.</span><span class="sxs-lookup"><span data-stu-id="d7edf-156">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="d7edf-157">`AsNoTracking`viene illustrata più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d7edf-157">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="d7edf-158">Visualizzare le registrazioni correlate nella pagina dei dettagli</span><span class="sxs-lookup"><span data-stu-id="d7edf-158">Display related enrollments on the Details page</span></span>

<span data-ttu-id="d7edf-159">Aprire *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d7edf-159">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="d7edf-160">Aggiungere il codice evidenziato di seguito per visualizzare un elenco delle registrazioni:</span><span class="sxs-lookup"><span data-stu-id="d7edf-160">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="d7edf-161">Se dopo avere incollato il codice, rientro del codice non è corretto, premere CTRL-K-D per correggere l'errore.</span><span class="sxs-lookup"><span data-stu-id="d7edf-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="d7edf-162">Il codice precedente consente di scorrere le entità nel `Enrollments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="d7edf-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="d7edf-163">Per ogni registrazione, viene visualizzato il titolo del corso e il livello.</span><span class="sxs-lookup"><span data-stu-id="d7edf-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="d7edf-164">Il titolo del corso viene recuperato dall'entità che viene archiviato in corso la `Course` proprietà di navigazione di entità le registrazioni.</span><span class="sxs-lookup"><span data-stu-id="d7edf-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="d7edf-165">Eseguire l'app, selezionare il **studenti** scheda, quindi scegliere il **dettagli** collegamento per uno studente.</span><span class="sxs-lookup"><span data-stu-id="d7edf-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="d7edf-166">Viene visualizzato l'elenco di corsi e livelli per gli studenti selezionato.</span><span class="sxs-lookup"><span data-stu-id="d7edf-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="d7edf-167">Aggiornare la pagina di creazione</span><span class="sxs-lookup"><span data-stu-id="d7edf-167">Update the Create page</span></span>

<span data-ttu-id="d7edf-168">Aggiornamento di `OnPostAsync` metodo *Pages/Students/Create.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d7edf-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="d7edf-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="d7edf-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="d7edf-170">Esaminare il [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) codice:</span><span class="sxs-lookup"><span data-stu-id="d7edf-170">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="d7edf-171">Nel codice precedente, `TryUpdateModelAsync<Student>` tenta di aggiornare il `emptyStudent` utilizzando i valori del form inseriti dal [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) proprietà il [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="d7edf-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="d7edf-172">`TryUpdateModelAsync`Aggiorna solo le proprietà elencate (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="d7edf-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="d7edf-173">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="d7edf-173">In the preceding sample:</span></span>

* <span data-ttu-id="d7edf-174">Il secondo argomento (` "student", // Prefix`) è il prefisso utilizzato per cercare i valori.</span><span class="sxs-lookup"><span data-stu-id="d7edf-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="d7edf-175">Non è tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="d7edf-175">It's not case sensitive.</span></span>
* <span data-ttu-id="d7edf-176">I valori del form inseriti vengono convertiti in tipi nel `Student` del modello utilizzando [associazione del modello](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="d7edf-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="d7edf-177">Overposting</span><span class="sxs-lookup"><span data-stu-id="d7edf-177">Overposting</span></span>

<span data-ttu-id="d7edf-178">Utilizzando `TryUpdateModel` per aggiornare i campi con valori pubblicati è una protezione ottimale, perché impedisce overposting.</span><span class="sxs-lookup"><span data-stu-id="d7edf-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="d7edf-179">Ad esempio, si supponga che l'entità Student includa un `Secret` proprietà di questa pagina web non devono aggiornare o aggiungere:</span><span class="sxs-lookup"><span data-stu-id="d7edf-179">For example, suppose the Student entity includes a `Secret` property that this web page should not update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="d7edf-180">Anche se l'app non dispone di un `Secret` campo pagina Razor, un pirata informatico create/update è stato possibile impostare il `Secret` valore overposting.</span><span class="sxs-lookup"><span data-stu-id="d7edf-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="d7edf-181">Un pirata informatico potrebbe utilizzare uno strumento come Fiddler o scrivere alcuni JavaScript, per registrare un `Secret` modulo valore.</span><span class="sxs-lookup"><span data-stu-id="d7edf-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="d7edf-182">Il codice originale non si limita i campi che il gestore di associazione del modello viene utilizzato durante la creazione di un'istanza di studenti.</span><span class="sxs-lookup"><span data-stu-id="d7edf-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="d7edf-183">Valore che il pirata informatico specificato per il `Secret` campo del form viene aggiornato nel database.</span><span class="sxs-lookup"><span data-stu-id="d7edf-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="d7edf-184">La figura seguente mostra l'aggiunta di strumento Fiddler il `Secret` campo (con il valore "OverPost") per i valori del form inseriti.</span><span class="sxs-lookup"><span data-stu-id="d7edf-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Campo segreto aggiunta Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="d7edf-186">Il valore "OverPost" viene aggiunto al `Secret` proprietà della riga inserita.</span><span class="sxs-lookup"><span data-stu-id="d7edf-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="d7edf-187">La finestra di progettazione di app non è destinata la `Secret` proprietà da impostare con la pagina di creazione.</span><span class="sxs-lookup"><span data-stu-id="d7edf-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="d7edf-188">Modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="d7edf-188">View model</span></span>

<span data-ttu-id="d7edf-189">In genere, un modello di visualizzazione contiene un subset delle proprietà incluse nel modello utilizzato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d7edf-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="d7edf-190">Il modello di applicazione è spesso definito il modello di dominio.</span><span class="sxs-lookup"><span data-stu-id="d7edf-190">The application model is often called the domain model.</span></span> <span data-ttu-id="d7edf-191">Il modello di dominio in genere contiene tutte le proprietà necessarie per l'entità corrispondente nel database.</span><span class="sxs-lookup"><span data-stu-id="d7edf-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="d7edf-192">Il modello di visualizzazione contiene solo le proprietà necessarie per il livello di interfaccia utente (ad esempio, la pagina Crea).</span><span class="sxs-lookup"><span data-stu-id="d7edf-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="d7edf-193">Oltre al modello di visualizzazione, alcune applicazioni di utilizzano un modello di associazione o il modello di input per passare dati tra la classe code-behind pagine Razor e il browser.</span><span class="sxs-lookup"><span data-stu-id="d7edf-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages code-behind class and the browser.</span></span> <span data-ttu-id="d7edf-194">Tenere presente quanto segue `Student` modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="d7edf-194">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="d7edf-195">Visualizzazione di modelli fornisce un modo alternativo per impedire overposting.</span><span class="sxs-lookup"><span data-stu-id="d7edf-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="d7edf-196">Il modello di visualizzazione contiene solo le proprietà per visualizzare (visualizzazione) o l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="d7edf-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="d7edf-197">Il codice seguente usa il `StudentVM` modello di visualizzazione per creare un nuovo studente:</span><span class="sxs-lookup"><span data-stu-id="d7edf-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="d7edf-198">Il [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metodo imposta i valori di questo oggetto tramite la lettura di valori da un altro [i PropertyValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) oggetto.</span><span class="sxs-lookup"><span data-stu-id="d7edf-198">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="d7edf-199">`SetValues`Usa la corrispondenza dei nomi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="d7edf-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="d7edf-200">Il tipo di modello di visualizzazione non deve essere correlato al tipo di modello, ma è sufficiente disporre le proprietà corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="d7edf-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="d7edf-201">Utilizzando `StudentVM` richiede [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) aggiornate per l'utilizzo `StudentVM` anziché `Student`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="d7edf-202">Nelle pagine Razor, la `PageModel` classe derivata è il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d7edf-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="d7edf-203">Aggiornare la pagina di modifica</span><span class="sxs-lookup"><span data-stu-id="d7edf-203">Update the Edit page</span></span>

<span data-ttu-id="d7edf-204">Aggiornare il file code-behind pagina Modifica:</span><span class="sxs-lookup"><span data-stu-id="d7edf-204">Update the Edit page code-behind file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="d7edf-205">Le modifiche al codice sono simili alla pagina Crea con alcune eccezioni:</span><span class="sxs-lookup"><span data-stu-id="d7edf-205">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="d7edf-206">`OnPostAsync`è facoltativo `id` parametro.</span><span class="sxs-lookup"><span data-stu-id="d7edf-206">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="d7edf-207">Gli studenti correnti vengono recuperati dal database, invece di creare un studente vuoto.</span><span class="sxs-lookup"><span data-stu-id="d7edf-207">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="d7edf-208">`FirstOrDefaultAsync`è stato sostituito con [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="d7edf-208">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="d7edf-209">`FindAsync`è una scelta ottimale quando si seleziona un'entità dalla chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="d7edf-209">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="d7edf-210">Vedere [FindAsync](#FindAsync) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="d7edf-210">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="d7edf-211">La modifica di test e creare pagine</span><span class="sxs-lookup"><span data-stu-id="d7edf-211">Test the Edit and Create pages</span></span>

<span data-ttu-id="d7edf-212">Creare e modificare alcune entità studente.</span><span class="sxs-lookup"><span data-stu-id="d7edf-212">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="d7edf-213">Stati di entità</span><span class="sxs-lookup"><span data-stu-id="d7edf-213">Entity States</span></span>

<span data-ttu-id="d7edf-214">Il contesto DB tiene traccia delle entità in memoria siano sincronizzate con le relative righe corrispondenti nel database di.</span><span class="sxs-lookup"><span data-stu-id="d7edf-214">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="d7edf-215">Le informazioni di sincronizzazione di contesto DB determinano cosa accade quando `SaveChanges` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="d7edf-215">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="d7edf-216">Ad esempio, quando una nuova entità viene passata al `Add` (metodo), che lo stato dell'entità è impostato su `Added`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-216">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="d7edf-217">Quando `SaveChanges` viene chiamato, il database contesto esegue un comando SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="d7edf-217">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="d7edf-218">Un'entità può essere in uno dei seguenti stati:</span><span class="sxs-lookup"><span data-stu-id="d7edf-218">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="d7edf-219">`Added`: L'entità non esiste ancora nel database.</span><span class="sxs-lookup"><span data-stu-id="d7edf-219">`Added`: The entity does not yet exist in the DB.</span></span> <span data-ttu-id="d7edf-220">Il `SaveChanges` metodo genera un'istruzione INSERT.</span><span class="sxs-lookup"><span data-stu-id="d7edf-220">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="d7edf-221">`Unchanged`: Nessuna modifica desidera essere salvati con questa entità.</span><span class="sxs-lookup"><span data-stu-id="d7edf-221">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="d7edf-222">Un'entità ha questo stato quando viene letto dal database.</span><span class="sxs-lookup"><span data-stu-id="d7edf-222">An entity has this status when it is read from the DB.</span></span>

* <span data-ttu-id="d7edf-223">`Modified`: Sono stati modificati alcuni o tutti i valori di proprietà dell'entità.</span><span class="sxs-lookup"><span data-stu-id="d7edf-223">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="d7edf-224">Il `SaveChanges` metodo genera un'istruzione UPDATE.</span><span class="sxs-lookup"><span data-stu-id="d7edf-224">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="d7edf-225">`Deleted`: L'entità è stato contrassegnato per l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="d7edf-225">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="d7edf-226">Il `SaveChanges` metodo genera un'istruzione DELETE.</span><span class="sxs-lookup"><span data-stu-id="d7edf-226">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="d7edf-227">`Detached`: L'entità non viene rilevato dal contesto DB.</span><span class="sxs-lookup"><span data-stu-id="d7edf-227">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="d7edf-228">In un'applicazione desktop, le modifiche dello stato vengono in genere impostate automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d7edf-228">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="d7edf-229">Un'entità è di lettura, vengono apportate modifiche e lo stato dell'entità viene convertito automaticamente in `Modified`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-229">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="d7edf-230">La chiamata `SaveChanges` genera un'istruzione SQL UPDATE che aggiorna solo le proprietà modificate.</span><span class="sxs-lookup"><span data-stu-id="d7edf-230">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="d7edf-231">In un'app web, il `DbContext` che legge un'entità e consente di visualizzare i dati sono stati eliminati dopo il rendering di una pagina.</span><span class="sxs-lookup"><span data-stu-id="d7edf-231">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="d7edf-232">Quando una pagina `OnPostAsync` metodo viene chiamato, viene effettuata una richiesta web nuovo e con una nuova istanza di `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-232">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="d7edf-233">Rilettura delle entità in tale contesto nuovo simula l'elaborazione del desktop.</span><span class="sxs-lookup"><span data-stu-id="d7edf-233">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="d7edf-234">Aggiornare la pagina di eliminazione</span><span class="sxs-lookup"><span data-stu-id="d7edf-234">Update the Delete page</span></span>

<span data-ttu-id="d7edf-235">In questa sezione viene aggiunto codice per implementare un errore personalizzato messaggio quando la chiamata a `SaveChanges` ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="d7edf-235">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="d7edf-236">Aggiungere una stringa contenente i messaggi di errore possile:</span><span class="sxs-lookup"><span data-stu-id="d7edf-236">Add a string to contain possile error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="d7edf-237">Sostituire il metodo `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d7edf-237">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="d7edf-238">Il codice precedente contiene il parametro facoltativo `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-238">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="d7edf-239">`saveChangesError`indica se il metodo è stato chiamato dopo un errore di eliminazione dell'oggetto studente.</span><span class="sxs-lookup"><span data-stu-id="d7edf-239">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="d7edf-240">L'operazione di eliminazione potrebbe non riuscire a causa di temporanei problemi di rete.</span><span class="sxs-lookup"><span data-stu-id="d7edf-240">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="d7edf-241">Gli errori di rete temporanei sono più probabile che nel cloud.</span><span class="sxs-lookup"><span data-stu-id="d7edf-241">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="d7edf-242">`saveChangesError`è false quando la pagina di eliminazione `OnGetAsync` viene chiamato dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d7edf-242">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="d7edf-243">Quando `OnGetAsync` viene chiamato da `OnPostAsync` (perché l'operazione di eliminazione non riuscita), il `saveChangesError` parametro è true.</span><span class="sxs-lookup"><span data-stu-id="d7edf-243">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="d7edf-244">Metodo Delete pagine OnPostAsync</span><span class="sxs-lookup"><span data-stu-id="d7edf-244">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="d7edf-245">Sostituire il `OnPostAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d7edf-245">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="d7edf-246">Il codice precedente recupera l'entità selezionata, quindi chiama il `Remove` per impostare lo stato dell'entità su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-246">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="d7edf-247">Quando `SaveChanges` viene chiamato un SQL DELETE comando viene generato.</span><span class="sxs-lookup"><span data-stu-id="d7edf-247">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="d7edf-248">Se `Remove` ha esito negativo:</span><span class="sxs-lookup"><span data-stu-id="d7edf-248">If `Remove` fails:</span></span>

* <span data-ttu-id="d7edf-249">Viene rilevata l'eccezione di database.</span><span class="sxs-lookup"><span data-stu-id="d7edf-249">The DB exception is caught.</span></span>
* <span data-ttu-id="d7edf-250">Le pagine di eliminazione `OnGetAsync` metodo viene chiamato con `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="d7edf-250">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="d7edf-251">Aggiornare la pagina di eliminazione Razor</span><span class="sxs-lookup"><span data-stu-id="d7edf-251">Update the Delete Razor Page</span></span>

<span data-ttu-id="d7edf-252">Aggiungere il seguente messaggio di errore evidenziato eliminare Razor.</span><span class="sxs-lookup"><span data-stu-id="d7edf-252">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="d7edf-253">Eliminazione di test.</span><span class="sxs-lookup"><span data-stu-id="d7edf-253">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="d7edf-254">Errori comuni</span><span class="sxs-lookup"><span data-stu-id="d7edf-254">Common errors</span></span>

<span data-ttu-id="d7edf-255">Non funzionano studente/Home o altri collegamenti:</span><span class="sxs-lookup"><span data-stu-id="d7edf-255">Student/Home or other links don't work:</span></span>

<span data-ttu-id="d7edf-256">Verificare la pagina Razor contiene corrette `@page` direttiva.</span><span class="sxs-lookup"><span data-stu-id="d7edf-256">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="d7edf-257">Ad esempio, la pagina di Razor studente/iniziale deve **non** contiene un modello di route:</span><span class="sxs-lookup"><span data-stu-id="d7edf-257">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="d7edf-258">Ogni pagina Razor deve includere il `@page` direttiva.</span><span class="sxs-lookup"><span data-stu-id="d7edf-258">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d7edf-259">[Precedente](xref:data/ef-rp/intro)
[Successivo](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="d7edf-259">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
