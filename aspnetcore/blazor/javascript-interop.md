---
title: Interoperabilità ASP.NET Core Blazor JavaScript
author: guardrex
description: Informazioni su come richiamare funzioni JavaScript da metodi .NET e .NET da JavaScript in app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: f55eda512f8dcf0695c2e7f4655db83b26ea4159
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317202"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="0cf70-103">Interoperabilità ASP.NET Core Blazor JavaScript</span><span class="sxs-lookup"><span data-stu-id="0cf70-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="0cf70-104">Di [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0cf70-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="0cf70-105">Un'app Blazor può richiamare funzioni JavaScript da metodi .NET e .NET dal codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cf70-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="0cf70-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0cf70-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="0cf70-107">Richiamare funzioni JavaScript da metodi .NET</span><span class="sxs-lookup"><span data-stu-id="0cf70-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="0cf70-108">In alcuni casi è necessario che il codice .NET chiami una funzione JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cf70-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="0cf70-109">Ad esempio, una chiamata JavaScript può esporre le funzionalità o le funzionalità del browser da una libreria JavaScript all'app.</span><span class="sxs-lookup"><span data-stu-id="0cf70-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="0cf70-110">Questo scenario è denominato *interoperabilità JavaScript (interoperabilità* *JS*).</span><span class="sxs-lookup"><span data-stu-id="0cf70-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="0cf70-111">Per chiamare JavaScript da .NET, usare l'astrazione `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="0cf70-112">Il metodo `InvokeAsync<T>` accetta un identificatore per la funzione JavaScript che si vuole richiamare insieme a un numero qualsiasi di argomenti serializzabili in JSON.</span><span class="sxs-lookup"><span data-stu-id="0cf70-112">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="0cf70-113">L'identificatore della funzione è relativo all'ambito globale (`window`).</span><span class="sxs-lookup"><span data-stu-id="0cf70-113">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="0cf70-114">Se si desidera chiamare `window.someScope.someFunction`, l'identificatore viene `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-114">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="0cf70-115">Non è necessario registrare la funzione prima che venga chiamata.</span><span class="sxs-lookup"><span data-stu-id="0cf70-115">There's no need to register the function before it's called.</span></span> <span data-ttu-id="0cf70-116">Il tipo restituito `T` deve anche essere serializzabile in JSON.</span><span class="sxs-lookup"><span data-stu-id="0cf70-116">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="0cf70-117">Per le app di Blazor server:</span><span class="sxs-lookup"><span data-stu-id="0cf70-117">For Blazor Server apps:</span></span>

* <span data-ttu-id="0cf70-118">Più richieste utente vengono elaborate dall'app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="0cf70-118">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="0cf70-119">Non chiamare `JSRuntime.Current` in un componente per richiamare funzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cf70-119">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="0cf70-120">Inserire l'astrazione `IJSRuntime` e usare l'oggetto inserito per emettere chiamate di interoperabilità JS.</span><span class="sxs-lookup"><span data-stu-id="0cf70-120">Inject the `IJSRuntime` abstraction and use the injected object to issue JS interop calls.</span></span>
* <span data-ttu-id="0cf70-121">Mentre un'app Blazor è prerenderizzata, non è possibile chiamare JavaScript perché non è stata stabilita una connessione con il browser.</span><span class="sxs-lookup"><span data-stu-id="0cf70-121">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="0cf70-122">Per ulteriori informazioni, vedere la sezione [rilevare quando un'app Blazor è prerendering](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="0cf70-122">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="0cf70-123">L'esempio seguente è basato su [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un decodificatore basato su JavaScript sperimentale.</span><span class="sxs-lookup"><span data-stu-id="0cf70-123">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="0cf70-124">Nell'esempio viene illustrato come richiamare una funzione JavaScript da un C# metodo.</span><span class="sxs-lookup"><span data-stu-id="0cf70-124">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="0cf70-125">La funzione JavaScript accetta una matrice di byte da C# un metodo, decodifica la matrice e restituisce il testo al componente per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0cf70-125">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="0cf70-126">All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript che usa `TextDecoder` per decodificare una matrice passata e restituire il valore decodificato:</span><span class="sxs-lookup"><span data-stu-id="0cf70-126">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="0cf70-127">Il codice JavaScript, ad esempio il codice illustrato nell'esempio precedente, può anche essere caricato da un file JavaScript (con*estensione js*) con un riferimento al file script:</span><span class="sxs-lookup"><span data-stu-id="0cf70-127">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="0cf70-128">Il componente seguente:</span><span class="sxs-lookup"><span data-stu-id="0cf70-128">The following component:</span></span>

* <span data-ttu-id="0cf70-129">Richiama la funzione JavaScript `convertArray` usando `JSRuntime` quando viene selezionato un pulsante del componente (**Convert Array**).</span><span class="sxs-lookup"><span data-stu-id="0cf70-129">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="0cf70-130">Dopo la chiamata della funzione JavaScript, la matrice passata viene convertita in una stringa.</span><span class="sxs-lookup"><span data-stu-id="0cf70-130">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="0cf70-131">La stringa viene restituita al componente per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0cf70-131">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="0cf70-132">Uso di IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="0cf70-132">Use of IJSRuntime</span></span>

<span data-ttu-id="0cf70-133">Per usare l'astrazione `IJSRuntime`, adottare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="0cf70-133">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="0cf70-134">Inserire l'astrazione del `IJSRuntime` nel componente Razor (*Razor*):</span><span class="sxs-lookup"><span data-stu-id="0cf70-134">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="0cf70-135">All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-135">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="0cf70-136">La funzione viene chiamata con `IJSRuntime.InvokeVoidAsync` e non restituisce un valore:</span><span class="sxs-lookup"><span data-stu-id="0cf70-136">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="0cf70-137">Inserire l'astrazione `IJSRuntime` in una classe ( *. cs*):</span><span class="sxs-lookup"><span data-stu-id="0cf70-137">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="0cf70-138">All'interno dell'elemento `<head>` di *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), fornire una funzione JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-138">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="0cf70-139">La funzione viene chiamata con `JSRuntime.InvokeAsync` e restituisce un valore:</span><span class="sxs-lookup"><span data-stu-id="0cf70-139">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="0cf70-140">Per la generazione di contenuto dinamico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), usare l'attributo `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="0cf70-140">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="0cf70-141">Nell'app di esempio lato client che accompagna questo argomento sono disponibili due funzioni JavaScript per l'app che interagiscono con il DOM per ricevere l'input dell'utente e visualizzare un messaggio di benvenuto:</span><span class="sxs-lookup"><span data-stu-id="0cf70-141">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="0cf70-142">`showPrompt` &ndash; genera un messaggio di richiesta per accettare l'input dell'utente (il nome dell'utente) e restituisce il nome al chiamante.</span><span class="sxs-lookup"><span data-stu-id="0cf70-142">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="0cf70-143">`displayWelcome` &ndash; assegna un messaggio di benvenuto dal chiamante a un oggetto DOM con un `id` di `welcome`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-143">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="0cf70-144">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="0cf70-144">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="0cf70-145">Inserire il tag `<script>` che fa riferimento al file JavaScript nel file *wwwroot/index.html* (Blazor webassembly) o nel file *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="0cf70-145">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="0cf70-146">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="0cf70-146">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="0cf70-147">*Pagine/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="0cf70-147">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="0cf70-148">Non inserire un tag `<script>` in un file di componente perché il tag `<script>` non può essere aggiornato dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="0cf70-148">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="0cf70-149">I metodi .NET interoperano con le funzioni JavaScript nel file *exampleJsInterop. js* chiamando `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-149">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="0cf70-150">La `IJSRuntime` astrazione è asincrona per consentire scenari Blazor server.</span><span class="sxs-lookup"><span data-stu-id="0cf70-150">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="0cf70-151">Se l'app è un'app webassembly Blazor e si vuole richiamare una funzione JavaScript in modo sincrono, abbattuti per `IJSInProcessRuntime` e chiamare invece `Invoke<T>`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-151">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="0cf70-152">È consigliabile che la maggior parte delle librerie di interoperabilità JS usi le API asincrone per assicurarsi che le librerie siano disponibili in tutti gli scenari.</span><span class="sxs-lookup"><span data-stu-id="0cf70-152">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="0cf70-153">L'app di esempio include un componente per dimostrare l'interoperabilità JS.</span><span class="sxs-lookup"><span data-stu-id="0cf70-153">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="0cf70-154">Il componente:</span><span class="sxs-lookup"><span data-stu-id="0cf70-154">The component:</span></span>

* <span data-ttu-id="0cf70-155">Riceve l'input dell'utente tramite un prompt di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cf70-155">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="0cf70-156">Restituisce il testo al componente per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="0cf70-156">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="0cf70-157">Chiama una seconda funzione JavaScript che interagisce con il DOM per visualizzare un messaggio di benvenuto.</span><span class="sxs-lookup"><span data-stu-id="0cf70-157">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="0cf70-158">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0cf70-158">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="0cf70-159">Quando `TriggerJsPrompt` viene eseguita selezionando il pulsante di **richiesta trigger JavaScript** del componente, viene chiamata la funzione JavaScript `showPrompt` specificata nel file *wwwroot/exampleJsInterop. js* .</span><span class="sxs-lookup"><span data-stu-id="0cf70-159">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="0cf70-160">La funzione `showPrompt` accetta l'input dell'utente (il nome dell'utente), che è codificato in HTML e restituito al componente.</span><span class="sxs-lookup"><span data-stu-id="0cf70-160">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="0cf70-161">Il componente archivia il nome dell'utente in una variabile locale, `name`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-161">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="0cf70-162">La stringa archiviata in `name` viene incorporata in un messaggio di benvenuto, che viene passato a una funzione JavaScript, `displayWelcome`, che esegue il rendering del messaggio di benvenuto in un tag di intestazione.</span><span class="sxs-lookup"><span data-stu-id="0cf70-162">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="0cf70-163">Chiamare una funzione JavaScript void</span><span class="sxs-lookup"><span data-stu-id="0cf70-163">Call a void JavaScript function</span></span>

<span data-ttu-id="0cf70-164">Le funzioni JavaScript che restituiscono [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) vengono chiamate con `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-164">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="0cf70-165">Rileva quando è in fase di prerendering un'app Blazor</span><span class="sxs-lookup"><span data-stu-id="0cf70-165">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="0cf70-166">Acquisisci riferimenti a elementi</span><span class="sxs-lookup"><span data-stu-id="0cf70-166">Capture references to elements</span></span>

<span data-ttu-id="0cf70-167">Alcuni scenari di interoperabilità JS richiedono riferimenti agli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="0cf70-167">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="0cf70-168">Ad esempio, una libreria dell'interfaccia utente può richiedere un riferimento a un elemento per l'inizializzazione o potrebbe essere necessario chiamare API simili a quelle di un elemento, ad esempio `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-168">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="0cf70-169">Acquisire i riferimenti agli elementi HTML in un componente usando l'approccio seguente:</span><span class="sxs-lookup"><span data-stu-id="0cf70-169">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="0cf70-170">Aggiungere un attributo `@ref` all'elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="0cf70-170">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="0cf70-171">Definire un campo di tipo `ElementReference` il cui nome corrisponde al valore dell'attributo `@ref`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-171">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="0cf70-172">Nell'esempio seguente viene illustrata l'acquisizione di un riferimento all'elemento `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="0cf70-172">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="0cf70-173">Usare solo un riferimento a un elemento per mutare il contenuto di un elemento vuoto che non interagisce con Blazor.</span><span class="sxs-lookup"><span data-stu-id="0cf70-173">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="0cf70-174">Questo scenario è utile quando un'API di terze parti fornisce contenuto all'elemento.</span><span class="sxs-lookup"><span data-stu-id="0cf70-174">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="0cf70-175">Poiché Blazor non interagisce con l'elemento, non è possibile che si verifichi un conflitto tra la rappresentazione Blazordell'elemento e il DOM.</span><span class="sxs-lookup"><span data-stu-id="0cf70-175">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="0cf70-176">Nell'esempio seguente è *pericoloso* modificare il contenuto dell'elenco non ordinato (`ul`) perché Blazor interagisce con il Dom per popolare gli elementi elenco di questo elemento (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="0cf70-176">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
>
> ```cshtml
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> <span data-ttu-id="0cf70-177">Se l'interoperabilità JS modifica il contenuto del `MyList` di elementi e Blazor tenta di applicare le differenze all'elemento, le differenze non corrispondono al DOM.</span><span class="sxs-lookup"><span data-stu-id="0cf70-177">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="0cf70-178">Per quanto riguarda il codice .NET, un `ElementReference` è un handle opaco.</span><span class="sxs-lookup"><span data-stu-id="0cf70-178">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="0cf70-179">L' *unica* cosa che è possibile fare con `ElementReference` è passarla al codice JavaScript tramite l'interoperabilità js.</span><span class="sxs-lookup"><span data-stu-id="0cf70-179">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="0cf70-180">Quando si esegue questa operazione, il codice sul lato JavaScript riceve un'istanza `HTMLElement`, che può essere usata con le API DOM normali.</span><span class="sxs-lookup"><span data-stu-id="0cf70-180">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="0cf70-181">Il codice seguente, ad esempio, definisce un metodo di estensione .NET che consente di impostare lo stato attivo su un elemento:</span><span class="sxs-lookup"><span data-stu-id="0cf70-181">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="0cf70-182">*exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="0cf70-182">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="0cf70-183">Usare `IJSRuntime.InvokeAsync<T>` e chiamare `exampleJsFunctions.focusElement` con un `ElementReference` per concentrare un elemento:</span><span class="sxs-lookup"><span data-stu-id="0cf70-183">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="0cf70-184">Per usare un metodo di estensione per lo stato attivo di un elemento, creare un metodo di estensione statico che riceva l'istanza di `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="0cf70-184">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="0cf70-185">Il metodo viene chiamato direttamente nell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="0cf70-185">The method is called directly on the object.</span></span> <span data-ttu-id="0cf70-186">Nell'esempio seguente si presuppone che il metodo `Focus` statico sia disponibile dallo spazio dei nomi `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="0cf70-186">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="0cf70-187">La variabile `username` viene popolata solo dopo il rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="0cf70-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="0cf70-188">Se un `ElementReference` non popolato viene passato al codice JavaScript, il codice JavaScript riceve un valore di `null`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="0cf70-189">Per modificare i riferimenti agli elementi dopo che il componente ha terminato il rendering (per impostare lo stato attivo iniziale su un elemento), usare i [metodi del ciclo](xref:blazor/components#lifecycle-methods)di vita dei componenti `OnAfterRenderAsync` o `OnAfterRender`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="0cf70-190">Richiamare metodi .NET da funzioni JavaScript</span><span class="sxs-lookup"><span data-stu-id="0cf70-190">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="0cf70-191">Chiamata al metodo .NET statico</span><span class="sxs-lookup"><span data-stu-id="0cf70-191">Static .NET method call</span></span>

<span data-ttu-id="0cf70-192">Per richiamare un metodo .NET statico da JavaScript, usare le funzioni `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-192">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="0cf70-193">Passare l'identificatore del metodo statico che si desidera chiamare, il nome dell'assembly che contiene la funzione e gli eventuali argomenti.</span><span class="sxs-lookup"><span data-stu-id="0cf70-193">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="0cf70-194">La versione asincrona è preferibile per supportare Blazor scenari server.</span><span class="sxs-lookup"><span data-stu-id="0cf70-194">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="0cf70-195">Per richiamare un metodo .NET da JavaScript, il metodo .NET deve essere pubblico, statico e avere l'attributo `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-195">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="0cf70-196">Per impostazione predefinita, l'identificatore del metodo è il nome del metodo, ma è possibile specificare un identificatore diverso usando il costruttore `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-196">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="0cf70-197">La chiamata ai metodi generici aperti non è attualmente supportata.</span><span class="sxs-lookup"><span data-stu-id="0cf70-197">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="0cf70-198">L'app di esempio include C# un metodo per restituire una matrice di `int`s.</span><span class="sxs-lookup"><span data-stu-id="0cf70-198">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="0cf70-199">L'attributo `JSInvokable` viene applicato al metodo.</span><span class="sxs-lookup"><span data-stu-id="0cf70-199">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="0cf70-200">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0cf70-200">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="0cf70-201">JavaScript servito al client richiama il C# metodo .NET.</span><span class="sxs-lookup"><span data-stu-id="0cf70-201">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="0cf70-202">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="0cf70-202">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="0cf70-203">Quando si seleziona il pulsante **trigger .net static method ReturnArrayAsync** , esaminare l'output della console negli strumenti di sviluppo Web del browser.</span><span class="sxs-lookup"><span data-stu-id="0cf70-203">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="0cf70-204">L'output della console è:</span><span class="sxs-lookup"><span data-stu-id="0cf70-204">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="0cf70-205">Il quarto valore della matrice viene inserito nella matrice (`data.push(4);`) restituito da `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-205">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="0cf70-206">Chiamata al metodo di istanza</span><span class="sxs-lookup"><span data-stu-id="0cf70-206">Instance method call</span></span>

<span data-ttu-id="0cf70-207">È anche possibile chiamare i metodi di istanza .NET da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cf70-207">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="0cf70-208">Per richiamare un metodo di istanza .NET da JavaScript:</span><span class="sxs-lookup"><span data-stu-id="0cf70-208">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="0cf70-209">Passare l'istanza di .NET a JavaScript eseguendone il wrapping in un'istanza di `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-209">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="0cf70-210">L'istanza .NET viene passata per riferimento a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cf70-210">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="0cf70-211">Richiamare i metodi di istanza .NET sull'istanza utilizzando le funzioni `invokeMethod` o `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-211">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="0cf70-212">L'istanza di .NET può essere passata anche come argomento quando si richiamano altri metodi .NET da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cf70-212">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="0cf70-213">L'app di esempio consente di registrare i messaggi nella console lato client.</span><span class="sxs-lookup"><span data-stu-id="0cf70-213">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="0cf70-214">Per gli esempi seguenti illustrati dall'app di esempio, esaminare l'output della console del browser negli strumenti di sviluppo del browser.</span><span class="sxs-lookup"><span data-stu-id="0cf70-214">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="0cf70-215">Quando il pulsante del **metodo di istanza .NET del trigger HelloHelper. sayHello** è selezionato, `ExampleJsInterop.CallHelloHelperSayHello` viene chiamato e passa un nome, `Blazor`, al metodo.</span><span class="sxs-lookup"><span data-stu-id="0cf70-215">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="0cf70-216">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0cf70-216">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="0cf70-217">`CallHelloHelperSayHello` richiama la funzione JavaScript `sayHello` con una nuova istanza di `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-217">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="0cf70-218">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="0cf70-218">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="0cf70-219">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="0cf70-219">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="0cf70-220">Il nome viene passato al costruttore `HelloHelper`, che imposta la proprietà `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="0cf70-220">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="0cf70-221">Quando viene eseguita la funzione JavaScript `sayHello`, `HelloHelper.SayHello` restituisce il messaggio `Hello, {Name}!`, che viene scritto nella console dalla funzione JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cf70-221">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="0cf70-222">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="0cf70-222">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="0cf70-223">Output della console negli strumenti di sviluppo Web del browser:</span><span class="sxs-lookup"><span data-stu-id="0cf70-223">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="0cf70-224">Condividere il codice di interoperabilità in una libreria di classi</span><span class="sxs-lookup"><span data-stu-id="0cf70-224">Share interop code in a class library</span></span>

<span data-ttu-id="0cf70-225">Il codice di interoperabilità JS può essere incluso in una libreria di classi, che consente di condividere il codice in un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="0cf70-225">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="0cf70-226">La libreria di classi gestisce l'incorporamento delle risorse JavaScript nell'assembly compilato.</span><span class="sxs-lookup"><span data-stu-id="0cf70-226">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="0cf70-227">I file JavaScript vengono inseriti nella cartella *wwwroot*</span><span class="sxs-lookup"><span data-stu-id="0cf70-227">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="0cf70-228">Gli strumenti si occupano di incorporare le risorse quando la libreria viene compilata.</span><span class="sxs-lookup"><span data-stu-id="0cf70-228">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="0cf70-229">Nel file di progetto dell'app viene fatto riferimento al pacchetto NuGet compilato nello stesso modo in cui viene fatto riferimento a un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="0cf70-229">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="0cf70-230">Dopo il ripristino del pacchetto, il codice dell'app può chiamare JavaScript come se fosse C#.</span><span class="sxs-lookup"><span data-stu-id="0cf70-230">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="0cf70-231">Per altre informazioni, vedere <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="0cf70-231">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="0cf70-232">Chiamate di interoperabilità JS di Harden</span><span class="sxs-lookup"><span data-stu-id="0cf70-232">Harden JS interop calls</span></span>

<span data-ttu-id="0cf70-233">L'interoperabilità JS potrebbe non riuscire a causa di errori di rete e deve essere considerata inaffidabile.</span><span class="sxs-lookup"><span data-stu-id="0cf70-233">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="0cf70-234">Per impostazione predefinita, un'app di Blazor server timeout le chiamate di interoperabilità JS nel server dopo un minuto.</span><span class="sxs-lookup"><span data-stu-id="0cf70-234">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="0cf70-235">Se un'app può tollerare un timeout più aggressivo, ad esempio 10 secondi, impostare il timeout usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="0cf70-235">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="0cf70-236">A livello globale in `Startup.ConfigureServices`, specificare il timeout:</span><span class="sxs-lookup"><span data-stu-id="0cf70-236">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="0cf70-237">Per chiamata nel codice componente, una singola chiamata può specificare il timeout:</span><span class="sxs-lookup"><span data-stu-id="0cf70-237">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="0cf70-238">Per ulteriori informazioni sull'esaurimento delle risorse, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="0cf70-238">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
