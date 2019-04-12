---
title: Componenti di Razor routing
author: guardrex
description: Informazioni su come instradare le richieste nelle App e sul componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/routing
ms.openlocfilehash: ef82fa7e0d571979a43fd8ce712bf4f22c06f9a7
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515548"
---
# <a name="razor-components-routing"></a>Componenti di Razor routing

Di [Luke Latham](https://github.com/guardrex)

Informazioni su come instradare le richieste nelle App e sul componente NavLink.

## <a name="aspnet-core-endpoint-routing-integration"></a>Integrazione di routing di ASP.NET Core endpoint

I componenti di Razor sono integrati [routing di ASP.NET Core](xref:fundamentals/routing). Configurato per accettare le connessioni in ingresso per i componenti di Razor interattiva con un'app ASP.NET Core `MapComponentHub<TComponent>` in `Startup.Configure`. `MapComponentHub` Specifica che il componente radice `App` deve essere eseguito il rendering all'interno di un elemento DOM corrispondente selettore `app`:

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a>Modelli di route

Il `<Router>` componente Abilita routing e viene fornito un modello di route per ogni componente accessibile. Il `<Router>` viene visualizzato nel componente le *Components/App.razor* file:

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

Quando un *.razor* oppure *. cshtml* del file con un' `@page` direttiva viene compilata, la classe generata viene assegnata un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> che specifica il modello di route. In fase di esecuzione, il router cerca classi di componenti con un `RouteAttribute` ed esegue il rendering qualunque sia il componente dispone di un modello di route corrispondente all'URL richiesto.

Più modelli di route possono essere applicati a un componente. Il seguente componente risponde alle richieste per `/BlazorRoute` e `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` supporta l'impostazione di un componente di fallback per il rendering quando una route richiesta viene risolta. Abilitare questo scenario di consenso esplicito, impostando il `FallbackComponent` parametro nel tipo della classe del componente di fallback.

L'esempio seguente imposta un componente definito nelle *Pages/MyFallbackRazorComponent.cshtml* come componente di fallback per un `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Per generare le route in modo corretto, l'app deve includere un `<base>` tag nel relativo *wwwroot/index.html* file con il percorso base dell'app specificato nel `href` attributo (`<base href="/">`). Per altre informazioni, vedere <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.

## <a name="route-parameters"></a>Parametri di route

Il router usa i parametri di route per popolare i parametri del componente corrispondente con lo stesso nome (maiuscole e minuscole):

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

I parametri facoltativi non sono supportati, pertanto, due `@page` direttive vengono applicate nell'esempio precedente. La prima consente al componente senza un parametro di navigazione. La seconda `@page` direttiva accetta il `{text}` parametro di route e assegna il valore per il `Text` proprietà.

## <a name="route-constraints"></a>Vincoli di route

Un vincolo di route consente di applicare tipo corrispondente in un segmento di route a un componente.

Nell'esempio seguente, la route per il componente di utenti corrisponde solo se:

* Un `Id` segmento di route è presente nell'URL della richiesta.
* Il `Id` segmento è un numero intero (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

I vincoli di route illustrati nella tabella seguente sono disponibili. Per i vincoli di route corrispondenti con le impostazioni cultura invarianti, vedere l'avviso sotto la tabella per altre informazioni.

| Vincolo | Esempio           | Esempi di corrispondenza                                                                  | Invariante<br>impostazioni cultura<br>corrispondenti |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | No                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Yes                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Yes                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Yes                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Yes                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | No                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Yes                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Yes                              |

> [!WARNING]
> I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica, presupponendo che l'URL sia non localizzabile.

## <a name="navlink-component"></a>Componente NavLink

Utilizzare un componente NavLink al posto di HTML `<a>` elementi durante la creazione di collegamenti di navigazione. Un componente NavLink si comporta come un `<a>` elemento, ad eccezione del fatto che attiva o disattiva un `active` classe CSS a seconda che il `href` corrisponda all'URL corrente. Il `active` classe consente all'utente di comprendere quali della pagina è la pagina attiva tra i collegamenti di navigazione visualizzati.

Il componente NavMenu seguente crea una [Bootstrap](https://getbootstrap.com/docs/) barra di spostamento che illustra come usare i componenti NavLink:

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

Esistono due `NavLinkMatch` opzioni:

* `NavLinkMatch.All` &ndash; Specifica che il NavLink deve essere attivo quando individua una corrispondenza l'intero URL corrente.
* `NavLinkMatch.Prefix` &ndash; Specifica che il NavLink deve essere attivo quando corrisponde a qualsiasi prefisso dell'URL corrente.

Nell'esempio precedente, la Home NavLink (`href=""`) corrisponde a tutti gli URL e riceve sempre il `active` classe CSS. Il secondo NavLink riceve solo le `active` classe quando l'utente visita il componente Blazor Route (`href="BlazorRoute"`).
