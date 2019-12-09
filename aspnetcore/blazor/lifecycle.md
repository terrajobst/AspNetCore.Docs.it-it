---
title: Ciclo di vita Blazor ASP.NET Core
author: guardrex
description: Informazioni su come usare i metodi del ciclo di vita del componente Razor nelle app ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/lifecycle
ms.openlocfilehash: e600e7c7a6a8c646a655520bd5c127f2cd662753
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944031"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="f45a0-103">Ciclo di vita Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f45a0-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="f45a0-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f45a0-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="f45a0-105">Il Framework Blazor include metodi del ciclo di vita sincroni e asincroni.</span><span class="sxs-lookup"><span data-stu-id="f45a0-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="f45a0-106">Eseguire l'override dei metodi Lifecycle per eseguire operazioni aggiuntive sui componenti durante l'inizializzazione e il rendering dei componenti.</span><span class="sxs-lookup"><span data-stu-id="f45a0-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="f45a0-107">Metodi del ciclo di vita</span><span class="sxs-lookup"><span data-stu-id="f45a0-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="f45a0-108">Metodi di inizializzazione componenti</span><span class="sxs-lookup"><span data-stu-id="f45a0-108">Component initialization methods</span></span>

<span data-ttu-id="f45a0-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> eseguire codice che Inizializza un componente.</span><span class="sxs-lookup"><span data-stu-id="f45a0-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> execute code that initializes a component.</span></span> <span data-ttu-id="f45a0-110">Questi metodi vengono chiamati solo una volta quando viene creata la prima istanza del componente.</span><span class="sxs-lookup"><span data-stu-id="f45a0-110">These methods are only called one time when the component is first instantiated.</span></span>

<span data-ttu-id="f45a0-111">Per eseguire un'operazione asincrona, usare `OnInitializedAsync` e la parola chiave `await` sull'operazione:</span><span class="sxs-lookup"><span data-stu-id="f45a0-111">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="f45a0-112">Il lavoro asincrono durante l'inizializzazione del componente deve verificarsi durante l'evento del ciclo di vita del `OnInitializedAsync`.</span><span class="sxs-lookup"><span data-stu-id="f45a0-112">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="f45a0-113">Per un'operazione sincrona, usare `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="f45a0-113">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

### <a name="before-parameters-are-set"></a><span data-ttu-id="f45a0-114">Prima dell'impostazione dei parametri</span><span class="sxs-lookup"><span data-stu-id="f45a0-114">Before parameters are set</span></span>

<span data-ttu-id="f45a0-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> imposta i parametri forniti dall'elemento padre del componente nell'albero di rendering:</span><span class="sxs-lookup"><span data-stu-id="f45a0-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="f45a0-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contiene l'intero set di valori dei parametri ogni volta che viene chiamato `SetParametersAsync`.</span><span class="sxs-lookup"><span data-stu-id="f45a0-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="f45a0-117">L'implementazione predefinita di `SetParametersAsync` imposta il valore di ogni proprietà con l'attributo `[Parameter]` o `[CascadingParameter]` con un valore corrispondente nell'`ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="f45a0-117">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="f45a0-118">I parametri che non hanno un valore corrispondente in `ParameterView` vengono lasciati invariati.</span><span class="sxs-lookup"><span data-stu-id="f45a0-118">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="f45a0-119">Se `base.SetParametersAync` non viene richiamato, il codice personalizzato può interpretare il valore dei parametri in ingresso in qualsiasi modo necessario.</span><span class="sxs-lookup"><span data-stu-id="f45a0-119">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="f45a0-120">Ad esempio, non è necessario assegnare i parametri in ingresso alle proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="f45a0-120">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="f45a0-121">Parametri after impostati</span><span class="sxs-lookup"><span data-stu-id="f45a0-121">After parameters are set</span></span>

<span data-ttu-id="f45a0-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> vengono chiamati quando un componente riceve parametri dal relativo elemento padre e i valori vengono assegnati alle proprietà.</span><span class="sxs-lookup"><span data-stu-id="f45a0-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="f45a0-123">Questi metodi vengono eseguiti dopo l'inizializzazione del componente e ogni volta che vengono specificati nuovi valori di parametro:</span><span class="sxs-lookup"><span data-stu-id="f45a0-123">These methods are executed after component initialization and each time new parameter values are specified:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="f45a0-124">Il lavoro asincrono quando si applicano i parametri e i valori delle proprietà deve verificarsi durante l'evento ciclo di vita `OnParametersSetAsync`.</span><span class="sxs-lookup"><span data-stu-id="f45a0-124">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="f45a0-125">Rendering del componente dopo</span><span class="sxs-lookup"><span data-stu-id="f45a0-125">After component render</span></span>

