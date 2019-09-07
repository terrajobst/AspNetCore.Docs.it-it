<span data-ttu-id="88773-101">Mentre un'app del lato server Blaze è prerendering, alcune azioni, ad esempio la chiamata a JavaScript, non sono possibili perché non è stata stabilita una connessione con il browser.</span><span class="sxs-lookup"><span data-stu-id="88773-101">While a Blazor server-side app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="88773-102">I componenti potrebbero dover eseguire il rendering in modo diverso quando ne viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="88773-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="88773-103">Per ritardare le chiamate di interoperabilità JavaScript finché non viene stabilita la connessione con il `OnAfterRenderAsync` browser, è possibile usare l'evento ciclo di vita dei componenti.</span><span class="sxs-lookup"><span data-stu-id="88773-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="88773-104">Questo evento viene chiamato solo dopo che viene eseguito il rendering completo dell'app e viene stabilita la connessione client.</span><span class="sxs-lookup"><span data-stu-id="88773-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            JSRuntime.InvokeAsync<object>(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

<span data-ttu-id="88773-105">Nel componente seguente viene illustrato come utilizzare l'interoperabilità JavaScript come parte della logica di inizializzazione di un componente in modo che sia compatibile con il prerendering.</span><span class="sxs-lookup"><span data-stu-id="88773-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="88773-106">Il componente indica che è possibile attivare un aggiornamento di rendering dall'interno `OnAfterRenderAsync`di.</span><span class="sxs-lookup"><span data-stu-id="88773-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="88773-107">Lo sviluppatore deve evitare di creare un ciclo infinito in questo scenario.</span><span class="sxs-lookup"><span data-stu-id="88773-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="88773-108">Dove `JSRuntime.InvokeAsync` viene chiamato, `ElementRef` viene usato solo in `OnAfterRenderAsync` e non in nessun metodo del ciclo di vita precedente perché non è presente alcun elemento JavaScript finché non viene eseguito il rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="88773-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="88773-109">`StateHasChanged`viene chiamato per eseguire il rendering del componente con il nuovo stato ottenuto dalla chiamata di interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="88773-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="88773-110">Il codice non crea un ciclo infinito perché `StateHasChanged` viene chiamato solo quando `infoFromJs` è `null`.</span><span class="sxs-lookup"><span data-stu-id="88773-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IComponentContext ComponentContext
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="myElem" />
</p>

@code {
    private string infoFromJs;
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```

<span data-ttu-id="88773-111">Per eseguire il rendering condizionale di contenuto diverso a seconda che l'app stia attualmente eseguendo il rendering `IsConnected` del contenuto, `IComponentContext` usare la proprietà nel servizio.</span><span class="sxs-lookup"><span data-stu-id="88773-111">To conditionally render different content based on whether the app is currently prerendering content, use the `IsConnected` property on the `IComponentContext` service.</span></span> <span data-ttu-id="88773-112">Quando si esegue il lato server `IsConnected` , restituisce `true` solo se è presente una connessione attiva al client.</span><span class="sxs-lookup"><span data-stu-id="88773-112">When running server-side, `IsConnected` only returns `true` if there's an active connection to the client.</span></span> <span data-ttu-id="88773-113">Viene sempre restituito `true` quando si esegue il lato client.</span><span class="sxs-lookup"><span data-stu-id="88773-113">It always returns `true` when running client-side.</span></span>

```cshtml
@page "/isconnected-example"
@using Microsoft.AspNetCore.Components.Services
@inject IComponentContext ComponentContext

<h1>IsConnected Example</h1>

<p>
    Current state:
    <strong id="connected-state">
        @(ComponentContext.IsConnected ? "connected" : "not connected")
    </strong>
</p>

<p>
    Clicks:
    <strong id="count">@count</strong>
    <button id="increment-count" @onclick="@(() => count++)">Click me</button>
</p>

@code {
    private int count;
}
```
