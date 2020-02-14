---
title: Ciclo di vita Blazor ASP.NET Core
author: guardrex
description: Informazioni su come usare i metodi del ciclo di vita del componente Razor nelle app ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: ecacd0a9728cbefd716e9dc7cd8a8c62f3df6e0d
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213389"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="0685a-103">Ciclo di vita Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0685a-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="0685a-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="0685a-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="0685a-105">Il Framework Blazor include metodi del ciclo di vita sincroni e asincroni.</span><span class="sxs-lookup"><span data-stu-id="0685a-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="0685a-106">Eseguire l'override dei metodi Lifecycle per eseguire operazioni aggiuntive sui componenti durante l'inizializzazione e il rendering dei componenti.</span><span class="sxs-lookup"><span data-stu-id="0685a-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="0685a-107">Metodi del ciclo di vita</span><span class="sxs-lookup"><span data-stu-id="0685a-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="0685a-108">Metodi di inizializzazione componenti</span><span class="sxs-lookup"><span data-stu-id="0685a-108">Component initialization methods</span></span>

<span data-ttu-id="0685a-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> vengono richiamati quando il componente viene inizializzato dopo aver ricevuto i parametri iniziali dal componente padre.</span><span class="sxs-lookup"><span data-stu-id="0685a-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="0685a-110">Utilizzare `OnInitializedAsync` quando il componente esegue un'operazione asincrona e deve essere aggiornato al termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="0685a-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span>

<span data-ttu-id="0685a-111">Per un'operazione sincrona, eseguire l'override di `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="0685a-111">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="0685a-112">Per eseguire un'operazione asincrona, eseguire l'override `OnInitializedAsync` e usare la parola chiave `await` sull'operazione:</span><span class="sxs-lookup"><span data-stu-id="0685a-112">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor<span data-ttu-id="0685a-113"> le app server che [preeseguono la](xref:blazor/hosting-model-configuration#render-mode) chiamata al contenuto `OnInitializedAsync` **_due volte_** :</span><span class="sxs-lookup"><span data-stu-id="0685a-113"> Server apps that [prerender their content](xref:blazor/hosting-model-configuration#render-mode) call `OnInitializedAsync` **_twice_**:</span></span>

* <span data-ttu-id="0685a-114">Una volta quando il componente viene inizialmente sottoposto a rendering statico come parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="0685a-114">Once when the component is initially rendered statically as part of the page.</span></span>
* <span data-ttu-id="0685a-115">Una seconda volta quando il browser stabilisce una connessione al server.</span><span class="sxs-lookup"><span data-stu-id="0685a-115">A second time when the browser establishes a connection back to the server.</span></span>

<span data-ttu-id="0685a-116">Per impedire che il codice dello sviluppatore in `OnInitializedAsync` venga eseguito due volte, vedere la sezione [riconnessione con stato dopo il rendering](#stateful-reconnection-after-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="0685a-116">To prevent developer code in `OnInitializedAsync` from running twice, see the [Stateful reconnection after prerendering](#stateful-reconnection-after-prerendering) section.</span></span>

<span data-ttu-id="0685a-117">Sebbene un'app di Blazor server sia prerendering, alcune azioni, ad esempio la chiamata a JavaScript, non sono possibili perché non è stata stabilita una connessione con il browser.</span><span class="sxs-lookup"><span data-stu-id="0685a-117">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="0685a-118">I componenti potrebbero dover eseguire il rendering in modo diverso quando ne viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="0685a-118">Components may need to render differently when prerendered.</span></span> <span data-ttu-id="0685a-119">Per altre informazioni, vedere la sezione [rilevare quando l'app è prerendering](#detect-when-the-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="0685a-119">For more information, see the [Detect when the app is prerendering](#detect-when-the-app-is-prerendering) section.</span></span>

### <a name="before-parameters-are-set"></a><span data-ttu-id="0685a-120">Prima dell'impostazione dei parametri</span><span class="sxs-lookup"><span data-stu-id="0685a-120">Before parameters are set</span></span>

<span data-ttu-id="0685a-121"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> imposta i parametri forniti dall'elemento padre del componente nell'albero di rendering:</span><span class="sxs-lookup"><span data-stu-id="0685a-121"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="0685a-122"><xref:Microsoft.AspNetCore.Components.ParameterView> contiene l'intero set di valori dei parametri ogni volta che viene chiamato `SetParametersAsync`.</span><span class="sxs-lookup"><span data-stu-id="0685a-122"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="0685a-123">L'implementazione predefinita di `SetParametersAsync` imposta il valore di ogni proprietà con l'attributo `[Parameter]` o `[CascadingParameter]` con un valore corrispondente nell'`ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="0685a-123">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="0685a-124">I parametri che non hanno un valore corrispondente in `ParameterView` vengono lasciati invariati.</span><span class="sxs-lookup"><span data-stu-id="0685a-124">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="0685a-125">Se `base.SetParametersAync` non viene richiamato, il codice personalizzato può interpretare il valore dei parametri in ingresso in qualsiasi modo necessario.</span><span class="sxs-lookup"><span data-stu-id="0685a-125">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="0685a-126">Ad esempio, non è necessario assegnare i parametri in ingresso alle proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="0685a-126">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="0685a-127">Parametri after impostati</span><span class="sxs-lookup"><span data-stu-id="0685a-127">After parameters are set</span></span>

