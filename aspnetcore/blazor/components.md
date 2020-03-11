---
title: Creare e usare ASP.NET Core componenti Razor
author: guardrex
description: Informazioni su come creare e usare i componenti Razor, tra cui la modalità di associazione ai dati, la gestione degli eventi e la gestione dei cicli di vita dei componenti.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: e444ebfef5143a6c33ed2d122933903ad3a4f4a7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660699"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="2ef07-103">Creare e usare ASP.NET Core componenti Razor</span><span class="sxs-lookup"><span data-stu-id="2ef07-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="2ef07-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="2ef07-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="2ef07-105">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ef07-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="2ef07-106"> le app vengono compilate usando i *componenti*.</span><span class="sxs-lookup"><span data-stu-id="2ef07-106"> apps are built using *components*.</span></span> <span data-ttu-id="2ef07-107">Un componente è un blocco di interfaccia utente (UI) autonomo, ad esempio una pagina, una finestra di dialogo o un form.</span><span class="sxs-lookup"><span data-stu-id="2ef07-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="2ef07-108">Un componente include il markup HTML e la logica di elaborazione necessaria per inserire i dati o rispondere agli eventi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="2ef07-109">I componenti sono flessibili e leggeri.</span><span class="sxs-lookup"><span data-stu-id="2ef07-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="2ef07-110">Possono essere annidati, riutilizzati e condivisi tra i progetti.</span><span class="sxs-lookup"><span data-stu-id="2ef07-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="2ef07-111">Classi di componenti</span><span class="sxs-lookup"><span data-stu-id="2ef07-111">Component classes</span></span>

<span data-ttu-id="2ef07-112">I componenti sono implementati in file di componente [Razor](xref:mvc/views/razor) (*Razor*) usando una C# combinazione di e markup HTML.</span><span class="sxs-lookup"><span data-stu-id="2ef07-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="2ef07-113">Un componente in Blazor viene definito formalmente come *componente Razor*.</span><span class="sxs-lookup"><span data-stu-id="2ef07-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="2ef07-114">Il nome di un componente deve iniziare con un carattere maiuscolo.</span><span class="sxs-lookup"><span data-stu-id="2ef07-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="2ef07-115">Ad esempio, *MyCoolComponent. Razor* è valido e *MyCoolComponent. Razor* non è valido.</span><span class="sxs-lookup"><span data-stu-id="2ef07-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="2ef07-116">L'interfaccia utente per un componente viene definita utilizzando HTML.</span><span class="sxs-lookup"><span data-stu-id="2ef07-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="2ef07-117">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="2ef07-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="2ef07-118">Quando viene compilata un'app, il markup C# HTML e la logica di rendering vengono convertiti in una classe Component.</span><span class="sxs-lookup"><span data-stu-id="2ef07-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="2ef07-119">Il nome della classe generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="2ef07-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="2ef07-120">I membri della classe del componente vengono definiti in un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="2ef07-121">Nel blocco `@code`, lo stato del componente (proprietà, campi) viene specificato con i metodi per la gestione degli eventi o per la definizione di altre logiche dei componenti.</span><span class="sxs-lookup"><span data-stu-id="2ef07-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="2ef07-122">È consentito più di un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="2ef07-123">I membri dei componenti possono essere usati come parte della logica di rendering del C# componente usando espressioni che iniziano con `@`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="2ef07-124">Un C# campo, ad esempio, viene sottoposto a rendering anteponendo `@` al nome del campo.</span><span class="sxs-lookup"><span data-stu-id="2ef07-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="2ef07-125">Nell'esempio seguente viene valutato ed eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="2ef07-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="2ef07-126">`_headingFontStyle` al valore della proprietà CSS per `font-style`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="2ef07-127">`_headingText` al contenuto dell'elemento `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="2ef07-128">Una volta eseguito il rendering iniziale del componente, il componente rigenera l'albero di rendering in risposta agli eventi.</span><span class="sxs-lookup"><span data-stu-id="2ef07-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="2ef07-129"> confronta quindi il nuovo albero di rendering con quello precedente e applica le modifiche apportate al Document Object Model (DOM) del browser.</span><span class="sxs-lookup"><span data-stu-id="2ef07-129"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="2ef07-130">I componenti sono C# classi ordinarie e possono essere inseriti in qualsiasi punto all'interno di un progetto.</span><span class="sxs-lookup"><span data-stu-id="2ef07-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="2ef07-131">I componenti che producono pagine Web in genere risiedono nella cartella *pages* .</span><span class="sxs-lookup"><span data-stu-id="2ef07-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="2ef07-132">I componenti non di pagina vengono spesso inseriti nella cartella *condivisa* o in una cartella personalizzata aggiunta al progetto.</span><span class="sxs-lookup"><span data-stu-id="2ef07-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="2ef07-133">Lo spazio dei nomi di un componente viene in genere derivato dallo spazio dei nomi radice dell'app e dal percorso (cartella) del componente all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="2ef07-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="2ef07-134">Se lo spazio dei nomi radice dell'app è `BlazorApp` e il componente `Counter` si trova nella cartella *pages* :</span><span class="sxs-lookup"><span data-stu-id="2ef07-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="2ef07-135">Lo spazio dei nomi del componente `Counter` è `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="2ef07-136">Il nome completo del tipo del componente è `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="2ef07-137">Per ulteriori informazioni, vedere la sezione [Import Components](#import-components) .</span><span class="sxs-lookup"><span data-stu-id="2ef07-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="2ef07-138">Per usare una cartella personalizzata, aggiungere lo spazio dei nomi della cartella personalizzata al componente padre o al file *_Imports. Razor* dell'app.</span><span class="sxs-lookup"><span data-stu-id="2ef07-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="2ef07-139">Lo spazio dei nomi seguente, ad esempio, rende disponibili i componenti in una cartella di *componenti* quando lo spazio dei nomi radice dell'app è `BlazorApp`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a><span data-ttu-id="2ef07-140">Asset statici</span><span class="sxs-lookup"><span data-stu-id="2ef07-140">Static assets</span></span>

