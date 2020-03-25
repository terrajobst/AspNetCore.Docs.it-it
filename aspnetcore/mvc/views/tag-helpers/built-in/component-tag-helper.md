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
# <a name="component-tag-helper-in-aspnet-core"></a>Helper tag di componente in ASP.NET Core

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

Per eseguire il rendering di un componente da una pagina o da una vista, usare l' [Helper Tag Component](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).

L'helper tag di componente seguente esegue il rendering del componente `Counter` in una pagina o in una vista:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

L'helper Tag Component può anche passare parametri ai componenti. Si consideri il componente `ColorfulCheckbox` seguente che imposta il colore e le dimensioni dell'etichetta della casella di controllo:

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

I [parametri del componente](xref:blazor/components#component-parameters) `Size` (`int`) e `Color` (`string`) possono essere impostati dall'helper tag dei componenti:

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

Viene eseguito il rendering del codice HTML seguente nella pagina o nella visualizzazione:

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

Il passaggio di una stringa racchiusa tra virgolette richiede un' [espressione Razor esplicita](xref:mvc/views/razor#explicit-razor-expressions), come illustrato nell'esempio precedente `param-Color`. Il comportamento di analisi Razor per un valore di tipo `string` non si applica a un attributo `param-*` perché l'attributo è un tipo di `object`.

Il tipo di parametro deve essere serializzabile JSON, che in genere significa che il tipo deve avere un costruttore predefinito e le proprietà impostabili. È ad esempio possibile specificare un valore per `Size` e `Color` nell'esempio precedente perché i tipi di `Size` e `Color` sono tipi primitivi (`int` e `string`), supportati dal serializzatore JSON.

<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configura se il componente:

* Viene preeseguito nella pagina.
* Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazor dall'agente utente.

| Modalità di rendering | Descrizione |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor. Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | Esegue il rendering di un marcatore per un'app Server Blazor. L'output del componente non è incluso. Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | Esegue il rendering del componente in HTML statico. |

Mentre le pagine e le visualizzazioni possono usare i componenti, il contrario non è vero. I componenti non possono utilizzare funzionalità specifiche di visualizzazione e pagina, ad esempio visualizzazioni parziali e sezioni. Per usare la logica da una visualizzazione parziale in un componente, scomporre la logica di visualizzazione parziale in un componente.

Il rendering dei componenti server da una pagina HTML statica non è supportato.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
