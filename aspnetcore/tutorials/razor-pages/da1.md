---
title: Aggiornare le pagine generate in un'app ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiornare le pagine generate in un'app ASP.NET Core.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f1f69b7facf584d46248405c808e75bdd8448d2b
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440318"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="1cc88-103">Aggiornare le pagine generate in un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1cc88-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="1cc88-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1cc88-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1cc88-105">Le operazioni iniziali con l'app per i film creata con scaffolding sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="1cc88-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="1cc88-106">**ReleaseDate** deve essere **Release Date** (due parole).</span><span class="sxs-lookup"><span data-stu-id="1cc88-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![App per i film aperta in Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="1cc88-108">Aggiornare il codice generato</span><span class="sxs-lookup"><span data-stu-id="1cc88-108">Update the generated code</span></span>

<span data-ttu-id="1cc88-109">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1cc88-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="1cc88-110">L'annotazione dei dati `[Column(TypeName = "decimal(18, 2)")]` consente a Entity Framework Core di eseguire correttamente il mapping di `Price` nella valuta del database.</span><span class="sxs-lookup"><span data-stu-id="1cc88-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="1cc88-111">Per altre informazioni, vedere [Tipi di dati](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="1cc88-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="1cc88-112">L'attributo [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) viene esaminato nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="1cc88-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="1cc88-113">L'attributo [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) specifica il testo da visualizzare per il nome di un campo, in questo caso "Release Date" anziché "ReleaseDate".</span><span class="sxs-lookup"><span data-stu-id="1cc88-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="1cc88-114">L'attributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) specifica il tipo di dati (Date) e quindi non vengono visualizzate le informazioni sull'ora archiviate nel campo.</span><span class="sxs-lookup"><span data-stu-id="1cc88-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="1cc88-115">Accedere a Pages/Movies e passare il mouse su un collegamento **Edit** (Modifica) per visualizzare l'URL di destinazione.</span><span class="sxs-lookup"><span data-stu-id="1cc88-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Finestra del browser con il passaggio del mouse sul collegamento Edit (Modifica) e un URL di collegamento di http://localhost:1234/Movies/Edit/5](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="1cc88-117">I collegamenti **Edit** (Modifica), **Details** (Dettagli) e **Delete** (Elimina) vengono generati dall'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) nel file *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1cc88-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="1cc88-118">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="1cc88-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="1cc88-119">Nel codice precedente `AnchorTagHelper` genera in modo dinamico il valore di attributo `href` HTML dalla pagina Razor (la route è relativa), `asp-page` e l'ID di route (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="1cc88-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="1cc88-120">Per altre informazioni, vedere [Generazione di URL per le pagine](xref:razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="1cc88-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="1cc88-121">Usare **Visualizza origine** dal browser preferito per esaminare il codice generato.</span><span class="sxs-lookup"><span data-stu-id="1cc88-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="1cc88-122">Di seguito è riportata una parte del codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="1cc88-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="1cc88-123">I collegamenti generati dinamicamente passano l'ID del film con una stringa di query, ad esempio `?id=1` in `https://localhost:5001/Movies/Details?id=1`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="1cc88-124">Aggiornare le pagine Razor Edit (Modifica), Details (Dettagli) e Delete (Elimina) in modo da usare il modello di route "{id: int}".</span><span class="sxs-lookup"><span data-stu-id="1cc88-124">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="1cc88-125">Modificare la direttiva page per ognuna di queste pagine da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-125">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="1cc88-126">Eseguire l'app e quindi visualizzare l'origine.</span><span class="sxs-lookup"><span data-stu-id="1cc88-126">Run the app and then view source.</span></span> <span data-ttu-id="1cc88-127">Il codice HTML generato aggiunge l'ID alla parte di percorso dell'URL:</span><span class="sxs-lookup"><span data-stu-id="1cc88-127">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="1cc88-128">Una richiesta alla pagina con il modello di route "{id: int}" che **non** include l'intero restituirà un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="1cc88-128">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="1cc88-129">Ad esempio, `http://localhost:5000/Movies/Details` restituirà un errore 404.</span><span class="sxs-lookup"><span data-stu-id="1cc88-129">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="1cc88-130">Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:</span><span class="sxs-lookup"><span data-stu-id="1cc88-130">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="1cc88-131">Per testare il comportamento di `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="1cc88-131">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="1cc88-132">Impostare la direttiva page in *Pages/Movies/Details.cshtml* su `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-132">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="1cc88-133">Impostare un punto di interruzione in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="1cc88-133">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="1cc88-134">Passare a `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-134">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="1cc88-135">Con l'istruzione `@page "{id:int}"`, il breakpoint non viene mai raggiunto.</span><span class="sxs-lookup"><span data-stu-id="1cc88-135">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="1cc88-136">Il motore di routing restituisce HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="1cc88-136">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="1cc88-137">Se si utilizza `@page "{id:int?}"`, il metodo `OnGetAsync` restituisce `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="1cc88-137">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="1cc88-138">Verificare la gestione delle eccezioni di concorrenza</span><span class="sxs-lookup"><span data-stu-id="1cc88-138">Review concurrency exception handling</span></span>

