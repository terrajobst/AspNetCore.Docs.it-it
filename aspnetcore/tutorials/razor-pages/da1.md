---
title: Aggiornamento delle pagine generate
author: rick-anderson
description: Aggiornamento delle pagine generate con una migliore visualizzazione.
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: abf6839536150f29eaa2d07dafbe0d0c1a08e280
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="updating-the-generated-pages"></a><span data-ttu-id="f3873-103">Aggiornamento delle pagine generate</span><span class="sxs-lookup"><span data-stu-id="f3873-103">Updating the generated pages</span></span>

<span data-ttu-id="f3873-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f3873-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f3873-105">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="f3873-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="f3873-106">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere **Release Date** (due parole).</span><span class="sxs-lookup"><span data-stu-id="f3873-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![App per i film aperta in Chrome con i dati sui film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="f3873-108">Aggiornare il codice generato</span><span class="sxs-lookup"><span data-stu-id="f3873-108">Update the generated code</span></span>

<span data-ttu-id="f3873-109">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f3873-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="f3873-110">Fare clic con il pulsante destro del mouse su una riga rossa ondulata > **Azioni rapide e refactoring**.</span><span class="sxs-lookup"><span data-stu-id="f3873-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Menu di scelta rapida con **> Azioni rapide e refactoring**.](da1/qa.png)

<span data-ttu-id="f3873-112">Selezionare `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="f3873-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations all'inizio dell'elenco](da1/da.png)

  <span data-ttu-id="f3873-114">Visual Studio aggiunge `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="f3873-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="f3873-115">Gli attributi [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) verranno esaminati nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="f3873-115">We'll cover [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="f3873-116">L'attributo [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) specifica il testo da visualizzare per il nome di un campo, in questo caso "Release Date" anziché "ReleaseDate".</span><span class="sxs-lookup"><span data-stu-id="f3873-116">The [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="f3873-117">L'attributo [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) specifica il tipo di dati (Date) e quindi non vengono visualizzate le informazioni sull'ora archiviate nel campo.</span><span class="sxs-lookup"><span data-stu-id="f3873-117">The [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="f3873-118">Accedere a Pages/Movies e passare il mouse su un collegamento **Edit** (Modifica) per visualizzare l'URL di destinazione.</span><span class="sxs-lookup"><span data-stu-id="f3873-118">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Finestra del browser con il passaggio del mouse sul collegamento Edit (Modifica) e un URL di collegamento di http://localhost:1234/Movies/Edit/5](da1/edit7.png)

<span data-ttu-id="f3873-120">I collegamenti **Edit** (Modifica), **Details** (Dettagli) e **Delete** (Elimina) vengono generati dall'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) nel file *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f3873-120">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="f3873-121">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="f3873-121">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="f3873-122">Nel codice precedente `AnchorTagHelper` genera in modo dinamico il valore di attributo `href` HTML dalla pagina Razor (la route è relativa), `asp-page` e l'ID di route (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="f3873-122">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="f3873-123">Per altre informazioni, vedere [Generazione di URL per le pagine](xref:mvc/razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="f3873-123">See [URL generation for Pages](xref:mvc/razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="f3873-124">Usare **Visualizza origine** dal browser preferito per esaminare il codice generato.</span><span class="sxs-lookup"><span data-stu-id="f3873-124">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="f3873-125">Di seguito è riportata una parte del codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="f3873-125">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="f3873-126">I collegamenti generati dinamicamente passano l'ID del film con una stringa di query, ad esempio `http://localhost:5000/Movies/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="f3873-126">The dynamically-generated links pass the movie ID with a query string (for example, `http://localhost:5000/Movies/Details?id=2` ).</span></span> 

<span data-ttu-id="f3873-127">Aggiornare le pagine Razor Edit (Modifica), Details (Dettagli) e Delete (Elimina) in modo da usare il modello di route "{id: int}".</span><span class="sxs-lookup"><span data-stu-id="f3873-127">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="f3873-128">Modificare la direttiva page per ognuna di queste pagine da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="f3873-128">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="f3873-129">Eseguire l'app e quindi visualizzare l'origine.</span><span class="sxs-lookup"><span data-stu-id="f3873-129">Run the app and then view source.</span></span> <span data-ttu-id="f3873-130">Il codice HTML generato aggiunge l'ID alla parte di percorso dell'URL:</span><span class="sxs-lookup"><span data-stu-id="f3873-130">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="f3873-131">Una richiesta alla pagina con il modello di route "{id: int}" che **non** include l'intero restituirà un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="f3873-131">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="f3873-132">Ad esempio, `http://localhost:5000/Movies/Details` restituirà un errore 404.</span><span class="sxs-lookup"><span data-stu-id="f3873-132">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="f3873-133">Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:</span><span class="sxs-lookup"><span data-stu-id="f3873-133">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a><span data-ttu-id="f3873-134">Aggiornare la gestione delle eccezioni di concorrenza</span><span class="sxs-lookup"><span data-stu-id="f3873-134">Update concurrency exception handling</span></span>

<span data-ttu-id="f3873-135">Aggiornare il metodo `OnPostAsync` nel file *Pages/Movies/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="f3873-135">Update the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file.</span></span> <span data-ttu-id="f3873-136">Il codice evidenziato seguente illustra le modifiche:</span><span class="sxs-lookup"><span data-stu-id="f3873-136">The following highlighted code shows the changes:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

<span data-ttu-id="f3873-137">Il codice precedente rileva le eccezioni di concorrenza solo quando il primo client concorrente elimina il film e il secondo client concorrente invia le modifiche al film.</span><span class="sxs-lookup"><span data-stu-id="f3873-137">The previous code only detects concurrency exceptions when the first concurrent client deletes the movie, and the second concurrent client posts changes to the movie.</span></span>

<span data-ttu-id="f3873-138">Per testare il blocco `catch`:</span><span class="sxs-lookup"><span data-stu-id="f3873-138">To test the `catch` block:</span></span>

* <span data-ttu-id="f3873-139">Impostare un punto di interruzione su `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="f3873-139">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="f3873-140">Modificare un film.</span><span class="sxs-lookup"><span data-stu-id="f3873-140">Edit a movie.</span></span>
* <span data-ttu-id="f3873-141">In un'altra finestra del browser, selezionare il collegamento **Delete** (Elimina) per lo stesso film e quindi eliminare il film.</span><span class="sxs-lookup"><span data-stu-id="f3873-141">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="f3873-142">Nella finestra del browser precedente inviare le modifiche al film.</span><span class="sxs-lookup"><span data-stu-id="f3873-142">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="f3873-143">Il codice di produzione in genere rileva i conflitti di concorrenza quando due o più client hanno aggiornato contemporaneamente un record.</span><span class="sxs-lookup"><span data-stu-id="f3873-143">Production code would generally detect concurrency conflicts when two or more clients concurrently updated a record.</span></span> <span data-ttu-id="f3873-144">Per altre informazioni, vedere [Gestione dei conflitti di concorrenza](xref:data/ef-rp/concurrency).</span><span class="sxs-lookup"><span data-stu-id="f3873-144">See [Handling concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="f3873-145">Invio di post e analisi delle associazioni</span><span class="sxs-lookup"><span data-stu-id="f3873-145">Posting and binding review</span></span>

<span data-ttu-id="f3873-146">Esaminare il file *Pages/Movies/Edit.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="f3873-146">Examine the *Pages/Movies/Edit.cshtml.cs* file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span></span>

<span data-ttu-id="f3873-147">Quando viene eseguita una richiesta HTTP GET alla pagina Movies/Edit (Film/Modifica), ad esempio `http://localhost:5000/Movies/Edit/2`:</span><span class="sxs-lookup"><span data-stu-id="f3873-147">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="f3873-148">Il metodo `OnGetAsync` recupera il film dal database e restituisce il metodo `Page`.</span><span class="sxs-lookup"><span data-stu-id="f3873-148">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="f3873-149">Il metodo `Page` esegue il rendering della pagina Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f3873-149">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="f3873-150">Il file *Pages/Movies/Edit.cshtml* contiene la direttiva modello (`@model RazorPagesMovie.Pages.Movies.EditModel`) che rende il modello di film disponibile nella pagina.</span><span class="sxs-lookup"><span data-stu-id="f3873-150">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="f3873-151">Il modulo Edit (Modifica) viene visualizzato con i valori dal film.</span><span class="sxs-lookup"><span data-stu-id="f3873-151">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="f3873-152">Quando viene inviata la pagina Movies/Edit (Film/Modifica):</span><span class="sxs-lookup"><span data-stu-id="f3873-152">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="f3873-153">I valori del modulo nella pagina vengono associati alla proprietà `Movie`.</span><span class="sxs-lookup"><span data-stu-id="f3873-153">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="f3873-154">L'attributo `[BindProperty]` abilita l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="f3873-154">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="f3873-155">Se sono presenti errori nello stato del modello, ad esempio `ReleaseDate` non può essere convertito in una data, il modulo viene inviato nuovamente con i valori presentati.</span><span class="sxs-lookup"><span data-stu-id="f3873-155">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="f3873-156">Se non sono presenti errori del modello, il film viene salvato.</span><span class="sxs-lookup"><span data-stu-id="f3873-156">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="f3873-157">I metodi HTTP GET nelle pagine Razor Index, Create e Delete seguono un criterio simile.</span><span class="sxs-lookup"><span data-stu-id="f3873-157">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="f3873-158">Il metodo `OnPostAsync` HTTP POST nella pagina Razor Create segue un criterio simile al metodo `OnPostAsync` nella pagina Edit (Modifica) Razor .</span><span class="sxs-lookup"><span data-stu-id="f3873-158">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="f3873-159">La funzionalità di ricerca viene aggiunta nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="f3873-159">Search is added in the next tutorial.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f3873-160">[Precedente: Utilizzo di SQL Server Local DB](xref:tutorials/razor-pages/sql)
[Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="f3873-160">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
