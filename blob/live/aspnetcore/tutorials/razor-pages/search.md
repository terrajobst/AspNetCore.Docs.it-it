---
title: "Aggiunta della funzionalità di ricerca alle pagine Razor di ASP.NET Core MVC"
author: rick-anderson
description: "Illustra come aggiungere la funzionalità di ricerca alle pagine Razor di ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/search
ms.openlocfilehash: 2859d52e42d4430808e01739474df0598c07c805
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="24239-103">Aggiunta della funzionalità di ricerca a un'app di pagine Razor</span><span class="sxs-lookup"><span data-stu-id="24239-103">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="24239-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="24239-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="24239-105">In questo documento viene aggiunta una funzionalità di ricerca alla pagina di indice che consente di ricercare i film in base al *genere* o al *nome*.</span><span class="sxs-lookup"><span data-stu-id="24239-105">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="24239-106">Aggiornare il metodo `OnGetAsync` della pagina di indice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="24239-106">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="24239-107">La prima riga del metodo `OnGetAsync` crea una query [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) per selezionare i film:</span><span class="sxs-lookup"><span data-stu-id="24239-107">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="24239-108">La query viene *solo* definita in questo punto, ma **non** viene eseguita nel database.</span><span class="sxs-lookup"><span data-stu-id="24239-108">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="24239-109">Se il parametro `searchString` contiene una stringa, la query dei film viene modificata per filtrare gli elementi in base alla stringa di ricerca:</span><span class="sxs-lookup"><span data-stu-id="24239-109">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="24239-110">Il codice `s => s.Title.Contains()` è un'[espressione lambda](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="24239-110">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="24239-111">Le espressioni lambda vengono usate nelle query [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) basate sul metodo come argomenti dei metodi di operatore query standard, ad esempio il metodo [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) o `Contains` (usato nel codice precedente).</span><span class="sxs-lookup"><span data-stu-id="24239-111">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="24239-112">Le query LINQ non vengono eseguite al momento della definizione o della modifica chiamando un metodo, ad esempio `Where`, `Contains` o `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="24239-112">LINQ queries are not executed when they are defined or when they are modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="24239-113">L'esecuzione della query viene invece posticipata.</span><span class="sxs-lookup"><span data-stu-id="24239-113">Rather, query execution is deferred.</span></span> <span data-ttu-id="24239-114">Per esecuzione posticipata si intende che la valutazione di un'espressione viene ritardata finché non viene eseguita l'iterazione del valore ottenuto o non viene chiamato il metodo `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="24239-114">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="24239-115">Per altre informazioni, vedere [Esecuzione di query](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="24239-115">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="24239-116">**Nota:** il metodo [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) viene eseguito sul database, non nel codice C#.</span><span class="sxs-lookup"><span data-stu-id="24239-116">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="24239-117">La distinzione tra maiuscole/minuscole nella query dipende dal database e dalle regole di confronto.</span><span class="sxs-lookup"><span data-stu-id="24239-117">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="24239-118">In SQL Server `Contains` esegue il mapping a [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql) che fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="24239-118">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="24239-119">In SQLite, con le regole di confronto predefinite si fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="24239-119">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="24239-120">Passare alla pagina Movies e aggiungere una stringa di query, ad esempio `?searchString=Ghost` all'URL, ad esempio `http://localhost:5000/Movies?searchString=Ghost`.</span><span class="sxs-lookup"><span data-stu-id="24239-120">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="24239-121">Vengono visualizzati i film filtrati.</span><span class="sxs-lookup"><span data-stu-id="24239-121">The filtered movies are displayed.</span></span>

![Vista Index](search/_static/ghost.png)

<span data-ttu-id="24239-123">Se il modello di route seguente viene aggiunto alla pagina di indice, la stringa di ricerca può essere passata come segmento di URL, ad esempio `http://localhost:5000/Movies/ghost`.</span><span class="sxs-lookup"><span data-stu-id="24239-123">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="24239-124">Il vincolo di route precedente consente la ricerca del titolo come dati della route (un segmento URL), anziché come valore della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="24239-124">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="24239-125">Il carattere `?` in `"{searchString?}"` indica che questo parametro di route è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="24239-125">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Vista Index con la parola ghost aggiunta all'URL e un elenco restituito di due film: Ghostbusters e Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="24239-127">Non è tuttavia possibile supporre che gli utenti modifichino l'URL per cercare un film.</span><span class="sxs-lookup"><span data-stu-id="24239-127">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="24239-128">In questo passaggio viene aggiunta l'interfaccia utente per filtrare i film.</span><span class="sxs-lookup"><span data-stu-id="24239-128">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="24239-129">Rimuovere il vincolo di route `"{searchString?}"`, se è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="24239-129">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="24239-130">Aprire il file *Pages/Movies/Index.cshtml* e aggiungere il markup `<form>` evidenziato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="24239-130">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="24239-131">Il tag HTML `<form>` usa l'[helper tag del modulo](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="24239-131">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="24239-132">Quando il modulo viene inviato, la stringa di filtro verrà inviata alla pagina *Pages/Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="24239-132">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="24239-133">Salvare le modifiche e testare il filtro.</span><span class="sxs-lookup"><span data-stu-id="24239-133">Save the changes and test the filter.</span></span>

![Vista Index con la parola ghost digitata nella casella di testo del filtro del titolo](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="24239-135">Ricerca in base al genere</span><span class="sxs-lookup"><span data-stu-id="24239-135">Search by genre</span></span>

<span data-ttu-id="24239-136">Aggiungere le proprietà evidenziate seguenti in *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="24239-136">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]

<span data-ttu-id="24239-137">`SelectList Genres` contiene l'elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="24239-137">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="24239-138">Consente all'utente di selezionare un genere dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="24239-138">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="24239-139">La proprietà `MovieGenre` contiene il genere specificato selezionate dall'utente, ad esempio, "Western".</span><span class="sxs-lookup"><span data-stu-id="24239-139">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="24239-140">Aggiornare il metodo `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="24239-140">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="24239-141">Il codice seguente è una query LINQ che recupera tutti i generi dal database.</span><span class="sxs-lookup"><span data-stu-id="24239-141">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="24239-142">L'elenco `SelectList` di generi viene creato presentando generi distinti.</span><span class="sxs-lookup"><span data-stu-id="24239-142">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There is no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="24239-143">Aggiunta della funzionalità di ricerca in base al genere</span><span class="sxs-lookup"><span data-stu-id="24239-143">Adding search by genre</span></span>

<span data-ttu-id="24239-144">Aggiornare *Index.cshtml* come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="24239-144">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="24239-145">Eseguire il test dell'app effettuando una ricerca per genere, titolo del film e usando entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="24239-145">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="24239-146">[Precedente: Aggiornamento delle pagine](xref:tutorials/razor-pages/da1)
[Successivo: Aggiunta di un nuovo campo](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="24239-146">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
