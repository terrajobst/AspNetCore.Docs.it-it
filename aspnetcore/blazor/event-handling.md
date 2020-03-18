---
title: Gestione degli eventi di Blazor ASP.NET Core
author: guardrex
description: Informazioni sulle funzionalità di gestione degli eventi di Blazor, inclusi i tipi di argomento degli eventi, i callback degli eventi e la gestione degli eventi predefiniti del browser.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/event-handling
ms.openlocfilehash: c144841805e07a136f153c25a78c7f9af7c5801b
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511366"
---
# <a name="aspnet-core-blazor-event-handling"></a>Gestione degli eventi di ASP.NET Core Blazer

Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)

I componenti Razor forniscono funzionalità di gestione degli eventi. Per un attributo dell'elemento HTML denominato [`@on{EVENT}`](xref:mvc/views/razor#onevent) (ad esempio `@onclick`) con un valore tipizzato dal delegato, un componente Razor considera il valore dell'attributo come un gestore eventi.

Il codice seguente chiama il metodo `UpdateHeading` quando si seleziona il pulsante nell'interfaccia utente:

```razor
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

Il codice seguente chiama il metodo `CheckChanged` quando la casella di controllo viene modificata nell'interfaccia utente:

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

I gestori di eventi possono anche essere asincroni e restituire un <xref:System.Threading.Tasks.Task>. Non è necessario chiamare manualmente [StateHasChanged](xref:blazor/lifecycle#state-changes). Le eccezioni vengono registrate quando si verificano.

Nell'esempio seguente `UpdateHeading` viene chiamato in modo asincrono quando si seleziona il pulsante:

```razor
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

## <a name="event-argument-types"></a>Tipi di argomenti dell'evento

Per alcuni eventi, i tipi di argomento dell'evento sono consentiti. La specifica di un tipo di evento nella chiamata al metodo è necessaria solo se il tipo di evento viene usato nel metodo.

I `EventArgs` supportati sono riportati nella tabella seguente.

| Evento            | Classe                | Eventi e note DOM |
| ---------------- | -------------------- | -------------------- |
| Appunti        | `ClipboardEventArgs` | `oncut`, `oncopy`, `onpaste` |
| Trascinare             | `DragEventArgs`      | `ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`<br><br>`DataTransfer` e `DataTransferItem` contengono dati di elementi trascinati. |
| Errore            | `ErrorEventArgs`     | `onerror` |
| Evento            | `EventArgs`          | *Generale*<br>`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`<br><br>*Appunti*<br>`onbeforecut`, `onbeforecopy`, `onbeforepaste`<br><br>*Input*<br>`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`<br><br>*Supporti*<br>`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting` |
| Focus            | `FocusEventArgs`     | `onfocus`, `onblur`, `onfocusin`, `onfocusout`<br><br>Non include il supporto per `relatedTarget`. |
| Input            | `ChangeEventArgs`    | `onchange`, `oninput` |
| Tastiera         | `KeyboardEventArgs`  | `onkeydown`, `onkeypress`, `onkeyup` |
| Mouse            | `MouseEventArgs`     | `onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout` |
| Puntatore del mouse    | `PointerEventArgs`   | `onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture` |
| Rotellina del mouse      | `WheelEventArgs`     | `onwheel`, `onmousewheel` |
| Stato         | `ProgressEventArgs`  | `onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout` |
| Tocco            | `TouchEventArgs`     | `ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`<br><br>`TouchPoint` rappresenta un singolo punto di contatto su un dispositivo sensibile al tocco. |

Per ulteriori informazioni, vedere le seguenti risorse:

