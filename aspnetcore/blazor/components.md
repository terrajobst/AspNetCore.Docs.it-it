---
title: Creare e usare ASP.NET Core componenti Razor
author: guardrex
description: Informazioni su come creare e usare i componenti Razor, tra cui la modalità di associazione ai dati, la gestione degli eventi e la gestione dei cicli di vita dei componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/23/2019
no-loc:
- Blazor
uid: blazor/components
ms.openlocfilehash: 764e5e7db995b2dcadccf6d93c826ccf32c9ba04
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681006"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="194ec-103">Creare e usare ASP.NET Core componenti Razor</span><span class="sxs-lookup"><span data-stu-id="194ec-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="194ec-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="194ec-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="194ec-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="194ec-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="194ec-106"> le app vengono compilate usando i *componenti*.</span><span class="sxs-lookup"><span data-stu-id="194ec-106"> apps are built using *components*.</span></span> <span data-ttu-id="194ec-107">Un componente è un blocco di interfaccia utente (UI) autonomo, ad esempio una pagina, una finestra di dialogo o un form.</span><span class="sxs-lookup"><span data-stu-id="194ec-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="194ec-108">Un componente include il markup HTML e la logica di elaborazione necessaria per inserire i dati o rispondere agli eventi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="194ec-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="194ec-109">I componenti sono flessibili e leggeri.</span><span class="sxs-lookup"><span data-stu-id="194ec-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="194ec-110">Possono essere annidati, riutilizzati e condivisi tra i progetti.</span><span class="sxs-lookup"><span data-stu-id="194ec-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="194ec-111">Classi di componenti</span><span class="sxs-lookup"><span data-stu-id="194ec-111">Component classes</span></span>

<span data-ttu-id="194ec-112">I componenti sono implementati in file di componente [Razor](xref:mvc/views/razor) (*Razor*) usando una C# combinazione di e markup HTML.</span><span class="sxs-lookup"><span data-stu-id="194ec-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="194ec-113">Un componente in Blazor viene definito formalmente come *componente Razor*.</span><span class="sxs-lookup"><span data-stu-id="194ec-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="194ec-114">Il nome di un componente deve iniziare con un carattere maiuscolo.</span><span class="sxs-lookup"><span data-stu-id="194ec-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="194ec-115">Ad esempio, *MyCoolComponent. Razor* è valido e *MyCoolComponent. Razor* non è valido.</span><span class="sxs-lookup"><span data-stu-id="194ec-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="194ec-116">L'interfaccia utente per un componente viene definita utilizzando HTML.</span><span class="sxs-lookup"><span data-stu-id="194ec-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="194ec-117">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="194ec-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="194ec-118">Quando viene compilata un'app, il markup C# HTML e la logica di rendering vengono convertiti in una classe Component.</span><span class="sxs-lookup"><span data-stu-id="194ec-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="194ec-119">Il nome della classe generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="194ec-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="194ec-120">I membri della classe del componente vengono definiti in un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="194ec-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="194ec-121">Nel blocco `@code`, lo stato del componente (proprietà, campi) viene specificato con i metodi per la gestione degli eventi o per la definizione di altre logiche dei componenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="194ec-122">È consentito più di un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="194ec-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="194ec-123">Nelle anteprime precedenti di ASP.NET Core 3,0 `@functions` blocchi venivano usati per lo stesso scopo dei blocchi `@code` nei componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="194ec-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="194ec-124">i blocchi di `@functions` continuano a funzionare nei componenti Razor, ma è consigliabile usare il blocco di `@code` in ASP.NET Core 3,0 Preview 6 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="194ec-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="194ec-125">I membri dei componenti possono essere usati come parte della logica di rendering del C# componente usando espressioni che iniziano con `@`.</span><span class="sxs-lookup"><span data-stu-id="194ec-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="194ec-126">Un C# campo, ad esempio, viene sottoposto a rendering anteponendo `@` al nome del campo.</span><span class="sxs-lookup"><span data-stu-id="194ec-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="194ec-127">Nell'esempio seguente viene valutato ed eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="194ec-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="194ec-128">`_headingFontStyle` al valore della proprietà CSS per `font-style`.</span><span class="sxs-lookup"><span data-stu-id="194ec-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="194ec-129">`_headingText` al contenuto dell'elemento `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="194ec-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="194ec-130">Una volta eseguito il rendering iniziale del componente, il componente rigenera l'albero di rendering in risposta agli eventi.</span><span class="sxs-lookup"><span data-stu-id="194ec-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="194ec-131"> confronta quindi il nuovo albero di rendering con quello precedente e applica le modifiche apportate al Document Object Model (DOM) del browser.</span><span class="sxs-lookup"><span data-stu-id="194ec-131"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="194ec-132">I componenti sono C# classi ordinarie e possono essere inseriti in qualsiasi punto all'interno di un progetto.</span><span class="sxs-lookup"><span data-stu-id="194ec-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="194ec-133">I componenti che producono pagine Web in genere risiedono nella cartella *pages* .</span><span class="sxs-lookup"><span data-stu-id="194ec-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="194ec-134">I componenti non di pagina vengono spesso inseriti nella cartella *condivisa* o in una cartella personalizzata aggiunta al progetto.</span><span class="sxs-lookup"><span data-stu-id="194ec-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="194ec-135">Per usare una cartella personalizzata, aggiungere lo spazio dei nomi della cartella personalizzata al componente padre o al file *_Imports. Razor* dell'app.</span><span class="sxs-lookup"><span data-stu-id="194ec-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="194ec-136">Lo spazio dei nomi seguente, ad esempio, rende disponibili i componenti in una cartella di *componenti* quando lo spazio dei nomi radice dell'app è `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="194ec-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="194ec-137">Integrare i componenti nelle app Razor Pages e MVC</span><span class="sxs-lookup"><span data-stu-id="194ec-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="194ec-138">Usare i componenti con le app Razor Pages e MVC esistenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="194ec-139">Non è necessario riscrivere le pagine o le visualizzazioni esistenti per usare i componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="194ec-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="194ec-140">Quando viene eseguito il rendering della pagina o della vista, i componenti vengono prerenderizzati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="194ec-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="194ec-141">Per eseguire il rendering di un componente da una pagina o da una vista, usare l'helper Tag `Component`:</span><span class="sxs-lookup"><span data-stu-id="194ec-141">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="194ec-142">`RenderMode` configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="194ec-142">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="194ec-143">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="194ec-143">Is prerendered into the page.</span></span>
* <span data-ttu-id="194ec-144">Viene sottoposto a rendering come HTML statico nella pagina o se include le informazioni necessarie per avviare un'app Blazor dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="194ec-144">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="194ec-145">Descrizione</span><span class="sxs-lookup"><span data-stu-id="194ec-145">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="194ec-146">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-146">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="194ec-147">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-147">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="194ec-148">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-148">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="194ec-149">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="194ec-149">Output from the component isn't included.</span></span> <span data-ttu-id="194ec-150">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-150">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="194ec-151">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="194ec-151">Renders the component into static HTML.</span></span> |

<span data-ttu-id="194ec-152">Mentre le pagine e le visualizzazioni possono usare i componenti, il contrario non è vero.</span><span class="sxs-lookup"><span data-stu-id="194ec-152">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="194ec-153">I componenti non possono usare scenari di visualizzazione e di pagina specifici, ad esempio visualizzazioni parziali e sezioni.</span><span class="sxs-lookup"><span data-stu-id="194ec-153">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="194ec-154">Per usare la logica dalla visualizzazione parziale in un componente, scomporre la logica di visualizzazione parziale in un componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-154">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="194ec-155">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="194ec-155">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="194ec-156">Per ulteriori informazioni sulla modalità di rendering dei componenti, sullo stato del componente e sull'helper Tag `Component`, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="194ec-156">For more information on how components are rendered, component state, and the `Component` Tag Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

<span data-ttu-id="194ec-157">Per eseguire il rendering di un componente da una pagina o da una vista, usare il metodo helper HTML `RenderComponentAsync<TComponent>`:</span><span class="sxs-lookup"><span data-stu-id="194ec-157">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
```

<span data-ttu-id="194ec-158">`RenderMode` configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="194ec-158">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="194ec-159">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="194ec-159">Is prerendered into the page.</span></span>
* <span data-ttu-id="194ec-160">Viene sottoposto a rendering come HTML statico nella pagina o se include le informazioni necessarie per avviare un'app Blazor dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="194ec-160">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="194ec-161">Descrizione</span><span class="sxs-lookup"><span data-stu-id="194ec-161">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="194ec-162">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-162">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="194ec-163">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-163">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="194ec-164">I parametri non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="194ec-164">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="194ec-165">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-165">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="194ec-166">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="194ec-166">Output from the component isn't included.</span></span> <span data-ttu-id="194ec-167">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-167">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="194ec-168">I parametri non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="194ec-168">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="194ec-169">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="194ec-169">Renders the component into static HTML.</span></span> <span data-ttu-id="194ec-170">I parametri sono supportati.</span><span class="sxs-lookup"><span data-stu-id="194ec-170">Parameters are supported.</span></span> |

<span data-ttu-id="194ec-171">Mentre le pagine e le visualizzazioni possono usare i componenti, il contrario non è vero.</span><span class="sxs-lookup"><span data-stu-id="194ec-171">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="194ec-172">I componenti non possono usare scenari di visualizzazione e di pagina specifici, ad esempio visualizzazioni parziali e sezioni.</span><span class="sxs-lookup"><span data-stu-id="194ec-172">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="194ec-173">Per usare la logica dalla visualizzazione parziale in un componente, scomporre la logica di visualizzazione parziale in un componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-173">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="194ec-174">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="194ec-174">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="194ec-175">Per ulteriori informazioni sulla modalità di rendering dei componenti, sullo stato del componente e sull'helper HTML `RenderComponentAsync`, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="194ec-175">For more information on how components are rendered, component state, and the `RenderComponentAsync` HTML Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

## <a name="use-components"></a><span data-ttu-id="194ec-176">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="194ec-176">Use components</span></span>

<span data-ttu-id="194ec-177">I componenti possono includere altri componenti dichiarando questi ultimi usando la sintassi degli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="194ec-177">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="194ec-178">Il markup per l'uso di un componente è simile a un tag HTML, in cui il nome del tag è il tipo di componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-178">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="194ec-179">L'associazione di attributi distingue tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="194ec-179">Attribute binding is case sensitive.</span></span> <span data-ttu-id="194ec-180">Ad esempio, `@bind` è valido e `@Bind` non è valido.</span><span class="sxs-lookup"><span data-stu-id="194ec-180">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="194ec-181">Il markup seguente in *index. Razor* esegue il rendering di un'istanza di `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="194ec-181">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="194ec-182">*Components/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-182">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="194ec-183">Se un componente contiene un elemento HTML con una prima lettera maiuscola che non corrisponde a un nome di componente, viene emesso un avviso che indica che l'elemento ha un nome imprevisto.</span><span class="sxs-lookup"><span data-stu-id="194ec-183">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="194ec-184">L'aggiunta di un'istruzione `@using` per lo spazio dei nomi del componente rende disponibile il componente, che rimuove l'avviso.</span><span class="sxs-lookup"><span data-stu-id="194ec-184">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="194ec-185">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="194ec-185">Component parameters</span></span>

<span data-ttu-id="194ec-186">I componenti possono avere *parametri del componente*, che vengono definiti usando proprietà pubbliche nella classe Component con l'attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="194ec-186">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="194ec-187">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="194ec-187">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="194ec-188">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-188">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="194ec-189">Nell'esempio seguente, il `ParentComponent` imposta il valore della proprietà `Title` dell'`ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="194ec-189">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="194ec-190">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-190">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="194ec-191">Contenuto figlio</span><span class="sxs-lookup"><span data-stu-id="194ec-191">Child content</span></span>

