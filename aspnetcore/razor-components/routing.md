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
# <a name="razor-components-routing"></a><span data-ttu-id="5006f-103">Componenti di Razor routing</span><span class="sxs-lookup"><span data-stu-id="5006f-103">Razor Components routing</span></span>

<span data-ttu-id="5006f-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5006f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5006f-105">Informazioni su come instradare le richieste nelle App e sul componente NavLink.</span><span class="sxs-lookup"><span data-stu-id="5006f-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="5006f-106">Integrazione di routing di ASP.NET Core endpoint</span><span class="sxs-lookup"><span data-stu-id="5006f-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="5006f-107">I componenti di Razor sono integrati [routing di ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="5006f-107">Razor Components are integrated into [ASP.NET Core routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="5006f-108">Configurato per accettare le connessioni in ingresso per i componenti di Razor interattiva con un'app ASP.NET Core `MapComponentHub<TComponent>` in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="5006f-108">An ASP.NET Core app is configured to accept incoming connections for interactive Razor Components with `MapComponentHub<TComponent>` in `Startup.Configure`.</span></span> `MapComponentHub` <span data-ttu-id="5006f-109">Specifica che il componente radice `App` deve essere eseguito il rendering all'interno di un elemento DOM corrispondente selettore `app`:</span><span class="sxs-lookup"><span data-stu-id="5006f-109">specifies that the root component `App` should be rendered within a DOM element matching the selector `app`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a><span data-ttu-id="5006f-110">Modelli di route</span><span class="sxs-lookup"><span data-stu-id="5006f-110">Route templates</span></span>

<span data-ttu-id="5006f-111">Il `<Router>` componente Abilita routing e viene fornito un modello di route per ogni componente accessibile.</span><span class="sxs-lookup"><span data-stu-id="5006f-111">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="5006f-112">Il `<Router>` viene visualizzato nel componente le *Components/App.razor* file:</span><span class="sxs-lookup"><span data-stu-id="5006f-112">The `<Router>` component appears in the *Components/App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="5006f-113">Quando un *.razor* oppure *. cshtml* del file con un' `@page` direttiva viene compilata, la classe generata viene assegnata un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> che specifica il modello di route.</span><span class="sxs-lookup"><span data-stu-id="5006f-113">When a *.razor* or *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="5006f-114">In fase di esecuzione, il router cerca classi di componenti con un `RouteAttribute` ed esegue il rendering qualunque sia il componente dispone di un modello di route corrispondente all'URL richiesto.</span><span class="sxs-lookup"><span data-stu-id="5006f-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="5006f-115">Più modelli di route possono essere applicati a un componente.</span><span class="sxs-lookup"><span data-stu-id="5006f-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="5006f-116">Il seguente componente risponde alle richieste per `/BlazorRoute` e `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="5006f-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` <span data-ttu-id="5006f-117">supporta l'impostazione di un componente di fallback per il rendering quando una route richiesta viene risolta.</span><span class="sxs-lookup"><span data-stu-id="5006f-117">supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="5006f-118">Abilitare questo scenario di consenso esplicito, impostando il `FallbackComponent` parametro nel tipo della classe del componente di fallback.</span><span class="sxs-lookup"><span data-stu-id="5006f-118">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="5006f-119">L'esempio seguente imposta un componente definito nelle *Pages/MyFallbackRazorComponent.cshtml* come componente di fallback per un `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="5006f-119">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="5006f-120">Per generare le route in modo corretto, l'app deve includere un `<base>` tag nel relativo *wwwroot/index.html* file con il percorso base dell'app specificato nel `href` attributo (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="5006f-120">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="5006f-121">Per altre informazioni, vedere <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="5006f-121">For more information, see <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="5006f-122">Parametri di route</span><span class="sxs-lookup"><span data-stu-id="5006f-122">Route parameters</span></span>

<span data-ttu-id="5006f-123">Il router usa i parametri di route per popolare i parametri del componente corrispondente con lo stesso nome (maiuscole e minuscole):</span><span class="sxs-lookup"><span data-stu-id="5006f-123">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="5006f-124">I parametri facoltativi non sono supportati, pertanto, due `@page` direttive vengono applicate nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="5006f-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="5006f-125">La prima consente al componente senza un parametro di navigazione.</span><span class="sxs-lookup"><span data-stu-id="5006f-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="5006f-126">La seconda `@page` direttiva accetta il `{text}` parametro di route e assegna il valore per il `Text` proprietà.</span><span class="sxs-lookup"><span data-stu-id="5006f-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="5006f-127">Vincoli di route</span><span class="sxs-lookup"><span data-stu-id="5006f-127">Route constraints</span></span>

<span data-ttu-id="5006f-128">Un vincolo di route consente di applicare tipo corrispondente in un segmento di route a un componente.</span><span class="sxs-lookup"><span data-stu-id="5006f-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="5006f-129">Nell'esempio seguente, la route per il componente di utenti corrisponde solo se:</span><span class="sxs-lookup"><span data-stu-id="5006f-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="5006f-130">Un `Id` segmento di route è presente nell'URL della richiesta.</span><span class="sxs-lookup"><span data-stu-id="5006f-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="5006f-131">Il `Id` segmento è un numero intero (`int`).</span><span class="sxs-lookup"><span data-stu-id="5006f-131">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

<span data-ttu-id="5006f-132">I vincoli di route illustrati nella tabella seguente sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="5006f-132">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="5006f-133">Per i vincoli di route corrispondenti con le impostazioni cultura invarianti, vedere l'avviso sotto la tabella per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="5006f-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="5006f-134">Vincolo</span><span class="sxs-lookup"><span data-stu-id="5006f-134">Constraint</span></span> | <span data-ttu-id="5006f-135">Esempio</span><span class="sxs-lookup"><span data-stu-id="5006f-135">Example</span></span>           | <span data-ttu-id="5006f-136">Esempi di corrispondenza</span><span class="sxs-lookup"><span data-stu-id="5006f-136">Example Matches</span></span>                                                                  | <span data-ttu-id="5006f-137">Invariante</span><span class="sxs-lookup"><span data-stu-id="5006f-137">Invariant</span></span><br><span data-ttu-id="5006f-138">impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="5006f-138">culture</span></span><br><span data-ttu-id="5006f-139">corrispondenti</span><span class="sxs-lookup"><span data-stu-id="5006f-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`<span data-ttu-id="5006f-140">,</span><span class="sxs-lookup"><span data-stu-id="5006f-140">,</span></span> `FALSE`                                                                  | <span data-ttu-id="5006f-141">No</span><span class="sxs-lookup"><span data-stu-id="5006f-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`<span data-ttu-id="5006f-142">,</span><span class="sxs-lookup"><span data-stu-id="5006f-142">,</span></span> `2016-12-31 7:32pm`                                                | <span data-ttu-id="5006f-143">Yes</span><span class="sxs-lookup"><span data-stu-id="5006f-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | `49.99`<span data-ttu-id="5006f-144">,</span><span class="sxs-lookup"><span data-stu-id="5006f-144">,</span></span> `-1,000.01`                                                             | <span data-ttu-id="5006f-145">Yes</span><span class="sxs-lookup"><span data-stu-id="5006f-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | `1.234`<span data-ttu-id="5006f-146">,</span><span class="sxs-lookup"><span data-stu-id="5006f-146">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="5006f-147">Yes</span><span class="sxs-lookup"><span data-stu-id="5006f-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | `1.234`<span data-ttu-id="5006f-148">,</span><span class="sxs-lookup"><span data-stu-id="5006f-148">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="5006f-149">Yes</span><span class="sxs-lookup"><span data-stu-id="5006f-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`<span data-ttu-id="5006f-150">,</span><span class="sxs-lookup"><span data-stu-id="5006f-150">,</span></span> `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | <span data-ttu-id="5006f-151">No</span><span class="sxs-lookup"><span data-stu-id="5006f-151">No</span></span>                               |
| `int`      | `{id:int}`        | `123456789`<span data-ttu-id="5006f-152">,</span><span class="sxs-lookup"><span data-stu-id="5006f-152">,</span></span> `-123456789`                                                        | <span data-ttu-id="5006f-153">Yes</span><span class="sxs-lookup"><span data-stu-id="5006f-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | `123456789`<span data-ttu-id="5006f-154">,</span><span class="sxs-lookup"><span data-stu-id="5006f-154">,</span></span> `-123456789`                                                        | <span data-ttu-id="5006f-155">Yes</span><span class="sxs-lookup"><span data-stu-id="5006f-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="5006f-156">I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica,</span><span class="sxs-lookup"><span data-stu-id="5006f-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="5006f-157">presupponendo che l'URL sia non localizzabile.</span><span class="sxs-lookup"><span data-stu-id="5006f-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="5006f-158">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="5006f-158">NavLink component</span></span>

<span data-ttu-id="5006f-159">Utilizzare un componente NavLink al posto di HTML `<a>` elementi durante la creazione di collegamenti di navigazione.</span><span class="sxs-lookup"><span data-stu-id="5006f-159">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="5006f-160">Un componente NavLink si comporta come un `<a>` elemento, ad eccezione del fatto che attiva o disattiva un `active` classe CSS a seconda che il `href` corrisponda all'URL corrente.</span><span class="sxs-lookup"><span data-stu-id="5006f-160">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="5006f-161">Il `active` classe consente all'utente di comprendere quali della pagina è la pagina attiva tra i collegamenti di navigazione visualizzati.</span><span class="sxs-lookup"><span data-stu-id="5006f-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="5006f-162">Il componente NavMenu seguente crea una [Bootstrap](https://getbootstrap.com/docs/) barra di spostamento che illustra come usare i componenti NavLink:</span><span class="sxs-lookup"><span data-stu-id="5006f-162">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="5006f-163">Esistono due `NavLinkMatch` opzioni:</span><span class="sxs-lookup"><span data-stu-id="5006f-163">There are two `NavLinkMatch` options:</span></span>

* `NavLinkMatch.All` <span data-ttu-id="5006f-164">&ndash; Specifica che il NavLink deve essere attivo quando individua una corrispondenza l'intero URL corrente.</span><span class="sxs-lookup"><span data-stu-id="5006f-164">&ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* `NavLinkMatch.Prefix` <span data-ttu-id="5006f-165">&ndash; Specifica che il NavLink deve essere attivo quando corrisponde a qualsiasi prefisso dell'URL corrente.</span><span class="sxs-lookup"><span data-stu-id="5006f-165">&ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="5006f-166">Nell'esempio precedente, la Home NavLink (`href=""`) corrisponde a tutti gli URL e riceve sempre il `active` classe CSS.</span><span class="sxs-lookup"><span data-stu-id="5006f-166">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="5006f-167">Il secondo NavLink riceve solo le `active` classe quando l'utente visita il componente Blazor Route (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="5006f-167">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