* [Classi EventArgs nell'origine riferimento ASP.NET Core (ramo DotNet/aspnetcore Release/3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).
* [Documentazione Web MDN: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; include informazioni sugli elementi HTML che supportano ogni evento DOM.

## <a name="lambda-expressions"></a>Espressioni lambda

È inoltre possibile utilizzare le [espressioni lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) :

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Spesso è consigliabile chiudere i valori aggiuntivi, ad esempio quando si esegue l'iterazione su un set di elementi. Nell'esempio seguente vengono creati tre pulsanti, ciascuno dei quali chiama `UpdateHeading` passando un argomento di evento (`MouseEventArgs`) e il relativo numero di pulsante (`buttonNumber`) quando viene selezionato nell'interfaccia utente:

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> **Non** usare la variabile di ciclo (`i`) in un ciclo di `for` direttamente in un'espressione lambda. In caso contrario, la stessa variabile viene utilizzata da tutte le espressioni lambda causando che il valore di `i`sia lo stesso in tutte le espressioni lambda. Acquisire sempre il relativo valore in una variabile locale (`buttonNumber` nell'esempio precedente) e quindi usarlo.

## <a name="eventcallback"></a>EventCallback

Uno scenario comune con i componenti annidati è la volontà di eseguire il metodo di un componente padre quando si verifica un evento del componente figlio&mdash;ad esempio quando si verifica un evento `onclick` nell'elemento figlio. Per esporre gli eventi tra i componenti, usare un `EventCallback`. Un componente padre può assegnare un metodo di callback a un `EventCallback`di un componente figlio.

Il `ChildComponent` nell'app di esempio (*Components/ChildComponent. Razor*) illustra come viene configurato il gestore di `onclick` di un pulsante per ricevere un delegato `EventCallback` dal `ParentComponent`dell'esempio. Il `EventCallback` viene digitato con `MouseEventArgs`, appropriato per un evento di `onclick` da un dispositivo periferico:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

Il `ParentComponent` imposta il `EventCallback<T>` (`OnClickCallback`) del figlio sul relativo metodo `ShowMessage`.

*Pages/ParentComponent. Razor*:

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

Quando il pulsante è selezionato nella `ChildComponent`:

* Viene chiamato il metodo di `ShowMessage` del `ParentComponent`. `_messageText` viene aggiornato e visualizzato nella `ParentComponent`.
* Una chiamata a [StateHasChanged](xref:blazor/lifecycle#state-changes) non è obbligatoria nel metodo del callback (`ShowMessage`). `StateHasChanged` viene chiamato automaticamente per eseguire nuovamente il rendering del `ParentComponent`, così come gli eventi figlio attivano il rendering dei componenti nei gestori eventi che vengono eseguiti all'interno dell'elemento figlio.

`EventCallback` e `EventCallback<T>` consentono delegati asincroni. `EventCallback<T>` è fortemente tipizzato e richiede un tipo di argomento specifico. `EventCallback` è debolmente tipizzato e consente qualsiasi tipo di argomento.

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

Richiama un `EventCallback` o `EventCallback<T>` con `InvokeAsync` e attendi il <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Utilizzare `EventCallback` e `EventCallback<T>` per la gestione degli eventi e i parametri del componente di associazione.

Preferisce la `EventCallback<T>` fortemente tipizzata rispetto a `EventCallback`. `EventCallback<T>` fornisce un migliore feedback sugli errori agli utenti del componente. Analogamente ad altri gestori di eventi dell'interfaccia utente, la specifica del parametro dell'evento è facoltativa. Utilizzare `EventCallback` quando non viene passato alcun valore al callback.

## <a name="prevent-default-actions"></a>Impedisci azioni predefinite

Usare l'attributo [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) Directive per impedire l'azione predefinita per un evento.

Quando si seleziona un tasto in un dispositivo di input e lo stato attivo dell'elemento si trova in una casella di testo, in un browser viene in genere visualizzato il carattere della chiave nella casella di testo. Nell'esempio seguente viene impedito il comportamento predefinito specificando l'attributo della direttiva `@onkeypress:preventDefault`. Il contatore viene incrementato e la chiave **+** non viene acquisita nel valore dell'elemento `<input>`:

```razor
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

Specificare l'attributo `@on{EVENT}:preventDefault` senza un valore equivale a `@on{EVENT}:preventDefault="true"`.

Il valore dell'attributo può anche essere un'espressione. Nell'esempio seguente `_shouldPreventDefault` è un `bool` campo impostato su `true` o `false`:

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

Per impedire l'azione predefinita, non è necessario un gestore eventi. Il gestore eventi e impedire scenari di azione predefiniti possono essere usati in modo indipendente.

## <a name="stop-event-propagation"></a>Arresta propagazione eventi

Usare l'attributo della direttiva [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) per arrestare la propagazione degli eventi.

Nell'esempio seguente la selezione della casella di controllo impedisce la propagazione degli eventi click dal secondo `<div>` figlio al `<div>`padre:

```razor
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
