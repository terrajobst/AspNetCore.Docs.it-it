---
title: Interoperabilità ASP.NET Core Blazor JavaScript
author: guardrex
description: Informazioni su come richiamare funzioni JavaScript da metodi .NET e .NET da JavaScript in app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: c4f2444b60fc2d3a8af893df379cf62636a7bdd5
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213363"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="3049b-103">Interoperabilità ASP.NET Core Blazor JavaScript</span><span class="sxs-lookup"><span data-stu-id="3049b-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="3049b-104">Di [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3049b-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3049b-105">Un'app Blazor può richiamare funzioni JavaScript da metodi .NET e .NET dal codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3049b-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="3049b-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3049b-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="3049b-107">Richiamare funzioni JavaScript da metodi .NET</span><span class="sxs-lookup"><span data-stu-id="3049b-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="3049b-108">In alcuni casi è necessario che il codice .NET chiami una funzione JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3049b-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="3049b-109">Ad esempio, una chiamata JavaScript può esporre le funzionalità o le funzionalità del browser da una libreria JavaScript all'app.</span><span class="sxs-lookup"><span data-stu-id="3049b-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="3049b-110">Questo scenario è denominato *interoperabilità JavaScript (interoperabilità* *JS*).</span><span class="sxs-lookup"><span data-stu-id="3049b-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="3049b-111">Per chiamare JavaScript da .NET, usare l'astrazione `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="3049b-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="3049b-112">Per rilasciare chiamate di interoperabilità JS, inserire l'astrazione `IJSRuntime` nel componente.</span><span class="sxs-lookup"><span data-stu-id="3049b-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="3049b-113">Il metodo `InvokeAsync<T>` accetta un identificatore per la funzione JavaScript che si vuole richiamare insieme a un numero qualsiasi di argomenti serializzabili in JSON.</span><span class="sxs-lookup"><span data-stu-id="3049b-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="3049b-114">L'identificatore della funzione è relativo all'ambito globale (`window`).</span><span class="sxs-lookup"><span data-stu-id="3049b-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="3049b-115">Se si desidera chiamare `window.someScope.someFunction`, l'identificatore viene `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="3049b-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="3049b-116">Non è necessario registrare la funzione prima che venga chiamata.</span><span class="sxs-lookup"><span data-stu-id="3049b-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="3049b-117">Il tipo restituito `T` deve anche essere serializzabile in JSON.</span><span class="sxs-lookup"><span data-stu-id="3049b-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="3049b-118">`T` deve corrispondere al tipo .NET che meglio esegue il mapping al tipo JSON restituito.</span><span class="sxs-lookup"><span data-stu-id="3049b-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="3049b-119">Per le app di Blazor server con il prerendering abilitato, non è possibile chiamare JavaScript durante il prerendering iniziale.</span><span class="sxs-lookup"><span data-stu-id="3049b-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="3049b-120">È necessario rinviare le chiamate di interoperabilità JavaScript finché non viene stabilita la connessione con il browser.</span><span class="sxs-lookup"><span data-stu-id="3049b-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="3049b-121">Per ulteriori informazioni, vedere la sezione [rilevare quando un'app Blazor è prerendering](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="3049b-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="3049b-122">L'esempio seguente è basato su [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un decodificatore basato su JavaScript sperimentale.</span><span class="sxs-lookup"><span data-stu-id="3049b-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="3049b-123">Nell'esempio viene illustrato come richiamare una funzione JavaScript da un C# metodo.</span><span class="sxs-lookup"><span data-stu-id="3049b-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="3049b-124">La funzione JavaScript accetta una matrice di byte da C# un metodo, decodifica la matrice e restituisce il testo al componente per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3049b-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="3049b-125">All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript che usa `TextDecoder` per decodificare una matrice passata e restituire il valore decodificato:</span><span class="sxs-lookup"><span data-stu-id="3049b-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="3049b-126">Il codice JavaScript, ad esempio il codice illustrato nell'esempio precedente, può anche essere caricato da un file JavaScript (con*estensione js*) con un riferimento al file script:</span><span class="sxs-lookup"><span data-stu-id="3049b-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="3049b-127">Il componente seguente:</span><span class="sxs-lookup"><span data-stu-id="3049b-127">The following component:</span></span>

* <span data-ttu-id="3049b-128">Richiama la funzione JavaScript `convertArray` usando `JSRuntime` quando viene selezionato un pulsante del componente (**Convert Array**).</span><span class="sxs-lookup"><span data-stu-id="3049b-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="3049b-129">Dopo la chiamata della funzione JavaScript, la matrice passata viene convertita in una stringa.</span><span class="sxs-lookup"><span data-stu-id="3049b-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="3049b-130">La stringa viene restituita al componente per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3049b-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="3049b-131">Uso di IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="3049b-131">Use of IJSRuntime</span></span>

<span data-ttu-id="3049b-132">Per usare l'astrazione `IJSRuntime`, adottare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="3049b-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="3049b-133">Inserire l'astrazione del `IJSRuntime` nel componente Razor (*Razor*):</span><span class="sxs-lookup"><span data-stu-id="3049b-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="3049b-134">All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="3049b-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="3049b-135">La funzione viene chiamata con `IJSRuntime.InvokeVoidAsync` e non restituisce un valore:</span><span class="sxs-lookup"><span data-stu-id="3049b-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="3049b-136">Inserire l'astrazione `IJSRuntime` in una classe ( *. cs*):</span><span class="sxs-lookup"><span data-stu-id="3049b-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="3049b-137">All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="3049b-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="3049b-138">La funzione viene chiamata con `JSRuntime.InvokeAsync` e restituisce un valore:</span><span class="sxs-lookup"><span data-stu-id="3049b-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="3049b-139">Per la generazione di contenuto dinamico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), usare l'attributo `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="3049b-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="3049b-140">Nell'app di esempio lato client che accompagna questo argomento sono disponibili due funzioni JavaScript per l'app che interagiscono con il DOM per ricevere l'input dell'utente e visualizzare un messaggio di benvenuto:</span><span class="sxs-lookup"><span data-stu-id="3049b-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="3049b-141">`showPrompt` &ndash; genera un messaggio di richiesta per accettare l'input dell'utente (il nome dell'utente) e restituisce il nome al chiamante.</span><span class="sxs-lookup"><span data-stu-id="3049b-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="3049b-142">`displayWelcome` &ndash; assegna un messaggio di benvenuto dal chiamante a un oggetto DOM con un `id` di `welcome`.</span><span class="sxs-lookup"><span data-stu-id="3049b-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="3049b-143">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="3049b-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="3049b-144">Inserire il tag `<script>` che fa riferimento al file JavaScript nel file *wwwroot/index.html* (Blazor webassembly) o nel file *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="3049b-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="3049b-145">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="3049b-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="3049b-146">*Pagine/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="3049b-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="3049b-147">Non inserire un tag `<script>` in un file di componente perché il tag `<script>` non può essere aggiornato dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="3049b-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="3049b-148">I metodi .NET interoperano con le funzioni JavaScript nel file *exampleJsInterop. js* chiamando `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="3049b-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="3049b-149">La `IJSRuntime` astrazione è asincrona per consentire scenari Blazor server.</span><span class="sxs-lookup"><span data-stu-id="3049b-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="3049b-150">Se l'app è un'app webassembly Blazor e si vuole richiamare una funzione JavaScript in modo sincrono, abbattuti per `IJSInProcessRuntime` e chiamare invece `Invoke<T>`.</span><span class="sxs-lookup"><span data-stu-id="3049b-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="3049b-151">È consigliabile che la maggior parte delle librerie di interoperabilità JS usi le API asincrone per assicurarsi che le librerie siano disponibili in tutti gli scenari.</span><span class="sxs-lookup"><span data-stu-id="3049b-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="3049b-152">L'app di esempio include un componente per dimostrare l'interoperabilità JS.</span><span class="sxs-lookup"><span data-stu-id="3049b-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="3049b-153">Il componente:</span><span class="sxs-lookup"><span data-stu-id="3049b-153">The component:</span></span>

* <span data-ttu-id="3049b-154">Riceve l'input dell'utente tramite un prompt di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3049b-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="3049b-155">Restituisce il testo al componente per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="3049b-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="3049b-156">Chiama una seconda funzione JavaScript che interagisce con il DOM per visualizzare un messaggio di benvenuto.</span><span class="sxs-lookup"><span data-stu-id="3049b-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="3049b-157">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3049b-157">*Pages/JSInterop.razor*:</span></span>

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

1. <span data-ttu-id="3049b-158">Quando `TriggerJsPrompt` viene eseguita selezionando il pulsante di **richiesta trigger JavaScript** del componente, viene chiamata la funzione JavaScript `showPrompt` specificata nel file *wwwroot/exampleJsInterop. js* .</span><span class="sxs-lookup"><span data-stu-id="3049b-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="3049b-159">La funzione `showPrompt` accetta l'input dell'utente (il nome dell'utente), che è codificato in HTML e restituito al componente.</span><span class="sxs-lookup"><span data-stu-id="3049b-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="3049b-160">Il componente archivia il nome dell'utente in una variabile locale, `name`.</span><span class="sxs-lookup"><span data-stu-id="3049b-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="3049b-161">La stringa archiviata in `name` viene incorporata in un messaggio di benvenuto, che viene passato a una funzione JavaScript, `displayWelcome`, che esegue il rendering del messaggio di benvenuto in un tag di intestazione.</span><span class="sxs-lookup"><span data-stu-id="3049b-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="3049b-162">Chiamare una funzione JavaScript void</span><span class="sxs-lookup"><span data-stu-id="3049b-162">Call a void JavaScript function</span></span>

<span data-ttu-id="3049b-163">Le funzioni JavaScript che restituiscono [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) vengono chiamate con `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="3049b-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="3049b-164">Rileva quando è in fase di prerendering un'app Blazor</span><span class="sxs-lookup"><span data-stu-id="3049b-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="3049b-165">Acquisisci riferimenti a elementi</span><span class="sxs-lookup"><span data-stu-id="3049b-165">Capture references to elements</span></span>

<span data-ttu-id="3049b-166">Alcuni scenari di interoperabilità JS richiedono riferimenti agli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="3049b-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="3049b-167">Ad esempio, una libreria dell'interfaccia utente può richiedere un riferimento a un elemento per l'inizializzazione o potrebbe essere necessario chiamare API simili a quelle di un elemento, ad esempio `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="3049b-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="3049b-168">Acquisire i riferimenti agli elementi HTML in un componente usando l'approccio seguente:</span><span class="sxs-lookup"><span data-stu-id="3049b-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="3049b-169">Aggiungere un attributo `@ref` all'elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="3049b-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="3049b-170">Definire un campo di tipo `ElementReference` il cui nome corrisponde al valore dell'attributo `@ref`.</span><span class="sxs-lookup"><span data-stu-id="3049b-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="3049b-171">Nell'esempio seguente viene illustrata l'acquisizione di un riferimento all'elemento `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="3049b-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="3049b-172">Usare solo un riferimento a un elemento per mutare il contenuto di un elemento vuoto che non interagisce con Blazor.</span><span class="sxs-lookup"><span data-stu-id="3049b-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="3049b-173">Questo scenario è utile quando un'API di terze parti fornisce contenuto all'elemento.</span><span class="sxs-lookup"><span data-stu-id="3049b-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="3049b-174">Poiché Blazor non interagisce con l'elemento, non è possibile che si verifichi un conflitto tra la rappresentazione Blazordell'elemento e il DOM.</span><span class="sxs-lookup"><span data-stu-id="3049b-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="3049b-175">Nell'esempio seguente è *pericoloso* modificare il contenuto dell'elenco non ordinato (`ul`) perché Blazor interagisce con il Dom per popolare gli elementi elenco di questo elemento (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="3049b-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="3049b-176">Se l'interoperabilità JS modifica il contenuto del `MyList` di elementi e Blazor tenta di applicare le differenze all'elemento, le differenze non corrispondono al DOM.</span><span class="sxs-lookup"><span data-stu-id="3049b-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="3049b-177">Per quanto riguarda il codice .NET, un `ElementReference` è un handle opaco.</span><span class="sxs-lookup"><span data-stu-id="3049b-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="3049b-178">L' *unica* cosa che è possibile fare con `ElementReference` è passarla al codice JavaScript tramite l'interoperabilità js.</span><span class="sxs-lookup"><span data-stu-id="3049b-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="3049b-179">Quando si esegue questa operazione, il codice sul lato JavaScript riceve un'istanza `HTMLElement`, che può essere usata con le API DOM normali.</span><span class="sxs-lookup"><span data-stu-id="3049b-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="3049b-180">Il codice seguente, ad esempio, definisce un metodo di estensione .NET che consente di impostare lo stato attivo su un elemento:</span><span class="sxs-lookup"><span data-stu-id="3049b-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="3049b-181">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="3049b-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="3049b-182">Per chiamare una funzione JavaScript che non restituisce un valore, usare `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="3049b-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="3049b-183">Il codice seguente imposta lo stato attivo sull'input del nome utente chiamando la funzione JavaScript precedente con la `ElementReference`acquisita:</span><span class="sxs-lookup"><span data-stu-id="3049b-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="3049b-184">Per usare un metodo di estensione, creare un metodo di estensione statico che riceva l'istanza di `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="3049b-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="3049b-185">Il metodo `Focus` viene chiamato direttamente nell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="3049b-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="3049b-186">Nell'esempio seguente si presuppone che il metodo `Focus` sia disponibile dallo spazio dei nomi `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="3049b-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="3049b-187">La variabile `username` viene popolata solo dopo il rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="3049b-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="3049b-188">Se un `ElementReference` non popolato viene passato al codice JavaScript, il codice JavaScript riceve un valore di `null`.</span><span class="sxs-lookup"><span data-stu-id="3049b-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="3049b-189">Per modificare i riferimenti agli elementi dopo che il componente ha terminato il rendering (per impostare lo stato attivo iniziale su un elemento), usare i metodi del ciclo di vita dei [componenti OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="3049b-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="3049b-190">Quando si utilizzano tipi generici e si restituisce un valore, utilizzare [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="3049b-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="3049b-191">`GenericMethod` viene chiamato direttamente sull'oggetto con un tipo.</span><span class="sxs-lookup"><span data-stu-id="3049b-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="3049b-192">Nell'esempio seguente si presuppone che il `GenericMethod` sia disponibile dallo spazio dei nomi `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="3049b-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="3049b-193">Elementi di riferimento tra i componenti</span><span class="sxs-lookup"><span data-stu-id="3049b-193">Reference elements across components</span></span>

<span data-ttu-id="3049b-194">Un `ElementReference` è garantito valido solo nel metodo di `OnAfterRender` di un componente (e un riferimento a un elemento è un `struct`), pertanto non è possibile passare un riferimento a un elemento tra i componenti.</span><span class="sxs-lookup"><span data-stu-id="3049b-194">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="3049b-195">Affinché un componente padre renda disponibile un riferimento a un elemento per altri componenti, il componente padre può:</span><span class="sxs-lookup"><span data-stu-id="3049b-195">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="3049b-196">Consenti ai componenti figlio di registrare i callback.</span><span class="sxs-lookup"><span data-stu-id="3049b-196">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="3049b-197">Richiamare i callback registrati durante l'evento `OnAfterRender` con il riferimento all'elemento passato.</span><span class="sxs-lookup"><span data-stu-id="3049b-197">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="3049b-198">Indirettamente, questo approccio consente ai componenti figlio di interagire con il riferimento all'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="3049b-198">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="3049b-199">Nell'esempio di Blazor webassembly riportato di seguito viene illustrato l'approccio.</span><span class="sxs-lookup"><span data-stu-id="3049b-199">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="3049b-200">Nel `<head>` di *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="3049b-200">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="3049b-201">Nel `<body>` di *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="3049b-201">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="3049b-202">*Pages/index. Razor* (componente padre):</span><span class="sxs-lookup"><span data-stu-id="3049b-202">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="3049b-203">*Pages/index. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="3049b-203">*Pages/Index.razor.cs*:</span></span>

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool _disposing;
        private IList<IObserver<ElementReference>> _subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference _title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnNext(_title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            _disposing = true;

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            _subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (_disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            _subscriptions.Add(observer);

            return new Subscription(observer, this);
        }

        private class Subscription : IDisposable
        {
            public Subscription(IObserver<ElementReference> observer, Index self)
            {
                Observer = observer;
                Self = self;
            }

            public IObserver<ElementReference> Observer { get; }
            public Index Self { get; }

            public void Dispose()
            {
                Self._subscriptions.Remove(Observer);
            }
        }
    }
}
```

<span data-ttu-id="3049b-204">*Shared/SurveyPrompt. Razor* (componente figlio):</span><span class="sxs-lookup"><span data-stu-id="3049b-204">*Shared/SurveyPrompt.razor* (child component):</span></span>

```razor
@inject IJSRuntime JS

<div class="alert alert-secondary mt-4" role="alert">
    <span class="oi oi-pencil mr-2" aria-hidden="true"></span>
    <strong>@Title</strong>

    <span class="text-nowrap">
        Please take our
        <a target="_blank" class="font-weight-bold" 
            href="https://go.microsoft.com/fwlink/?linkid=2109206">brief survey</a>
    </span>
    and tell us what you think.
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

<span data-ttu-id="3049b-205">*Shared/SurveyPrompt. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="3049b-205">*Shared/SurveyPrompt.razor.cs*:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable _subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (_subscription != null)
            {
                _subscription.Dispose();
            }

            _subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            _subscription = null;
        }

        public void OnError(Exception error)
        {
            _subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            _subscription?.Dispose();
        }
    }
}
```

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="3049b-206">Richiamare metodi .NET da funzioni JavaScript</span><span class="sxs-lookup"><span data-stu-id="3049b-206">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="3049b-207">Chiamata al metodo .NET statico</span><span class="sxs-lookup"><span data-stu-id="3049b-207">Static .NET method call</span></span>

<span data-ttu-id="3049b-208">Per richiamare un metodo .NET statico da JavaScript, usare le funzioni `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="3049b-208">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="3049b-209">Passare l'identificatore del metodo statico che si desidera chiamare, il nome dell'assembly che contiene la funzione e gli eventuali argomenti.</span><span class="sxs-lookup"><span data-stu-id="3049b-209">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="3049b-210">La versione asincrona è preferibile per supportare Blazor scenari server.</span><span class="sxs-lookup"><span data-stu-id="3049b-210">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="3049b-211">Il metodo .NET deve essere pubblico, statico e avere l'attributo `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="3049b-211">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="3049b-212">La chiamata ai metodi generici aperti non è attualmente supportata.</span><span class="sxs-lookup"><span data-stu-id="3049b-212">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="3049b-213">L'app di esempio include C# un metodo per restituire una matrice di `int`s.</span><span class="sxs-lookup"><span data-stu-id="3049b-213">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="3049b-214">L'attributo `JSInvokable` viene applicato al metodo.</span><span class="sxs-lookup"><span data-stu-id="3049b-214">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="3049b-215">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3049b-215">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="3049b-216">JavaScript servito al client richiama il C# metodo .NET.</span><span class="sxs-lookup"><span data-stu-id="3049b-216">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="3049b-217">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="3049b-217">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="3049b-218">Quando si seleziona il pulsante **trigger .net static method ReturnArrayAsync** , esaminare l'output della console negli strumenti di sviluppo Web del browser.</span><span class="sxs-lookup"><span data-stu-id="3049b-218">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="3049b-219">L'output della console è:</span><span class="sxs-lookup"><span data-stu-id="3049b-219">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="3049b-220">Il quarto valore della matrice viene inserito nella matrice (`data.push(4);`) restituito da `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="3049b-220">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="3049b-221">Per impostazione predefinita, l'identificatore del metodo è il nome del metodo, ma è possibile specificare un identificatore diverso usando il costruttore `JSInvokableAttribute`:</span><span class="sxs-lookup"><span data-stu-id="3049b-221">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="3049b-222">Nel file JavaScript sul lato client:</span><span class="sxs-lookup"><span data-stu-id="3049b-222">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a><span data-ttu-id="3049b-223">Chiamata al metodo di istanza</span><span class="sxs-lookup"><span data-stu-id="3049b-223">Instance method call</span></span>