<span data-ttu-id="0685a-128">vengono chiamati <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>:</span><span class="sxs-lookup"><span data-stu-id="0685a-128"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="0685a-129">Quando il componente viene inizializzato e ha ricevuto il primo set di parametri dal componente padre.</span><span class="sxs-lookup"><span data-stu-id="0685a-129">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="0685a-130">Quando il componente padre esegue nuovamente il rendering e fornisce:</span><span class="sxs-lookup"><span data-stu-id="0685a-130">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="0685a-131">Solo i tipi non modificabili primitivi noti di cui è stato modificato almeno un parametro.</span><span class="sxs-lookup"><span data-stu-id="0685a-131">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="0685a-132">Tutti i parametri tipizzati complessi.</span><span class="sxs-lookup"><span data-stu-id="0685a-132">Any complex-typed parameters.</span></span> <span data-ttu-id="0685a-133">Il Framework non è in grado di stabilire se i valori di un parametro tipizzato complesso sono stati modificati internamente, quindi considera il set di parametri come modificato.</span><span class="sxs-lookup"><span data-stu-id="0685a-133">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="0685a-134">Il lavoro asincrono quando si applicano i parametri e i valori delle proprietà deve verificarsi durante l'evento ciclo di vita `OnParametersSetAsync`.</span><span class="sxs-lookup"><span data-stu-id="0685a-134">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="0685a-135">Rendering del componente dopo</span><span class="sxs-lookup"><span data-stu-id="0685a-135">After component render</span></span>

