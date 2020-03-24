---
title: Ciclo di vita Blazor ASP.NET Core
author: guardrex
description: Informazioni su come usare i metodi del ciclo di vita del componente Razor nelle app ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: 831f575afa6ce11d06c016d43ecd1bb59d09eab6
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218908"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>Ciclo di vita Blazor ASP.NET Core

Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)

Il Framework Blazor include metodi del ciclo di vita sincroni e asincroni. Eseguire l'override dei metodi Lifecycle per eseguire operazioni aggiuntive sui componenti durante l'inizializzazione e il rendering dei componenti.

## <a name="lifecycle-methods"></a>Metodi del ciclo di vita

### <a name="component-initialization-methods"></a>Metodi di inizializzazione componenti

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> vengono richiamati quando il componente viene inizializzato dopo aver ricevuto i parametri iniziali dal componente padre. Utilizzare `OnInitializedAsync` quando il componente esegue un'operazione asincrona e deve essere aggiornato al termine dell'operazione.

Per un'operazione sincrona, eseguire l'override di `OnInitialized`:

```csharp
protected override void OnInitialized()
{
    ...
}
```

Per eseguire un'operazione asincrona, eseguire l'override `OnInitializedAsync` e usare la parola chiave `await` sull'operazione:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor le app server che [preeseguono la](xref:blazor/hosting-model-configuration#render-mode) chiamata al contenuto `OnInitializedAsync` **_due volte_** :

* Una volta quando il componente viene inizialmente sottoposto a rendering statico come parte della pagina.
* Una seconda volta quando il browser stabilisce una connessione al server.

Per impedire che il codice dello sviluppatore in `OnInitializedAsync` venga eseguito due volte, vedere la sezione [riconnessione con stato dopo il rendering](#stateful-reconnection-after-prerendering) .

Sebbene un'app di Blazor server sia prerendering, alcune azioni, ad esempio la chiamata a JavaScript, non sono possibili perché non è stata stabilita una connessione con il browser. I componenti potrebbero dover eseguire il rendering in modo diverso quando ne viene eseguito il rendering. Per altre informazioni, vedere la sezione [rilevare quando l'app è prerendering](#detect-when-the-app-is-prerendering) .

Se sono configurati gestori di eventi, rimuoverli a disposizione. Per ulteriori informazioni, vedere la sezione relativa all' [eliminazione dei componenti con IDisposable](#component-disposal-with-idisposable) .

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

Se sono configurati gestori di eventi, rimuoverli a disposizione. Per ulteriori informazioni, vedere la sezione relativa all' [eliminazione dei componenti con IDisposable](#component-disposal-with-idisposable) .

### <a name="after-parameters-are-set"></a>Parametri after impostati

vengono chiamati <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> e <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>:

* Quando il componente viene inizializzato e ha ricevuto il primo set di parametri dal componente padre.
* Quando il componente padre esegue nuovamente il rendering e fornisce:
  * Solo i tipi non modificabili primitivi noti di cui è stato modificato almeno un parametro.
  * Tutti i parametri tipizzati complessi. Il Framework non è in grado di stabilire se i valori di un parametro tipizzato complesso sono stati modificati internamente, quindi considera il set di parametri come modificato.

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

Se sono configurati gestori di eventi, rimuoverli a disposizione. Per ulteriori informazioni, vedere la sezione relativa all' [eliminazione dei componenti con IDisposable](#component-disposal-with-idisposable) .

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

Se sono configurati gestori di eventi, rimuoverli a disposizione. Per ulteriori informazioni, vedere la sezione relativa all' [eliminazione dei componenti con IDisposable](#component-disposal-with-idisposable) .

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

## <a name="state-changes"></a>Modifiche stato

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifica al componente che lo stato è stato modificato. Se applicabile, la chiamata di `StateHasChanged` causa il rendering del componente.

## <a name="handle-incomplete-async-actions-at-render"></a>Gestisci azioni asincrone incomplete durante il rendering

Le azioni asincrone eseguite negli eventi del ciclo di vita potrebbero non essere state completate prima del rendering del componente. Gli oggetti potrebbero essere `null` o compilati in modo incompleto con i dati mentre è in esecuzione il metodo del ciclo di vita. Fornire la logica di rendering per confermare che gli oggetti vengono inizializzati. Esegue il rendering degli elementi dell'interfaccia utente segnaposto (ad esempio, un messaggio di caricamento) mentre gli oggetti sono `null`.

Nel componente `FetchData` dei modelli di Blazor, `OnInitializedAsync` viene sottoposto a override in asincrono ricevere i dati delle previsioni (`forecasts`). Quando `forecasts` viene `null`, all'utente viene visualizzato un messaggio di caricamento. Al termine dell'`Task` restituito da `OnInitializedAsync`, il componente viene nuovamente sottoposta a rendering con lo stato aggiornato.

*Pages/fetchData. Razor* nel modello di Blazor server:

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>Eliminazione di componenti con IDisposable

Se un componente implementa <xref:System.IDisposable>, il [metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose) viene chiamato quando il componente viene rimosso dall'interfaccia utente. Il componente seguente usa `@implements IDisposable` e il metodo `Dispose`:

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
> La chiamata di <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` non è supportata. `StateHasChanged` potrebbe essere richiamato nell'ambito della rimozione del renderer, quindi la richiesta degli aggiornamenti dell'interfaccia utente in quel momento non è supportata.

Annulla la sottoscrizione di gestori eventi da eventi .NET. Gli esempi di [Blazor form](xref:blazor/forms-validation) seguenti illustrano come eseguire l'unhook di un gestore eventi nel metodo `Dispose`:

* Approccio basato su campo privato e lambda

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-1.razor?highlight=23,28)]

* Approccio metodo privato

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-2.razor?highlight=16,26)]

## <a name="handle-errors"></a>Gestire gli errori

Per informazioni sulla gestione degli errori durante l'esecuzione del metodo del ciclo di vita, vedere <xref:blazor/handle-errors#lifecycle-methods>.

## <a name="stateful-reconnection-after-prerendering"></a>Riconnessione con stato dopo il rendering preliminare

In un'app Server Blazor quando `RenderMode` è `ServerPrerendered`, il componente viene inizialmente sottoposto a rendering statico come parte della pagina. Quando il browser stabilisce una connessione al server, viene *nuovamente*eseguito il rendering del componente e il componente è ora interattivo. Se è presente il metodo [{Async} Lifecycle OnInitialized](xref:blazor/lifecycle#component-initialization-methods) per l'inizializzazione del componente, il metodo viene eseguito *due volte*:

* Quando il componente viene preeseguito in modo statico.
* Una volta stabilita la connessione al server.

Ciò può comportare una modifica evidente nei dati visualizzati nell'interfaccia utente quando il componente viene sottoposto a rendering.

Per evitare lo scenario di doppio rendering in un'app Server Blazor:

* Passare un identificatore che può essere usato per memorizzare nella cache lo stato durante il prerendering e recuperare lo stato dopo il riavvio dell'app.
* Utilizzare l'identificatore durante il prerendering per salvare lo stato del componente.
* Utilizzare l'identificatore dopo il prerendering per recuperare lo stato memorizzato nella cache.

Il codice seguente illustra un `WeatherForecastService` aggiornato in un'app Server Blazor basata su modelli che evita il doppio rendering:

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

Per ulteriori informazioni sulla `RenderMode`, vedere <xref:blazor/hosting-model-configuration#render-mode>.

## <a name="detect-when-the-app-is-prerendering"></a>Rilevare il momento in cui viene eseguito il prerendering dell'app

[!INCLUDE[](~/includes/blazor-prerendering.md)]