<span data-ttu-id="3049b-224">È anche possibile chiamare i metodi di istanza .NET da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3049b-224">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="3049b-225">Per richiamare un metodo di istanza .NET da JavaScript:</span><span class="sxs-lookup"><span data-stu-id="3049b-225">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="3049b-226">Passare l'istanza di .NET a JavaScript eseguendone il wrapping in un'istanza di `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="3049b-226">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="3049b-227">L'istanza .NET viene passata per riferimento a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3049b-227">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="3049b-228">Richiamare i metodi di istanza .NET sull'istanza utilizzando le funzioni `invokeMethod` o `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="3049b-228">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="3049b-229">L'istanza di .NET può essere passata anche come argomento quando si richiamano altri metodi .NET da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3049b-229">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="3049b-230">L'app di esempio consente di registrare i messaggi nella console lato client.</span><span class="sxs-lookup"><span data-stu-id="3049b-230">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="3049b-231">Per gli esempi seguenti illustrati dall'app di esempio, esaminare l'output della console del browser negli strumenti di sviluppo del browser.</span><span class="sxs-lookup"><span data-stu-id="3049b-231">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="3049b-232">Quando il pulsante del **metodo di istanza .NET del trigger HelloHelper. sayHello** è selezionato, `ExampleJsInterop.CallHelloHelperSayHello` viene chiamato e passa un nome, `Blazor`, al metodo.</span><span class="sxs-lookup"><span data-stu-id="3049b-232">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="3049b-233">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3049b-233">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="3049b-234">`CallHelloHelperSayHello` richiama la funzione JavaScript `sayHello` con una nuova istanza di `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="3049b-234">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="3049b-235">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="3049b-235">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="3049b-236">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="3049b-236">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="3049b-237">Il nome viene passato al costruttore `HelloHelper`, che imposta la proprietà `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="3049b-237">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="3049b-238">Quando viene eseguita la funzione JavaScript `sayHello`, `HelloHelper.SayHello` restituisce il messaggio `Hello, {Name}!`, che viene scritto nella console dalla funzione JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3049b-238">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="3049b-239">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="3049b-239">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="3049b-240">Output della console negli strumenti di sviluppo Web del browser:</span><span class="sxs-lookup"><span data-stu-id="3049b-240">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="3049b-241">Condividere il codice di interoperabilità in una libreria di classi</span><span class="sxs-lookup"><span data-stu-id="3049b-241">Share interop code in a class library</span></span>

