---
title: Pagine Razor create in ASP.NET Core tramite scaffolding
author: rick-anderson
description: Illustra le pagine Razor generate tramite scaffolding.
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 594fd6186cc73aa054fc9a1478850fa01e481ef2
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034204"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="8245b-103">Pagine Razor create in ASP.NET Core tramite scaffolding</span><span class="sxs-lookup"><span data-stu-id="8245b-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8245b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8245b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8245b-105">In questa esercitazione vengono esaminate le pagine Razor create tramite scaffolding nell'[esercitazione precedente](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="8245b-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="8245b-106">Pagine di creazione, eliminazione, dettagli e modifica</span><span class="sxs-lookup"><span data-stu-id="8245b-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="8245b-107">Esaminare il modello di pagina *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8245b-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="8245b-108">Le pagine Razor vengono derivate da `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8245b-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="8245b-109">Per convenzione, la classe derivata `PageModel` viene denominata `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="8245b-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="8245b-110">Il costruttore usa l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) per aggiungere `RazorPagesMovieContext` alla pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="8245b-111">Tutte le pagine create tramite scaffolding seguono questo schema.</span><span class="sxs-lookup"><span data-stu-id="8245b-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="8245b-112">Vedere [Codice asincrono](xref:data/ef-rp/intro#asynchronous-code) per altre informazioni sulla programmazione asincrona con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8245b-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="8245b-113">Quando per la pagina viene eseguita una richiesta, il metodo `OnGetAsync` restituisce un elenco di filmati alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="8245b-114">viene chiamato `OnGetAsync` o `OnGet` per inizializzare lo stato della pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-114">`OnGetAsync` or `OnGet` is called to initialize the state of the page.</span></span> <span data-ttu-id="8245b-115">In questo caso, `OnGetAsync` ottiene un elenco di filmati e li visualizza.</span><span class="sxs-lookup"><span data-stu-id="8245b-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="8245b-116">Quando `OnGet` restituisce `void` o `OnGetAsync` restituisce`Task`, non viene utilizzata alcuna istruzione return.</span><span class="sxs-lookup"><span data-stu-id="8245b-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return statement is used.</span></span> <span data-ttu-id="8245b-117">Quando il tipo restituito è `IActionResult` o `Task<IActionResult>`, è necessario specificare un'istruzione return.</span><span class="sxs-lookup"><span data-stu-id="8245b-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="8245b-118">Ad esempio, il metodo *Pages/Movies/Create.cshtml.cs* `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="8245b-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="8245b-119">Esaminare la pagina Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8245b-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="8245b-120">Con Razor è possibile passare da HTML a C# o scegliere un markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="8245b-121">Quando il simbolo `@` è seguito da una [parola chiave riservata Razor ](xref:mvc/views/razor#razor-reserved-keywords), la transizione avviene in un markup specifico per Razor, altrimenti in C#.</span><span class="sxs-lookup"><span data-stu-id="8245b-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

### <a name="the-page-directive"></a><span data-ttu-id="8245b-122">La direttiva @page</span><span class="sxs-lookup"><span data-stu-id="8245b-122">The @page directive</span></span>

