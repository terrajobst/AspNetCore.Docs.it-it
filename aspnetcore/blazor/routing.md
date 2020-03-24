---
title: Routing ASP.NET Core Blazor
author: guardrex
description: Informazioni su come instradare le richieste nelle app e sul componente NavLink.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/routing
ms.openlocfilehash: 87579c88a37e0258921e199db2b5d8c7627f5499
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218895"
---
# <a name="aspnet-core-blazor-routing"></a>Routing di ASP.NET Core Blazor

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Informazioni su come instradare le richieste e su come usare il componente `NavLink` per creare collegamenti di navigazione nelle app blazer.

## <a name="aspnet-core-endpoint-routing-integration"></a>Integrazione di routing endpoint ASP.NET Core

Il server blazer è integrato nel [Routing ASP.NET Core endpoint](xref:fundamentals/routing). Un'app ASP.NET Core è configurata in modo da accettare le connessioni in ingresso per i componenti interattivi con `MapBlazorHub` in `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

La configurazione più tipica consiste nel routing di tutte le richieste a una pagina Razor, che funge da host per la parte lato server dell'app del server Blaze. Per convenzione, la pagina *host* è in genere denominata *_Host. cshtml*. La route specificata nel file host viene chiamata route di *fallback* perché funziona con una priorità bassa nella corrispondenza della route. La route di fallback viene considerata quando altre route non corrispondono. Ciò consente all'app di usare altri controller e pagine senza interferire con l'app del server Blaze.

## <a name="route-templates"></a>Modelli di route

Il componente `Router` consente il routing a ogni componente con una route specificata. Il componente `Router` viene visualizzato nel file *app. Razor* :

```razor
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

Quando viene compilato un file *Razor* con una direttiva `@page`, alla classe generata viene fornito un <xref:Microsoft.AspNetCore.Components.RouteAttribute> specificando il modello di route.

In fase di esecuzione, il componente `RouteView`:

* Riceve il `RouteData` dall'`Router` insieme a tutti i parametri desiderati.
* Esegue il rendering del componente specificato con il layout (o un layout predefinito facoltativo) utilizzando i parametri specificati.

Facoltativamente, è possibile specificare un parametro `DefaultLayout` con una classe layout da utilizzare per i componenti che non specificano un layout. I modelli di Blazer predefiniti specificano il componente `MainLayout`. *MainLayout. Razor* si trova nella cartella *condivisa* del progetto modello. Per ulteriori informazioni sui layout, vedere <xref:blazor/layouts>.

È possibile applicare più modelli di route a un componente. Il componente seguente risponde alle richieste di `/BlazorRoute` e `/DifferentBlazorRoute`:

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

> [!IMPORTANT]
> Per risolvere correttamente gli URL, è necessario che l'app includa un tag `<base>` nel file *wwwroot/index.html* (Webassembly Blazer) o nel file *pages/_Host. cshtml* (server Blazer) con il percorso di base dell'app specificato nell'attributo `href` (`<base href="/">`). Per altre informazioni, vedere <xref:host-and-deploy/blazor/index#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Fornire contenuto personalizzato quando il contenuto non è stato trovato

Il componente `Router` consente all'app di specificare contenuto personalizzato se non viene trovato contenuto per la route richiesta.

Nel file *app. Razor* impostare il contenuto personalizzato nel parametro del modello di `NotFound` del componente `Router`:

```razor
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

Il contenuto dei tag `<NotFound>` può includere elementi arbitrari, ad esempio altri componenti interattivi. Per applicare un layout predefinito a `NotFound` contenuto, vedere <xref:blazor/layouts>.

## <a name="route-to-components-from-multiple-assemblies"></a>Routing a componenti da più assembly

Usare il parametro `AdditionalAssemblies` per specificare gli assembly aggiuntivi da considerare per il componente `Router` durante la ricerca di componenti instradabili. Gli assembly specificati vengono considerati oltre all'assembly specificato `AppAssembly`. Nell'esempio seguente `Component1` è un componente instradabile definito in una libreria di classi a cui si fa riferimento. Nell'esempio di `AdditionalAssemblies` seguente viene restituito il supporto del routing per `Component1`:

