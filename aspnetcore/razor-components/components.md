---
title: Creare e usare componenti Razor
author: guardrex
description: Informazioni su come creare e usare componenti Razor, incluse le procedure eseguire l'associazione ai dati, gestire gli eventi e gestire i cicli di vita del componente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/components
ms.openlocfilehash: f8ac7f3844b94a162e8d1c45f80ae153d89536ee
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515563"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="37e6a-103">Creare e usare componenti Razor</span><span class="sxs-lookup"><span data-stu-id="37e6a-103">Create and use Razor Components</span></span>

<span data-ttu-id="37e6a-104">Dal [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), e [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="37e6a-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="37e6a-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="37e6a-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="37e6a-106">Vedere l'argomento [Introduzione a Razor Components](xref:razor-components/get-started) per i prerequisiti.</span><span class="sxs-lookup"><span data-stu-id="37e6a-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="37e6a-107">Le app di componenti Razor vengono compilate usando *componenti*.</span><span class="sxs-lookup"><span data-stu-id="37e6a-107">Razor Components apps are built using *components*.</span></span> <span data-ttu-id="37e6a-108">Un componente è un blocco autonomo dell'interfaccia utente (UI), ad esempio una pagina, finestra di dialogo o form.</span><span class="sxs-lookup"><span data-stu-id="37e6a-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="37e6a-109">Un componente include il markup HTML e la logica di elaborazione necessaria per inserire i dati o per rispondere a eventi di interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-109">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="37e6a-110">I componenti sono flessibili e leggero.</span><span class="sxs-lookup"><span data-stu-id="37e6a-110">Components are flexible and lightweight.</span></span> <span data-ttu-id="37e6a-111">Possono essere annidati, riutilizzati e condivise tra progetti.</span><span class="sxs-lookup"><span data-stu-id="37e6a-111">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="37e6a-112">Classi Component</span><span class="sxs-lookup"><span data-stu-id="37e6a-112">Component classes</span></span>

<span data-ttu-id="37e6a-113">I componenti sono in genere implementati nei file Razor componente (*.razor*) usando una combinazione di C# e il markup HTML (*cshtml* file vengono usati nelle App Blazor).</span><span class="sxs-lookup"><span data-stu-id="37e6a-113">Components are typically implemented in Razor Component files (*.razor*) using a combination of C# and HTML markup (*.cshtml* files are used in Blazor apps).</span></span>

<span data-ttu-id="37e6a-114">I componenti possono essere creati nelle App Razor componenti usando il *. cshtml* estensione di file, purché i file vengono identificati come i file Razor componente utilizzando il `_RazorComponentInclude` proprietà MSBuild.</span><span class="sxs-lookup"><span data-stu-id="37e6a-114">Components can be authored in Razor Components apps using the *.cshtml* file extension as long as the files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="37e6a-115">Ad esempio, un'app creata usando il modello del componente Razor specifica che tutti i *. cshtml* file sotto il *componenti* cartella deve essere trattata come file dei componenti di Razor:</span><span class="sxs-lookup"><span data-stu-id="37e6a-115">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components files:</span></span>

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="37e6a-116">L'interfaccia utente per un componente viene definito utilizzando il codice HTML.</span><span class="sxs-lookup"><span data-stu-id="37e6a-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="37e6a-117">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="37e6a-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="37e6a-118">Quando viene compilata un'app Razor componenti, il markup HTML e C# per la logica per il rendering vengono convertiti in una classe del componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-118">When a Razor Components app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="37e6a-119">Il nome della classe generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="37e6a-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="37e6a-120">I membri della classe del componente sono definiti un `@functions` blocco (più `@functions` blocco è consentito).</span><span class="sxs-lookup"><span data-stu-id="37e6a-120">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="37e6a-121">Nel `@functions` blocco, stato del componente (proprietà, campi) viene specificato con i metodi per la gestione degli eventi o per definire logica altri componenti.</span><span class="sxs-lookup"><span data-stu-id="37e6a-121">In the `@functions` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="37e6a-122">I membri di componenti possono quindi essere usati come parte del componente di rendering per la logica usando C# le espressioni che iniziano con `@`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-122">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="37e6a-123">Ad esempio, un C# viene eseguito il rendering di campo anteponendo `@` per il nome del campo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-123">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="37e6a-124">Nell'esempio seguente viene valutata e viene eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="37e6a-124">The following example evaluates and renders:</span></span>

* `_headingFontStyle` <span data-ttu-id="37e6a-125">il valore di proprietà CSS per `font-style`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-125">to the CSS property value for `font-style`.</span></span>
* `_headingText` <span data-ttu-id="37e6a-126">per il contenuto di `<h1>` elemento.</span><span class="sxs-lookup"><span data-stu-id="37e6a-126">to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="37e6a-127">Dopo che il componente inizialmente viene eseguito il rendering, il componente Rigenera relativa struttura ad albero di rendering in risposta agli eventi.</span><span class="sxs-lookup"><span data-stu-id="37e6a-127">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="37e6a-128">I componenti di Razor quindi confronta il nuovo albero di rendering confrontandolo con quello precedente e si applica a tutte le modifiche a modello di oggetti del browser documento (DOM).</span><span class="sxs-lookup"><span data-stu-id="37e6a-128">Razor Components then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="37e6a-129">Integrare i componenti nelle App Razor Pages ed MVC</span><span class="sxs-lookup"><span data-stu-id="37e6a-129">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="37e6a-130">Usare i componenti con le app esistenti di Razor Pages ed MVC.</span><span class="sxs-lookup"><span data-stu-id="37e6a-130">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="37e6a-131">Non è necessario riscrivere le pagine esistenti o le viste per utilizzare i componenti di Razor.</span><span class="sxs-lookup"><span data-stu-id="37e6a-131">There's no need to rewrite existing pages or views to use Razor Components.</span></span> <span data-ttu-id="37e6a-132">Quando viene eseguito il rendering della pagina o della vista, i componenti sono già sottoposte a rendering&dagger; nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="37e6a-132">When the page or view is rendered, components are prerendered&dagger; at the same time.</span></span> 

> [!NOTE]
> <span data-ttu-id="37e6a-133">&dagger;Il rendering preliminare sul lato server è abilitato per App i componenti di Razor per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="37e6a-133">&dagger;Server-side prerendering is enabled for Razor Components apps by default.</span></span> <span data-ttu-id="37e6a-134">App sul lato client Blazor supporterà il rendering preliminare nella prossima versione di anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="37e6a-134">Client-side Blazor apps will support prerendering in the upcoming Preview 4 release.</span></span> <span data-ttu-id="37e6a-135">Per altre informazioni, vedere [aggiornare i modelli/middleware usare MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).</span><span class="sxs-lookup"><span data-stu-id="37e6a-135">For more information, see [Update templates/middleware to use MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).</span></span>

<span data-ttu-id="37e6a-136">Per eseguire il rendering di un componente da una pagina o vista, utilizzare il `RenderComponentAsync<TComponent>` metodo helper HTML:</span><span class="sxs-lookup"><span data-stu-id="37e6a-136">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="37e6a-137">Viene eseguito il rendering delle pagine e delle viste di componenti non sono ancora interattivi nella versione Preview 3.</span><span class="sxs-lookup"><span data-stu-id="37e6a-137">Components rendered from pages and views aren't yet interactive in the Preview 3 release.</span></span> <span data-ttu-id="37e6a-138">Ad esempio, selezionando un pulsante non attiva una chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-138">For example, selecting a button doesn't trigger a method call.</span></span> <span data-ttu-id="37e6a-139">Una futura versione di anteprima verranno risolvere questa limitazione e aggiungere il supporto per i componenti per il rendering usando la normale sintassi di elementi e attributi.</span><span class="sxs-lookup"><span data-stu-id="37e6a-139">A future preview will address this limitation and add support for rendering components using the normal element and attribute syntax.</span></span>

<span data-ttu-id="37e6a-140">Sebbene le pagine e le viste è possono usare i componenti, non accade il contrario.</span><span class="sxs-lookup"><span data-stu-id="37e6a-140">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="37e6a-141">I componenti non è possibile usare scenari specifici della pagina e visualizzazione, ad esempio le visualizzazioni parziali e sezioni.</span><span class="sxs-lookup"><span data-stu-id="37e6a-141">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="37e6a-142">Da utilizzare per la logica di visualizzazione parziale in un componente, separare la logica di visualizzazione parziale in un componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-142">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

## <a name="using-components"></a><span data-ttu-id="37e6a-143">Uso di componenti</span><span class="sxs-lookup"><span data-stu-id="37e6a-143">Using components</span></span>

<span data-ttu-id="37e6a-144">I componenti possono includere altri componenti dichiarandoli usando la sintassi degli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="37e6a-144">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="37e6a-145">Il markup per l'uso di un componente è simile a un tag HTML, in cui il nome del tag è il tipo di componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-145">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="37e6a-146">Il markup seguente esegue il rendering di un `HeadingComponent` (*HeadingComponent.cshtml*) istanza:</span><span class="sxs-lookup"><span data-stu-id="37e6a-146">The following markup renders a `HeadingComponent` (*HeadingComponent.cshtml*) instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a><span data-ttu-id="37e6a-147">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="37e6a-147">Component parameters</span></span>

<span data-ttu-id="37e6a-148">I componenti possono avere *parametri del componente*, che vengono definiti utilizzando *pubblici* le proprietà nella classe del componente decorati con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-148">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="37e6a-149">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="37e6a-149">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="37e6a-150">Nell'esempio seguente, il `ParentComponent` imposta il valore della `Title` proprietà del `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="37e6a-150">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="37e6a-151">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-151">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

<span data-ttu-id="37e6a-152">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-152">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a><span data-ttu-id="37e6a-153">Contenuto figlio</span><span class="sxs-lookup"><span data-stu-id="37e6a-153">Child content</span></span>

<span data-ttu-id="37e6a-154">I componenti possono impostare il contenuto di un altro componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-154">Components can set the content of another component.</span></span> <span data-ttu-id="37e6a-155">Il componente assegnazione fornisce il contenuto tra i tag che specificano il componente ricevente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-155">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="37e6a-156">Ad esempio, un `ParentComponent` grado di fornire contenuto per il rendering da un componente figlio da posizionare il contenuto all'interno di `<ChildComponent>` tag.</span><span class="sxs-lookup"><span data-stu-id="37e6a-156">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="37e6a-157">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-157">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

<span data-ttu-id="37e6a-158">Il componente figlio ha un `ChildContent` proprietà che rappresenta un `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-158">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="37e6a-159">Il valore di `ChildContent` viene posizionato nel markup del componente figlio in cui il contenuto deve essere sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="37e6a-159">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="37e6a-160">Nell'esempio seguente, il valore di `ChildContent` viene ricevuto dal componente padre e il rendering all'interno del pannello Bootstrap `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-160">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="37e6a-161">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-161">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> <span data-ttu-id="37e6a-162">La ricezione di proprietà di `RenderFragment` contenuto deve essere denominato `ChildContent` per convenzione.</span><span class="sxs-lookup"><span data-stu-id="37e6a-162">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="37e6a-163">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="37e6a-163">Data binding</span></span>

<span data-ttu-id="37e6a-164">Viene eseguita l'associazione dati a entrambi i componenti e gli elementi DOM con il `bind` attributo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-164">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="37e6a-165">L'esempio seguente associa il `ItalicsCheck` selezionata dello stato per la casella di controllo:</span><span class="sxs-lookup"><span data-stu-id="37e6a-165">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

<span data-ttu-id="37e6a-166">Quando la casella di controllo è selezionata e viene deselezionata, il valore della proprietà viene aggiornato alla `true` e `false`, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-166">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="37e6a-167">La casella di controllo viene aggiornata nell'interfaccia utente solo quando il componente viene eseguito il rendering, non in risposta alla modifica il valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="37e6a-167">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="37e6a-168">Poiché i componenti stessi esegue il rendering dopo l'esecuzione di codice del gestore eventi, gli aggiornamenti delle proprietà sono in genere immediatamente nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-168">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="37e6a-169">Usando `bind` con un `CurrentValue` proprietà (`<input bind="@CurrentValue">`) è essenzialmente equivalente a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="37e6a-169">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue">`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

<span data-ttu-id="37e6a-170">Quando il componente viene eseguito il rendering, il `value` dell'elemento di input proviene il `CurrentValue` proprietà.</span><span class="sxs-lookup"><span data-stu-id="37e6a-170">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="37e6a-171">Quando l'utente digita nella casella di testo, il `onchange` evento viene generato e `CurrentValue` proprietà è impostata per il valore modificato.</span><span class="sxs-lookup"><span data-stu-id="37e6a-171">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="37e6a-172">In realtà, la generazione di codice è un po' più complesso perché `bind` gestisce alcuni casi in cui vengono eseguite conversioni dei tipi.</span><span class="sxs-lookup"><span data-stu-id="37e6a-172">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="37e6a-173">In linea di principio `bind` associa il valore corrente di un'espressione con un `value` attributo e gestisce le modifiche utilizzando il gestore registrato.</span><span class="sxs-lookup"><span data-stu-id="37e6a-173">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="37e6a-174">Oltre a `onchange`, la proprietà può essere associata usando altri eventi, ad esempio `oninput` verificando più esplicito sulla destinazione dell'associazione:</span><span class="sxs-lookup"><span data-stu-id="37e6a-174">In addition to `onchange`, the property can be bound using other events like `oninput` by being more explicit about what to bind to:</span></span>

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

<span data-ttu-id="37e6a-175">A differenza `onchange`, `oninput` viene attivato in tutti i caratteri che costituisce l'input nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-175">Unlike `onchange`, `oninput` fires for every character that is input into the text box.</span></span>

**<span data-ttu-id="37e6a-176">Stringhe di formato</span><span class="sxs-lookup"><span data-stu-id="37e6a-176">Format strings</span></span>**

<span data-ttu-id="37e6a-177">Data binding dati funziona con <xref:System.DateTime> stringhe di formato.</span><span class="sxs-lookup"><span data-stu-id="37e6a-177">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="37e6a-178">Altre espressioni di formato, ad esempio valuta o formati di numero, non sono disponibili in questo momento.</span><span class="sxs-lookup"><span data-stu-id="37e6a-178">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="37e6a-179">Il `format-value` attributo specifica il formato della data da applicare al `value` del `input` elemento.</span><span class="sxs-lookup"><span data-stu-id="37e6a-179">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="37e6a-180">Il formato viene usato anche per analizzare il valore quando un `onchange` evento si verifica.</span><span class="sxs-lookup"><span data-stu-id="37e6a-180">The format is also used to parse the value when an `onchange` event occurs.</span></span>

**<span data-ttu-id="37e6a-181">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="37e6a-181">Component parameters</span></span>**

<span data-ttu-id="37e6a-182">Binding riconosce anche i parametri del componente, in cui `bind-{property}` possibile associare un valore della proprietà tra i componenti.</span><span class="sxs-lookup"><span data-stu-id="37e6a-182">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="37e6a-183">Il componente seguente usa `ChildComponent` e associa il `ParentYear` parametro dall'oggetto padre di `Year` parametro sul componente figlio:</span><span class="sxs-lookup"><span data-stu-id="37e6a-183">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="37e6a-184">Componente padre:</span><span class="sxs-lookup"><span data-stu-id="37e6a-184">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="37e6a-185">Componente di figlio:</span><span class="sxs-lookup"><span data-stu-id="37e6a-185">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

<span data-ttu-id="37e6a-186">Il caricamento di `ParentComponent` genera il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="37e6a-186">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="37e6a-187">Se il valore della `ParentYear` proprietà viene modificata selezionando il pulsante nel `ParentComponent`, il `Year` proprietà del `ChildComponent` viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="37e6a-187">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="37e6a-188">Il nuovo valore della `Year` viene eseguito il rendering nell'interfaccia utente quando il `ParentComponent` è modificate:</span><span class="sxs-lookup"><span data-stu-id="37e6a-188">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="37e6a-189">Il `Year` parametro è associabile perché contiene complementare `YearChanged` che corrisponde al tipo di evento il `Year` parametro.</span><span class="sxs-lookup"><span data-stu-id="37e6a-189">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="37e6a-190">Per convenzione, `<ChildComponent bind-Year="@ParentYear" />` è essenzialmente equivalente a scrittura,</span><span class="sxs-lookup"><span data-stu-id="37e6a-190">By convention, `<ChildComponent bind-Year="@ParentYear" />` is essentially equivalent to writing,</span></span>

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

<span data-ttu-id="37e6a-191">In generale, una proprietà può essere associata a un gestore eventi corrispondente utilizzando `bind-property-event` attributo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-191">In general, a property can be bound to a corresponding event handler using `bind-property-event` attribute.</span></span>

## <a name="event-handling"></a><span data-ttu-id="37e6a-192">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="37e6a-192">Event handling</span></span>

<span data-ttu-id="37e6a-193">I componenti di Razor forniscono funzionalità di gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="37e6a-193">Razor Components provide event handling features.</span></span> <span data-ttu-id="37e6a-194">Per un attributo dell'elemento HTML denominato `on<event>` (ad esempio `onclick`, `onsubmit`) con un valore di tipo delegato, Razor componenti considera il valore dell'attributo come un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="37e6a-194">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="37e6a-195">Il nome dell'attributo inizia sempre con `on`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-195">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="37e6a-196">Il codice seguente chiama il `UpdateHeading` metodo quando si seleziona il pulsante nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="37e6a-196">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="37e6a-197">Il codice seguente chiama il `CheckboxChanged` metodo quando la casella di controllo viene modificata nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="37e6a-197">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="37e6a-198">Gestori di eventi possono anche essere asincrono e restituire un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="37e6a-198">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="37e6a-199">Non è necessario chiamare manualmente `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-199">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="37e6a-200">Le eccezioni vengono registrate quando si verificano.</span><span class="sxs-lookup"><span data-stu-id="37e6a-200">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="37e6a-201">Per alcuni eventi, sono consentiti i tipi di argomento di evento specifiche degli eventi.</span><span class="sxs-lookup"><span data-stu-id="37e6a-201">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="37e6a-202">Se l'accesso a uno di questi tipi di evento non è necessaria, non è necessario nella chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-202">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="37e6a-203">L'elenco degli argomenti dell'evento supportato è:</span><span class="sxs-lookup"><span data-stu-id="37e6a-203">The list of supported event arguments is:</span></span>

* <span data-ttu-id="37e6a-204">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="37e6a-204">UIEventArgs</span></span>
* <span data-ttu-id="37e6a-205">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="37e6a-205">UIChangeEventArgs</span></span>
* <span data-ttu-id="37e6a-206">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="37e6a-206">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="37e6a-207">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="37e6a-207">UIMouseEventArgs</span></span>

<span data-ttu-id="37e6a-208">Sono anche possibile usare espressioni lambda:</span><span class="sxs-lookup"><span data-stu-id="37e6a-208">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="37e6a-209">Spesso è comodo chiuse su valori aggiuntivi, ad esempio quando si esegue l'iterazione in un set di elementi.</span><span class="sxs-lookup"><span data-stu-id="37e6a-209">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="37e6a-210">L'esempio seguente crea tre pulsanti, ognuno dei quali chiama `UpdateHeading` passando un argomento dell'evento (`UIMouseEventArgs`) e il relativo numero di pulsante (`buttonNumber`) se selezionata nell'interfaccia utente:</span><span class="sxs-lookup"><span data-stu-id="37e6a-210">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="37e6a-211">Effettuare **non** usare la variabile di ciclo (`i`) in un `for` ciclo direttamente in un'espressione lambda.</span><span class="sxs-lookup"><span data-stu-id="37e6a-211">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="37e6a-212">In caso contrario, la stessa variabile viene utilizzata da tutte le espressioni lambda causando `i`del valore per lo stesso in tutte le espressioni lambda.</span><span class="sxs-lookup"><span data-stu-id="37e6a-212">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="37e6a-213">Sempre acquisisce il relativo valore in una variabile locale (`buttonNumber` nell'esempio precedente) e quindi usarla.</span><span class="sxs-lookup"><span data-stu-id="37e6a-213">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="37e6a-214">Acquisire riferimenti ai componenti</span><span class="sxs-lookup"><span data-stu-id="37e6a-214">Capture references to components</span></span>

