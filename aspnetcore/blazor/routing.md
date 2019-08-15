---
title: Routing di ASP.NET Core Blazer
author: guardrex
description: Informazioni su come instradare le richieste nelle app e sul componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/routing
ms.openlocfilehash: 197b1a91b3540d21639c3ee775b2c490da7b23fe
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030397"
---
# <a name="aspnet-core-blazor-routing"></a>Routing di ASP.NET Core Blazer

Di [Luke Latham](https://github.com/guardrex)

Informazioni su come instradare le richieste e su `NavLink` come usare il componente per creare collegamenti di navigazione nelle app blazer.

## <a name="aspnet-core-endpoint-routing-integration"></a>Integrazione di routing endpoint ASP.NET Core

Il lato server di Blazer è integrato in [ASP.net core routing degli endpoint](xref:fundamentals/routing). Un'app ASP.NET Core è configurata in modo da accettare le connessioni in `MapBlazorHub` ingresso `Startup.Configure`per i componenti interattivi con in:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a>Modelli di route

Il `Router` componente Abilita il routing e viene fornito un modello di route a ogni componente accessibile. Il `Router` componente viene visualizzato nel file *app. Razor* :

In un'app Blazer sul lato server:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

In un'app Blazer sul lato client:

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

Quando viene compilato un file *Razor* con `@page` una direttiva, alla classe generata viene fornito un oggetto <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> che specifica il modello di route. In fase di esecuzione, il router cerca le classi di `RouteAttribute` componenti con un oggetto ed esegue il rendering del componente con un modello di route corrispondente all'URL richiesto.

È possibile applicare più modelli di route a un componente. Il componente seguente risponde alle richieste per `/BlazorRoute` e: `/DifferentBlazorRoute`

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Per generare correttamente le route, l'app deve includere `<base>` un tag nel file *wwwroot/index.html* (lato client Blazer) o nel file *pages/_Host. cshtml* (lato server fiammeggiar) con il percorso di base dell' `href` app specificato nell'attributo ( `<base href="/">`). Per altre informazioni, vedere <xref:host-and-deploy/blazor/client-side#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Fornire contenuto personalizzato quando il contenuto non è stato trovato

Il `Router` componente consente all'app di specificare contenuto personalizzato se non viene trovato contenuto per la route richiesta.

Nel file *app. Razor* impostare il contenuto personalizzato nell' `<NotFoundContent>` elemento del `Router` componente:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <NotFoundContent>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFoundContent>
</Router>
```

Il contenuto di `<NotFoundContent>` può includere elementi arbitrari, ad esempio altri componenti interattivi.

## <a name="route-parameters"></a>Parametri di route

Il router usa parametri di route per popolare i parametri del componente corrispondenti con lo stesso nome (senza distinzione tra maiuscole e minuscole):

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

I parametri facoltativi non sono supportati per le app Blazer nell'anteprima ASP.NET Core 3,0. Nell' `@page` esempio precedente vengono applicate due direttive. Il primo consente la navigazione al componente senza un parametro. La seconda `@page` direttiva accetta il `{text}` parametro Route e assegna il valore alla `Text` proprietà.

## <a name="route-constraints"></a>Vincoli di route

Un vincolo di route impone la corrispondenza tra i tipi in un segmento di route e un componente.

Nell'esempio seguente, la route per il `Users` componente corrisponde solo a se:

* Un `Id` segmento di route è presente nell'URL della richiesta.
* Il `Id` segmento è un numero intero`int`().

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Sono disponibili i vincoli di route indicati nella tabella seguente. Per ulteriori informazioni, vedere l'avviso sotto la tabella per i vincoli di route corrispondenti alla lingua inglese.

| Vincolo | Esempio           | Esempi di corrispondenza                                                                  | Invariante<br>culture<br>corrispondenti |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | No                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Sì                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Sì                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Sì                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Yes                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | No                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Sì                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Sì                              |

> [!WARNING]
> I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica, presupponendo che l'URL sia non localizzabile.

## <a name="navlink-component"></a>Componente NavLink

Usare un `NavLink` componente al posto di elementi collegamento ipertestuale`<a>`HTML () durante la creazione di collegamenti di navigazione. Un `NavLink` componente si comporta come un `<a>` elemento, con la differenza che viene attivata `active` o disattivata una classe `href` CSS a seconda che corrisponda all'URL corrente. La `active` classe consente a un utente di comprendere quale pagina è la pagina attiva tra i collegamenti di navigazione visualizzati.

Il componente `NavMenu` seguente crea una barra di navigazione [bootstrap](https://getbootstrap.com/docs/) che illustra come usare `NavLink` i componenti di:

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Sono disponibili due `NavLinkMatch` opzioni che è possibile assegnare `Match` all'attributo dell' `<NavLink>` elemento:

* `NavLinkMatch.All`L' oggetto`NavLink` è attivo quando corrisponde all'intero URL corrente. &ndash;
* `NavLinkMatch.Prefix`(*impostazione predefinita*) L' oggetto`NavLink` è attivo quando corrisponde a qualsiasi prefisso dell'URL corrente. &ndash;

Nell' `NavLink` esempio precedente la Home `href=""` corrisponde all'URL Home e riceve solo la `active` classe CSS nell'URL del percorso di base predefinito dell'app (ad esempio, `https://localhost:5001/`). Il secondo `NavLink` riceve la `active` classe quando l'utente visita un URL con un `MyComponent` prefisso, `https://localhost:5001/MyComponent` ad esempio e `https://localhost:5001/MyComponent/AnotherSegment`.

Gli `NavLink` attributi dei componenti aggiuntivi vengono passati al tag di ancoraggio sottoposto a rendering. Nell'esempio seguente il `NavLink` componente include l' `target` attributo:

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Viene eseguito il rendering del markup HTML seguente:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>Helper per lo stato di navigazione e URI

Usare `Microsoft.AspNetCore.Components.IUriHelper` per lavorare con gli URI e la C# navigazione nel codice. `IUriHelper`fornisce l'evento e i metodi illustrati nella tabella seguente.

| Member | DESCRIZIONE |
| ------ | ----------- |
| `GetAbsoluteUri` | Ottiene l'URI assoluto corrente. |
| `GetBaseUri` | Ottiene l'URI di base (con una barra finale) che può essere anteposto ai percorsi URI relativi per produrre un URI assoluto. `GetBaseUri` Corrisponde in genere `href` all'`<base>` attributo sull'elemento del documento in *wwwroot/index.html* (lato client Blazer) o *pages/_Host. cshtml* (lato server Blaze). |
| `NavigateTo` | Passa all'URI specificato. Se `forceLoad` è `true`:<ul><li>Il routing lato client viene ignorato.</li><li>Il browser è forzato a caricare la nuova pagina dal server, indipendentemente dal fatto che l'URI venga normalmente gestito dal router lato client.</li></ul> |
| `OnLocationChanged` | Un evento che viene attivato quando il percorso di navigazione viene modificato. |
| `ToAbsoluteUri` | Converte un URI relativo in un URI assoluto. |
| `ToBaseRelativePath` | Dato un URI di base (ad esempio, un URI restituito in `GetBaseUri`precedenza da), converte un URI assoluto in un URI relativo al prefisso URI di base. |

Quando si seleziona il pulsante, il componente seguente `Counter` passa al componente dell'app:

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```