Blazor<span data-ttu-id="2ef07-141"> segue la convenzione di ASP.NET Core app che collocano asset statici nella [cartella radice Web del progetto (wwwroot)](xref:fundamentals/index#web-root).</span><span class="sxs-lookup"><span data-stu-id="2ef07-141"> follows the convention of ASP.NET Core apps placing static assets under the project's [web root (wwwroot) folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="2ef07-142">Usare un percorso relativo di base (`/`) per fare riferimento alla radice Web per un asset statico.</span><span class="sxs-lookup"><span data-stu-id="2ef07-142">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="2ef07-143">Nell'esempio seguente *logo. png* si trova fisicamente nella cartella *{Project root}/wwwroot/images* :</span><span class="sxs-lookup"><span data-stu-id="2ef07-143">In the following example, *logo.png* is physically located in the *{PROJECT ROOT}/wwwroot/images* folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

<span data-ttu-id="2ef07-144">I componenti Razor **non** supportano la notazione tilde-barra (`~/`).</span><span class="sxs-lookup"><span data-stu-id="2ef07-144">Razor components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="2ef07-145">Per informazioni sull'impostazione del percorso di base di un'app, vedere <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="2ef07-145">For information on setting an app's base path, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="2ef07-146">Gli helper tag non sono supportati nei componenti</span><span class="sxs-lookup"><span data-stu-id="2ef07-146">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="2ef07-147">Gli [Helper Tag](xref:mvc/views/tag-helpers/intro) non sono supportati nei componenti Razor (file*Razor* ).</span><span class="sxs-lookup"><span data-stu-id="2ef07-147">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="2ef07-148">Per fornire funzionalità di tipo Helper tag in Blazor, creare un componente con le stesse funzionalità dell'helper tag e usare invece il componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-148">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="2ef07-149">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="2ef07-149">Use components</span></span>

<span data-ttu-id="2ef07-150">I componenti possono includere altri componenti dichiarando questi ultimi usando la sintassi degli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="2ef07-150">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="2ef07-151">Il markup per l'uso di un componente è simile a un tag HTML, in cui il nome del tag è il tipo di componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-151">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="2ef07-152">L'associazione di attributi distingue tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="2ef07-152">Attribute binding is case sensitive.</span></span> <span data-ttu-id="2ef07-153">Ad esempio, `@bind` è valido e `@Bind` non è valido.</span><span class="sxs-lookup"><span data-stu-id="2ef07-153">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="2ef07-154">Il markup seguente in *index. Razor* esegue il rendering di un'istanza di `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-154">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="2ef07-155">*Components/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-155">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="2ef07-156">Se un componente contiene un elemento HTML con una prima lettera maiuscola che non corrisponde a un nome di componente, viene emesso un avviso che indica che l'elemento ha un nome imprevisto.</span><span class="sxs-lookup"><span data-stu-id="2ef07-156">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="2ef07-157">L'aggiunta di una direttiva `@using` per lo spazio dei nomi del componente rende disponibile il componente, che risolve l'avviso.</span><span class="sxs-lookup"><span data-stu-id="2ef07-157">Adding an `@using` directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="routing"></a><span data-ttu-id="2ef07-158">Routing.</span><span class="sxs-lookup"><span data-stu-id="2ef07-158">Routing</span></span>

<span data-ttu-id="2ef07-159">Il routing in Blazor viene effettuato fornendo un modello di route a ogni componente accessibile nell'app.</span><span class="sxs-lookup"><span data-stu-id="2ef07-159">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="2ef07-160">Quando viene compilato un file Razor con una direttiva `@page`, alla classe generata viene assegnato un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specificando il modello di route.</span><span class="sxs-lookup"><span data-stu-id="2ef07-160">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="2ef07-161">In fase di esecuzione, il router cerca le classi di componenti con una `RouteAttribute` ed esegue il rendering di qualsiasi componente con un modello di route corrispondente all'URL richiesto.</span><span class="sxs-lookup"><span data-stu-id="2ef07-161">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

```razor
@page "/ParentComponent"

...
```

<span data-ttu-id="2ef07-162">Per altre informazioni, vedere <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="2ef07-162">For more information, see <xref:blazor/routing>.</span></span>

## <a name="parameters"></a><span data-ttu-id="2ef07-163">Parametri</span><span class="sxs-lookup"><span data-stu-id="2ef07-163">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="2ef07-164">Parametri di route</span><span class="sxs-lookup"><span data-stu-id="2ef07-164">Route parameters</span></span>

<span data-ttu-id="2ef07-165">I componenti possono ricevere parametri di route dal modello di route fornito nella direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-165">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="2ef07-166">Il router usa parametri di route per popolare i parametri del componente corrispondente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-166">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="2ef07-167">*Pages/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-167">*Pages/RouteParameter.razor*:</span></span>

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="2ef07-168">I parametri facoltativi non sono supportati, pertanto due direttive `@page` vengono applicate nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-168">Optional parameters aren't supported, so two `@page` directives are applied in the preceding example.</span></span> <span data-ttu-id="2ef07-169">Il primo consente la navigazione al componente senza un parametro.</span><span class="sxs-lookup"><span data-stu-id="2ef07-169">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="2ef07-170">La seconda direttiva `@page` riceve il parametro di route `{text}` e assegna il valore alla proprietà `Text`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-170">The second `@page` directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="2ef07-171">La sintassi dei parametri *catch-all* (`*`/`**`), che acquisisce il percorso tra più limiti di cartella, **non** è supportata nei*componenti Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="2ef07-171">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="2ef07-172">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="2ef07-172">Component parameters</span></span>

<span data-ttu-id="2ef07-173">I componenti possono avere *parametri del componente*, che vengono definiti usando proprietà pubbliche nella classe Component con l'attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-173">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="2ef07-174">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="2ef07-174">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="2ef07-175">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-175">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="2ef07-176">Nell'esempio seguente dall'app di esempio, il `ParentComponent` imposta il valore della proprietà `Title` dell'`ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-176">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="2ef07-177">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-177">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="2ef07-178">Contenuto figlio</span><span class="sxs-lookup"><span data-stu-id="2ef07-178">Child content</span></span>

<span data-ttu-id="2ef07-179">I componenti possono impostare il contenuto di un altro componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-179">Components can set the content of another component.</span></span> <span data-ttu-id="2ef07-180">Il componente di assegnazione fornisce il contenuto tra i tag che specificano il componente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2ef07-180">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="2ef07-181">Nell'esempio seguente, il `ChildComponent` dispone di una proprietà `ChildContent` che rappresenta un `RenderFragment`, che rappresenta un segmento di interfaccia utente di cui eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="2ef07-181">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="2ef07-182">Il valore di `ChildContent` è posizionato nel markup del componente in cui deve essere eseguito il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="2ef07-182">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="2ef07-183">Il valore di `ChildContent` viene ricevuto dal componente padre e ne viene eseguito il rendering all'interno del `panel-body`del pannello bootstrap.</span><span class="sxs-lookup"><span data-stu-id="2ef07-183">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="2ef07-184">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-184">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="2ef07-185">La proprietà che riceve il contenuto di `RenderFragment` deve essere denominata `ChildContent` per convenzione.</span><span class="sxs-lookup"><span data-stu-id="2ef07-185">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="2ef07-186">Il `ParentComponent` nell'app di esempio può fornire contenuto per il rendering del `ChildComponent` inserendo il contenuto all'interno dei tag `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-186">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="2ef07-187">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-187">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="2ef07-188">Attributo splatting e parametri arbitrari</span><span class="sxs-lookup"><span data-stu-id="2ef07-188">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="2ef07-189">I componenti possono acquisire ed eseguire il rendering di attributi aggiuntivi oltre ai parametri dichiarati del componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-189">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="2ef07-190">È possibile acquisire attributi aggiuntivi in un dizionario e quindi *Splatted* su un elemento quando il componente viene sottoposto a rendering usando la direttiva Razor [`@attributes`](xref:mvc/views/razor#attributes) .</span><span class="sxs-lookup"><span data-stu-id="2ef07-190">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="2ef07-191">Questo scenario è utile quando si definisce un componente che produce un elemento di markup che supporta un'ampia gamma di personalizzazioni.</span><span class="sxs-lookup"><span data-stu-id="2ef07-191">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="2ef07-192">Ad esempio, può essere noioso definire gli attributi separatamente per un `<input>` che supporta molti parametri.</span><span class="sxs-lookup"><span data-stu-id="2ef07-192">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="2ef07-193">Nell'esempio seguente, il primo elemento `<input>` (`id="useIndividualParams"`) USA parametri dei singoli componenti, mentre il secondo elemento `<input>` (`id="useAttributesDict"`) usa l'attributo splatting:</span><span class="sxs-lookup"><span data-stu-id="2ef07-193">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
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

<span data-ttu-id="2ef07-194">Il tipo del parametro deve implementare `IEnumerable<KeyValuePair<string, object>>` con chiavi di stringa.</span><span class="sxs-lookup"><span data-stu-id="2ef07-194">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="2ef07-195">In questo scenario è inoltre possibile utilizzare `IReadOnlyDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-195">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="2ef07-196">Gli elementi `<input>` sottoposti a rendering usando entrambi gli approcci sono identici:</span><span class="sxs-lookup"><span data-stu-id="2ef07-196">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="2ef07-197">Per accettare attributi arbitrari, definire un parametro component usando l'attributo `[Parameter]` con la proprietà `CaptureUnmatchedValues` impostata su `true`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-197">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="2ef07-198">La proprietà `CaptureUnmatchedValues` in `[Parameter]` consente al parametro di trovare la corrispondenza con tutti gli attributi che non corrispondono ad altri parametri.</span><span class="sxs-lookup"><span data-stu-id="2ef07-198">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="2ef07-199">Un componente può definire un solo parametro con `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-199">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="2ef07-200">Il tipo di proprietà utilizzato con `CaptureUnmatchedValues` deve essere assegnabile da `Dictionary<string, object>` con chiavi di stringa.</span><span class="sxs-lookup"><span data-stu-id="2ef07-200">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="2ef07-201">in questo scenario `IEnumerable<KeyValuePair<string, object>>` o `IReadOnlyDictionary<string, object>` sono inoltre disponibili opzioni.</span><span class="sxs-lookup"><span data-stu-id="2ef07-201">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="2ef07-202">La posizione di `@attributes` rispetto alla posizione degli attributi degli elementi è importante.</span><span class="sxs-lookup"><span data-stu-id="2ef07-202">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="2ef07-203">Quando `@attributes` sono Splatted sull'elemento, gli attributi vengono elaborati da destra a sinistra (dall'ultimo al primo).</span><span class="sxs-lookup"><span data-stu-id="2ef07-203">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="2ef07-204">Si consideri l'esempio seguente di un componente che utilizza un componente `Child`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-204">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="2ef07-205">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-205">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="2ef07-206">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-206">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="2ef07-207">L'attributo `extra` del componente `Child` è impostato a destra di `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-207">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="2ef07-208">Il rendering del componente `Parent` `<div>` contiene `extra="5"` quando viene passato attraverso l'attributo aggiuntivo, perché gli attributi vengono elaborati da destra a sinistra (dall'ultimo al primo):</span><span class="sxs-lookup"><span data-stu-id="2ef07-208">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="2ef07-209">Nell'esempio seguente, l'ordine delle `extra` e `@attributes` viene invertito nell'`<div>`del componente `Child`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-209">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="2ef07-210">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-210">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="2ef07-211">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-211">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="2ef07-212">Il `<div>` sottoposto a rendering nel componente `Parent` contiene `extra="10"` quando viene passato tramite l'attributo aggiuntivo:</span><span class="sxs-lookup"><span data-stu-id="2ef07-212">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="2ef07-213">Acquisisci riferimenti ai componenti</span><span class="sxs-lookup"><span data-stu-id="2ef07-213">Capture references to components</span></span>

<span data-ttu-id="2ef07-214">I riferimenti ai componenti forniscono un modo per fare riferimento a un'istanza del componente in modo da poter emettere comandi per tale istanza, ad esempio `Show` o `Reset`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-214">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="2ef07-215">Per acquisire un riferimento a un componente:</span><span class="sxs-lookup"><span data-stu-id="2ef07-215">To capture a component reference:</span></span>

* <span data-ttu-id="2ef07-216">Aggiungere un attributo [`@ref`](xref:mvc/views/razor#ref) al componente figlio.</span><span class="sxs-lookup"><span data-stu-id="2ef07-216">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="2ef07-217">Definire un campo con lo stesso tipo del componente figlio.</span><span class="sxs-lookup"><span data-stu-id="2ef07-217">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

<span data-ttu-id="2ef07-218">Quando viene eseguito il rendering del componente, il campo `_loginDialog` viene popolato con l'istanza `MyLoginDialog` componente figlio.</span><span class="sxs-lookup"><span data-stu-id="2ef07-218">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="2ef07-219">È quindi possibile richiamare i metodi .NET nell'istanza del componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-219">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2ef07-220">La variabile `_loginDialog` viene popolata solo dopo il rendering del componente e l'output include l'elemento `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-220">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="2ef07-221">Fino a quel momento, non c'è niente a cui fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="2ef07-221">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="2ef07-222">Per modificare i riferimenti ai componenti dopo che il componente ha terminato il rendering, usare i [Metodi OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="2ef07-222">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="2ef07-223">Mentre l'acquisizione di riferimenti ai componenti usa una sintassi simile per l' [acquisizione di riferimenti a elementi](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), non è una funzionalità di interoperabilità di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ef07-223">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="2ef07-224">I riferimenti ai componenti non vengono passati al codice JavaScript&mdash;vengono usati solo nel codice .NET.</span><span class="sxs-lookup"><span data-stu-id="2ef07-224">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="2ef07-225">**Non** usare i riferimenti ai componenti per mutare lo stato dei componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="2ef07-225">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="2ef07-226">Usare invece i normali parametri dichiarativi per passare i dati ai componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="2ef07-226">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="2ef07-227">L'utilizzo di normali parametri dichiarativi restituisce automaticamente i componenti figlio che eseguono il rendering alle ore corrette.</span><span class="sxs-lookup"><span data-stu-id="2ef07-227">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="2ef07-228">Richiama i metodi del componente esternamente per aggiornare lo stato</span><span class="sxs-lookup"><span data-stu-id="2ef07-228">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="2ef07-229"> usa un `SynchronizationContext` per applicare un singolo thread logico di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2ef07-229"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="2ef07-230">I metodi del [ciclo](xref:blazor/lifecycle) di vita di un componente e tutti i callback di evento generati da Blazor vengono eseguiti in questa `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-230">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="2ef07-231">Nel caso in cui un componente deve essere aggiornato in base a un evento esterno, ad esempio un timer o altre notifiche, usare il metodo `InvokeAsync`, che invierà al `SynchronizationContext`di Blazor.</span><span class="sxs-lookup"><span data-stu-id="2ef07-231">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="2ef07-232">Si consideri ad esempio un *servizio Notifier* che può inviare notifiche a qualsiasi componente in ascolto dello stato aggiornato:</span><span class="sxs-lookup"><span data-stu-id="2ef07-232">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="2ef07-233">Registrare il `NotifierService` come singletion:</span><span class="sxs-lookup"><span data-stu-id="2ef07-233">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="2ef07-234">In Blazor webassembly registrare il servizio in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-234">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="2ef07-235">In Blazor server registrare il servizio in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-235">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="2ef07-236">Usare il `NotifierService` per aggiornare un componente:</span><span class="sxs-lookup"><span data-stu-id="2ef07-236">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="2ef07-237">Nell'esempio precedente `NotifierService` richiama il metodo di `OnNotify` del componente al di fuori della `SynchronizationContext`di Blazor.</span><span class="sxs-lookup"><span data-stu-id="2ef07-237">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="2ef07-238">`InvokeAsync` viene utilizzato per passare al contesto corretto e accodare un rendering.</span><span class="sxs-lookup"><span data-stu-id="2ef07-238">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="2ef07-239">Usare \@chiave per controllare la conservazione di elementi e componenti</span><span class="sxs-lookup"><span data-stu-id="2ef07-239">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="2ef07-240">Quando si esegue il rendering di un elenco di elementi o componenti e gli elementi o i componenti vengono modificati successivamente, l'algoritmo diff di Blazordeve decidere quali elementi o componenti precedenti possono essere conservati e come eseguire il mapping degli oggetti modello.</span><span class="sxs-lookup"><span data-stu-id="2ef07-240">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="2ef07-241">In genere, questo processo è automatico e può essere ignorato, ma in alcuni casi potrebbe essere necessario controllare il processo.</span><span class="sxs-lookup"><span data-stu-id="2ef07-241">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="2ef07-242">Prendere in considerazione gli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2ef07-242">Consider the following example:</span></span>

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

<span data-ttu-id="2ef07-243">Il contenuto della raccolta di `People` può cambiare con le voci inserite, eliminate o riordinate.</span><span class="sxs-lookup"><span data-stu-id="2ef07-243">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="2ef07-244">Quando viene eseguito il rendering del componente, il componente `<DetailsEditor>` può variare in modo da ricevere valori `Details` parametri diversi.</span><span class="sxs-lookup"><span data-stu-id="2ef07-244">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="2ef07-245">Questo può causare un rirendering più complesso del previsto.</span><span class="sxs-lookup"><span data-stu-id="2ef07-245">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="2ef07-246">In alcuni casi, il rirendering può comportare differenze di comportamento visibili, ad esempio lo stato attivo degli elementi persi.</span><span class="sxs-lookup"><span data-stu-id="2ef07-246">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="2ef07-247">È possibile controllare il processo di mapping con l'attributo `@key` direttiva.</span><span class="sxs-lookup"><span data-stu-id="2ef07-247">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="2ef07-248">`@key` fa in modo che l'algoritmo diffi garantisca la conservazione di elementi o componenti in base al valore della chiave:</span><span class="sxs-lookup"><span data-stu-id="2ef07-248">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="2ef07-249">Quando la raccolta `People` viene modificata, l'algoritmo diffing mantiene l'associazione tra istanze `<DetailsEditor>` e `person` istanze:</span><span class="sxs-lookup"><span data-stu-id="2ef07-249">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="2ef07-250">Se un `Person` viene eliminato dall'elenco `People`, solo l'istanza corrispondente di `<DetailsEditor>` viene rimossa dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-250">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="2ef07-251">Altre istanze rimangono invariate.</span><span class="sxs-lookup"><span data-stu-id="2ef07-251">Other instances are left unchanged.</span></span>
* <span data-ttu-id="2ef07-252">Se un `Person` viene inserito in una determinata posizione nell'elenco, viene inserita una nuova istanza di `<DetailsEditor>` in corrispondenza della posizione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-252">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="2ef07-253">Altre istanze rimangono invariate.</span><span class="sxs-lookup"><span data-stu-id="2ef07-253">Other instances are left unchanged.</span></span>
* <span data-ttu-id="2ef07-254">Se `Person` le voci vengono riordinate, le istanze di `<DetailsEditor>` corrispondenti vengono mantenute e nuovamente ordinate nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-254">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="2ef07-255">In alcuni scenari, l'utilizzo di `@key` riduce al minimo la complessità del rendering ed evita potenziali problemi con le parti con stato del DOM che cambiano, ad esempio la posizione di messa a fuoco.</span><span class="sxs-lookup"><span data-stu-id="2ef07-255">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2ef07-256">Le chiavi sono locali per ogni elemento contenitore o componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-256">Keys are local to each container element or component.</span></span> <span data-ttu-id="2ef07-257">Le chiavi non vengono confrontate globalmente nell'intero documento.</span><span class="sxs-lookup"><span data-stu-id="2ef07-257">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="2ef07-258">Quando usare \@chiave</span><span class="sxs-lookup"><span data-stu-id="2ef07-258">When to use \@key</span></span>

<span data-ttu-id="2ef07-259">In genere, è opportuno usare `@key` ogni volta che viene eseguito il rendering di un elenco (ad esempio, in un blocco di `@foreach`) e un valore appropriato per definire il `@key`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-259">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="2ef07-260">È anche possibile usare `@key` per impedire Blazor di mantenere un sottoalbero di elementi o componenti quando un oggetto viene modificato:</span><span class="sxs-lookup"><span data-stu-id="2ef07-260">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="2ef07-261">Se `@currentPerson` viene modificata, la direttiva attribute `@key` impone Blazor di rimuovere l'intero `<div>` e i relativi discendenti e ricompilare il sottoalbero all'interno dell'interfaccia utente con nuovi elementi e componenti.</span><span class="sxs-lookup"><span data-stu-id="2ef07-261">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="2ef07-262">Questo può essere utile se è necessario garantire che lo stato dell'interfaccia utente non venga mantenuto quando `@currentPerson` modifiche.</span><span class="sxs-lookup"><span data-stu-id="2ef07-262">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="2ef07-263">Quando non usare \@chiave</span><span class="sxs-lookup"><span data-stu-id="2ef07-263">When not to use \@key</span></span>

<span data-ttu-id="2ef07-264">Si verifica un costo in termini di prestazioni quando si verificano differenze con `@key`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-264">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="2ef07-265">Il costo delle prestazioni non è elevato, ma specifica solo `@key` se il controllo delle regole di conservazione degli elementi o dei componenti avvantaggia l'app.</span><span class="sxs-lookup"><span data-stu-id="2ef07-265">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="2ef07-266">Anche se non viene usato `@key`, Blazor conserva il più possibile le istanze di elementi e componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="2ef07-266">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="2ef07-267">L'unico vantaggio dell'utilizzo di `@key` è il controllo sulla *modalità* di mapping delle istanze del modello alle istanze dei componenti conservati, anziché sull'algoritmo di diffing che seleziona il mapping.</span><span class="sxs-lookup"><span data-stu-id="2ef07-267">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="2ef07-268">Valori da usare per la chiave di \@</span><span class="sxs-lookup"><span data-stu-id="2ef07-268">What values to use for \@key</span></span>

<span data-ttu-id="2ef07-269">In genere, è opportuno fornire uno dei seguenti tipi di valore per `@key`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-269">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="2ef07-270">Istanze di oggetti modello (ad esempio, un'istanza di `Person` come nell'esempio precedente).</span><span class="sxs-lookup"><span data-stu-id="2ef07-270">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="2ef07-271">In questo modo si garantisce la conservazione in base all'uguaglianza del riferimento all'oggetto.</span><span class="sxs-lookup"><span data-stu-id="2ef07-271">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="2ef07-272">Identificatori univoci (ad esempio, valori di chiave primaria di tipo `int`, `string`o `Guid`).</span><span class="sxs-lookup"><span data-stu-id="2ef07-272">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="2ef07-273">Assicurarsi che i valori usati per `@key` non siano in conflitto.</span><span class="sxs-lookup"><span data-stu-id="2ef07-273">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="2ef07-274">Se vengono rilevati valori di conflitto nello stesso elemento padre, Blazor genera un'eccezione perché non è in grado di eseguire il mapping deterministico di elementi o componenti precedenti a nuovi elementi o componenti.</span><span class="sxs-lookup"><span data-stu-id="2ef07-274">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="2ef07-275">Utilizzare solo valori distinti, ad esempio istanze di oggetti o valori di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="2ef07-275">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="2ef07-276">Supporto di classi parziali</span><span class="sxs-lookup"><span data-stu-id="2ef07-276">Partial class support</span></span>

<span data-ttu-id="2ef07-277">I componenti Razor vengono generati come classi parziali.</span><span class="sxs-lookup"><span data-stu-id="2ef07-277">Razor components are generated as partial classes.</span></span> <span data-ttu-id="2ef07-278">I componenti Razor vengono creati usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="2ef07-278">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="2ef07-279">C#il codice viene definito in un blocco di [`@code`](xref:mvc/views/razor#code) con markup HTML e codice Razor in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="2ef07-279">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="2ef07-280">i modelli di Blazor definiscono i componenti Razor usando questo approccio.</span><span class="sxs-lookup"><span data-stu-id="2ef07-280">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="2ef07-281">C#il codice viene inserito in un file code-behind definito come classe parziale.</span><span class="sxs-lookup"><span data-stu-id="2ef07-281">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="2ef07-282">L'esempio seguente illustra il componente `Counter` predefinito con un blocco di `@code` in un'app generata da un modello di Blazor.</span><span class="sxs-lookup"><span data-stu-id="2ef07-282">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="2ef07-283">Il markup HTML, il codice Razor C# e il codice si trovano nello stesso file:</span><span class="sxs-lookup"><span data-stu-id="2ef07-283">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="2ef07-284">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-284">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="2ef07-285">Il componente `Counter` può essere creato anche usando un file code-behind con una classe parziale:</span><span class="sxs-lookup"><span data-stu-id="2ef07-285">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="2ef07-286">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-286">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="2ef07-287">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-287">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

<span data-ttu-id="2ef07-288">Aggiungere gli spazi dei nomi necessari al file di classe parziale in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="2ef07-288">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="2ef07-289">Gli spazi dei nomi tipici usati dai componenti Razor includono:</span><span class="sxs-lookup"><span data-stu-id="2ef07-289">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="2ef07-290">Specificare una classe di base</span><span class="sxs-lookup"><span data-stu-id="2ef07-290">Specify a base class</span></span>

<span data-ttu-id="2ef07-291">È possibile utilizzare la direttiva [`@inherits`](xref:mvc/views/razor#inherits) per specificare una classe di base per un componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-291">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="2ef07-292">Nell'esempio seguente viene illustrato come un componente può ereditare una classe base, `BlazorRocksBase`, per fornire le proprietà e i metodi del componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-292">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="2ef07-293">La classe base deve derivare da `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-293">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="2ef07-294">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-294">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="2ef07-295">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-295">*BlazorRocksBase.cs*:</span></span>

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

## <a name="specify-an-attribute"></a><span data-ttu-id="2ef07-296">Specificare un attributo</span><span class="sxs-lookup"><span data-stu-id="2ef07-296">Specify an attribute</span></span>

<span data-ttu-id="2ef07-297">Gli attributi possono essere specificati nei componenti Razor con la direttiva [`@attribute`](xref:mvc/views/razor#attribute) .</span><span class="sxs-lookup"><span data-stu-id="2ef07-297">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="2ef07-298">Nell'esempio seguente viene applicato l'attributo `[Authorize]` alla classe Component:</span><span class="sxs-lookup"><span data-stu-id="2ef07-298">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="2ef07-299">Importa componenti</span><span class="sxs-lookup"><span data-stu-id="2ef07-299">Import components</span></span>

<span data-ttu-id="2ef07-300">Lo spazio dei nomi di un componente creato con Razor si basa su (in ordine di priorità):</span><span class="sxs-lookup"><span data-stu-id="2ef07-300">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="2ef07-301">[`@namespace`](xref:mvc/views/razor#namespace) designazione nel markup del*file Razor (razor) (* `@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="2ef07-301">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="2ef07-302">`RootNamespace` del progetto nel file di progetto (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="2ef07-302">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="2ef07-303">Il nome del progetto, tratto dal nome file del file di progetto (con*estensione csproj*), e il percorso dalla radice del progetto al componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-303">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="2ef07-304">Ad esempio, il Framework risolve *{Project root}/pages/index.Razor* (*BlazorSample. csproj*) nello spazio dei nomi `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-304">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="2ef07-305">I componenti C# seguono le regole di associazione dei nomi.</span><span class="sxs-lookup"><span data-stu-id="2ef07-305">Components follow C# name binding rules.</span></span> <span data-ttu-id="2ef07-306">Per il componente `Index` in questo esempio, i componenti nell'ambito sono tutti componenti:</span><span class="sxs-lookup"><span data-stu-id="2ef07-306">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="2ef07-307">Nella stessa cartella, *pagine*.</span><span class="sxs-lookup"><span data-stu-id="2ef07-307">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="2ef07-308">Componenti nella radice del progetto che non specificano in modo esplicito uno spazio dei nomi diverso.</span><span class="sxs-lookup"><span data-stu-id="2ef07-308">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="2ef07-309">I componenti definiti in uno spazio dei nomi diverso vengono introdotti nell'ambito usando la direttiva [`@using`](xref:mvc/views/razor#using) di Razor.</span><span class="sxs-lookup"><span data-stu-id="2ef07-309">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="2ef07-310">Se un altro componente, `NavMenu.razor`, esiste nella cartella *BlazorSample/Shared* , il componente può essere utilizzato in `Index.razor` con la seguente istruzione `@using`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-310">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="2ef07-311">È anche possibile fare riferimento ai componenti usando i relativi nomi completi, che non richiedono la direttiva [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="2ef07-311">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="2ef07-312">La qualificazione `global::` non è supportata.</span><span class="sxs-lookup"><span data-stu-id="2ef07-312">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="2ef07-313">L'importazione di componenti con istruzioni `using` con alias, ad esempio `@using Foo = Bar`, non è supportata.</span><span class="sxs-lookup"><span data-stu-id="2ef07-313">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="2ef07-314">I nomi parzialmente qualificati non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="2ef07-314">Partially qualified names aren't supported.</span></span> <span data-ttu-id="2ef07-315">Ad esempio, l'aggiunta di `@using BlazorSample` e il riferimento `NavMenu.razor` con `<Shared.NavMenu></Shared.NavMenu>` non è supportata.</span><span class="sxs-lookup"><span data-stu-id="2ef07-315">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="2ef07-316">Attributi dell'elemento HTML condizionale</span><span class="sxs-lookup"><span data-stu-id="2ef07-316">Conditional HTML element attributes</span></span>

<span data-ttu-id="2ef07-317">Gli attributi degli elementi HTML vengono sottoposti a rendering in modo condizionale in base al valore .NET.</span><span class="sxs-lookup"><span data-stu-id="2ef07-317">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="2ef07-318">Se il valore è `false` o `null`, l'attributo non viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="2ef07-318">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="2ef07-319">Se il valore è `true`, l'attributo viene sottoposto a rendering ridotto a icona.</span><span class="sxs-lookup"><span data-stu-id="2ef07-319">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="2ef07-320">Nell'esempio seguente `IsCompleted` determina se viene eseguito il rendering `checked` nel markup dell'elemento:</span><span class="sxs-lookup"><span data-stu-id="2ef07-320">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="2ef07-321">Se `IsCompleted` è `true`, viene eseguito il rendering della casella di controllo come segue:</span><span class="sxs-lookup"><span data-stu-id="2ef07-321">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="2ef07-322">Se `IsCompleted` è `false`, viene eseguito il rendering della casella di controllo come segue:</span><span class="sxs-lookup"><span data-stu-id="2ef07-322">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="2ef07-323">Per altre informazioni, vedere <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="2ef07-323">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="2ef07-324">Alcuni attributi HTML, ad esempio [aria-premuti](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), non funzionano correttamente quando il tipo .NET è un `bool`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-324">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="2ef07-325">In questi casi, usare un tipo di `string` anziché una `bool`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-325">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="2ef07-326">HTML non elaborato</span><span class="sxs-lookup"><span data-stu-id="2ef07-326">Raw HTML</span></span>

<span data-ttu-id="2ef07-327">Le stringhe vengono in genere sottoposte a rendering usando i nodi di testo DOM, il che significa che qualsiasi markup che può contenere viene ignorato e considerato come testo letterale.</span><span class="sxs-lookup"><span data-stu-id="2ef07-327">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="2ef07-328">Per eseguire il rendering del codice HTML non elaborato, eseguire il wrapping del contenuto HTML in un valore `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-328">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="2ef07-329">Il valore viene analizzato in formato HTML o SVG e inserito nel DOM.</span><span class="sxs-lookup"><span data-stu-id="2ef07-329">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="2ef07-330">Il rendering di codice HTML non elaborato costruito da un'origine non attendibile costituisce un rischio per la **sicurezza** e deve essere evitato.</span><span class="sxs-lookup"><span data-stu-id="2ef07-330">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="2ef07-331">Nell'esempio seguente viene illustrato l'utilizzo del tipo di `MarkupString` per aggiungere un blocco di contenuto HTML statico all'output sottoposto a rendering di un componente:</span><span class="sxs-lookup"><span data-stu-id="2ef07-331">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="2ef07-332">Parametri e valori di propagazione</span><span class="sxs-lookup"><span data-stu-id="2ef07-332">Cascading values and parameters</span></span>

<span data-ttu-id="2ef07-333">In alcuni scenari, non è pratico eseguire il flusso dei dati da un componente predecessore a un componente discendente usando i [parametri del componente](#component-parameters), soprattutto quando sono presenti diversi livelli di componenti.</span><span class="sxs-lookup"><span data-stu-id="2ef07-333">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="2ef07-334">I valori e i parametri di propagazione consentono di risolvere questo problema fornendo un modo pratico per un componente predecessore per fornire un valore a tutti i relativi componenti discendenti.</span><span class="sxs-lookup"><span data-stu-id="2ef07-334">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="2ef07-335">I parametri e i valori di propagazione forniscono anche un approccio per la coordinazione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="2ef07-335">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="2ef07-336">Esempio di tema</span><span class="sxs-lookup"><span data-stu-id="2ef07-336">Theme example</span></span>

<span data-ttu-id="2ef07-337">Nell'esempio seguente dall'app di esempio, la classe `ThemeInfo` specifica le informazioni sul tema per scorrere la gerarchia dei componenti in modo che tutti i pulsanti all'interno di una determinata parte dell'app condividono lo stesso stile.</span><span class="sxs-lookup"><span data-stu-id="2ef07-337">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="2ef07-338">*UIThemeClasses/themeinfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="2ef07-338">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="2ef07-339">Un componente predecessore può fornire un valore di propagazione utilizzando il componente valore di propagazione.</span><span class="sxs-lookup"><span data-stu-id="2ef07-339">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="2ef07-340">Il componente `CascadingValue` esegue il wrapping di un sottoalbero della gerarchia dei componenti e fornisce un singolo valore a tutti i componenti all'interno di tale sottoalbero.</span><span class="sxs-lookup"><span data-stu-id="2ef07-340">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="2ef07-341">Ad esempio, l'app di esempio specifica le informazioni sul tema (`ThemeInfo`) in uno dei layout dell'app come parametro di propagazione per tutti i componenti che costituiscono il corpo del layout della proprietà `@Body`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-341">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="2ef07-342">al `ButtonClass` viene assegnato un valore di `btn-success` nel componente layout.</span><span class="sxs-lookup"><span data-stu-id="2ef07-342">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="2ef07-343">Qualsiasi componente discendente può utilizzare questa proprietà tramite l'`ThemeInfo` oggetto a cascata.</span><span class="sxs-lookup"><span data-stu-id="2ef07-343">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="2ef07-344">componente `CascadingValuesParametersLayout`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-344">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="2ef07-345">Per utilizzare i valori di propagazione, i componenti dichiarano i parametri di propagazione utilizzando l'attributo `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-345">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="2ef07-346">I valori a cascata vengono associati ai parametri di propagazione per tipo.</span><span class="sxs-lookup"><span data-stu-id="2ef07-346">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="2ef07-347">Nell'app di esempio il componente `CascadingValuesParametersTheme` associa il `ThemeInfo` valore di propagazione a un parametro di propagazione.</span><span class="sxs-lookup"><span data-stu-id="2ef07-347">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="2ef07-348">Il parametro viene usato per impostare la classe CSS per uno dei pulsanti visualizzati dal componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-348">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="2ef07-349">componente `CascadingValuesParametersTheme`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-349">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

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
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="2ef07-350">Per eseguire il propagazione di più valori dello stesso tipo all'interno dello stesso sottoalbero, specificare una stringa di `Name` univoca per ogni componente `CascadingValue` e la `CascadingParameter`corrispondente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-350">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="2ef07-351">Nell'esempio seguente due componenti `CascadingValue` propagano istanze diverse di `MyCascadingType` in base al nome:</span><span class="sxs-lookup"><span data-stu-id="2ef07-351">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="2ef07-352">In un componente discendente i parametri a cascata ricevono i rispettivi valori dai valori a cascata corrispondenti nel componente predecessore per nome:</span><span class="sxs-lookup"><span data-stu-id="2ef07-352">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="2ef07-353">Esempio di tabulazione</span><span class="sxs-lookup"><span data-stu-id="2ef07-353">TabSet example</span></span>

<span data-ttu-id="2ef07-354">I parametri di propagazione consentono inoltre ai componenti di collaborare attraverso la gerarchia dei componenti.</span><span class="sxs-lookup"><span data-stu-id="2ef07-354">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="2ef07-355">Si consideri, ad esempio, l'esempio di *tabulazione* seguente nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="2ef07-355">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="2ef07-356">L'app di esempio dispone di un'interfaccia `ITab` che le schede implementano:</span><span class="sxs-lookup"><span data-stu-id="2ef07-356">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="2ef07-357">Il componente `CascadingValuesParametersTabSet` usa il componente `TabSet`, che contiene diversi componenti di `Tab`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-357">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

<span data-ttu-id="2ef07-358">I componenti di `Tab` figlio non vengono passati in modo esplicito come parametri al `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-358">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="2ef07-359">Al contrario, i componenti `Tab` figlio fanno parte del contenuto figlio della `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="2ef07-359">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="2ef07-360">Tuttavia, il `TabSet` deve comunque conoscere ogni componente `Tab` in modo che sia in grado di eseguire il rendering delle intestazioni e della scheda attiva. Per abilitare questo coordinamento senza richiedere codice aggiuntivo, il componente `TabSet` *può fornire se stesso come valore* di propagazione che viene quindi prelevato dai componenti `Tab` discendenti.</span><span class="sxs-lookup"><span data-stu-id="2ef07-360">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="2ef07-361">componente `TabSet`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-361">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="2ef07-362">I componenti `Tab` discendenti acquisiscono l'`TabSet` contenitore come parametro di propagazione, quindi i componenti `Tab` si aggiungono al `TabSet` e coordinano la scheda attiva.</span><span class="sxs-lookup"><span data-stu-id="2ef07-362">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="2ef07-363">componente `Tab`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-363">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="2ef07-364">Modelli Razor</span><span class="sxs-lookup"><span data-stu-id="2ef07-364">Razor templates</span></span>

<span data-ttu-id="2ef07-365">I frammenti di rendering possono essere definiti usando la sintassi del modello Razor.</span><span class="sxs-lookup"><span data-stu-id="2ef07-365">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="2ef07-366">I modelli Razor sono un modo per definire un frammento di interfaccia utente e presupporre il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="2ef07-366">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="2ef07-367">Nell'esempio seguente viene illustrato come specificare `RenderFragment` e `RenderFragment<T>` valori ed eseguire il rendering dei modelli direttamente in un componente.</span><span class="sxs-lookup"><span data-stu-id="2ef07-367">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="2ef07-368">I frammenti di rendering possono anche essere passati come argomenti ai [componenti basati su modelli](xref:blazor/templated-components).</span><span class="sxs-lookup"><span data-stu-id="2ef07-368">Render fragments can also be passed as arguments to [templated components](xref:blazor/templated-components).</span></span>

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="2ef07-369">Output del codice precedente sottoposto a rendering:</span><span class="sxs-lookup"><span data-stu-id="2ef07-369">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="2ef07-370">Immagini SVG (Scalable Vector Graphics)</span><span class="sxs-lookup"><span data-stu-id="2ef07-370">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="2ef07-371">Poiché Blazor esegue il rendering del codice HTML, le immagini supportate dal browser, incluse le immagini SVG (Scalable Vector Graphics *) (SVG),* sono supportate tramite il tag `<img>`:</span><span class="sxs-lookup"><span data-stu-id="2ef07-371">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="2ef07-372">Analogamente, le immagini SVG sono supportate nelle regole CSS di un file di foglio di stile (*CSS*):</span><span class="sxs-lookup"><span data-stu-id="2ef07-372">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="2ef07-373">Tuttavia, il markup SVG inline non è supportato in tutti gli scenari.</span><span class="sxs-lookup"><span data-stu-id="2ef07-373">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="2ef07-374">Se si inserisce un tag di `<svg>` direttamente in un file di componente (*Razor*), il rendering delle immagini di base è supportato, ma molti scenari avanzati non sono ancora supportati.</span><span class="sxs-lookup"><span data-stu-id="2ef07-374">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="2ef07-375">Ad esempio, i tag di `<use>` non sono attualmente rispettati e non è possibile usare `@bind` con alcuni tag SVG.</span><span class="sxs-lookup"><span data-stu-id="2ef07-375">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="2ef07-376">Queste limitazioni verranno affrontate in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="2ef07-376">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2ef07-377">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2ef07-377">Additional resources</span></span>

* <span data-ttu-id="2ef07-378"><xref:security/blazor/server> &ndash; include informazioni aggiuntive sulla creazione di app di Blazor server che devono essere confrontate con l'esaurimento delle risorse.</span><span class="sxs-lookup"><span data-stu-id="2ef07-378"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
