---
title: Aggiungere la funzionalità di ricerca a pagine Razor in ASP.NET Core
author: rick-anderson
description: Illustra come aggiungere la funzionalità di ricerca alle pagine Razor di ASP.NET Core
ms.author: riande
ms.date: 12/3/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 8a8e51632a75efd6411bae46b9a7f9e4f16b480f
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346200"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="57ffe-103">Aggiungere la funzionalità di ricerca a pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57ffe-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="57ffe-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="57ffe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="57ffe-105">Nelle sezioni seguenti viene aggiunta la funzionalità di ricerca di film in base al *genere* oppure al *nome*.</span><span class="sxs-lookup"><span data-stu-id="57ffe-105">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="57ffe-106">Aggiungere le proprietà evidenziate seguenti in *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="57ffe-106">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="57ffe-107">`SearchString`: contiene il testo immesso dagli utenti nella casella di testo di ricerca.</span><span class="sxs-lookup"><span data-stu-id="57ffe-107">`SearchString`: contains the text users enter in the search text box.</span></span> <span data-ttu-id="57ffe-108">`SearchString` è decorato con l'attributo [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute).</span><span class="sxs-lookup"><span data-stu-id="57ffe-108">`SearchString` is decorated with the [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute.</span></span> <span data-ttu-id="57ffe-109">`[BindProperty]` associa i valori modulo e le stringhe di query al nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="57ffe-109">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="57ffe-110">`(SupportsGet = true)` è necessario per l'associazione alle richieste GET.</span><span class="sxs-lookup"><span data-stu-id="57ffe-110">`(SupportsGet = true)` is required for binding on GET requests.</span></span>
* <span data-ttu-id="57ffe-111">`Genres` contiene l'elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="57ffe-111">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="57ffe-112">`Genres` consente all'utente di selezionare un genere dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="57ffe-112">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="57ffe-113">`SelectList` richiede `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="57ffe-113">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="57ffe-114">`MovieGenre`: contiene il genere specificato selezionato dall'utente, ad esempio "Western".</span><span class="sxs-lookup"><span data-stu-id="57ffe-114">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>
* <span data-ttu-id="57ffe-115">`Genres` e `MovieGenre` sono utilizzati più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="57ffe-115">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="57ffe-116">Aggiornare il metodo `OnGetAsync` della pagina di indice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="57ffe-116">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="57ffe-117">La prima riga del metodo `OnGetAsync` crea una query [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) per selezionare i film:</span><span class="sxs-lookup"><span data-stu-id="57ffe-117">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="57ffe-118">La query viene *solo* definita in questo punto, ma **non** viene eseguita nel database.</span><span class="sxs-lookup"><span data-stu-id="57ffe-118">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="57ffe-119">Se la proprietà `SearchString` non è null o vuota, la query dei film viene modificata per filtrare gli elementi in base alla stringa di ricerca:</span><span class="sxs-lookup"><span data-stu-id="57ffe-119">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="57ffe-120">Il codice `s => s.Title.Contains()` è un'[espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="57ffe-120">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="57ffe-121">Le espressioni lambda vengono usate nelle query [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) basate sul metodo come argomenti dei metodi di operatore query standard, ad esempio il metodo [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) o `Contains` (usato nel codice precedente).</span><span class="sxs-lookup"><span data-stu-id="57ffe-121">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="57ffe-122">Le query LINQ non vengono eseguite al momento della definizione o della modifica chiamando un metodo, ad esempio `Where`, `Contains` o `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="57ffe-122">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="57ffe-123">L'esecuzione della query viene invece posticipata.</span><span class="sxs-lookup"><span data-stu-id="57ffe-123">Rather, query execution is deferred.</span></span> <span data-ttu-id="57ffe-124">Per esecuzione posticipata si intende che la valutazione di un'espressione viene ritardata finché non viene eseguita l'iterazione del valore ottenuto o non viene chiamato il metodo `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="57ffe-124">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="57ffe-125">Per altre informazioni, vedere [Esecuzione di query](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="57ffe-125">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="57ffe-126">**Nota:** il metodo [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) viene eseguito nel database, non nel codice C#.</span><span class="sxs-lookup"><span data-stu-id="57ffe-126">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="57ffe-127">La distinzione tra maiuscole/minuscole nella query dipende dal database e dalle regole di confronto.</span><span class="sxs-lookup"><span data-stu-id="57ffe-127">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="57ffe-128">In SQL Server `Contains` esegue il mapping a [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql) che fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="57ffe-128">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="57ffe-129">In SQLite, con le regole di confronto predefinite si fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="57ffe-129">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="57ffe-130">Passare alla pagina Movies e aggiungere una stringa di query, ad esempio `?searchString=Ghost` all'URL, ad esempio `https://localhost:5001/Movies?searchString=Ghost`.</span><span class="sxs-lookup"><span data-stu-id="57ffe-130">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `https://localhost:5001/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="57ffe-131">Vengono visualizzati i film filtrati.</span><span class="sxs-lookup"><span data-stu-id="57ffe-131">The filtered movies are displayed.</span></span>

![Vista Index](search/_static/ghost.png)

<span data-ttu-id="57ffe-133">Se il modello di route seguente viene aggiunto alla pagina di indice, la stringa di ricerca può essere passata come segmento di URL, ad esempio `https://localhost:5001/Movies/Ghost`.</span><span class="sxs-lookup"><span data-stu-id="57ffe-133">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `https://localhost:5001/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="57ffe-134">Il vincolo di route precedente consente la ricerca del titolo come dati della route (un segmento URL), anziché come valore della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="57ffe-134">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="57ffe-135">Il carattere `?` in `"{searchString?}"` indica che questo parametro di route è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="57ffe-135">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Vista Index con la parola ghost aggiunta all'URL e un elenco restituito di due film: Ghostbusters e Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="57ffe-137">Il runtime di ASP.NET Core usa l'[associazione di modelli](xref:mvc/models/model-binding) per impostare il valore della proprietà `SearchString` dalla stringa di query (`?searchString=Ghost`) o dai dati della route (`https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="57ffe-137">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="57ffe-138">All'associazione di modelli non viene applicata la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="57ffe-138">Model binding is not case sensitive.</span></span>