```razor
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a>Parametri di route

Il router usa parametri di route per popolare i parametri del componente corrispondenti con lo stesso nome (senza distinzione tra maiuscole e minuscole):

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

I parametri facoltativi non sono supportati. Nell'esempio precedente vengono applicate due direttive `@page`. Il primo consente la navigazione al componente senza un parametro. La seconda direttiva `@page` accetta il parametro di route `{text}` e assegna il valore alla proprietà `Text`.

## <a name="route-constraints"></a>Vincoli di route

Un vincolo di route impone la corrispondenza tra i tipi in un segmento di route e un componente.

Nell'esempio seguente, la route per il componente `Users` corrisponde solo a se:

* Un segmento di route `Id` è presente nell'URL della richiesta.
* Il segmento `Id` è un numero intero (`int`).

[!code-razor[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Sono disponibili i vincoli di route indicati nella tabella seguente. Per ulteriori informazioni, vedere l'avviso sotto la tabella per i vincoli di route corrispondenti alla lingua inglese.

| Vincolo | Esempio           | Esempi di corrispondenza                                                                  | Invariante<br>culture<br>corrispondenze |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | No                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Sì                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Sì                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Sì                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Sì                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | No                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Sì                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Sì                              |

> [!WARNING]
> I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica, presupponendo che l'URL sia non localizzabile.

### <a name="routing-with-urls-that-contain-dots"></a>Routing con URL contenenti punti

Nelle app del server blazer, la route predefinita in *_Host. cshtml* è `/` (`@page "/"`). Un URL di richiesta che contiene un punto (`.`) non corrisponde alla route predefinita perché l'URL viene visualizzato per richiedere un file. Un'app Blazer restituisce una risposta *404 non trovata* per un file statico che non esiste. Per usare le route che contengono un punto, configurare *_Host. cshtml* con il modello di route seguente:

```cshtml
@page "/{**path}"
```

Il modello di `"/{**path}"` include:

* Sintassi Double-asterisco *catch-all* (`**`) per acquisire il percorso tra più limiti di cartella senza barre di codifica (`/`).
* nome del parametro di route `path`.

> [!NOTE]
> La sintassi dei parametri *catch-all* (`*`/`**`) **non** è supportata nei componenti Razor (*Razor*).

Per altre informazioni, vedere <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Componente NavLink

Usare un componente `NavLink` al posto degli elementi collegamento ipertestuale HTML (`<a>`) durante la creazione di collegamenti di navigazione. Un componente `NavLink` si comporta come un elemento `<a>`, con la differenza che viene attivata o disattivata una classe `active` CSS a seconda che la `href` corrisponda all'URL corrente. La classe `active` consente agli utenti di comprendere quale pagina è la pagina attiva tra i collegamenti di navigazione visualizzati.

Il componente `NavMenu` seguente crea una barra di navigazione [bootstrap](https://getbootstrap.com/docs/) che illustra come usare `NavLink` componenti:

[!code-razor[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Sono disponibili due opzioni di `NavLinkMatch` che è possibile assegnare all'attributo `Match` dell'elemento `<NavLink>`:

* `NavLinkMatch.All` &ndash; il `NavLink` è attivo quando corrisponde all'intero URL corrente.
* `NavLinkMatch.Prefix` (*impostazione predefinita*) &ndash; il `NavLink` è attivo quando corrisponde a qualsiasi prefisso dell'URL corrente.

Nell'esempio precedente, il `href=""` Home `NavLink` corrisponde all'URL Home e riceve solo la classe `active` CSS nell'URL del percorso di base predefinito dell'app, ad esempio `https://localhost:5001/`. Il secondo `NavLink` riceve la classe `active` quando l'utente visita un URL con un prefisso `MyComponent` (ad esempio, `https://localhost:5001/MyComponent` e `https://localhost:5001/MyComponent/AnotherSegment`).

Gli attributi aggiuntivi del componente `NavLink` vengono passati al tag di ancoraggio sottoposto a rendering. Nell'esempio seguente il componente `NavLink` include l'attributo `target`:

```razor
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Viene eseguito il rendering del markup HTML seguente:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>Helper per lo stato di navigazione e URI

Usare <xref:Microsoft.AspNetCore.Components.NavigationManager> per lavorare con gli URI e la C# navigazione nel codice. `NavigationManager` fornisce l'evento e i metodi illustrati nella tabella seguente.

| Membro | Descrizione |
| ------ | ----------- |
| Uri | Ottiene l'URI assoluto corrente. |
| BaseUri | Ottiene l'URI di base (con una barra finale) che può essere anteposto ai percorsi URI relativi per produrre un URI assoluto. In genere, `BaseUri` corrisponde all'attributo `href` sull'elemento `<base>` del documento in *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server). |
| NavigateTo | Passa all'URI specificato. Se `forceLoad` è `true`:<ul><li>Il routing lato client viene ignorato.</li><li>Il browser è forzato a caricare la nuova pagina dal server, indipendentemente dal fatto che l'URI venga normalmente gestito dal router lato client.</li></ul> |
| LocationChanged | Un evento che viene attivato quando il percorso di navigazione viene modificato. |
| ToAbsoluteUri | Converte un URI relativo in un URI assoluto. |
| <span style="word-break:normal;word-wrap:normal">ToBaseRelativePath</span> | Dato un URI di base (ad esempio, un URI restituito in precedenza da `GetBaseUri`), converte un URI assoluto in un URI relativo al prefisso URI di base. |

Quando si seleziona il pulsante, il componente seguente passa al componente `Counter` dell'app:

```razor
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```

Il componente seguente gestisce un evento di modifica della posizione. Il metodo `HandleLocationChanged` viene disassociato quando `Dispose` viene chiamato dal Framework. L'unhook del metodo consente Garbage Collection del componente.

```razor
@implement IDisposable
@inject NavigationManager NavigationManager

...

protected override void OnInitialized()
{
    NavigationManager.LocationChanged += HandleLocationChanged;
}

private void HandleLocationChanged(object sender, LocationChangedEventArgs e)
{
    ...
}

public void Dispose()
{
    NavigationManager.LocationChanged -= HandleLocationChanged;
}
```

<xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs> fornisce le informazioni seguenti sull'evento:

* <xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location> &ndash; l'URL della nuova posizione.
* <xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted> &ndash; se `true`, Blazor intercettato la navigazione dal browser. Se `false`, [NavigationManager. NavigateTo](xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A) ha causato la navigazione.

Per ulteriori informazioni sull'eliminazione dei componenti, vedere <xref:blazor/lifecycle#component-disposal-with-idisposable>.