<span data-ttu-id="3049b-242">Il codice di interoperabilità JS può essere incluso in una libreria di classi, che consente di condividere il codice in un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="3049b-242">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="3049b-243">La libreria di classi gestisce l'incorporamento delle risorse JavaScript nell'assembly compilato.</span><span class="sxs-lookup"><span data-stu-id="3049b-243">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="3049b-244">I file JavaScript vengono inseriti nella cartella *wwwroot*</span><span class="sxs-lookup"><span data-stu-id="3049b-244">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="3049b-245">Gli strumenti si occupano di incorporare le risorse quando la libreria viene compilata.</span><span class="sxs-lookup"><span data-stu-id="3049b-245">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="3049b-246">Nel file di progetto dell'app viene fatto riferimento al pacchetto NuGet compilato nello stesso modo in cui viene fatto riferimento a un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="3049b-246">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="3049b-247">Dopo il ripristino del pacchetto, il codice dell'app può chiamare JavaScript come se fosse C#.</span><span class="sxs-lookup"><span data-stu-id="3049b-247">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="3049b-248">Per altre informazioni, vedere <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="3049b-248">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="3049b-249">Chiamate di interoperabilità JS di Harden</span><span class="sxs-lookup"><span data-stu-id="3049b-249">Harden JS interop calls</span></span>

