# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="6cff4-101">Pagine Razor create in ASP.NET Core tramite scaffolding</span><span class="sxs-lookup"><span data-stu-id="6cff4-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="6cff4-102">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6cff4-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6cff4-103">In questa esercitazione vengono esaminate le pagine Razor create tramite scaffolding nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="6cff4-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="6cff4-104">[Visualizzare o scaricare](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) l'esempio.</span><span class="sxs-lookup"><span data-stu-id="6cff4-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="6cff4-105">Pagine di creazione, eliminazione, dettagli e modifica.</span><span class="sxs-lookup"><span data-stu-id="6cff4-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="6cff4-106">Esaminare il modello di pagina *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6cff4-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="6cff4-107">Le pagine Razor vengono derivate da `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="6cff4-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="6cff4-108">Per convenzione, la classe derivata `PageModel` viene denominata `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="6cff4-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="6cff4-109">Il costruttore usa l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) per aggiungere `MovieContext` alla pagina.</span><span class="sxs-lookup"><span data-stu-id="6cff4-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="6cff4-110">Tutte le pagine create tramite scaffolding seguono questo schema.</span><span class="sxs-lookup"><span data-stu-id="6cff4-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="6cff4-111">Vedere [Codice asincrono](xref:data/ef-rp/intro#asynchronous-code) per altre informazioni sulla programmazione asincrona con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6cff4-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="6cff4-112">Quando per la pagina viene eseguita una richiesta, il metodo `OnGetAsync` restituisce un elenco di filmati alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="6cff4-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="6cff4-113">Nella pagina Razor viene chiamato `OnGetAsync`o `OnGet` per inizializzare lo stato della pagina.</span><span class="sxs-lookup"><span data-stu-id="6cff4-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="6cff4-114">In questo caso, `OnGetAsync` ottiene un elenco di filmati e li visualizza.</span><span class="sxs-lookup"><span data-stu-id="6cff4-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="6cff4-115">Quando `OnGet` restituisce `void` o `OnGetAsync` restituisce`Task`, non viene usato alcun metodo restituito.</span><span class="sxs-lookup"><span data-stu-id="6cff4-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="6cff4-116">Quando il tipo restituito è `IActionResult` o `Task<IActionResult>`, è necessario specificare un'istruzione return.</span><span class="sxs-lookup"><span data-stu-id="6cff4-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="6cff4-117">Ad esempio, il metodo *Pages/Movies/Create.cshtml.cs* `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="6cff4-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="6cff4-118">Esaminare la pagina Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6cff4-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="6cff4-119">Con Razor è possibile passare da HTML a C# o scegliere un markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="6cff4-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="6cff4-120">Quando il simbolo `@` è seguito da una [parola chiave riservata Razor ](xref:mvc/views/razor#razor-reserved-keywords), la transizione avviene in un markup specifico per Razor, altrimenti in C#.</span><span class="sxs-lookup"><span data-stu-id="6cff4-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="6cff4-121">La direttiva Razor `@page` trasforma il file in un'azione MVC &mdash; in modo che possa gestire le richieste.</span><span class="sxs-lookup"><span data-stu-id="6cff4-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="6cff4-122">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="6cff4-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="6cff4-123">`@page` è un esempio di transizione nel markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="6cff4-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="6cff4-124">Vedere [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintassi Razor) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="6cff4-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="6cff4-125">Esaminare l'espressione lambda usata nell'helper HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="6cff4-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="6cff4-126">L'helper HTML `DisplayNameFor` controlla la proprietà `Title` a cui fa riferimento nell'espressione lambda per determinare il nome visualizzato.</span><span class="sxs-lookup"><span data-stu-id="6cff4-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="6cff4-127">L'espressione lambda viene controllata anziché valutata.</span><span class="sxs-lookup"><span data-stu-id="6cff4-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="6cff4-128">Non sussiste pertanto violazione di accesso quando `model`, `model.Movie`, o `model.Movie[0]` sono `null` o vuoti.</span><span class="sxs-lookup"><span data-stu-id="6cff4-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="6cff4-129">Quando invece l'espressione lambda viene valutata (ad esempio con `@Html.DisplayFor(modelItem => item.Title)`), vengono valutati i valori proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="6cff4-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="6cff4-130">La direttiva @model</span><span class="sxs-lookup"><span data-stu-id="6cff4-130">The @model directive</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="6cff4-131">La direttiva `@model` specifica il tipo di modello passato alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="6cff4-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="6cff4-132">Nell'esempio precedente la riga `@model` rende disponibile la classe derivata `PageModel` alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="6cff4-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="6cff4-133">Il modello viene usato negli [helper HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` e `@Html.DisplayFor` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="6cff4-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="6cff4-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData e layout</span><span class="sxs-lookup"><span data-stu-id="6cff4-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="6cff4-135">Esaminare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6cff4-135">Consider the following code:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="6cff4-136">Il codice evidenziato sopra è un esempio di transizione Razor in C#.</span><span class="sxs-lookup"><span data-stu-id="6cff4-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="6cff4-137">Tra i caratteri `{` e `}` è racchiuso un blocco di codice C#.</span><span class="sxs-lookup"><span data-stu-id="6cff4-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="6cff4-138">La classe di base `PageModel` ha una proprietà del dizionario `ViewData` che può essere usata per aggiungere i dati da passare a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="6cff4-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="6cff4-139">Gli oggetti vengono aggiunti al dizionario `ViewData` usando uno schema chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="6cff4-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="6cff4-140">Nell'esempio precedente la proprietà "Title" viene aggiunta al dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="6cff4-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6cff4-141">La proprietà "Title" viene usata nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6cff4-141">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="6cff4-142">Il markup seguente illustra le prime righe del file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6cff4-142">The following markup shows the first few lines of the *Pages/Shared/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6cff4-143">La proprietà "Title" viene usata nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6cff4-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="6cff4-144">Il markup seguente illustra le prime righe del file *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6cff4-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="6cff4-145">La riga `@*Markup removed for brevity.*@` è un commento Razor.</span><span class="sxs-lookup"><span data-stu-id="6cff4-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="6cff4-146">A differenza dei commenti HTML (`<!-- -->`), i commenti Razor non vengono inviati al client.</span><span class="sxs-lookup"><span data-stu-id="6cff4-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="6cff4-147">Eseguire l'app e i collegamenti di test nel progetto (**Home**, **Informazioni**, **Contatto**, **Crea**, **Modifica** ed **Elimina**).</span><span class="sxs-lookup"><span data-stu-id="6cff4-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="6cff4-148">In ogni pagina viene impostato il titolo che è possibile visualizzare nella scheda del browser. Quando una pagina viene specificata come segnalibro, il titolo viene usato per il segnalibro.</span><span class="sxs-lookup"><span data-stu-id="6cff4-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="6cff4-149">*Pages/Index.cshtml* e *Pages/Movies/Index.cshtml* hanno attualmente lo stesso titolo, ma è possibile modificarli in modo che i valori siano diversi.</span><span class="sxs-lookup"><span data-stu-id="6cff4-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="6cff4-150">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="6cff4-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="6cff4-151">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese degli Stati Uniti, è necessario eseguire alcuni passaggi per globalizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="6cff4-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="6cff4-152">[Problema 4076 su GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) per istruzioni sull'aggiunta della virgola decimale.</span><span class="sxs-lookup"><span data-stu-id="6cff4-152">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="6cff4-153">La proprietà `Layout` viene impostata nel file *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6cff4-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="6cff4-154">Il markup precedente imposta il file di layout su *Pages/Shared/_Layout.cshtml* per tutti i file Razor contenuti nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6cff4-154">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="6cff4-155">Vedere [Layout](xref:razor-pages/index#layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="6cff4-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="6cff4-156">Aggiornare il layout</span><span class="sxs-lookup"><span data-stu-id="6cff4-156">Update the layout</span></span>

