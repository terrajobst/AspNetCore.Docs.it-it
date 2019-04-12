---
title: Interoperabilità JavaScript componenti Razor
author: guardrex
description: Informazioni su come richiamare le funzioni JavaScript da .NET e .NET metodi da JavaScript nelle App Blazor e componenti di Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: f2588f4ed1ec2f01218283625fae4632d0a8ae58
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515551"
---
# <a name="razor-components-javascript-interop"></a>Interoperabilità JavaScript componenti Razor

Dal [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), e [Luke Latham](https://github.com/guardrex)

Un'app Razor componenti può richiamare funzioni di JavaScript da .NET e .NET metodi dal codice JavaScript.

## <a name="invoke-javascript-functions-from-net-methods"></a>Richiamare funzioni JavaScript dai metodi di .NET

Esistono casi in cui è necessario chiamare una funzione JavaScript codice .NET. Ad esempio, una chiamata JavaScript può esporre le funzionalità del browser o la funzionalità da una libreria JavaScript per l'app.

Per chiamare JavaScript da .NET, usare il `IJSRuntime` astrazione. Il `InvokeAsync<T>` metodo accetta un identificatore per la funzione JavaScript che si desidera richiamare insieme a qualsiasi numero di argomenti serializzabile in JSON. L'identificatore della funzione è relativo all'ambito globale (`window`). Se si desidera chiamare `window.someScope.someFunction`, l'identificatore è `someScope.someFunction`. Non è necessario registrare la funzione di prima che venga chiamato. Il tipo restituito `T` deve anche essere JSON serializzabile.

Per le app ASP.NET Core Razor componenti lato server:

* Più richieste utente vengono elaborate dall'app sul lato server. Non chiamare `JSRuntime.Current` in un componente per richiamare le funzioni JavaScript.
* Inserire il `IJSRuntime` astrazione e usare l'oggetto inserito per eseguire chiamate di interoperabilità JavaScript.

Nell'esempio seguente si basa sul [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un decodificatore sperimentale basate su JavaScript. Nell'esempio viene illustrato come richiamare una funzione JavaScript da un C# metodo. La funzione JavaScript accetta una matrice di byte da un C# metodo, consente di decodificare la matrice e restituisce il testo al componente per la visualizzazione.

All'interno di `<head>` elemento della *wwwroot/index.html*, fornire una funzione che usa `TextDecoder` per decodificare una matrice passata:

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

Il codice JavaScript, ad esempio il codice illustrato nell'esempio precedente, può anche essere caricato da un file JavaScript (*js*) con un riferimento al file di script nel *wwwroot/index.html* file:

```html
<script src="exampleJsInterop.js"></script>
```

Il seguente componente:

* Richiama il `ConvertArray` per le funzioni JavaScript usando `JsRuntime` quando un pulsante di componente (**convertire matrice**) sia selezionata.
* Dopo la chiamata di funzione JavaScript, la matrice passata viene convertita in una stringa. La stringa viene restituita al componente per la visualizzazione.

```cshtml
@page "/"
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    private async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

Usare il `IJSRuntime` astrazione, adottare uno degli approcci seguenti:

* Inserire il `IJSRuntime` astrazione nel file Razor (*.razor*, *cshtml*):

  ```cshtml
  @inject IJSRuntime JSRuntime

  @functions {
      public override void OnInit()
      {
          StocksService.OnStockTickerUpdated += stockUpdate =>
          {
              JSRuntime.InvokeAsync<object>(
                  "handleTickerChanged",
                  stockUpdate.symbol,
                  stockUpdate.price);
          };
      }
  }
  ```

* Inserire il `IJSRuntime` astrazione in una classe (*cs*):

  ```csharp
  public class JsInteropClasses
  {
      private readonly IJSRuntime _jsRuntime;

      public JsInteropClasses(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public Task<string> TickerChanged(string data)
      {
          // The handleTickerChanged JavaScript method is implemented
          // in a JavaScript file, such as 'wwwroot/tickerJsInterop.js'.
          return _jsRuntime.InvokeAsync<object>(
              "handleTickerChanged",
              stockUpdate.symbol,
              stockUpdate.price);
      }
  }
  ```

* Per la generazione del contenuto dinamica con `BuildRenderTree`, usare il `[Inject]` attributo:

  ```csharp
  [Inject] IJSRuntime JSRuntime { get; set; }
  ```

Nell'app di esempio dal lato client che accompagna questo argomento, due funzioni JavaScript sono disponibili per l'app sul lato client che interagiscono con il modello DOM per ricevere l'input dell'utente e visualizzare un messaggio di benvenuto:

* `showPrompt` &ndash; Produce una richiesta per accettare l'input utente (il nome dell'utente) e restituisce il nome al chiamante.
* `displayWelcome` &ndash; Assegna un messaggio di benvenuto dal chiamante a un oggetto DOM con un `id` di `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Sul posto di `<script>` tag che fa riferimento al file JavaScript nel *wwwroot/index.html* file:

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

Non inserire un `<script>` tag in un file del componente perché il `<script>` tag non può essere aggiornato in modo dinamico.

Funzioni di interoperabilità di metodi .NET con il codice JavaScript nel *exampleJsInterop.js* file chiamando `IJSRuntime.InvokeAsync<T>`.

Il `IJSRuntime` astrazione è asincrona per consentire scenari server-side. Se l'app viene eseguita lato client e si vuole richiamare in modo sincrono, una funzione JavaScript per eseguire il downcast `IJSInProcessRuntime` e chiamare `Invoke<T>` invece. È consigliabile che la maggior parte delle librerie di interoperabilità JavaScript usano l'API asincrona per verificare che le librerie sono disponibili in tutti gli scenari, dal lato client o lato server.

L'app di esempio include un componente per illustrare l'interoperabilità di JavaScript. Il componente:

* Riceve l'input utente tramite un prompt dei comandi di JavaScript.
* Restituisce il testo per il componente per l'elaborazione.
* Chiama una seconda funzione JavaScript che interagisce con il modello DOM per visualizzare un messaggio di benvenuto.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Quando `TriggerJsPrompt` viene eseguito selezionando il componente **Trigger JavaScript Prompt** pulsante, il codice JavaScript `showPrompt` funzione fornita nel *wwwroot/exampleJsInterop.js* file è viene chiamato.
1. Il `showPrompt` funzione accetta l'input dell'utente (il nome dell'utente), che è codificata in formato HTML e viene restituita al componente. Il componente archivia il nome dell'utente in una variabile locale, `name`.
1. La stringa archiviata in `name` viene incorporato in un messaggio di benvenuto, che viene passato a una funzione JavaScript, `displayWelcome`, che esegue il rendering il messaggio di benvenuto in un tag di intestazione.

## <a name="capture-references-to-elements"></a>Acquisire i riferimenti agli elementi

Alcuni [interoperabilità JavaScript](xref:razor-components/javascript-interop) scenari richiedono riferimenti agli elementi HTML. Ad esempio, una libreria dell'interfaccia utente può richiedere un riferimento all'elemento per l'inizializzazione o potrebbe essere necessario chiamare le API di comandi simili in un elemento, ad esempio `focus` o `play`.

È possibile acquisire i riferimenti agli elementi HTML in un componente tramite l'aggiunta di un `ref` dell'attributo all'elemento HTML e quindi definendo un campo di tipo `ElementRef` il cui nome corrisponde al valore della `ref` attributo.

L'esempio seguente illustra l'acquisizione di un riferimento all'elemento di input di nome utente:

```cshtml
<input ref="username" ...>

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Effettuare **non** utilizzare riferimenti a elementi acquisiti come modalità di popolamento di DOM. In questo modo può interferire con il modello dichiarativo per il rendering.

Per quanto riguarda il codice .NET, un `ElementRef` è un handle opaco. Il *soltanto* cosa puoi fare con `ElementRef` è passarlo al codice JavaScript tramite l'interoperabilità di JavaScript. Quando si esegue questa operazione, il codice JavaScript lato riceve un `HTMLElement` istanza, che è possibile usare con le API DOM normale.

Ad esempio, il codice seguente definisce un metodo di estensione .NET che consente di impostare lo stato attivo su un elemento:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Uso `IJSRuntime.InvokeAsync<T>` e chiamare `exampleJsFunctions.focusElement` con un `ElementRef` per concentrare l'attenzione di un elemento:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.cshtml?highlight=1,3,7,11-12)]

Per usare un metodo di estensione per concentrare l'attenzione di un elemento, creare un metodo di estensione statici che riceve il `IJSRuntime` istanza:

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Il metodo viene chiamato direttamente nell'oggetto. Nell'esempio seguente si presuppone che il metodo statico `Focus` è disponibile dal metodo di `JsInteropClasses` dello spazio dei nomi:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.cshtml?highlight=1,4,8,12)]

> [!IMPORTANT]
> Il `username` variabile viene popolata solo dopo che il componente viene eseguito il rendering e l'output include il `>` elemento. Se si prova a passare un non popolata `ElementRef` al codice JavaScript, riceve il codice JavaScript `null`. Per modificare i riferimenti agli elementi dopo il componente ha completato il rendering (per impostare lo stato attivo iniziale in un elemento) usare la `OnAfterRenderAsync` oppure `OnAfterRender` [i metodi del ciclo di vita del componente](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Richiamare i metodi .NET da funzioni di JavaScript

### <a name="static-net-method-call"></a>Chiamata al metodo statico .NET

Per richiamare un metodo statico di .NET da JavaScript, usare il `DotNet.invokeMethod` o `DotNet.invokeMethodAsync` funzioni. Passare l'identificatore del metodo statico da chiamare, il nome dell'assembly contenente la funzione e tutti gli argomenti. La versione asincrona è preferibile per supportare scenari server-side. Per essere richiamabili da JavaScript, il metodo .NET deve essere pubblici, statici e decorato con `[JSInvokable]`. Per impostazione predefinita, l'identificatore del metodo è il nome del metodo, ma è possibile specificare un identificatore differente utilizzando la `JSInvokableAttribute` costruttore. Chiamata di metodi generici aperti non è attualmente supportata.

L'app di esempio include un C# per restituire una matrice di `int`s. Il metodo è decorato con il `JSInvokable` attributo.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop2&highlight=7-11)]

Richiama JavaScript servita al client di C# metodo .NET.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Quando la **metodo statico Trigger .NET ReturnArrayAsync** è selezionato, esaminare l'output della console negli strumenti di sviluppo del browser web.

L'output della console è:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Il quarto valore di matrice viene inserito nella matrice (`data.push(4);`) restituito da `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Chiamata di metodo di istanza

È anche possibile chiamare i metodi di istanza di .NET da JavaScript. Per richiamare un metodo di istanza .NET da JavaScript, passare innanzitutto l'istanza di .NET a JavaScript mediante il wrapping in un `DotNetObjectRef` istanza. L'istanza di .NET viene passato per riferimento a JavaScript ed è possibile richiamare i metodi di istanza .NET di istanza utilizzando il `invokeMethod` o `invokeMethodAsync` funzioni. L'istanza di .NET può anche essere passato come argomento quando si chiamano altri metodi di .NET da JavaScript.

> [!NOTE]
> L'app di esempio Registra i messaggi nella console del client. Per gli esempi seguenti dimostrati dall'app di esempio, esaminare l'output di console del browser negli strumenti di sviluppo del browser.

Quando la **metodo di istanza Trigger .NET HelloHelper.SayHello** pulsante è selezionato, `ExampleJsInterop.CallHelloHelperSayHello` viene chiamato e passa un nome, `Blazor`, al metodo.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` richiama la funzione JavaScript `sayHello` con una nuova istanza della `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Il nome è passato a `HelloHelper`del costruttore che imposta il `HelloHelper.Name` proprietà. Quando la funzione JavaScript `sayHello` viene eseguito `HelloHelper.SayHello` restituisce il `Hello, {Name}!` messaggio, che viene scritto nella console per la funzione JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Console di output negli strumenti di sviluppo del browser web:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Condividi codice in una libreria di classi Razor componente di interoperabilità

Codice di interoperabilità JavaScript può essere incluso in una libreria di classi Razor componente (`dotnet new razorclasslib`), che consente di condividere il codice in un pacchetto NuGet.

La libreria di classi Razor componente gestisce le risorse JavaScript incorporare nell'assembly compilato. I file JavaScript vengono inseriti nel *wwwroot* cartella. Gli strumenti si occupa di incorporamento delle risorse quando la libreria viene compilata.

Il pacchetto NuGet compilato viene fatto riferimento nel file di progetto dell'app, esattamente come qualsiasi normale pacchetto NuGet viene fatto riferimento. Dopo aver ripristinato l'app, codice dell'app può effettuare chiamate nel JavaScript come se fosse C#.

Per altre informazioni, vedere <xref:razor-components/class-libraries>.
