---
title: Chiamare i metodi .NET da funzioni JavaScript in ASP.NET Core Blazor
author: guardrex
description: Informazioni su come richiamare i metodi .NET dalle funzioni JavaScript nelle app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: dbf44fe7923998c65119e42d97c304890fa95523
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218791"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a>Chiamare i metodi .NET da funzioni JavaScript in ASP.NET Core Blazor

Di [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), [Rudrawadi](http://wisne.co)e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Un'app Blazor può richiamare funzioni JavaScript da metodi .NET e metodi .NET da funzioni JavaScript. Questi scenari sono detti *interoperabilità JavaScript (interoperabilità* *JS*).

Questo articolo descrive come richiamare i metodi .NET da JavaScript. Per informazioni su come chiamare funzioni JavaScript da .NET, vedere <xref:blazor/call-javascript-from-dotnet>.

[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="static-net-method-call"></a>Chiamata al metodo .NET statico

Per richiamare un metodo .NET statico da JavaScript, usare le funzioni `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`. Passare l'identificatore del metodo statico che si desidera chiamare, il nome dell'assembly che contiene la funzione e gli eventuali argomenti. La versione asincrona è preferibile per supportare Blazor scenari server. Il metodo .NET deve essere pubblico, statico e avere l'attributo `[JSInvokable]`. La chiamata ai metodi generici aperti non è attualmente supportata.

L'app di esempio include C# un metodo per restituire una matrice di `int`. L'attributo `JSInvokable` viene applicato al metodo.

*Pages/JsInterop. Razor*:

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

JavaScript servito al client richiama il C# metodo .NET.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Quando si seleziona il pulsante **trigger .net static method ReturnArrayAsync** , esaminare l'output della console negli strumenti di sviluppo Web del browser.

L'output della console è:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Il quarto valore della matrice viene inserito nella matrice (`data.push(4);`) restituito da `ReturnArrayAsync`.

Per impostazione predefinita, l'identificatore del metodo è il nome del metodo, ma è possibile specificare un identificatore diverso usando il costruttore `JSInvokableAttribute`:

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

Nel file JavaScript sul lato client:

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

## <a name="instance-method-call"></a>Chiamata al metodo di istanza

È anche possibile chiamare i metodi di istanza .NET da JavaScript. Per richiamare un metodo di istanza .NET da JavaScript:

* Passa l'istanza .NET per riferimento a JavaScript:
  * Eseguire una chiamata statica a `DotNetObjectReference.Create`.
  * Eseguire il wrapping dell'istanza in un'istanza di `DotNetObjectReference` e chiamare `Create` nell'istanza di `DotNetObjectReference`. Eliminare gli oggetti `DotNetObjectReference` (un esempio viene visualizzato più avanti in questa sezione).
* Richiamare i metodi di istanza .NET sull'istanza utilizzando le funzioni `invokeMethod` o `invokeMethodAsync`. L'istanza di .NET può essere passata anche come argomento quando si richiamano altri metodi .NET da JavaScript.

> [!NOTE]
> L'app di esempio consente di registrare i messaggi nella console lato client. Per gli esempi seguenti illustrati dall'app di esempio, esaminare l'output della console del browser negli strumenti di sviluppo del browser.

Quando il pulsante del **metodo di istanza .NET del trigger HelloHelper. sayHello** è selezionato, `ExampleJsInterop.CallHelloHelperSayHello` viene chiamato e passa un nome, `Blazor`, al metodo.

*Pages/JsInterop. Razor*:

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

`CallHelloHelperSayHello` richiama la funzione JavaScript `sayHello` con una nuova istanza di `HelloHelper`.

*JsInteropClasses/ExampleJsInterop. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=11-18)]

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Il nome viene passato al costruttore `HelloHelper`, che imposta la proprietà `HelloHelper.Name`. Quando viene eseguita la funzione JavaScript `sayHello`, `HelloHelper.SayHello` restituisce il messaggio `Hello, {Name}!`, che viene scritto nella console dalla funzione JavaScript.

*JsInteropClasses/HelloHelper. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Output della console negli strumenti di sviluppo Web del browser:

```console
Hello, Blazor!
```

Per evitare una perdita di memoria e consentire Garbage Collection su un componente che crea un `DotNetObjectReference`, adottare uno degli approcci seguenti:

