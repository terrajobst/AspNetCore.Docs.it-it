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
ms.openlocfilehash: 280ea832f492852e425e3e15c61cac54fd1e39d6
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879679"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>Ciclo di vita Blazor ASP.NET Core

Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)

Il Framework Blazor include metodi del ciclo di vita sincroni e asincroni. Eseguire l'override dei metodi Lifecycle per eseguire operazioni aggiuntive sui componenti durante l'inizializzazione e il rendering dei componenti.

## <a name="lifecycle-methods"></a>Metodi del ciclo di vita

### <a name="component-initialization-methods"></a>Metodi di inizializzazione componenti

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> eseguire codice che Inizializza un componente. Questi metodi vengono chiamati solo una volta quando viene creata la prima istanza del componente.

Per eseguire un'operazione asincrona, usare `OnInitializedAsync` e la parola chiave `await` sull'operazione:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> Il lavoro asincrono durante l'inizializzazione del componente deve verificarsi durante l'evento del ciclo di vita del `OnInitializedAsync`.

Per un'operazione sincrona, usare `OnInitialized`:

```csharp
protected override void OnInitialized()
{
    ...
}
```

### <a name="before-parameters-are-set"></a>Prima dell'impostazione dei parametri

<xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> imposta i parametri forniti dall'elemento padre del componente nell'albero di rendering:

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<xref:Microsoft.AspNetCore.Components.ParameterView> contiene l'intero set di valori dei parametri ogni volta che viene chiamato `SetParametersAsync`.

L'implementazione predefinita di `SetParametersAsync` imposta il valore di ogni proprietà con l'attributo `[Parameter]` o `[CascadingParameter]` con un valore corrispondente nell'`ParameterView`. I parametri che non hanno un valore corrispondente in `ParameterView` vengono lasciati invariati.

Se `base.SetParametersAync` non viene richiamato, il codice personalizzato può interpretare il valore dei parametri in ingresso in qualsiasi modo necessario. Ad esempio, non è necessario assegnare i parametri in ingresso alle proprietà della classe.

### <a name="after-parameters-are-set"></a>Parametri after impostati

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> vengono chiamati quando un componente riceve parametri dal relativo elemento padre e i valori vengono assegnati alle proprietà. Questi metodi vengono eseguiti dopo l'inizializzazione del componente e ogni volta che vengono specificati nuovi valori di parametro:

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> Il lavoro asincrono quando si applicano i parametri e i valori delle proprietà deve verificarsi durante l'evento ciclo di vita `OnParametersSetAsync`.

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a>Rendering del componente dopo

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> vengono chiamati dopo che un componente ha terminato il rendering. I riferimenti a elementi e componenti vengono popolati a questo punto. Usare questa fase per eseguire passaggi di inizializzazione aggiuntivi usando il contenuto sottoposto a rendering, ad esempio l'attivazione di librerie JavaScript di terze parti che operano sugli elementi DOM sottoposti a rendering.

Il parametro `firstRender` per `OnAfterRenderAsync` e `OnAfterRender`:

* È impostato su `true` la prima volta che viene eseguito il rendering dell'istanza del componente.
* Può essere usato per garantire che il lavoro di inizializzazione venga eseguito una sola volta.

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
> Il lavoro asincrono immediatamente dopo il rendering deve verificarsi durante l'evento del ciclo di vita del `OnAfterRenderAsync`.
>
> Anche se si restituisce un <xref:System.Threading.Tasks.Task> da `OnAfterRenderAsync`, il Framework non pianifica un ulteriore ciclo di rendering per il componente al termine dell'attività. In questo modo è possibile evitare un ciclo di rendering infinito. È diverso dagli altri metodi del ciclo di vita, che pianificano un ulteriore ciclo di rendering dopo il completamento dell'attività restituita.

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

`OnAfterRender` e `OnAfterRenderAsync` *non vengono chiamati durante il prerendering sul server.*

### <a name="suppress-ui-refreshing"></a>Evita aggiornamento dell'interfaccia utente

Eseguire l'override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> per disattivare l'aggiornamento dell'interfaccia utente. Se l'implementazione restituisce `true`, l'interfaccia utente viene aggiornata:

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

`ShouldRender` viene chiamato ogni volta che il componente viene sottoposto a rendering.

Anche se `ShouldRender` viene sottoposto a override, il componente viene sempre sottoposto a rendering iniziale.

## <a name="state-changes"></a>Modifiche dello stato

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifica al componente che lo stato è stato modificato. Se applicabile, la chiamata di `StateHasChanged` causa il rendering del componente.

## <a name="handle-incomplete-async-actions-at-render"></a>Gestisci azioni asincrone incomplete durante il rendering

Le azioni asincrone eseguite negli eventi del ciclo di vita potrebbero non essere state completate prima del rendering del componente. Gli oggetti potrebbero essere `null` o compilati in modo incompleto con i dati mentre è in esecuzione il metodo del ciclo di vita. Fornire la logica di rendering per confermare che gli oggetti vengono inizializzati. Esegue il rendering degli elementi dell'interfaccia utente segnaposto (ad esempio, un messaggio di caricamento) mentre gli oggetti sono `null`.

Nel componente `FetchData` dei modelli di Blazor, `OnInitializedAsync` viene sottoposto a override in asincrono ricevere i dati delle previsioni (`forecasts`). Quando `forecasts` viene `null`, all'utente viene visualizzato un messaggio di caricamento. Al termine dell'`Task` restituito da `OnInitializedAsync`, il componente viene nuovamente sottoposta a rendering con lo stato aggiornato.

*Pages/fetchData. Razor* nel modello di Blazor server:

[!code-cshtml[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>Eliminazione di componenti con IDisposable

Se un componente implementa <xref:System.IDisposable>, il [metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose) viene chiamato quando il componente viene rimosso dall'interfaccia utente. Il componente seguente usa `@implements IDisposable` e il metodo `Dispose`:

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
> La chiamata di <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` non è supportata. `StateHasChanged` potrebbe essere richiamato nell'ambito della rimozione del renderer, quindi la richiesta degli aggiornamenti dell'interfaccia utente in quel momento non è supportata.

## <a name="handle-errors"></a>Gestire gli errori

Per informazioni sulla gestione degli errori durante l'esecuzione del metodo del ciclo di vita, vedere <xref:blazor/handle-errors#lifecycle-methods>.
