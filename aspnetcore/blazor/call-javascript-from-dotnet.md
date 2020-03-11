---
title: Chiamare funzioni JavaScript da metodi .NET in ASP.NET Core Blazor
author: guardrex
description: Informazioni su come richiamare funzioni JavaScript da metodi .NET in app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-javascript-from-dotnet
ms.openlocfilehash: 7a27b6f1be2ef296d5b2b2a4f566e0cdedbe6480
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659712"
---
# <a name="call-javascript-functions-from-net-methods-in-aspnet-core-opno-locblazor"></a><span data-ttu-id="0daba-103">Chiamare funzioni JavaScript da metodi .NET in ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="0daba-103">Call JavaScript functions from .NET methods in ASP.NET Core Blazor</span></span>

<span data-ttu-id="0daba-104">Di [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0daba-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="0daba-105">Un'app Blazor può richiamare funzioni JavaScript da metodi .NET e metodi .NET da funzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0daba-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="0daba-106">Questi scenari sono detti *interoperabilità JavaScript (interoperabilità* *JS*).</span><span class="sxs-lookup"><span data-stu-id="0daba-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="0daba-107">Questo articolo illustra come richiamare funzioni JavaScript da .NET.</span><span class="sxs-lookup"><span data-stu-id="0daba-107">This article covers invoking JavaScript functions from .NET.</span></span> <span data-ttu-id="0daba-108">Per informazioni su come chiamare i metodi .NET da JavaScript, vedere <xref:blazor/call-dotnet-from-javascript>.</span><span class="sxs-lookup"><span data-stu-id="0daba-108">For information on how to call .NET methods from JavaScript, see <xref:blazor/call-dotnet-from-javascript>.</span></span>

<span data-ttu-id="0daba-109">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0daba-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0daba-110">Per chiamare JavaScript da .NET, usare l'astrazione `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="0daba-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="0daba-111">Per rilasciare chiamate di interoperabilità JS, inserire l'astrazione `IJSRuntime` nel componente.</span><span class="sxs-lookup"><span data-stu-id="0daba-111">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="0daba-112">Il metodo `InvokeAsync<T>` accetta un identificatore per la funzione JavaScript che si vuole richiamare insieme a un numero qualsiasi di argomenti serializzabili in JSON.</span><span class="sxs-lookup"><span data-stu-id="0daba-112">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="0daba-113">L'identificatore della funzione è relativo all'ambito globale (`window`).</span><span class="sxs-lookup"><span data-stu-id="0daba-113">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="0daba-114">Se si desidera chiamare `window.someScope.someFunction`, l'identificatore viene `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="0daba-114">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="0daba-115">Non è necessario registrare la funzione prima che venga chiamata.</span><span class="sxs-lookup"><span data-stu-id="0daba-115">There's no need to register the function before it's called.</span></span> <span data-ttu-id="0daba-116">Il tipo restituito `T` deve anche essere serializzabile in JSON.</span><span class="sxs-lookup"><span data-stu-id="0daba-116">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="0daba-117">`T` deve corrispondere al tipo .NET che meglio esegue il mapping al tipo JSON restituito.</span><span class="sxs-lookup"><span data-stu-id="0daba-117">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="0daba-118">Per le app di Blazor server con il prerendering abilitato, non è possibile chiamare JavaScript durante il prerendering iniziale.</span><span class="sxs-lookup"><span data-stu-id="0daba-118">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="0daba-119">È necessario rinviare le chiamate di interoperabilità JavaScript finché non viene stabilita la connessione con il browser.</span><span class="sxs-lookup"><span data-stu-id="0daba-119">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="0daba-120">Per ulteriori informazioni, vedere la sezione [rilevare quando un'app Server Blazor è prerendering](#detect-when-a-blazor-server-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="0daba-120">For more information, see the [Detect when a Blazor Server app is prerendering](#detect-when-a-blazor-server-app-is-prerendering) section.</span></span>

<span data-ttu-id="0daba-121">L'esempio seguente è basato su [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un decodificatore basato su JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0daba-121">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), a JavaScript-based decoder.</span></span> <span data-ttu-id="0daba-122">Nell'esempio viene illustrato come richiamare una funzione JavaScript da un C# metodo.</span><span class="sxs-lookup"><span data-stu-id="0daba-122">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="0daba-123">La funzione JavaScript accetta una matrice di byte da C# un metodo, decodifica la matrice e restituisce il testo al componente per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0daba-123">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="0daba-124">All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript che usa `TextDecoder` per decodificare una matrice passata e restituire il valore decodificato:</span><span class="sxs-lookup"><span data-stu-id="0daba-124">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="0daba-125">Il codice JavaScript, ad esempio il codice illustrato nell'esempio precedente, può anche essere caricato da un file JavaScript (con*estensione js*) con un riferimento al file script:</span><span class="sxs-lookup"><span data-stu-id="0daba-125">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="0daba-126">Il componente seguente:</span><span class="sxs-lookup"><span data-stu-id="0daba-126">The following component:</span></span>

* <span data-ttu-id="0daba-127">Richiama la funzione JavaScript `convertArray` usando `JSRuntime` quando viene selezionato un pulsante del componente (**Convert Array**).</span><span class="sxs-lookup"><span data-stu-id="0daba-127">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="0daba-128">Dopo la chiamata della funzione JavaScript, la matrice passata viene convertita in una stringa.</span><span class="sxs-lookup"><span data-stu-id="0daba-128">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="0daba-129">La stringa viene restituita al componente per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0daba-129">The string is returned to the component for display.</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="ijsruntime"></a><span data-ttu-id="0daba-130">IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="0daba-130">IJSRuntime</span></span>

<span data-ttu-id="0daba-131">Per usare l'astrazione `IJSRuntime`, adottare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="0daba-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="0daba-132">Inserire l'astrazione del `IJSRuntime` nel componente Razor (*Razor*):</span><span class="sxs-lookup"><span data-stu-id="0daba-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="0daba-133">All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="0daba-133">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="0daba-134">La funzione viene chiamata con `IJSRuntime.InvokeVoidAsync` e non restituisce un valore:</span><span class="sxs-lookup"><span data-stu-id="0daba-134">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="0daba-135">Inserire l'astrazione `IJSRuntime` in una classe ( *. cs*):</span><span class="sxs-lookup"><span data-stu-id="0daba-135">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="0daba-136">All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="0daba-136">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="0daba-137">La funzione viene chiamata con `JSRuntime.InvokeAsync` e restituisce un valore:</span><span class="sxs-lookup"><span data-stu-id="0daba-137">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="0daba-138">Per la generazione di contenuto dinamico con [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), usare l'attributo `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="0daba-138">For dynamic content generation with [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="0daba-139">Nell'app di esempio lato client che accompagna questo argomento sono disponibili due funzioni JavaScript per l'app che interagiscono con il DOM per ricevere l'input dell'utente e visualizzare un messaggio di benvenuto:</span><span class="sxs-lookup"><span data-stu-id="0daba-139">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="0daba-140">`showPrompt` &ndash; genera un messaggio di richiesta per accettare l'input dell'utente (il nome dell'utente) e restituisce il nome al chiamante.</span><span class="sxs-lookup"><span data-stu-id="0daba-140">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="0daba-141">`displayWelcome` &ndash; assegna un messaggio di benvenuto dal chiamante a un oggetto DOM con un `id` di `welcome`.</span><span class="sxs-lookup"><span data-stu-id="0daba-141">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="0daba-142">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="0daba-142">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="0daba-143">Inserire il tag `<script>` che fa riferimento al file JavaScript nel file *wwwroot/index.html* (Blazor webassembly) o nel file *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="0daba-143">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="0daba-144">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="0daba-144">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="0daba-145">*Pagine/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="0daba-145">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="0daba-146">Non inserire un tag `<script>` in un file di componente perché il tag `<script>` non può essere aggiornato dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="0daba-146">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="0daba-147">I metodi .NET interoperano con le funzioni JavaScript nel file *exampleJsInterop. js* chiamando `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="0daba-147">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="0daba-148">La `IJSRuntime` astrazione è asincrona per consentire scenari Blazor server.</span><span class="sxs-lookup"><span data-stu-id="0daba-148">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="0daba-149">Se l'app è un'app webassembly Blazor e si vuole richiamare una funzione JavaScript in modo sincrono, abbattuti per `IJSInProcessRuntime` e chiamare invece `Invoke<T>`.</span><span class="sxs-lookup"><span data-stu-id="0daba-149">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="0daba-150">È consigliabile che la maggior parte delle librerie di interoperabilità JS usi le API asincrone per assicurarsi che le librerie siano disponibili in tutti gli scenari.</span><span class="sxs-lookup"><span data-stu-id="0daba-150">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="0daba-151">L'app di esempio include un componente per dimostrare l'interoperabilità JS.</span><span class="sxs-lookup"><span data-stu-id="0daba-151">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="0daba-152">Il componente:</span><span class="sxs-lookup"><span data-stu-id="0daba-152">The component:</span></span>

* <span data-ttu-id="0daba-153">Riceve l'input dell'utente tramite un prompt di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0daba-153">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="0daba-154">Restituisce il testo al componente per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="0daba-154">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="0daba-155">Chiama una seconda funzione JavaScript che interagisce con il DOM per visualizzare un messaggio di benvenuto.</span><span class="sxs-lookup"><span data-stu-id="0daba-155">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="0daba-156">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0daba-156">*Pages/JSInterop.razor*:</span></span>

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
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");

        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. <span data-ttu-id="0daba-157">Quando `TriggerJsPrompt` viene eseguita selezionando il pulsante di **richiesta trigger JavaScript** del componente, viene chiamata la funzione JavaScript `showPrompt` specificata nel file *wwwroot/exampleJsInterop. js* .</span><span class="sxs-lookup"><span data-stu-id="0daba-157">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="0daba-158">La funzione `showPrompt` accetta l'input dell'utente (il nome dell'utente), che è codificato in HTML e restituito al componente.</span><span class="sxs-lookup"><span data-stu-id="0daba-158">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="0daba-159">Il componente archivia il nome dell'utente in una variabile locale, `name`.</span><span class="sxs-lookup"><span data-stu-id="0daba-159">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="0daba-160">La stringa archiviata in `name` viene incorporata in un messaggio di benvenuto, che viene passato a una funzione JavaScript, `displayWelcome`, che esegue il rendering del messaggio di benvenuto in un tag di intestazione.</span><span class="sxs-lookup"><span data-stu-id="0daba-160">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="0daba-161">Chiamare una funzione JavaScript void</span><span class="sxs-lookup"><span data-stu-id="0daba-161">Call a void JavaScript function</span></span>

<span data-ttu-id="0daba-162">Le funzioni JavaScript che restituiscono [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) vengono chiamate con `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="0daba-162">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-server-app-is-prerendering"></a><span data-ttu-id="0daba-163">Rileva quando è in fase di prerendering un'app Server Blazor</span><span class="sxs-lookup"><span data-stu-id="0daba-163">Detect when a Blazor Server app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="0daba-164">Acquisisci riferimenti a elementi</span><span class="sxs-lookup"><span data-stu-id="0daba-164">Capture references to elements</span></span>

<span data-ttu-id="0daba-165">Alcuni scenari di interoperabilità JS richiedono riferimenti agli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="0daba-165">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="0daba-166">Ad esempio, una libreria dell'interfaccia utente può richiedere un riferimento a un elemento per l'inizializzazione o potrebbe essere necessario chiamare API simili a quelle di un elemento, ad esempio `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="0daba-166">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="0daba-167">Acquisire i riferimenti agli elementi HTML in un componente usando l'approccio seguente:</span><span class="sxs-lookup"><span data-stu-id="0daba-167">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="0daba-168">Aggiungere un attributo `@ref` all'elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="0daba-168">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="0daba-169">Definire un campo di tipo `ElementReference` il cui nome corrisponde al valore dell'attributo `@ref`.</span><span class="sxs-lookup"><span data-stu-id="0daba-169">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="0daba-170">Nell'esempio seguente viene illustrata l'acquisizione di un riferimento all'elemento `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="0daba-170">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="0daba-171">Usare solo un riferimento a un elemento per mutare il contenuto di un elemento vuoto che non interagisce con Blazor.</span><span class="sxs-lookup"><span data-stu-id="0daba-171">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="0daba-172">Questo scenario è utile quando un'API di terze parti fornisce contenuto all'elemento.</span><span class="sxs-lookup"><span data-stu-id="0daba-172">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="0daba-173">Poiché Blazor non interagisce con l'elemento, non è possibile che si verifichi un conflitto tra la rappresentazione Blazordell'elemento e il DOM.</span><span class="sxs-lookup"><span data-stu-id="0daba-173">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="0daba-174">Nell'esempio seguente è *pericoloso* modificare il contenuto dell'elenco non ordinato (`ul`) perché Blazor interagisce con il Dom per popolare gli elementi elenco di questo elemento (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="0daba-174">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="0daba-175">Se l'interoperabilità JS modifica il contenuto del `MyList` di elementi e Blazor tenta di applicare le differenze all'elemento, le differenze non corrispondono al DOM.</span><span class="sxs-lookup"><span data-stu-id="0daba-175">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="0daba-176">Per quanto riguarda il codice .NET, un `ElementReference` è un handle opaco.</span><span class="sxs-lookup"><span data-stu-id="0daba-176">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="0daba-177">L' *unica* cosa che è possibile fare con `ElementReference` è passarla al codice JavaScript tramite l'interoperabilità js.</span><span class="sxs-lookup"><span data-stu-id="0daba-177">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="0daba-178">Quando si esegue questa operazione, il codice sul lato JavaScript riceve un'istanza `HTMLElement`, che può essere usata con le API DOM normali.</span><span class="sxs-lookup"><span data-stu-id="0daba-178">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="0daba-179">Il codice seguente, ad esempio, definisce un metodo di estensione .NET che consente di impostare lo stato attivo su un elemento:</span><span class="sxs-lookup"><span data-stu-id="0daba-179">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="0daba-180">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="0daba-180">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="0daba-181">Per chiamare una funzione JavaScript che non restituisce un valore, usare `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="0daba-181">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="0daba-182">Il codice seguente imposta lo stato attivo sull'input del nome utente chiamando la funzione JavaScript precedente con la `ElementReference`acquisita:</span><span class="sxs-lookup"><span data-stu-id="0daba-182">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="0daba-183">Per usare un metodo di estensione, creare un metodo di estensione statico che riceva l'istanza di `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="0daba-183">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="0daba-184">Il metodo `Focus` viene chiamato direttamente nell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="0daba-184">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="0daba-185">Nell'esempio seguente si presuppone che il metodo `Focus` sia disponibile dallo spazio dei nomi `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="0daba-185">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="0daba-186">La variabile `username` viene popolata solo dopo il rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="0daba-186">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="0daba-187">Se un `ElementReference` non popolato viene passato al codice JavaScript, il codice JavaScript riceve un valore di `null`.</span><span class="sxs-lookup"><span data-stu-id="0daba-187">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="0daba-188">Per modificare i riferimenti agli elementi dopo che il componente ha terminato il rendering (per impostare lo stato attivo iniziale su un elemento), usare i metodi del ciclo di vita dei [componenti OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="0daba-188">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="0daba-189">Quando si utilizzano tipi generici e si restituisce un valore, utilizzare [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="0daba-189">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="0daba-190">`GenericMethod` viene chiamato direttamente sull'oggetto con un tipo.</span><span class="sxs-lookup"><span data-stu-id="0daba-190">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="0daba-191">Nell'esempio seguente si presuppone che il `GenericMethod` sia disponibile dallo spazio dei nomi `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="0daba-191">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="0daba-192">Elementi di riferimento tra i componenti</span><span class="sxs-lookup"><span data-stu-id="0daba-192">Reference elements across components</span></span>

<span data-ttu-id="0daba-193">Un `ElementReference` è garantito valido solo nel metodo di `OnAfterRender` di un componente (e un riferimento a un elemento è un `struct`), pertanto non è possibile passare un riferimento a un elemento tra i componenti.</span><span class="sxs-lookup"><span data-stu-id="0daba-193">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="0daba-194">Affinché un componente padre renda disponibile un riferimento a un elemento per altri componenti, il componente padre può:</span><span class="sxs-lookup"><span data-stu-id="0daba-194">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="0daba-195">Consenti ai componenti figlio di registrare i callback.</span><span class="sxs-lookup"><span data-stu-id="0daba-195">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="0daba-196">Richiamare i callback registrati durante l'evento `OnAfterRender` con il riferimento all'elemento passato.</span><span class="sxs-lookup"><span data-stu-id="0daba-196">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="0daba-197">Indirettamente, questo approccio consente ai componenti figlio di interagire con il riferimento all'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="0daba-197">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="0daba-198">Nell'esempio di Blazor webassembly riportato di seguito viene illustrato l'approccio.</span><span class="sxs-lookup"><span data-stu-id="0daba-198">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="0daba-199">Nel `<head>` di *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="0daba-199">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="0daba-200">Nel `<body>` di *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="0daba-200">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="0daba-201">*Pages/index. Razor* (componente padre):</span><span class="sxs-lookup"><span data-stu-id="0daba-201">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="0daba-202">*Pages/index. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="0daba-202">*Pages/Index.razor.cs*:</span></span>

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

<span data-ttu-id="0daba-203">*Shared/SurveyPrompt. Razor* (componente figlio):</span><span class="sxs-lookup"><span data-stu-id="0daba-203">*Shared/SurveyPrompt.razor* (child component):</span></span>

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

<span data-ttu-id="0daba-204">*Shared/SurveyPrompt. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="0daba-204">*Shared/SurveyPrompt.razor.cs*:</span></span>

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

## <a name="harden-js-interop-calls"></a><span data-ttu-id="0daba-205">Chiamate di interoperabilità JS di Harden</span><span class="sxs-lookup"><span data-stu-id="0daba-205">Harden JS interop calls</span></span>

<span data-ttu-id="0daba-206">L'interoperabilità JS potrebbe non riuscire a causa di errori di rete e deve essere considerata inaffidabile.</span><span class="sxs-lookup"><span data-stu-id="0daba-206">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="0daba-207">Per impostazione predefinita, un'app di Blazor server timeout le chiamate di interoperabilità JS nel server dopo un minuto.</span><span class="sxs-lookup"><span data-stu-id="0daba-207">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="0daba-208">Se un'app può tollerare un timeout più aggressivo, ad esempio 10 secondi, impostare il timeout usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="0daba-208">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="0daba-209">A livello globale in `Startup.ConfigureServices`, specificare il timeout:</span><span class="sxs-lookup"><span data-stu-id="0daba-209">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="0daba-210">Per chiamata nel codice componente, una singola chiamata può specificare il timeout:</span><span class="sxs-lookup"><span data-stu-id="0daba-210">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="0daba-211">Per ulteriori informazioni sull'esaurimento delle risorse, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="0daba-211">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="additional-resources"></a><span data-ttu-id="0daba-212">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0daba-212">Additional resources</span></span>

* <xref:blazor/call-dotnet-from-javascript>
* [<span data-ttu-id="0daba-213">Esempio di InteropComponent. Razor (repository GitHub DotNet/AspNetCore, Branch versione 3,1)</span><span class="sxs-lookup"><span data-stu-id="0daba-213">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="0daba-214">[Eseguire trasferimenti di dati di grandi dimensioni nelle app Blazor server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="0daba-214">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
