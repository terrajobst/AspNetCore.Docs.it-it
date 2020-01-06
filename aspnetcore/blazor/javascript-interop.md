---
title: Interoperabilità ASP.NET Core Blazor JavaScript
author: guardrex
description: Informazioni su come richiamare funzioni JavaScript da metodi .NET e .NET da JavaScript in app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: 2350870f8548a9c8df324182883a105706c12c20
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355741"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>Interoperabilità ASP.NET Core Blazor JavaScript

Di [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Un'app Blazor può richiamare funzioni JavaScript da metodi .NET e .NET dal codice JavaScript.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Richiamare funzioni JavaScript da metodi .NET

In alcuni casi è necessario che il codice .NET chiami una funzione JavaScript. Ad esempio, una chiamata JavaScript può esporre le funzionalità o le funzionalità del browser da una libreria JavaScript all'app. Questo scenario è denominato *interoperabilità JavaScript (interoperabilità* *JS*).

Per chiamare JavaScript da .NET, usare l'astrazione `IJSRuntime`. Per rilasciare chiamate di interoperabilità JS, inserire l'astrazione `IJSRuntime` nel componente. Il metodo `InvokeAsync<T>` accetta un identificatore per la funzione JavaScript che si vuole richiamare insieme a un numero qualsiasi di argomenti serializzabili in JSON. L'identificatore della funzione è relativo all'ambito globale (`window`). Se si desidera chiamare `window.someScope.someFunction`, l'identificatore viene `someScope.someFunction`. Non è necessario registrare la funzione prima che venga chiamata. Il tipo restituito `T` deve anche essere serializzabile in JSON. `T` deve corrispondere al tipo .NET che meglio esegue il mapping al tipo JSON restituito.

Per le app di Blazor server con il prerendering abilitato, non è possibile chiamare JavaScript durante il prerendering iniziale. È necessario rinviare le chiamate di interoperabilità JavaScript finché non viene stabilita la connessione con il browser. Per ulteriori informazioni, vedere la sezione [rilevare quando un'app Blazor è prerendering](#detect-when-a-blazor-app-is-prerendering) .

L'esempio seguente è basato su [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un decodificatore basato su JavaScript sperimentale. Nell'esempio viene illustrato come richiamare una funzione JavaScript da un C# metodo. La funzione JavaScript accetta una matrice di byte da C# un metodo, decodifica la matrice e restituisce il testo al componente per la visualizzazione.

All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript che usa `TextDecoder` per decodificare una matrice passata e restituire il valore decodificato:

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

Il codice JavaScript, ad esempio il codice illustrato nell'esempio precedente, può anche essere caricato da un file JavaScript (con*estensione js*) con un riferimento al file script:

```html
<script src="exampleJsInterop.js"></script>
```

Il componente seguente:

* Richiama la funzione JavaScript `convertArray` usando `JSRuntime` quando viene selezionato un pulsante del componente (**Convert Array**).
* Dopo la chiamata della funzione JavaScript, la matrice passata viene convertita in una stringa. La stringa viene restituita al componente per la visualizzazione.

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a>Uso di IJSRuntime

Per usare l'astrazione `IJSRuntime`, adottare uno degli approcci seguenti:

* Inserire l'astrazione del `IJSRuntime` nel componente Razor (*Razor*):

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript `handleTickerChanged`. La funzione viene chiamata con `IJSRuntime.InvokeVoidAsync` e non restituisce un valore:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* Inserire l'astrazione `IJSRuntime` in una classe ( *. cs*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript `handleTickerChanged`. La funzione viene chiamata con `JSRuntime.InvokeAsync` e restituisce un valore:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* Per la generazione di contenuto dinamico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), usare l'attributo `[Inject]`:

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

Nell'app di esempio lato client che accompagna questo argomento sono disponibili due funzioni JavaScript per l'app che interagiscono con il DOM per ricevere l'input dell'utente e visualizzare un messaggio di benvenuto:

* `showPrompt` &ndash; genera un messaggio di richiesta per accettare l'input dell'utente (il nome dell'utente) e restituisce il nome al chiamante.
* `displayWelcome` &ndash; assegna un messaggio di benvenuto dal chiamante a un oggetto DOM con un `id` di `welcome`.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Inserire il tag `<script>` che fa riferimento al file JavaScript nel file *wwwroot/index.html* (Blazor webassembly) o nel file *pages/_Host. cshtml* (Blazor Server).

*wwwroot/index.html* (Blazor webassembly):

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

*Pagine/_Host. cshtml* (Blazor Server):

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

Non inserire un tag `<script>` in un file di componente perché il tag `<script>` non può essere aggiornato dinamicamente.

I metodi .NET interoperano con le funzioni JavaScript nel file *exampleJsInterop. js* chiamando `IJSRuntime.InvokeAsync<T>`.

La `IJSRuntime` astrazione è asincrona per consentire scenari Blazor server. Se l'app è un'app webassembly Blazor e si vuole richiamare una funzione JavaScript in modo sincrono, abbattuti per `IJSInProcessRuntime` e chiamare invece `Invoke<T>`. È consigliabile che la maggior parte delle librerie di interoperabilità JS usi le API asincrone per assicurarsi che le librerie siano disponibili in tutti gli scenari.

L'app di esempio include un componente per dimostrare l'interoperabilità JS. Il componente:

* Riceve l'input dell'utente tramite un prompt di JavaScript.
* Restituisce il testo al componente per l'elaborazione.
* Chiama una seconda funzione JavaScript che interagisce con il DOM per visualizzare un messaggio di benvenuto.

*Pages/JSInterop. Razor*:

```razor
@page "/JSInterop"
@using BlazorSample.JsInteropClasses
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<h2>Invoke JavaScript functions from .NET methods</h2>

<button type="button" class="btn btn-primary" @onclick="TriggerJsPrompt">
    Trigger JavaScript Prompt
</button>

<h3 id="welcome" style="color:green;font-style:italic"></h3>

@code {
    public async Task TriggerJsPrompt()
    {
        // showPrompt is implemented in wwwroot/exampleJsInterop.js
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");
        // displayWelcome is implemented in wwwroot/exampleJsInterop.js
        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. Quando `TriggerJsPrompt` viene eseguita selezionando il pulsante di **richiesta trigger JavaScript** del componente, viene chiamata la funzione JavaScript `showPrompt` specificata nel file *wwwroot/exampleJsInterop. js* .
1. La funzione `showPrompt` accetta l'input dell'utente (il nome dell'utente), che è codificato in HTML e restituito al componente. Il componente archivia il nome dell'utente in una variabile locale, `name`.
1. La stringa archiviata in `name` viene incorporata in un messaggio di benvenuto, che viene passato a una funzione JavaScript, `displayWelcome`, che esegue il rendering del messaggio di benvenuto in un tag di intestazione.

## <a name="call-a-void-javascript-function"></a>Chiamare una funzione JavaScript void

Le funzioni JavaScript che restituiscono [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) vengono chiamate con `IJSRuntime.InvokeVoidAsync`.

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a>Rileva quando è in fase di prerendering un'app Blazor
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Acquisisci riferimenti a elementi

Alcuni scenari di interoperabilità JS richiedono riferimenti agli elementi HTML. Ad esempio, una libreria dell'interfaccia utente può richiedere un riferimento a un elemento per l'inizializzazione o potrebbe essere necessario chiamare API simili a quelle di un elemento, ad esempio `focus` o `play`.

Acquisire i riferimenti agli elementi HTML in un componente usando l'approccio seguente:

* Aggiungere un attributo `@ref` all'elemento HTML.
* Definire un campo di tipo `ElementReference` il cui nome corrisponde al valore dell'attributo `@ref`.

Nell'esempio seguente viene illustrata l'acquisizione di un riferimento all'elemento `username` `<input>`:

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> Usare solo un riferimento a un elemento per mutare il contenuto di un elemento vuoto che non interagisce con Blazor. Questo scenario è utile quando un'API di terze parti fornisce contenuto all'elemento. Poiché Blazor non interagisce con l'elemento, non è possibile che si verifichi un conflitto tra la rappresentazione Blazordell'elemento e il DOM.
>
> Nell'esempio seguente è *pericoloso* modificare il contenuto dell'elenco non ordinato (`ul`) perché Blazor interagisce con il Dom per popolare gli elementi elenco di questo elemento (`<li>`):
>
> ```razor
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> Se l'interoperabilità JS modifica il contenuto del `MyList` di elementi e Blazor tenta di applicare le differenze all'elemento, le differenze non corrispondono al DOM.

Per quanto riguarda il codice .NET, un `ElementReference` è un handle opaco. L' *unica* cosa che è possibile fare con `ElementReference` è passarla al codice JavaScript tramite l'interoperabilità js. Quando si esegue questa operazione, il codice sul lato JavaScript riceve un'istanza `HTMLElement`, che può essere usata con le API DOM normali.

Il codice seguente, ad esempio, definisce un metodo di estensione .NET che consente di impostare lo stato attivo su un elemento:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Per chiamare una funzione JavaScript che non restituisce un valore, usare `IJSRuntime.InvokeVoidAsync`. Il codice seguente imposta lo stato attivo sull'input del nome utente chiamando la funzione JavaScript precedente con la `ElementReference`acquisita:

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Per usare un metodo di estensione, creare un metodo di estensione statico che riceva l'istanza di `IJSRuntime`:

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Il metodo `Focus` viene chiamato direttamente nell'oggetto. Nell'esempio seguente si presuppone che il metodo `Focus` sia disponibile dallo spazio dei nomi `JsInteropClasses`:

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> La variabile `username` viene popolata solo dopo il rendering del componente. Se un `ElementReference` non popolato viene passato al codice JavaScript, il codice JavaScript riceve un valore di `null`. Per modificare i riferimenti agli elementi dopo che il componente ha terminato il rendering (per impostare lo stato attivo iniziale su un elemento), usare i metodi del ciclo di vita dei [componenti OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).

Quando si utilizzano tipi generici e si restituisce un valore, utilizzare [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

`GenericMethod` viene chiamato direttamente sull'oggetto con un tipo. Nell'esempio seguente si presuppone che il `GenericMethod` sia disponibile dallo spazio dei nomi `JsInteropClasses`:

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="invoke-net-methods-from-javascript-functions"></a>Richiamare metodi .NET da funzioni JavaScript

### <a name="static-net-method-call"></a>Chiamata al metodo .NET statico

Per richiamare un metodo .NET statico da JavaScript, usare le funzioni `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`. Passare l'identificatore del metodo statico che si desidera chiamare, il nome dell'assembly che contiene la funzione e gli eventuali argomenti. La versione asincrona è preferibile per supportare Blazor scenari server. Per richiamare un metodo .NET da JavaScript, il metodo .NET deve essere pubblico, statico e avere l'attributo `[JSInvokable]`. Per impostazione predefinita, l'identificatore del metodo è il nome del metodo, ma è possibile specificare un identificatore diverso usando il costruttore `JSInvokableAttribute`. La chiamata ai metodi generici aperti non è attualmente supportata.

L'app di esempio include C# un metodo per restituire una matrice di `int`s. L'attributo `JSInvokable` viene applicato al metodo.

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

### <a name="instance-method-call"></a>Chiamata al metodo di istanza

È anche possibile chiamare i metodi di istanza .NET da JavaScript. Per richiamare un metodo di istanza .NET da JavaScript:

* Passare l'istanza di .NET a JavaScript eseguendone il wrapping in un'istanza di `DotNetObjectReference`. L'istanza .NET viene passata per riferimento a JavaScript.
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

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Il nome viene passato al costruttore `HelloHelper`, che imposta la proprietà `HelloHelper.Name`. Quando viene eseguita la funzione JavaScript `sayHello`, `HelloHelper.SayHello` restituisce il messaggio `Hello, {Name}!`, che viene scritto nella console dalla funzione JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Output della console negli strumenti di sviluppo Web del browser:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Condividere il codice di interoperabilità in una libreria di classi

Il codice di interoperabilità JS può essere incluso in una libreria di classi, che consente di condividere il codice in un pacchetto NuGet.

La libreria di classi gestisce l'incorporamento delle risorse JavaScript nell'assembly compilato. I file JavaScript vengono inseriti nella cartella *wwwroot* Gli strumenti si occupano di incorporare le risorse quando la libreria viene compilata.

Nel file di progetto dell'app viene fatto riferimento al pacchetto NuGet compilato nello stesso modo in cui viene fatto riferimento a un pacchetto NuGet. Dopo il ripristino del pacchetto, il codice dell'app può chiamare JavaScript come se fosse C#.

Per ulteriori informazioni, vedere <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Chiamate di interoperabilità JS di Harden

L'interoperabilità JS potrebbe non riuscire a causa di errori di rete e deve essere considerata inaffidabile. Per impostazione predefinita, un'app di Blazor server timeout le chiamate di interoperabilità JS nel server dopo un minuto. Se un'app può tollerare un timeout più aggressivo, ad esempio 10 secondi, impostare il timeout usando uno degli approcci seguenti:

* A livello globale in `Startup.ConfigureServices`, specificare il timeout:

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* Per chiamata nel codice componente, una singola chiamata può specificare il timeout:

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

Per ulteriori informazioni sull'esaurimento delle risorse, vedere <xref:security/blazor/server>.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Esempio di InteropComponent. Razor (repository GitHub ASPNET/AspNetCore, Branch versione 3,0)](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
