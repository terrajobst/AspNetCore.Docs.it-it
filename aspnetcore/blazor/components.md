---
title: Creare e usare ASP.NET Core componenti Razor
author: guardrex
description: Informazioni su come creare e usare i componenti Razor, tra cui la modalità di associazione ai dati, la gestione degli eventi e la gestione dei cicli di vita dei componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/30/2019
uid: blazor/components
ms.openlocfilehash: ea216e405e5be52b578e99a529d8c6a726ea9cdd
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688034"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="7bc9b-103">Creare e usare ASP.NET Core componenti Razor</span><span class="sxs-lookup"><span data-stu-id="7bc9b-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="7bc9b-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7bc9b-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="7bc9b-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7bc9b-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7bc9b-106">Le app Blazer vengono compilate usando i *componenti*.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="7bc9b-107">Un componente è un blocco di interfaccia utente (UI) autonomo, ad esempio una pagina, una finestra di dialogo o un form.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="7bc9b-108">Un componente include il markup HTML e la logica di elaborazione necessaria per inserire i dati o rispondere agli eventi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="7bc9b-109">I componenti sono flessibili e leggeri.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="7bc9b-110">Possono essere annidati, riutilizzati e condivisi tra i progetti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="7bc9b-111">Classi di componenti</span><span class="sxs-lookup"><span data-stu-id="7bc9b-111">Component classes</span></span>

<span data-ttu-id="7bc9b-112">I componenti sono implementati in file di componente [Razor](xref:mvc/views/razor) (*Razor*) usando una C# combinazione di e markup HTML.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="7bc9b-113">Un componente in blazer viene definito formalmente come *componente Razor*.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="7bc9b-114">Il nome di un componente deve iniziare con un carattere maiuscolo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="7bc9b-115">Ad esempio, *MyCoolComponent. Razor* è valido e *MyCoolComponent. Razor* non è valido.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="7bc9b-116">L'interfaccia utente per un componente viene definita utilizzando HTML.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="7bc9b-117">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="7bc9b-118">Quando viene compilata un'app, il markup C# HTML e la logica di rendering vengono convertiti in una classe Component.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="7bc9b-119">Il nome della classe generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="7bc9b-120">I membri della classe del componente vengono definiti in un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="7bc9b-121">Nel blocco `@code` , lo stato del componente (proprietà, campi) viene specificato con i metodi per la gestione degli eventi o per la definizione di altre logiche dei componenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="7bc9b-122">È consentito più di un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="7bc9b-123">Nelle anteprime precedenti di ASP.NET Core 3,0, `@functions` i blocchi venivano usati per lo stesso `@code` scopo dei blocchi nei componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="7bc9b-124">`@functions`i blocchi continuano a funzionare nei componenti Razor, ma è consigliabile usare `@code` il blocco in ASP.NET Core 3,0 Preview 6 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="7bc9b-125">I membri dei componenti possono essere usati come parte della logica di rendering del C# componente usando espressioni che `@`iniziano con.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="7bc9b-126">Ad esempio, viene C# eseguito il rendering di un campo `@` anteponendo il nome del campo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="7bc9b-127">Nell'esempio seguente viene valutato ed eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="7bc9b-128">`_headingFontStyle`al valore della proprietà CSS per `font-style`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="7bc9b-129">`_headingText`al contenuto dell' `<h1>` elemento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="7bc9b-130">Una volta eseguito il rendering iniziale del componente, il componente rigenera l'albero di rendering in risposta agli eventi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="7bc9b-131">Blazer confronta quindi il nuovo albero di rendering con quello precedente e applica le modifiche apportate all'Document Object Model (DOM) del browser.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-131">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="7bc9b-132">I componenti sono C# classi ordinarie e possono essere inseriti in qualsiasi punto all'interno di un progetto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="7bc9b-133">I componenti che producono pagine Web in genere risiedono nella cartella *pages* .</span><span class="sxs-lookup"><span data-stu-id="7bc9b-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="7bc9b-134">I componenti non di pagina vengono spesso inseriti nella cartella *condivisa* o in una cartella personalizzata aggiunta al progetto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="7bc9b-135">Per usare una cartella personalizzata, aggiungere lo spazio dei nomi della cartella personalizzata al componente padre o al file *_Imports. Razor* dell'app.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="7bc9b-136">Lo spazio dei nomi seguente, ad esempio, rende disponibili i componenti in una cartella di *componenti* quando lo `WebApplication`spazio dei nomi radice dell'app è:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="7bc9b-137">Integrare i componenti nelle app Razor Pages e MVC</span><span class="sxs-lookup"><span data-stu-id="7bc9b-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="7bc9b-138">Usare i componenti con le app Razor Pages e MVC esistenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="7bc9b-139">Non è necessario riscrivere le pagine o le visualizzazioni esistenti per usare i componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="7bc9b-140">Quando viene eseguito il rendering della pagina o della vista, i componenti vengono prerenderizzati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="7bc9b-141">Per eseguire il rendering di un componente da una pagina o da `RenderComponentAsync<TComponent>` una vista, usare il metodo helper HTML:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-141">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="7bc9b-142">Mentre le pagine e le visualizzazioni possono usare i componenti, il contrario non è vero.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-142">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="7bc9b-143">I componenti non possono usare scenari di visualizzazione e di pagina specifici, ad esempio visualizzazioni parziali e sezioni.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-143">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="7bc9b-144">Per usare la logica dalla visualizzazione parziale in un componente, scomporre la logica di visualizzazione parziale in un componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-144">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="7bc9b-145">Per altre informazioni su come viene eseguito il rendering dei componenti e lo stato del componente viene gestito nelle app del <xref:blazor/hosting-models> server blazer, vedere l'articolo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-145">For more information on how components are rendered and component state is managed in Blazor Server apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="7bc9b-146">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="7bc9b-146">Use components</span></span>

<span data-ttu-id="7bc9b-147">I componenti possono includere altri componenti dichiarando questi ultimi usando la sintassi degli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-147">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="7bc9b-148">Il markup per l'uso di un componente è simile a un tag HTML, in cui il nome del tag è il tipo di componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-148">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="7bc9b-149">L'associazione di attributi distingue tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-149">Attribute binding is case sensitive.</span></span> <span data-ttu-id="7bc9b-150">Ad esempio, `@bind` è valido e `@Bind` non è valido.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-150">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="7bc9b-151">Il markup seguente in *index. Razor* esegue il rendering `HeadingComponent` di un'istanza:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-151">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="7bc9b-152">*Components/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-152">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="7bc9b-153">Se un componente contiene un elemento HTML con una prima lettera maiuscola che non corrisponde a un nome di componente, viene emesso un avviso che indica che l'elemento ha un nome imprevisto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-153">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="7bc9b-154">L'aggiunta `@using` di un'istruzione per lo spazio dei nomi del componente rende disponibile il componente, che rimuove l'avviso.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-154">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="7bc9b-155">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="7bc9b-155">Component parameters</span></span>

<span data-ttu-id="7bc9b-156">I componenti possono avere *parametri del componente*, che vengono definiti usando proprietà pubbliche nella classe Component con `[Parameter]` l'attributo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-156">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="7bc9b-157">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-157">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="7bc9b-158">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-158">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="7bc9b-159">Nell'esempio seguente, `ParentComponent` imposta il valore `Title` della proprietà dell'oggetto `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-159">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="7bc9b-160">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-160">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="7bc9b-161">Contenuto figlio</span><span class="sxs-lookup"><span data-stu-id="7bc9b-161">Child content</span></span>