<span data-ttu-id="3049b-250">L'interoperabilità JS potrebbe non riuscire a causa di errori di rete e deve essere considerata inaffidabile.</span><span class="sxs-lookup"><span data-stu-id="3049b-250">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="3049b-251">Per impostazione predefinita, un'app di Blazor server timeout le chiamate di interoperabilità JS nel server dopo un minuto.</span><span class="sxs-lookup"><span data-stu-id="3049b-251">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="3049b-252">Se un'app può tollerare un timeout più aggressivo, ad esempio 10 secondi, impostare il timeout usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="3049b-252">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="3049b-253">A livello globale in `Startup.ConfigureServices`, specificare il timeout:</span><span class="sxs-lookup"><span data-stu-id="3049b-253">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="3049b-254">Per chiamata nel codice componente, una singola chiamata può specificare il timeout:</span><span class="sxs-lookup"><span data-stu-id="3049b-254">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="3049b-255">Per ulteriori informazioni sull'esaurimento delle risorse, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="3049b-255">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a><span data-ttu-id="3049b-256">Eseguire trasferimenti di dati di grandi dimensioni nelle app Blazor server</span><span class="sxs-lookup"><span data-stu-id="3049b-256">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="3049b-257">In alcuni scenari, è necessario trasferire grandi quantità di dati tra JavaScript e Blazor.</span><span class="sxs-lookup"><span data-stu-id="3049b-257">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="3049b-258">Generalmente, i trasferimenti di dati di grandi dimensioni si verificano quando:</span><span class="sxs-lookup"><span data-stu-id="3049b-258">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="3049b-259">Le API del browser file system vengono usate per caricare o scaricare un file.</span><span class="sxs-lookup"><span data-stu-id="3049b-259">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="3049b-260">È necessaria l'interoperabilità con una libreria di terze parti.</span><span class="sxs-lookup"><span data-stu-id="3049b-260">Interop with a third party library is required.</span></span>