* Eliminare l'oggetto nella classe che ha creato l'istanza di `DotNetObjectReference`:

  ```csharp
  public class ExampleJsInterop : IDisposable
  {
      private readonly IJSRuntime _jsRuntime;
      private DotNetObjectReference<HelloHelper> _objRef;

      public ExampleJsInterop(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public ValueTask<string> CallHelloHelperSayHello(string name)
      {
          _objRef = DotNetObjectReference.Create(new HelloHelper(name));

          return _jsRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              _objRef);
      }

      public void Dispose()
      {
          _objRef?.Dispose();
      }
  }
  ```

  Il modello precedente illustrato nella classe `ExampleJsInterop` può anche essere implementato in un componente:

  ```razor
  @page "/JSInteropComponent"
  @using BlazorSample.JsInteropClasses
  @implements IDisposable
  @inject IJSRuntime JSRuntime

  <h1>JavaScript Interop</h1>

  <button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
      Trigger .NET instance method HelloHelper.SayHello
  </button>

  @code {
      private DotNetObjectReference<HelloHelper> _objRef;

      public async Task TriggerNetInstanceMethod()
      {
          _objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

          await JSRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              _objRef);
      }

      public void Dispose()
      {
          _objRef?.Dispose();
      }
  }
  ```

* Quando il componente o la classe non elimina il `DotNetObjectReference`, eliminare l'oggetto nel client chiamando `.dispose()`:

  ```javascript
  window.myFunction = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'MyMethod');
    dotnetHelper.dispose();
  }
  ```

## <a name="component-instance-method-call"></a>Chiamata al metodo dell'istanza del componente

Per richiamare i metodi .NET di un componente:

* Utilizzare la funzione `invokeMethod` o `invokeMethodAsync` per eseguire una chiamata al metodo statico al componente.
* Il metodo statico del componente esegue il wrapping della chiamata al metodo di istanza come `Action`richiamato.

Nel codice JavaScript lato client:

```javascript
function updateMessageCallerJS() {
  DotNet.invokeMethod('BlazorSample', 'UpdateMessageCaller');
}
```

*Pages/JSInteropComponent. Razor*:

```razor
@page "/JSInteropComponent"

<p>
    Message: @_message
</p>

<p>
    <button onclick="updateMessageCallerJS()">Call JS Method</button>
</p>

@code {
    private static Action _action;
    private string _message = "Select the button.";

    protected override void OnInitialized()
    {
        _action = UpdateMessage;
    }

    private void UpdateMessage()
    {
        _message = "UpdateMessage Called!";
        StateHasChanged();
    }

    [JSInvokable]
    public static void UpdateMessageCaller()
    {
        _action.Invoke();
    }
}
```

Quando sono presenti diversi componenti, ognuno con i metodi di istanza da chiamare, usare una classe helper per richiamare i metodi di istanza (come `Action`s) di ogni componente.

Nell'esempio seguente:

* Il componente `JSInterop` contiene diversi componenti `ListItem`.
* Ogni componente `ListItem` è costituito da un messaggio e da un pulsante.
* Quando si seleziona un pulsante `ListItem` componente, il metodo `UpdateMessage` di `ListItem`modifica il testo dell'elemento dell'elenco e nasconde il pulsante.

*MessageUpdateInvokeHelper.cs*:

```csharp
using System;
using Microsoft.JSInterop;

public class MessageUpdateInvokeHelper
{
    private Action _action;

    public MessageUpdateInvokeHelper(Action action)
    {
        _action = action;
    }

    [JSInvokable("BlazorSample")]
    public void UpdateMessageCaller()
    {
        _action.Invoke();
    }
}
```

Nel codice JavaScript lato client:

```javascript
window.updateMessageCallerJS = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'UpdateMessageCaller');
    dotnetHelper.dispose();
}
```

*Shared/ListItem. Razor*:

```razor
@inject IJSRuntime JsRuntime

<li>
    @_message
    <button @onclick="InteropCall" style="display:@_display">InteropCall</button>
</li>

@code {
    private string _message = "Select one of these list item buttons.";
    private string _display = "inline-block";
    private MessageUpdateInvokeHelper _messageUpdateInvokeHelper;

    protected override void OnInitialized()
    {
        _messageUpdateInvokeHelper = new MessageUpdateInvokeHelper(UpdateMessage);
    }

    protected async Task InteropCall()
    {
        await JsRuntime.InvokeVoidAsync("updateMessageCallerJS",
            DotNetObjectReference.Create(_messageUpdateInvokeHelper));
    }

    private void UpdateMessage()
    {
        _message = "UpdateMessage Called!";
        _display = "none";
        StateHasChanged();
    }
}
```

*Pages/JSInterop. Razor*:

```razor
@page "/JSInterop"

<h1>List of components</h1>

<ul>
    <ListItem />
    <ListItem />
    <ListItem />
    <ListItem />
</ul>
```

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/call-javascript-from-dotnet>
* [Esempio di InteropComponent. Razor (repository GitHub DotNet/AspNetCore, Branch versione 3,1)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [Eseguire trasferimenti di dati di grandi dimensioni nelle app Blazor server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