<span data-ttu-id="194ec-192">I componenti possono impostare il contenuto di un altro componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-192">Components can set the content of another component.</span></span> <span data-ttu-id="194ec-193">Il componente di assegnazione fornisce il contenuto tra i tag che specificano il componente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="194ec-193">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="194ec-194">Nell'esempio seguente, il `ChildComponent` dispone di una proprietà `ChildContent` che rappresenta un `RenderFragment`, che rappresenta un segmento di interfaccia utente di cui eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="194ec-194">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="194ec-195">Il valore di `ChildContent` è posizionato nel markup del componente in cui deve essere eseguito il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="194ec-195">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="194ec-196">Il valore di `ChildContent` viene ricevuto dal componente padre e ne viene eseguito il rendering all'interno del `panel-body`del pannello bootstrap.</span><span class="sxs-lookup"><span data-stu-id="194ec-196">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="194ec-197">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-197">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="194ec-198">La proprietà che riceve il contenuto di `RenderFragment` deve essere denominata `ChildContent` per convenzione.</span><span class="sxs-lookup"><span data-stu-id="194ec-198">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="194ec-199">Il `ParentComponent` seguente può fornire contenuti per il rendering del `ChildComponent` inserendo il contenuto all'interno dei tag `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="194ec-199">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="194ec-200">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-200">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="194ec-201">Attributo splatting e parametri arbitrari</span><span class="sxs-lookup"><span data-stu-id="194ec-201">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="194ec-202">I componenti possono acquisire ed eseguire il rendering di attributi aggiuntivi oltre ai parametri dichiarati del componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-202">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="194ec-203">È possibile acquisire attributi aggiuntivi in un dizionario e quindi *Splatted* su un elemento quando il componente viene sottoposto a rendering usando la direttiva Razor [@attributes](xref:mvc/views/razor#attributes) .</span><span class="sxs-lookup"><span data-stu-id="194ec-203">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="194ec-204">Questo scenario è utile quando si definisce un componente che produce un elemento di markup che supporta un'ampia gamma di personalizzazioni.</span><span class="sxs-lookup"><span data-stu-id="194ec-204">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="194ec-205">Ad esempio, può essere noioso definire gli attributi separatamente per un `<input>` che supporta molti parametri.</span><span class="sxs-lookup"><span data-stu-id="194ec-205">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="194ec-206">Nell'esempio seguente, il primo elemento `<input>` (`id="useIndividualParams"`) USA parametri dei singoli componenti, mentre il secondo elemento `<input>` (`id="useAttributesDict"`) usa l'attributo splatting:</span><span class="sxs-lookup"><span data-stu-id="194ec-206">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```cshtml
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="194ec-207">Il tipo del parametro deve implementare `IEnumerable<KeyValuePair<string, object>>` con chiavi di stringa.</span><span class="sxs-lookup"><span data-stu-id="194ec-207">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="194ec-208">In questo scenario è inoltre possibile utilizzare `IReadOnlyDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="194ec-208">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="194ec-209">Gli elementi `<input>` sottoposti a rendering usando entrambi gli approcci sono identici:</span><span class="sxs-lookup"><span data-stu-id="194ec-209">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="194ec-210">Per accettare attributi arbitrari, definire un parametro component usando l'attributo `[Parameter]` con la proprietà `CaptureUnmatchedValues` impostata su `true`:</span><span class="sxs-lookup"><span data-stu-id="194ec-210">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="194ec-211">La proprietà `CaptureUnmatchedValues` in `[Parameter]` consente al parametro di trovare la corrispondenza con tutti gli attributi che non corrispondono ad altri parametri.</span><span class="sxs-lookup"><span data-stu-id="194ec-211">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="194ec-212">Un componente può definire un solo parametro con `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="194ec-212">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="194ec-213">Il tipo di proprietà utilizzato con `CaptureUnmatchedValues` deve essere assegnabile da `Dictionary<string, object>` con chiavi di stringa.</span><span class="sxs-lookup"><span data-stu-id="194ec-213">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="194ec-214">in questo scenario `IEnumerable<KeyValuePair<string, object>>` o `IReadOnlyDictionary<string, object>` sono inoltre disponibili opzioni.</span><span class="sxs-lookup"><span data-stu-id="194ec-214">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="194ec-215">La posizione di `@attributes` rispetto alla posizione degli attributi degli elementi è importante.</span><span class="sxs-lookup"><span data-stu-id="194ec-215">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="194ec-216">Quando `@attributes` sono Splatted sull'elemento, gli attributi vengono elaborati da destra a sinistra (dall'ultimo al primo).</span><span class="sxs-lookup"><span data-stu-id="194ec-216">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="194ec-217">Si consideri l'esempio seguente di un componente che utilizza un componente `Child`:</span><span class="sxs-lookup"><span data-stu-id="194ec-217">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="194ec-218">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-218">*ParentComponent.razor*:</span></span>

```cshtml
<ChildComponent extra="10" />
```

<span data-ttu-id="194ec-219">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-219">*ChildComponent.razor*:</span></span>

```cshtml
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="194ec-220">L'attributo `extra` del componente `Child` è impostato a destra di `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="194ec-220">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="194ec-221">Il rendering del componente `Parent` `<div>` contiene `extra="5"` quando viene passato attraverso l'attributo aggiuntivo, perché gli attributi vengono elaborati da destra a sinistra (dall'ultimo al primo):</span><span class="sxs-lookup"><span data-stu-id="194ec-221">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="194ec-222">Nell'esempio seguente, l'ordine delle `extra` e `@attributes` viene invertito nell'`<div>`del componente `Child`:</span><span class="sxs-lookup"><span data-stu-id="194ec-222">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="194ec-223">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-223">*ParentComponent.razor*:</span></span>

```cshtml
<ChildComponent extra="10" />
```

<span data-ttu-id="194ec-224">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-224">*ChildComponent.razor*:</span></span>

