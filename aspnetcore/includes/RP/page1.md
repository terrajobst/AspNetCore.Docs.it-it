# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="20486-101">Pagine Razor create in ASP.NET Core tramite scaffolding</span><span class="sxs-lookup"><span data-stu-id="20486-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="20486-102">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="20486-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="20486-103">In questa esercitazione vengono esaminate le pagine Razor create tramite scaffolding nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="20486-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="20486-104">[Visualizzare o scaricare](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) l'esempio.</span><span class="sxs-lookup"><span data-stu-id="20486-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="20486-105">Pagine di creazione, eliminazione, dettagli e modifica.</span><span class="sxs-lookup"><span data-stu-id="20486-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="20486-106">Esaminare il modello di pagina *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="20486-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="20486-107">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="20486-107">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="20486-108">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="20486-108">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]</span></span>

::: moniker-end

<span data-ttu-id="20486-109">Le pagine Razor vengono derivate da `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="20486-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="20486-110">Per convenzione, la classe derivata `PageModel` viene denominata `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="20486-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="20486-111">Il costruttore usa l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) per aggiungere `MovieContext` alla pagina.</span><span class="sxs-lookup"><span data-stu-id="20486-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="20486-112">Tutte le pagine create tramite scaffolding seguono questo schema.</span><span class="sxs-lookup"><span data-stu-id="20486-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="20486-113">Vedere [Codice asincrono](xref:data/ef-rp/intro#asynchronous-code) per altre informazioni sulla programmazione asincrona con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="20486-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="20486-114">Quando per la pagina viene eseguita una richiesta, il metodo `OnGetAsync` restituisce un elenco di filmati alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="20486-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="20486-115">Nella pagina Razor viene chiamato `OnGetAsync`o `OnGet` per inizializzare lo stato della pagina.</span><span class="sxs-lookup"><span data-stu-id="20486-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="20486-116">In questo caso, `OnGetAsync` ottiene un elenco di filmati e li visualizza.</span><span class="sxs-lookup"><span data-stu-id="20486-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span> 

<span data-ttu-id="20486-117">Quando `OnGet` restituisce `void` o `OnGetAsync` restituisce`Task`, non viene usato alcun metodo restituito.</span><span class="sxs-lookup"><span data-stu-id="20486-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="20486-118">Quando il tipo restituito è `IActionResult` o `Task<IActionResult>`, è necessario specificare un'istruzione return.</span><span class="sxs-lookup"><span data-stu-id="20486-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="20486-119">Ad esempio, il metodo *Pages/Movies/Create.cshtml.cs* `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="20486-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

<!-- TODO - replace with snippet
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="20486-120">Esaminare la pagina Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20486-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="20486-121">Con Razor è possibile passare da HTML a C# o scegliere un markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="20486-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="20486-122">Quando il simbolo `@` è seguito da una [parola chiave riservata Razor ](xref:mvc/views/razor#razor-reserved-keywords), la transizione avviene in un markup specifico per Razor, altrimenti in C#.</span><span class="sxs-lookup"><span data-stu-id="20486-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="20486-123">La direttiva Razor `@page` trasforma il file in un'azione MVC &mdash; in modo che possa gestire le richieste.</span><span class="sxs-lookup"><span data-stu-id="20486-123">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="20486-124">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="20486-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="20486-125">`@page` è un esempio di transizione nel markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="20486-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="20486-126">Vedere [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintassi Razor) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="20486-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="20486-127">Esaminare l'espressione lambda usata nell'helper HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="20486-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="20486-128">L'helper HTML `DisplayNameFor` controlla la proprietà `Title` a cui fa riferimento nell'espressione lambda per determinare il nome visualizzato.</span><span class="sxs-lookup"><span data-stu-id="20486-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="20486-129">L'espressione lambda viene controllata anziché valutata.</span><span class="sxs-lookup"><span data-stu-id="20486-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="20486-130">Non sussiste pertanto violazione di accesso quando `model`, `model.Movie`, o `model.Movie[0]` sono `null` o vuoti.</span><span class="sxs-lookup"><span data-stu-id="20486-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="20486-131">Quando invece l'espressione lambda viene valutata (ad esempio con `@Html.DisplayFor(modelItem => item.Title)`), vengono valutati i valori proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="20486-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="20486-132">La direttiva @model</span><span class="sxs-lookup"><span data-stu-id="20486-132">The @model directive</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="20486-133">La direttiva `@model` specifica il tipo di modello passato alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="20486-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="20486-134">Nell'esempio precedente la riga `@model` rende disponibile la classe derivata `PageModel` alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="20486-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="20486-135">Il modello viene usato negli [helper HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` e `@Html.DisplayName` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="20486-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="20486-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData e layout</span><span class="sxs-lookup"><span data-stu-id="20486-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="20486-137">Esaminare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="20486-137">Consider the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="20486-138">Il codice evidenziato sopra è un esempio di transizione Razor in C#.</span><span class="sxs-lookup"><span data-stu-id="20486-138">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="20486-139">Tra i caratteri `{` e `}` è racchiuso un blocco di codice C#.</span><span class="sxs-lookup"><span data-stu-id="20486-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="20486-140">La classe di base `PageModel` ha una proprietà del dizionario `ViewData` che può essere usata per aggiungere i dati da passare a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="20486-140">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="20486-141">Gli oggetti vengono aggiunti al dizionario `ViewData` usando uno schema chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="20486-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="20486-142">Nell'esempio precedente la proprietà "Title" viene aggiunta al dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="20486-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="20486-143">La proprietà "Title" viene usata nel file *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20486-143">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="20486-144">Il markup seguente illustra le prime righe del file *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20486-144">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="20486-145">La proprietà "Title" viene usata nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20486-145">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="20486-146">Il markup seguente illustra le prime righe del file *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20486-146">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="20486-147">La riga `@*Markup removed for brevity.*@` è un commento Razor.</span><span class="sxs-lookup"><span data-stu-id="20486-147">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="20486-148">A differenza dei commenti HTML (`<!-- -->`), i commenti Razor non vengono inviati al client.</span><span class="sxs-lookup"><span data-stu-id="20486-148">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="20486-149">Eseguire l'app e i collegamenti di test nel progetto (**Home**, **Informazioni**, **Contatto**, **Crea**, **Modifica** ed **Elimina**).</span><span class="sxs-lookup"><span data-stu-id="20486-149">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="20486-150">In ogni pagina viene impostato il titolo che è possibile visualizzare nella scheda del browser. Quando una pagina viene specificata come segnalibro, il titolo viene usato per il segnalibro.</span><span class="sxs-lookup"><span data-stu-id="20486-150">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="20486-151">*Pages/Index.cshtml* e *Pages/Movies/Index.cshtml* hanno attualmente lo stesso titolo, ma è possibile modificarli in modo che i valori siano diversi.</span><span class="sxs-lookup"><span data-stu-id="20486-151">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="20486-152">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="20486-152">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="20486-153">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese degli Stati Uniti, è necessario eseguire alcuni passaggi per globalizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="20486-153">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="20486-154">[Problema 4076 su GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) per istruzioni sull'aggiunta della virgola decimale.</span><span class="sxs-lookup"><span data-stu-id="20486-154">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="20486-155">La proprietà `Layout` viene impostata nel file *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20486-155">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="20486-156">Il markup precedente imposta il file di layout su *Pages/_Layout.cshtml* per tutti i file Razor contenuti nella cartella *Pagine*.</span><span class="sxs-lookup"><span data-stu-id="20486-156">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="20486-157">Vedere [Layout](xref:razor-pages/index#layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="20486-157">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="20486-158">Aggiornare il layout</span><span class="sxs-lookup"><span data-stu-id="20486-158">Update the layout</span></span>

