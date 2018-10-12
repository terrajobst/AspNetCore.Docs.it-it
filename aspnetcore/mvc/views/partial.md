---
title: Visualizzazioni parziali in ASP.NET Core
author: ardalis
description: Informazioni su come usare le visualizzazioni parziali per suddividere file di markup di grandi dimensioni e ridurre la duplicazione del markup comune nelle pagine Web nelle app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2018
uid: mvc/views/partial
ms.openlocfilehash: a836ed073dfe769fc3cc0cd0622b17937747928b
ms.sourcegitcommit: 70fb7c9d5f2ddfcf4747382a9f7159feca7a6aa7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45601756"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="671eb-103">Visualizzazioni parziali in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="671eb-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="671eb-104">Di [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="671eb-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="671eb-105">Una visualizzazione parziale è un file di markup [Razor](xref:mvc/views/razor) (con estensione *cshtml*) che esegue il rendering dell'output HTML *all'interno* dell'output sottoposto a rendering di un altro file di markup.</span><span class="sxs-lookup"><span data-stu-id="671eb-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="671eb-106">Il termine *visualizzazione parziale* viene usato nell'ambito dello sviluppo di app MVC, in cui i file di markup si chiamano *visualizzazioni*, o di app Razor Pages, dove i file di markup si chiamano *pagine*.</span><span class="sxs-lookup"><span data-stu-id="671eb-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="671eb-107">Questo argomento si riferisce genericamente alle visualizzazioni MVC e alle pagine Razor Pages come *file di markup*.</span><span class="sxs-lookup"><span data-stu-id="671eb-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="671eb-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="671eb-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="671eb-109">Quando usare le visualizzazioni parziali</span><span class="sxs-lookup"><span data-stu-id="671eb-109">When to use partial views</span></span>

<span data-ttu-id="671eb-110">Le visualizzazioni parziali sono un modo efficace di:</span><span class="sxs-lookup"><span data-stu-id="671eb-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="671eb-111">Suddividere file di markup di grandi dimensioni in componenti più piccoli.</span><span class="sxs-lookup"><span data-stu-id="671eb-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="671eb-112">In un file di markup complesso e di grandi dimensioni, composto da diverse parti logiche, gestire ognuna di queste parti isolate in una visualizzazione parziale offre dei vantaggi.</span><span class="sxs-lookup"><span data-stu-id="671eb-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="671eb-113">Il codice del file di markup è infatti gestibile in quanto il markup contiene solo la struttura della pagina complessiva e riferimenti alle visualizzazioni parziali.</span><span class="sxs-lookup"><span data-stu-id="671eb-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="671eb-114">Ridurre la duplicazione del contenuto di markup comune nei file di markup.</span><span class="sxs-lookup"><span data-stu-id="671eb-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="671eb-115">Quando nei vari file di markup vengono usati gli stessi elementi di markup, una visualizzazione parziale rimuove la duplicazione del contenuto di markup in un file di visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="671eb-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="671eb-116">Quando il markup viene modificato nella visualizzazione parziale, aggiorna l'output sottoposto a rendering dei file di markup che usano la visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="671eb-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="671eb-117">Le visualizzazioni parziali non devono essere usate per gestire gli elementi di layout comuni.</span><span class="sxs-lookup"><span data-stu-id="671eb-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="671eb-118">Gli elementi comuni del layout devono essere specificati nei file [_Layout.cshtml](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="671eb-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="671eb-119">Non usare una visualizzazione parziale in cui per il rendering del markup è necessaria una logica di rendering complessa o l'esecuzione di codice.</span><span class="sxs-lookup"><span data-stu-id="671eb-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="671eb-120">Invece di una visualizzazione parziale, usare un [componente di visualizzazione](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="671eb-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="671eb-121">Dichiarare le visualizzazioni parziali</span><span class="sxs-lookup"><span data-stu-id="671eb-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="671eb-122">Una visualizzazione parziale è un file di markup con estensione *cshtml* che si trova nella cartella *Views* (MVC) o *Pages* (Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="671eb-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="671eb-123">In ASP.NET Core MVC, l'elemento <xref:Microsoft.AspNetCore.Mvc.ViewResult> di un controller è in grado di restituire una visualizzazione o una visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="671eb-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="671eb-124">Una capacità analoga è pianificata per Razor Pages in ASP.NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="671eb-124">An analogous capability is planned for Razor Pages in ASP.NET Core 2.2.</span></span> <span data-ttu-id="671eb-125">In Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> può restituire un <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span><span class="sxs-lookup"><span data-stu-id="671eb-125">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span></span> <span data-ttu-id="671eb-126">Il riferimento e il rendering delle visualizzazioni parziali sono descritti nella sezione [Riferimento a una visualizzazione parziale](#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="671eb-126">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="671eb-127">A differenza del rendering di pagine o visualizzazioni MVC, una visualizzazione parziale non esegue *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="671eb-127">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="671eb-128">Per altre informazioni su *_ViewStart.cshtml*, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="671eb-128">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="671eb-129">I nomi dei file di visualizzazione parziale iniziano spesso con un carattere di sottolineatura (`_`).</span><span class="sxs-lookup"><span data-stu-id="671eb-129">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="671eb-130">Questa convenzione di denominazione non è obbligatoria, ma è utile per differenziare visivamente le visualizzazioni parziali dalle visualizzazioni e dalle pagine.</span><span class="sxs-lookup"><span data-stu-id="671eb-130">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span> <span data-ttu-id="671eb-131">Quando il nome file inizia con un carattere di sottolineatura, Razor Pages non elabora il file di markup come pagina Razor Pages, anche quando il markup del file include la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="671eb-131">When the file name starts with an underscore, Razor Pages doesn't process the markup file as a Razor Pages page, even when the file's markup includes the `@page` directive.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="671eb-132">Una visualizzazione parziale è un file di markup con estensione *cshtml* che si trova nella cartella *Views*.</span><span class="sxs-lookup"><span data-stu-id="671eb-132">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="671eb-133">L'elemento <xref:Microsoft.AspNetCore.Mvc.ViewResult> di un controller è in grado di restituire una visualizzazione o una visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="671eb-133">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span>

<span data-ttu-id="671eb-134">A differenza del rendering di visualizzazioni MVC, una visualizzazione parziale non esegue *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="671eb-134">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="671eb-135">Per altre informazioni su *_ViewStart.cshtml*, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="671eb-135">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="671eb-136">I nomi dei file di visualizzazione parziale iniziano spesso con un carattere di sottolineatura (`_`).</span><span class="sxs-lookup"><span data-stu-id="671eb-136">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="671eb-137">Questa convenzione di denominazione non è obbligatoria, ma è utile per differenziare visivamente le visualizzazioni parziali dalle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="671eb-137">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="671eb-138">Riferimento a una visualizzazione parziale</span><span class="sxs-lookup"><span data-stu-id="671eb-138">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="671eb-139">All'interno di un file di markup è possibile fare riferimento a una visualizzazione parziale in diversi modi.</span><span class="sxs-lookup"><span data-stu-id="671eb-139">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="671eb-140">È consigliabile usare uno dei metodi asincroni seguenti nelle app:</span><span class="sxs-lookup"><span data-stu-id="671eb-140">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="671eb-141">Helper per tag parziale</span><span class="sxs-lookup"><span data-stu-id="671eb-141">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="671eb-142">Helper HTML asincrono</span><span class="sxs-lookup"><span data-stu-id="671eb-142">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="671eb-143">All'interno di un file di markup è possibile fare riferimento a una visualizzazione parziale in due modi:</span><span class="sxs-lookup"><span data-stu-id="671eb-143">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="671eb-144">Helper HTML asincrono</span><span class="sxs-lookup"><span data-stu-id="671eb-144">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="671eb-145">Helper HTML sincrono</span><span class="sxs-lookup"><span data-stu-id="671eb-145">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="671eb-146">È consigliabile usare l'[helper HTML asincrono](#asynchronous-html-helper) nelle app.</span><span class="sxs-lookup"><span data-stu-id="671eb-146">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="671eb-147">Helper tag Partial</span><span class="sxs-lookup"><span data-stu-id="671eb-147">Partial Tag Helper</span></span>

<span data-ttu-id="671eb-148">L'[helper tag Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) richiede ASP.NET Core 2.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="671eb-148">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="671eb-149">Questo helper esegue il rendering del contenuto in modo asincrono e usa una sintassi HTML:</span><span class="sxs-lookup"><span data-stu-id="671eb-149">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="671eb-150">Quando è presente un'estensione di file, l'helper tag fa riferimento a una visualizzazione parziale che deve essere contenuta nella stessa cartella in cui si trova il file di markup che chiama la visualizzazione parziale:</span><span class="sxs-lookup"><span data-stu-id="671eb-150">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="671eb-151">L'esempio seguente fa riferimento a una visualizzazione parziale dalla radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="671eb-151">The following example references a partial view from the app root.</span></span> <span data-ttu-id="671eb-152">I percorsi che iniziano con una tilde e una barra (`~/`) o con una barra (`/`) fanno riferimento alla radice dell'app:</span><span class="sxs-lookup"><span data-stu-id="671eb-152">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="671eb-153">**Razor Pages**</span><span class="sxs-lookup"><span data-stu-id="671eb-153">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="671eb-154">**MVC**</span><span class="sxs-lookup"><span data-stu-id="671eb-154">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="671eb-155">L'esempio seguente fa riferimento a una visualizzazione parziale con un percorso relativo:</span><span class="sxs-lookup"><span data-stu-id="671eb-155">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="671eb-156">Per ulteriori informazioni, vedere <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="671eb-156">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="671eb-157">Helper HTML asincrono</span><span class="sxs-lookup"><span data-stu-id="671eb-157">Asynchronous HTML Helper</span></span>

<span data-ttu-id="671eb-158">Quando si usa un helper HTML, la procedura consigliata consiste nell'usare <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="671eb-158">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="671eb-159">`PartialAsync` restituisce un tipo <xref:Microsoft.AspNetCore.Html.IHtmlContent> di cui è stato eseguito il wrapping in un <xref:System.Threading.Tasks.Task`1>.</span><span class="sxs-lookup"><span data-stu-id="671eb-159">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task`1>.</span></span> <span data-ttu-id="671eb-160">Il riferimento al metodo avviene facendo precedere la chiamata attesa da un carattere `@`:</span><span class="sxs-lookup"><span data-stu-id="671eb-160">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="671eb-161">Quando è presente l'estensione di file, l'helper HTML fa riferimento a una visualizzazione parziale che deve essere contenuta nella stessa cartella in cui si trova il file di markup che chiama la visualizzazione parziale:</span><span class="sxs-lookup"><span data-stu-id="671eb-161">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="671eb-162">L'esempio seguente fa riferimento a una visualizzazione parziale dalla radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="671eb-162">The following example references a partial view from the app root.</span></span> <span data-ttu-id="671eb-163">I percorsi che iniziano con una tilde e una barra (`~/`) o con una barra (`/`) fanno riferimento alla radice dell'app:</span><span class="sxs-lookup"><span data-stu-id="671eb-163">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="671eb-164">**Razor Pages**</span><span class="sxs-lookup"><span data-stu-id="671eb-164">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="671eb-165">**MVC**</span><span class="sxs-lookup"><span data-stu-id="671eb-165">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="671eb-166">L'esempio seguente fa riferimento a una visualizzazione parziale con un percorso relativo:</span><span class="sxs-lookup"><span data-stu-id="671eb-166">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="671eb-167">In alternativa è possibile eseguire il rendering di una visualizzazione parziale con <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="671eb-167">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="671eb-168">Questo metodo non restituisce un <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span><span class="sxs-lookup"><span data-stu-id="671eb-168">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="671eb-169">Trasmette l'output sottoposto a rendering direttamente alla risposta.</span><span class="sxs-lookup"><span data-stu-id="671eb-169">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="671eb-170">Poiché il metodo non restituisce un risultato, deve essere chiamato all'interno di un blocco di codice Razor:</span><span class="sxs-lookup"><span data-stu-id="671eb-170">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="671eb-171">Poiché `RenderPartialAsync` trasmette il contenuto sottoposto a rendering, assicura prestazioni migliori in alcuni scenari.</span><span class="sxs-lookup"><span data-stu-id="671eb-171">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="671eb-172">In situazioni in cui le prestazioni sono importanti, eseguire il benchmark della pagina usando entrambi gli approcci e usare quindi l'approccio che genera una risposta più rapidamente.</span><span class="sxs-lookup"><span data-stu-id="671eb-172">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="671eb-173">Helper HTML sincrono</span><span class="sxs-lookup"><span data-stu-id="671eb-173">Synchronous HTML Helper</span></span>

<span data-ttu-id="671eb-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> e <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> sono gli equivalenti sincroni rispettivamente di `PartialAsync` e `RenderPartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="671eb-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="671eb-175">L'uso degli equivalenti sincroni non è consigliabile perché in alcuni scenari causano un deadlock.</span><span class="sxs-lookup"><span data-stu-id="671eb-175">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="671eb-176">I metodi sincroni verranno rimossi in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="671eb-176">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="671eb-177">Se è necessario eseguire codice, usare un [componente di visualizzazione](xref:mvc/views/view-components) invece di una visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="671eb-177">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="671eb-178">La chiamata di `Partial` o `RenderPartial` genera un avviso di Visual Studio Analyzer.</span><span class="sxs-lookup"><span data-stu-id="671eb-178">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="671eb-179">Ad esempio, la presenza di `Partial` produce il messaggio di avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="671eb-179">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="671eb-180">L'uso di IHtmlHelper.Partial potrebbe determinare deadlock dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="671eb-180">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="671eb-181">È consigliabile usare l'helper tag &lt;Partial&gt; o IHtmlHelper.PartialAsync.</span><span class="sxs-lookup"><span data-stu-id="671eb-181">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="671eb-182">Sostituire le chiamate a `@Html.Partial` con `@await Html.PartialAsync` o con l'[helper tag Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="671eb-182">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="671eb-183">Per altre informazioni sulla migrazione dell'helper tag Partial, vedere [Eseguire la migrazione da un helper HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span><span class="sxs-lookup"><span data-stu-id="671eb-183">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="671eb-184">Individuazione delle visualizzazioni parziali</span><span class="sxs-lookup"><span data-stu-id="671eb-184">Partial view discovery</span></span>

<span data-ttu-id="671eb-185">Quando si fa riferimento a una visualizzazione parziale per nome senza un'estensione di file, viene eseguita una ricerca nelle posizioni seguenti, nell'ordine indicato:</span><span class="sxs-lookup"><span data-stu-id="671eb-185">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="671eb-186">**Razor Pages**</span><span class="sxs-lookup"><span data-stu-id="671eb-186">**Razor Pages**</span></span>

1. <span data-ttu-id="671eb-187">Cartella della pagina attualmente in esecuzione</span><span class="sxs-lookup"><span data-stu-id="671eb-187">Currently executing page's folder</span></span>
1. <span data-ttu-id="671eb-188">Grafico delle directory sopra la cartella della pagina</span><span class="sxs-lookup"><span data-stu-id="671eb-188">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="671eb-189">**MVC**</span><span class="sxs-lookup"><span data-stu-id="671eb-189">**MVC**</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

<span data-ttu-id="671eb-190">Le convenzioni seguenti si applicano all'individuazione delle visualizzazioni parziali:</span><span class="sxs-lookup"><span data-stu-id="671eb-190">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="671eb-191">Sono consentite visualizzazioni parziali diverse con lo stesso nome purché si trovino in cartelle diverse.</span><span class="sxs-lookup"><span data-stu-id="671eb-191">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="671eb-192">Quando si fa riferimento a una visualizzazione parziale per nome senza un'estensione di file e la visualizzazione parziale è presente sia nella cartella del chiamante sia nella cartella *Shared*, viene usata la visualizzazione parziale nella cartella del chiamante.</span><span class="sxs-lookup"><span data-stu-id="671eb-192">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="671eb-193">Se la visualizzazione parziale non è presente nella cartella del chiamante, viene usata la visualizzazione parziale nella cartella *Shared*.</span><span class="sxs-lookup"><span data-stu-id="671eb-193">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="671eb-194">Le visualizzazioni parziali nella cartella *Shared* sono dette *visualizzazioni parziali condivise* o *visualizzazioni parziali predefinite*.</span><span class="sxs-lookup"><span data-stu-id="671eb-194">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="671eb-195">Le visualizzazioni parziali possono essere *concatenate*, che significa che una visualizzazione parziale può chiamarne un'altra se le chiamate non formano un riferimento circolare.</span><span class="sxs-lookup"><span data-stu-id="671eb-195">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="671eb-196">I percorsi relativi sono sempre relativi al file corrente, non alla radice o al padre del file.</span><span class="sxs-lookup"><span data-stu-id="671eb-196">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="671eb-197">Un oggetto [Razor](xref:mvc/views/razor) `section` definito in una visualizzazione parziale non è visibile ai file di markup padre.</span><span class="sxs-lookup"><span data-stu-id="671eb-197">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="671eb-198">L'oggetto `section` è visibile solo per la visualizzazione parziale in cui è definito.</span><span class="sxs-lookup"><span data-stu-id="671eb-198">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="671eb-199">Accesso ai dati da visualizzazioni parziali</span><span class="sxs-lookup"><span data-stu-id="671eb-199">Access data from partial views</span></span>

<span data-ttu-id="671eb-200">Quando viene creata un'istanza di una visualizzazione parziale, riceve una *copia* del dizionario `ViewData` del padre.</span><span class="sxs-lookup"><span data-stu-id="671eb-200">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="671eb-201">Gli aggiornamenti apportati ai dati all'interno della visualizzazione parziale non vengono mantenuti per la visualizzazione padre.</span><span class="sxs-lookup"><span data-stu-id="671eb-201">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="671eb-202">Le modifiche a `ViewData` in una visualizzazione parziale vengono perse quando viene restituita la visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="671eb-202">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="671eb-203">L'esempio seguente mostra come passare un'istanza di [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) a una visualizzazione parziale:</span><span class="sxs-lookup"><span data-stu-id="671eb-203">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="671eb-204">È possibile trasmettere un modello in una visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="671eb-204">You can pass a model into a partial view.</span></span> <span data-ttu-id="671eb-205">Il modello può essere un oggetto personalizzato.</span><span class="sxs-lookup"><span data-stu-id="671eb-205">The model can be a custom object.</span></span> <span data-ttu-id="671eb-206">È possibile passare un modello con `PartialAsync` (esegue il rendering di un blocco di contenuto al chiamante) o con `RenderPartialAsync` (trasmette il contenuto all'output):</span><span class="sxs-lookup"><span data-stu-id="671eb-206">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="671eb-207">**Razor Pages**</span><span class="sxs-lookup"><span data-stu-id="671eb-207">**Razor Pages**</span></span>

<span data-ttu-id="671eb-208">Il markup seguente nella pagina di esempio è tratto dalla pagina *Pages/ArticlesRP/ReadRP.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="671eb-208">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="671eb-209">La pagina contiene due visualizzazioni parziali.</span><span class="sxs-lookup"><span data-stu-id="671eb-209">The page contains two partial views.</span></span> <span data-ttu-id="671eb-210">La seconda visualizzazione parziale viene trasmessa a un modello e `ViewData` viene trasmesso alla visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="671eb-210">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="671eb-211">L'overload del costruttore `ViewDataDictionary` viene usato per passare un nuovo dizionario `ViewData` mantenendo il dizionario `ViewData` esistente.</span><span class="sxs-lookup"><span data-stu-id="671eb-211">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-19)]

<span data-ttu-id="671eb-212">*Pages/Shared/_AuthorPartialRP.cshtml* è la prima visualizzazione parziale a cui fa riferimento il file di markup *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="671eb-212">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="671eb-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* è la seconda visualizzazione parziale a cui fa riferimento il file di markup *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="671eb-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="671eb-214">**MVC**</span><span class="sxs-lookup"><span data-stu-id="671eb-214">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="671eb-215">Il markup seguente nell'app di esempio mostra la visualizzazione *Views/Articles/ReadRP.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="671eb-215">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="671eb-216">La visualizzazione contiene due visualizzazioni parziali.</span><span class="sxs-lookup"><span data-stu-id="671eb-216">The view contains two partial views.</span></span> <span data-ttu-id="671eb-217">La seconda visualizzazione parziale viene trasmessa a un modello e `ViewData` viene trasmesso alla visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="671eb-217">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="671eb-218">L'overload del costruttore `ViewDataDictionary` viene usato per passare un nuovo dizionario `ViewData` mantenendo il dizionario `ViewData` esistente.</span><span class="sxs-lookup"><span data-stu-id="671eb-218">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-19)]

<span data-ttu-id="671eb-219">*Views/Shared/_AuthorPartial.cshtml* è la prima visualizzazione parziale a cui fa riferimento il file di markup *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="671eb-219">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="671eb-220">*Views/Articles/_ArticleSection.cshtml* è la seconda visualizzazione parziale a cui fa riferimento il file di markup *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="671eb-220">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="671eb-221">In fase di esecuzione, le visualizzazioni parziali vengono sottoposte a rendering nell'output sottoposto a rendering del file di markup, di cui viene a sua volta eseguito il rendering all'interno dell'elemento *_Layout.cshtml* condiviso.</span><span class="sxs-lookup"><span data-stu-id="671eb-221">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="671eb-222">La prima visualizzazione parziale esegue il rendering del nome dell'autore e della data di pubblicazione dell'articolo:</span><span class="sxs-lookup"><span data-stu-id="671eb-222">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="671eb-223">Abraham Lincoln</span><span class="sxs-lookup"><span data-stu-id="671eb-223">Abraham Lincoln</span></span>
>
> <span data-ttu-id="671eb-224">Questa visualizzazione parziale dal &lt;percorso del file di visualizzazione parziale condivisa&gt;.</span><span class="sxs-lookup"><span data-stu-id="671eb-224">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="671eb-225">11/19/1863 12:00:00 AM</span><span class="sxs-lookup"><span data-stu-id="671eb-225">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="671eb-226">La seconda visualizzazione parziale esegue il rendering delle sezioni dell'articolo:</span><span class="sxs-lookup"><span data-stu-id="671eb-226">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="671eb-227">Section One Index: 0</span><span class="sxs-lookup"><span data-stu-id="671eb-227">Section One Index: 0</span></span>
>
> <span data-ttu-id="671eb-228">Four score and seven years ago ...</span><span class="sxs-lookup"><span data-stu-id="671eb-228">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="671eb-229">Section Two Index: 1</span><span class="sxs-lookup"><span data-stu-id="671eb-229">Section Two Index: 1</span></span>
>
> <span data-ttu-id="671eb-230">Now we are engaged in a great civil war, testing ...</span><span class="sxs-lookup"><span data-stu-id="671eb-230">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="671eb-231">Section Three Index: 2</span><span class="sxs-lookup"><span data-stu-id="671eb-231">Section Three Index: 2</span></span>
>
> <span data-ttu-id="671eb-232">But, in a larger sense, we can not dedicate ...</span><span class="sxs-lookup"><span data-stu-id="671eb-232">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="671eb-233">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="671eb-233">Additional resources</span></span>

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