<span data-ttu-id="8245b-123">La direttiva `@page` Razor rende il file un'azione MVC, il che significa che può gestire le richieste.</span><span class="sxs-lookup"><span data-stu-id="8245b-123">The `@page` Razor directive makes the file an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="8245b-124">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="8245b-125">`@page` è un esempio di transizione nel markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="8245b-126">Vedere [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintassi Razor) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="8245b-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="8245b-127">Esaminare l'espressione lambda usata nell'helper HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="8245b-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="8245b-128">L'helper HTML `DisplayNameFor` controlla la proprietà `Title` a cui fa riferimento nell'espressione lambda per determinare il nome visualizzato.</span><span class="sxs-lookup"><span data-stu-id="8245b-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="8245b-129">L'espressione lambda viene controllata anziché valutata.</span><span class="sxs-lookup"><span data-stu-id="8245b-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="8245b-130">Ciò significa che non si verifica alcuna violazione di accesso quando `model`, `model.Movie`o `model.Movie[0]` è `null` o vuoto.</span><span class="sxs-lookup"><span data-stu-id="8245b-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` is `null` or empty.</span></span> <span data-ttu-id="8245b-131">Quando invece l'espressione lambda viene valutata (ad esempio con `@Html.DisplayFor(modelItem => item.Title)`), vengono valutati i valori proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="8245b-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="8245b-132">La direttiva @model</span><span class="sxs-lookup"><span data-stu-id="8245b-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="8245b-133">La direttiva `@model` specifica il tipo di modello passato alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="8245b-134">Nell'esempio precedente la riga `@model` rende disponibile la classe derivata `PageModel` alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="8245b-135">Il modello viene usato negli `@Html.DisplayNameFor`helper HTML`@Html.DisplayFor` [ e ](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) nella pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="8245b-136">Pagina di layout</span><span class="sxs-lookup"><span data-stu-id="8245b-136">The layout page</span></span>

<span data-ttu-id="8245b-137">Selezionare i collegamenti di menu: **RazorPagesMovie**, **Home** e **Privacy**.</span><span class="sxs-lookup"><span data-stu-id="8245b-137">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="8245b-138">Ogni pagina mostra lo stesso layout di menu.</span><span class="sxs-lookup"><span data-stu-id="8245b-138">Each page shows the same menu layout.</span></span> <span data-ttu-id="8245b-139">Il layout di menu viene implementato nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-139">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="8245b-140">Aprire il file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-140">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="8245b-141">I modelli [Layout](xref:mvc/views/layout) consentono di:</span><span class="sxs-lookup"><span data-stu-id="8245b-141">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="8245b-142">Specificare il layout del contenitore HTML in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="8245b-142">Specified in one place.</span></span>
* <span data-ttu-id="8245b-143">Applicare il layout in più pagine del sito.</span><span class="sxs-lookup"><span data-stu-id="8245b-143">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="8245b-144">Trovare la riga `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="8245b-144">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="8245b-145">`RenderBody` è un segnaposto dove tutte le visualizzazioni specifiche della pagina vengono presentate, *incapsulate* nella pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="8245b-145">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="8245b-146">Ad esempio, selezionare il collegamento **Privacy** per eseguire il rendering della visualizzazione *Pages/Privacy.cshtml* all'interno del metodo `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="8245b-146">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="8245b-147">ViewData e layout</span><span class="sxs-lookup"><span data-stu-id="8245b-147">ViewData and layout</span></span>

<span data-ttu-id="8245b-148">Si consideri il markup seguente dal file *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8245b-148">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="8245b-149">Il markup evidenziato sopra è un esempio di transizione Razor in C#.</span><span class="sxs-lookup"><span data-stu-id="8245b-149">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="8245b-150">Tra i caratteri `{` e `}` è racchiuso un blocco di codice C#.</span><span class="sxs-lookup"><span data-stu-id="8245b-150">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="8245b-151">La classe di base `PageModel` contiene una proprietà del dizionario `ViewData` che può essere utilizzata per passare dati a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8245b-151">The `PageModel` base class contains a `ViewData` dictionary property that can be used to pass data to a View.</span></span> <span data-ttu-id="8245b-152">Gli oggetti vengono aggiunti al dizionario `ViewData` usando uno schema chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="8245b-152">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="8245b-153">Nell'esempio precedente la proprietà `"Title"` viene aggiunta al dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="8245b-153">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="8245b-154">La proprietà `"Title"` viene usata nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-154">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="8245b-155">Il markup seguente illustra le prime righe del file *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-155">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="8245b-156">La riga `@*Markup removed for brevity.*@` è un commento Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-156">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="8245b-157">A differenza dei commenti HTML (`<!-- -->`), i commenti Razor non vengono inviati al client.</span><span class="sxs-lookup"><span data-stu-id="8245b-157">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="8245b-158">Aggiornare il layout</span><span class="sxs-lookup"><span data-stu-id="8245b-158">Update the layout</span></span>