<span data-ttu-id="3049b-261">In Blazor server è prevista una limitazione per impedire il passaggio di singoli messaggi di grandi dimensioni che potrebbero causare problemi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="3049b-261">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="3049b-262">Quando si sviluppa codice che trasferisce i dati tra JavaScript e Blazor, tenere presenti le linee guida seguenti:</span><span class="sxs-lookup"><span data-stu-id="3049b-262">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="3049b-263">Sezionare i dati in parti più piccole e inviare i segmenti di dati in sequenza fino a quando non vengono ricevuti tutti i dati dal server.</span><span class="sxs-lookup"><span data-stu-id="3049b-263">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="3049b-264">Non allocare oggetti di grandi dimensioni C# in JavaScript e codice.</span><span class="sxs-lookup"><span data-stu-id="3049b-264">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="3049b-265">Non bloccare il thread principale dell'interfaccia utente per periodi prolungati durante l'invio o la ricezione di dati.</span><span class="sxs-lookup"><span data-stu-id="3049b-265">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="3049b-266">Liberare la memoria utilizzata quando il processo viene completato o annullato.</span><span class="sxs-lookup"><span data-stu-id="3049b-266">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="3049b-267">Per motivi di sicurezza, applicare i seguenti requisiti aggiuntivi:</span><span class="sxs-lookup"><span data-stu-id="3049b-267">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="3049b-268">Dichiarare le dimensioni massime di file o dati che è possibile passare.</span><span class="sxs-lookup"><span data-stu-id="3049b-268">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="3049b-269">Dichiarare la velocità di caricamento minima dal client al server.</span><span class="sxs-lookup"><span data-stu-id="3049b-269">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="3049b-270">Dopo la ricezione dei dati da parte del server, i dati possono essere:</span><span class="sxs-lookup"><span data-stu-id="3049b-270">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="3049b-271">Archiviati temporaneamente in un buffer di memoria fino a quando non vengono raccolti tutti i segmenti.</span><span class="sxs-lookup"><span data-stu-id="3049b-271">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="3049b-272">Utilizzato immediatamente.</span><span class="sxs-lookup"><span data-stu-id="3049b-272">Consumed immediately.</span></span> <span data-ttu-id="3049b-273">Ad esempio, i dati possono essere archiviati immediatamente in un database o scritti su disco durante la ricezione di ogni segmento.</span><span class="sxs-lookup"><span data-stu-id="3049b-273">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="3049b-274">La classe del caricatore di file seguente gestisce l'interoperabilità JS con il client.</span><span class="sxs-lookup"><span data-stu-id="3049b-274">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="3049b-275">La classe Uploader usa l'interoperabilità JS per:</span><span class="sxs-lookup"><span data-stu-id="3049b-275">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="3049b-276">Eseguire il polling del client per inviare un segmento di dati.</span><span class="sxs-lookup"><span data-stu-id="3049b-276">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="3049b-277">Interrompere la transazione se si verifica il timeout del polling.</span><span class="sxs-lookup"><span data-stu-id="3049b-277">Abort the transaction if polling times out.</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

