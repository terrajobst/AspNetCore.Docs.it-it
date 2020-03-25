---
title: Helper tag di componente in ASP.NET Core
author: guardrex
ms.author: riande
description: Informazioni su come usare l'helper Tag componente ASP.NET Core per eseguire il rendering dei componenti Razor in pagine e visualizzazioni.
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 801ceb73de5bb4ef7500624e1fbddbf96d1ab89c
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226396"
---
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="260a6-103">Helper tag di componente in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="260a6-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="260a6-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="260a6-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="260a6-105">Per eseguire il rendering di un componente da una pagina o da una vista, usare l' [Helper Tag Component](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span><span class="sxs-lookup"><span data-stu-id="260a6-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

<span data-ttu-id="260a6-106">L'helper tag di componente seguente esegue il rendering del componente `Counter` in una pagina o in una vista:</span><span class="sxs-lookup"><span data-stu-id="260a6-106">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="260a6-107">L'helper Tag Component può anche passare parametri ai componenti.</span><span class="sxs-lookup"><span data-stu-id="260a6-107">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="260a6-108">Si consideri il componente `ColorfulCheckbox` seguente che imposta il colore e le dimensioni dell'etichetta della casella di controllo:</span><span class="sxs-lookup"><span data-stu-id="260a6-108">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

```razor
<label style="font-size:@(Size)px;color:@Color">
    <input @bind="Value"
           id="survey" 
           name="blazor" 
           type="checkbox" />
    Enjoying Blazor?
</label>

@code {
    [Parameter]
    public bool Value { get; set; }

    [Parameter]
    public int Size { get; set; } = 8;

    [Parameter]
    public string Color { get; set; }

    protected override void OnInitialized()
    {
        Size += 10;
    }
}
```

<span data-ttu-id="260a6-109">I [parametri del componente](xref:blazor/components#component-parameters) `Size` (`int`) e `Color` (`string`) possono essere impostati dall'helper tag dei componenti:</span><span class="sxs-lookup"><span data-stu-id="260a6-109">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="260a6-110">Viene eseguito il rendering del codice HTML seguente nella pagina o nella visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="260a6-110">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="260a6-111">Il passaggio di una stringa racchiusa tra virgolette richiede un' [espressione Razor esplicita](xref:mvc/views/razor#explicit-razor-expressions), come illustrato nell'esempio precedente `param-Color`.</span><span class="sxs-lookup"><span data-stu-id="260a6-111">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="260a6-112">Il comportamento di analisi Razor per un valore di tipo `string` non si applica a un attributo `param-*` perché l'attributo è un tipo di `object`.</span><span class="sxs-lookup"><span data-stu-id="260a6-112">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="260a6-113">Il tipo di parametro deve essere serializzabile JSON, che in genere significa che il tipo deve avere un costruttore predefinito e le proprietà impostabili.</span><span class="sxs-lookup"><span data-stu-id="260a6-113">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="260a6-114">È ad esempio possibile specificare un valore per `Size` e `Color` nell'esempio precedente perché i tipi di `Size` e `Color` sono tipi primitivi (`int` e `string`), supportati dal serializzatore JSON.</span><span class="sxs-lookup"><span data-stu-id="260a6-114">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="260a6-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="260a6-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="260a6-116">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="260a6-116">Is prerendered into the page.</span></span>
* <span data-ttu-id="260a6-117">Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazor dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="260a6-117">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="260a6-118">Modalità di rendering</span><span class="sxs-lookup"><span data-stu-id="260a6-118">Render Mode</span></span> | <span data-ttu-id="260a6-119">Descrizione</span><span class="sxs-lookup"><span data-stu-id="260a6-119">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="260a6-120">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="260a6-120">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="260a6-121">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="260a6-121">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="260a6-122">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="260a6-122">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="260a6-123">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="260a6-123">Output from the component isn't included.</span></span> <span data-ttu-id="260a6-124">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="260a6-124">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="260a6-125">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="260a6-125">Renders the component into static HTML.</span></span> |

<span data-ttu-id="260a6-126">Mentre le pagine e le visualizzazioni possono usare i componenti, il contrario non è vero.</span><span class="sxs-lookup"><span data-stu-id="260a6-126">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="260a6-127">I componenti non possono utilizzare funzionalità specifiche di visualizzazione e pagina, ad esempio visualizzazioni parziali e sezioni.</span><span class="sxs-lookup"><span data-stu-id="260a6-127">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="260a6-128">Per usare la logica da una visualizzazione parziale in un componente, scomporre la logica di visualizzazione parziale in un componente.</span><span class="sxs-lookup"><span data-stu-id="260a6-128">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="260a6-129">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="260a6-129">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="260a6-130">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="260a6-130">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
