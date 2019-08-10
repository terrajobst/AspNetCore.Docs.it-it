---
title: Creare e usare ASP.NET Core componenti Razor
author: guardrex
description: Informazioni su come creare e usare i componenti Razor, tra cui la modalità di associazione ai dati, la gestione degli eventi e la gestione dei cicli di vita dei componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/02/2019
uid: blazor/components
ms.openlocfilehash: 43457bffd748ebba68cc86d33fdeb98dc419704b
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68948431"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="d07c0-103">Creare e usare ASP.NET Core componenti Razor</span><span class="sxs-lookup"><span data-stu-id="d07c0-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="d07c0-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d07c0-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="d07c0-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d07c0-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d07c0-106">Le app Blazer vengono compilate usando i *componenti*.</span><span class="sxs-lookup"><span data-stu-id="d07c0-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="d07c0-107">Un componente è un blocco di interfaccia utente (UI) autonomo, ad esempio una pagina, una finestra di dialogo o un form.</span><span class="sxs-lookup"><span data-stu-id="d07c0-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="d07c0-108">Un componente include il markup HTML e la logica di elaborazione necessaria per inserire i dati o rispondere agli eventi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="d07c0-109">I componenti sono flessibili e leggeri.</span><span class="sxs-lookup"><span data-stu-id="d07c0-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="d07c0-110">Possono essere annidati, riutilizzati e condivisi tra i progetti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="d07c0-111">Classi di componenti</span><span class="sxs-lookup"><span data-stu-id="d07c0-111">Component classes</span></span>

<span data-ttu-id="d07c0-112">I componenti sono implementati in file di componente [Razor](xref:mvc/views/razor) (*Razor*) usando una C# combinazione di e markup HTML.</span><span class="sxs-lookup"><span data-stu-id="d07c0-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="d07c0-113">Un componente in blazer viene definito formalmente come *componente Razor*.</span><span class="sxs-lookup"><span data-stu-id="d07c0-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="d07c0-114">I componenti possono essere creati usando l'estensione di file *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="d07c0-114">Components can be authored using the *.cshtml* file extension.</span></span> <span data-ttu-id="d07c0-115">Usare la `_RazorComponentInclude` proprietà MSBuild nel file di progetto per identificare i file Component *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="d07c0-115">Use the `_RazorComponentInclude` MSBuild property in the project file to identify the component *.cshtml* files.</span></span> <span data-ttu-id="d07c0-116">Ad esempio, un'app che specifica che tutti i file con *estensione cshtml* nella cartella *pages* devono essere considerati come file di componenti Razor:</span><span class="sxs-lookup"><span data-stu-id="d07c0-116">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="d07c0-117">L'interfaccia utente per un componente viene definita utilizzando HTML.</span><span class="sxs-lookup"><span data-stu-id="d07c0-117">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="d07c0-118">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="d07c0-118">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="d07c0-119">Quando viene compilata un'app, il markup C# HTML e la logica di rendering vengono convertiti in una classe Component.</span><span class="sxs-lookup"><span data-stu-id="d07c0-119">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="d07c0-120">Il nome della classe generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="d07c0-120">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="d07c0-121">I membri della classe del componente vengono definiti in un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="d07c0-121">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="d07c0-122">Nel blocco `@code` , lo stato del componente (proprietà, campi) viene specificato con i metodi per la gestione degli eventi o per la definizione di altre logiche dei componenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-122">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="d07c0-123">È consentito più `@code` di un blocco.</span><span class="sxs-lookup"><span data-stu-id="d07c0-123">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="d07c0-124">Nelle anteprime precedenti di ASP.NET Core 3,0, `@functions` i blocchi venivano usati per lo stesso `@code` scopo dei blocchi nei componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="d07c0-124">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="d07c0-125">`@functions`i blocchi continuano a funzionare nei componenti Razor, ma è consigliabile usare `@code` il blocco in ASP.NET Core 3,0 Preview 6 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="d07c0-125">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="d07c0-126">I membri dei componenti possono essere usati come parte della logica di rendering del C# componente usando espressioni che `@`iniziano con.</span><span class="sxs-lookup"><span data-stu-id="d07c0-126">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="d07c0-127">Ad esempio, viene C# eseguito il rendering di un campo `@` anteponendo il nome del campo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-127">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="d07c0-128">Nell'esempio seguente viene valutato ed eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="d07c0-128">The following example evaluates and renders:</span></span>

* <span data-ttu-id="d07c0-129">`_headingFontStyle`al valore della proprietà CSS per `font-style`.</span><span class="sxs-lookup"><span data-stu-id="d07c0-129">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="d07c0-130">`_headingText`al contenuto dell' `<h1>` elemento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-130">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="d07c0-131">Una volta eseguito il rendering iniziale del componente, il componente rigenera l'albero di rendering in risposta agli eventi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-131">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="d07c0-132">Blazer confronta quindi il nuovo albero di rendering con quello precedente e applica le modifiche apportate all'Document Object Model (DOM) del browser.</span><span class="sxs-lookup"><span data-stu-id="d07c0-132">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="d07c0-133">I componenti sono C# classi ordinarie e possono essere inseriti in qualsiasi punto all'interno di un progetto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-133">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="d07c0-134">I componenti che producono pagine Web in genere risiedono nella cartella pages.</span><span class="sxs-lookup"><span data-stu-id="d07c0-134">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="d07c0-135">I componenti non di pagina vengono spesso inseriti nella cartella *condivisa* o in una cartella personalizzata aggiunta al progetto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-135">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="d07c0-136">Per usare una cartella personalizzata, aggiungere lo spazio dei nomi della cartella personalizzata al componente padre o al file *_Imports. Razor* dell'app.</span><span class="sxs-lookup"><span data-stu-id="d07c0-136">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="d07c0-137">Lo spazio dei nomi seguente, ad esempio, rende disponibili i componenti in una cartella di *componenti* quando lo `WebApplication`spazio dei nomi radice dell'app è:</span><span class="sxs-lookup"><span data-stu-id="d07c0-137">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="d07c0-138">Integrare i componenti nelle app Razor Pages e MVC</span><span class="sxs-lookup"><span data-stu-id="d07c0-138">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="d07c0-139">Usare i componenti con le app Razor Pages e MVC esistenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-139">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="d07c0-140">Non è necessario riscrivere le pagine o le visualizzazioni esistenti per usare i componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="d07c0-140">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="d07c0-141">Quando viene eseguito il rendering della pagina o della vista, i componenti vengono prerenderizzati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-141">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="d07c0-142">Per eseguire il rendering di un componente da una pagina o da `RenderComponentAsync<TComponent>` una vista, usare il metodo helper HTML:</span><span class="sxs-lookup"><span data-stu-id="d07c0-142">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="d07c0-143">Mentre le pagine e le visualizzazioni possono usare i componenti, il contrario non è vero.</span><span class="sxs-lookup"><span data-stu-id="d07c0-143">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="d07c0-144">I componenti non possono usare scenari di visualizzazione e di pagina specifici, ad esempio visualizzazioni parziali e sezioni.</span><span class="sxs-lookup"><span data-stu-id="d07c0-144">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="d07c0-145">Per usare la logica dalla visualizzazione parziale in un componente, scomporre la logica di visualizzazione parziale in un componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-145">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="d07c0-146">Per altre informazioni su come viene eseguito il rendering dei componenti e lo stato del componente viene gestito nelle app sul lato server <xref:blazor/hosting-models> di Blazer, vedere l'articolo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-146">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="using-components"></a><span data-ttu-id="d07c0-147">Uso dei componenti</span><span class="sxs-lookup"><span data-stu-id="d07c0-147">Using components</span></span>

<span data-ttu-id="d07c0-148">I componenti possono includere altri componenti dichiarando questi ultimi usando la sintassi degli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="d07c0-148">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="d07c0-149">Il markup per l'uso di un componente è simile a un tag HTML, in cui il nome del tag è il tipo di componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-149">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="d07c0-150">Il markup seguente in *index. Razor* esegue il rendering `HeadingComponent` di un'istanza:</span><span class="sxs-lookup"><span data-stu-id="d07c0-150">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="d07c0-151">*Components/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-151">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

## <a name="component-parameters"></a><span data-ttu-id="d07c0-152">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="d07c0-152">Component parameters</span></span>

<span data-ttu-id="d07c0-153">I componenti possono avere *parametri del componente*, che vengono definiti usando proprietà (in genere *non pubbliche*) nella classe Component con `[Parameter]` l'attributo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-153">Components can have *component parameters*, which are defined using properties (usually *non-public*) on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="d07c0-154">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="d07c0-154">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="d07c0-155">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-155">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="d07c0-156">Nell'esempio seguente, `ParentComponent` imposta il valore `Title` della proprietà dell'oggetto `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="d07c0-156">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="d07c0-157">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-157">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="d07c0-158">Contenuto figlio</span><span class="sxs-lookup"><span data-stu-id="d07c0-158">Child content</span></span>