```cshtml
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="194ec-225">Il `<div>` sottoposto a rendering nel componente `Parent` contiene `extra="10"` quando viene passato tramite l'attributo aggiuntivo:</span><span class="sxs-lookup"><span data-stu-id="194ec-225">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="194ec-226">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="194ec-226">Data binding</span></span>

<span data-ttu-id="194ec-227">L'associazione dati a entrambi i componenti e gli elementi DOM viene eseguita con l'attributo [@bind](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="194ec-227">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="194ec-228">Nell'esempio seguente viene associato un `CurrentValue` proprietà al valore della casella di testo:</span><span class="sxs-lookup"><span data-stu-id="194ec-228">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="194ec-229">Quando la casella di testo perde lo stato attivo, viene aggiornato il valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="194ec-229">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="194ec-230">La casella di testo viene aggiornata nell'interfaccia utente solo quando viene eseguito il rendering del componente, non in risposta alla modifica del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="194ec-230">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="194ec-231">Poiché i componenti eseguono il rendering dopo l'esecuzione del codice del gestore eventi, gli aggiornamenti delle proprietà vengono in *genere* riflessi nell'interfaccia utente immediatamente dopo l'attivazione di un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="194ec-231">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="194ec-232">L'uso di `@bind` con la proprietà `CurrentValue` (`<input @bind="CurrentValue" />`) è essenzialmente equivalente a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="194ec-232">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="194ec-233">Quando viene eseguito il rendering del componente, il `value` dell'elemento di input deriva dalla proprietà `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="194ec-233">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="194ec-234">Quando l'utente digita nella casella di testo e modifica lo stato attivo dell'elemento, viene generato l'evento `onchange` e la proprietà `CurrentValue` viene impostata sul valore modificato.</span><span class="sxs-lookup"><span data-stu-id="194ec-234">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="194ec-235">In realtà, la generazione del codice è più complessa perché `@bind` gestisce i casi in cui vengono eseguite le conversioni dei tipi.</span><span class="sxs-lookup"><span data-stu-id="194ec-235">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="194ec-236">In linea di principio, `@bind` associa il valore corrente di un'espressione a un attributo `value` e gestisce le modifiche utilizzando il gestore registrato.</span><span class="sxs-lookup"><span data-stu-id="194ec-236">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="194ec-237">Oltre a gestire gli eventi di `onchange` con `@bind` sintassi, una proprietà o un campo può essere associato utilizzando altri eventi specificando un attributo [@bind-value](xref:mvc/views/razor#bind) con un parametro `event` ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="194ec-237">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="194ec-238">Nell'esempio seguente viene associata la proprietà `CurrentValue` per l'evento `oninput`:</span><span class="sxs-lookup"><span data-stu-id="194ec-238">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="194ec-239">A differenza `onchange`, che viene attivato quando l'elemento perde lo stato attivo, `oninput` generato quando viene modificato il valore della casella di testo.</span><span class="sxs-lookup"><span data-stu-id="194ec-239">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="194ec-240">**Valori non analizzabili**</span><span class="sxs-lookup"><span data-stu-id="194ec-240">**Unparsable values**</span></span>

<span data-ttu-id="194ec-241">Quando un utente fornisce un valore non analizzabile a un elemento associato a un oggetto DataBound, il valore non analizzabile viene automaticamente ripristinato al valore precedente quando viene attivato l'evento bind.</span><span class="sxs-lookup"><span data-stu-id="194ec-241">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="194ec-242">Si consideri lo scenario seguente:</span><span class="sxs-lookup"><span data-stu-id="194ec-242">Consider the following scenario:</span></span>

* <span data-ttu-id="194ec-243">Un elemento `<input>` è associato a un tipo di `int` con un valore iniziale di `123`:</span><span class="sxs-lookup"><span data-stu-id="194ec-243">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="194ec-244">L'utente aggiorna il valore dell'elemento per `123.45` nella pagina e modifica lo stato attivo dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="194ec-244">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="194ec-245">Nello scenario precedente, il valore dell'elemento viene ripristinato in `123`.</span><span class="sxs-lookup"><span data-stu-id="194ec-245">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="194ec-246">Quando il valore `123.45` viene rifiutato a favore del valore originale di `123`, l'utente riconosce che il relativo valore non è stato accettato.</span><span class="sxs-lookup"><span data-stu-id="194ec-246">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="194ec-247">Per impostazione predefinita, l'associazione si applica all'evento `onchange` dell'elemento (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="194ec-247">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="194ec-248">Usare `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` per impostare un evento diverso.</span><span class="sxs-lookup"><span data-stu-id="194ec-248">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="194ec-249">Per l'evento `oninput` (`@bind-value:event="oninput"`), la riversione viene eseguita dopo qualsiasi sequenza di tasti che introduce un valore non analizzabile.</span><span class="sxs-lookup"><span data-stu-id="194ec-249">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="194ec-250">Quando la destinazione è `oninput` evento con un tipo associato a `int`, a un utente viene impedito di digitare un `.` carattere.</span><span class="sxs-lookup"><span data-stu-id="194ec-250">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="194ec-251">Un carattere `.` viene immediatamente rimosso, quindi l'utente riceve il feedback immediato che sono consentiti solo numeri interi.</span><span class="sxs-lookup"><span data-stu-id="194ec-251">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="194ec-252">Esistono scenari in cui il ripristino del valore nell'evento `oninput` non è ideale, ad esempio quando l'utente deve essere autorizzato a cancellare un valore `<input>` non analizzabile.</span><span class="sxs-lookup"><span data-stu-id="194ec-252">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="194ec-253">Le alternative includono:</span><span class="sxs-lookup"><span data-stu-id="194ec-253">Alternatives include:</span></span>

* <span data-ttu-id="194ec-254">Non usare l'evento `oninput`.</span><span class="sxs-lookup"><span data-stu-id="194ec-254">Don't use the `oninput` event.</span></span> <span data-ttu-id="194ec-255">Usare l'evento `onchange` predefinito (`@bind="{PROPERTY OR FIELD}"`), in cui un valore non valido non viene ripristinato fino a quando l'elemento non perde lo stato attivo.</span><span class="sxs-lookup"><span data-stu-id="194ec-255">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="194ec-256">Eseguire l'associazione a un tipo nullable, ad esempio `int?` o `string`, e fornire la logica personalizzata per gestire le voci non valide.</span><span class="sxs-lookup"><span data-stu-id="194ec-256">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="194ec-257">Utilizzare un [componente di convalida del modulo](xref:blazor/forms-validation), ad esempio `InputNumber` o `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="194ec-257">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="194ec-258">I componenti di convalida dei moduli includono il supporto predefinito per la gestione di input non validi.</span><span class="sxs-lookup"><span data-stu-id="194ec-258">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="194ec-259">Componenti di convalida dei moduli:</span><span class="sxs-lookup"><span data-stu-id="194ec-259">Form validation components:</span></span>
  * <span data-ttu-id="194ec-260">Consente all'utente di fornire un input non valido e di ricevere errori di convalida nel `EditContext`associato.</span><span class="sxs-lookup"><span data-stu-id="194ec-260">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="194ec-261">Visualizzare gli errori di convalida nell'interfaccia utente senza interferire con l'utente che immette dati Web Form aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="194ec-261">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="194ec-262">**Globalizzazione**</span><span class="sxs-lookup"><span data-stu-id="194ec-262">**Globalization**</span></span>

<span data-ttu-id="194ec-263">`@bind` valori sono formattati per la visualizzazione e l'analisi usando le regole delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="194ec-263">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="194ec-264">È possibile accedere alle impostazioni cultura correnti dalla proprietà <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="194ec-264">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="194ec-265">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) viene usato per i tipi di campo seguenti (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="194ec-265">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="194ec-266">I tipi di campo precedenti:</span><span class="sxs-lookup"><span data-stu-id="194ec-266">The preceding field types:</span></span>

* <span data-ttu-id="194ec-267">Vengono visualizzati utilizzando le regole di formattazione appropriate basate su browser.</span><span class="sxs-lookup"><span data-stu-id="194ec-267">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="194ec-268">Non può contenere testo in formato libero.</span><span class="sxs-lookup"><span data-stu-id="194ec-268">Can't contain free-form text.</span></span>
* <span data-ttu-id="194ec-269">Fornire le caratteristiche di interazione utente in base all'implementazione del browser.</span><span class="sxs-lookup"><span data-stu-id="194ec-269">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="194ec-270">I tipi di campo seguenti hanno requisiti di formattazione specifici e non sono attualmente supportati da Blazor perché non sono supportati da tutti i browser principali:</span><span class="sxs-lookup"><span data-stu-id="194ec-270">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="194ec-271">`@bind` supporta il parametro `@bind:culture` per fornire un <xref:System.Globalization.CultureInfo?displayProperty=fullName> per l'analisi e la formattazione di un valore.</span><span class="sxs-lookup"><span data-stu-id="194ec-271">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="194ec-272">Non è consigliabile specificare impostazioni cultura quando si usano i tipi di campo `date` e `number`.</span><span class="sxs-lookup"><span data-stu-id="194ec-272">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="194ec-273">`date` e `number` hanno un supporto incorporato Blazor che fornisce le impostazioni cultura richieste.</span><span class="sxs-lookup"><span data-stu-id="194ec-273">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="194ec-274">Per informazioni su come impostare le impostazioni cultura dell'utente, vedere la sezione [localizzazione](#localization) .</span><span class="sxs-lookup"><span data-stu-id="194ec-274">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="194ec-275">**Stringhe di formato**</span><span class="sxs-lookup"><span data-stu-id="194ec-275">**Format strings**</span></span>

<span data-ttu-id="194ec-276">Il data binding funziona con stringhe di formato <xref:System.DateTime> usando [@bind:format](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="194ec-276">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="194ec-277">Altre espressioni di formato, ad esempio i formati di valuta o numerici, non sono disponibili in questo momento.</span><span class="sxs-lookup"><span data-stu-id="194ec-277">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="194ec-278">Nel codice precedente, il tipo di campo dell'elemento `<input>` (`type`) viene impostato come valore predefinito `text`.</span><span class="sxs-lookup"><span data-stu-id="194ec-278">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="194ec-279">`@bind:format` è supportato per l'associazione dei tipi .NET seguenti:</span><span class="sxs-lookup"><span data-stu-id="194ec-279">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="194ec-280"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="194ec-280"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="194ec-281"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="194ec-281"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="194ec-282">L'attributo `@bind:format` specifica il formato di data da applicare al `value` dell'elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="194ec-282">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="194ec-283">Il formato viene usato anche per analizzare il valore quando si verifica un evento `onchange`.</span><span class="sxs-lookup"><span data-stu-id="194ec-283">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="194ec-284">Non è consigliabile specificare un formato per il tipo di campo `date` perché Blazor dispone del supporto incorporato per formattare le date.</span><span class="sxs-lookup"><span data-stu-id="194ec-284">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="194ec-285">**Parametri dei componenti**</span><span class="sxs-lookup"><span data-stu-id="194ec-285">**Component parameters**</span></span>

<span data-ttu-id="194ec-286">Il binding riconosce i parametri del componente, dove `@bind-{property}` può associare un valore della proprietà tra i componenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-286">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="194ec-287">Il componente figlio seguente (`ChildComponent`) dispone di un parametro del componente `Year` e di un callback `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="194ec-287">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="194ec-288">`EventCallback<T>` è illustrato nella sezione [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="194ec-288">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="194ec-289">Il componente padre seguente utilizza `ChildComponent` e associa il parametro `ParentYear` dal padre al parametro `Year` sul componente figlio:</span><span class="sxs-lookup"><span data-stu-id="194ec-289">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="194ec-290">Il caricamento del `ParentComponent` produce il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="194ec-290">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="194ec-291">Se il valore della proprietà `ParentYear` viene modificato selezionando il pulsante nell'`ParentComponent`, viene aggiornata la proprietà `Year` del `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="194ec-291">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="194ec-292">Il rendering del nuovo valore di `Year` viene eseguito nell'interfaccia utente quando viene eseguito il rendering del `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="194ec-292">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="194ec-293">Il parametro `Year` è associabile perché contiene un evento `YearChanged` complementare che corrisponde al tipo del parametro `Year`.</span><span class="sxs-lookup"><span data-stu-id="194ec-293">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="194ec-294">Per convenzione, `<ChildComponent @bind-Year="ParentYear" />` è essenzialmente equivalente alla scrittura:</span><span class="sxs-lookup"><span data-stu-id="194ec-294">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="194ec-295">In generale, una proprietà può essere associata a un gestore eventi corrispondente usando `@bind-property:event` attributo.</span><span class="sxs-lookup"><span data-stu-id="194ec-295">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="194ec-296">Ad esempio, la proprietà `MyProp` può essere associata a `MyEventHandler` utilizzando i due attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="194ec-296">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="194ec-297">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="194ec-297">Event handling</span></span>

<span data-ttu-id="194ec-298">I componenti Razor forniscono funzionalità di gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="194ec-298">Razor components provide event handling features.</span></span> <span data-ttu-id="194ec-299">Per un attributo dell'elemento HTML denominato `on{EVENT}` (ad esempio, `onclick` e `onsubmit`) con un valore tipizzato dal delegato, i componenti Razor considera il valore dell'attributo come un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="194ec-299">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="194ec-300">Il nome dell'attributo è sempre formattato [@on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="194ec-300">The attribute's name is always formatted [@on{EVENT}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="194ec-301">Il codice seguente chiama il metodo `UpdateHeading` quando si seleziona il pulsante nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="194ec-301">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="194ec-302">Il codice seguente chiama il metodo `CheckChanged` quando la casella di controllo viene modificata nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="194ec-302">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="194ec-303">I gestori di eventi possono anche essere asincroni e restituire un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="194ec-303">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="194ec-304">Non è necessario chiamare manualmente [StateHasChanged](xref:blazor/lifecycle#state-changes).</span><span class="sxs-lookup"><span data-stu-id="194ec-304">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="194ec-305">Le eccezioni vengono registrate quando si verificano.</span><span class="sxs-lookup"><span data-stu-id="194ec-305">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="194ec-306">Nell'esempio seguente `UpdateHeading` viene chiamato in modo asincrono quando si seleziona il pulsante:</span><span class="sxs-lookup"><span data-stu-id="194ec-306">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="194ec-307">Tipi di argomenti dell'evento</span><span class="sxs-lookup"><span data-stu-id="194ec-307">Event argument types</span></span>

<span data-ttu-id="194ec-308">Per alcuni eventi, i tipi di argomento dell'evento sono consentiti.</span><span class="sxs-lookup"><span data-stu-id="194ec-308">For some events, event argument types are permitted.</span></span> <span data-ttu-id="194ec-309">Se l'accesso a uno di questi tipi di evento non è necessario, non è obbligatorio nella chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="194ec-309">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="194ec-310">I `EventArgs` supportati sono riportati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="194ec-310">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="194ec-311">Event</span><span class="sxs-lookup"><span data-stu-id="194ec-311">Event</span></span>            | <span data-ttu-id="194ec-312">Classe</span><span class="sxs-lookup"><span data-stu-id="194ec-312">Class</span></span>                | <span data-ttu-id="194ec-313">Eventi e note DOM</span><span class="sxs-lookup"><span data-stu-id="194ec-313">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="194ec-314">Appunti</span><span class="sxs-lookup"><span data-stu-id="194ec-314">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="194ec-315">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="194ec-315">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="194ec-316">Trascinare</span><span class="sxs-lookup"><span data-stu-id="194ec-316">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="194ec-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="194ec-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="194ec-318">`DataTransfer` e `DataTransferItem` contengono dati di elementi trascinati.</span><span class="sxs-lookup"><span data-stu-id="194ec-318">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="194ec-319">Errore di</span><span class="sxs-lookup"><span data-stu-id="194ec-319">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="194ec-320">Event</span><span class="sxs-lookup"><span data-stu-id="194ec-320">Event</span></span>            | `EventArgs`          | <span data-ttu-id="194ec-321">*Generalee*</span><span class="sxs-lookup"><span data-stu-id="194ec-321">*General*</span></span><br><span data-ttu-id="194ec-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="194ec-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="194ec-323">*Appunti*</span><span class="sxs-lookup"><span data-stu-id="194ec-323">*Clipboard*</span></span><br><span data-ttu-id="194ec-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="194ec-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="194ec-325">*Input*</span><span class="sxs-lookup"><span data-stu-id="194ec-325">*Input*</span></span><br><span data-ttu-id="194ec-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="194ec-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="194ec-327">*Media*</span><span class="sxs-lookup"><span data-stu-id="194ec-327">*Media*</span></span><br><span data-ttu-id="194ec-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="194ec-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="194ec-329">Stato attivo</span><span class="sxs-lookup"><span data-stu-id="194ec-329">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="194ec-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="194ec-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="194ec-331">Non include il supporto per `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="194ec-331">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="194ec-332">Input</span><span class="sxs-lookup"><span data-stu-id="194ec-332">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="194ec-333">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="194ec-333">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="194ec-334">Tastiera</span><span class="sxs-lookup"><span data-stu-id="194ec-334">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="194ec-335">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="194ec-335">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="194ec-336">Mouse</span><span class="sxs-lookup"><span data-stu-id="194ec-336">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="194ec-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="194ec-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="194ec-338">Puntatore del mouse</span><span class="sxs-lookup"><span data-stu-id="194ec-338">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="194ec-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="194ec-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="194ec-340">Rotellina del mouse</span><span class="sxs-lookup"><span data-stu-id="194ec-340">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="194ec-341">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="194ec-341">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="194ec-342">Stato</span><span class="sxs-lookup"><span data-stu-id="194ec-342">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="194ec-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="194ec-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="194ec-344">Tocco</span><span class="sxs-lookup"><span data-stu-id="194ec-344">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="194ec-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="194ec-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="194ec-346">`TouchPoint` rappresenta un singolo punto di contatto su un dispositivo sensibile al tocco.</span><span class="sxs-lookup"><span data-stu-id="194ec-346">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="194ec-347">Per informazioni sulle proprietà e sul comportamento di gestione degli eventi degli eventi nella tabella precedente, vedere le [classi EventArgs nell'origine riferimento (ASPNET/AspNetCore Release/3.0 Branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="194ec-347">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="194ec-348">Espressioni lambda</span><span class="sxs-lookup"><span data-stu-id="194ec-348">Lambda expressions</span></span>

<span data-ttu-id="194ec-349">È inoltre possibile utilizzare le espressioni lambda:</span><span class="sxs-lookup"><span data-stu-id="194ec-349">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="194ec-350">Spesso è consigliabile chiudere i valori aggiuntivi, ad esempio quando si esegue l'iterazione su un set di elementi.</span><span class="sxs-lookup"><span data-stu-id="194ec-350">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="194ec-351">Nell'esempio seguente vengono creati tre pulsanti, ciascuno dei quali chiama `UpdateHeading` passando un argomento di evento (`MouseEventArgs`) e il relativo numero di pulsante (`buttonNumber`) quando viene selezionato nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="194ec-351">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="194ec-352">**Non** usare la variabile di ciclo (`i`) in un ciclo di `for` direttamente in un'espressione lambda.</span><span class="sxs-lookup"><span data-stu-id="194ec-352">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="194ec-353">In caso contrario, la stessa variabile viene utilizzata da tutte le espressioni lambda causando che il valore di `i`sia lo stesso in tutte le espressioni lambda.</span><span class="sxs-lookup"><span data-stu-id="194ec-353">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="194ec-354">Acquisire sempre il relativo valore in una variabile locale (`buttonNumber` nell'esempio precedente) e quindi usarlo.</span><span class="sxs-lookup"><span data-stu-id="194ec-354">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="194ec-355">EventCallback</span><span class="sxs-lookup"><span data-stu-id="194ec-355">EventCallback</span></span>

<span data-ttu-id="194ec-356">Uno scenario comune con i componenti annidati è la volontà di eseguire il metodo di un componente padre quando si verifica un evento del componente figlio&mdash;ad esempio quando si verifica un evento `onclick` nell'elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-356">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="194ec-357">Per esporre gli eventi tra i componenti, usare un `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="194ec-357">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="194ec-358">Un componente padre può assegnare un metodo di callback a un `EventCallback`di un componente figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-358">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="194ec-359">Il `ChildComponent` nell'app di esempio illustra come viene configurato il gestore di `onclick` di un pulsante per ricevere un delegato `EventCallback` dal `ParentComponent`dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="194ec-359">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="194ec-360">Il `EventCallback` viene digitato con `MouseEventArgs`, appropriato per un evento di `onclick` da un dispositivo periferico:</span><span class="sxs-lookup"><span data-stu-id="194ec-360">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="194ec-361">Il `ParentComponent` imposta il `EventCallback<T>` del figlio sul relativo metodo di `ShowMessage`:</span><span class="sxs-lookup"><span data-stu-id="194ec-361">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="194ec-362">Quando il pulsante è selezionato nella `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="194ec-362">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="194ec-363">Viene chiamato il metodo di `ShowMessage` del `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="194ec-363">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="194ec-364">`messageText` viene aggiornato e visualizzato nella `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="194ec-364">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="194ec-365">Una chiamata a [StateHasChanged](xref:blazor/lifecycle#state-changes) non è obbligatoria nel metodo del callback (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="194ec-365">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="194ec-366">`StateHasChanged` viene chiamato automaticamente per eseguire nuovamente il rendering del `ParentComponent`, così come gli eventi figlio attivano il rendering dei componenti nei gestori eventi che vengono eseguiti all'interno dell'elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-366">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="194ec-367">`EventCallback` e `EventCallback<T>` consentono delegati asincroni.</span><span class="sxs-lookup"><span data-stu-id="194ec-367">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="194ec-368">`EventCallback<T>` è fortemente tipizzato e richiede un tipo di argomento specifico.</span><span class="sxs-lookup"><span data-stu-id="194ec-368">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="194ec-369">`EventCallback` è debolmente tipizzato e consente qualsiasi tipo di argomento.</span><span class="sxs-lookup"><span data-stu-id="194ec-369">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="194ec-370">Richiama un `EventCallback` o `EventCallback<T>` con `InvokeAsync` e attendi il <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="194ec-370">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="194ec-371">Utilizzare `EventCallback` e `EventCallback<T>` per la gestione degli eventi e i parametri del componente di associazione.</span><span class="sxs-lookup"><span data-stu-id="194ec-371">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="194ec-372">Preferisce la `EventCallback<T>` fortemente tipizzata rispetto a `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="194ec-372">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="194ec-373">`EventCallback<T>` fornisce un migliore feedback sugli errori agli utenti del componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-373">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="194ec-374">Analogamente ad altri gestori di eventi dell'interfaccia utente, la specifica del parametro dell'evento è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="194ec-374">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="194ec-375">Utilizzare `EventCallback` quando non viene passato alcun valore al callback.</span><span class="sxs-lookup"><span data-stu-id="194ec-375">Use `EventCallback` when there's no value passed to the callback.</span></span>

::: moniker range=">= aspnetcore-3.1"

### <a name="prevent-default-actions"></a><span data-ttu-id="194ec-376">Impedisci azioni predefinite</span><span class="sxs-lookup"><span data-stu-id="194ec-376">Prevent default actions</span></span>

<span data-ttu-id="194ec-377">Usare l' [@on{Event}:p](xref:mvc/views/razor#oneventpreventdefault) attributo della direttiva reventdefault per impedire l'azione predefinita per un evento.</span><span class="sxs-lookup"><span data-stu-id="194ec-377">Use the [@on{EVENT}:preventDefault](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="194ec-378">Quando si seleziona un tasto in un dispositivo di input e lo stato attivo dell'elemento si trova in una casella di testo, in un browser viene in genere visualizzato il carattere della chiave nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="194ec-378">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="194ec-379">Nell'esempio seguente viene impedito il comportamento predefinito specificando l'attributo della direttiva `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="194ec-379">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="194ec-380">Il contatore viene incrementato e la chiave **+** non viene acquisita nel valore dell'elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="194ec-380">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```cshtml
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

<span data-ttu-id="194ec-381">Specificare l'attributo `@on{EVENT}:preventDefault` senza un valore equivale a `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="194ec-381">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="194ec-382">Il valore dell'attributo può anche essere un'espressione.</span><span class="sxs-lookup"><span data-stu-id="194ec-382">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="194ec-383">Nell'esempio seguente `_shouldPreventDefault` è un `bool` campo impostato su `true` o `false`:</span><span class="sxs-lookup"><span data-stu-id="194ec-383">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```cshtml
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="194ec-384">Per impedire l'azione predefinita, non è necessario un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="194ec-384">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="194ec-385">Il gestore eventi e impedire scenari di azione predefiniti possono essere usati in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="194ec-385">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="194ec-386">Arresta propagazione eventi</span><span class="sxs-lookup"><span data-stu-id="194ec-386">Stop event propagation</span></span>

<span data-ttu-id="194ec-387">Usare l'attributo della direttiva [@on{Event}: stopPropagation](xref:mvc/views/razor#oneventstoppropagation) per arrestare la propagazione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="194ec-387">Use the [@on{EVENT}:stopPropagation](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="194ec-388">Nell'esempio seguente la selezione della casella di controllo impedisce la propagazione degli eventi click dal secondo `<div>` figlio al `<div>`padre:</span><span class="sxs-lookup"><span data-stu-id="194ec-388">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```cshtml
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```

::: moniker-end

## <a name="chained-bind"></a><span data-ttu-id="194ec-389">Binding concatenato</span><span class="sxs-lookup"><span data-stu-id="194ec-389">Chained bind</span></span>

<span data-ttu-id="194ec-390">Uno scenario comune consiste nel concatenare un parametro associato a dati a un elemento Page nell'output del componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-390">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="194ec-391">Questo scenario è denominato *Binding concatenato* perché si verificano simultaneamente più livelli di associazione.</span><span class="sxs-lookup"><span data-stu-id="194ec-391">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="194ec-392">Un binding concatenato non può essere implementato con `@bind` sintassi nell'elemento della pagina.</span><span class="sxs-lookup"><span data-stu-id="194ec-392">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="194ec-393">Il gestore eventi e il valore devono essere specificati separatamente.</span><span class="sxs-lookup"><span data-stu-id="194ec-393">The event handler and value must be specified separately.</span></span> <span data-ttu-id="194ec-394">Un componente padre, tuttavia, può utilizzare la sintassi `@bind` con il parametro del componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-394">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="194ec-395">Il componente `PasswordField` seguente (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="194ec-395">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="194ec-396">Imposta il valore di un elemento `<input>` su una proprietà `Password`.</span><span class="sxs-lookup"><span data-stu-id="194ec-396">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="194ec-397">Espone le modifiche della proprietà `Password` a un componente padre con un [EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="194ec-397">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="194ec-398">Il componente `PasswordField` viene usato in un altro componente:</span><span class="sxs-lookup"><span data-stu-id="194ec-398">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="194ec-399">Per eseguire controlli o intercettare gli errori sulla password nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="194ec-399">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="194ec-400">Creare un campo di supporto per `Password` (`password` nell'esempio di codice seguente).</span><span class="sxs-lookup"><span data-stu-id="194ec-400">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="194ec-401">Eseguire i controlli o gli errori di trap nel Setter del `Password`.</span><span class="sxs-lookup"><span data-stu-id="194ec-401">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="194ec-402">Nell'esempio seguente viene fornito un feedback immediato all'utente se viene utilizzato uno spazio nel valore della password:</span><span class="sxs-lookup"><span data-stu-id="194ec-402">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="194ec-403">Acquisisci riferimenti ai componenti</span><span class="sxs-lookup"><span data-stu-id="194ec-403">Capture references to components</span></span>

<span data-ttu-id="194ec-404">I riferimenti ai componenti forniscono un modo per fare riferimento a un'istanza del componente in modo da poter emettere comandi per tale istanza, ad esempio `Show` o `Reset`.</span><span class="sxs-lookup"><span data-stu-id="194ec-404">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="194ec-405">Per acquisire un riferimento a un componente:</span><span class="sxs-lookup"><span data-stu-id="194ec-405">To capture a component reference:</span></span>

* <span data-ttu-id="194ec-406">Aggiungere un attributo [@ref](xref:mvc/views/razor#ref) al componente figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-406">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="194ec-407">Definire un campo con lo stesso tipo del componente figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-407">Define a field with the same type as the child component.</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="194ec-408">Quando viene eseguito il rendering del componente, il campo `loginDialog` viene popolato con l'istanza `MyLoginDialog` componente figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-408">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="194ec-409">È quindi possibile richiamare i metodi .NET nell'istanza del componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-409">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="194ec-410">La variabile `loginDialog` viene popolata solo dopo il rendering del componente e l'output include l'elemento `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="194ec-410">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="194ec-411">Fino a quel momento, non c'è niente a cui fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="194ec-411">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="194ec-412">Per modificare i riferimenti ai componenti dopo che il componente ha terminato il rendering, usare i [Metodi OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="194ec-412">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="194ec-413">Mentre l'acquisizione di riferimenti ai componenti usa una sintassi simile per l' [acquisizione di riferimenti a elementi](xref:blazor/javascript-interop#capture-references-to-elements), non è una funzionalità di [interoperabilità di JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="194ec-413">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="194ec-414">I riferimenti ai componenti non vengono passati al codice JavaScript&mdash;vengono usati solo nel codice .NET.</span><span class="sxs-lookup"><span data-stu-id="194ec-414">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="194ec-415">**Non** usare i riferimenti ai componenti per mutare lo stato dei componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-415">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="194ec-416">Usare invece i normali parametri dichiarativi per passare i dati ai componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-416">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="194ec-417">L'utilizzo di normali parametri dichiarativi restituisce automaticamente i componenti figlio che eseguono il rendering alle ore corrette.</span><span class="sxs-lookup"><span data-stu-id="194ec-417">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="194ec-418">Richiama i metodi del componente esternamente per aggiornare lo stato</span><span class="sxs-lookup"><span data-stu-id="194ec-418">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="194ec-419"> usa un `SynchronizationContext` per applicare un singolo thread logico di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="194ec-419"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="194ec-420">I metodi del [ciclo](xref:blazor/lifecycle) di vita di un componente e tutti i callback di evento generati da Blazor vengono eseguiti in questa `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="194ec-420">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="194ec-421">Nel caso in cui un componente deve essere aggiornato in base a un evento esterno, ad esempio un timer o altre notifiche, usare il metodo `InvokeAsync`, che invierà al `SynchronizationContext`di Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-421">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="194ec-422">Si consideri ad esempio un *servizio Notifier* che può inviare notifiche a qualsiasi componente in ascolto dello stato aggiornato:</span><span class="sxs-lookup"><span data-stu-id="194ec-422">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="194ec-423">Utilizzo del `NotifierService` per aggiornare un componente:</span><span class="sxs-lookup"><span data-stu-id="194ec-423">Usage of the `NotifierService` to update a component:</span></span>

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="194ec-424">Nell'esempio precedente `NotifierService` richiama il metodo di `OnNotify` del componente al di fuori della `SynchronizationContext`di Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-424">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="194ec-425">`InvokeAsync` viene utilizzato per passare al contesto corretto e accodare un rendering.</span><span class="sxs-lookup"><span data-stu-id="194ec-425">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="194ec-426">Usare \@chiave per controllare la conservazione di elementi e componenti</span><span class="sxs-lookup"><span data-stu-id="194ec-426">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="194ec-427">Quando si esegue il rendering di un elenco di elementi o componenti e gli elementi o i componenti vengono modificati successivamente, l'algoritmo diff di Blazordeve decidere quali elementi o componenti precedenti possono essere conservati e come eseguire il mapping degli oggetti modello.</span><span class="sxs-lookup"><span data-stu-id="194ec-427">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="194ec-428">In genere, questo processo è automatico e può essere ignorato, ma in alcuni casi potrebbe essere necessario controllare il processo.</span><span class="sxs-lookup"><span data-stu-id="194ec-428">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="194ec-429">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="194ec-429">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="194ec-430">Il contenuto della raccolta di `People` può cambiare con le voci inserite, eliminate o riordinate.</span><span class="sxs-lookup"><span data-stu-id="194ec-430">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="194ec-431">Quando viene eseguito il rendering del componente, il componente `<DetailsEditor>` può variare in modo da ricevere valori `Details` parametri diversi.</span><span class="sxs-lookup"><span data-stu-id="194ec-431">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="194ec-432">Questo può causare un rirendering più complesso del previsto.</span><span class="sxs-lookup"><span data-stu-id="194ec-432">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="194ec-433">In alcuni casi, il rirendering può comportare differenze di comportamento visibili, ad esempio lo stato attivo degli elementi persi.</span><span class="sxs-lookup"><span data-stu-id="194ec-433">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="194ec-434">È possibile controllare il processo di mapping con l'attributo `@key` direttiva.</span><span class="sxs-lookup"><span data-stu-id="194ec-434">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="194ec-435">`@key` fa in modo che l'algoritmo diffi garantisca la conservazione di elementi o componenti in base al valore della chiave:</span><span class="sxs-lookup"><span data-stu-id="194ec-435">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="194ec-436">Quando la raccolta `People` viene modificata, l'algoritmo diffing mantiene l'associazione tra istanze `<DetailsEditor>` e `person` istanze:</span><span class="sxs-lookup"><span data-stu-id="194ec-436">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="194ec-437">Se un `Person` viene eliminato dall'elenco `People`, solo l'istanza corrispondente di `<DetailsEditor>` viene rimossa dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="194ec-437">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="194ec-438">Altre istanze rimangono invariate.</span><span class="sxs-lookup"><span data-stu-id="194ec-438">Other instances are left unchanged.</span></span>
* <span data-ttu-id="194ec-439">Se un `Person` viene inserito in una determinata posizione nell'elenco, viene inserita una nuova istanza di `<DetailsEditor>` in corrispondenza della posizione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="194ec-439">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="194ec-440">Altre istanze rimangono invariate.</span><span class="sxs-lookup"><span data-stu-id="194ec-440">Other instances are left unchanged.</span></span>
* <span data-ttu-id="194ec-441">Se `Person` le voci vengono riordinate, le istanze di `<DetailsEditor>` corrispondenti vengono mantenute e nuovamente ordinate nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="194ec-441">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="194ec-442">In alcuni scenari, l'utilizzo di `@key` riduce al minimo la complessità del rendering ed evita potenziali problemi con le parti con stato del DOM che cambiano, ad esempio la posizione di messa a fuoco.</span><span class="sxs-lookup"><span data-stu-id="194ec-442">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="194ec-443">Le chiavi sono locali per ogni elemento contenitore o componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-443">Keys are local to each container element or component.</span></span> <span data-ttu-id="194ec-444">Le chiavi non vengono confrontate globalmente nell'intero documento.</span><span class="sxs-lookup"><span data-stu-id="194ec-444">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="194ec-445">Quando usare \@chiave</span><span class="sxs-lookup"><span data-stu-id="194ec-445">When to use \@key</span></span>

<span data-ttu-id="194ec-446">In genere, è opportuno usare `@key` ogni volta che viene eseguito il rendering di un elenco (ad esempio, in un blocco di `@foreach`) e un valore appropriato per definire il `@key`.</span><span class="sxs-lookup"><span data-stu-id="194ec-446">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="194ec-447">È anche possibile usare `@key` per impedire Blazor di mantenere un sottoalbero di elementi o componenti quando un oggetto viene modificato:</span><span class="sxs-lookup"><span data-stu-id="194ec-447">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="194ec-448">Se `@currentPerson` viene modificata, la direttiva attribute `@key` impone Blazor di rimuovere l'intero `<div>` e i relativi discendenti e ricompilare il sottoalbero all'interno dell'interfaccia utente con nuovi elementi e componenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-448">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="194ec-449">Questo può essere utile se è necessario garantire che lo stato dell'interfaccia utente non venga mantenuto quando `@currentPerson` modifiche.</span><span class="sxs-lookup"><span data-stu-id="194ec-449">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="194ec-450">Quando non usare \@chiave</span><span class="sxs-lookup"><span data-stu-id="194ec-450">When not to use \@key</span></span>

<span data-ttu-id="194ec-451">Si verifica un costo in termini di prestazioni quando si verificano differenze con `@key`.</span><span class="sxs-lookup"><span data-stu-id="194ec-451">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="194ec-452">Il costo delle prestazioni non è elevato, ma specifica solo `@key` se il controllo delle regole di conservazione degli elementi o dei componenti avvantaggia l'app.</span><span class="sxs-lookup"><span data-stu-id="194ec-452">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="194ec-453">Anche se non viene usato `@key`, Blazor conserva il più possibile le istanze di elementi e componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-453">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="194ec-454">L'unico vantaggio dell'utilizzo di `@key` è il controllo sulla *modalità* di mapping delle istanze del modello alle istanze dei componenti conservati, anziché sull'algoritmo di diffing che seleziona il mapping.</span><span class="sxs-lookup"><span data-stu-id="194ec-454">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="194ec-455">Valori da usare per la chiave di \@</span><span class="sxs-lookup"><span data-stu-id="194ec-455">What values to use for \@key</span></span>

<span data-ttu-id="194ec-456">In genere, è opportuno fornire uno dei seguenti tipi di valore per `@key`:</span><span class="sxs-lookup"><span data-stu-id="194ec-456">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="194ec-457">Istanze di oggetti modello (ad esempio, un'istanza di `Person` come nell'esempio precedente).</span><span class="sxs-lookup"><span data-stu-id="194ec-457">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="194ec-458">In questo modo si garantisce la conservazione in base all'uguaglianza del riferimento all'oggetto.</span><span class="sxs-lookup"><span data-stu-id="194ec-458">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="194ec-459">Identificatori univoci (ad esempio, valori di chiave primaria di tipo `int`, `string`o `Guid`).</span><span class="sxs-lookup"><span data-stu-id="194ec-459">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="194ec-460">Assicurarsi che i valori usati per `@key` non siano in conflitto.</span><span class="sxs-lookup"><span data-stu-id="194ec-460">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="194ec-461">Se vengono rilevati valori di conflitto nello stesso elemento padre, Blazor genera un'eccezione perché non è in grado di eseguire il mapping deterministico di elementi o componenti precedenti a nuovi elementi o componenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-461">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="194ec-462">Utilizzare solo valori distinti, ad esempio istanze di oggetti o valori di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="194ec-462">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="194ec-463">Routing</span><span class="sxs-lookup"><span data-stu-id="194ec-463">Routing</span></span>

<span data-ttu-id="194ec-464">Il routing in Blazor viene effettuato fornendo un modello di route a ogni componente accessibile nell'app.</span><span class="sxs-lookup"><span data-stu-id="194ec-464">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="194ec-465">Quando viene compilato un file Razor con una direttiva `@page`, alla classe generata viene assegnato un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specificando il modello di route.</span><span class="sxs-lookup"><span data-stu-id="194ec-465">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="194ec-466">In fase di esecuzione, il router cerca le classi di componenti con una `RouteAttribute` ed esegue il rendering di qualsiasi componente con un modello di route corrispondente all'URL richiesto.</span><span class="sxs-lookup"><span data-stu-id="194ec-466">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="194ec-467">È possibile applicare più modelli di route a un componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-467">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="194ec-468">Il componente seguente risponde alle richieste di `/BlazorRoute` e `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="194ec-468">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="194ec-469">Parametri di route</span><span class="sxs-lookup"><span data-stu-id="194ec-469">Route parameters</span></span>

<span data-ttu-id="194ec-470">I componenti possono ricevere parametri di route dal modello di route fornito nella direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="194ec-470">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="194ec-471">Il router usa parametri di route per popolare i parametri del componente corrispondente.</span><span class="sxs-lookup"><span data-stu-id="194ec-471">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="194ec-472">*Componente parametro di route*:</span><span class="sxs-lookup"><span data-stu-id="194ec-472">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="194ec-473">I parametri facoltativi non sono supportati, quindi vengono applicate due direttive `@page` nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="194ec-473">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="194ec-474">Il primo consente la navigazione al componente senza un parametro.</span><span class="sxs-lookup"><span data-stu-id="194ec-474">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="194ec-475">La seconda direttiva `@page` accetta il parametro di route `{text}` e assegna il valore alla proprietà `Text`.</span><span class="sxs-lookup"><span data-stu-id="194ec-475">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="194ec-476">La sintassi dei parametri *catch-all* (`*`/`**`), che acquisisce il percorso tra più limiti di cartella, **non** è supportata nei*componenti Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="194ec-476">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="partial-class-support"></a><span data-ttu-id="194ec-477">Supporto di classi parziali</span><span class="sxs-lookup"><span data-stu-id="194ec-477">Partial class support</span></span>

<span data-ttu-id="194ec-478">I componenti Razor vengono generati come classi parziali.</span><span class="sxs-lookup"><span data-stu-id="194ec-478">Razor components are generated as partial classes.</span></span> <span data-ttu-id="194ec-479">I componenti Razor vengono creati usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="194ec-479">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="194ec-480">C#il codice viene definito in un blocco di [@code](xref:mvc/views/razor#code) con markup HTML e codice Razor in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="194ec-480">C# code is defined in an [@code](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="194ec-481">i modelli di Blazor definiscono i componenti Razor usando questo approccio.</span><span class="sxs-lookup"><span data-stu-id="194ec-481">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="194ec-482">C#il codice viene inserito in un file code-behind definito come classe parziale.</span><span class="sxs-lookup"><span data-stu-id="194ec-482">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="194ec-483">L'esempio seguente illustra il componente `Counter` predefinito con un blocco di `@code` in un'app generata da un modello di Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-483">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="194ec-484">Il markup HTML, il codice Razor C# e il codice si trovano nello stesso file:</span><span class="sxs-lookup"><span data-stu-id="194ec-484">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="194ec-485">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-485">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="194ec-486">Il componente `Counter` può essere creato anche usando un file code-behind con una classe parziale:</span><span class="sxs-lookup"><span data-stu-id="194ec-486">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="194ec-487">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-487">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="194ec-488">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="194ec-488">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

## <a name="specify-a-component-base-class"></a><span data-ttu-id="194ec-489">Specificare una classe di base del componente</span><span class="sxs-lookup"><span data-stu-id="194ec-489">Specify a component base class</span></span>

<span data-ttu-id="194ec-490">È possibile utilizzare la direttiva `@inherits` per specificare una classe di base per un componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-490">The `@inherits` directive can be used to specify a base class for a component.</span></span>

<span data-ttu-id="194ec-491">L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) Mostra come un componente può ereditare una classe di base, `BlazorRocksBase`, per fornire le proprietà e i metodi del componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-491">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="194ec-492">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="194ec-492">*Pages/BlazorRocks.razor*:</span></span>

```cshtml
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="194ec-493">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="194ec-493">*BlazorRocksBase.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

<span data-ttu-id="194ec-494">La classe base deve derivare da `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="194ec-494">The base class should derive from `ComponentBase`.</span></span>

::: moniker-end

## <a name="import-components"></a><span data-ttu-id="194ec-495">Importa componenti</span><span class="sxs-lookup"><span data-stu-id="194ec-495">Import components</span></span>

<span data-ttu-id="194ec-496">Lo spazio dei nomi di un componente creato con Razor si basa su (in ordine di priorità):</span><span class="sxs-lookup"><span data-stu-id="194ec-496">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="194ec-497">[@namespace](xref:mvc/views/razor#namespace) designazione nel markup del*file Razor (razor) (* `@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="194ec-497">[@namespace](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="194ec-498">`RootNamespace` del progetto nel file di progetto (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="194ec-498">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="194ec-499">Il nome del progetto, tratto dal nome file del file di progetto (con*estensione csproj*), e il percorso dalla radice del progetto al componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-499">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="194ec-500">Ad esempio, il Framework risolve *{Project root}/pages/index.Razor* (*BlazorSample. csproj*) nello spazio dei nomi `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="194ec-500">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="194ec-501">I componenti C# seguono le regole di associazione dei nomi.</span><span class="sxs-lookup"><span data-stu-id="194ec-501">Components follow C# name binding rules.</span></span> <span data-ttu-id="194ec-502">Per il componente `Index` in questo esempio, i componenti nell'ambito sono tutti componenti:</span><span class="sxs-lookup"><span data-stu-id="194ec-502">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="194ec-503">Nella stessa cartella, *pagine*.</span><span class="sxs-lookup"><span data-stu-id="194ec-503">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="194ec-504">Componenti nella radice del progetto che non specificano in modo esplicito uno spazio dei nomi diverso.</span><span class="sxs-lookup"><span data-stu-id="194ec-504">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="194ec-505">I componenti definiti in uno spazio dei nomi diverso vengono introdotti nell'ambito usando la direttiva [@using](xref:mvc/views/razor#using) di Razor.</span><span class="sxs-lookup"><span data-stu-id="194ec-505">Components defined in a different namespace are brought into scope using Razor's [@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="194ec-506">Se un altro componente, `NavMenu.razor`, esiste nella cartella *BlazorSample/Shared* , il componente può essere utilizzato in `Index.razor` con la seguente istruzione `@using`:</span><span class="sxs-lookup"><span data-stu-id="194ec-506">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="194ec-507">È anche possibile fare riferimento ai componenti usando i relativi nomi completi, che non richiedono la direttiva [@using](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="194ec-507">Components can also be referenced using their fully qualified names, which doesn't require the [@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="194ec-508">La qualificazione `global::` non è supportata.</span><span class="sxs-lookup"><span data-stu-id="194ec-508">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="194ec-509">L'importazione di componenti con istruzioni `using` con alias, ad esempio `@using Foo = Bar`, non è supportata.</span><span class="sxs-lookup"><span data-stu-id="194ec-509">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="194ec-510">I nomi parzialmente qualificati non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="194ec-510">Partially qualified names aren't supported.</span></span> <span data-ttu-id="194ec-511">Ad esempio, l'aggiunta di `@using BlazorSample` e il riferimento `NavMenu.razor` con `<Shared.NavMenu></Shared.NavMenu>` non è supportata.</span><span class="sxs-lookup"><span data-stu-id="194ec-511">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="194ec-512">Attributi dell'elemento HTML condizionale</span><span class="sxs-lookup"><span data-stu-id="194ec-512">Conditional HTML element attributes</span></span>

<span data-ttu-id="194ec-513">Gli attributi degli elementi HTML vengono sottoposti a rendering in modo condizionale in base al valore .NET.</span><span class="sxs-lookup"><span data-stu-id="194ec-513">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="194ec-514">Se il valore è `false` o `null`, l'attributo non viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="194ec-514">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="194ec-515">Se il valore è `true`, l'attributo viene sottoposto a rendering ridotto a icona.</span><span class="sxs-lookup"><span data-stu-id="194ec-515">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="194ec-516">Nell'esempio seguente `IsCompleted` determina se viene eseguito il rendering `checked` nel markup dell'elemento:</span><span class="sxs-lookup"><span data-stu-id="194ec-516">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="194ec-517">Se `IsCompleted` è `true`, viene eseguito il rendering della casella di controllo come segue:</span><span class="sxs-lookup"><span data-stu-id="194ec-517">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="194ec-518">Se `IsCompleted` è `false`, viene eseguito il rendering della casella di controllo come segue:</span><span class="sxs-lookup"><span data-stu-id="194ec-518">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="194ec-519">Per ulteriori informazioni, vedere <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="194ec-519">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="194ec-520">Alcuni attributi HTML, ad esempio [aria-premuti](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), non funzionano correttamente quando il tipo .NET è un `bool`.</span><span class="sxs-lookup"><span data-stu-id="194ec-520">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="194ec-521">In questi casi, usare un tipo di `string` anziché una `bool`.</span><span class="sxs-lookup"><span data-stu-id="194ec-521">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="194ec-522">HTML non elaborato</span><span class="sxs-lookup"><span data-stu-id="194ec-522">Raw HTML</span></span>

<span data-ttu-id="194ec-523">Le stringhe vengono in genere sottoposte a rendering usando i nodi di testo DOM, il che significa che qualsiasi markup che può contenere viene ignorato e considerato come testo letterale.</span><span class="sxs-lookup"><span data-stu-id="194ec-523">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="194ec-524">Per eseguire il rendering del codice HTML non elaborato, eseguire il wrapping del contenuto HTML in un valore `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="194ec-524">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="194ec-525">Il valore viene analizzato in formato HTML o SVG e inserito nel DOM.</span><span class="sxs-lookup"><span data-stu-id="194ec-525">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="194ec-526">Il rendering di codice HTML non elaborato costruito da un'origine non attendibile costituisce un rischio per la **sicurezza** e deve essere evitato.</span><span class="sxs-lookup"><span data-stu-id="194ec-526">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="194ec-527">Nell'esempio seguente viene illustrato l'utilizzo del tipo di `MarkupString` per aggiungere un blocco di contenuto HTML statico all'output sottoposto a rendering di un componente:</span><span class="sxs-lookup"><span data-stu-id="194ec-527">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="194ec-528">Componenti basati su modelli</span><span class="sxs-lookup"><span data-stu-id="194ec-528">Templated components</span></span>

<span data-ttu-id="194ec-529">I componenti basati su modelli sono componenti che accettano uno o più modelli di interfaccia utente come parametri, che possono quindi essere usati come parte della logica di rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-529">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="194ec-530">I componenti basati su modelli consentono di creare componenti di livello superiore più riutilizzabili rispetto ai componenti normali.</span><span class="sxs-lookup"><span data-stu-id="194ec-530">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="194ec-531">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="194ec-531">A couple of examples include:</span></span>

* <span data-ttu-id="194ec-532">Componente della tabella che consente a un utente di specificare i modelli per l'intestazione, le righe e il piè di pagina della tabella.</span><span class="sxs-lookup"><span data-stu-id="194ec-532">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="194ec-533">Componente di elenco che consente a un utente di specificare un modello per il rendering degli elementi in un elenco.</span><span class="sxs-lookup"><span data-stu-id="194ec-533">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="194ec-534">Parametri modello</span><span class="sxs-lookup"><span data-stu-id="194ec-534">Template parameters</span></span>

<span data-ttu-id="194ec-535">Un componente basato su modelli viene definito specificando uno o più parametri del componente di tipo `RenderFragment` o `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="194ec-535">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="194ec-536">Un frammento di rendering rappresenta un segmento di interfaccia utente di cui eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="194ec-536">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="194ec-537">`RenderFragment<T>` accetta un parametro di tipo che può essere specificato quando viene richiamato il frammento di rendering.</span><span class="sxs-lookup"><span data-stu-id="194ec-537">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="194ec-538">componente `TableTemplate`:</span><span class="sxs-lookup"><span data-stu-id="194ec-538">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="194ec-539">Quando si usa un componente basato su modelli, è possibile specificare i parametri del modello usando gli elementi figlio che corrispondono ai nomi dei parametri (`TableHeader` e `RowTemplate` nell'esempio seguente):</span><span class="sxs-lookup"><span data-stu-id="194ec-539">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="194ec-540">Parametri di contesto del modello</span><span class="sxs-lookup"><span data-stu-id="194ec-540">Template context parameters</span></span>

<span data-ttu-id="194ec-541">Gli argomenti del componente di tipo `RenderFragment<T>` passati come elementi hanno un parametro implicito denominato `context`, ad esempio dall'esempio di codice precedente, `@context.PetId`, ma è possibile modificare il nome del parametro usando l'attributo `Context` nell'elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="194ec-541">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="194ec-542">Nell'esempio seguente, l'attributo `Context` dell'elemento `RowTemplate` specifica il parametro `pet`:</span><span class="sxs-lookup"><span data-stu-id="194ec-542">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="194ec-543">In alternativa, è possibile specificare l'attributo `Context` sull'elemento Component.</span><span class="sxs-lookup"><span data-stu-id="194ec-543">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="194ec-544">L'attributo `Context` specificato si applica a tutti i parametri di modello specificati.</span><span class="sxs-lookup"><span data-stu-id="194ec-544">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="194ec-545">Questa operazione può essere utile quando si desidera specificare il nome del parametro di contenuto per il contenuto figlio implicito (senza alcun elemento figlio di wrapping).</span><span class="sxs-lookup"><span data-stu-id="194ec-545">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="194ec-546">Nell'esempio seguente l'attributo `Context` viene visualizzato nell'elemento `TableTemplate` e si applica a tutti i parametri del modello:</span><span class="sxs-lookup"><span data-stu-id="194ec-546">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="194ec-547">Componenti tipizzati in modo generico</span><span class="sxs-lookup"><span data-stu-id="194ec-547">Generic-typed components</span></span>

<span data-ttu-id="194ec-548">I componenti basati su modelli spesso sono tipizzati in modo generico.</span><span class="sxs-lookup"><span data-stu-id="194ec-548">Templated components are often generically typed.</span></span> <span data-ttu-id="194ec-549">È ad esempio possibile utilizzare un componente `ListViewTemplate` generico per eseguire il rendering dei valori `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="194ec-549">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="194ec-550">Per definire un componente generico, usare la direttiva [@typeparam](xref:mvc/views/razor#typeparam) per specificare i parametri di tipo:</span><span class="sxs-lookup"><span data-stu-id="194ec-550">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="194ec-551">Quando si usano componenti tipizzati generici, il parametro di tipo viene dedotto, se possibile:</span><span class="sxs-lookup"><span data-stu-id="194ec-551">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="194ec-552">In caso contrario, il parametro di tipo deve essere specificato in modo esplicito utilizzando un attributo che corrisponde al nome del parametro di tipo.</span><span class="sxs-lookup"><span data-stu-id="194ec-552">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="194ec-553">Nell'esempio seguente `TItem="Pet"` specifica il tipo:</span><span class="sxs-lookup"><span data-stu-id="194ec-553">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="194ec-554">Parametri e valori di propagazione</span><span class="sxs-lookup"><span data-stu-id="194ec-554">Cascading values and parameters</span></span>

<span data-ttu-id="194ec-555">In alcuni scenari, non è pratico eseguire il flusso dei dati da un componente predecessore a un componente discendente usando i [parametri del componente](#component-parameters), soprattutto quando sono presenti diversi livelli di componenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-555">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="194ec-556">I valori e i parametri di propagazione consentono di risolvere questo problema fornendo un modo pratico per un componente predecessore per fornire un valore a tutti i relativi componenti discendenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-556">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="194ec-557">I parametri e i valori di propagazione forniscono anche un approccio per la coordinazione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-557">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="194ec-558">Esempio di tema</span><span class="sxs-lookup"><span data-stu-id="194ec-558">Theme example</span></span>

<span data-ttu-id="194ec-559">Nell'esempio seguente dall'app di esempio, la classe `ThemeInfo` specifica le informazioni sul tema per scorrere la gerarchia dei componenti in modo che tutti i pulsanti all'interno di una determinata parte dell'app condividono lo stesso stile.</span><span class="sxs-lookup"><span data-stu-id="194ec-559">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="194ec-560">*UIThemeClasses/themeinfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="194ec-560">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="194ec-561">Un componente predecessore può fornire un valore di propagazione utilizzando il componente valore di propagazione.</span><span class="sxs-lookup"><span data-stu-id="194ec-561">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="194ec-562">Il componente `CascadingValue` esegue il wrapping di un sottoalbero della gerarchia dei componenti e fornisce un singolo valore a tutti i componenti all'interno di tale sottoalbero.</span><span class="sxs-lookup"><span data-stu-id="194ec-562">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="194ec-563">Ad esempio, l'app di esempio specifica le informazioni sul tema (`ThemeInfo`) in uno dei layout dell'app come parametro di propagazione per tutti i componenti che costituiscono il corpo del layout della proprietà `@Body`.</span><span class="sxs-lookup"><span data-stu-id="194ec-563">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="194ec-564">al `ButtonClass` viene assegnato un valore di `btn-success` nel componente layout.</span><span class="sxs-lookup"><span data-stu-id="194ec-564">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="194ec-565">Qualsiasi componente discendente può utilizzare questa proprietà tramite l'`ThemeInfo` oggetto a cascata.</span><span class="sxs-lookup"><span data-stu-id="194ec-565">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="194ec-566">componente `CascadingValuesParametersLayout`:</span><span class="sxs-lookup"><span data-stu-id="194ec-566">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="194ec-567">Per utilizzare i valori di propagazione, i componenti dichiarano i parametri di propagazione utilizzando l'attributo `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="194ec-567">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="194ec-568">I valori a cascata vengono associati ai parametri di propagazione per tipo.</span><span class="sxs-lookup"><span data-stu-id="194ec-568">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="194ec-569">Nell'app di esempio il componente `CascadingValuesParametersTheme` associa il `ThemeInfo` valore di propagazione a un parametro di propagazione.</span><span class="sxs-lookup"><span data-stu-id="194ec-569">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="194ec-570">Il parametro viene usato per impostare la classe CSS per uno dei pulsanti visualizzati dal componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-570">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="194ec-571">componente `CascadingValuesParametersTheme`:</span><span class="sxs-lookup"><span data-stu-id="194ec-571">`CascadingValuesParametersTheme` component:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="194ec-572">Per eseguire il propagazione di più valori dello stesso tipo all'interno dello stesso sottoalbero, specificare una stringa di `Name` univoca per ogni componente `CascadingValue` e la `CascadingParameter`corrispondente.</span><span class="sxs-lookup"><span data-stu-id="194ec-572">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="194ec-573">Nell'esempio seguente due componenti `CascadingValue` propagano istanze diverse di `MyCascadingType` in base al nome:</span><span class="sxs-lookup"><span data-stu-id="194ec-573">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="194ec-574">In un componente discendente i parametri a cascata ricevono i rispettivi valori dai valori a cascata corrispondenti nel componente predecessore per nome:</span><span class="sxs-lookup"><span data-stu-id="194ec-574">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="194ec-575">Esempio di tabulazione</span><span class="sxs-lookup"><span data-stu-id="194ec-575">TabSet example</span></span>

<span data-ttu-id="194ec-576">I parametri di propagazione consentono inoltre ai componenti di collaborare attraverso la gerarchia dei componenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-576">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="194ec-577">Si consideri, ad esempio, l'esempio di *tabulazione* seguente nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="194ec-577">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="194ec-578">L'app di esempio dispone di un'interfaccia `ITab` che le schede implementano:</span><span class="sxs-lookup"><span data-stu-id="194ec-578">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="194ec-579">Il componente `CascadingValuesParametersTabSet` usa il componente `TabSet`, che contiene diversi componenti di `Tab`:</span><span class="sxs-lookup"><span data-stu-id="194ec-579">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="194ec-580">I componenti di `Tab` figlio non vengono passati in modo esplicito come parametri al `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="194ec-580">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="194ec-581">Al contrario, i componenti `Tab` figlio fanno parte del contenuto figlio della `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="194ec-581">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="194ec-582">Tuttavia, il `TabSet` deve comunque conoscere ogni componente `Tab` in modo che sia in grado di eseguire il rendering delle intestazioni e della scheda attiva. Per abilitare questo coordinamento senza richiedere codice aggiuntivo, il componente `TabSet` *può fornire se stesso come valore* di propagazione che viene quindi prelevato dai componenti `Tab` discendenti.</span><span class="sxs-lookup"><span data-stu-id="194ec-582">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="194ec-583">componente `TabSet`:</span><span class="sxs-lookup"><span data-stu-id="194ec-583">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="194ec-584">I componenti `Tab` discendenti acquisiscono l'`TabSet` contenitore come parametro di propagazione, quindi i componenti `Tab` si aggiungono al `TabSet` e coordinano la scheda attiva.</span><span class="sxs-lookup"><span data-stu-id="194ec-584">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="194ec-585">componente `Tab`:</span><span class="sxs-lookup"><span data-stu-id="194ec-585">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="194ec-586">Modelli Razor</span><span class="sxs-lookup"><span data-stu-id="194ec-586">Razor templates</span></span>

<span data-ttu-id="194ec-587">I frammenti di rendering possono essere definiti usando la sintassi del modello Razor.</span><span class="sxs-lookup"><span data-stu-id="194ec-587">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="194ec-588">I modelli Razor sono un modo per definire un frammento di interfaccia utente e presupporre il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="194ec-588">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="194ec-589">Nell'esempio seguente viene illustrato come specificare `RenderFragment` e `RenderFragment<T>` valori ed eseguire il rendering dei modelli direttamente in un componente.</span><span class="sxs-lookup"><span data-stu-id="194ec-589">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="194ec-590">I frammenti di rendering possono anche essere passati come argomenti ai [componenti basati su modelli](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="194ec-590">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="194ec-591">Output del codice precedente sottoposto a rendering:</span><span class="sxs-lookup"><span data-stu-id="194ec-591">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="194ec-592">Logica RenderTreeBuilder manuale</span><span class="sxs-lookup"><span data-stu-id="194ec-592">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="194ec-593">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` fornisce metodi per la modifica di componenti ed elementi, inclusa la compilazione manuale C# dei componenti nel codice.</span><span class="sxs-lookup"><span data-stu-id="194ec-593">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="194ec-594">L'uso di `RenderTreeBuilder` per creare componenti è uno scenario avanzato.</span><span class="sxs-lookup"><span data-stu-id="194ec-594">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="194ec-595">Un componente con formato non valido, ad esempio un tag di markup non chiuso, può causare un comportamento indefinito.</span><span class="sxs-lookup"><span data-stu-id="194ec-595">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="194ec-596">Si consideri il componente `PetDetails` seguente, che può essere incorporato manualmente in un altro componente:</span><span class="sxs-lookup"><span data-stu-id="194ec-596">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="194ec-597">Nell'esempio seguente il ciclo nel metodo `CreateComponent` genera tre componenti `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="194ec-597">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="194ec-598">Quando si chiamano `RenderTreeBuilder` metodi per creare i componenti (`OpenComponent` e `AddAttribute`), i numeri di sequenza sono numeri di riga del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="194ec-598">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="194ec-599">L'algoritmo Blazor Difference si basa sui numeri di sequenza corrispondenti a righe di codice distinte, non a chiamate di chiamata distinte.</span><span class="sxs-lookup"><span data-stu-id="194ec-599">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="194ec-600">Quando si crea un componente con `RenderTreeBuilder` metodi, impostare come hardcoded gli argomenti per i numeri di sequenza.</span><span class="sxs-lookup"><span data-stu-id="194ec-600">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="194ec-601">**L'utilizzo di un calcolo o di un contatore per generare il numero di sequenza può causare un calo delle prestazioni.**</span><span class="sxs-lookup"><span data-stu-id="194ec-601">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="194ec-602">Per ulteriori informazioni, vedere la sezione [numeri di sequenza correlati ai numeri di riga del codice e non all'ordine di esecuzione](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="194ec-602">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="194ec-603">componente `BuiltContent`:</span><span class="sxs-lookup"><span data-stu-id="194ec-603">`BuiltContent` component:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> <span data-ttu-id="194ec-604">! AVVISO I tipi in `Microsoft.AspNetCore.Components.RenderTree` consentono l'elaborazione dei *risultati* delle operazioni di rendering.</span><span class="sxs-lookup"><span data-stu-id="194ec-604">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="194ec-605">Questi sono i dettagli interni dell'implementazione di Blazor Framework.</span><span class="sxs-lookup"><span data-stu-id="194ec-605">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="194ec-606">Questi tipi devono essere considerati *instabili* e soggetti a modifiche nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="194ec-606">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="194ec-607">I numeri di sequenza sono correlati ai numeri di riga del codice e non all'ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="194ec-607">Sequence numbers relate to code line numbers and not execution order</span></span>

Blazor<span data-ttu-id="194ec-608"> file di `.razor` vengono sempre compilati.</span><span class="sxs-lookup"><span data-stu-id="194ec-608"> `.razor` files are always compiled.</span></span> <span data-ttu-id="194ec-609">Questo è potenzialmente un ottimo vantaggio per `.razor` perché il passaggio di compilazione può essere usato per inserire informazioni che migliorano le prestazioni dell'app in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="194ec-609">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="194ec-610">Un esempio fondamentale di questi miglioramenti riguarda i *numeri di sequenza*.</span><span class="sxs-lookup"><span data-stu-id="194ec-610">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="194ec-611">I numeri di sequenza indicano al runtime quali output provengono da righe di codice distinte e ordinate.</span><span class="sxs-lookup"><span data-stu-id="194ec-611">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="194ec-612">Il runtime usa queste informazioni per generare differenze di albero efficienti nel tempo lineare, che è molto più veloce rispetto a quanto normalmente è possibile per un algoritmo diff della struttura ad albero generale.</span><span class="sxs-lookup"><span data-stu-id="194ec-612">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="194ec-613">Si consideri il seguente file di `.razor` semplice:</span><span class="sxs-lookup"><span data-stu-id="194ec-613">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="194ec-614">Il codice precedente viene compilato in un modo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="194ec-614">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="194ec-615">Quando il codice viene eseguito per la prima volta, se `someFlag` è `true`, il generatore riceve:</span><span class="sxs-lookup"><span data-stu-id="194ec-615">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="194ec-616">Sequence</span><span class="sxs-lookup"><span data-stu-id="194ec-616">Sequence</span></span> | <span data-ttu-id="194ec-617">Tipo di</span><span class="sxs-lookup"><span data-stu-id="194ec-617">Type</span></span>      | <span data-ttu-id="194ec-618">Data</span><span class="sxs-lookup"><span data-stu-id="194ec-618">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="194ec-619">0</span><span class="sxs-lookup"><span data-stu-id="194ec-619">0</span></span>        | <span data-ttu-id="194ec-620">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="194ec-620">Text node</span></span> | <span data-ttu-id="194ec-621">First</span><span class="sxs-lookup"><span data-stu-id="194ec-621">First</span></span>  |
| <span data-ttu-id="194ec-622">1</span><span class="sxs-lookup"><span data-stu-id="194ec-622">1</span></span>        | <span data-ttu-id="194ec-623">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="194ec-623">Text node</span></span> | <span data-ttu-id="194ec-624">Second</span><span class="sxs-lookup"><span data-stu-id="194ec-624">Second</span></span> |

<span data-ttu-id="194ec-625">Si supponga che `someFlag` diventi `false`e che venga nuovamente eseguito il rendering del markup.</span><span class="sxs-lookup"><span data-stu-id="194ec-625">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="194ec-626">Questa volta, il generatore riceve:</span><span class="sxs-lookup"><span data-stu-id="194ec-626">This time, the builder receives:</span></span>

| <span data-ttu-id="194ec-627">Sequence</span><span class="sxs-lookup"><span data-stu-id="194ec-627">Sequence</span></span> | <span data-ttu-id="194ec-628">Tipo di</span><span class="sxs-lookup"><span data-stu-id="194ec-628">Type</span></span>       | <span data-ttu-id="194ec-629">Data</span><span class="sxs-lookup"><span data-stu-id="194ec-629">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="194ec-630">1</span><span class="sxs-lookup"><span data-stu-id="194ec-630">1</span></span>        | <span data-ttu-id="194ec-631">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="194ec-631">Text node</span></span>  | <span data-ttu-id="194ec-632">Second</span><span class="sxs-lookup"><span data-stu-id="194ec-632">Second</span></span> |

<span data-ttu-id="194ec-633">Quando il runtime esegue una diff, rileva che l'elemento in sequenza `0` è stato rimosso, quindi genera lo *script di modifica*semplice seguente:</span><span class="sxs-lookup"><span data-stu-id="194ec-633">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="194ec-634">Rimuovere il primo nodo di testo.</span><span class="sxs-lookup"><span data-stu-id="194ec-634">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="194ec-635">Cosa accade se si generano numeri di sequenza a livello di codice</span><span class="sxs-lookup"><span data-stu-id="194ec-635">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="194ec-636">Si supponga invece che sia stata scritta la logica del generatore di albero di rendering seguente:</span><span class="sxs-lookup"><span data-stu-id="194ec-636">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="194ec-637">A questo punto, il primo output è:</span><span class="sxs-lookup"><span data-stu-id="194ec-637">Now, the first output is:</span></span>

| <span data-ttu-id="194ec-638">Sequence</span><span class="sxs-lookup"><span data-stu-id="194ec-638">Sequence</span></span> | <span data-ttu-id="194ec-639">Tipo di</span><span class="sxs-lookup"><span data-stu-id="194ec-639">Type</span></span>      | <span data-ttu-id="194ec-640">Data</span><span class="sxs-lookup"><span data-stu-id="194ec-640">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="194ec-641">0</span><span class="sxs-lookup"><span data-stu-id="194ec-641">0</span></span>        | <span data-ttu-id="194ec-642">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="194ec-642">Text node</span></span> | <span data-ttu-id="194ec-643">First</span><span class="sxs-lookup"><span data-stu-id="194ec-643">First</span></span>  |
| <span data-ttu-id="194ec-644">1</span><span class="sxs-lookup"><span data-stu-id="194ec-644">1</span></span>        | <span data-ttu-id="194ec-645">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="194ec-645">Text node</span></span> | <span data-ttu-id="194ec-646">Second</span><span class="sxs-lookup"><span data-stu-id="194ec-646">Second</span></span> |

<span data-ttu-id="194ec-647">Questo risultato è identico al caso precedente, pertanto non esistono problemi negativi.</span><span class="sxs-lookup"><span data-stu-id="194ec-647">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="194ec-648">`someFlag` è `false` sul secondo rendering e l'output è:</span><span class="sxs-lookup"><span data-stu-id="194ec-648">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="194ec-649">Sequence</span><span class="sxs-lookup"><span data-stu-id="194ec-649">Sequence</span></span> | <span data-ttu-id="194ec-650">Tipo di</span><span class="sxs-lookup"><span data-stu-id="194ec-650">Type</span></span>      | <span data-ttu-id="194ec-651">Data</span><span class="sxs-lookup"><span data-stu-id="194ec-651">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="194ec-652">0</span><span class="sxs-lookup"><span data-stu-id="194ec-652">0</span></span>        | <span data-ttu-id="194ec-653">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="194ec-653">Text node</span></span> | <span data-ttu-id="194ec-654">Second</span><span class="sxs-lookup"><span data-stu-id="194ec-654">Second</span></span> |

<span data-ttu-id="194ec-655">Questa volta, l'algoritmo Diff rileva che si sono verificate *due* modifiche e l'algoritmo genera lo script di modifica seguente:</span><span class="sxs-lookup"><span data-stu-id="194ec-655">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="194ec-656">Modificare il valore del primo nodo di testo in `Second`.</span><span class="sxs-lookup"><span data-stu-id="194ec-656">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="194ec-657">Rimuovere il secondo nodo di testo.</span><span class="sxs-lookup"><span data-stu-id="194ec-657">Remove the second text node.</span></span>

<span data-ttu-id="194ec-658">La generazione dei numeri di sequenza ha perso tutte le informazioni utili sul punto in cui i `if/else` rami e i cicli erano presenti nel codice originale.</span><span class="sxs-lookup"><span data-stu-id="194ec-658">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="194ec-659">In questo modo si ottiene una differenza **due volte più a lungo** .</span><span class="sxs-lookup"><span data-stu-id="194ec-659">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="194ec-660">Questo è un esempio semplice.</span><span class="sxs-lookup"><span data-stu-id="194ec-660">This is a trivial example.</span></span> <span data-ttu-id="194ec-661">Nei casi più realistici con strutture complesse e profondamente annidate e soprattutto con i cicli, il costo delle prestazioni è più grave.</span><span class="sxs-lookup"><span data-stu-id="194ec-661">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="194ec-662">Invece di identificare immediatamente i blocchi o i rami del ciclo che sono stati inseriti o rimossi, l'algoritmo Diff deve ripresentarsi in modo approfondito negli alberi di rendering e, in genere, compilare script di modifica molto più lunghi perché non è stato informato in modo indesiderato sul modo in cui le nuove strutture relazione tra loro.</span><span class="sxs-lookup"><span data-stu-id="194ec-662">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="194ec-663">Linee guida e conclusioni</span><span class="sxs-lookup"><span data-stu-id="194ec-663">Guidance and conclusions</span></span>

* <span data-ttu-id="194ec-664">Le prestazioni dell'app soffrono se i numeri di sequenza vengono generati dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="194ec-664">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="194ec-665">Il Framework non è in grado di creare automaticamente i propri numeri di sequenza in fase di esecuzione perché le informazioni necessarie non esistono a meno che non vengano acquisite in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="194ec-665">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="194ec-666">Non scrivere blocchi lunghi di logica di `RenderTreeBuilder` implementata manualmente.</span><span class="sxs-lookup"><span data-stu-id="194ec-666">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="194ec-667">Preferire `.razor` file e consentire al compilatore di gestire i numeri di sequenza.</span><span class="sxs-lookup"><span data-stu-id="194ec-667">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="194ec-668">Se non si è in grado di evitare la logica manuale `RenderTreeBuilder`, suddividere i blocchi di codice lunghi in parti più piccole racchiuse in `OpenRegion`/chiamate `CloseRegion`.</span><span class="sxs-lookup"><span data-stu-id="194ec-668">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="194ec-669">Ogni area ha il proprio spazio separato dei numeri di sequenza, quindi è possibile riavviare da zero (o qualsiasi altro numero arbitrario) all'interno di ogni area.</span><span class="sxs-lookup"><span data-stu-id="194ec-669">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="194ec-670">Se i numeri di sequenza sono hardcoded, l'algoritmo Diff richiede solo che i numeri di sequenza aumentino nel valore.</span><span class="sxs-lookup"><span data-stu-id="194ec-670">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="194ec-671">Il valore iniziale e i gap sono irrilevanti.</span><span class="sxs-lookup"><span data-stu-id="194ec-671">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="194ec-672">Una delle opzioni legittime consiste nell'usare il numero di riga del codice come numero di sequenza oppure iniziare da zero e aumentare di uno o di centinaia (o qualsiasi intervallo preferito).</span><span class="sxs-lookup"><span data-stu-id="194ec-672">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="194ec-673"> usa i numeri di sequenza, mentre altri Framework dell'interfaccia utente per le differenze tra gli alberi non li usano.</span><span class="sxs-lookup"><span data-stu-id="194ec-673"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="194ec-674">La diffing è molto più veloce quando si usano i numeri di sequenza e Blazor ha il vantaggio di un passaggio di compilazione che gestisce automaticamente i numeri di sequenza per gli sviluppatori che creano file con *estensione Razor* .</span><span class="sxs-lookup"><span data-stu-id="194ec-674">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="194ec-675">Localizzazione</span><span class="sxs-lookup"><span data-stu-id="194ec-675">Localization</span></span>

<span data-ttu-id="194ec-676">le app di Blazor server sono localizzate usando il [middleware di localizzazione](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="194ec-676">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="194ec-677">Il middleware seleziona le impostazioni cultura appropriate per gli utenti che richiedono risorse dall'app.</span><span class="sxs-lookup"><span data-stu-id="194ec-677">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="194ec-678">Le impostazioni cultura possono essere impostate utilizzando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="194ec-678">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="194ec-679">Cookie</span><span class="sxs-lookup"><span data-stu-id="194ec-679">Cookies</span></span>](#cookies)
* [<span data-ttu-id="194ec-680">Fornire l'interfaccia utente per scegliere le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="194ec-680">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="194ec-681">Per altre informazioni ed esempi, vedere <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="194ec-681">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="194ec-682">Configurare il linker per l'internazionalizzazione (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="194ec-682">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="194ec-683">Per impostazione predefinita, la configurazione del linker Blazorper le app Blazor webassembly rimuove le informazioni di internazionalizzazione ad eccezione delle impostazioni locali richieste in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="194ec-683">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="194ec-684">Per ulteriori informazioni e istruzioni sul controllo del comportamento del linker, vedere <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="194ec-684">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="194ec-685">Cookie</span><span class="sxs-lookup"><span data-stu-id="194ec-685">Cookies</span></span>

<span data-ttu-id="194ec-686">Un cookie di impostazioni cultura di localizzazione può salvare in maniera permanente le impostazioni cultura dell'utente.</span><span class="sxs-lookup"><span data-stu-id="194ec-686">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="194ec-687">Il cookie viene creato tramite il metodo `OnGet` della pagina host dell'app (*pages/host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="194ec-687">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="194ec-688">Il middleware di localizzazione legge il cookie nelle richieste successive per impostare le impostazioni cultura dell'utente.</span><span class="sxs-lookup"><span data-stu-id="194ec-688">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="194ec-689">L'uso di un cookie garantisce che la connessione WebSocket possa propagare correttamente le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="194ec-689">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="194ec-690">Se gli schemi di localizzazione sono basati sul percorso dell'URL o sulla stringa di query, lo schema potrebbe non essere in grado di funzionare con WebSocket, quindi non è possibile salvare in modo permanente le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="194ec-690">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="194ec-691">Pertanto, l'utilizzo di un cookie di impostazioni cultura di localizzazione è l'approccio consigliato.</span><span class="sxs-lookup"><span data-stu-id="194ec-691">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="194ec-692">Qualsiasi tecnica può essere utilizzata per assegnare impostazioni cultura se le impostazioni cultura vengono rese permanente in un cookie di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="194ec-692">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="194ec-693">Se l'app dispone già di uno schema di localizzazione stabilito per ASP.NET Core lato server, continuare a usare l'infrastruttura di localizzazione esistente dell'app e impostare il cookie delle impostazioni cultura di localizzazione nello schema dell'app.</span><span class="sxs-lookup"><span data-stu-id="194ec-693">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="194ec-694">Nell'esempio seguente viene illustrato come impostare le impostazioni cultura correnti in un cookie che può essere letto dal middleware di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="194ec-694">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="194ec-695">Creare un file *pages/host. cshtml. cs* con il contenuto seguente nell'app Blazor server:</span><span class="sxs-lookup"><span data-stu-id="194ec-695">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="194ec-696">La localizzazione viene gestita nell'app:</span><span class="sxs-lookup"><span data-stu-id="194ec-696">Localization is handled in the app:</span></span>

1. <span data-ttu-id="194ec-697">Il browser invia una richiesta HTTP iniziale all'app.</span><span class="sxs-lookup"><span data-stu-id="194ec-697">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="194ec-698">Le impostazioni cultura vengono assegnate dal middleware di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="194ec-698">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="194ec-699">Il metodo `OnGet` in *_Host. cshtml. cs* rende permanente le impostazioni cultura di un cookie come parte della risposta.</span><span class="sxs-lookup"><span data-stu-id="194ec-699">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="194ec-700">Il browser apre una connessione WebSocket per creare una sessione interattiva di Blazor server.</span><span class="sxs-lookup"><span data-stu-id="194ec-700">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="194ec-701">Il middleware di localizzazione legge il cookie e assegna le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="194ec-701">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="194ec-702">La sessione del server Blazor inizia con le impostazioni cultura corrette.</span><span class="sxs-lookup"><span data-stu-id="194ec-702">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="194ec-703">Fornire l'interfaccia utente per scegliere le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="194ec-703">Provide UI to choose the culture</span></span>

<span data-ttu-id="194ec-704">Per consentire a un utente di selezionare le impostazioni cultura, è consigliabile un *approccio basato su Reindirizzamento* .</span><span class="sxs-lookup"><span data-stu-id="194ec-704">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="194ec-705">Il processo è simile a quello che accade in un'app Web quando un utente tenta di accedere a una risorsa protetta&mdash;l'utente viene reindirizzato a una pagina di accesso e quindi reindirizzato di nuovo alla risorsa originale.</span><span class="sxs-lookup"><span data-stu-id="194ec-705">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="194ec-706">L'app rende permanente le impostazioni cultura selezionate dall'utente tramite un reindirizzamento a un controller.</span><span class="sxs-lookup"><span data-stu-id="194ec-706">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="194ec-707">Il controller imposta le impostazioni cultura selezionate dall'utente in un cookie e reindirizza di nuovo l'utente all'URI originale.</span><span class="sxs-lookup"><span data-stu-id="194ec-707">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="194ec-708">Stabilire un endpoint HTTP nel server per impostare le impostazioni cultura selezionate dall'utente in un cookie ed eseguire di nuovo il reindirizzamento all'URI originale:</span><span class="sxs-lookup"><span data-stu-id="194ec-708">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="194ec-709">Usare il risultato dell'azione `LocalRedirect` per impedire gli attacchi di reindirizzamento aperti.</span><span class="sxs-lookup"><span data-stu-id="194ec-709">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="194ec-710">Per ulteriori informazioni, vedere <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="194ec-710">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="194ec-711">Il componente seguente mostra un esempio di come eseguire il reindirizzamento iniziale quando l'utente seleziona le impostazioni cultura:</span><span class="sxs-lookup"><span data-stu-id="194ec-711">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="194ec-712">Usare scenari di localizzazione .NET in app Blazor</span><span class="sxs-lookup"><span data-stu-id="194ec-712">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="194ec-713">All'interno Blazor app sono disponibili gli scenari di globalizzazione e localizzazione .NET seguenti:</span><span class="sxs-lookup"><span data-stu-id="194ec-713">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="194ec-714">. Sistema di risorse di NET</span><span class="sxs-lookup"><span data-stu-id="194ec-714">.NET's resources system</span></span>
* <span data-ttu-id="194ec-715">Formattazione di numeri e date specifiche delle impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="194ec-715">Culture-specific number and date formatting</span></span>

<span data-ttu-id="194ec-716">la funzionalità `@bind` di Blazoresegue la globalizzazione in base alle impostazioni cultura correnti dell'utente.</span><span class="sxs-lookup"><span data-stu-id="194ec-716">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="194ec-717">Per ulteriori informazioni, vedere la sezione [Data Binding](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="194ec-717">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="194ec-718">Sono attualmente supportati un set limitato di scenari di localizzazione di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="194ec-718">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="194ec-719">`IStringLocalizer<>` *è supportata* nelle app Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-719">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="194ec-720">la localizzazione `IHtmlLocalizer<>`, `IViewLocalizer<>`e le annotazioni dei dati sono ASP.NET Core scenari MVC e **non sono supportate** nelle app Blazor.</span><span class="sxs-lookup"><span data-stu-id="194ec-720">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="194ec-721">Per ulteriori informazioni, vedere <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="194ec-721">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="194ec-722">Immagini SVG (Scalable Vector Graphics)</span><span class="sxs-lookup"><span data-stu-id="194ec-722">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="194ec-723">Poiché Blazor esegue il rendering del codice HTML, le immagini supportate dal browser, incluse le immagini SVG (Scalable Vector Graphics *) (SVG),* sono supportate tramite il tag `<img>`:</span><span class="sxs-lookup"><span data-stu-id="194ec-723">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="194ec-724">Analogamente, le immagini SVG sono supportate nelle regole CSS di un file di foglio di stile (*CSS*):</span><span class="sxs-lookup"><span data-stu-id="194ec-724">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="194ec-725">Tuttavia, il markup SVG inline non è supportato in tutti gli scenari.</span><span class="sxs-lookup"><span data-stu-id="194ec-725">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="194ec-726">Se si inserisce un tag di `<svg>` direttamente in un file di componente (*Razor*), il rendering delle immagini di base è supportato, ma molti scenari avanzati non sono ancora supportati.</span><span class="sxs-lookup"><span data-stu-id="194ec-726">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="194ec-727">Ad esempio, i tag di `<use>` non sono attualmente rispettati e non è possibile usare `@bind` con alcuni tag SVG.</span><span class="sxs-lookup"><span data-stu-id="194ec-727">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="194ec-728">Queste limitazioni verranno affrontate in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="194ec-728">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="194ec-729">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="194ec-729">Additional resources</span></span>

* <span data-ttu-id="194ec-730"><xref:security/blazor/server> &ndash; include informazioni aggiuntive sulla creazione di app di Blazor server che devono essere confrontate con l'esaurimento delle risorse.</span><span class="sxs-lookup"><span data-stu-id="194ec-730"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