<span data-ttu-id="8245b-159">Modificare l'elemento `<title>` nel file *Pages/Shared/_Layout.cshtml* per visualizzare **Movie** anziché **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="8245b-159">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="8245b-160">Trovare l'elemento di ancoraggio seguente nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-160">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="8245b-161">Sostituire l'elemento precedente con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="8245b-161">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="8245b-162">L'elemento di ancoraggio precedente è un [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8245b-162">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="8245b-163">In questo caso, si tratta dell'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8245b-163">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="8245b-164">L'attributo e il valore dell'helper tag `asp-page="/Movies/Index"` creano un collegamento alla pagina Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="8245b-164">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="8245b-165">Poiché il valore dell'attributo `asp-area` è vuoto, l'area non viene usata nel collegamento.</span><span class="sxs-lookup"><span data-stu-id="8245b-165">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="8245b-166">Per altre informazioni, vedere [Aree](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="8245b-166">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="8245b-167">Salvare le modifiche e testare l'app selezionando il collegamento **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="8245b-167">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="8245b-168">In caso di problemi, vedere il file [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) in GitHub.</span><span class="sxs-lookup"><span data-stu-id="8245b-168">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="8245b-169">Testare gli altri collegamenti (**Home**, **RpMovie**, **Create**, **Edit** e **Delete**).</span><span class="sxs-lookup"><span data-stu-id="8245b-169">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="8245b-170">Ogni pagina imposta il titolo, che è possibile visualizzare nella scheda del browser. Quando si aggiunge un segnalibro a una pagina, il titolo viene usato per il segnalibro.</span><span class="sxs-lookup"><span data-stu-id="8245b-170">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="8245b-171">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="8245b-171">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="8245b-172">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese degli Stati Uniti, è necessario eseguire alcuni passaggi per globalizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="8245b-172">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="8245b-173">Vedere questo [problema 4076 su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) per istruzioni sull'aggiunta della virgola decimale.</span><span class="sxs-lookup"><span data-stu-id="8245b-173">See this [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="8245b-174">La proprietà `Layout` viene impostata nel file *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8245b-174">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="8245b-175">Il markup precedente imposta il file di layout su *Pages/Shared/_Layout.cshtml* per tutti i file Razor contenuti nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="8245b-175">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="8245b-176">Vedere [Layout](xref:razor-pages/index#layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="8245b-176">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="8245b-177">Modello di pagina di creazione</span><span class="sxs-lookup"><span data-stu-id="8245b-177">The Create page model</span></span>

<span data-ttu-id="8245b-178">Esaminare il modello di pagina *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8245b-178">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="8245b-179">Il metodo `OnGet` inizializza lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-179">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="8245b-180">La pagina di creazione non possiede uno stato da inizializzare. Viene restituito `Page`.</span><span class="sxs-lookup"><span data-stu-id="8245b-180">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="8245b-181">Più avanti nell'esercitazione viene visualizzato un esempio di inizializzazione dello stato con `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="8245b-181">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="8245b-182">Il metodo `Page` crea un oggetto `PageResult` che esegue il rendering della pagina *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-182">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="8245b-183">La proprietà `Movie` usa l'attributo `[BindProperty]` per consentire l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="8245b-183">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="8245b-184">Dopo che il modulo di creazione registra i valori, il runtime di ASP.NET Core associa i valori registrati al modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="8245b-184">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="8245b-185">Il metodo `OnPostAsync` viene eseguito quando la pagina inserisce i dati del modulo:</span><span class="sxs-lookup"><span data-stu-id="8245b-185">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="8245b-186">Se il modello contiene errori, il modulo viene nuovamente visualizzato insieme ai dati del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="8245b-186">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="8245b-187">La maggior parte degli errori del modello sono riscontrabili sul lato client prima che il modulo sia inviato.</span><span class="sxs-lookup"><span data-stu-id="8245b-187">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="8245b-188">La registrazione di un valore per il campo data non convertibile in una data è un esempio di errore del modello.</span><span class="sxs-lookup"><span data-stu-id="8245b-188">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="8245b-189">Durante l'esercitazione saranno poi trattate nel dettaglio la convalida lato client e la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="8245b-189">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="8245b-190">Se non sono presenti errori nel modello, i dati vengono salvati e il browser viene reindirizzato alla pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="8245b-190">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="8245b-191">Pagina di creazione di Razor</span><span class="sxs-lookup"><span data-stu-id="8245b-191">The Create Razor Page</span></span>