<span data-ttu-id="57ffe-139">Non è tuttavia possibile supporre che gli utenti modifichino l'URL per cercare un film.</span><span class="sxs-lookup"><span data-stu-id="57ffe-139">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="57ffe-140">In questo passaggio viene aggiunta l'interfaccia utente per filtrare i film.</span><span class="sxs-lookup"><span data-stu-id="57ffe-140">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="57ffe-141">Rimuovere il vincolo di route `"{searchString?}"`, se è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="57ffe-141">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="57ffe-142">Aprire il file *Pages/Movies/Index.cshtml* e aggiungere il markup `<form>` evidenziato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="57ffe-142">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="57ffe-143">Il tag HTML `<form>` usa gli [helper tag](xref:mvc/views/tag-helpers/intro) seguenti:</span><span class="sxs-lookup"><span data-stu-id="57ffe-143">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="57ffe-144">[Helper tag Form](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="57ffe-144">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="57ffe-145">Quando il modulo viene inviato, la stringa di filtro verrà inviata alla pagina *Pages/Movies/Index* tramite una stringa di query.</span><span class="sxs-lookup"><span data-stu-id="57ffe-145">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="57ffe-146">Helper tag di input</span><span class="sxs-lookup"><span data-stu-id="57ffe-146">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="57ffe-147">Salvare le modifiche e testare il filtro.</span><span class="sxs-lookup"><span data-stu-id="57ffe-147">Save the changes and test the filter.</span></span>

![Vista Index con la parola ghost digitata nella casella di testo del filtro del titolo](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="57ffe-149">Ricerca in base al genere</span><span class="sxs-lookup"><span data-stu-id="57ffe-149">Search by genre</span></span>

<span data-ttu-id="57ffe-150">Aggiornare il metodo `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="57ffe-150">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="57ffe-151">Il codice seguente è una query LINQ che recupera tutti i generi dal database.</span><span class="sxs-lookup"><span data-stu-id="57ffe-151">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="57ffe-152">L'elenco `SelectList` di generi viene creato presentando generi distinti.</span><span class="sxs-lookup"><span data-stu-id="57ffe-152">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="57ffe-153">Aggiungere la ricerca in base al genere alla pagina Razor</span><span class="sxs-lookup"><span data-stu-id="57ffe-153">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="57ffe-154">Aggiornare *Index.cshtml* come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="57ffe-154">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="57ffe-155">Eseguire il test dell'app effettuando una ricerca per genere, titolo del film e usando entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="57ffe-155">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="57ffe-156">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="57ffe-156">Additional resources</span></span>

* [<span data-ttu-id="57ffe-157">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="57ffe-157">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)


> [!div class="step-by-step"]
> <span data-ttu-id="57ffe-158">[Precedente: Aggiornamento delle pagine](xref:tutorials/razor-pages/da1)
> [Successivo: Aggiunta di un nuovo campo](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="57ffe-158">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