<span data-ttu-id="6cff4-157">Modificare l'elemento `<title>` nel file *Pages/Shared/_Layout.cshtml* per usare una stringa più corta.</span><span class="sxs-lookup"><span data-stu-id="6cff4-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="6cff4-158">Trovare l'elemento di ancoraggio seguente nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6cff4-158">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="6cff4-159">Sostituire l'elemento precedente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="6cff4-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="6cff4-160">L'elemento di ancoraggio precedente è un [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="6cff4-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="6cff4-161">In questo caso, si tratta dell'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="6cff4-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="6cff4-162">L'attributo e il valore dell'helper tag `asp-page="/Movies/Index"` creano un collegamento alla pagina Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="6cff4-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="6cff4-163">Salvare le modifiche e testare l'app selezionando il collegamento **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="6cff4-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="6cff4-164">Visualizzare il file [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) in GitHub.</span><span class="sxs-lookup"><span data-stu-id="6cff4-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="6cff4-165">Modello di pagina di creazione</span><span class="sxs-lookup"><span data-stu-id="6cff4-165">The Create page model</span></span>

<span data-ttu-id="6cff4-166">Esaminare il modello di pagina *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6cff4-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end


<span data-ttu-id="6cff4-167">Il metodo `OnGet` inizializza lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="6cff4-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="6cff4-168">La pagina di creazione non possiede uno stato da inizializzare. Viene restituito `Page`.</span><span class="sxs-lookup"><span data-stu-id="6cff4-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="6cff4-169">Più avanti nell'esercitazione viene illustrato lo stato di inizializzazione del metodo `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="6cff4-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="6cff4-170">Il metodo `Page` crea un oggetto `PageResult` che esegue il rendering della pagina *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6cff4-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="6cff4-171">La proprietà `Movie` usa l'attributo `[BindProperty]` per consentire l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="6cff4-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="6cff4-172">Dopo che il modulo di creazione registra i valori, il runtime di ASP.NET Core associa i valori registrati al modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="6cff4-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="6cff4-173">Il metodo `OnPostAsync` viene eseguito quando la pagina inserisce i dati del modulo:</span><span class="sxs-lookup"><span data-stu-id="6cff4-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="6cff4-174">Se il modello contiene errori, il modulo viene nuovamente visualizzato insieme ai dati del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="6cff4-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="6cff4-175">La maggior parte degli errori del modello sono riscontrabili sul lato client prima che il modulo sia inviato.</span><span class="sxs-lookup"><span data-stu-id="6cff4-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="6cff4-176">La registrazione di un valore per il campo data non convertibile in una data è un esempio di errore del modello.</span><span class="sxs-lookup"><span data-stu-id="6cff4-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="6cff4-177">Durante l'esercitazione saranno poi trattate nel dettaglio la convalida lato client e la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="6cff4-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="6cff4-178">Se non sono presenti errori nel modello, i dati vengono salvati e il browser viene reindirizzato alla pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="6cff4-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="6cff4-179">Pagina di creazione di Razor</span><span class="sxs-lookup"><span data-stu-id="6cff4-179">The Create Razor Page</span></span>

<span data-ttu-id="6cff4-180">Esaminare il file della pagina Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6cff4-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