<span data-ttu-id="8245b-192">Esaminare il file della pagina Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8245b-192">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8245b-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8245b-193">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8245b-194">Visual Studio visualizza i tag seguenti con un tipo di carattere in grassetto distintivo specifico per gli helper tag:</span><span class="sxs-lookup"><span data-stu-id="8245b-194">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Visualizzazione di VS17 della pagina Create.cshtml](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8245b-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8245b-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8245b-197">Nel markup precedente sono visualizzati gli helper tag seguenti:</span><span class="sxs-lookup"><span data-stu-id="8245b-197">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8245b-198">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="8245b-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="8245b-199">Visual Studio visualizza i tag seguenti con un tipo di carattere in grassetto distintivo specifico per gli helper tag:</span><span class="sxs-lookup"><span data-stu-id="8245b-199">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="8245b-200">L'elemento `<form method="post">` è un [helper tag del modulo](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8245b-200">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="8245b-201">L'helper tag del modulo include automaticamente un [token antifalsificazione](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="8245b-201">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="8245b-202">Il motore di scaffolding crea un markup Razor per ogni campo nel modello (tranne l'ID) simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8245b-202">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="8245b-203">Gli [helper tag di convalida](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` e `<span asp-validation-for`) visualizzano gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="8245b-203">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="8245b-204">Il tema della convalida viene trattato in modo più dettagliato successivamente in questa serie.</span><span class="sxs-lookup"><span data-stu-id="8245b-204">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="8245b-205">L'[helper tag di etichetta](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera la didascalia dell'etichetta e l'attributo `for` per la proprietà `Title`.</span><span class="sxs-lookup"><span data-stu-id="8245b-205">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="8245b-206">L'[helper tag di input](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) usa gli attributi [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produce gli attributi HTML necessari per la convalida jQuery sul lato client.</span><span class="sxs-lookup"><span data-stu-id="8245b-206">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="8245b-207">Per altre informazioni sugli helper tag, ad esempio `<form method="post">`, vedere [Helper tag in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8245b-207">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8245b-208">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8245b-208">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8245b-209">[Precedente: aggiunta di un modello](xref:tutorials/razor-pages/model)
> [Next: database](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="8245b-209">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8245b-210">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8245b-210">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8245b-211">In questa esercitazione vengono esaminate le pagine Razor create tramite scaffolding nell'[esercitazione precedente](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="8245b-211">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="8245b-212">[Visualizzare o scaricare](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) l'esempio.</span><span class="sxs-lookup"><span data-stu-id="8245b-212">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="8245b-213">Pagine di creazione, eliminazione, dettagli e modifica</span><span class="sxs-lookup"><span data-stu-id="8245b-213">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="8245b-214">Esaminare il modello di pagina *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8245b-214">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="8245b-215">Le pagine Razor vengono derivate da `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8245b-215">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="8245b-216">Per convenzione, la classe derivata `PageModel` viene denominata `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="8245b-216">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="8245b-217">Il costruttore usa l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) per aggiungere `RazorPagesMovieContext` alla pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-217">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="8245b-218">Tutte le pagine create tramite scaffolding seguono questo schema.</span><span class="sxs-lookup"><span data-stu-id="8245b-218">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="8245b-219">Vedere [Codice asincrono](xref:data/ef-rp/intro#asynchronous-code) per altre informazioni sulla programmazione asincrona con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8245b-219">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="8245b-220">Quando per la pagina viene eseguita una richiesta, il metodo `OnGetAsync` restituisce un elenco di filmati alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-220">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="8245b-221">Nella pagina Razor viene chiamato `OnGetAsync`o `OnGet` per inizializzare lo stato della pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-221">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="8245b-222">In questo caso, `OnGetAsync` ottiene un elenco di filmati e li visualizza.</span><span class="sxs-lookup"><span data-stu-id="8245b-222">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="8245b-223">Quando `OnGet` restituisce `void` o `OnGetAsync` restituisce`Task`, non viene usato alcun metodo restituito.</span><span class="sxs-lookup"><span data-stu-id="8245b-223">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="8245b-224">Quando il tipo restituito è `IActionResult` o `Task<IActionResult>`, è necessario specificare un'istruzione return.</span><span class="sxs-lookup"><span data-stu-id="8245b-224">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="8245b-225">Ad esempio, il metodo *Pages/Movies/Create.cshtml.cs* `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="8245b-225">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="8245b-226">Esaminare la pagina Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8245b-226">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="8245b-227">Con Razor è possibile passare da HTML a C# o scegliere un markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-227">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="8245b-228">Quando il simbolo `@` è seguito da una [parola chiave riservata Razor ](xref:mvc/views/razor#razor-reserved-keywords), la transizione avviene in un markup specifico per Razor, altrimenti in C#.</span><span class="sxs-lookup"><span data-stu-id="8245b-228">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="8245b-229">La direttiva Razor `@page` trasforma il file in un'azione MVC in modo che possa gestire le richieste.</span><span class="sxs-lookup"><span data-stu-id="8245b-229">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="8245b-230">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-230">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="8245b-231">`@page` è un esempio di transizione nel markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-231">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="8245b-232">Vedere [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintassi Razor) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="8245b-232">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="8245b-233">Esaminare l'espressione lambda usata nell'helper HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="8245b-233">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="8245b-234">L'helper HTML `DisplayNameFor` controlla la proprietà `Title` a cui fa riferimento nell'espressione lambda per determinare il nome visualizzato.</span><span class="sxs-lookup"><span data-stu-id="8245b-234">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="8245b-235">L'espressione lambda viene controllata anziché valutata.</span><span class="sxs-lookup"><span data-stu-id="8245b-235">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="8245b-236">Non sussiste pertanto violazione di accesso quando `model`, `model.Movie`, o `model.Movie[0]` sono `null` o vuoti.</span><span class="sxs-lookup"><span data-stu-id="8245b-236">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="8245b-237">Quando invece l'espressione lambda viene valutata (ad esempio con `@Html.DisplayFor(modelItem => item.Title)`), vengono valutati i valori proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="8245b-237">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="8245b-238">La direttiva @model</span><span class="sxs-lookup"><span data-stu-id="8245b-238">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="8245b-239">La direttiva `@model` specifica il tipo di modello passato alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-239">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="8245b-240">Nell'esempio precedente la riga `@model` rende disponibile la classe derivata `PageModel` alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="8245b-240">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="8245b-241">Il modello viene usato negli `@Html.DisplayNameFor`helper HTML`@Html.DisplayFor` [ e ](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) nella pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-241">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="8245b-242">Pagina di layout</span><span class="sxs-lookup"><span data-stu-id="8245b-242">The layout page</span></span>

<span data-ttu-id="8245b-243">Selezionare i collegamenti di menu: **RazorPagesMovie**, **Home** e **Privacy**.</span><span class="sxs-lookup"><span data-stu-id="8245b-243">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="8245b-244">Ogni pagina mostra lo stesso layout di menu.</span><span class="sxs-lookup"><span data-stu-id="8245b-244">Each page shows the same menu layout.</span></span> <span data-ttu-id="8245b-245">Il layout di menu viene implementato nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-245">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="8245b-246">Aprire il file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-246">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="8245b-247">I modelli di [layout](xref:mvc/views/layout) consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi di applicarlo in più pagine del sito.</span><span class="sxs-lookup"><span data-stu-id="8245b-247">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="8245b-248">Trovare la riga `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="8245b-248">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="8245b-249">`RenderBody` è un segnaposto dove tutte le visualizzazioni specifiche della pagina vengono presentate, *incapsulate* nella pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="8245b-249">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="8245b-250">Ad esempio, se si seleziona il collegamento **Privacy**, il rendering della visualizzazione **Pages/Privacy.cshtml** viene eseguito all'interno del metodo `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="8245b-250">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="8245b-251">ViewData e layout</span><span class="sxs-lookup"><span data-stu-id="8245b-251">ViewData and layout</span></span>

<span data-ttu-id="8245b-252">Si consideri il seguente codice del file *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8245b-252">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="8245b-253">Il codice evidenziato sopra è un esempio di transizione Razor in C#.</span><span class="sxs-lookup"><span data-stu-id="8245b-253">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="8245b-254">Tra i caratteri `{` e `}` è racchiuso un blocco di codice C#.</span><span class="sxs-lookup"><span data-stu-id="8245b-254">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="8245b-255">La classe di base `PageModel` ha una proprietà del dizionario `ViewData` che può essere usata per aggiungere i dati da passare a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8245b-255">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="8245b-256">Gli oggetti vengono aggiunti al dizionario `ViewData` usando uno schema chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="8245b-256">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="8245b-257">Nell'esempio precedente la proprietà "Title" viene aggiunta al dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="8245b-257">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="8245b-258">La proprietà "Title" viene usata nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-258">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="8245b-259">Il markup seguente illustra le prime righe del file *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-259">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="8245b-260">La riga `@*Markup removed for brevity.*@` è un commento Razor che non viene visualizzato nel file di layout.</span><span class="sxs-lookup"><span data-stu-id="8245b-260">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="8245b-261">A differenza dei commenti HTML (`<!-- -->`), i commenti Razor non vengono inviati al client.</span><span class="sxs-lookup"><span data-stu-id="8245b-261">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="8245b-262">Aggiornare il layout</span><span class="sxs-lookup"><span data-stu-id="8245b-262">Update the layout</span></span>

<span data-ttu-id="8245b-263">Modificare l'elemento `<title>` nel file *Pages/Shared/_Layout.cshtml* per visualizzare **Movie** anziché **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="8245b-263">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="8245b-264">Trovare l'elemento di ancoraggio seguente nel file *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-264">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="8245b-265">Sostituire l'elemento precedente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="8245b-265">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="8245b-266">L'elemento di ancoraggio precedente è un [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8245b-266">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="8245b-267">In questo caso, si tratta dell'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8245b-267">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="8245b-268">L'attributo e il valore dell'helper tag `asp-page="/Movies/Index"` creano un collegamento alla pagina Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="8245b-268">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="8245b-269">Poiché il valore dell'attributo `asp-area` è vuoto, l'area non viene usata nel collegamento.</span><span class="sxs-lookup"><span data-stu-id="8245b-269">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="8245b-270">Per altre informazioni, vedere [Aree](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="8245b-270">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="8245b-271">Salvare le modifiche e testare l'app selezionando il collegamento **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="8245b-271">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="8245b-272">In caso di problemi, vedere il file [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) in GitHub.</span><span class="sxs-lookup"><span data-stu-id="8245b-272">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="8245b-273">Testare gli altri collegamenti (**Home**, **RpMovie**, **Create**, **Edit** e **Delete**).</span><span class="sxs-lookup"><span data-stu-id="8245b-273">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="8245b-274">Ogni pagina imposta il titolo, che è possibile visualizzare nella scheda del browser. Quando si aggiunge un segnalibro a una pagina, il titolo viene usato per il segnalibro.</span><span class="sxs-lookup"><span data-stu-id="8245b-274">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="8245b-275">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="8245b-275">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="8245b-276">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese degli Stati Uniti, è necessario eseguire alcuni passaggi per globalizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="8245b-276">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="8245b-277">[Problema 4076 su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) per istruzioni sull'aggiunta della virgola decimale.</span><span class="sxs-lookup"><span data-stu-id="8245b-277">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="8245b-278">La proprietà `Layout` viene impostata nel file *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8245b-278">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="8245b-279">Il markup precedente imposta il file di layout su *Pages/Shared/_Layout.cshtml* per tutti i file Razor contenuti nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="8245b-279">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="8245b-280">Vedere [Layout](xref:razor-pages/index#layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="8245b-280">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="8245b-281">Modello di pagina di creazione</span><span class="sxs-lookup"><span data-stu-id="8245b-281">The Create page model</span></span>

<span data-ttu-id="8245b-282">Esaminare il modello di pagina *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8245b-282">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="8245b-283">Il metodo `OnGet` inizializza lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="8245b-283">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="8245b-284">La pagina di creazione non possiede uno stato da inizializzare. Viene restituito `Page`.</span><span class="sxs-lookup"><span data-stu-id="8245b-284">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="8245b-285">Più avanti nell'esercitazione viene illustrato lo stato di inizializzazione del metodo `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="8245b-285">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="8245b-286">Il metodo `Page` crea un oggetto `PageResult` che esegue il rendering della pagina *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8245b-286">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="8245b-287">La proprietà `Movie` usa l'attributo `[BindProperty]` per consentire l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="8245b-287">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="8245b-288">Dopo che il modulo di creazione registra i valori, il runtime di ASP.NET Core associa i valori registrati al modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="8245b-288">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="8245b-289">Il metodo `OnPostAsync` viene eseguito quando la pagina inserisce i dati del modulo:</span><span class="sxs-lookup"><span data-stu-id="8245b-289">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="8245b-290">Se il modello contiene errori, il modulo viene nuovamente visualizzato insieme ai dati del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="8245b-290">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="8245b-291">La maggior parte degli errori del modello sono riscontrabili sul lato client prima che il modulo sia inviato.</span><span class="sxs-lookup"><span data-stu-id="8245b-291">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="8245b-292">La registrazione di un valore per il campo data non convertibile in una data è un esempio di errore del modello.</span><span class="sxs-lookup"><span data-stu-id="8245b-292">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="8245b-293">Durante l'esercitazione saranno poi trattate nel dettaglio la convalida lato client e la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="8245b-293">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="8245b-294">Se non sono presenti errori nel modello, i dati vengono salvati e il browser viene reindirizzato alla pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="8245b-294">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="8245b-295">Pagina di creazione di Razor</span><span class="sxs-lookup"><span data-stu-id="8245b-295">The Create Razor Page</span></span>

<span data-ttu-id="8245b-296">Esaminare il file della pagina Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8245b-296">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8245b-297">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8245b-297">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8245b-298">Per visualizzare il tag `<form method="post">`, Visual Studio usa un tipo di carattere in grassetto distintivo specifico per gli helper tag:</span><span class="sxs-lookup"><span data-stu-id="8245b-298">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Visualizzazione di VS17 della pagina Create.cshtml](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8245b-300">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8245b-300">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8245b-301">Per altre informazioni sugli helper tag, ad esempio `<form method="post">`, vedere [Helper tag in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8245b-301">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8245b-302">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="8245b-302">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="8245b-303">Per visualizzare il tag `<form method="post">`, Visual Studio per Mac usa un tipo di carattere in grassetto distintivo specifico per gli helper tag.</span><span class="sxs-lookup"><span data-stu-id="8245b-303">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="8245b-304">L'elemento `<form method="post">` è un [helper tag del modulo](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8245b-304">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="8245b-305">L'helper tag del modulo include automaticamente un [token antifalsificazione](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="8245b-305">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="8245b-306">Il motore di scaffolding crea un markup Razor per ogni campo nel modello (tranne l'ID) simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8245b-306">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="8245b-307">Gli [helper tag di convalida](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` e `<span asp-validation-for`) visualizzano gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="8245b-307">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="8245b-308">Il tema della convalida viene trattato in modo più dettagliato successivamente in questa serie.</span><span class="sxs-lookup"><span data-stu-id="8245b-308">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="8245b-309">L'[helper tag di etichetta](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera la didascalia dell'etichetta e l'attributo `for` per la proprietà `Title`.</span><span class="sxs-lookup"><span data-stu-id="8245b-309">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="8245b-310">L'[helper tag di input](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) usa gli attributi [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produce gli attributi HTML necessari per la convalida jQuery sul lato client.</span><span class="sxs-lookup"><span data-stu-id="8245b-310">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8245b-311">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8245b-311">Additional resources</span></span>

* [<span data-ttu-id="8245b-312">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="8245b-312">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="8245b-313">[Precedente: aggiunta di un modello](xref:tutorials/razor-pages/model)
> [Next: database](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="8245b-313">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end