<span data-ttu-id="0685a-136"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> vengono chiamati dopo che un componente ha terminato il rendering.</span><span class="sxs-lookup"><span data-stu-id="0685a-136"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="0685a-137">I riferimenti a elementi e componenti vengono popolati a questo punto.</span><span class="sxs-lookup"><span data-stu-id="0685a-137">Element and component references are populated at this point.</span></span> <span data-ttu-id="0685a-138">Usare questa fase per eseguire passaggi di inizializzazione aggiuntivi usando il contenuto sottoposto a rendering, ad esempio l'attivazione di librerie JavaScript di terze parti che operano sugli elementi DOM sottoposti a rendering.</span><span class="sxs-lookup"><span data-stu-id="0685a-138">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="0685a-139">Il parametro `firstRender` per `OnAfterRenderAsync` e `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="0685a-139">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="0685a-140">È impostato su `true` la prima volta che viene eseguito il rendering dell'istanza del componente.</span><span class="sxs-lookup"><span data-stu-id="0685a-140">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="0685a-141">Può essere usato per garantire che il lavoro di inizializzazione venga eseguito una sola volta.</span><span class="sxs-lookup"><span data-stu-id="0685a-141">Can be used to ensure that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="0685a-142">Il lavoro asincrono immediatamente dopo il rendering deve verificarsi durante l'evento del ciclo di vita del `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="0685a-142">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="0685a-143">Anche se si restituisce un <xref:System.Threading.Tasks.Task> da `OnAfterRenderAsync`, il Framework non pianifica un ulteriore ciclo di rendering per il componente al termine dell'attività.</span><span class="sxs-lookup"><span data-stu-id="0685a-143">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="0685a-144">In questo modo è possibile evitare un ciclo di rendering infinito.</span><span class="sxs-lookup"><span data-stu-id="0685a-144">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="0685a-145">È diverso dagli altri metodi del ciclo di vita, che pianificano un ulteriore ciclo di rendering dopo il completamento dell'attività restituita.</span><span class="sxs-lookup"><span data-stu-id="0685a-145">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="0685a-146">`OnAfterRender` e `OnAfterRenderAsync` *non vengono chiamati durante il prerendering sul server.*</span><span class="sxs-lookup"><span data-stu-id="0685a-146">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="0685a-147">Evita aggiornamento dell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="0685a-147">Suppress UI refreshing</span></span>

<span data-ttu-id="0685a-148">Eseguire l'override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> per disattivare l'aggiornamento dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="0685a-148">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="0685a-149">Se l'implementazione restituisce `true`, l'interfaccia utente viene aggiornata:</span><span class="sxs-lookup"><span data-stu-id="0685a-149">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="0685a-150">`ShouldRender` viene chiamato ogni volta che il componente viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="0685a-150">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="0685a-151">Anche se `ShouldRender` viene sottoposto a override, il componente viene sempre sottoposto a rendering iniziale.</span><span class="sxs-lookup"><span data-stu-id="0685a-151">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="0685a-152">Modifiche stato</span><span class="sxs-lookup"><span data-stu-id="0685a-152">State changes</span></span>

<span data-ttu-id="0685a-153"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifica al componente che lo stato è stato modificato.</span><span class="sxs-lookup"><span data-stu-id="0685a-153"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="0685a-154">Se applicabile, la chiamata di `StateHasChanged` causa il rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="0685a-154">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="0685a-155">Gestisci azioni asincrone incomplete durante il rendering</span><span class="sxs-lookup"><span data-stu-id="0685a-155">Handle incomplete async actions at render</span></span>

<span data-ttu-id="0685a-156">Le azioni asincrone eseguite negli eventi del ciclo di vita potrebbero non essere state completate prima del rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="0685a-156">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="0685a-157">Gli oggetti potrebbero essere `null` o compilati in modo incompleto con i dati mentre è in esecuzione il metodo del ciclo di vita.</span><span class="sxs-lookup"><span data-stu-id="0685a-157">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="0685a-158">Fornire la logica di rendering per confermare che gli oggetti vengono inizializzati.</span><span class="sxs-lookup"><span data-stu-id="0685a-158">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="0685a-159">Esegue il rendering degli elementi dell'interfaccia utente segnaposto (ad esempio, un messaggio di caricamento) mentre gli oggetti sono `null`.</span><span class="sxs-lookup"><span data-stu-id="0685a-159">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="0685a-160">Nel componente `FetchData` dei modelli di Blazor, `OnInitializedAsync` viene sottoposto a override in asincrono ricevere i dati delle previsioni (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="0685a-160">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="0685a-161">Quando `forecasts` viene `null`, all'utente viene visualizzato un messaggio di caricamento.</span><span class="sxs-lookup"><span data-stu-id="0685a-161">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="0685a-162">Al termine dell'`Task` restituito da `OnInitializedAsync`, il componente viene nuovamente sottoposta a rendering con lo stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="0685a-162">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="0685a-163">*Pages/fetchData. Razor* nel modello di Blazor server:</span><span class="sxs-lookup"><span data-stu-id="0685a-163">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="0685a-164">Eliminazione di componenti con IDisposable</span><span class="sxs-lookup"><span data-stu-id="0685a-164">Component disposal with IDisposable</span></span>