<span data-ttu-id="d07c0-159">I componenti possono impostare il contenuto di un altro componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-159">Components can set the content of another component.</span></span> <span data-ttu-id="d07c0-160">Il componente di assegnazione fornisce il contenuto tra i tag che specificano il componente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="d07c0-160">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="d07c0-161">Nell'esempio seguente, `ChildComponent` ha una `ChildContent` proprietà che rappresenta un oggetto `RenderFragment`, che rappresenta un segmento di interfaccia utente di cui eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="d07c0-161">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="d07c0-162">Il valore di `ChildContent` viene posizionato nel markup del componente in cui deve essere eseguito il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-162">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="d07c0-163">Il valore di `ChildContent` viene ricevuto dal componente padre e ne viene eseguito il rendering all'interno `panel-body`del pannello bootstrap.</span><span class="sxs-lookup"><span data-stu-id="d07c0-163">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="d07c0-164">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-164">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="d07c0-165">La proprietà che riceve `RenderFragment` il contenuto deve essere `ChildContent` denominata per convenzione.</span><span class="sxs-lookup"><span data-stu-id="d07c0-165">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="d07c0-166">Gli elementi `ParentComponent` seguenti possono fornire contenuto per il `ChildComponent` rendering di inserendo il contenuto all' `<ChildComponent>` interno dei tag.</span><span class="sxs-lookup"><span data-stu-id="d07c0-166">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="d07c0-167">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-167">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="d07c0-168">Attributo splatting e parametri arbitrari</span><span class="sxs-lookup"><span data-stu-id="d07c0-168">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="d07c0-169">I componenti possono acquisire ed eseguire il rendering di attributi aggiuntivi oltre ai parametri dichiarati del componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-169">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="d07c0-170">È possibile acquisire attributi aggiuntivi in un dizionario e quindi *Splatted* su un elemento quando il componente viene sottoposto a [@attributes](xref:mvc/views/razor#attributes) rendering usando la direttiva Razor.</span><span class="sxs-lookup"><span data-stu-id="d07c0-170">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="d07c0-171">Questo scenario è utile quando si definisce un componente che produce un elemento di markup che supporta un'ampia gamma di personalizzazioni.</span><span class="sxs-lookup"><span data-stu-id="d07c0-171">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="d07c0-172">Ad esempio, può essere noioso definire gli attributi separatamente per un `<input>` che supporta molti parametri.</span><span class="sxs-lookup"><span data-stu-id="d07c0-172">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="d07c0-173">Nell'esempio seguente `<input>` , il primo elemento (`id="useIndividualParams"`) USA parametri dei singoli componenti, mentre il secondo `<input>` elemento (`id="useAttributesDict"`) usa l'attributo splatting:</span><span class="sxs-lookup"><span data-stu-id="d07c0-173">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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
    private string Maxlength { get; set; } = "10";

    [Parameter]
    private string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    private string Required { get; set; } = "required";

    [Parameter]
    private string Size { get; set; } = "50";

    [Parameter]
    private Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "true" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="d07c0-174">Il tipo del parametro deve implementare `IEnumerable<KeyValuePair<string, object>>` con chiavi di stringa.</span><span class="sxs-lookup"><span data-stu-id="d07c0-174">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="d07c0-175">L' `IReadOnlyDictionary<string, object>` uso di è anche un'opzione in questo scenario.</span><span class="sxs-lookup"><span data-stu-id="d07c0-175">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="d07c0-176">Gli elementi `<input>` sottoposti a rendering usando entrambi gli approcci sono identici:</span><span class="sxs-lookup"><span data-stu-id="d07c0-176">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="true"
       size="50">
```

<span data-ttu-id="d07c0-177">Per accettare attributi arbitrari, definire un parametro component usando l' `[Parameter]` attributo con la `CaptureUnmatchedValues` proprietà impostata su `true`:</span><span class="sxs-lookup"><span data-stu-id="d07c0-177">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    private Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="d07c0-178">La `CaptureUnmatchedValues` proprietà su `[Parameter]` consente al parametro di trovare la corrispondenza con tutti gli attributi che non corrispondono ad altri parametri.</span><span class="sxs-lookup"><span data-stu-id="d07c0-178">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="d07c0-179">Un componente può definire solo un singolo parametro con `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="d07c0-179">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="d07c0-180">Il tipo di proprietà utilizzato `CaptureUnmatchedValues` con deve essere assegnabile `Dictionary<string, object>` da con chiavi di stringa.</span><span class="sxs-lookup"><span data-stu-id="d07c0-180">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="d07c0-181">`IEnumerable<KeyValuePair<string, object>>`o `IReadOnlyDictionary<string, object>` sono anche opzioni in questo scenario.</span><span class="sxs-lookup"><span data-stu-id="d07c0-181">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="d07c0-182">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="d07c0-182">Data binding</span></span>

<span data-ttu-id="d07c0-183">L'associazione dati a entrambi i componenti e gli elementi DOM viene [@bind](xref:mvc/views/razor#bind) eseguita con l'attributo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-183">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="d07c0-184">Nell'esempio seguente il `_italicsCheck` campo viene associato allo stato di selezione della casella di controllo:</span><span class="sxs-lookup"><span data-stu-id="d07c0-184">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="d07c0-185">Quando la casella di controllo è selezionata e deselezionata, il valore della proprietà `true` viene `false`aggiornato rispettivamente a e.</span><span class="sxs-lookup"><span data-stu-id="d07c0-185">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="d07c0-186">La casella di controllo viene aggiornata nell'interfaccia utente solo quando viene eseguito il rendering del componente, non in risposta alla modifica del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="d07c0-186">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="d07c0-187">Poiché i componenti eseguono il rendering dopo l'esecuzione del codice del gestore eventi, gli aggiornamenti delle proprietà vengono in genere riflessi immediatamente nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-187">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="d07c0-188">L' `@bind` utilizzo di `CurrentValue` con una`<input @bind="CurrentValue" />`proprietà () è essenzialmente equivalente a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="d07c0-188">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="d07c0-189">Quando viene eseguito il rendering del componente `value` , l'oggetto dell'elemento di input `CurrentValue` deriva dalla proprietà.</span><span class="sxs-lookup"><span data-stu-id="d07c0-189">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="d07c0-190">Quando l'utente digita nella casella di testo, l' `onchange` evento viene generato e la `CurrentValue` proprietà viene impostata sul valore modificato.</span><span class="sxs-lookup"><span data-stu-id="d07c0-190">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="d07c0-191">In realtà, la generazione del codice è un po' più complessa `@bind` perché gestisce alcuni casi in cui vengono eseguite le conversioni dei tipi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-191">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="d07c0-192">In linea di `@bind` principio, associa il valore corrente di un'espressione `value` a un attributo e gestisce le modifiche utilizzando il gestore registrato.</span><span class="sxs-lookup"><span data-stu-id="d07c0-192">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="d07c0-193">`onchange` Oltre a gestire gli eventi con `@bind` la sintassi, una proprietà o un campo può essere associato utilizzando altri eventi specificando un [@bind-value](xref:mvc/views/razor#bind) attributo `event` con un[@bind-value:event](xref:mvc/views/razor#bind)parametro ().</span><span class="sxs-lookup"><span data-stu-id="d07c0-193">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="d07c0-194">Nell'esempio seguente viene associata la `CurrentValue` proprietà per l' `oninput` evento:</span><span class="sxs-lookup"><span data-stu-id="d07c0-194">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="d07c0-195">Diversamente `onchange`da, che viene attivato quando l'elemento perde lo `oninput` stato attivo, viene attivato quando viene modificato il valore della casella di testo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-195">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="d07c0-196">**Stringhe di formato**</span><span class="sxs-lookup"><span data-stu-id="d07c0-196">**Format strings**</span></span>

<span data-ttu-id="d07c0-197">Il data binding funziona <xref:System.DateTime> con stringhe di [@bind:format](xref:mvc/views/razor#bind)formato usando.</span><span class="sxs-lookup"><span data-stu-id="d07c0-197">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="d07c0-198">Altre espressioni di formato, ad esempio i formati di valuta o numerici, non sono disponibili in questo momento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-198">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="d07c0-199">L' `@bind:format` attributo specifica il formato di data da applicare all' `value` oggetto dell' `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-199">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="d07c0-200">Il formato viene usato anche per analizzare il valore quando si `onchange` verifica un evento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-200">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="d07c0-201">**Parametri dei componenti**</span><span class="sxs-lookup"><span data-stu-id="d07c0-201">**Component parameters**</span></span>

<span data-ttu-id="d07c0-202">Il binding riconosce i parametri del `@bind-{property}` componente, dove può associare un valore di proprietà tra i componenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-202">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="d07c0-203">Il componente figlio seguente (`ChildComponent`) ha un `Year` parametro del componente `YearChanged` e un callback:</span><span class="sxs-lookup"><span data-stu-id="d07c0-203">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="d07c0-204">`EventCallback<T>`viene illustrato nella sezione [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="d07c0-204">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="d07c0-205">Il componente padre seguente utilizza `ChildComponent` e associa il `ParentYear` parametro `Year` dall'elemento padre al parametro nel componente figlio:</span><span class="sxs-lookup"><span data-stu-id="d07c0-205">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="d07c0-206">Il caricamento `ParentComponent` di produce il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-206">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="d07c0-207">`ParentYear` Se il valore della proprietà viene modificato selezionando il pulsante `ParentComponent`in, viene aggiornata la `Year` proprietà dell'oggetto `ChildComponent` .</span><span class="sxs-lookup"><span data-stu-id="d07c0-207">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="d07c0-208">Viene eseguito il rendering `Year` del nuovo valore di nell'interfaccia utente `ParentComponent` quando viene eseguito il rendering dell'oggetto:</span><span class="sxs-lookup"><span data-stu-id="d07c0-208">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="d07c0-209">Il `Year` parametro è associabile perché contiene un evento `YearChanged` complementare che `Year` corrisponde al tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="d07c0-209">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="d07c0-210">Per convenzione, `<ChildComponent @bind-Year="ParentYear" />` equivale essenzialmente alla scrittura:</span><span class="sxs-lookup"><span data-stu-id="d07c0-210">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="d07c0-211">In generale, una proprietà può essere associata a un gestore eventi corrispondente usando `@bind-property:event` l'attributo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-211">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="d07c0-212">Ad esempio, la proprietà `MyProp` può essere associata a `MyEventHandler` utilizzando i due attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="d07c0-212">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="d07c0-213">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="d07c0-213">Event handling</span></span>

<span data-ttu-id="d07c0-214">I componenti Razor forniscono funzionalità di gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-214">Razor components provide event handling features.</span></span> <span data-ttu-id="d07c0-215">Per un attributo dell'elemento HTML `on{event}` denominato (ad esempio `onclick` , `onsubmit`e) con un valore tipizzato da un delegato, i componenti Razor considera il valore dell'attributo come un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-215">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="d07c0-216">Il nome dell'attributo è sempre formattato [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="d07c0-216">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="d07c0-217">Il codice seguente chiama il `UpdateHeading` metodo quando si seleziona il pulsante nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-217">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="d07c0-218">Il codice seguente chiama il `CheckChanged` metodo quando la casella di controllo viene modificata nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-218">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="d07c0-219">I gestori di eventi possono anche essere asincroni e restituire <xref:System.Threading.Tasks.Task>un oggetto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-219">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="d07c0-220">Non è necessario chiamare `StateHasChanged()`manualmente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-220">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="d07c0-221">Le eccezioni vengono registrate quando si verificano.</span><span class="sxs-lookup"><span data-stu-id="d07c0-221">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="d07c0-222">Nell'esempio seguente, `UpdateHeading` viene chiamato in modo asincrono quando si seleziona il pulsante:</span><span class="sxs-lookup"><span data-stu-id="d07c0-222">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="d07c0-223">Tipi di argomenti dell'evento</span><span class="sxs-lookup"><span data-stu-id="d07c0-223">Event argument types</span></span>

<span data-ttu-id="d07c0-224">Per alcuni eventi, i tipi di argomento dell'evento sono consentiti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-224">For some events, event argument types are permitted.</span></span> <span data-ttu-id="d07c0-225">Se l'accesso a uno di questi tipi di evento non è necessario, non è obbligatorio nella chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-225">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="d07c0-226">Le [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) supportate sono illustrate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-226">Supported [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) are shown in the following table.</span></span>

| <span data-ttu-id="d07c0-227">event</span><span class="sxs-lookup"><span data-stu-id="d07c0-227">Event</span></span> | <span data-ttu-id="d07c0-228">Classe</span><span class="sxs-lookup"><span data-stu-id="d07c0-228">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="d07c0-229">Appunti</span><span class="sxs-lookup"><span data-stu-id="d07c0-229">Clipboard</span></span> | `UIClipboardEventArgs` |
| <span data-ttu-id="d07c0-230">Trascinare</span><span class="sxs-lookup"><span data-stu-id="d07c0-230">Drag</span></span>  | <span data-ttu-id="d07c0-231">`UIDragEventArgs`viene usato per conservare i dati trascinati durante un'operazione di trascinamento della selezione e può essere uno o più `UIDataTransferItem`. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="d07c0-231">`UIDragEventArgs` &ndash; `DataTransfer` is used to hold the dragged data during a drag and drop operation and may hold one or more `UIDataTransferItem`.</span></span> <span data-ttu-id="d07c0-232">`UIDataTransferItem`rappresenta un elemento di dati di trascinamento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-232">`UIDataTransferItem` represents one drag data item.</span></span> |
| <span data-ttu-id="d07c0-233">Errore</span><span class="sxs-lookup"><span data-stu-id="d07c0-233">Error</span></span> | `UIErrorEventArgs` |
| <span data-ttu-id="d07c0-234">Stato attivo</span><span class="sxs-lookup"><span data-stu-id="d07c0-234">Focus</span></span> | <span data-ttu-id="d07c0-235">`UIFocusEventArgs`Non include il supporto `relatedTarget`per. &ndash;</span><span class="sxs-lookup"><span data-stu-id="d07c0-235">`UIFocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="d07c0-236">Modifica di`<input>`</span><span class="sxs-lookup"><span data-stu-id="d07c0-236">`<input>` change</span></span> | `UIChangeEventArgs` |
| <span data-ttu-id="d07c0-237">Tastiera</span><span class="sxs-lookup"><span data-stu-id="d07c0-237">Keyboard</span></span> | `UIKeyboardEventArgs` |
| <span data-ttu-id="d07c0-238">Mouse</span><span class="sxs-lookup"><span data-stu-id="d07c0-238">Mouse</span></span> | `UIMouseEventArgs` |
| <span data-ttu-id="d07c0-239">Puntatore del mouse</span><span class="sxs-lookup"><span data-stu-id="d07c0-239">Mouse pointer</span></span> | `UIPointerEventArgs` |
| <span data-ttu-id="d07c0-240">Rotellina del mouse</span><span class="sxs-lookup"><span data-stu-id="d07c0-240">Mouse wheel</span></span> | `UIWheelEventArgs` |
| <span data-ttu-id="d07c0-241">Avanzamento</span><span class="sxs-lookup"><span data-stu-id="d07c0-241">Progress</span></span> | `UIProgressEventArgs` |
| <span data-ttu-id="d07c0-242">Tocco</span><span class="sxs-lookup"><span data-stu-id="d07c0-242">Touch</span></span> | <span data-ttu-id="d07c0-243">`UITouchEventArgs`&ndash; rappresentaunsingolopuntodicontattosuun`UITouchPoint` dispositivo sensibile al tocco.</span><span class="sxs-lookup"><span data-stu-id="d07c0-243">`UITouchEventArgs` &ndash; `UITouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="d07c0-244">Per informazioni sulle proprietà e sul comportamento di gestione degli eventi della tabella precedente, vedere [classi EventArgs nell'origine riferimento](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span><span class="sxs-lookup"><span data-stu-id="d07c0-244">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="d07c0-245">Espressioni lambda</span><span class="sxs-lookup"><span data-stu-id="d07c0-245">Lambda expressions</span></span>

<span data-ttu-id="d07c0-246">È inoltre possibile utilizzare le espressioni lambda:</span><span class="sxs-lookup"><span data-stu-id="d07c0-246">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="d07c0-247">Spesso è consigliabile chiudere i valori aggiuntivi, ad esempio quando si esegue l'iterazione su un set di elementi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-247">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="d07c0-248">Nell'esempio seguente vengono creati tre pulsanti, ciascuno dei quali `UpdateHeading` chiama il passaggio di un`UIMouseEventArgs`argomento di evento () e`buttonNumber`il relativo numero di pulsante () quando vengono selezionati nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-248">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="d07c0-249">**Non** usare la variabile di ciclo (`i`) in un `for` ciclo direttamente in un'espressione lambda.</span><span class="sxs-lookup"><span data-stu-id="d07c0-249">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="d07c0-250">In caso contrario, la stessa variabile viene utilizzata da tutte `i`le espressioni lambda causando che il valore di sia lo stesso in tutte le espressioni lambda.</span><span class="sxs-lookup"><span data-stu-id="d07c0-250">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="d07c0-251">Acquisire sempre il relativo valore in una variabile locale`buttonNumber` (nell'esempio precedente) e quindi usarlo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-251">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="d07c0-252">EventCallback</span><span class="sxs-lookup"><span data-stu-id="d07c0-252">EventCallback</span></span>

<span data-ttu-id="d07c0-253">Uno scenario comune con i componenti annidati è la volontà di eseguire il metodo di un componente padre quando si verifica&mdash;un evento del componente figlio `onclick` , ad esempio, quando si verifica un evento nell'elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="d07c0-253">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="d07c0-254">Per esporre gli eventi tra i componenti, `EventCallback`usare un oggetto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-254">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="d07c0-255">Un componente padre può assegnare un metodo di callback a un componente `EventCallback`figlio.</span><span class="sxs-lookup"><span data-stu-id="d07c0-255">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="d07c0-256">Nell'app di esempio illustra come viene configurato il `onclick` gestore di un pulsante per ricevere un `EventCallback` delegato dall'oggetto dell'esempio `ParentComponent`. `ChildComponent`</span><span class="sxs-lookup"><span data-stu-id="d07c0-256">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="d07c0-257">Viene digitato `UIMouseEventArgs`con, appropriato per un `onclick` evento da un dispositivo periferico: `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="d07c0-257">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="d07c0-258">Imposta l'oggetto `EventCallback<T>` figlio sul relativo `ShowMessage` metodo: `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="d07c0-258">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="d07c0-259">Quando il pulsante è selezionato in `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="d07c0-259">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="d07c0-260">Viene `ParentComponent`chiamato `ShowMessage` il metodo di.</span><span class="sxs-lookup"><span data-stu-id="d07c0-260">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="d07c0-261">`messageText`viene aggiornato e visualizzato nell'oggetto `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="d07c0-261">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="d07c0-262">Una chiamata a `StateHasChanged` non è obbligatoria nel metodo del callback (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="d07c0-262">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="d07c0-263">`StateHasChanged`viene chiamato automaticamente per eseguire il rendering `ParentComponent`di, proprio come gli eventi figlio attivano il rendering dei componenti nei gestori eventi che vengono eseguiti all'interno dell'elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="d07c0-263">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="d07c0-264">`EventCallback`e `EventCallback<T>` consentono delegati asincroni.</span><span class="sxs-lookup"><span data-stu-id="d07c0-264">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="d07c0-265">`EventCallback<T>`è fortemente tipizzato e richiede un tipo di argomento specifico.</span><span class="sxs-lookup"><span data-stu-id="d07c0-265">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="d07c0-266">`EventCallback`è debolmente tipizzato e consente qualsiasi tipo di argomento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-266">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="d07c0-267">`EventCallback` Richiamare o `EventCallback<T>` con e`InvokeAsync` attendere :<xref:System.Threading.Tasks.Task></span><span class="sxs-lookup"><span data-stu-id="d07c0-267">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="d07c0-268">Usare `EventCallback` e`EventCallback<T>` per la gestione degli eventi e i parametri del componente di associazione.</span><span class="sxs-lookup"><span data-stu-id="d07c0-268">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="d07c0-269">Preferisci l'oggetto `EventCallback<T>` fortemente `EventCallback`tipizzato.</span><span class="sxs-lookup"><span data-stu-id="d07c0-269">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="d07c0-270">`EventCallback<T>`fornisce un migliore feedback sugli errori agli utenti del componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-270">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="d07c0-271">Analogamente ad altri gestori di eventi dell'interfaccia utente, la specifica del parametro dell'evento è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="d07c0-271">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="d07c0-272">Usare `EventCallback` quando non è stato passato alcun valore al callback.</span><span class="sxs-lookup"><span data-stu-id="d07c0-272">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="d07c0-273">Acquisisci riferimenti ai componenti</span><span class="sxs-lookup"><span data-stu-id="d07c0-273">Capture references to components</span></span>

<span data-ttu-id="d07c0-274">I riferimenti ai componenti forniscono un modo per fare riferimento a un'istanza del componente in modo da poter emettere comandi per `Show` tale `Reset`istanza, ad esempio o.</span><span class="sxs-lookup"><span data-stu-id="d07c0-274">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="d07c0-275">Per acquisire un riferimento a un componente, [@ref](xref:mvc/views/razor#ref) aggiungere un attributo al componente figlio e quindi definire un campo con lo stesso nome e lo stesso tipo del componente figlio.</span><span class="sxs-lookup"><span data-stu-id="d07c0-275">To capture a component reference, add a [@ref](xref:mvc/views/razor#ref) attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

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

<span data-ttu-id="d07c0-276">Quando viene eseguito il rendering del componente `loginDialog` , il campo viene popolato con l'istanza del `MyLoginDialog` componente figlio.</span><span class="sxs-lookup"><span data-stu-id="d07c0-276">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="d07c0-277">È quindi possibile richiamare i metodi .NET nell'istanza del componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-277">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d07c0-278">La `loginDialog` variabile viene popolata solo dopo il rendering del componente e l'output include `MyLoginDialog` l'elemento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-278">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="d07c0-279">Fino a quel momento, non c'è niente a cui fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-279">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="d07c0-280">Per modificare i riferimenti ai componenti dopo che il componente ha terminato il `OnAfterRenderAsync` rendering `OnAfterRender` , usare i metodi o.</span><span class="sxs-lookup"><span data-stu-id="d07c0-280">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="d07c0-281">Mentre l'acquisizione di riferimenti ai componenti usa una sintassi simile per l' [acquisizione di riferimenti a elementi](xref:blazor/javascript-interop#capture-references-to-elements), non è una funzionalità di interoperabilità di [JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="d07c0-281">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="d07c0-282">I riferimenti ai componenti non vengono passati&mdash;al codice JavaScript che vengono usati solo nel codice .NET.</span><span class="sxs-lookup"><span data-stu-id="d07c0-282">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="d07c0-283">**Non** usare i riferimenti ai componenti per mutare lo stato dei componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="d07c0-283">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="d07c0-284">Usare invece i normali parametri dichiarativi per passare i dati ai componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="d07c0-284">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="d07c0-285">L'utilizzo di normali parametri dichiarativi restituisce automaticamente i componenti figlio che eseguono il rendering alle ore corrette.</span><span class="sxs-lookup"><span data-stu-id="d07c0-285">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="d07c0-286">Usare \@la chiave per controllare la conservazione di elementi e componenti</span><span class="sxs-lookup"><span data-stu-id="d07c0-286">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="d07c0-287">Quando si esegue il rendering di un elenco di elementi o componenti e gli elementi o i componenti successivamente cambiano, l'algoritmo di differenziazione di Blazer deve decidere quali elementi o componenti precedenti possono essere conservati e come eseguire il mapping degli oggetti modello.</span><span class="sxs-lookup"><span data-stu-id="d07c0-287">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="d07c0-288">In genere, questo processo è automatico e può essere ignorato, ma in alcuni casi potrebbe essere necessario controllare il processo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-288">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="d07c0-289">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-289">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="d07c0-290">Il contenuto della `People` raccolta può cambiare con le voci inserite, eliminate o riordinate.</span><span class="sxs-lookup"><span data-stu-id="d07c0-290">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="d07c0-291">Quando viene eseguito il rendering del componente, `<DetailsEditor>` è possibile che il componente venga `Details` modificato in modo da ricevere valori di parametro diversi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-291">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="d07c0-292">Questo può causare un rirendering più complesso del previsto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-292">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="d07c0-293">In alcuni casi, il rirendering può comportare differenze di comportamento visibili, ad esempio lo stato attivo degli elementi persi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-293">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="d07c0-294">È possibile controllare il processo di mapping con `@key` l'attributo della direttiva.</span><span class="sxs-lookup"><span data-stu-id="d07c0-294">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="d07c0-295">`@key`fa in modo che l'algoritmo di diffing garantisca la conservazione di elementi o componenti in base al valore della chiave:</span><span class="sxs-lookup"><span data-stu-id="d07c0-295">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="d07c0-296">Quando la `People` raccolta viene modificata, l'algoritmo diffing mantiene l'associazione tra `<DetailsEditor>` istanze e `person` istanze:</span><span class="sxs-lookup"><span data-stu-id="d07c0-296">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="d07c0-297">Se un `Person` oggetto viene eliminato `People` dall'elenco, solo l'istanza `<DetailsEditor>` corrispondente viene rimossa dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-297">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="d07c0-298">Altre istanze rimangono invariate.</span><span class="sxs-lookup"><span data-stu-id="d07c0-298">Other instances are left unchanged.</span></span>
* <span data-ttu-id="d07c0-299">Se un `Person` oggetto viene inserito in una determinata posizione nell'elenco, viene `<DetailsEditor>` inserita una nuova istanza in corrispondenza della posizione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-299">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="d07c0-300">Altre istanze rimangono invariate.</span><span class="sxs-lookup"><span data-stu-id="d07c0-300">Other instances are left unchanged.</span></span>
* <span data-ttu-id="d07c0-301">Se `Person` le voci vengono riordinate, le `<DetailsEditor>` istanze corrispondenti vengono mantenute e nuovamente ordinate nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-301">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="d07c0-302">In alcuni scenari, l'utilizzo `@key` di riduce al minimo la complessità del rendering ed evita potenziali problemi con le parti con stato del DOM che cambiano, ad esempio la posizione dello stato attivo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-302">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d07c0-303">Le chiavi sono locali per ogni elemento contenitore o componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-303">Keys are local to each container element or component.</span></span> <span data-ttu-id="d07c0-304">Le chiavi non vengono confrontate globalmente nell'intero documento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-304">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="d07c0-305">Quando usare \@la chiave</span><span class="sxs-lookup"><span data-stu-id="d07c0-305">When to use \@key</span></span>

<span data-ttu-id="d07c0-306">In genere, è opportuno usare `@key` ogni volta che viene eseguito il rendering di un elenco (ad esempio, in un `@foreach` blocco) e un `@key`valore appropriato per definire.</span><span class="sxs-lookup"><span data-stu-id="d07c0-306">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="d07c0-307">È anche possibile usare `@key` per impedire a blazer di mantenere un sottoalbero di elementi o componenti quando un oggetto viene modificato:</span><span class="sxs-lookup"><span data-stu-id="d07c0-307">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="d07c0-308">Se `@currentPerson` viene modificata, `@key` la direttiva attribute impone a blazer di rimuovere `<div>` l'intero oggetto e i relativi discendenti e di ricompilare il sottoalbero all'interno dell'interfaccia utente con nuovi elementi e componenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-308">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="d07c0-309">Questo può essere utile se è necessario garantire che lo stato dell'interfaccia utente non venga `@currentPerson` mantenuto quando viene modificato.</span><span class="sxs-lookup"><span data-stu-id="d07c0-309">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="d07c0-310">Quando non usare \@la chiave</span><span class="sxs-lookup"><span data-stu-id="d07c0-310">When not to use \@key</span></span>

<span data-ttu-id="d07c0-311">Si verifica un costo in termini di prestazioni quando `@key`si verificano differenze con.</span><span class="sxs-lookup"><span data-stu-id="d07c0-311">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="d07c0-312">Il costo delle prestazioni non è elevato, ma `@key` è sufficiente specificare solo se il controllo delle regole di conservazione degli elementi o dei componenti avvantaggia l'app.</span><span class="sxs-lookup"><span data-stu-id="d07c0-312">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="d07c0-313">Anche se `@key` non viene usato, blazer conserva il più possibile le istanze di elementi e componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="d07c0-313">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="d07c0-314">L'unico vantaggio dell'utilizzo `@key` di è il controllo sulla *modalità* di mapping delle istanze del modello alle istanze dei componenti mantenute, anziché sull'algoritmo di diffing che seleziona il mapping.</span><span class="sxs-lookup"><span data-stu-id="d07c0-314">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="d07c0-315">Valori da usare per la \@chiave</span><span class="sxs-lookup"><span data-stu-id="d07c0-315">What values to use for \@key</span></span>

<span data-ttu-id="d07c0-316">In genere, è opportuno fornire uno dei seguenti tipi di valore per `@key`:</span><span class="sxs-lookup"><span data-stu-id="d07c0-316">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="d07c0-317">Istanze di oggetti modello (ad esempio, `Person` un'istanza come nell'esempio precedente).</span><span class="sxs-lookup"><span data-stu-id="d07c0-317">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="d07c0-318">In questo modo si garantisce la conservazione in base all'uguaglianza del riferimento all'oggetto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-318">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="d07c0-319">Identificatori univoci, ad esempio valori di chiave primaria di `int`tipo `string`, o `Guid`.</span><span class="sxs-lookup"><span data-stu-id="d07c0-319">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="d07c0-320">Evitare di fornire un valore che può scontrarsi in modo imprevisto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-320">Avoid supplying a value that can clash unexpectedly.</span></span> <span data-ttu-id="d07c0-321">Se `@key="@someObject.GetHashCode()"` viene fornito, possono verificarsi conflitti imprevisti perché i codici hash degli oggetti non correlati possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="d07c0-321">If `@key="@someObject.GetHashCode()"` is supplied, unexpected clashes may occur because the hash codes of unrelated objects can be the same.</span></span> <span data-ttu-id="d07c0-322">Se vengono richiesti `@key` valori in conflitto all'interno dello stesso elemento padre `@key` , i valori non verranno rispettati.</span><span class="sxs-lookup"><span data-stu-id="d07c0-322">If clashing `@key` values are requested within the same parent, the `@key` values won't be honored.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="d07c0-323">Metodi del ciclo di vita</span><span class="sxs-lookup"><span data-stu-id="d07c0-323">Lifecycle methods</span></span>

<span data-ttu-id="d07c0-324">`OnInitAsync`ed `OnInit` eseguono il codice per inizializzare il componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-324">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="d07c0-325">Per eseguire un'operazione asincrona, usare `OnInitAsync` e la `await` parola chiave sull'operazione:</span><span class="sxs-lookup"><span data-stu-id="d07c0-325">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="d07c0-326">Per un'operazione sincrona, `OnInit`usare:</span><span class="sxs-lookup"><span data-stu-id="d07c0-326">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="d07c0-327">`OnParametersSetAsync`e `OnParametersSet` vengono chiamati quando un componente riceve parametri dal padre e i valori vengono assegnati alle proprietà.</span><span class="sxs-lookup"><span data-stu-id="d07c0-327">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="d07c0-328">Questi metodi vengono eseguiti dopo l'inizializzazione del componente e ogni volta che viene eseguito il rendering del componente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-328">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="d07c0-329">`OnAfterRenderAsync`e `OnAfterRender` vengono chiamati dopo che un componente ha terminato il rendering.</span><span class="sxs-lookup"><span data-stu-id="d07c0-329">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="d07c0-330">I riferimenti a elementi e componenti vengono popolati a questo punto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-330">Element and component references are populated at this point.</span></span> <span data-ttu-id="d07c0-331">Usare questa fase per eseguire passaggi di inizializzazione aggiuntivi usando il contenuto sottoposto a rendering, ad esempio l'attivazione di librerie JavaScript di terze parti che operano sugli elementi DOM sottoposti a rendering.</span><span class="sxs-lookup"><span data-stu-id="d07c0-331">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="d07c0-332">Gestisci azioni asincrone incomplete durante il rendering</span><span class="sxs-lookup"><span data-stu-id="d07c0-332">Handle incomplete async actions at render</span></span>

<span data-ttu-id="d07c0-333">Le azioni asincrone eseguite negli eventi del ciclo di vita potrebbero non essere state completate prima del rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-333">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="d07c0-334">Gli oggetti possono `null` essere o compilati in modo non completo con i dati mentre è in esecuzione il metodo del ciclo di vita.</span><span class="sxs-lookup"><span data-stu-id="d07c0-334">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="d07c0-335">Fornire la logica di rendering per confermare che gli oggetti vengono inizializzati.</span><span class="sxs-lookup"><span data-stu-id="d07c0-335">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="d07c0-336">Esegue il rendering degli elementi dell'interfaccia utente segnaposto (ad esempio, un `null`messaggio di caricamento) mentre gli oggetti sono.</span><span class="sxs-lookup"><span data-stu-id="d07c0-336">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="d07c0-337">Nel componente `FetchData` dei `OnInitAsync` modelli di Blazer viene eseguito l'override di asincrono Receive forecast data (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="d07c0-337">In the `FetchData` component of the Blazor templates, `OnInitAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="d07c0-338">Quando `forecasts` è`null`, all'utente viene visualizzato un messaggio di caricamento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-338">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="d07c0-339">Quando l' `Task` oggetto restituito `OnInitAsync` da viene completato, viene eseguito il rendering del componente con lo stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="d07c0-339">After the `Task` returned by `OnInitAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="d07c0-340">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-340">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="d07c0-341">Esegui codice prima dell'impostazione dei parametri</span><span class="sxs-lookup"><span data-stu-id="d07c0-341">Execute code before parameters are set</span></span>

<span data-ttu-id="d07c0-342">`SetParameters`è possibile eseguire l'override del codice prima di impostare i parametri:</span><span class="sxs-lookup"><span data-stu-id="d07c0-342">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="d07c0-343">Se `base.SetParameters` non viene richiamato, il codice personalizzato può interpretare il valore dei parametri in ingresso in qualsiasi modo necessario.</span><span class="sxs-lookup"><span data-stu-id="d07c0-343">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="d07c0-344">I parametri in ingresso, ad esempio, non devono essere assegnati alle proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="d07c0-344">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="d07c0-345">Non aggiornare l'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="d07c0-345">Suppress refreshing of the UI</span></span>

<span data-ttu-id="d07c0-346">`ShouldRender`è possibile eseguire l'override di per non aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-346">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="d07c0-347">Se l'implementazione restituisce `true`, viene aggiornata l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-347">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="d07c0-348">Anche se `ShouldRender` viene sottoposto a override, il componente viene sempre sottoposto a rendering iniziale.</span><span class="sxs-lookup"><span data-stu-id="d07c0-348">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="d07c0-349">Eliminazione di componenti con IDisposable</span><span class="sxs-lookup"><span data-stu-id="d07c0-349">Component disposal with IDisposable</span></span>

<span data-ttu-id="d07c0-350">Se un componente implementa <xref:System.IDisposable>, il [metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose) viene chiamato quando il componente viene rimosso dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-350">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="d07c0-351">Il componente seguente utilizza `@implements IDisposable` e il `Dispose` metodo:</span><span class="sxs-lookup"><span data-stu-id="d07c0-351">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="d07c0-352">Routing</span><span class="sxs-lookup"><span data-stu-id="d07c0-352">Routing</span></span>

<span data-ttu-id="d07c0-353">Il routing in blazer viene effettuato fornendo un modello di route a ogni componente accessibile nell'app.</span><span class="sxs-lookup"><span data-stu-id="d07c0-353">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="d07c0-354">Quando viene compilato un file Razor `@page` con una direttiva, alla classe generata viene assegnato un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> oggetto che specifica il modello di route.</span><span class="sxs-lookup"><span data-stu-id="d07c0-354">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="d07c0-355">In fase di esecuzione, il router cerca le classi di `RouteAttribute` componenti con un oggetto ed esegue il rendering di qualsiasi componente con un modello di route corrispondente all'URL richiesto.</span><span class="sxs-lookup"><span data-stu-id="d07c0-355">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="d07c0-356">È possibile applicare più modelli di route a un componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-356">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="d07c0-357">Il componente seguente risponde alle richieste per `/BlazorRoute` e: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="d07c0-357">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="d07c0-358">Parametri di route</span><span class="sxs-lookup"><span data-stu-id="d07c0-358">Route parameters</span></span>

<span data-ttu-id="d07c0-359">I componenti possono ricevere parametri di route dal modello di route fornito `@page` nella direttiva.</span><span class="sxs-lookup"><span data-stu-id="d07c0-359">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="d07c0-360">Il router usa parametri di route per popolare i parametri del componente corrispondente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-360">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="d07c0-361">*Componente parametro di route*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-361">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="d07c0-362">I parametri facoltativi non sono supportati `@page` , quindi vengono applicate due direttive nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-362">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="d07c0-363">Il primo consente la navigazione al componente senza un parametro.</span><span class="sxs-lookup"><span data-stu-id="d07c0-363">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="d07c0-364">La seconda `@page` direttiva accetta il `{text}` parametro Route e assegna il valore alla `Text` proprietà.</span><span class="sxs-lookup"><span data-stu-id="d07c0-364">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="d07c0-365">Ereditarietà della classe base per un'esperienza di "code-behind"</span><span class="sxs-lookup"><span data-stu-id="d07c0-365">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="d07c0-366">I file dei componenti combinano C# il markup HTML e il codice di elaborazione nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="d07c0-366">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="d07c0-367">La `@inherits` direttiva può essere usata per fornire alle app Blazer un'esperienza di "code-behind" che separa il markup dei componenti dal codice di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="d07c0-367">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="d07c0-368">L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) Mostra come un componente può ereditare una classe `BlazorRocksBase`base,, per fornire le proprietà e i metodi del componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-368">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="d07c0-369">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-369">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="d07c0-370">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-370">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="d07c0-371">La classe base deve derivare `ComponentBase`da.</span><span class="sxs-lookup"><span data-stu-id="d07c0-371">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="d07c0-372">Importa componenti</span><span class="sxs-lookup"><span data-stu-id="d07c0-372">Import components</span></span>

<span data-ttu-id="d07c0-373">Lo spazio dei nomi di un componente creato con Razor si basa su:</span><span class="sxs-lookup"><span data-stu-id="d07c0-373">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="d07c0-374">Oggetto del progetto `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="d07c0-374">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="d07c0-375">Percorso dalla radice del progetto al componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-375">The path from the project root to the component.</span></span> <span data-ttu-id="d07c0-376">Ad esempio, `ComponentsSample/Pages/Index.razor` è nello spazio dei `ComponentsSample.Pages`nomi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-376">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="d07c0-377">I componenti C# seguono le regole di associazione dei nomi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-377">Components follow C# name binding rules.</span></span> <span data-ttu-id="d07c0-378">Nel caso di *index. Razor*, tutti i componenti nella stessa cartella, le *pagine*e la cartella padre, *ComponentsSample*, rientrano nell'ambito.</span><span class="sxs-lookup"><span data-stu-id="d07c0-378">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="d07c0-379">I componenti definiti in uno spazio dei nomi diverso possono essere inseriti nell'ambito usando la direttiva [ \@using](xref:mvc/views/razor#using) di Razor.</span><span class="sxs-lookup"><span data-stu-id="d07c0-379">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="d07c0-380">Se nella cartella `ComponentsSample/Shared/`è `NavMenu.razor`presente un altro componente, il componente può essere utilizzato in `Index.razor` con l'istruzione seguente `@using` :</span><span class="sxs-lookup"><span data-stu-id="d07c0-380">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="d07c0-381">È anche possibile fare riferimento ai componenti usando i relativi nomi completi, che elimina la necessità della [ \@direttiva using](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="d07c0-381">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="d07c0-382">La `global::` qualificazione non è supportata.</span><span class="sxs-lookup"><span data-stu-id="d07c0-382">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="d07c0-383">L'importazione di componenti con `using` istruzioni con alias (ad `@using Foo = Bar`esempio,) non è supportata.</span><span class="sxs-lookup"><span data-stu-id="d07c0-383">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="d07c0-384">I nomi parzialmente qualificati non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="d07c0-384">Partially qualified names aren't supported.</span></span> <span data-ttu-id="d07c0-385">Ad esempio, l' `@using ComponentsSample` aggiunta e `NavMenu.razor` il `<Shared.NavMenu></Shared.NavMenu>` riferimento a non sono supportate.</span><span class="sxs-lookup"><span data-stu-id="d07c0-385">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="d07c0-386">Attributi dell'elemento HTML condizionale</span><span class="sxs-lookup"><span data-stu-id="d07c0-386">Conditional HTML element attributes</span></span>

<span data-ttu-id="d07c0-387">Gli attributi degli elementi HTML vengono sottoposti a rendering in modo condizionale in base al valore .NET.</span><span class="sxs-lookup"><span data-stu-id="d07c0-387">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="d07c0-388">Se il valore è `false` o `null`, l'attributo non viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="d07c0-388">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="d07c0-389">Se il valore è `true`, l'attributo viene sottoposto a rendering ridotto a icona.</span><span class="sxs-lookup"><span data-stu-id="d07c0-389">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="d07c0-390">Nell'esempio seguente, `IsCompleted` determina se `checked` viene eseguito il rendering nel markup dell'elemento:</span><span class="sxs-lookup"><span data-stu-id="d07c0-390">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="d07c0-391">Se `IsCompleted` è`true`, viene eseguito il rendering della casella di controllo come segue:</span><span class="sxs-lookup"><span data-stu-id="d07c0-391">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="d07c0-392">Se `IsCompleted` è`false`, viene eseguito il rendering della casella di controllo come segue:</span><span class="sxs-lookup"><span data-stu-id="d07c0-392">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="d07c0-393">Per altre informazioni, vedere <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="d07c0-393">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="d07c0-394">HTML non elaborato</span><span class="sxs-lookup"><span data-stu-id="d07c0-394">Raw HTML</span></span>

<span data-ttu-id="d07c0-395">Le stringhe vengono in genere sottoposte a rendering usando i nodi di testo DOM, il che significa che qualsiasi markup che può contenere viene ignorato e considerato come testo letterale.</span><span class="sxs-lookup"><span data-stu-id="d07c0-395">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="d07c0-396">Per eseguire il rendering del codice HTML non elaborato, eseguire `MarkupString` il wrapping del contenuto HTML in un valore.</span><span class="sxs-lookup"><span data-stu-id="d07c0-396">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="d07c0-397">Il valore viene analizzato in formato HTML o SVG e inserito nel DOM.</span><span class="sxs-lookup"><span data-stu-id="d07c0-397">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="d07c0-398">Il rendering di codice HTML non elaborato costruito da un'origine non attendibile costituisce un rischio per la **sicurezza** e deve essere evitato.</span><span class="sxs-lookup"><span data-stu-id="d07c0-398">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="d07c0-399">Nell'esempio seguente viene illustrato l' `MarkupString` utilizzo del tipo per aggiungere un blocco di contenuto HTML statico all'output sottoposto a rendering di un componente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-399">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="d07c0-400">Componenti basati su modelli</span><span class="sxs-lookup"><span data-stu-id="d07c0-400">Templated components</span></span>

<span data-ttu-id="d07c0-401">I componenti basati su modelli sono componenti che accettano uno o più modelli di interfaccia utente come parametri, che possono quindi essere usati come parte della logica di rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-401">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="d07c0-402">I componenti basati su modelli consentono di creare componenti di livello superiore più riutilizzabili rispetto ai componenti normali.</span><span class="sxs-lookup"><span data-stu-id="d07c0-402">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="d07c0-403">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="d07c0-403">A couple of examples include:</span></span>

* <span data-ttu-id="d07c0-404">Componente della tabella che consente a un utente di specificare i modelli per l'intestazione, le righe e il piè di pagina della tabella.</span><span class="sxs-lookup"><span data-stu-id="d07c0-404">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="d07c0-405">Componente di elenco che consente a un utente di specificare un modello per il rendering degli elementi in un elenco.</span><span class="sxs-lookup"><span data-stu-id="d07c0-405">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="d07c0-406">Parametri di modelli</span><span class="sxs-lookup"><span data-stu-id="d07c0-406">Template parameters</span></span>

<span data-ttu-id="d07c0-407">Un componente basato su modelli viene definito specificando uno o più parametri del componente `RenderFragment` di `RenderFragment<T>`tipo o.</span><span class="sxs-lookup"><span data-stu-id="d07c0-407">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="d07c0-408">Un frammento di rendering rappresenta un segmento di interfaccia utente di cui eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="d07c0-408">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="d07c0-409">`RenderFragment<T>`accetta un parametro di tipo che può essere specificato quando viene richiamato il frammento di rendering.</span><span class="sxs-lookup"><span data-stu-id="d07c0-409">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="d07c0-410">`TableTemplate`componente</span><span class="sxs-lookup"><span data-stu-id="d07c0-410">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="d07c0-411">Quando si usa un componente basato su modelli, i parametri del modello possono essere specificati usando gli elementi figlio che corrispondono ai nomi`TableHeader` dei `RowTemplate` parametri (e nell'esempio seguente):</span><span class="sxs-lookup"><span data-stu-id="d07c0-411">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
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

### <a name="template-context-parameters"></a><span data-ttu-id="d07c0-412">Parametri di contesto del modello</span><span class="sxs-lookup"><span data-stu-id="d07c0-412">Template context parameters</span></span>

<span data-ttu-id="d07c0-413">Gli argomenti del componente `RenderFragment<T>` di tipo passati come elementi hanno un parametro `context` implicito denominato (ad esempio, nell'esempio `@context.PetId`di codice precedente,), ma è possibile modificare il `Context` nome del parametro usando l'attributo nell'elemento figlio elemento.</span><span class="sxs-lookup"><span data-stu-id="d07c0-413">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="d07c0-414">Nell'esempio seguente, l' `RowTemplate` `Context` attributo dell'elemento specifica il `pet` parametro:</span><span class="sxs-lookup"><span data-stu-id="d07c0-414">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
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

<span data-ttu-id="d07c0-415">In alternativa, è possibile specificare l' `Context` attributo sull'elemento Component.</span><span class="sxs-lookup"><span data-stu-id="d07c0-415">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="d07c0-416">L'attributo `Context` specificato si applica a tutti i parametri di modello specificati.</span><span class="sxs-lookup"><span data-stu-id="d07c0-416">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="d07c0-417">Questa operazione può essere utile quando si desidera specificare il nome del parametro di contenuto per il contenuto figlio implicito (senza alcun elemento figlio di wrapping).</span><span class="sxs-lookup"><span data-stu-id="d07c0-417">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="d07c0-418">Nell'esempio seguente l' `Context` attributo viene visualizzato `TableTemplate` nell'elemento e si applica a tutti i parametri del modello:</span><span class="sxs-lookup"><span data-stu-id="d07c0-418">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
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

### <a name="generic-typed-components"></a><span data-ttu-id="d07c0-419">Componenti tipizzati in modo generico</span><span class="sxs-lookup"><span data-stu-id="d07c0-419">Generic-typed components</span></span>

<span data-ttu-id="d07c0-420">I componenti basati su modelli spesso sono tipizzati in modo generico.</span><span class="sxs-lookup"><span data-stu-id="d07c0-420">Templated components are often generically typed.</span></span> <span data-ttu-id="d07c0-421">È ad esempio possibile utilizzare `ListViewTemplate` un componente generico per eseguire il `IEnumerable<T>` rendering dei valori.</span><span class="sxs-lookup"><span data-stu-id="d07c0-421">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="d07c0-422">Per definire un componente generico, usare la `@typeparam` direttiva per specificare i parametri di tipo:</span><span class="sxs-lookup"><span data-stu-id="d07c0-422">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="d07c0-423">Quando si usano componenti tipizzati generici, il parametro di tipo viene dedotto, se possibile:</span><span class="sxs-lookup"><span data-stu-id="d07c0-423">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="d07c0-424">In caso contrario, il parametro di tipo deve essere specificato in modo esplicito utilizzando un attributo che corrisponde al nome del parametro di tipo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-424">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="d07c0-425">Nell'esempio seguente viene `TItem="Pet"` specificato il tipo:</span><span class="sxs-lookup"><span data-stu-id="d07c0-425">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="d07c0-426">Parametri e valori di propagazione</span><span class="sxs-lookup"><span data-stu-id="d07c0-426">Cascading values and parameters</span></span>

<span data-ttu-id="d07c0-427">In alcuni scenari, non è pratico eseguire il flusso dei dati da un componente predecessore a un componente discendente usando i [parametri del componente](#component-parameters), soprattutto quando sono presenti diversi livelli di componenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-427">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="d07c0-428">I valori e i parametri di propagazione consentono di risolvere questo problema fornendo un modo pratico per un componente predecessore per fornire un valore a tutti i relativi componenti discendenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-428">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="d07c0-429">I parametri e i valori di propagazione forniscono anche un approccio per la coordinazione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-429">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="d07c0-430">Esempio di tema</span><span class="sxs-lookup"><span data-stu-id="d07c0-430">Theme example</span></span>

<span data-ttu-id="d07c0-431">Nell'esempio seguente dall'app di esempio, la `ThemeInfo` classe specifica le informazioni sul tema per scorrere la gerarchia dei componenti in modo che tutti i pulsanti all'interno di una determinata parte dell'app condividono lo stesso stile.</span><span class="sxs-lookup"><span data-stu-id="d07c0-431">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="d07c0-432">*UIThemeClasses/themeinfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="d07c0-432">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="d07c0-433">Un componente predecessore può fornire un valore di propagazione utilizzando il componente valore di propagazione.</span><span class="sxs-lookup"><span data-stu-id="d07c0-433">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="d07c0-434">Il `CascadingValue` componente esegue il wrapping di un sottoalbero della gerarchia dei componenti e fornisce un singolo valore a tutti i componenti all'interno di tale sottoalbero.</span><span class="sxs-lookup"><span data-stu-id="d07c0-434">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="d07c0-435">Ad esempio, l'app di esempio specifica le informazioni`ThemeInfo`sul tema () in uno dei layout dell'app come parametro di propagazione per tutti i componenti che costituiscono il corpo `@Body` del layout della proprietà.</span><span class="sxs-lookup"><span data-stu-id="d07c0-435">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="d07c0-436">`ButtonClass`viene assegnato un valore di `btn-success` nel componente layout.</span><span class="sxs-lookup"><span data-stu-id="d07c0-436">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="d07c0-437">Qualsiasi componente discendente può utilizzare questa proprietà tramite `ThemeInfo` l'oggetto a cascata.</span><span class="sxs-lookup"><span data-stu-id="d07c0-437">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="d07c0-438">`CascadingValuesParametersLayout`componente</span><span class="sxs-lookup"><span data-stu-id="d07c0-438">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
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

<span data-ttu-id="d07c0-439">Per usare i valori di propagazione, i componenti dichiarano i `[CascadingParameter]` parametri di propagazione usando l'attributo o in base a un valore di nome stringa:</span><span class="sxs-lookup"><span data-stu-id="d07c0-439">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="d07c0-440">Il binding con un valore di nome stringa è pertinente se si dispone di più valori di propagazione dello stesso tipo ed è necessario distinguerli nello stesso sottoalbero.</span><span class="sxs-lookup"><span data-stu-id="d07c0-440">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="d07c0-441">I valori a cascata vengono associati ai parametri di propagazione per tipo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-441">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="d07c0-442">Nell'app di esempio, il `CascadingValuesParametersTheme` componente associa il `ThemeInfo` valore a cascata a un parametro di propagazione.</span><span class="sxs-lookup"><span data-stu-id="d07c0-442">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="d07c0-443">Il parametro viene usato per impostare la classe CSS per uno dei pulsanti visualizzati dal componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-443">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="d07c0-444">`CascadingValuesParametersTheme`componente</span><span class="sxs-lookup"><span data-stu-id="d07c0-444">`CascadingValuesParametersTheme` component:</span></span>

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

### <a name="tabset-example"></a><span data-ttu-id="d07c0-445">Esempio di tabulazione</span><span class="sxs-lookup"><span data-stu-id="d07c0-445">TabSet example</span></span>

<span data-ttu-id="d07c0-446">I parametri di propagazione consentono inoltre ai componenti di collaborare attraverso la gerarchia dei componenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-446">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="d07c0-447">Si consideri, ad esempio, l'esempio di tabulazione seguente nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d07c0-447">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="d07c0-448">L'app di esempio ha `ITab` un'interfaccia implementata dalle schede:</span><span class="sxs-lookup"><span data-stu-id="d07c0-448">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="d07c0-449">Il `CascadingValuesParametersTabSet` componente usa il `TabSet` componente, che contiene diversi `Tab` componenti:</span><span class="sxs-lookup"><span data-stu-id="d07c0-449">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="d07c0-450">I componenti `Tab` figlio non vengono passati in modo esplicito come `TabSet`parametri a.</span><span class="sxs-lookup"><span data-stu-id="d07c0-450">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="d07c0-451">Al contrario, i `Tab` componenti figlio fanno parte del contenuto figlio `TabSet`di.</span><span class="sxs-lookup"><span data-stu-id="d07c0-451">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="d07c0-452">Tuttavia, `TabSet` è comunque necessario conoscere ogni `Tab` componente in modo che sia in grado di eseguire il rendering delle intestazioni e della scheda attiva. Per abilitare questo coordinamento senza richiedere codice aggiuntivo, il `TabSet` componente *può fornire se stesso come valore* di propagazione che viene quindi prelevato dai `Tab` componenti discendenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-452">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="d07c0-453">`TabSet`componente</span><span class="sxs-lookup"><span data-stu-id="d07c0-453">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="d07c0-454">I componenti `Tab` discendenti acquisiscono `TabSet` l'oggetto che contiene come parametro di propagazione, in modo `TabSet` che i `Tab` componenti si aggiungano alla coordinata e della scheda attiva.</span><span class="sxs-lookup"><span data-stu-id="d07c0-454">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="d07c0-455">`Tab`componente</span><span class="sxs-lookup"><span data-stu-id="d07c0-455">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="d07c0-456">Modelli Razor</span><span class="sxs-lookup"><span data-stu-id="d07c0-456">Razor templates</span></span>

<span data-ttu-id="d07c0-457">I frammenti di rendering possono essere definiti usando la sintassi del modello Razor.</span><span class="sxs-lookup"><span data-stu-id="d07c0-457">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="d07c0-458">I modelli Razor sono un modo per definire un frammento di interfaccia utente e presupporre il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-458">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="d07c0-459">Nell'esempio seguente viene illustrato come specificare `RenderFragment` i valori e `RenderFragment<T>` ed eseguire il rendering dei modelli direttamente in un componente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-459">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="d07c0-460">I frammenti di rendering possono anche essere passati come argomenti ai [componenti basati su modelli](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="d07c0-460">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="d07c0-461">Output del codice precedente sottoposto a rendering:</span><span class="sxs-lookup"><span data-stu-id="d07c0-461">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="d07c0-462">Logica RenderTreeBuilder manuale</span><span class="sxs-lookup"><span data-stu-id="d07c0-462">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="d07c0-463">`Microsoft.AspNetCore.Components.RenderTree`fornisce metodi per la modifica di componenti ed elementi, inclusa la compilazione manuale C# dei componenti nel codice.</span><span class="sxs-lookup"><span data-stu-id="d07c0-463">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="d07c0-464">L'utilizzo `RenderTreeBuilder` di per la creazione di componenti è uno scenario avanzato.</span><span class="sxs-lookup"><span data-stu-id="d07c0-464">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="d07c0-465">Un componente con formato non valido, ad esempio un tag di markup non chiuso, può causare un comportamento indefinito.</span><span class="sxs-lookup"><span data-stu-id="d07c0-465">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="d07c0-466">Si consideri il componente seguente `PetDetails` , che può essere incorporato manualmente in un altro componente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-466">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    private string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="d07c0-467">Nell'esempio seguente il ciclo nel `CreateComponent` metodo genera tre `PetDetails` componenti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-467">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="d07c0-468">Quando si `RenderTreeBuilder` chiamano i metodi per creare i`OpenComponent` componenti `AddAttribute`(e), i numeri di sequenza sono numeri di riga del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-468">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="d07c0-469">L'algoritmo di differenza Blaze si basa sui numeri di sequenza corrispondenti a righe di codice distinte, non sulle chiamate di chiamata distinti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-469">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="d07c0-470">Quando si crea un componente `RenderTreeBuilder` con i metodi, impostare come hardcoded gli argomenti per i numeri di sequenza.</span><span class="sxs-lookup"><span data-stu-id="d07c0-470">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="d07c0-471">**L'utilizzo di un calcolo o di un contatore per generare il numero di sequenza può causare un calo delle prestazioni.**</span><span class="sxs-lookup"><span data-stu-id="d07c0-471">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="d07c0-472">Per ulteriori informazioni, vedere la sezione [numeri di sequenza correlati ai numeri di riga del codice e non all'ordine di esecuzione](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="d07c0-472">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="d07c0-473">`BuiltContent`componente</span><span class="sxs-lookup"><span data-stu-id="d07c0-473">`BuiltContent` component:</span></span>

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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="d07c0-474">I numeri di sequenza sono correlati ai numeri di riga del codice e non all'ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="d07c0-474">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="d07c0-475">`.razor` I file Blazer vengono sempre compilati.</span><span class="sxs-lookup"><span data-stu-id="d07c0-475">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="d07c0-476">Questo è potenzialmente un ottimo vantaggio per `.razor` perché il passaggio di compilazione può essere usato per inserire informazioni che migliorano le prestazioni dell'app in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d07c0-476">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="d07c0-477">Un esempio fondamentale di questi miglioramenti riguarda i *numeri di sequenza*.</span><span class="sxs-lookup"><span data-stu-id="d07c0-477">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="d07c0-478">I numeri di sequenza indicano al runtime quali output provengono da righe di codice distinte e ordinate.</span><span class="sxs-lookup"><span data-stu-id="d07c0-478">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="d07c0-479">Il runtime usa queste informazioni per generare differenze di albero efficienti nel tempo lineare, che è molto più veloce rispetto a quanto normalmente è possibile per un algoritmo diff della struttura ad albero generale.</span><span class="sxs-lookup"><span data-stu-id="d07c0-479">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="d07c0-480">Si consideri `.razor` il seguente file semplice:</span><span class="sxs-lookup"><span data-stu-id="d07c0-480">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="d07c0-481">Il codice precedente viene compilato in un modo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-481">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="d07c0-482">Quando il codice viene eseguito per la prima volta, se `someFlag` è `true`, il generatore riceve:</span><span class="sxs-lookup"><span data-stu-id="d07c0-482">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="d07c0-483">Sequenza</span><span class="sxs-lookup"><span data-stu-id="d07c0-483">Sequence</span></span> | <span data-ttu-id="d07c0-484">Type</span><span class="sxs-lookup"><span data-stu-id="d07c0-484">Type</span></span>      | <span data-ttu-id="d07c0-485">Data</span><span class="sxs-lookup"><span data-stu-id="d07c0-485">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="d07c0-486">0</span><span class="sxs-lookup"><span data-stu-id="d07c0-486">0</span></span>        | <span data-ttu-id="d07c0-487">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="d07c0-487">Text node</span></span> | <span data-ttu-id="d07c0-488">Primo</span><span class="sxs-lookup"><span data-stu-id="d07c0-488">First</span></span>  |
| <span data-ttu-id="d07c0-489">1</span><span class="sxs-lookup"><span data-stu-id="d07c0-489">1</span></span>        | <span data-ttu-id="d07c0-490">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="d07c0-490">Text node</span></span> | <span data-ttu-id="d07c0-491">Secondo</span><span class="sxs-lookup"><span data-stu-id="d07c0-491">Second</span></span> |

<span data-ttu-id="d07c0-492">Si supponga `someFlag` che `false`diventi e che venga eseguito nuovamente il rendering del markup.</span><span class="sxs-lookup"><span data-stu-id="d07c0-492">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="d07c0-493">Questa volta, il generatore riceve:</span><span class="sxs-lookup"><span data-stu-id="d07c0-493">This time, the builder receives:</span></span>

| <span data-ttu-id="d07c0-494">Sequenza</span><span class="sxs-lookup"><span data-stu-id="d07c0-494">Sequence</span></span> | <span data-ttu-id="d07c0-495">Type</span><span class="sxs-lookup"><span data-stu-id="d07c0-495">Type</span></span>       | <span data-ttu-id="d07c0-496">Data</span><span class="sxs-lookup"><span data-stu-id="d07c0-496">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="d07c0-497">1</span><span class="sxs-lookup"><span data-stu-id="d07c0-497">1</span></span>        | <span data-ttu-id="d07c0-498">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="d07c0-498">Text node</span></span>  | <span data-ttu-id="d07c0-499">Secondo</span><span class="sxs-lookup"><span data-stu-id="d07c0-499">Second</span></span> |

<span data-ttu-id="d07c0-500">Quando il runtime esegue una diff, rileva che l'elemento in sequenza `0` è stato rimosso, quindi genera lo script di *modifica*semplice seguente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-500">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="d07c0-501">Rimuovere il primo nodo di testo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-501">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="d07c0-502">Cosa accade se si generano numeri di sequenza a livello di codice</span><span class="sxs-lookup"><span data-stu-id="d07c0-502">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="d07c0-503">Si supponga invece che sia stata scritta la logica del generatore di albero di rendering seguente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-503">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="d07c0-504">A questo punto, il primo output è:</span><span class="sxs-lookup"><span data-stu-id="d07c0-504">Now, the first output is:</span></span>

| <span data-ttu-id="d07c0-505">Sequenza</span><span class="sxs-lookup"><span data-stu-id="d07c0-505">Sequence</span></span> | <span data-ttu-id="d07c0-506">Type</span><span class="sxs-lookup"><span data-stu-id="d07c0-506">Type</span></span>      | <span data-ttu-id="d07c0-507">Data</span><span class="sxs-lookup"><span data-stu-id="d07c0-507">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="d07c0-508">0</span><span class="sxs-lookup"><span data-stu-id="d07c0-508">0</span></span>        | <span data-ttu-id="d07c0-509">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="d07c0-509">Text node</span></span> | <span data-ttu-id="d07c0-510">Primo</span><span class="sxs-lookup"><span data-stu-id="d07c0-510">First</span></span>  |
| <span data-ttu-id="d07c0-511">1</span><span class="sxs-lookup"><span data-stu-id="d07c0-511">1</span></span>        | <span data-ttu-id="d07c0-512">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="d07c0-512">Text node</span></span> | <span data-ttu-id="d07c0-513">Secondo</span><span class="sxs-lookup"><span data-stu-id="d07c0-513">Second</span></span> |

<span data-ttu-id="d07c0-514">Questo risultato è identico al caso precedente, pertanto non esistono problemi negativi.</span><span class="sxs-lookup"><span data-stu-id="d07c0-514">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="d07c0-515">`someFlag`si `false` trova nel secondo rendering e l'output è:</span><span class="sxs-lookup"><span data-stu-id="d07c0-515">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="d07c0-516">Sequenza</span><span class="sxs-lookup"><span data-stu-id="d07c0-516">Sequence</span></span> | <span data-ttu-id="d07c0-517">Type</span><span class="sxs-lookup"><span data-stu-id="d07c0-517">Type</span></span>      | <span data-ttu-id="d07c0-518">Data</span><span class="sxs-lookup"><span data-stu-id="d07c0-518">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="d07c0-519">0</span><span class="sxs-lookup"><span data-stu-id="d07c0-519">0</span></span>        | <span data-ttu-id="d07c0-520">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="d07c0-520">Text node</span></span> | <span data-ttu-id="d07c0-521">Secondo</span><span class="sxs-lookup"><span data-stu-id="d07c0-521">Second</span></span> |

<span data-ttu-id="d07c0-522">Questa volta, l'algoritmo Diff rileva che si sono verificate *due* modifiche e l'algoritmo genera lo script di modifica seguente:</span><span class="sxs-lookup"><span data-stu-id="d07c0-522">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="d07c0-523">Modificare il valore del primo nodo di testo in `Second`.</span><span class="sxs-lookup"><span data-stu-id="d07c0-523">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="d07c0-524">Rimuovere il secondo nodo di testo.</span><span class="sxs-lookup"><span data-stu-id="d07c0-524">Remove the second text node.</span></span>

<span data-ttu-id="d07c0-525">La generazione dei numeri di sequenza ha perso tutte le informazioni utili sul `if/else` punto in cui i rami e i cicli erano presenti nel codice originale.</span><span class="sxs-lookup"><span data-stu-id="d07c0-525">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="d07c0-526">In questo modo si ottiene una differenza **due volte più a lungo** .</span><span class="sxs-lookup"><span data-stu-id="d07c0-526">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="d07c0-527">Questo è un esempio semplice.</span><span class="sxs-lookup"><span data-stu-id="d07c0-527">This is a trivial example.</span></span> <span data-ttu-id="d07c0-528">Nei casi più realistici con strutture complesse e profondamente annidate e soprattutto con i cicli, il costo delle prestazioni è più grave.</span><span class="sxs-lookup"><span data-stu-id="d07c0-528">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="d07c0-529">Invece di identificare immediatamente i blocchi o i rami del ciclo che sono stati inseriti o rimossi, l'algoritmo Diff deve ripresentarsi in modo approfondito negli alberi di rendering e, in genere, compilare script di modifica molto più lunghi perché non è stato informato in modo indesiderato sul modo in cui le nuove strutture relazione tra loro.</span><span class="sxs-lookup"><span data-stu-id="d07c0-529">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="d07c0-530">Linee guida e conclusioni</span><span class="sxs-lookup"><span data-stu-id="d07c0-530">Guidance and conclusions</span></span>

* <span data-ttu-id="d07c0-531">Le prestazioni dell'app soffrono se i numeri di sequenza vengono generati dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-531">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="d07c0-532">Il Framework non è in grado di creare automaticamente i propri numeri di sequenza in fase di esecuzione perché le informazioni necessarie non esistono a meno che non vengano acquisite in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="d07c0-532">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="d07c0-533">Non scrivere blocchi lunghi di logica implementata `RenderTreeBuilder` manualmente.</span><span class="sxs-lookup"><span data-stu-id="d07c0-533">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="d07c0-534">Preferire `.razor` i file e consentire al compilatore di gestire i numeri di sequenza.</span><span class="sxs-lookup"><span data-stu-id="d07c0-534">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="d07c0-535">Se i numeri di sequenza sono hardcoded, l'algoritmo Diff richiede solo che i numeri di sequenza aumentino nel valore.</span><span class="sxs-lookup"><span data-stu-id="d07c0-535">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="d07c0-536">Il valore iniziale e i gap sono irrilevanti.</span><span class="sxs-lookup"><span data-stu-id="d07c0-536">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="d07c0-537">Una delle opzioni legittime consiste nell'usare il numero di riga del codice come numero di sequenza oppure iniziare da zero e aumentare di uno o di centinaia (o qualsiasi intervallo preferito).</span><span class="sxs-lookup"><span data-stu-id="d07c0-537">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="d07c0-538">Blazer usa i numeri di sequenza, mentre altri Framework dell'interfaccia utente con differenze tra gli alberi non li usano.</span><span class="sxs-lookup"><span data-stu-id="d07c0-538">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="d07c0-539">La diffing è molto più veloce quando si usano i numeri di sequenza e Blazer ha il vantaggio di un passaggio di compilazione che riguarda automaticamente i numeri di `.razor` sequenza per gli sviluppatori che creano file.</span><span class="sxs-lookup"><span data-stu-id="d07c0-539">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>
