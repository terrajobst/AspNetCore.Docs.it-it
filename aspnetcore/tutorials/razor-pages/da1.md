---
title: Aggiornare le pagine generate in un'app ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiornare le pagine generate in un'app ASP.NET Core.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 19f54e04e748f5cf0469c2b5428800694d2c9344
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64884076"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="ae10c-103">Aggiornare le pagine generate in un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae10c-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="ae10c-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ae10c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ae10c-105">Le operazioni iniziali con l'app creata con scaffolding per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="ae10c-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="ae10c-106">**ReleaseDate** deve essere **Release Date** (due parole).</span><span class="sxs-lookup"><span data-stu-id="ae10c-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![App per i film aperta in Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="ae10c-108">Aggiornare il codice generato</span><span class="sxs-lookup"><span data-stu-id="ae10c-108">Update the generated code</span></span>

<span data-ttu-id="ae10c-109">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ae10c-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=12,17)]

<span data-ttu-id="ae10c-110">L'annotazione dei dati `[Column(TypeName = "decimal(18, 2)")]` consente a Entity Framework Core di eseguire correttamente il mapping di `Price` nella valuta del database.</span><span class="sxs-lookup"><span data-stu-id="ae10c-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="ae10c-111">Per altre informazioni, vedere [Tipi di dati](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="ae10c-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="ae10c-112">L'attributo [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) viene esaminato nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="ae10c-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="ae10c-113">L'attributo [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) specifica il testo da visualizzare per il nome di un campo, in questo caso "Release Date" anziché "ReleaseDate".</span><span class="sxs-lookup"><span data-stu-id="ae10c-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="ae10c-114">L'attributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) specifica il tipo di dati (Date) e quindi non vengono visualizzate le informazioni sull'ora archiviate nel campo.</span><span class="sxs-lookup"><span data-stu-id="ae10c-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="ae10c-115">Accedere a Pages/Movies e passare il mouse su un collegamento **Edit** (Modifica) per visualizzare l'URL di destinazione.</span><span class="sxs-lookup"><span data-stu-id="ae10c-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Finestra del browser con il passaggio del mouse sul collegamento Edit (Modifica) e un URL di collegamento di http://localhost:1234/Movies/Edit/5](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="ae10c-117">I collegamenti **Edit** (Modifica), **Details** (Dettagli) e **Delete** (Elimina) vengono generati dall'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) nel file *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae10c-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="ae10c-118">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="ae10c-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="ae10c-119">Nel codice precedente `AnchorTagHelper` genera in modo dinamico il valore di attributo `href` HTML dalla pagina Razor (la route è relativa), `asp-page` e l'ID di route (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="ae10c-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="ae10c-120">Per altre informazioni, vedere [Generazione di URL per le pagine](xref:razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="ae10c-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="ae10c-121">Usare **Visualizza origine** dal browser preferito per esaminare il codice generato.</span><span class="sxs-lookup"><span data-stu-id="ae10c-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="ae10c-122">Di seguito è riportata una parte del codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="ae10c-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="ae10c-123">I collegamenti generati dinamicamente passano l'ID del film con una stringa di query, ad esempio `?id=1` in `https://localhost:5001/Movies/Details?id=1`.</span><span class="sxs-lookup"><span data-stu-id="ae10c-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="ae10c-124">Aggiornare le pagine Razor Edit (Modifica), Details (Dettagli) e Delete (Elimina) in modo da usare il modello di route "{id: int}".</span><span class="sxs-lookup"><span data-stu-id="ae10c-124">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="ae10c-125">Modificare la direttiva page per ognuna di queste pagine da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="ae10c-125">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="ae10c-126">Eseguire l'app e quindi visualizzare l'origine.</span><span class="sxs-lookup"><span data-stu-id="ae10c-126">Run the app and then view source.</span></span> <span data-ttu-id="ae10c-127">Il codice HTML generato aggiunge l'ID alla parte di percorso dell'URL:</span><span class="sxs-lookup"><span data-stu-id="ae10c-127">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="ae10c-128">Una richiesta alla pagina con il modello di route "{id: int}" che **non** include l'intero restituirà un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="ae10c-128">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="ae10c-129">Ad esempio, `http://localhost:5000/Movies/Details` restituirà un errore 404.</span><span class="sxs-lookup"><span data-stu-id="ae10c-129">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="ae10c-130">Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:</span><span class="sxs-lookup"><span data-stu-id="ae10c-130">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="ae10c-131">Per testare il comportamento di `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="ae10c-131">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="ae10c-132">Impostare la direttiva page in *Pages/Movies/Details.cshtml* su `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="ae10c-132">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="ae10c-133">Impostare un punto di interruzione in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="ae10c-133">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="ae10c-134">Passare a `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="ae10c-134">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="ae10c-135">Con l'istruzione `@page "{id:int}"`, il punto di interruzione non viene mai raggiunto.</span><span class="sxs-lookup"><span data-stu-id="ae10c-135">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="ae10c-136">Il motore di routing restituisce HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="ae10c-136">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="ae10c-137">Se si utilizza `@page "{id:int?}"`, il metodo `OnGetAsync` restituisce `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="ae10c-137">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

<span data-ttu-id="ae10c-138">Anche se non è consigliabile, è possibile scrivere il metodo `OnGetAsync` (in *Pages/Movies/Delete.cshtml.cs*) come:</span><span class="sxs-lookup"><span data-stu-id="ae10c-138">Although not recommended, you could write the `OnGetAsync` method (in *Pages/Movies/Delete.cshtml.cs*) as:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Delete.cshtml.cs?name=snippet)]

<span data-ttu-id="ae10c-139">Per testare il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="ae10c-139">Test the preceding code:</span></span>