<span data-ttu-id="0685a-165">Se un componente implementa <xref:System.IDisposable>, il [metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose) viene chiamato quando il componente viene rimosso dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="0685a-165">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="0685a-166">Il componente seguente usa `@implements IDisposable` e il metodo `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="0685a-166">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```razor
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="0685a-167">La chiamata di <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` non è supportata.</span><span class="sxs-lookup"><span data-stu-id="0685a-167">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="0685a-168">`StateHasChanged` potrebbe essere richiamato nell'ambito della rimozione del renderer, quindi la richiesta degli aggiornamenti dell'interfaccia utente in quel momento non è supportata.</span><span class="sxs-lookup"><span data-stu-id="0685a-168">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="0685a-169">Gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="0685a-169">Handle errors</span></span>

<span data-ttu-id="0685a-170">Per informazioni sulla gestione degli errori durante l'esecuzione del metodo del ciclo di vita, vedere <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="0685a-170">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>

## <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="0685a-171">Riconnessione con stato dopo il rendering preliminare</span><span class="sxs-lookup"><span data-stu-id="0685a-171">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="0685a-172">In un'app Server Blazor quando `RenderMode` è `ServerPrerendered`, il componente viene inizialmente sottoposto a rendering statico come parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="0685a-172">In a Blazor Server app when `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="0685a-173">Quando il browser stabilisce una connessione al server, viene *nuovamente*eseguito il rendering del componente e il componente è ora interattivo.</span><span class="sxs-lookup"><span data-stu-id="0685a-173">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="0685a-174">Se è presente il metodo [{Async} Lifecycle OnInitialized](xref:blazor/lifecycle#component-initialization-methods) per l'inizializzazione del componente, il metodo viene eseguito *due volte*:</span><span class="sxs-lookup"><span data-stu-id="0685a-174">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="0685a-175">Quando il componente viene preeseguito in modo statico.</span><span class="sxs-lookup"><span data-stu-id="0685a-175">When the component is prerendered statically.</span></span>
* <span data-ttu-id="0685a-176">Una volta stabilita la connessione al server.</span><span class="sxs-lookup"><span data-stu-id="0685a-176">After the server connection has been established.</span></span>

<span data-ttu-id="0685a-177">Ciò può comportare una modifica evidente nei dati visualizzati nell'interfaccia utente quando il componente viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="0685a-177">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="0685a-178">Per evitare lo scenario di doppio rendering in un'app Server Blazor:</span><span class="sxs-lookup"><span data-stu-id="0685a-178">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="0685a-179">Passare un identificatore che può essere usato per memorizzare nella cache lo stato durante il prerendering e recuperare lo stato dopo il riavvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="0685a-179">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="0685a-180">Utilizzare l'identificatore durante il prerendering per salvare lo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="0685a-180">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="0685a-181">Utilizzare l'identificatore dopo il prerendering per recuperare lo stato memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="0685a-181">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="0685a-182">Il codice seguente illustra un `WeatherForecastService` aggiornato in un'app Server Blazor basata su modelli che evita il doppio rendering:</span><span class="sxs-lookup"><span data-stu-id="0685a-182">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

<span data-ttu-id="0685a-183">Per ulteriori informazioni sulla `RenderMode`, vedere <xref:blazor/hosting-model-configuration#render-mode>.</span><span class="sxs-lookup"><span data-stu-id="0685a-183">For more information on the `RenderMode`, see <xref:blazor/hosting-model-configuration#render-mode>.</span></span>

## <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="0685a-184">Rilevare il momento in cui viene eseguito il prerendering dell'app</span><span class="sxs-lookup"><span data-stu-id="0685a-184">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]