<span data-ttu-id="3049b-278">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="3049b-278">In the preceding example:</span></span>

* <span data-ttu-id="3049b-279">Il `_maxBase64SegmentSize` è impostato su `8192`, che viene calcolato da `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span><span class="sxs-lookup"><span data-stu-id="3049b-279">The `_maxBase64SegmentSize` is set to `8192`, which is calculated from `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="3049b-280">Le API di gestione della memoria .NET Core di basso livello vengono usate per archiviare i segmenti di memoria nel server `_uploadedSegments`.</span><span class="sxs-lookup"><span data-stu-id="3049b-280">Low level .NET Core memory management APIs are used to store the memory segments on the server in `_uploadedSegments`.</span></span>
* <span data-ttu-id="3049b-281">Viene usato un metodo di `ReceiveFile` per gestire il caricamento tramite l'interoperabilità JS:</span><span class="sxs-lookup"><span data-stu-id="3049b-281">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="3049b-282">Le dimensioni del file vengono determinate in byte tramite l'interoperabilità JS con `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span><span class="sxs-lookup"><span data-stu-id="3049b-282">The file size is determined in bytes through JS interop with `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="3049b-283">Il numero di segmenti da ricevere viene calcolato e archiviato in `numberOfSegments`.</span><span class="sxs-lookup"><span data-stu-id="3049b-283">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="3049b-284">I segmenti sono richiesti in un ciclo `for` tramite l'interoperabilità JS con `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span><span class="sxs-lookup"><span data-stu-id="3049b-284">The segments are requested in a `for` loop through JS interop with `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="3049b-285">Tutti i segmenti ma l'ultimo devono essere 8.192 byte prima della decodifica.</span><span class="sxs-lookup"><span data-stu-id="3049b-285">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="3049b-286">Il client è forzato a inviare i dati in modo efficiente.</span><span class="sxs-lookup"><span data-stu-id="3049b-286">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="3049b-287">Per ogni segmento ricevuto, i controlli vengono eseguiti prima della decodifica con <xref:System.Convert.TryFromBase64String*>.</span><span class="sxs-lookup"><span data-stu-id="3049b-287">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String*>.</span></span>
  * <span data-ttu-id="3049b-288">Un flusso con i dati viene restituito come nuovo <xref:System.IO.Stream> (`SegmentedStream`) dopo il completamento del caricamento.</span><span class="sxs-lookup"><span data-stu-id="3049b-288">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="3049b-289">La classe del flusso segmentato espone l'elenco di segmenti come <xref:System.IO.Stream>di sola lettura non ricercabile:</span><span class="sxs-lookup"><span data-stu-id="3049b-289">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

<span data-ttu-id="3049b-290">Il codice seguente implementa le funzioni JavaScript per ricevere i dati:</span><span class="sxs-lookup"><span data-stu-id="3049b-290">The following code implements JavaScript functions to receive the data:</span></span>

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```

## <a name="additional-resources"></a><span data-ttu-id="3049b-291">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3049b-291">Additional resources</span></span>

* [<span data-ttu-id="3049b-292">Esempio di InteropComponent. Razor (repository GitHub DotNet/AspNetCore, Branch versione 3,1)</span><span class="sxs-lookup"><span data-stu-id="3049b-292">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