<span data-ttu-id="1cc88-139">Verificare il metodo `OnPostAsync` nel file *Pages/Movies/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1cc88-139">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="1cc88-140">Il codice precedente rileva le eccezioni di concorrenza quando un client elimina il film e l'altro client invia modifiche al film.</span><span class="sxs-lookup"><span data-stu-id="1cc88-140">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="1cc88-141">Per testare il blocco `catch`:</span><span class="sxs-lookup"><span data-stu-id="1cc88-141">To test the `catch` block:</span></span>

* <span data-ttu-id="1cc88-142">Impostare un breakpoint su `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="1cc88-142">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="1cc88-143">Selezionare **Edit** (Modifica) per un film, apportare modifiche, ma non immettere **Save** (Salva).</span><span class="sxs-lookup"><span data-stu-id="1cc88-143">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="1cc88-144">In un'altra finestra del browser, selezionare il collegamento **Delete** (Elimina) per lo stesso film e quindi eliminare il film.</span><span class="sxs-lookup"><span data-stu-id="1cc88-144">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="1cc88-145">Nella finestra del browser precedente inviare le modifiche al film.</span><span class="sxs-lookup"><span data-stu-id="1cc88-145">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="1cc88-146">In alcuni casi, il codice utilizzabile in ambienti di produzione potrebbe voler rilevare i conflitti di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="1cc88-146">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="1cc88-147">Per altre informazioni, vedere [Gestire i conflitti di concorrenza](xref:data/ef-rp/concurrency).</span><span class="sxs-lookup"><span data-stu-id="1cc88-147">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="1cc88-148">Invio di post e analisi delle associazioni</span><span class="sxs-lookup"><span data-stu-id="1cc88-148">Posting and binding review</span></span>

<span data-ttu-id="1cc88-149">Esaminare il file *Pages/Movies/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1cc88-149">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/SnapShots/Edit.cshtml.cs?name=snippet2)]

<span data-ttu-id="1cc88-150">Quando viene eseguita una richiesta HTTP GET alla pagina Movies/Edit (Film/Modifica), ad esempio `http://localhost:5000/Movies/Edit/2`:</span><span class="sxs-lookup"><span data-stu-id="1cc88-150">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="1cc88-151">Il metodo `OnGetAsync` recupera il film dal database e restituisce il metodo `Page`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-151">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span>
* <span data-ttu-id="1cc88-152">Il metodo `Page` esegue il rendering della pagina Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1cc88-152">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="1cc88-153">Il file *Pages/Movies/Edit.cshtml* contiene la direttiva modello (`@model RazorPagesMovie.Pages.Movies.EditModel`) che rende il modello di film disponibile nella pagina.</span><span class="sxs-lookup"><span data-stu-id="1cc88-153">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="1cc88-154">Il modulo Edit (Modifica) viene visualizzato con i valori dal film.</span><span class="sxs-lookup"><span data-stu-id="1cc88-154">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="1cc88-155">Quando viene inviata la pagina Movies/Edit (Film/Modifica):</span><span class="sxs-lookup"><span data-stu-id="1cc88-155">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="1cc88-156">I valori del modulo nella pagina vengono associati alla proprietà `Movie`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-156">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="1cc88-157">L'attributo `[BindProperty]` abilita l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="1cc88-157">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="1cc88-158">Se sono presenti errori nello stato del modello, ad esempio non è possibile convertire `ReleaseDate` in una data, il modulo viene rivisualizzato con i valori inviati.</span><span class="sxs-lookup"><span data-stu-id="1cc88-158">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is redisplayed with the submitted values.</span></span>
* <span data-ttu-id="1cc88-159">Se non sono presenti errori del modello, il film viene salvato.</span><span class="sxs-lookup"><span data-stu-id="1cc88-159">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="1cc88-160">I metodi HTTP GET nelle pagine Razor Index, Create e Delete seguono un criterio simile.</span><span class="sxs-lookup"><span data-stu-id="1cc88-160">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="1cc88-161">Il metodo `OnPostAsync` HTTP POST nella pagina Razor Create segue un criterio simile al metodo `OnPostAsync` nella pagina Edit (Modifica) Razor .</span><span class="sxs-lookup"><span data-stu-id="1cc88-161">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1cc88-162">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1cc88-162">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1cc88-163">[Precedente: Utilizzo di un database](xref:tutorials/razor-pages/sql)
> [Successivo: Aggiungere la funzionalità di ricerca](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="1cc88-163">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1cc88-164">Le operazioni iniziali con l'app per i film creata con scaffolding sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="1cc88-164">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="1cc88-165">**ReleaseDate** deve essere **Release Date** (due parole).</span><span class="sxs-lookup"><span data-stu-id="1cc88-165">**ReleaseDate** should be **Release Date** (two words).</span></span>

![App per i film aperta in Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="1cc88-167">Aggiornare il codice generato</span><span class="sxs-lookup"><span data-stu-id="1cc88-167">Update the generated code</span></span>

<span data-ttu-id="1cc88-168">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1cc88-168">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="1cc88-169">L'annotazione dei dati `[Column(TypeName = "decimal(18, 2)")]` consente a Entity Framework Core di eseguire correttamente il mapping di `Price` nella valuta del database.</span><span class="sxs-lookup"><span data-stu-id="1cc88-169">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="1cc88-170">Per altre informazioni, vedere [Tipi di dati](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="1cc88-170">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="1cc88-171">L'attributo [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) viene esaminato nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="1cc88-171">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="1cc88-172">L'attributo [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) specifica il testo da visualizzare per il nome di un campo, in questo caso "Release Date" anziché "ReleaseDate".</span><span class="sxs-lookup"><span data-stu-id="1cc88-172">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="1cc88-173">L'attributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) specifica il tipo di dati (Date) e quindi non vengono visualizzate le informazioni sull'ora archiviate nel campo.</span><span class="sxs-lookup"><span data-stu-id="1cc88-173">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="1cc88-174">Accedere a Pages/Movies e passare il mouse su un collegamento **Edit** (Modifica) per visualizzare l'URL di destinazione.</span><span class="sxs-lookup"><span data-stu-id="1cc88-174">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Finestra del browser con il passaggio del mouse sul collegamento Edit (Modifica) e un URL di collegamento di http://localhost:1234/Movies/Edit/5](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="1cc88-176">I collegamenti **Edit** (Modifica), **Details** (Dettagli) e **Delete** (Elimina) vengono generati dall'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) nel file *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1cc88-176">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="1cc88-177">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="1cc88-177">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="1cc88-178">Nel codice precedente `AnchorTagHelper` genera in modo dinamico il valore di attributo `href` HTML dalla pagina Razor (la route è relativa), `asp-page` e l'ID di route (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="1cc88-178">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="1cc88-179">Per altre informazioni, vedere [Generazione di URL per le pagine](xref:razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="1cc88-179">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="1cc88-180">Usare **Visualizza origine** dal browser preferito per esaminare il codice generato.</span><span class="sxs-lookup"><span data-stu-id="1cc88-180">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="1cc88-181">Di seguito è riportata una parte del codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="1cc88-181">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="1cc88-182">I collegamenti generati dinamicamente passano l'ID del film con una stringa di query, ad esempio `?id=1` in `https://localhost:5001/Movies/Details?id=1`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-182">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="1cc88-183">Aggiornare le pagine Razor Edit (Modifica), Details (Dettagli) e Delete (Elimina) in modo da usare il modello di route "{id: int}".</span><span class="sxs-lookup"><span data-stu-id="1cc88-183">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="1cc88-184">Modificare la direttiva page per ognuna di queste pagine da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-184">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="1cc88-185">Eseguire l'app e quindi visualizzare l'origine.</span><span class="sxs-lookup"><span data-stu-id="1cc88-185">Run the app and then view source.</span></span> <span data-ttu-id="1cc88-186">Il codice HTML generato aggiunge l'ID alla parte di percorso dell'URL:</span><span class="sxs-lookup"><span data-stu-id="1cc88-186">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="1cc88-187">Una richiesta alla pagina con il modello di route "{id: int}" che **non** include l'intero restituirà un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="1cc88-187">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="1cc88-188">Ad esempio, `http://localhost:5000/Movies/Details` restituirà un errore 404.</span><span class="sxs-lookup"><span data-stu-id="1cc88-188">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="1cc88-189">Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:</span><span class="sxs-lookup"><span data-stu-id="1cc88-189">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="1cc88-190">Per testare il comportamento di `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="1cc88-190">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="1cc88-191">Impostare la direttiva page in *Pages/Movies/Details.cshtml* su `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-191">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="1cc88-192">Impostare un punto di interruzione in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="1cc88-192">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="1cc88-193">Passare a `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-193">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="1cc88-194">Con l'istruzione `@page "{id:int}"`, il breakpoint non viene mai raggiunto.</span><span class="sxs-lookup"><span data-stu-id="1cc88-194">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="1cc88-195">Il motore di routing restituisce HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="1cc88-195">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="1cc88-196">Se si utilizza `@page "{id:int?}"`, il metodo `OnGetAsync` restituisce `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="1cc88-196">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="1cc88-197">Verificare la gestione delle eccezioni di concorrenza</span><span class="sxs-lookup"><span data-stu-id="1cc88-197">Review concurrency exception handling</span></span>

<span data-ttu-id="1cc88-198">Verificare il metodo `OnPostAsync` nel file *Pages/Movies/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1cc88-198">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="1cc88-199">Il codice precedente rileva le eccezioni di concorrenza quando un client elimina il film e l'altro client invia modifiche al film.</span><span class="sxs-lookup"><span data-stu-id="1cc88-199">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="1cc88-200">Per testare il blocco `catch`:</span><span class="sxs-lookup"><span data-stu-id="1cc88-200">To test the `catch` block:</span></span>

* <span data-ttu-id="1cc88-201">Impostare un breakpoint su `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="1cc88-201">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="1cc88-202">Selezionare **Edit** (Modifica) per un film, apportare modifiche, ma non immettere **Save** (Salva).</span><span class="sxs-lookup"><span data-stu-id="1cc88-202">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="1cc88-203">In un'altra finestra del browser, selezionare il collegamento **Delete** (Elimina) per lo stesso film e quindi eliminare il film.</span><span class="sxs-lookup"><span data-stu-id="1cc88-203">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="1cc88-204">Nella finestra del browser precedente inviare le modifiche al film.</span><span class="sxs-lookup"><span data-stu-id="1cc88-204">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="1cc88-205">In alcuni casi, il codice utilizzabile in ambienti di produzione potrebbe voler rilevare i conflitti di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="1cc88-205">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="1cc88-206">Per altre informazioni, vedere [Gestire i conflitti di concorrenza](xref:data/ef-rp/concurrency).</span><span class="sxs-lookup"><span data-stu-id="1cc88-206">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="1cc88-207">Invio di post e analisi delle associazioni</span><span class="sxs-lookup"><span data-stu-id="1cc88-207">Posting and binding review</span></span>

<span data-ttu-id="1cc88-208">Esaminare il file *Pages/Movies/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1cc88-208">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="1cc88-209">Quando viene eseguita una richiesta HTTP GET alla pagina Movies/Edit (Film/Modifica), ad esempio `http://localhost:5000/Movies/Edit/2`:</span><span class="sxs-lookup"><span data-stu-id="1cc88-209">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="1cc88-210">Il metodo `OnGetAsync` recupera il film dal database e restituisce il metodo `Page`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-210">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="1cc88-211">Il metodo `Page` esegue il rendering della pagina Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1cc88-211">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="1cc88-212">Il file *Pages/Movies/Edit.cshtml* contiene la direttiva modello (`@model RazorPagesMovie.Pages.Movies.EditModel`) che rende il modello di film disponibile nella pagina.</span><span class="sxs-lookup"><span data-stu-id="1cc88-212">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="1cc88-213">Il modulo Edit (Modifica) viene visualizzato con i valori dal film.</span><span class="sxs-lookup"><span data-stu-id="1cc88-213">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="1cc88-214">Quando viene inviata la pagina Movies/Edit (Film/Modifica):</span><span class="sxs-lookup"><span data-stu-id="1cc88-214">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="1cc88-215">I valori del modulo nella pagina vengono associati alla proprietà `Movie`.</span><span class="sxs-lookup"><span data-stu-id="1cc88-215">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="1cc88-216">L'attributo `[BindProperty]` abilita l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="1cc88-216">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="1cc88-217">Se sono presenti errori nello stato del modello, ad esempio non è possibile convertire `ReleaseDate` in una data, il modulo viene visualizzato con i valori inviati.</span><span class="sxs-lookup"><span data-stu-id="1cc88-217">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="1cc88-218">Se non sono presenti errori del modello, il film viene salvato.</span><span class="sxs-lookup"><span data-stu-id="1cc88-218">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="1cc88-219">I metodi HTTP GET nelle pagine Razor Index, Create e Delete seguono un criterio simile.</span><span class="sxs-lookup"><span data-stu-id="1cc88-219">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="1cc88-220">Il metodo `OnPostAsync` HTTP POST nella pagina Razor Create segue un criterio simile al metodo `OnPostAsync` nella pagina Edit (Modifica) Razor .</span><span class="sxs-lookup"><span data-stu-id="1cc88-220">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="1cc88-221">La funzionalità di ricerca viene aggiunta nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="1cc88-221">Search is added in the next tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1cc88-222">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1cc88-222">Additional resources</span></span>

* [<span data-ttu-id="1cc88-223">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="1cc88-223">YouTube version of this tutorial</span></span>](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> <span data-ttu-id="1cc88-224">[Precedente: Utilizzo di un database](xref:tutorials/razor-pages/sql)
> [Successivo: Aggiungere la funzionalità di ricerca](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="1cc88-224">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end