<span data-ttu-id="20486-159">Modificare l'elemento `<title>` nel file *Pages/_Layout.cshtml* per usare una stringa più corta.</span><span class="sxs-lookup"><span data-stu-id="20486-159">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="20486-160">Trovare l'elemento di ancoraggio seguente nel file *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20486-160">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="20486-161">Sostituire l'elemento precedente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="20486-161">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="20486-162">L'elemento di ancoraggio precedente è un [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="20486-162">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="20486-163">In questo caso, si tratta dell'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="20486-163">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="20486-164">L'attributo e il valore dell'helper tag `asp-page="/Movies/Index"` creano un collegamento alla pagina Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="20486-164">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="20486-165">Salvare le modifiche e testare l'app selezionando il collegamento **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="20486-165">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="20486-166">Visualizzare il file [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) in GitHub.</span><span class="sxs-lookup"><span data-stu-id="20486-166">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="20486-167">Modello di pagina di creazione</span><span class="sxs-lookup"><span data-stu-id="20486-167">The Create page model</span></span>

<span data-ttu-id="20486-168">Esaminare il modello di pagina *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="20486-168">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="20486-169">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]</span><span class="sxs-lookup"><span data-stu-id="20486-169">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="20486-170">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]</span><span class="sxs-lookup"><span data-stu-id="20486-170">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]</span></span>
::: moniker-end


<span data-ttu-id="20486-171">Il metodo `OnGet` inizializza lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="20486-171">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="20486-172">La pagina di creazione non possiede uno stato da inizializzare. Viene restituito `Page`.</span><span class="sxs-lookup"><span data-stu-id="20486-172">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="20486-173">Più avanti nell'esercitazione viene illustrato lo stato di inizializzazione del metodo `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="20486-173">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="20486-174">Il metodo `Page` crea un oggetto `PageResult` che esegue il rendering della pagina *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20486-174">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="20486-175">La proprietà `Movie` usa l'attributo `[BindProperty]` per consentire l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="20486-175">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="20486-176">Dopo che il modulo di creazione registra i valori, il runtime di ASP.NET Core associa i valori registrati al modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="20486-176">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="20486-177">Il metodo `OnPostAsync` viene eseguito quando la pagina inserisce i dati del modulo:</span><span class="sxs-lookup"><span data-stu-id="20486-177">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="20486-178">Se il modello contiene errori, il modulo viene nuovamente visualizzato insieme ai dati del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="20486-178">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="20486-179">La maggior parte degli errori del modello sono riscontrabili sul lato client prima che il modulo sia inviato.</span><span class="sxs-lookup"><span data-stu-id="20486-179">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="20486-180">La registrazione di un valore per il campo data non convertibile in una data è un esempio di errore del modello.</span><span class="sxs-lookup"><span data-stu-id="20486-180">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="20486-181">Durante l'esercitazione saranno poi trattate nel dettaglio la convalida lato client e la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="20486-181">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="20486-182">Se non sono presenti errori nel modello, i dati vengono salvati e il browser viene reindirizzato alla pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="20486-182">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="20486-183">Pagina di creazione di Razor</span><span class="sxs-lookup"><span data-stu-id="20486-183">The Create Razor Page</span></span>

<span data-ttu-id="20486-184">Esaminare il file della pagina Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20486-184">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