<span data-ttu-id="f45a0-126"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> vengono chiamati dopo che un componente ha terminato il rendering.</span><span class="sxs-lookup"><span data-stu-id="f45a0-126"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="f45a0-127">I riferimenti a elementi e componenti vengono popolati a questo punto.</span><span class="sxs-lookup"><span data-stu-id="f45a0-127">Element and component references are populated at this point.</span></span> <span data-ttu-id="f45a0-128">Usare questa fase per eseguire passaggi di inizializzazione aggiuntivi usando il contenuto sottoposto a rendering, ad esempio l'attivazione di librerie JavaScript di terze parti che operano sugli elementi DOM sottoposti a rendering.</span><span class="sxs-lookup"><span data-stu-id="f45a0-128">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="f45a0-129">Il parametro `firstRender` per `OnAfterRenderAsync` e `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="f45a0-129">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="f45a0-130">È impostato su `true` la prima volta che viene eseguito il rendering dell'istanza del componente.</span><span class="sxs-lookup"><span data-stu-id="f45a0-130">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="f45a0-131">Può essere usato per garantire che il lavoro di inizializzazione venga eseguito una sola volta.</span><span class="sxs-lookup"><span data-stu-id="f45a0-131">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="f45a0-132">Il lavoro asincrono immediatamente dopo il rendering deve verificarsi durante l'evento del ciclo di vita del `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="f45a0-132">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="f45a0-133">Anche se si restituisce un <xref:System.Threading.Tasks.Task> da `OnAfterRenderAsync`, il Framework non pianifica un ulteriore ciclo di rendering per il componente al termine dell'attività.</span><span class="sxs-lookup"><span data-stu-id="f45a0-133">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="f45a0-134">In questo modo è possibile evitare un ciclo di rendering infinito.</span><span class="sxs-lookup"><span data-stu-id="f45a0-134">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="f45a0-135">È diverso dagli altri metodi del ciclo di vita, che pianificano un ulteriore ciclo di rendering dopo il completamento dell'attività restituita.</span><span class="sxs-lookup"><span data-stu-id="f45a0-135">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="f45a0-136">`OnAfterRender` e `OnAfterRenderAsync` *non vengono chiamati durante il prerendering sul server.*</span><span class="sxs-lookup"><span data-stu-id="f45a0-136">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="f45a0-137">Evita aggiornamento dell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="f45a0-137">Suppress UI refreshing</span></span>

<span data-ttu-id="f45a0-138">Eseguire l'override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> per disattivare l'aggiornamento dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="f45a0-138">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="f45a0-139">Se l'implementazione restituisce `true`, l'interfaccia utente viene aggiornata:</span><span class="sxs-lookup"><span data-stu-id="f45a0-139">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="f45a0-140">`ShouldRender` viene chiamato ogni volta che il componente viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="f45a0-140">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="f45a0-141">Anche se `ShouldRender` viene sottoposto a override, il componente viene sempre sottoposto a rendering iniziale.</span><span class="sxs-lookup"><span data-stu-id="f45a0-141">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="f45a0-142">Modifiche dello stato</span><span class="sxs-lookup"><span data-stu-id="f45a0-142">State changes</span></span>

<span data-ttu-id="f45a0-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifica al componente che lo stato è stato modificato.</span><span class="sxs-lookup"><span data-stu-id="f45a0-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="f45a0-144">Se applicabile, la chiamata di `StateHasChanged` causa il rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="f45a0-144">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="f45a0-145">Gestisci azioni asincrone incomplete durante il rendering</span><span class="sxs-lookup"><span data-stu-id="f45a0-145">Handle incomplete async actions at render</span></span>

<span data-ttu-id="f45a0-146">Le azioni asincrone eseguite negli eventi del ciclo di vita potrebbero non essere state completate prima del rendering del componente.</span><span class="sxs-lookup"><span data-stu-id="f45a0-146">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="f45a0-147">Gli oggetti potrebbero essere `null` o compilati in modo incompleto con i dati mentre è in esecuzione il metodo del ciclo di vita.</span><span class="sxs-lookup"><span data-stu-id="f45a0-147">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="f45a0-148">Fornire la logica di rendering per confermare che gli oggetti vengono inizializzati.</span><span class="sxs-lookup"><span data-stu-id="f45a0-148">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="f45a0-149">Esegue il rendering degli elementi dell'interfaccia utente segnaposto (ad esempio, un messaggio di caricamento) mentre gli oggetti sono `null`.</span><span class="sxs-lookup"><span data-stu-id="f45a0-149">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="f45a0-150">Nel componente `FetchData` dei modelli di Blazor, `OnInitializedAsync` viene sottoposto a override in asincrono ricevere i dati delle previsioni (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="f45a0-150">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="f45a0-151">Quando `forecasts` viene `null`, all'utente viene visualizzato un messaggio di caricamento.</span><span class="sxs-lookup"><span data-stu-id="f45a0-151">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="f45a0-152">Al termine dell'`Task` restituito da `OnInitializedAsync`, il componente viene nuovamente sottoposta a rendering con lo stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="f45a0-152">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="f45a0-153">*Pages/fetchData. Razor* nel modello di Blazor server:</span><span class="sxs-lookup"><span data-stu-id="f45a0-153">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="f45a0-154">Eliminazione di componenti con IDisposable</span><span class="sxs-lookup"><span data-stu-id="f45a0-154">Component disposal with IDisposable</span></span>

<span data-ttu-id="f45a0-155">Se un componente implementa <xref:System.IDisposable>, il [metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose) viene chiamato quando il componente viene rimosso dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="f45a0-155">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="f45a0-156">Il componente seguente usa `@implements IDisposable` e il metodo `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="f45a0-156">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
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
> <span data-ttu-id="f45a0-157">La chiamata di <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` non è supportata.</span><span class="sxs-lookup"><span data-stu-id="f45a0-157">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="f45a0-158">`StateHasChanged` potrebbe essere richiamato nell'ambito della rimozione del renderer, quindi la richiesta degli aggiornamenti dell'interfaccia utente in quel momento non è supportata.</span><span class="sxs-lookup"><span data-stu-id="f45a0-158">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="f45a0-159">Gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="f45a0-159">Handle errors</span></span>

<span data-ttu-id="f45a0-160">Per informazioni sulla gestione degli errori durante l'esecuzione del metodo del ciclo di vita, vedere <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="f45a0-160">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>