* <span data-ttu-id="ae10c-140">Selezionare un collegamento **Delete**.</span><span class="sxs-lookup"><span data-stu-id="ae10c-140">Select a **Delete** link.</span></span>
* <span data-ttu-id="ae10c-141">Rimuovere l'ID dall'URL.</span><span class="sxs-lookup"><span data-stu-id="ae10c-141">Remove the ID from the URL.</span></span> <span data-ttu-id="ae10c-142">Ad esempio, modificare `https://localhost:5001/Movies/Delete/8` in `https://localhost:5001/Movies/Delete`.</span><span class="sxs-lookup"><span data-stu-id="ae10c-142">For example, change `https://localhost:5001/Movies/Delete/8` to `https://localhost:5001/Movies/Delete`.</span></span>
* <span data-ttu-id="ae10c-143">Eseguire un'istruzione alla volta del codice nel debugger.</span><span class="sxs-lookup"><span data-stu-id="ae10c-143">Step through the code in the debugger.</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="ae10c-144">Verificare la gestione delle eccezioni di concorrenza</span><span class="sxs-lookup"><span data-stu-id="ae10c-144">Review concurrency exception handling</span></span>

<span data-ttu-id="ae10c-145">Verificare il metodo `OnPostAsync` nel file *Pages/Movies/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ae10c-145">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="ae10c-146">Il codice precedente rileva le eccezioni di concorrenza quando un client elimina il film e l'altro client invia modifiche al film.</span><span class="sxs-lookup"><span data-stu-id="ae10c-146">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="ae10c-147">Per testare il blocco `catch`:</span><span class="sxs-lookup"><span data-stu-id="ae10c-147">To test the `catch` block:</span></span>

* <span data-ttu-id="ae10c-148">Impostare un punto di interruzione su `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="ae10c-148">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="ae10c-149">Selezionare **Edit** (Modifica) per un film, apportare modifiche, ma non immettere **Save** (Salva).</span><span class="sxs-lookup"><span data-stu-id="ae10c-149">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="ae10c-150">In un'altra finestra del browser, selezionare il collegamento **Delete** (Elimina) per lo stesso film e quindi eliminare il film.</span><span class="sxs-lookup"><span data-stu-id="ae10c-150">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="ae10c-151">Nella finestra del browser precedente inviare le modifiche al film.</span><span class="sxs-lookup"><span data-stu-id="ae10c-151">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="ae10c-152">In alcuni casi, il codice utilizzabile in ambienti di produzione potrebbe voler rilevare i conflitti di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="ae10c-152">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="ae10c-153">Per altre informazioni, vedere [Gestire i conflitti di concorrenza](xref:data/ef-rp/concurrency).</span><span class="sxs-lookup"><span data-stu-id="ae10c-153">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="ae10c-154">Invio di post e analisi delle associazioni</span><span class="sxs-lookup"><span data-stu-id="ae10c-154">Posting and binding review</span></span>

<span data-ttu-id="ae10c-155">Esaminare il file *Pages/Movies/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ae10c-155">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="ae10c-156">Quando viene eseguita una richiesta HTTP GET alla pagina Movies/Edit (Film/Modifica), ad esempio `http://localhost:5000/Movies/Edit/2`:</span><span class="sxs-lookup"><span data-stu-id="ae10c-156">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="ae10c-157">Il metodo `OnGetAsync` recupera il film dal database e restituisce il metodo `Page`.</span><span class="sxs-lookup"><span data-stu-id="ae10c-157">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="ae10c-158">Il metodo `Page` esegue il rendering della pagina Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae10c-158">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="ae10c-159">Il file *Pages/Movies/Edit.cshtml* contiene la direttiva modello (`@model RazorPagesMovie.Pages.Movies.EditModel`) che rende il modello di film disponibile nella pagina.</span><span class="sxs-lookup"><span data-stu-id="ae10c-159">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="ae10c-160">Il modulo Edit (Modifica) viene visualizzato con i valori dal film.</span><span class="sxs-lookup"><span data-stu-id="ae10c-160">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="ae10c-161">Quando viene inviata la pagina Movies/Edit (Film/Modifica):</span><span class="sxs-lookup"><span data-stu-id="ae10c-161">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="ae10c-162">I valori del modulo nella pagina vengono associati alla proprietà `Movie`.</span><span class="sxs-lookup"><span data-stu-id="ae10c-162">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="ae10c-163">L'attributo `[BindProperty]` abilita l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="ae10c-163">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="ae10c-164">Se sono presenti errori nello stato del modello, ad esempio non è possibile convertire `ReleaseDate` in una data, il modulo viene visualizzato con i valori inviati.</span><span class="sxs-lookup"><span data-stu-id="ae10c-164">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="ae10c-165">Se non sono presenti errori del modello, il film viene salvato.</span><span class="sxs-lookup"><span data-stu-id="ae10c-165">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="ae10c-166">I metodi HTTP GET nelle pagine Razor Index, Create e Delete seguono un criterio simile.</span><span class="sxs-lookup"><span data-stu-id="ae10c-166">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="ae10c-167">Il metodo `OnPostAsync` HTTP POST nella pagina Razor Create segue un criterio simile al metodo `OnPostAsync` nella pagina Edit (Modifica) Razor .</span><span class="sxs-lookup"><span data-stu-id="ae10c-167">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="ae10c-168">La funzionalità di ricerca viene aggiunta nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="ae10c-168">Search is added in the next tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ae10c-169">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ae10c-169">Additional resources</span></span>

* [<span data-ttu-id="ae10c-170">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="ae10c-170">YouTube version of this tutorial</span></span>](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> <span data-ttu-id="ae10c-171">[Precedente: Utilizzo di un database](xref:tutorials/razor-pages/sql)
> [Successivo: Aggiungere la funzionalità di ricerca](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="ae10c-171">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>