<span data-ttu-id="7bc9b-162">I componenti possono impostare il contenuto di un altro componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-162">Components can set the content of another component.</span></span> <span data-ttu-id="7bc9b-163">Il componente di assegnazione fornisce il contenuto tra i tag che specificano il componente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-163">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="7bc9b-164">Nell'esempio seguente, `ChildComponent` ha una `ChildContent` proprietà che rappresenta un oggetto `RenderFragment`, che rappresenta un segmento di interfaccia utente di cui eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-164">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="7bc9b-165">Il valore di `ChildContent` viene posizionato nel markup del componente in cui deve essere eseguito il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-165">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="7bc9b-166">Il valore di `ChildContent` viene ricevuto dal componente padre e ne viene eseguito il rendering all'interno `panel-body`del pannello bootstrap.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-166">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="7bc9b-167">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-167">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="7bc9b-168">La proprietà che riceve `RenderFragment` il contenuto deve essere `ChildContent` denominata per convenzione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-168">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="7bc9b-169">Gli elementi `ParentComponent` seguenti possono fornire contenuto per il `ChildComponent` rendering di inserendo il contenuto all' `<ChildComponent>` interno dei tag.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-169">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="7bc9b-170">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-170">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="7bc9b-171">Attributo splatting e parametri arbitrari</span><span class="sxs-lookup"><span data-stu-id="7bc9b-171">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="7bc9b-172">I componenti possono acquisire ed eseguire il rendering di attributi aggiuntivi oltre ai parametri dichiarati del componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-172">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="7bc9b-173">È possibile acquisire attributi aggiuntivi in un dizionario e quindi *Splatted* su un elemento quando il componente viene sottoposto a [@attributes](xref:mvc/views/razor#attributes) rendering usando la direttiva Razor.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-173">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="7bc9b-174">Questo scenario è utile quando si definisce un componente che produce un elemento di markup che supporta un'ampia gamma di personalizzazioni.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-174">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="7bc9b-175">Ad esempio, può essere noioso definire gli attributi separatamente per un `<input>` che supporta molti parametri.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-175">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="7bc9b-176">Nell'esempio seguente `<input>` , il primo elemento (`id="useIndividualParams"`) USA parametri dei singoli componenti, mentre il secondo `<input>` elemento (`id="useAttributesDict"`) usa l'attributo splatting:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-176">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="7bc9b-177">Il tipo del parametro deve implementare `IEnumerable<KeyValuePair<string, object>>` con chiavi di stringa.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-177">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="7bc9b-178">L' `IReadOnlyDictionary<string, object>` uso di è anche un'opzione in questo scenario.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-178">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="7bc9b-179">Gli elementi `<input>` sottoposti a rendering usando entrambi gli approcci sono identici:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-179">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="7bc9b-180">Per accettare attributi arbitrari, definire un parametro component usando l' `[Parameter]` attributo con la `CaptureUnmatchedValues` proprietà impostata su `true`:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-180">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="7bc9b-181">La `CaptureUnmatchedValues` proprietà su `[Parameter]` consente al parametro di trovare la corrispondenza con tutti gli attributi che non corrispondono ad altri parametri.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-181">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="7bc9b-182">Un componente può definire solo un singolo parametro con `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-182">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="7bc9b-183">Il tipo di proprietà utilizzato `CaptureUnmatchedValues` con deve essere assegnabile `Dictionary<string, object>` da con chiavi di stringa.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-183">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="7bc9b-184">`IEnumerable<KeyValuePair<string, object>>`o `IReadOnlyDictionary<string, object>` sono anche opzioni in questo scenario.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-184">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="7bc9b-185">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="7bc9b-185">Data binding</span></span>

<span data-ttu-id="7bc9b-186">L'associazione dati a entrambi i componenti e gli elementi DOM viene [@bind](xref:mvc/views/razor#bind) eseguita con l'attributo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-186">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="7bc9b-187">Nell'esempio seguente il `_italicsCheck` campo viene associato allo stato di selezione della casella di controllo:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-187">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="7bc9b-188">Quando la casella di controllo è selezionata e deselezionata, il valore della proprietà `true` viene `false`aggiornato rispettivamente a e.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-188">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="7bc9b-189">La casella di controllo viene aggiornata nell'interfaccia utente solo quando viene eseguito il rendering del componente, non in risposta alla modifica del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-189">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="7bc9b-190">Poiché i componenti eseguono il rendering dopo l'esecuzione del codice del gestore eventi, gli aggiornamenti delle proprietà vengono in genere riflessi immediatamente nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-190">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="7bc9b-191">L' `@bind` utilizzo di `CurrentValue` con una`<input @bind="CurrentValue" />`proprietà () è essenzialmente equivalente a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-191">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="7bc9b-192">Quando viene eseguito il rendering del componente `value` , l'oggetto dell'elemento di input `CurrentValue` deriva dalla proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-192">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="7bc9b-193">Quando l'utente digita nella casella di testo, l' `onchange` evento viene generato e la `CurrentValue` proprietà viene impostata sul valore modificato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-193">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="7bc9b-194">In realtà, la generazione del codice è un po' più complessa `@bind` perché gestisce alcuni casi in cui vengono eseguite le conversioni dei tipi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-194">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="7bc9b-195">In linea di `@bind` principio, associa il valore corrente di un'espressione `value` a un attributo e gestisce le modifiche utilizzando il gestore registrato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-195">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="7bc9b-196">`onchange` Oltre a gestire gli eventi con `@bind` la sintassi, una proprietà o un campo può essere associato utilizzando altri eventi specificando un [@bind-value](xref:mvc/views/razor#bind) attributo `event` con un[@bind-value:event](xref:mvc/views/razor#bind)parametro ().</span><span class="sxs-lookup"><span data-stu-id="7bc9b-196">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="7bc9b-197">Nell'esempio seguente viene associata la `CurrentValue` proprietà per l' `oninput` evento:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-197">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="7bc9b-198">Diversamente `onchange`da, che viene attivato quando l'elemento perde lo `oninput` stato attivo, viene attivato quando viene modificato il valore della casella di testo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-198">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="7bc9b-199">**Valori non analizzabili**</span><span class="sxs-lookup"><span data-stu-id="7bc9b-199">**Unparsable values**</span></span>

<span data-ttu-id="7bc9b-200">Quando un utente fornisce un valore non analizzabile a un elemento associato a un oggetto DataBound, il valore non analizzabile viene automaticamente ripristinato al valore precedente quando viene attivato l'evento bind.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-200">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="7bc9b-201">Si consideri lo scenario seguente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-201">Consider the following scenario:</span></span>

* <span data-ttu-id="7bc9b-202">Un `<input>` elemento è associato a un `int` tipo con un valore iniziale di `123`:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-202">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="7bc9b-203">L'utente aggiorna il valore dell'elemento a `123.45` nella pagina e modifica lo stato attivo dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-203">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="7bc9b-204">Nello scenario precedente, il valore dell'elemento viene ripristinato `123`in.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-204">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="7bc9b-205">Quando il valore `123.45` viene rifiutato a favore del valore originale di `123`, l'utente riconosce che il relativo valore non è stato accettato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-205">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="7bc9b-206">Per impostazione predefinita, l'associazione si applica all' `onchange` evento dell'`@bind="{PROPERTY OR FIELD}"`elemento ().</span><span class="sxs-lookup"><span data-stu-id="7bc9b-206">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="7bc9b-207">Utilizzare `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` per impostare un evento diverso.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-207">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="7bc9b-208">Per l' `oninput` evento (`@bind-value:event="oninput"`), la riversione viene eseguita dopo qualsiasi sequenza di tasti che introduce un valore non analizzabile.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-208">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="7bc9b-209">Quando la destinazione è `oninput` l'evento con `int`un tipo associato a, a un utente viene impedito `.` di digitare un carattere.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-209">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="7bc9b-210">Un `.` carattere viene immediatamente rimosso, quindi l'utente riceve il feedback immediato che sono consentiti solo numeri interi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-210">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="7bc9b-211">Esistono scenari in `oninput` cui il ripristino del valore dell'evento non è ideale, ad esempio quando l'utente deve essere autorizzato a cancellare un `<input>` valore non analizzabile.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-211">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="7bc9b-212">Le alternative includono:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-212">Alternatives include:</span></span>

* <span data-ttu-id="7bc9b-213">Non usare l' `oninput` evento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-213">Don't use the `oninput` event.</span></span> <span data-ttu-id="7bc9b-214">Usare l'evento `onchange` predefinito (`@bind="{PROPERTY OR FIELD}"`), in cui un valore non valido non viene ripristinato fino a quando l'elemento non perde lo stato attivo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-214">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="7bc9b-215">Eseguire l'associazione a un tipo nullable, `int?` ad `string`esempio o, e fornire la logica personalizzata per gestire le voci non valide.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-215">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="7bc9b-216">Utilizzare un [componente di convalida del modulo](xref:blazor/forms-validation), `InputNumber` ad `InputDate`esempio o.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-216">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="7bc9b-217">I componenti di convalida dei moduli includono il supporto predefinito per la gestione di input non validi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-217">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="7bc9b-218">Componenti di convalida dei moduli:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-218">Form validation components:</span></span>
  * <span data-ttu-id="7bc9b-219">Consente all'utente di fornire un input non valido e di ricevere errori di `EditContext`convalida nell'oggetto associato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-219">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="7bc9b-220">Visualizzare gli errori di convalida nell'interfaccia utente senza interferire con l'utente che immette dati Web Form aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-220">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="7bc9b-221">**Globalizzazione**</span><span class="sxs-lookup"><span data-stu-id="7bc9b-221">**Globalization**</span></span>

<span data-ttu-id="7bc9b-222">`@bind`i valori vengono formattati per la visualizzazione e l'analisi usando le regole delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-222">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="7bc9b-223">È possibile accedere alle impostazioni cultura correnti dalla <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-223">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="7bc9b-224">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) viene usato per i tipi di campo seguenti`<input type="{TYPE}" />`():</span><span class="sxs-lookup"><span data-stu-id="7bc9b-224">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="7bc9b-225">I tipi di campo precedenti:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-225">The preceding field types:</span></span>

* <span data-ttu-id="7bc9b-226">Vengono visualizzati utilizzando le regole di formattazione appropriate basate su browser.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-226">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="7bc9b-227">Non può contenere testo in formato libero.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-227">Can't contain free-form text.</span></span>
* <span data-ttu-id="7bc9b-228">Fornire le caratteristiche di interazione utente in base all'implementazione del browser.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-228">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="7bc9b-229">I tipi di campo seguenti hanno requisiti di formattazione specifici e non sono attualmente supportati da Blazer perché non sono supportati da tutti i browser principali:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-229">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="7bc9b-230">`@bind`supporta il `@bind:culture` parametro per fornire un <xref:System.Globalization.CultureInfo?displayProperty=fullName> oggetto per l'analisi e la formattazione di un valore.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-230">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="7bc9b-231">Non è consigliabile specificare impostazioni cultura quando si `date` usano `number` i tipi di campo e.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-231">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="7bc9b-232">`date`e `number` hanno il supporto predefinito di blazer che fornisce le impostazioni cultura richieste.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-232">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="7bc9b-233">Per informazioni su come impostare le impostazioni cultura dell'utente, vedere la sezione [localizzazione](#localization) .</span><span class="sxs-lookup"><span data-stu-id="7bc9b-233">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="7bc9b-234">**Stringhe di formato**</span><span class="sxs-lookup"><span data-stu-id="7bc9b-234">**Format strings**</span></span>

<span data-ttu-id="7bc9b-235">Il data binding funziona <xref:System.DateTime> con stringhe di [@bind:format](xref:mvc/views/razor#bind)formato usando.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-235">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="7bc9b-236">Altre espressioni di formato, ad esempio i formati di valuta o numerici, non sono disponibili in questo momento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-236">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="7bc9b-237">Nel codice precedente, il valore `<input>` predefinito `text`del tipo di campo`type`dell'elemento () è.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-237">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="7bc9b-238">`@bind:format`è supportato per l'associazione dei tipi .NET seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-238">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="7bc9b-239"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="7bc9b-239"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="7bc9b-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="7bc9b-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="7bc9b-241">L' `@bind:format` attributo specifica il formato di data da applicare all' `value` oggetto dell' `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-241">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="7bc9b-242">Il formato viene usato anche per analizzare il valore quando si `onchange` verifica un evento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-242">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="7bc9b-243">La specifica di un formato `date` per il tipo di campo non è consigliata perché Blazer dispone del supporto incorporato per formattare le date.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-243">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="7bc9b-244">**Parametri dei componenti**</span><span class="sxs-lookup"><span data-stu-id="7bc9b-244">**Component parameters**</span></span>

<span data-ttu-id="7bc9b-245">Il binding riconosce i parametri del `@bind-{property}` componente, dove può associare un valore di proprietà tra i componenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-245">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="7bc9b-246">Il componente figlio seguente (`ChildComponent`) ha un `Year` parametro del componente `YearChanged` e un callback:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-246">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="7bc9b-247">`EventCallback<T>`viene illustrato nella sezione [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="7bc9b-247">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="7bc9b-248">Il componente padre seguente utilizza `ChildComponent` e associa il `ParentYear` parametro `Year` dall'elemento padre al parametro nel componente figlio:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-248">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="7bc9b-249">Il caricamento `ParentComponent` di produce il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-249">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="7bc9b-250">`ParentYear` Se il valore della proprietà viene modificato selezionando il pulsante `ParentComponent`in, viene aggiornata la `Year` proprietà dell'oggetto `ChildComponent` .</span><span class="sxs-lookup"><span data-stu-id="7bc9b-250">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="7bc9b-251">Viene eseguito il rendering `Year` del nuovo valore di nell'interfaccia utente `ParentComponent` quando viene eseguito il rendering dell'oggetto:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-251">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="7bc9b-252">Il `Year` parametro è associabile perché contiene un evento `YearChanged` complementare che `Year` corrisponde al tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-252">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="7bc9b-253">Per convenzione, `<ChildComponent @bind-Year="ParentYear" />` equivale essenzialmente alla scrittura:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-253">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="7bc9b-254">In generale, una proprietà può essere associata a un gestore eventi corrispondente usando `@bind-property:event` l'attributo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-254">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="7bc9b-255">Ad esempio, la proprietà `MyProp` può essere associata a `MyEventHandler` utilizzando i due attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-255">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="7bc9b-256">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="7bc9b-256">Event handling</span></span>

<span data-ttu-id="7bc9b-257">I componenti Razor forniscono funzionalità di gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-257">Razor components provide event handling features.</span></span> <span data-ttu-id="7bc9b-258">Per un attributo dell'elemento HTML `on{event}` denominato (ad esempio `onclick` , `onsubmit`e) con un valore tipizzato da un delegato, i componenti Razor considera il valore dell'attributo come un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-258">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="7bc9b-259">Il nome dell'attributo è sempre formattato [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-259">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="7bc9b-260">Il codice seguente chiama il `UpdateHeading` metodo quando si seleziona il pulsante nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-260">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="7bc9b-261">Il codice seguente chiama il `CheckChanged` metodo quando la casella di controllo viene modificata nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-261">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="7bc9b-262">I gestori di eventi possono anche essere asincroni e restituire <xref:System.Threading.Tasks.Task>un oggetto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-262">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="7bc9b-263">Non è necessario chiamare `StateHasChanged()`manualmente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-263">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="7bc9b-264">Le eccezioni vengono registrate quando si verificano.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-264">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="7bc9b-265">Nell'esempio seguente, `UpdateHeading` viene chiamato in modo asincrono quando si seleziona il pulsante:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-265">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

### <a name="event-argument-types"></a><span data-ttu-id="7bc9b-266">Tipi di argomenti dell'evento</span><span class="sxs-lookup"><span data-stu-id="7bc9b-266">Event argument types</span></span>

<span data-ttu-id="7bc9b-267">Per alcuni eventi, i tipi di argomento dell'evento sono consentiti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-267">For some events, event argument types are permitted.</span></span> <span data-ttu-id="7bc9b-268">Se l'accesso a uno di questi tipi di evento non è necessario, non è obbligatorio nella chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-268">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="7bc9b-269">Nella tabella seguente sono riportati i `EventArgs` supportati.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-269">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="7bc9b-270">event</span><span class="sxs-lookup"><span data-stu-id="7bc9b-270">Event</span></span> | <span data-ttu-id="7bc9b-271">Classe</span><span class="sxs-lookup"><span data-stu-id="7bc9b-271">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="7bc9b-272">Appunti</span><span class="sxs-lookup"><span data-stu-id="7bc9b-272">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="7bc9b-273">Trascinare</span><span class="sxs-lookup"><span data-stu-id="7bc9b-273">Drag</span></span>             | <span data-ttu-id="7bc9b-274">`DragEventArgs`e contengono dati di`DataTransferItem` elementi trascinati. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="7bc9b-274">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="7bc9b-275">Errore</span><span class="sxs-lookup"><span data-stu-id="7bc9b-275">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="7bc9b-276">Stato attivo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-276">Focus</span></span>            | <span data-ttu-id="7bc9b-277">`FocusEventArgs`Non include il supporto `relatedTarget`per. &ndash;</span><span class="sxs-lookup"><span data-stu-id="7bc9b-277">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="7bc9b-278">Modifica di`<input>`</span><span class="sxs-lookup"><span data-stu-id="7bc9b-278">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="7bc9b-279">Tastiera</span><span class="sxs-lookup"><span data-stu-id="7bc9b-279">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="7bc9b-280">Mouse</span><span class="sxs-lookup"><span data-stu-id="7bc9b-280">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="7bc9b-281">Puntatore del mouse</span><span class="sxs-lookup"><span data-stu-id="7bc9b-281">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="7bc9b-282">Rotellina del mouse</span><span class="sxs-lookup"><span data-stu-id="7bc9b-282">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="7bc9b-283">Stato</span><span class="sxs-lookup"><span data-stu-id="7bc9b-283">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="7bc9b-284">Tocco</span><span class="sxs-lookup"><span data-stu-id="7bc9b-284">Touch</span></span>            | <span data-ttu-id="7bc9b-285">`TouchEventArgs`&ndash; rappresentaunsingolopuntodicontattosuun`TouchPoint` dispositivo sensibile al tocco.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-285">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="7bc9b-286">Per informazioni sulle proprietà e sul comportamento di gestione degli eventi degli eventi nella tabella precedente, vedere le [classi EventArgs nell'origine riferimento (ASPNET/AspNetCore Release/3.0 Branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-286">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="7bc9b-287">Espressioni lambda</span><span class="sxs-lookup"><span data-stu-id="7bc9b-287">Lambda expressions</span></span>

<span data-ttu-id="7bc9b-288">È inoltre possibile utilizzare le espressioni lambda:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-288">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="7bc9b-289">Spesso è consigliabile chiudere i valori aggiuntivi, ad esempio quando si esegue l'iterazione su un set di elementi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-289">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="7bc9b-290">Nell'esempio seguente vengono creati tre pulsanti, ciascuno dei quali `UpdateHeading` chiama il passaggio di un`MouseEventArgs`argomento di evento () e`buttonNumber`il relativo numero di pulsante () quando vengono selezionati nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-290">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="7bc9b-291">**Non** usare la variabile di ciclo (`i`) in un `for` ciclo direttamente in un'espressione lambda.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-291">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="7bc9b-292">In caso contrario, la stessa variabile viene utilizzata da tutte `i`le espressioni lambda causando che il valore di sia lo stesso in tutte le espressioni lambda.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-292">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="7bc9b-293">Acquisire sempre il relativo valore in una variabile locale`buttonNumber` (nell'esempio precedente) e quindi usarlo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-293">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="7bc9b-294">EventCallback</span><span class="sxs-lookup"><span data-stu-id="7bc9b-294">EventCallback</span></span>

<span data-ttu-id="7bc9b-295">Uno scenario comune con i componenti annidati è la volontà di eseguire il metodo di un componente padre quando si verifica&mdash;un evento del componente figlio `onclick` , ad esempio, quando si verifica un evento nell'elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-295">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="7bc9b-296">Per esporre gli eventi tra i componenti, `EventCallback`usare un oggetto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-296">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="7bc9b-297">Un componente padre può assegnare un metodo di callback a un componente `EventCallback`figlio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-297">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="7bc9b-298">Nell'app di esempio illustra come viene configurato il `onclick` gestore di un pulsante per ricevere un `EventCallback` delegato dall'oggetto dell'esempio `ParentComponent`. `ChildComponent`</span><span class="sxs-lookup"><span data-stu-id="7bc9b-298">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="7bc9b-299">Viene digitato `MouseEventArgs`con, appropriato per un `onclick` evento da un dispositivo periferico: `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="7bc9b-299">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="7bc9b-300">Imposta l'oggetto `EventCallback<T>` figlio sul relativo `ShowMessage` metodo: `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="7bc9b-300">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="7bc9b-301">Quando il pulsante è selezionato in `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-301">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="7bc9b-302">Viene `ParentComponent`chiamato `ShowMessage` il metodo di.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-302">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="7bc9b-303">`messageText`viene aggiornato e visualizzato nell'oggetto `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-303">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="7bc9b-304">Una chiamata a `StateHasChanged` non è obbligatoria nel metodo del callback (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-304">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="7bc9b-305">`StateHasChanged`viene chiamato automaticamente per eseguire il rendering `ParentComponent`di, proprio come gli eventi figlio attivano il rendering dei componenti nei gestori eventi che vengono eseguiti all'interno dell'elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-305">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="7bc9b-306">`EventCallback`e `EventCallback<T>` consentono delegati asincroni.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-306">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="7bc9b-307">`EventCallback<T>`è fortemente tipizzato e richiede un tipo di argomento specifico.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-307">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="7bc9b-308">`EventCallback`è debolmente tipizzato e consente qualsiasi tipo di argomento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-308">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="7bc9b-309">`EventCallback` Richiamare o `EventCallback<T>` con e`InvokeAsync` attendere :<xref:System.Threading.Tasks.Task></span><span class="sxs-lookup"><span data-stu-id="7bc9b-309">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="7bc9b-310">Usare `EventCallback` e`EventCallback<T>` per la gestione degli eventi e i parametri del componente di associazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-310">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="7bc9b-311">Preferisci l'oggetto `EventCallback<T>` fortemente `EventCallback`tipizzato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-311">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="7bc9b-312">`EventCallback<T>`fornisce un migliore feedback sugli errori agli utenti del componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-312">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="7bc9b-313">Analogamente ad altri gestori di eventi dell'interfaccia utente, la specifica del parametro dell'evento è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-313">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="7bc9b-314">Usare `EventCallback` quando non è stato passato alcun valore al callback.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-314">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="chained-bind"></a><span data-ttu-id="7bc9b-315">Binding concatenato</span><span class="sxs-lookup"><span data-stu-id="7bc9b-315">Chained bind</span></span>

<span data-ttu-id="7bc9b-316">Uno scenario comune consiste nel concatenare un parametro associato a dati a un elemento Page nell'output del componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-316">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="7bc9b-317">Questo scenario è denominato *Binding concatenato* perché si verificano simultaneamente più livelli di associazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-317">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="7bc9b-318">Un binding concatenato non può essere `@bind` implementato con la sintassi nell'elemento della pagina.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-318">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="7bc9b-319">Il gestore eventi e il valore devono essere specificati separatamente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-319">The event handler and value must be specified separately.</span></span> <span data-ttu-id="7bc9b-320">Un componente padre, tuttavia, può utilizzare `@bind` la sintassi con il parametro del componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-320">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="7bc9b-321">Il componente `PasswordField` seguente (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="7bc9b-321">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="7bc9b-322">Imposta il `<input>` valore di un elemento su `Password` una proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-322">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="7bc9b-323">Espone le modifiche della `Password` proprietà a un componente padre con un [EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-323">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

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

<span data-ttu-id="7bc9b-324">Il `PasswordField` componente viene usato in un altro componente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-324">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="7bc9b-325">Per eseguire controlli o intercettare gli errori sulla password nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-325">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="7bc9b-326">Creare un campo di supporto per `Password` (`password` nell'esempio di codice seguente).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-326">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="7bc9b-327">Eseguire i controlli o gli errori di trap `Password` nel Setter.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-327">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="7bc9b-328">Nell'esempio seguente viene fornito un feedback immediato all'utente se viene utilizzato uno spazio nel valore della password:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-328">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

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

## <a name="capture-references-to-components"></a><span data-ttu-id="7bc9b-329">Acquisisci riferimenti ai componenti</span><span class="sxs-lookup"><span data-stu-id="7bc9b-329">Capture references to components</span></span>

<span data-ttu-id="7bc9b-330">I riferimenti ai componenti forniscono un modo per fare riferimento a un'istanza del componente in modo da poter emettere comandi per `Show` tale `Reset`istanza, ad esempio o.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-330">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="7bc9b-331">Per acquisire un riferimento a un componente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-331">To capture a component reference:</span></span>

* <span data-ttu-id="7bc9b-332">Aggiungere un [@ref](xref:mvc/views/razor#ref) attributo al componente figlio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-332">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="7bc9b-333">Definire un campo con lo stesso tipo del componente figlio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-333">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="7bc9b-334">Quando viene eseguito il rendering del componente `loginDialog` , il campo viene popolato con l'istanza del `MyLoginDialog` componente figlio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-334">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="7bc9b-335">È quindi possibile richiamare i metodi .NET nell'istanza del componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-335">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7bc9b-336">La `loginDialog` variabile viene popolata solo dopo il rendering del componente e l'output include `MyLoginDialog` l'elemento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-336">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="7bc9b-337">Fino a quel momento, non c'è niente a cui fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-337">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="7bc9b-338">Per modificare i riferimenti ai componenti dopo che il componente ha terminato il `OnAfterRenderAsync` rendering `OnAfterRender` , usare i metodi o.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-338">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="7bc9b-339">Mentre l'acquisizione di riferimenti ai componenti usa una sintassi simile per l' [acquisizione di riferimenti a elementi](xref:blazor/javascript-interop#capture-references-to-elements), non è una funzionalità di [interoperabilità di JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="7bc9b-339">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="7bc9b-340">I riferimenti ai componenti non vengono passati&mdash;al codice JavaScript che vengono usati solo nel codice .NET.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-340">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="7bc9b-341">**Non** usare i riferimenti ai componenti per mutare lo stato dei componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-341">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="7bc9b-342">Usare invece i normali parametri dichiarativi per passare i dati ai componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-342">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="7bc9b-343">L'utilizzo di normali parametri dichiarativi restituisce automaticamente i componenti figlio che eseguono il rendering alle ore corrette.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-343">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="7bc9b-344">Richiama i metodi del componente esternamente per aggiornare lo stato</span><span class="sxs-lookup"><span data-stu-id="7bc9b-344">Invoke component methods externally to update state</span></span>

<span data-ttu-id="7bc9b-345">Blazer usa un `SynchronizationContext` per applicare un singolo thread logico di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-345">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="7bc9b-346">I metodi del ciclo di vita di un componente e tutti i callback di evento generati da Blazer vengono `SynchronizationContext`eseguiti in questo oggetto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-346">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="7bc9b-347">Nel caso in cui un componente deve essere aggiornato in base a un evento esterno, ad esempio un timer o altre notifiche, `InvokeAsync` usare il metodo, che invierà a blazer. `SynchronizationContext`</span><span class="sxs-lookup"><span data-stu-id="7bc9b-347">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="7bc9b-348">Si consideri ad esempio un *servizio Notifier* che può inviare notifiche a qualsiasi componente in ascolto dello stato aggiornato:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-348">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="7bc9b-349">Utilizzo di `NotifierService` per aggiornare un componente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-349">Usage of the `NotifierService` to update a component:</span></span>

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

<span data-ttu-id="7bc9b-350">Nell'esempio `NotifierService` precedente richiama il `OnNotify` metodo del componente `SynchronizationContext`all'esterno di Blazer.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-350">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="7bc9b-351">`InvokeAsync`viene usato per passare al contesto corretto e accodare un rendering.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-351">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="7bc9b-352">Usare \@la chiave per controllare la conservazione di elementi e componenti</span><span class="sxs-lookup"><span data-stu-id="7bc9b-352">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="7bc9b-353">Quando si esegue il rendering di un elenco di elementi o componenti e gli elementi o i componenti successivamente cambiano, l'algoritmo di differenziazione di Blazer deve decidere quali elementi o componenti precedenti possono essere conservati e come eseguire il mapping degli oggetti modello.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-353">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="7bc9b-354">In genere, questo processo è automatico e può essere ignorato, ma in alcuni casi potrebbe essere necessario controllare il processo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-354">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="7bc9b-355">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-355">Consider the following example:</span></span>

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

<span data-ttu-id="7bc9b-356">Il contenuto della `People` raccolta può cambiare con le voci inserite, eliminate o riordinate.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-356">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="7bc9b-357">Quando viene eseguito il rendering del componente, `<DetailsEditor>` è possibile che il componente venga `Details` modificato in modo da ricevere valori di parametro diversi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-357">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="7bc9b-358">Questo può causare un rirendering più complesso del previsto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-358">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="7bc9b-359">In alcuni casi, il rirendering può comportare differenze di comportamento visibili, ad esempio lo stato attivo degli elementi persi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-359">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="7bc9b-360">È possibile controllare il processo di mapping con `@key` l'attributo della direttiva.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-360">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="7bc9b-361">`@key`fa in modo che l'algoritmo di diffing garantisca la conservazione di elementi o componenti in base al valore della chiave:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-361">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="7bc9b-362">Quando la `People` raccolta viene modificata, l'algoritmo diffing mantiene l'associazione tra `<DetailsEditor>` istanze e `person` istanze:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-362">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="7bc9b-363">Se un `Person` oggetto viene eliminato `People` dall'elenco, solo l'istanza `<DetailsEditor>` corrispondente viene rimossa dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-363">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="7bc9b-364">Altre istanze rimangono invariate.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-364">Other instances are left unchanged.</span></span>
* <span data-ttu-id="7bc9b-365">Se un `Person` oggetto viene inserito in una determinata posizione nell'elenco, viene `<DetailsEditor>` inserita una nuova istanza in corrispondenza della posizione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-365">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="7bc9b-366">Altre istanze rimangono invariate.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-366">Other instances are left unchanged.</span></span>
* <span data-ttu-id="7bc9b-367">Se `Person` le voci vengono riordinate, le `<DetailsEditor>` istanze corrispondenti vengono mantenute e nuovamente ordinate nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-367">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="7bc9b-368">In alcuni scenari, l'utilizzo `@key` di riduce al minimo la complessità del rendering ed evita potenziali problemi con le parti con stato del DOM che cambiano, ad esempio la posizione dello stato attivo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-368">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7bc9b-369">Le chiavi sono locali per ogni elemento contenitore o componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-369">Keys are local to each container element or component.</span></span> <span data-ttu-id="7bc9b-370">Le chiavi non vengono confrontate globalmente nell'intero documento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-370">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="7bc9b-371">Quando usare \@la chiave</span><span class="sxs-lookup"><span data-stu-id="7bc9b-371">When to use \@key</span></span>

<span data-ttu-id="7bc9b-372">In genere, è opportuno usare `@key` ogni volta che viene eseguito il rendering di un elenco (ad esempio, in un `@foreach` blocco) e un `@key`valore appropriato per definire.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-372">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="7bc9b-373">È anche possibile usare `@key` per impedire a blazer di mantenere un sottoalbero di elementi o componenti quando un oggetto viene modificato:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-373">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="7bc9b-374">Se `@currentPerson` viene modificata, `@key` la direttiva attribute impone a blazer di rimuovere `<div>` l'intero oggetto e i relativi discendenti e di ricompilare il sottoalbero all'interno dell'interfaccia utente con nuovi elementi e componenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-374">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="7bc9b-375">Questo può essere utile se è necessario garantire che lo stato dell'interfaccia utente non venga `@currentPerson` mantenuto quando viene modificato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-375">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="7bc9b-376">Quando non usare \@la chiave</span><span class="sxs-lookup"><span data-stu-id="7bc9b-376">When not to use \@key</span></span>

<span data-ttu-id="7bc9b-377">Si verifica un costo in termini di prestazioni quando `@key`si verificano differenze con.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-377">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="7bc9b-378">Il costo delle prestazioni non è elevato, ma `@key` è sufficiente specificare solo se il controllo delle regole di conservazione degli elementi o dei componenti avvantaggia l'app.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-378">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="7bc9b-379">Anche se `@key` non viene usato, blazer conserva il più possibile le istanze di elementi e componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-379">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="7bc9b-380">L'unico vantaggio dell'utilizzo `@key` di è il controllo sulla *modalità* di mapping delle istanze del modello alle istanze dei componenti mantenute, anziché sull'algoritmo di diffing che seleziona il mapping.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-380">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="7bc9b-381">Valori da usare per la \@chiave</span><span class="sxs-lookup"><span data-stu-id="7bc9b-381">What values to use for \@key</span></span>

<span data-ttu-id="7bc9b-382">In genere, è opportuno fornire uno dei seguenti tipi di valore per `@key`:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-382">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="7bc9b-383">Istanze di oggetti modello (ad esempio, `Person` un'istanza come nell'esempio precedente).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-383">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="7bc9b-384">In questo modo si garantisce la conservazione in base all'uguaglianza del riferimento all'oggetto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-384">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="7bc9b-385">Identificatori univoci, ad esempio valori di chiave primaria di `int`tipo `string`, o `Guid`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-385">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="7bc9b-386">Verificare che i valori usati `@key` per non siano in conflitto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-386">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="7bc9b-387">Se vengono rilevati valori in conflitto all'interno dello stesso elemento padre, blazer genera un'eccezione perché non è in grado di eseguire il mapping deterministico di elementi o componenti precedenti a nuovi elementi o componenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-387">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="7bc9b-388">Utilizzare solo valori distinti, ad esempio istanze di oggetti o valori di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-388">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="7bc9b-389">Metodi del ciclo di vita</span><span class="sxs-lookup"><span data-stu-id="7bc9b-389">Lifecycle methods</span></span>

<span data-ttu-id="7bc9b-390">`OnInitializedAsync`ed `OnInitialized` eseguono il codice per inizializzare il componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-390">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="7bc9b-391">Per eseguire un'operazione asincrona, usare `OnInitializedAsync` e la `await` parola chiave sull'operazione:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-391">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="7bc9b-392">Per un'operazione sincrona, `OnInitialized`usare:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-392">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="7bc9b-393">`OnParametersSetAsync`e `OnParametersSet` vengono chiamati quando un componente riceve parametri dal padre e i valori vengono assegnati alle proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-393">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="7bc9b-394">Questi metodi vengono eseguiti dopo l'inizializzazione del componente e ogni volta che viene eseguito il rendering del componente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-394">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="7bc9b-395">`OnAfterRenderAsync`e `OnAfterRender` vengono chiamati dopo che un componente ha terminato il rendering.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-395">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="7bc9b-396">I riferimenti a elementi e componenti vengono popolati a questo punto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-396">Element and component references are populated at this point.</span></span> <span data-ttu-id="7bc9b-397">Usare questa fase per eseguire passaggi di inizializzazione aggiuntivi usando il contenuto sottoposto a rendering, ad esempio l'attivazione di librerie JavaScript di terze parti che operano sugli elementi DOM sottoposti a rendering.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-397">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="7bc9b-398">`OnAfterRender`*non viene chiamato quando si esegue il prerendering sul server.*</span><span class="sxs-lookup"><span data-stu-id="7bc9b-398">`OnAfterRender` *is not called when prerendering on the server.*</span></span>

<span data-ttu-id="7bc9b-399">Il `firstRender` parametro per `OnAfterRenderAsync` e `OnAfterRender` è:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-399">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="7bc9b-400">`true` Impostare la prima volta che l'istanza del componente viene richiamata.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-400">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="7bc9b-401">Garantisce che il lavoro di inizializzazione venga eseguito una sola volta.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-401">Ensures that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="7bc9b-402">Gestisci azioni asincrone incomplete durante il rendering</span><span class="sxs-lookup"><span data-stu-id="7bc9b-402">Handle incomplete async actions at render</span></span>

<span data-ttu-id="7bc9b-403">Le azioni asincrone eseguite negli eventi del ciclo di vita potrebbero non essere state completate prima del rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-403">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="7bc9b-404">Gli oggetti possono `null` essere o compilati in modo non completo con i dati mentre è in esecuzione il metodo del ciclo di vita.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-404">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="7bc9b-405">Fornire la logica di rendering per confermare che gli oggetti vengono inizializzati.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-405">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="7bc9b-406">Esegue il rendering degli elementi dell'interfaccia utente segnaposto (ad esempio, un `null`messaggio di caricamento) mentre gli oggetti sono.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-406">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="7bc9b-407">Nel componente `FetchData` dei `OnInitializedAsync` modelli di Blazer viene eseguito l'override di asincrono Receive forecast data (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-407">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="7bc9b-408">Quando `forecasts` è`null`, all'utente viene visualizzato un messaggio di caricamento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-408">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="7bc9b-409">Quando l' `Task` oggetto restituito `OnInitializedAsync` da viene completato, viene eseguito il rendering del componente con lo stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-409">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="7bc9b-410">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-410">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="7bc9b-411">Esegui codice prima dell'impostazione dei parametri</span><span class="sxs-lookup"><span data-stu-id="7bc9b-411">Execute code before parameters are set</span></span>

<span data-ttu-id="7bc9b-412">`SetParameters`è possibile eseguire l'override del codice prima di impostare i parametri:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-412">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="7bc9b-413">Se `base.SetParameters` non viene richiamato, il codice personalizzato può interpretare il valore dei parametri in ingresso in qualsiasi modo necessario.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-413">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="7bc9b-414">I parametri in ingresso, ad esempio, non devono essere assegnati alle proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-414">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="7bc9b-415">Non aggiornare l'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="7bc9b-415">Suppress refreshing of the UI</span></span>

<span data-ttu-id="7bc9b-416">`ShouldRender`è possibile eseguire l'override di per non aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-416">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="7bc9b-417">Se l'implementazione restituisce `true`, viene aggiornata l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-417">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="7bc9b-418">Anche se `ShouldRender` viene sottoposto a override, il componente viene sempre sottoposto a rendering iniziale.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-418">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="7bc9b-419">Eliminazione di componenti con IDisposable</span><span class="sxs-lookup"><span data-stu-id="7bc9b-419">Component disposal with IDisposable</span></span>

<span data-ttu-id="7bc9b-420">Se un componente implementa <xref:System.IDisposable>, il [metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose) viene chiamato quando il componente viene rimosso dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-420">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="7bc9b-421">Il componente seguente utilizza `@implements IDisposable` e il `Dispose` metodo:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-421">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="7bc9b-422">Routing</span><span class="sxs-lookup"><span data-stu-id="7bc9b-422">Routing</span></span>

<span data-ttu-id="7bc9b-423">Il routing in blazer viene effettuato fornendo un modello di route a ogni componente accessibile nell'app.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-423">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="7bc9b-424">Quando viene compilato un file Razor `@page` con una direttiva, alla classe generata viene assegnato un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> oggetto che specifica il modello di route.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-424">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="7bc9b-425">In fase di esecuzione, il router cerca le classi di `RouteAttribute` componenti con un oggetto ed esegue il rendering di qualsiasi componente con un modello di route corrispondente all'URL richiesto.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-425">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="7bc9b-426">È possibile applicare più modelli di route a un componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-426">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="7bc9b-427">Il componente seguente risponde alle richieste per `/BlazorRoute` e: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="7bc9b-427">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="7bc9b-428">Parametri di route</span><span class="sxs-lookup"><span data-stu-id="7bc9b-428">Route parameters</span></span>

<span data-ttu-id="7bc9b-429">I componenti possono ricevere parametri di route dal modello di route fornito `@page` nella direttiva.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-429">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="7bc9b-430">Il router usa parametri di route per popolare i parametri del componente corrispondente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-430">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="7bc9b-431">*Componente parametro di route*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-431">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="7bc9b-432">I parametri facoltativi non sono supportati `@page` , quindi vengono applicate due direttive nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-432">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="7bc9b-433">Il primo consente la navigazione al componente senza un parametro.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-433">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="7bc9b-434">La seconda `@page` direttiva accetta il `{text}` parametro Route e assegna il valore alla `Text` proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-434">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="7bc9b-435">Ereditarietà della classe base per un'esperienza di "code-behind"</span><span class="sxs-lookup"><span data-stu-id="7bc9b-435">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="7bc9b-436">I file dei componenti combinano C# il markup HTML e il codice di elaborazione nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-436">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="7bc9b-437">La `@inherits` direttiva può essere usata per fornire alle app Blazer un'esperienza di "code-behind" che separa il markup dei componenti dal codice di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-437">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="7bc9b-438">L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) Mostra come un componente può ereditare una classe `BlazorRocksBase`base,, per fornire le proprietà e i metodi del componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-438">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="7bc9b-439">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-439">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="7bc9b-440">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-440">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="7bc9b-441">La classe base deve derivare `ComponentBase`da.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-441">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="7bc9b-442">Importa componenti</span><span class="sxs-lookup"><span data-stu-id="7bc9b-442">Import components</span></span>

<span data-ttu-id="7bc9b-443">Lo spazio dei nomi di un componente creato con Razor si basa su:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-443">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="7bc9b-444">Oggetto del progetto `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-444">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="7bc9b-445">Percorso dalla radice del progetto al componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-445">The path from the project root to the component.</span></span> <span data-ttu-id="7bc9b-446">Ad esempio, `ComponentsSample/Pages/Index.razor` è nello spazio dei `ComponentsSample.Pages`nomi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-446">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="7bc9b-447">I componenti C# seguono le regole di associazione dei nomi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-447">Components follow C# name binding rules.</span></span> <span data-ttu-id="7bc9b-448">Nel caso di *index. Razor*, tutti i componenti nella stessa cartella, le *pagine*e la cartella padre, *ComponentsSample*, rientrano nell'ambito.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-448">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="7bc9b-449">I componenti definiti in uno spazio dei nomi diverso possono essere inseriti nell'ambito usando la direttiva [ \@using](xref:mvc/views/razor#using) di Razor.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-449">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="7bc9b-450">Se nella cartella `ComponentsSample/Shared/`è `NavMenu.razor`presente un altro componente, il componente può essere utilizzato in `Index.razor` con l'istruzione seguente `@using` :</span><span class="sxs-lookup"><span data-stu-id="7bc9b-450">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="7bc9b-451">È anche possibile fare riferimento ai componenti usando i relativi nomi completi, che elimina la necessità della [ \@direttiva using](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="7bc9b-451">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="7bc9b-452">La `global::` qualificazione non è supportata.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-452">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="7bc9b-453">L'importazione di componenti con `using` istruzioni con alias (ad `@using Foo = Bar`esempio,) non è supportata.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-453">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="7bc9b-454">I nomi parzialmente qualificati non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-454">Partially qualified names aren't supported.</span></span> <span data-ttu-id="7bc9b-455">Ad esempio, l' `@using ComponentsSample` aggiunta e `NavMenu.razor` il `<Shared.NavMenu></Shared.NavMenu>` riferimento a non sono supportate.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-455">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="7bc9b-456">Attributi dell'elemento HTML condizionale</span><span class="sxs-lookup"><span data-stu-id="7bc9b-456">Conditional HTML element attributes</span></span>

<span data-ttu-id="7bc9b-457">Gli attributi degli elementi HTML vengono sottoposti a rendering in modo condizionale in base al valore .NET.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-457">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="7bc9b-458">Se il valore è `false` o `null`, l'attributo non viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-458">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="7bc9b-459">Se il valore è `true`, l'attributo viene sottoposto a rendering ridotto a icona.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-459">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="7bc9b-460">Nell'esempio seguente, `IsCompleted` determina se `checked` viene eseguito il rendering nel markup dell'elemento:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-460">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="7bc9b-461">Se `IsCompleted` è`true`, viene eseguito il rendering della casella di controllo come segue:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-461">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="7bc9b-462">Se `IsCompleted` è`false`, viene eseguito il rendering della casella di controllo come segue:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-462">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="7bc9b-463">Per altre informazioni, vedere <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-463">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="7bc9b-464">Alcuni attributi HTML, ad esempio [aria-Pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), non funzionano correttamente quando il tipo .NET è `bool`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-464">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="7bc9b-465">In questi casi, usare un `string` tipo anziché un. `bool`</span><span class="sxs-lookup"><span data-stu-id="7bc9b-465">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="7bc9b-466">HTML non elaborato</span><span class="sxs-lookup"><span data-stu-id="7bc9b-466">Raw HTML</span></span>

<span data-ttu-id="7bc9b-467">Le stringhe vengono in genere sottoposte a rendering usando i nodi di testo DOM, il che significa che qualsiasi markup che può contenere viene ignorato e considerato come testo letterale.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-467">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="7bc9b-468">Per eseguire il rendering del codice HTML non elaborato, eseguire `MarkupString` il wrapping del contenuto HTML in un valore.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-468">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="7bc9b-469">Il valore viene analizzato in formato HTML o SVG e inserito nel DOM.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-469">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="7bc9b-470">Il rendering di codice HTML non elaborato costruito da un'origine non attendibile costituisce un rischio per la **sicurezza** e deve essere evitato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-470">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="7bc9b-471">Nell'esempio seguente viene illustrato l' `MarkupString` utilizzo del tipo per aggiungere un blocco di contenuto HTML statico all'output sottoposto a rendering di un componente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-471">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="7bc9b-472">Componenti basati su modelli</span><span class="sxs-lookup"><span data-stu-id="7bc9b-472">Templated components</span></span>

<span data-ttu-id="7bc9b-473">I componenti basati su modelli sono componenti che accettano uno o più modelli di interfaccia utente come parametri, che possono quindi essere usati come parte della logica di rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-473">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="7bc9b-474">I componenti basati su modelli consentono di creare componenti di livello superiore più riutilizzabili rispetto ai componenti normali.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-474">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="7bc9b-475">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-475">A couple of examples include:</span></span>

* <span data-ttu-id="7bc9b-476">Componente della tabella che consente a un utente di specificare i modelli per l'intestazione, le righe e il piè di pagina della tabella.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-476">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="7bc9b-477">Componente di elenco che consente a un utente di specificare un modello per il rendering degli elementi in un elenco.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-477">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="7bc9b-478">Parametri di modelli</span><span class="sxs-lookup"><span data-stu-id="7bc9b-478">Template parameters</span></span>

<span data-ttu-id="7bc9b-479">Un componente basato su modelli viene definito specificando uno o più parametri del componente `RenderFragment` di `RenderFragment<T>`tipo o.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-479">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="7bc9b-480">Un frammento di rendering rappresenta un segmento di interfaccia utente di cui eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-480">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="7bc9b-481">`RenderFragment<T>`accetta un parametro di tipo che può essere specificato quando viene richiamato il frammento di rendering.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-481">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="7bc9b-482">`TableTemplate`componente</span><span class="sxs-lookup"><span data-stu-id="7bc9b-482">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="7bc9b-483">Quando si usa un componente basato su modelli, i parametri del modello possono essere specificati usando gli elementi figlio che corrispondono ai nomi`TableHeader` dei `RowTemplate` parametri (e nell'esempio seguente):</span><span class="sxs-lookup"><span data-stu-id="7bc9b-483">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="7bc9b-484">Parametri di contesto del modello</span><span class="sxs-lookup"><span data-stu-id="7bc9b-484">Template context parameters</span></span>

<span data-ttu-id="7bc9b-485">Gli argomenti del componente `RenderFragment<T>` di tipo passati come elementi hanno un parametro `context` implicito denominato (ad esempio, nell'esempio `@context.PetId`di codice precedente,), ma è possibile modificare il `Context` nome del parametro usando l'attributo nell'elemento figlio elemento.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-485">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="7bc9b-486">Nell'esempio seguente, l' `RowTemplate` `Context` attributo dell'elemento specifica il `pet` parametro:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-486">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="7bc9b-487">In alternativa, è possibile specificare l' `Context` attributo sull'elemento Component.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-487">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="7bc9b-488">L'attributo `Context` specificato si applica a tutti i parametri di modello specificati.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-488">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="7bc9b-489">Questa operazione può essere utile quando si desidera specificare il nome del parametro di contenuto per il contenuto figlio implicito (senza alcun elemento figlio di wrapping).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-489">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="7bc9b-490">Nell'esempio seguente l' `Context` attributo viene visualizzato `TableTemplate` nell'elemento e si applica a tutti i parametri del modello:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-490">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="7bc9b-491">Componenti tipizzati in modo generico</span><span class="sxs-lookup"><span data-stu-id="7bc9b-491">Generic-typed components</span></span>

<span data-ttu-id="7bc9b-492">I componenti basati su modelli spesso sono tipizzati in modo generico.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-492">Templated components are often generically typed.</span></span> <span data-ttu-id="7bc9b-493">È ad esempio possibile utilizzare `ListViewTemplate` un componente generico per eseguire il `IEnumerable<T>` rendering dei valori.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-493">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="7bc9b-494">Per definire un componente generico, usare la [@typeparam](xref:mvc/views/razor#typeparam) direttiva per specificare i parametri di tipo:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-494">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="7bc9b-495">Quando si usano componenti tipizzati generici, il parametro di tipo viene dedotto, se possibile:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-495">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="7bc9b-496">In caso contrario, il parametro di tipo deve essere specificato in modo esplicito utilizzando un attributo che corrisponde al nome del parametro di tipo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-496">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="7bc9b-497">Nell'esempio seguente viene `TItem="Pet"` specificato il tipo:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-497">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="7bc9b-498">Parametri e valori di propagazione</span><span class="sxs-lookup"><span data-stu-id="7bc9b-498">Cascading values and parameters</span></span>

<span data-ttu-id="7bc9b-499">In alcuni scenari, non è pratico eseguire il flusso dei dati da un componente predecessore a un componente discendente usando i [parametri del componente](#component-parameters), soprattutto quando sono presenti diversi livelli di componenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-499">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="7bc9b-500">I valori e i parametri di propagazione consentono di risolvere questo problema fornendo un modo pratico per un componente predecessore per fornire un valore a tutti i relativi componenti discendenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-500">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="7bc9b-501">I parametri e i valori di propagazione forniscono anche un approccio per la coordinazione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-501">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="7bc9b-502">Esempio di tema</span><span class="sxs-lookup"><span data-stu-id="7bc9b-502">Theme example</span></span>

<span data-ttu-id="7bc9b-503">Nell'esempio seguente dall'app di esempio, la `ThemeInfo` classe specifica le informazioni sul tema per scorrere la gerarchia dei componenti in modo che tutti i pulsanti all'interno di una determinata parte dell'app condividono lo stesso stile.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-503">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="7bc9b-504">*UIThemeClasses/themeinfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-504">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="7bc9b-505">Un componente predecessore può fornire un valore di propagazione utilizzando il componente valore di propagazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-505">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="7bc9b-506">Il `CascadingValue` componente esegue il wrapping di un sottoalbero della gerarchia dei componenti e fornisce un singolo valore a tutti i componenti all'interno di tale sottoalbero.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-506">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="7bc9b-507">Ad esempio, l'app di esempio specifica le informazioni`ThemeInfo`sul tema () in uno dei layout dell'app come parametro di propagazione per tutti i componenti che costituiscono il corpo `@Body` del layout della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-507">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="7bc9b-508">`ButtonClass`viene assegnato un valore di `btn-success` nel componente layout.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-508">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="7bc9b-509">Qualsiasi componente discendente può utilizzare questa proprietà tramite `ThemeInfo` l'oggetto a cascata.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-509">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="7bc9b-510">`CascadingValuesParametersLayout`componente</span><span class="sxs-lookup"><span data-stu-id="7bc9b-510">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="7bc9b-511">Per utilizzare i valori di propagazione, i componenti dichiarano i `[CascadingParameter]` parametri di propagazione utilizzando l'attributo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-511">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="7bc9b-512">I valori a cascata vengono associati ai parametri di propagazione per tipo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-512">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="7bc9b-513">Nell'app di esempio, il `CascadingValuesParametersTheme` componente associa il `ThemeInfo` valore a cascata a un parametro di propagazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-513">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="7bc9b-514">Il parametro viene usato per impostare la classe CSS per uno dei pulsanti visualizzati dal componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-514">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="7bc9b-515">`CascadingValuesParametersTheme`componente</span><span class="sxs-lookup"><span data-stu-id="7bc9b-515">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="7bc9b-516">Per eseguire il propagazione di più valori dello stesso tipo all'interno dello stesso `Name` sottoalbero, `CascadingValue` fornire una stringa univoca a ogni componente e la corrispondente `CascadingParameter`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-516">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="7bc9b-517">Nell'esempio seguente due `CascadingValue` componenti propagano istanze diverse di `MyCascadingType` per nome:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-517">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

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

<span data-ttu-id="7bc9b-518">In un componente discendente i parametri a cascata ricevono i rispettivi valori dai valori a cascata corrispondenti nel componente predecessore per nome:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-518">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="7bc9b-519">Esempio di tabulazione</span><span class="sxs-lookup"><span data-stu-id="7bc9b-519">TabSet example</span></span>

<span data-ttu-id="7bc9b-520">I parametri di propagazione consentono inoltre ai componenti di collaborare attraverso la gerarchia dei componenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-520">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="7bc9b-521">Si consideri, ad esempio, l'esempio di *tabulazione* seguente nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-521">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="7bc9b-522">L'app di esempio ha `ITab` un'interfaccia implementata dalle schede:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-522">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="7bc9b-523">Il `CascadingValuesParametersTabSet` componente usa il `TabSet` componente, che contiene diversi `Tab` componenti:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-523">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="7bc9b-524">I componenti `Tab` figlio non vengono passati in modo esplicito come `TabSet`parametri a.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-524">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="7bc9b-525">Al contrario, i `Tab` componenti figlio fanno parte del contenuto figlio `TabSet`di.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-525">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="7bc9b-526">Tuttavia, `TabSet` è comunque necessario conoscere ogni `Tab` componente in modo che sia in grado di eseguire il rendering delle intestazioni e della scheda attiva. Per abilitare questo coordinamento senza richiedere codice aggiuntivo, il `TabSet` componente *può fornire se stesso come valore* di propagazione che viene quindi prelevato dai `Tab` componenti discendenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-526">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="7bc9b-527">`TabSet`componente</span><span class="sxs-lookup"><span data-stu-id="7bc9b-527">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="7bc9b-528">I componenti `Tab` discendenti acquisiscono `TabSet` l'oggetto che contiene come parametro di propagazione, in modo `TabSet` che i `Tab` componenti si aggiungano alla coordinata e della scheda attiva.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-528">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="7bc9b-529">`Tab`componente</span><span class="sxs-lookup"><span data-stu-id="7bc9b-529">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="7bc9b-530">Modelli Razor</span><span class="sxs-lookup"><span data-stu-id="7bc9b-530">Razor templates</span></span>

<span data-ttu-id="7bc9b-531">I frammenti di rendering possono essere definiti usando la sintassi del modello Razor.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-531">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="7bc9b-532">I modelli Razor sono un modo per definire un frammento di interfaccia utente e presupporre il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-532">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="7bc9b-533">Nell'esempio seguente viene illustrato come specificare `RenderFragment` i valori e `RenderFragment<T>` ed eseguire il rendering dei modelli direttamente in un componente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-533">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="7bc9b-534">I frammenti di rendering possono anche essere passati come argomenti ai [componenti basati su modelli](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-534">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="7bc9b-535">Output del codice precedente sottoposto a rendering:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-535">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="7bc9b-536">Logica RenderTreeBuilder manuale</span><span class="sxs-lookup"><span data-stu-id="7bc9b-536">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="7bc9b-537">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`fornisce metodi per la modifica di componenti ed elementi, inclusa la compilazione manuale C# dei componenti nel codice.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-537">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="7bc9b-538">L'utilizzo `RenderTreeBuilder` di per la creazione di componenti è uno scenario avanzato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-538">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="7bc9b-539">Un componente con formato non valido, ad esempio un tag di markup non chiuso, può causare un comportamento indefinito.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-539">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="7bc9b-540">Si consideri il componente seguente `PetDetails` , che può essere incorporato manualmente in un altro componente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-540">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="7bc9b-541">Nell'esempio seguente il ciclo nel `CreateComponent` metodo genera tre `PetDetails` componenti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-541">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="7bc9b-542">Quando si `RenderTreeBuilder` chiamano i metodi per creare i`OpenComponent` componenti `AddAttribute`(e), i numeri di sequenza sono numeri di riga del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-542">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="7bc9b-543">L'algoritmo di differenza Blaze si basa sui numeri di sequenza corrispondenti a righe di codice distinte, non sulle chiamate di chiamata distinti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-543">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="7bc9b-544">Quando si crea un componente `RenderTreeBuilder` con i metodi, impostare come hardcoded gli argomenti per i numeri di sequenza.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-544">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="7bc9b-545">**L'utilizzo di un calcolo o di un contatore per generare il numero di sequenza può causare un calo delle prestazioni.**</span><span class="sxs-lookup"><span data-stu-id="7bc9b-545">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="7bc9b-546">Per ulteriori informazioni, vedere la sezione [numeri di sequenza correlati ai numeri di riga del codice e non all'ordine di esecuzione](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="7bc9b-546">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="7bc9b-547">`BuiltContent`componente</span><span class="sxs-lookup"><span data-stu-id="7bc9b-547">`BuiltContent` component:</span></span>

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

> <span data-ttu-id="7bc9b-548">! AVVISO I tipi in `Microsoft.AspNetCore.Components.RenderTree` consentono l'elaborazione dei *risultati* delle operazioni di rendering.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-548">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="7bc9b-549">Questi sono i dettagli interni dell'implementazione del Framework blazer.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-549">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="7bc9b-550">Questi tipi devono essere considerati *instabili* e soggetti a modifiche nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-550">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="7bc9b-551">I numeri di sequenza sono correlati ai numeri di riga del codice e non all'ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="7bc9b-551">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="7bc9b-552">`.razor` I file Blazer vengono sempre compilati.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-552">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="7bc9b-553">Questo è potenzialmente un ottimo vantaggio per `.razor` perché il passaggio di compilazione può essere usato per inserire informazioni che migliorano le prestazioni dell'app in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-553">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="7bc9b-554">Un esempio fondamentale di questi miglioramenti riguarda i *numeri di sequenza*.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-554">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="7bc9b-555">I numeri di sequenza indicano al runtime quali output provengono da righe di codice distinte e ordinate.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-555">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="7bc9b-556">Il runtime usa queste informazioni per generare differenze di albero efficienti nel tempo lineare, che è molto più veloce rispetto a quanto normalmente è possibile per un algoritmo diff della struttura ad albero generale.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-556">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="7bc9b-557">Si consideri `.razor` il seguente file semplice:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-557">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="7bc9b-558">Il codice precedente viene compilato in un modo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-558">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="7bc9b-559">Quando il codice viene eseguito per la prima volta, se `someFlag` è `true`, il generatore riceve:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-559">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="7bc9b-560">Sequenza</span><span class="sxs-lookup"><span data-stu-id="7bc9b-560">Sequence</span></span> | <span data-ttu-id="7bc9b-561">Type</span><span class="sxs-lookup"><span data-stu-id="7bc9b-561">Type</span></span>      | <span data-ttu-id="7bc9b-562">Data</span><span class="sxs-lookup"><span data-stu-id="7bc9b-562">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="7bc9b-563">0</span><span class="sxs-lookup"><span data-stu-id="7bc9b-563">0</span></span>        | <span data-ttu-id="7bc9b-564">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-564">Text node</span></span> | <span data-ttu-id="7bc9b-565">Primo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-565">First</span></span>  |
| <span data-ttu-id="7bc9b-566">1</span><span class="sxs-lookup"><span data-stu-id="7bc9b-566">1</span></span>        | <span data-ttu-id="7bc9b-567">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-567">Text node</span></span> | <span data-ttu-id="7bc9b-568">Secondo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-568">Second</span></span> |

<span data-ttu-id="7bc9b-569">Si supponga `someFlag` che `false`diventi e che venga eseguito nuovamente il rendering del markup.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-569">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="7bc9b-570">Questa volta, il generatore riceve:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-570">This time, the builder receives:</span></span>

| <span data-ttu-id="7bc9b-571">Sequenza</span><span class="sxs-lookup"><span data-stu-id="7bc9b-571">Sequence</span></span> | <span data-ttu-id="7bc9b-572">Type</span><span class="sxs-lookup"><span data-stu-id="7bc9b-572">Type</span></span>       | <span data-ttu-id="7bc9b-573">Data</span><span class="sxs-lookup"><span data-stu-id="7bc9b-573">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="7bc9b-574">1</span><span class="sxs-lookup"><span data-stu-id="7bc9b-574">1</span></span>        | <span data-ttu-id="7bc9b-575">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-575">Text node</span></span>  | <span data-ttu-id="7bc9b-576">Secondo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-576">Second</span></span> |

<span data-ttu-id="7bc9b-577">Quando il runtime esegue una diff, rileva che l'elemento in sequenza `0` è stato rimosso, quindi genera lo script di *modifica*semplice seguente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-577">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="7bc9b-578">Rimuovere il primo nodo di testo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-578">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="7bc9b-579">Cosa accade se si generano numeri di sequenza a livello di codice</span><span class="sxs-lookup"><span data-stu-id="7bc9b-579">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="7bc9b-580">Si supponga invece che sia stata scritta la logica del generatore di albero di rendering seguente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-580">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="7bc9b-581">A questo punto, il primo output è:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-581">Now, the first output is:</span></span>

| <span data-ttu-id="7bc9b-582">Sequenza</span><span class="sxs-lookup"><span data-stu-id="7bc9b-582">Sequence</span></span> | <span data-ttu-id="7bc9b-583">Type</span><span class="sxs-lookup"><span data-stu-id="7bc9b-583">Type</span></span>      | <span data-ttu-id="7bc9b-584">Data</span><span class="sxs-lookup"><span data-stu-id="7bc9b-584">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="7bc9b-585">0</span><span class="sxs-lookup"><span data-stu-id="7bc9b-585">0</span></span>        | <span data-ttu-id="7bc9b-586">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-586">Text node</span></span> | <span data-ttu-id="7bc9b-587">Primo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-587">First</span></span>  |
| <span data-ttu-id="7bc9b-588">1</span><span class="sxs-lookup"><span data-stu-id="7bc9b-588">1</span></span>        | <span data-ttu-id="7bc9b-589">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-589">Text node</span></span> | <span data-ttu-id="7bc9b-590">Secondo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-590">Second</span></span> |

<span data-ttu-id="7bc9b-591">Questo risultato è identico al caso precedente, pertanto non esistono problemi negativi.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-591">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="7bc9b-592">`someFlag`si `false` trova nel secondo rendering e l'output è:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-592">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="7bc9b-593">Sequenza</span><span class="sxs-lookup"><span data-stu-id="7bc9b-593">Sequence</span></span> | <span data-ttu-id="7bc9b-594">Type</span><span class="sxs-lookup"><span data-stu-id="7bc9b-594">Type</span></span>      | <span data-ttu-id="7bc9b-595">Data</span><span class="sxs-lookup"><span data-stu-id="7bc9b-595">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="7bc9b-596">0</span><span class="sxs-lookup"><span data-stu-id="7bc9b-596">0</span></span>        | <span data-ttu-id="7bc9b-597">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-597">Text node</span></span> | <span data-ttu-id="7bc9b-598">Secondo</span><span class="sxs-lookup"><span data-stu-id="7bc9b-598">Second</span></span> |

<span data-ttu-id="7bc9b-599">Questa volta, l'algoritmo Diff rileva che si sono verificate *due* modifiche e l'algoritmo genera lo script di modifica seguente:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-599">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="7bc9b-600">Modificare il valore del primo nodo di testo in `Second`.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-600">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="7bc9b-601">Rimuovere il secondo nodo di testo.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-601">Remove the second text node.</span></span>

<span data-ttu-id="7bc9b-602">La generazione dei numeri di sequenza ha perso tutte le informazioni utili sul `if/else` punto in cui i rami e i cicli erano presenti nel codice originale.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-602">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="7bc9b-603">In questo modo si ottiene una differenza **due volte più a lungo** .</span><span class="sxs-lookup"><span data-stu-id="7bc9b-603">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="7bc9b-604">Questo è un esempio semplice.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-604">This is a trivial example.</span></span> <span data-ttu-id="7bc9b-605">Nei casi più realistici con strutture complesse e profondamente annidate e soprattutto con i cicli, il costo delle prestazioni è più grave.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-605">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="7bc9b-606">Invece di identificare immediatamente i blocchi o i rami del ciclo che sono stati inseriti o rimossi, l'algoritmo Diff deve ripresentarsi in modo approfondito negli alberi di rendering e, in genere, compilare script di modifica molto più lunghi perché non è stato informato in modo indesiderato sul modo in cui le nuove strutture relazione tra loro.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-606">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="7bc9b-607">Linee guida e conclusioni</span><span class="sxs-lookup"><span data-stu-id="7bc9b-607">Guidance and conclusions</span></span>

* <span data-ttu-id="7bc9b-608">Le prestazioni dell'app soffrono se i numeri di sequenza vengono generati dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-608">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="7bc9b-609">Il Framework non è in grado di creare automaticamente i propri numeri di sequenza in fase di esecuzione perché le informazioni necessarie non esistono a meno che non vengano acquisite in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-609">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="7bc9b-610">Non scrivere blocchi lunghi di logica implementata `RenderTreeBuilder` manualmente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-610">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="7bc9b-611">Preferire `.razor` i file e consentire al compilatore di gestire i numeri di sequenza.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-611">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="7bc9b-612">Se non si è in grado di `RenderTreeBuilder` evitare la logica manuale, suddividere i blocchi di codice lunghi `OpenRegion` in parti più piccole racchiuse tra / `CloseRegion` le chiamate.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-612">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="7bc9b-613">Ogni area ha il proprio spazio separato dei numeri di sequenza, quindi è possibile riavviare da zero (o qualsiasi altro numero arbitrario) all'interno di ogni area.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-613">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="7bc9b-614">Se i numeri di sequenza sono hardcoded, l'algoritmo Diff richiede solo che i numeri di sequenza aumentino nel valore.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-614">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="7bc9b-615">Il valore iniziale e i gap sono irrilevanti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-615">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="7bc9b-616">Una delle opzioni legittime consiste nell'usare il numero di riga del codice come numero di sequenza oppure iniziare da zero e aumentare di uno o di centinaia (o qualsiasi intervallo preferito).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-616">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="7bc9b-617">Blazer usa i numeri di sequenza, mentre altri Framework dell'interfaccia utente con differenze tra gli alberi non li usano.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-617">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="7bc9b-618">La diffing è molto più veloce quando si usano i numeri di sequenza e Blazer ha il vantaggio di un passaggio di compilazione che riguarda automaticamente i numeri di `.razor` sequenza per gli sviluppatori che creano file.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-618">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="7bc9b-619">Localizzazione</span><span class="sxs-lookup"><span data-stu-id="7bc9b-619">Localization</span></span>

<span data-ttu-id="7bc9b-620">Le app del server Blazer vengono localizzate usando il [middleware di localizzazione](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-620">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="7bc9b-621">Il middleware seleziona le impostazioni cultura appropriate per gli utenti che richiedono risorse dall'app.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-621">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="7bc9b-622">Le impostazioni cultura possono essere impostate utilizzando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-622">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="7bc9b-623">Cookie</span><span class="sxs-lookup"><span data-stu-id="7bc9b-623">Cookies</span></span>](#cookies)
* [<span data-ttu-id="7bc9b-624">Fornire l'interfaccia utente per scegliere le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="7bc9b-624">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="7bc9b-625">Per altre informazioni ed esempi, vedere <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-625">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="7bc9b-626">Cookie</span><span class="sxs-lookup"><span data-stu-id="7bc9b-626">Cookies</span></span>

<span data-ttu-id="7bc9b-627">Un cookie di impostazioni cultura di localizzazione può salvare in maniera permanente le impostazioni cultura dell'utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-627">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="7bc9b-628">Il cookie viene creato tramite il `OnGet` metodo della pagina host dell'app (*pages/host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="7bc9b-628">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="7bc9b-629">Il middleware di localizzazione legge il cookie nelle richieste successive per impostare le impostazioni cultura dell'utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-629">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="7bc9b-630">L'uso di un cookie garantisce che la connessione WebSocket possa propagare correttamente le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-630">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="7bc9b-631">Se gli schemi di localizzazione sono basati sul percorso dell'URL o sulla stringa di query, lo schema potrebbe non essere in grado di funzionare con WebSocket, quindi non è possibile salvare in modo permanente le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-631">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="7bc9b-632">Pertanto, l'utilizzo di un cookie di impostazioni cultura di localizzazione è l'approccio consigliato.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-632">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="7bc9b-633">Qualsiasi tecnica può essere utilizzata per assegnare impostazioni cultura se le impostazioni cultura vengono rese permanente in un cookie di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-633">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="7bc9b-634">Se l'app dispone già di uno schema di localizzazione stabilito per ASP.NET Core lato server, continuare a usare l'infrastruttura di localizzazione esistente dell'app e impostare il cookie delle impostazioni cultura di localizzazione nello schema dell'app.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-634">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="7bc9b-635">Nell'esempio seguente viene illustrato come impostare le impostazioni cultura correnti in un cookie che può essere letto dal middleware di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-635">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="7bc9b-636">Creare un file *pages/host. cshtml. cs* con il contenuto seguente nell'app del server Blazer:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-636">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="7bc9b-637">La localizzazione viene gestita nell'app:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-637">Localization is handled in the app:</span></span>

1. <span data-ttu-id="7bc9b-638">Il browser invia una richiesta HTTP iniziale all'app.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-638">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="7bc9b-639">Le impostazioni cultura vengono assegnate dal middleware di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-639">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="7bc9b-640">Il metodo `OnGet` in *_Host. cshtml. cs* rende permanente le impostazioni cultura di un cookie come parte della risposta.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-640">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="7bc9b-641">Il browser apre una connessione WebSocket per creare una sessione del server Interactive Blaze.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-641">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="7bc9b-642">Il middleware di localizzazione legge il cookie e assegna le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-642">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="7bc9b-643">La sessione del server Blaze inizia con le impostazioni cultura corrette.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-643">The Blazor Server session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="7bc9b-644">Fornire l'interfaccia utente per scegliere le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="7bc9b-644">Provide UI to choose the culture</span></span>

<span data-ttu-id="7bc9b-645">Per consentire a un utente di selezionare le impostazioni cultura, è consigliabile un *approccio basato su Reindirizzamento* .</span><span class="sxs-lookup"><span data-stu-id="7bc9b-645">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="7bc9b-646">Il processo è simile a quello che accade in un'app Web quando un utente tenta di accedere a una&mdash;risorsa protetta che l'utente viene reindirizzato a una pagina di accesso e quindi viene reindirizzato di nuovo alla risorsa originale.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-646">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="7bc9b-647">L'app rende permanente le impostazioni cultura selezionate dall'utente tramite un reindirizzamento a un controller.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-647">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="7bc9b-648">Il controller imposta le impostazioni cultura selezionate dall'utente in un cookie e reindirizza di nuovo l'utente all'URI originale.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-648">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="7bc9b-649">Stabilire un endpoint HTTP nel server per impostare le impostazioni cultura selezionate dall'utente in un cookie ed eseguire di nuovo il reindirizzamento all'URI originale:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-649">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="7bc9b-650">Usare il `LocalRedirect` risultato dell'azione per impedire gli attacchi di reindirizzamento aperti.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-650">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="7bc9b-651">Per altre informazioni, vedere <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-651">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="7bc9b-652">Il componente seguente mostra un esempio di come eseguire il reindirizzamento iniziale quando l'utente seleziona le impostazioni cultura:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-652">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

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

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="7bc9b-653">Usare scenari di localizzazione .NET in app Blazer</span><span class="sxs-lookup"><span data-stu-id="7bc9b-653">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="7bc9b-654">All'interno delle app blazer sono disponibili i seguenti scenari di globalizzazione e localizzazione di .NET:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-654">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="7bc9b-655">. Sistema di risorse di NET</span><span class="sxs-lookup"><span data-stu-id="7bc9b-655">.NET's resources system</span></span>
* <span data-ttu-id="7bc9b-656">Formattazione di numeri e date specifiche delle impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="7bc9b-656">Culture-specific number and date formatting</span></span>

<span data-ttu-id="7bc9b-657">La funzionalità di `@bind` Blazer esegue la globalizzazione in base alle impostazioni cultura correnti dell'utente.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-657">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="7bc9b-658">Per ulteriori informazioni, vedere la sezione [Data Binding](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="7bc9b-658">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="7bc9b-659">Sono attualmente supportati un set limitato di scenari di localizzazione di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-659">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="7bc9b-660">`IStringLocalizer<>`*è supportato* nelle app blazer.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-660">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="7bc9b-661">`IHtmlLocalizer<>`la `IViewLocalizer<>`localizzazione delle annotazioni dei dati, e è ASP.NET Core scenari MVC e **non è supportata** nelle app blazer.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-661">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="7bc9b-662">Per altre informazioni, vedere <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-662">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="7bc9b-663">Immagini SVG (Scalable Vector Graphics)</span><span class="sxs-lookup"><span data-stu-id="7bc9b-663">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="7bc9b-664">Poiché Blazer esegue il rendering di HTML, le immagini supportate dal browser, incluse le immagini SVG (Scalable Vector*Graphics),* sono supportate tramite `<img>` il tag:</span><span class="sxs-lookup"><span data-stu-id="7bc9b-664">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="7bc9b-665">Analogamente, le immagini SVG sono supportate nelle regole CSS di un file di foglio di stile (*CSS*):</span><span class="sxs-lookup"><span data-stu-id="7bc9b-665">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="7bc9b-666">Tuttavia, il markup SVG inline non è supportato in tutti gli scenari.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-666">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="7bc9b-667">Se si inserisce un `<svg>` tag direttamente in un file di componente (*Razor*), il rendering delle immagini di base è supportato, ma molti scenari avanzati non sono ancora supportati.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-667">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="7bc9b-668">Ad esempio, `<use>` i tag non sono attualmente rispettati e `@bind` non possono essere usati con alcuni tag svg.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-668">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="7bc9b-669">Queste limitazioni verranno affrontate in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-669">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7bc9b-670">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7bc9b-670">Additional resources</span></span>

* <span data-ttu-id="7bc9b-671"><xref:security/blazor/server>&ndash; Include informazioni aggiuntive sulla creazione di app del server blazer che devono essere confrontate con l'esaurimento delle risorse.</span><span class="sxs-lookup"><span data-stu-id="7bc9b-671"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