<span data-ttu-id="37e6a-215">Riferimenti a componenti forniscono un modo get un riferimento a un'istanza del componente in modo che è possibile inviare comandi a tale istanza, ad esempio `Show` o `Reset`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-215">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="37e6a-216">Per acquisire un riferimento del componente, aggiungere un `ref` attributo al componente figlio e quindi definire un campo con lo stesso nome e lo stesso tipo del componente figlio.</span><span class="sxs-lookup"><span data-stu-id="37e6a-216">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="37e6a-217">Quando il componente viene eseguito il rendering, il `loginDialog` campo viene popolato con il `MyLoginDialog` istanza del componente figlio.</span><span class="sxs-lookup"><span data-stu-id="37e6a-217">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="37e6a-218">È quindi possibile richiamare i metodi .NET nell'istanza del componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-218">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37e6a-219">Il `loginDialog` variabile viene popolata solo dopo che il componente viene eseguito il rendering e l'output include il `MyLoginDialog` elemento.</span><span class="sxs-lookup"><span data-stu-id="37e6a-219">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="37e6a-220">Fino a quel punto, non sono necessarie per fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="37e6a-220">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="37e6a-221">Per modificare i riferimenti a componenti dopo che il componente ha terminato il rendering, utilizzare il `OnAfterRenderAsync` o `OnAfterRender` metodi.</span><span class="sxs-lookup"><span data-stu-id="37e6a-221">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="37e6a-222">Durante l'acquisizione di riferimenti a componenti utilizza una sintassi simile a [riferimenti a elementi di acquisizione](xref:razor-components/javascript-interop#capture-references-to-elements), non è un [interoperabilità JavaScript](xref:razor-components/javascript-interop) funzionalità.</span><span class="sxs-lookup"><span data-stu-id="37e6a-222">While capturing component references uses a similar syntax to [capturing element references](xref:razor-components/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:razor-components/javascript-interop) feature.</span></span> <span data-ttu-id="37e6a-223">Riferimenti a componenti non sono passati al codice JavaScript. vengono utilizzati solo nel codice .NET.</span><span class="sxs-lookup"><span data-stu-id="37e6a-223">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="37e6a-224">Effettuare **non** usare riferimenti a componenti per modificare lo stato dei componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="37e6a-224">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="37e6a-225">Utilizzare invece parametri normali dichiarativi per passare i dati per i componenti figlio.</span><span class="sxs-lookup"><span data-stu-id="37e6a-225">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="37e6a-226">In questo modo i componenti figlio eseguire nuovamente il rendering negli orari corretti automaticamente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-226">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="37e6a-227">Metodi del ciclo di vita</span><span class="sxs-lookup"><span data-stu-id="37e6a-227">Lifecycle methods</span></span>

`OnInitAsync` <span data-ttu-id="37e6a-228">e `OnInit` eseguire codice per inizializzare il componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-228">and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="37e6a-229">Per eseguire un'operazione asincrona, utilizzare `OnInitAsync` e il `await` parola chiave per l'operazione:</span><span class="sxs-lookup"><span data-stu-id="37e6a-229">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="37e6a-230">Per un'operazione sincrona, usare `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="37e6a-230">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` <span data-ttu-id="37e6a-231">e `OnParametersSet` vengono chiamati quando un componente ha ricevuto i parametri dal relativo elemento padre e i valori vengono assegnati alle proprietà.</span><span class="sxs-lookup"><span data-stu-id="37e6a-231">and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="37e6a-232">Questi metodi vengono eseguiti dopo l'inizializzazione del componente e quindi ogni volta che il componente viene eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="37e6a-232">These methods are executed after component initialization and then each time the component is rendered:</span></span>

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

`OnAfterRenderAsync` <span data-ttu-id="37e6a-233">e `OnAfterRender` vengono chiamati dopo che un componente ha completato il rendering.</span><span class="sxs-lookup"><span data-stu-id="37e6a-233">and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="37e6a-234">I riferimenti di elemento e dei componenti vengono popolati a questo punto.</span><span class="sxs-lookup"><span data-stu-id="37e6a-234">Element and component references are populated at this point.</span></span> <span data-ttu-id="37e6a-235">Utilizzare questa fase per eseguire ulteriori passaggi di inizializzazione utilizzando il contenuto sottoposto a rendering, ad esempio l'attivazione di librerie JavaScript di terze parti che operano su elementi DOM di cui è stato eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="37e6a-235">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

`SetParameters` <span data-ttu-id="37e6a-236">può essere sottoposto a override per eseguire codice prima che vengano impostati i parametri:</span><span class="sxs-lookup"><span data-stu-id="37e6a-236">can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="37e6a-237">Se `base.SetParameters` non viene richiamato, il codice personalizzato può interpretare il valore di parametri in entrata in qualsiasi modo richiesto.</span><span class="sxs-lookup"><span data-stu-id="37e6a-237">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="37e6a-238">Ad esempio, i parametri in entrata non è necessari essere assegnati alle proprietà nella classe.</span><span class="sxs-lookup"><span data-stu-id="37e6a-238">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

`ShouldRender` <span data-ttu-id="37e6a-239">può essere sottoposto a override per eliminare l'aggiornamento dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-239">can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="37e6a-240">Se l'implementazione restituisce `true`, l'interfaccia utente viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="37e6a-240">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="37e6a-241">Anche se `ShouldRender` viene sottoposto a override, il componente è sempre inizialmente sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="37e6a-241">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="37e6a-242">Eliminazione di componenti con IDisposable</span><span class="sxs-lookup"><span data-stu-id="37e6a-242">Component disposal with IDisposable</span></span>

<span data-ttu-id="37e6a-243">Se un componente implementa <xref:System.IDisposable>, il [metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose) viene chiamato quando il componente è stato rimosso dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-243">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="37e6a-244">Il componente seguente usa `@implements IDisposable` e il `Dispose` metodo:</span><span class="sxs-lookup"><span data-stu-id="37e6a-244">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="37e6a-245">Routing</span><span class="sxs-lookup"><span data-stu-id="37e6a-245">Routing</span></span>

<span data-ttu-id="37e6a-246">Routing in Razor componenti avviene fornendo un modello di route per ogni componente accessibile nell'app.</span><span class="sxs-lookup"><span data-stu-id="37e6a-246">Routing in Razor Components is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="37e6a-247">Quando un *. cshtml* file con un `@page` direttiva viene compilata, la classe generata viene assegnata un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> che specifica il modello di route.</span><span class="sxs-lookup"><span data-stu-id="37e6a-247">When a *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="37e6a-248">In fase di esecuzione, il router cerca classi di componenti con un `RouteAttribute` ed esegue il rendering qualunque sia il componente dispone di un modello di route corrispondente all'URL richiesto.</span><span class="sxs-lookup"><span data-stu-id="37e6a-248">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="37e6a-249">Più modelli di route possono essere applicati a un componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-249">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="37e6a-250">Il seguente componente risponde alle richieste per `/BlazorRoute` e `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="37e6a-250">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="37e6a-251">Parametri di route</span><span class="sxs-lookup"><span data-stu-id="37e6a-251">Route parameters</span></span>

<span data-ttu-id="37e6a-252">I componenti possono accettare parametri di route dal modello di route di cui il `@page` direttiva.</span><span class="sxs-lookup"><span data-stu-id="37e6a-252">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="37e6a-253">Il router utilizza i parametri di route per popolare i parametri del componente corrispondente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-253">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="37e6a-254">*RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-254">*RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

<span data-ttu-id="37e6a-255">Non sono supportati i parametri facoltativi, pertanto, due `@page` direttive vengono applicate nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-255">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="37e6a-256">La prima consente al componente senza un parametro di navigazione.</span><span class="sxs-lookup"><span data-stu-id="37e6a-256">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="37e6a-257">La seconda `@page` direttiva accetta il `{text}` parametro di route e assegna il valore per il `Text` proprietà.</span><span class="sxs-lookup"><span data-stu-id="37e6a-257">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="37e6a-258">Ereditarietà di classe di base per un'esperienza "code-behind"</span><span class="sxs-lookup"><span data-stu-id="37e6a-258">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="37e6a-259">File componente (*. cshtml*) combinare il markup HTML e C# codice nello stesso file di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="37e6a-259">Component files (*.cshtml*) mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="37e6a-260">Il `@inherits` direttiva può essere utilizzata per fornire App Razor componenti con un'esperienza "code-behind" che separa il markup componente dal codice per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="37e6a-260">The `@inherits` directive can be used to provide Razor Components apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="37e6a-261">Il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) Mostra come un componente può ereditare una classe di base, `BlazorRocksBase`, per fornire i metodi e proprietà del componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-261">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="37e6a-262">*BlazorRocks.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-262">*BlazorRocks.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

<span data-ttu-id="37e6a-263">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-263">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="37e6a-264">La classe di base deve derivare da `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-264">The base class should derive from `ComponentBase`.</span></span>

## <a name="razor-support"></a><span data-ttu-id="37e6a-265">Supporto Razor</span><span class="sxs-lookup"><span data-stu-id="37e6a-265">Razor support</span></span>

**<span data-ttu-id="37e6a-266">Direttive Razor</span><span class="sxs-lookup"><span data-stu-id="37e6a-266">Razor directives</span></span>**

<span data-ttu-id="37e6a-267">Le direttive Razor vengono visualizzate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-267">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="37e6a-268">Direttiva</span><span class="sxs-lookup"><span data-stu-id="37e6a-268">Directive</span></span> | <span data-ttu-id="37e6a-269">Descrizione</span><span class="sxs-lookup"><span data-stu-id="37e6a-269">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="37e6a-270">\@Funzioni</span><span class="sxs-lookup"><span data-stu-id="37e6a-270">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="37e6a-271">Aggiunge un C# blocco di codice a un componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-271">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="37e6a-272">Implementa un'interfaccia per la classe del componente generato.</span><span class="sxs-lookup"><span data-stu-id="37e6a-272">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="37e6a-273">\@Eredita</span><span class="sxs-lookup"><span data-stu-id="37e6a-273">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="37e6a-274">Fornisce il controllo completo della classe che eredita il componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-274">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="37e6a-275">\@inserire</span><span class="sxs-lookup"><span data-stu-id="37e6a-275">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="37e6a-276">Consente di inserire di servizi di [contenitore di servizi](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="37e6a-276">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="37e6a-277">Per altre informazioni, vedere [Inserimento di dipendenze in visualizzazioni](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="37e6a-277">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="37e6a-278">Specifica un componente del layout.</span><span class="sxs-lookup"><span data-stu-id="37e6a-278">Specifies a layout component.</span></span> <span data-ttu-id="37e6a-279">Componenti di layout vengono utilizzati per evitare incoerenze e la duplicazione del codice.</span><span class="sxs-lookup"><span data-stu-id="37e6a-279">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="37e6a-280">\@Pagina</span><span class="sxs-lookup"><span data-stu-id="37e6a-280">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="37e6a-281">Specifica che il componente deve gestire le richieste direttamente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-281">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="37e6a-282">Il `@page` direttiva può essere specificata con una route e i parametri facoltativi.</span><span class="sxs-lookup"><span data-stu-id="37e6a-282">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="37e6a-283">A differenza di Razor Pages, il `@page` direttiva non deve necessariamente essere la prima direttiva all'inizio del file.</span><span class="sxs-lookup"><span data-stu-id="37e6a-283">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="37e6a-284">Per altre informazioni, vedere [Routing](xref:razor-components/routing).</span><span class="sxs-lookup"><span data-stu-id="37e6a-284">For more information, see [Routing](xref:razor-components/routing).</span></span> |
| [<span data-ttu-id="37e6a-285">\@Utilizzo</span><span class="sxs-lookup"><span data-stu-id="37e6a-285">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="37e6a-286">Aggiunge il C# `using` direttive alla classe del componente generato.</span><span class="sxs-lookup"><span data-stu-id="37e6a-286">Adds the C# `using` directive to the generated component class.</span></span> |
| [<span data-ttu-id="37e6a-287">\@addTagHelper</span><span class="sxs-lookup"><span data-stu-id="37e6a-287">\@addTagHelper</span></span>](xref:mvc/views/razor#tag-helpers) | <span data-ttu-id="37e6a-288">Usare `@addTagHelper` per utilizzare un componente in un assembly diverso rispetto all'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="37e6a-288">Use `@addTagHelper` to use a component in a different assembly than the app's assembly.</span></span> |

**<span data-ttu-id="37e6a-289">Attributi condizionali</span><span class="sxs-lookup"><span data-stu-id="37e6a-289">Conditional attributes</span></span>**

<span data-ttu-id="37e6a-290">Gli attributi vengono visualizzati in modo condizionale in base al valore di .NET.</span><span class="sxs-lookup"><span data-stu-id="37e6a-290">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="37e6a-291">Se il valore è `false` o `null`, non viene eseguito il rendering dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-291">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="37e6a-292">Se il valore è `true`, viene eseguito il rendering dell'attributo ridotta a icona.</span><span class="sxs-lookup"><span data-stu-id="37e6a-292">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="37e6a-293">Nell'esempio riportato di seguito `IsCompleted` determina se `checked` viene eseguito il rendering nel markup del controllo:</span><span class="sxs-lookup"><span data-stu-id="37e6a-293">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="37e6a-294">Se `IsCompleted` è `true`, la casella di controllo è sottoposto a rendering come:</span><span class="sxs-lookup"><span data-stu-id="37e6a-294">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked>
```

<span data-ttu-id="37e6a-295">Se `IsCompleted` è `false`, la casella di controllo è sottoposto a rendering come:</span><span class="sxs-lookup"><span data-stu-id="37e6a-295">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox">
```

**<span data-ttu-id="37e6a-296">Informazioni aggiuntive su Razor</span><span class="sxs-lookup"><span data-stu-id="37e6a-296">Additional information on Razor</span></span>**

<span data-ttu-id="37e6a-297">Per altre informazioni su Razor, vedere la [riferimento alla sintassi Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="37e6a-297">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="37e6a-298">HTML non elaborato</span><span class="sxs-lookup"><span data-stu-id="37e6a-298">Raw HTML</span></span>

<span data-ttu-id="37e6a-299">Le stringhe vengono in genere viene eseguito il rendering usando i nodi di testo DOM, il che significa che qualsiasi tag che possono contenere viene ignorata e trattata come testo letterale.</span><span class="sxs-lookup"><span data-stu-id="37e6a-299">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="37e6a-300">Per eseguire il rendering HTML non elaborato, eseguire il wrapping del contenuto HTML in un `MarkupString` valore.</span><span class="sxs-lookup"><span data-stu-id="37e6a-300">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="37e6a-301">Il valore viene analizzato come HTML o SVG e inserito nel DOM.</span><span class="sxs-lookup"><span data-stu-id="37e6a-301">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="37e6a-302">Il rendering HTML non elaborato costruito da qualsiasi non attendibile l'origine è un **rischio per la sicurezza** e deve essere evitata.</span><span class="sxs-lookup"><span data-stu-id="37e6a-302">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="37e6a-303">Nell'esempio seguente viene illustrato l'utilizzo di `MarkupString` tipo da aggiungere un blocco di contenuto HTML statico all'output del rendering di un componente:</span><span class="sxs-lookup"><span data-stu-id="37e6a-303">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="37e6a-304">Componenti basati su modelli</span><span class="sxs-lookup"><span data-stu-id="37e6a-304">Templated components</span></span>

<span data-ttu-id="37e6a-305">Componenti basati su modelli sono componenti che accettano uno o più modelli dell'interfaccia utente come parametri, che quindi possono essere utilizzati come parte della logica di rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-305">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="37e6a-306">Componenti basati su modelli consentono di modificare i componenti di livello superiore che sono più riutilizzabili più componenti regolari.</span><span class="sxs-lookup"><span data-stu-id="37e6a-306">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="37e6a-307">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="37e6a-307">A couple of examples include:</span></span>

* <span data-ttu-id="37e6a-308">Un componente di tabella che consente agli utenti di specificare i modelli per l'intestazione della tabella, righe e nel piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="37e6a-308">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="37e6a-309">Un componente di elenco che consente agli utenti di specificare un modello per il rendering degli elementi in un elenco.</span><span class="sxs-lookup"><span data-stu-id="37e6a-309">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="37e6a-310">Parametri di modelli</span><span class="sxs-lookup"><span data-stu-id="37e6a-310">Template parameters</span></span>

<span data-ttu-id="37e6a-311">Un componente basato su modelli viene definito specificando uno o più parametri del componente di tipo `RenderFragment` o `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="37e6a-311">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="37e6a-312">Un frammento di rendering rappresenta un segmento dell'interfaccia utente che viene eseguito il rendering dal componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-312">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="37e6a-313">Facoltativamente, un frammento di rendering accetta un parametro che può essere specificato quando viene richiamato il frammento di rendering.</span><span class="sxs-lookup"><span data-stu-id="37e6a-313">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="37e6a-314">*Components/TableTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-314">*Components/TableTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="37e6a-315">Quando si usa un componente basato su modelli, i parametri del modello possono essere specificati usando gli elementi figlio che corrispondono ai nomi dei parametri (`TableHeader` e `RowTemplate` nell'esempio seguente):</span><span class="sxs-lookup"><span data-stu-id="37e6a-315">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="37e6a-316">Parametri di contesto del modello</span><span class="sxs-lookup"><span data-stu-id="37e6a-316">Template context parameters</span></span>

<span data-ttu-id="37e6a-317">Gli argomenti di componente di tipo `RenderFragment<T>` passato come elementi hanno un parametro implicito denominato `context` (ad esempio dall'esempio di codice precedente, `@context.PetId`), ma è possibile modificare il nome di parametro usando il `Context` attributo sull'elemento figlio elemento.</span><span class="sxs-lookup"><span data-stu-id="37e6a-317">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="37e6a-318">Nell'esempio seguente, il `RowTemplate` dell'elemento `Context` attributo specifica il `pet` parametro:</span><span class="sxs-lookup"><span data-stu-id="37e6a-318">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="37e6a-319">In alternativa, è possibile specificare il `Context` attributo sull'elemento component.</span><span class="sxs-lookup"><span data-stu-id="37e6a-319">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="37e6a-320">L'oggetto specificato `Context` attributo viene applicato a tutti i parametri di modello specificato.</span><span class="sxs-lookup"><span data-stu-id="37e6a-320">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="37e6a-321">Ciò risulta utile quando si desidera specificare il nome di parametro del contenuto per il contenuto figlio implicita (senza wrapping qualsiasi elemento figlio).</span><span class="sxs-lookup"><span data-stu-id="37e6a-321">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="37e6a-322">Nell'esempio seguente, il `Context` attributo incluso nella `TableTemplate` elemento e si applica a tutti i parametri di modello:</span><span class="sxs-lookup"><span data-stu-id="37e6a-322">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="37e6a-323">Componenti tipizzate generiche</span><span class="sxs-lookup"><span data-stu-id="37e6a-323">Generic-typed components</span></span>

<span data-ttu-id="37e6a-324">Componenti basati su modelli sono spesso in modo generico tipizzati.</span><span class="sxs-lookup"><span data-stu-id="37e6a-324">Templated components are often generically typed.</span></span> <span data-ttu-id="37e6a-325">Ad esempio, un componente di modello di visualizzazione elenco generico è utilizzabile per eseguire il rendering `IEnumerable<T>` valori.</span><span class="sxs-lookup"><span data-stu-id="37e6a-325">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="37e6a-326">Per definire un componente generico, usare il `@typeparam` direttiva per specificare i parametri di tipo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-326">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="37e6a-327">*Components/ListViewTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-327">*Components/ListViewTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

<span data-ttu-id="37e6a-328">Quando si usano componenti tipizzate generiche, è possibile che il parametro di tipo viene dedotto se possibile:</span><span class="sxs-lookup"><span data-stu-id="37e6a-328">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="37e6a-329">In caso contrario, il parametro di tipo deve essere specificato in modo esplicito utilizzando un attributo che corrisponde al nome del parametro di tipo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-329">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="37e6a-330">Nell'esempio seguente, `TItem="Pet"` specifica il tipo:</span><span class="sxs-lookup"><span data-stu-id="37e6a-330">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="37e6a-331">I parametri e valori di propagazione</span><span class="sxs-lookup"><span data-stu-id="37e6a-331">Cascading values and parameters</span></span>

<span data-ttu-id="37e6a-332">In alcuni scenari, è poco pratica per propagare dati da un componente predecessore in un componente discendente utilizzando [parametri del componente](#component-parameters), soprattutto quando sono presenti diversi livelli di componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-332">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="37e6a-333">Propagazione di valori e parametri di risolvono questo problema fornendo un modo pratico per un componente predecessore fornire un valore per tutti i suoi componenti discendente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-333">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="37e6a-334">Propagazione di valori e parametri forniscono anche un approccio per i componenti per il coordinamento.</span><span class="sxs-lookup"><span data-stu-id="37e6a-334">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="37e6a-335">Esempio di tema</span><span class="sxs-lookup"><span data-stu-id="37e6a-335">Theme example</span></span>

<span data-ttu-id="37e6a-336">Di seguito *tema* esempio di app di esempio, il `ThemeInfo` classe specifica le informazioni sul tema per flusso verso il basso della gerarchia di componenti in modo che tutti i pulsanti all'interno di una determinata parte dell'app di condividere lo stesso stile.</span><span class="sxs-lookup"><span data-stu-id="37e6a-336">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="37e6a-337">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-337">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="37e6a-338">Un componente predecessore è possibile fornire un valore a catena mediante il componente di valore a catena.</span><span class="sxs-lookup"><span data-stu-id="37e6a-338">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="37e6a-339">Il componente di propagazione valore esegue il wrapping di un sottoalbero della gerarchia di componente e viene fornito un valore singolo per tutti i componenti all'interno di quel sottoalbero.</span><span class="sxs-lookup"><span data-stu-id="37e6a-339">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="37e6a-340">Ad esempio, l'app di esempio specifica le informazioni sul tema (`ThemeInfo`) in uno dei layout dell'app come un parametro di propagazione per tutti i componenti che costituiscono il corpo di layout del `@Body` proprietà.</span><span class="sxs-lookup"><span data-stu-id="37e6a-340">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> `ButtonClass` <span data-ttu-id="37e6a-341">viene assegnato un valore di `btn-success` nel componente del layout.</span><span class="sxs-lookup"><span data-stu-id="37e6a-341">is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="37e6a-342">Qualsiasi componente discendenti può utilizzare questa proprietà tramite la `ThemeInfo` oggetto CSS.</span><span class="sxs-lookup"><span data-stu-id="37e6a-342">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="37e6a-343">*Shared/CascadingValuesParametersLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-343">*Shared/CascadingValuesParametersLayout.cshtml*:</span></span>

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

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="37e6a-344">Per rendere utilizzare valori di propagazione, i componenti dichiarare i parametri di propagazione tramite la `[CascadingParameter]` attributo o basata su un valore di nome di stringa:</span><span class="sxs-lookup"><span data-stu-id="37e6a-344">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="37e6a-345">Associazione con un valore di nome di stringa è rilevante se si hanno più valori di propagazione dello stesso tipo ed è necessario per differenziarle all'interno del sottoalbero stesso.</span><span class="sxs-lookup"><span data-stu-id="37e6a-345">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="37e6a-346">Valori di propagazione sono associati a parametri di propagazione per tipo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-346">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="37e6a-347">Nelle app di esempio, il componente di propagazione valori parametri tema viene associato ai `ThemeInfo` valore CSS per un parametro di propagazione.</span><span class="sxs-lookup"><span data-stu-id="37e6a-347">In the sample app, the Cascading Values Parameters Theme component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="37e6a-348">Il parametro viene utilizzato per impostare la classe CSS per uno dei pulsanti visualizzati dal componente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-348">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="37e6a-349">*Pages/CascadingValuesParametersTheme.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-349">*Pages/CascadingValuesParametersTheme.cshtml*:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="37e6a-350">Esempio TabSet</span><span class="sxs-lookup"><span data-stu-id="37e6a-350">TabSet example</span></span>

<span data-ttu-id="37e6a-351">I parametri di propagazione consentono inoltre ai componenti di collaborare tra la gerarchia di componenti.</span><span class="sxs-lookup"><span data-stu-id="37e6a-351">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="37e6a-352">Ad esempio, tenere presente quanto segue *TabSet* esempio nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="37e6a-352">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="37e6a-353">L'app di esempio ha un `ITab` interfaccia che implementano con schede:</span><span class="sxs-lookup"><span data-stu-id="37e6a-353">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="37e6a-354">Il componente di propagazione valori parametri TabSet utilizza il componente impostato scheda, che contiene diversi componenti di scheda:</span><span class="sxs-lookup"><span data-stu-id="37e6a-354">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="37e6a-355">I componenti di scheda figlio in modo esplicito non vengono passati come parametri per impostare la scheda.</span><span class="sxs-lookup"><span data-stu-id="37e6a-355">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="37e6a-356">Al contrario, i componenti di scheda figlio fanno parte del contenuto figlio di un Set di scheda.</span><span class="sxs-lookup"><span data-stu-id="37e6a-356">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="37e6a-357">Tuttavia, impostare la scheda ancora dovrebbe conoscere su ogni componente di scheda in modo che possa eseguire il rendering di intestazioni e la scheda attiva. Per consentire tale coordinamento senza richiedere codice aggiuntivo, il componente scheda impostata *stesso può fornire come valore di propagazione* che viene quindi prelevata dai componenti di scheda discendenti.</span><span class="sxs-lookup"><span data-stu-id="37e6a-357">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="37e6a-358">*Components/TabSet.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-358">*Components/TabSet.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="37e6a-359">L'acquisizione di componenti scheda discendente che lo contiene scheda impostata come un parametro di propagazione, in modo che i componenti di scheda aggiungono scheda Imposta le coordinate nella quale scheda è attivo.</span><span class="sxs-lookup"><span data-stu-id="37e6a-359">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="37e6a-360">*Components/Tab.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-360">*Components/Tab.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="37e6a-361">Modelli Razor</span><span class="sxs-lookup"><span data-stu-id="37e6a-361">Razor templates</span></span>

<span data-ttu-id="37e6a-362">Eseguire il rendering di frammenti possono essere definiti utilizzando la sintassi dei modelli Razor.</span><span class="sxs-lookup"><span data-stu-id="37e6a-362">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="37e6a-363">I modelli Razor sono un modo per definire un frammento di codice dell'interfaccia utente e si presuppone il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="37e6a-363">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="37e6a-364">Nell'esempio seguente viene illustrato come specificare `RenderFragment` e `RenderFragment<T>` valori.</span><span class="sxs-lookup"><span data-stu-id="37e6a-364">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="37e6a-365">*RazorTemplates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37e6a-365">*RazorTemplates.cshtml*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="37e6a-366">Eseguire il rendering frammenti definiti usando Razor modelli possono essere passati come argomenti per componenti basati su modelli o sottoposto a rendering direttamente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-366">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="37e6a-367">Ad esempio, i modelli precedenti sono direttamente il rendering con il markup Razor seguente:</span><span class="sxs-lookup"><span data-stu-id="37e6a-367">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="37e6a-368">Output sottoposto a rendering:</span><span class="sxs-lookup"><span data-stu-id="37e6a-368">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="37e6a-369">Per la logica RenderTreeBuilder manuale</span><span class="sxs-lookup"><span data-stu-id="37e6a-369">Manual RenderTreeBuilder logic</span></span>

`Microsoft.AspNetCore.Components.RenderTree` <span data-ttu-id="37e6a-370">fornisce metodi per la modifica di componenti e dagli elementi, inclusa la creazione di componenti manualmente in C# codice.</span><span class="sxs-lookup"><span data-stu-id="37e6a-370">provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="37e6a-371">Uso di `RenderTreeBuilder` per creare componenti è uno scenario avanzato.</span><span class="sxs-lookup"><span data-stu-id="37e6a-371">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="37e6a-372">Un componente in formato non valido (ad esempio, un tag di markup non chiusi) può causare un comportamento indefinito.</span><span class="sxs-lookup"><span data-stu-id="37e6a-372">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="37e6a-373">Si consideri il seguente componente di dettagli Pet (*PetDetails.razor* nei componenti di Razor; *PetDetails.cshtml* in Blazor), che può essere compilato manualmente in un altro componente:</span><span class="sxs-lookup"><span data-stu-id="37e6a-373">Consider the following Pet Details component (*PetDetails.razor* in Razor Components; *PetDetails.cshtml* in Blazor), which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="37e6a-374">Nell'esempio seguente, al ciclo impostata nel `CreateComponent` metodo genera tre componenti dettagli Pet.</span><span class="sxs-lookup"><span data-stu-id="37e6a-374">In the following example, the loop in the `CreateComponent` method generates three Pet Details components.</span></span> <span data-ttu-id="37e6a-375">Quando si chiama `RenderTreeBuilder` metodi per creare i componenti (`OpenComponent` e `AddAttribute`), numeri di sequenza sono numeri di riga di codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="37e6a-375">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="37e6a-376">I componenti di Razor algoritmo differenza si basa sui numeri di sequenza corrispondenti alle righe distinte del codice, distinct non chiamare le chiamate.</span><span class="sxs-lookup"><span data-stu-id="37e6a-376">The Razor Components difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="37e6a-377">Quando si crea un componente con `RenderTreeBuilder` metodi, impostare come hardcoded gli argomenti per i numeri di sequenza.</span><span class="sxs-lookup"><span data-stu-id="37e6a-377">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> **<span data-ttu-id="37e6a-378">Utilizzo di un calcolo o un contatore per generare il numero di sequenza può causare una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="37e6a-378">Using a calculation or counter to generate the sequence number can lead to poor performance.</span></span>**

<span data-ttu-id="37e6a-379">Componente compilato (*BuiltContent.razor* nei componenti di Razor; *BuiltContent.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="37e6a-379">Built component (*BuiltContent.razor* in Razor Components; *BuiltContent.cshtml* in Blazor):</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
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
