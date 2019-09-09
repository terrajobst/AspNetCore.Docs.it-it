---
title: Routing di ASP.NET Core Blazer
author: guardrex
description: Informazioni su come instradare le richieste nelle app e sul componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/routing
ms.openlocfilehash: d348908261c51b477aa698a407266d05c0df5a33
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800334"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="8ea13-103">Routing di ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="8ea13-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="8ea13-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8ea13-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8ea13-105">Informazioni su come instradare le richieste e su `NavLink` come usare il componente per creare collegamenti di navigazione nelle app blazer.</span><span class="sxs-lookup"><span data-stu-id="8ea13-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="8ea13-106">Integrazione di routing endpoint ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ea13-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="8ea13-107">Il lato server di Blazer è integrato in [ASP.net core routing degli endpoint](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="8ea13-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="8ea13-108">Un'app ASP.NET Core è configurata in modo da accettare le connessioni in `MapBlazorHub` ingresso `Startup.Configure`per i componenti interattivi con in:</span><span class="sxs-lookup"><span data-stu-id="8ea13-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="8ea13-109">Modelli di route</span><span class="sxs-lookup"><span data-stu-id="8ea13-109">Route templates</span></span>

<span data-ttu-id="8ea13-110">Il `Router` componente consente il routing a ogni componente con una route specificata.</span><span class="sxs-lookup"><span data-stu-id="8ea13-110">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="8ea13-111">Il `Router` componente viene visualizzato nel file *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="8ea13-111">The `Router` component appears in the *App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

<span data-ttu-id="8ea13-112">Quando viene compilato un file *Razor* con `@page` una direttiva, alla classe generata viene fornito un oggetto <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> che specifica il modello di route.</span><span class="sxs-lookup"><span data-stu-id="8ea13-112">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="8ea13-113">In fase di esecuzione `RouteView` , il componente:</span><span class="sxs-lookup"><span data-stu-id="8ea13-113">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="8ea13-114">`RouteData` Riceve`Router` da insieme a tutti i parametri desiderati.</span><span class="sxs-lookup"><span data-stu-id="8ea13-114">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="8ea13-115">Esegue il rendering del componente specificato con il layout (o un layout predefinito facoltativo) utilizzando i parametri specificati.</span><span class="sxs-lookup"><span data-stu-id="8ea13-115">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="8ea13-116">Facoltativamente, è possibile specificare `DefaultLayout` un parametro con una classe layout da utilizzare per i componenti che non specificano un layout.</span><span class="sxs-lookup"><span data-stu-id="8ea13-116">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="8ea13-117">I modelli di Blazer predefiniti specificano il `MainLayout` componente.</span><span class="sxs-lookup"><span data-stu-id="8ea13-117">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="8ea13-118">*MainLayout. Razor* si trova nella cartella *condivisa* del progetto modello.</span><span class="sxs-lookup"><span data-stu-id="8ea13-118">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="8ea13-119">Per ulteriori informazioni sui layout, vedere <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="8ea13-119">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="8ea13-120">È possibile applicare più modelli di route a un componente.</span><span class="sxs-lookup"><span data-stu-id="8ea13-120">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="8ea13-121">Il componente seguente risponde alle richieste per `/BlazorRoute` e: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="8ea13-121">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="8ea13-122">Per la corretta risoluzione degli URL, l'app deve includere `<base>` un tag nel file *wwwroot/index.html* (lato client Blazer) o nel file *pages/_Host. cshtml* (lato server fiammeggiar) con il percorso di base dell' `href` applicazione specificato nell'attributo ( `<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="8ea13-122">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="8ea13-123">Per altre informazioni, vedere <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="8ea13-123">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="8ea13-124">Fornire contenuto personalizzato quando il contenuto non è stato trovato</span><span class="sxs-lookup"><span data-stu-id="8ea13-124">Provide custom content when content isn't found</span></span>

<span data-ttu-id="8ea13-125">Il `Router` componente consente all'app di specificare contenuto personalizzato se non viene trovato contenuto per la route richiesta.</span><span class="sxs-lookup"><span data-stu-id="8ea13-125">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="8ea13-126">Nel file *app. Razor* impostare il contenuto personalizzato nel `NotFound` parametro del `Router` modello del componente:</span><span class="sxs-lookup"><span data-stu-id="8ea13-126">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

```cshtml
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

<span data-ttu-id="8ea13-127">Il contenuto dei `<NotFound>` tag può includere elementi arbitrari, ad esempio altri componenti interattivi.</span><span class="sxs-lookup"><span data-stu-id="8ea13-127">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="8ea13-128">Per applicare un layout predefinito al `NotFound` contenuto, vedere <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="8ea13-128">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="8ea13-129">Routing a componenti da più assembly</span><span class="sxs-lookup"><span data-stu-id="8ea13-129">Route to components from multiple assemblies</span></span>

<span data-ttu-id="8ea13-130">Usare il `AdditionalAssemblies` parametro per specificare gli assembly aggiuntivi da `Router` considerare per il componente durante la ricerca di componenti instradabili.</span><span class="sxs-lookup"><span data-stu-id="8ea13-130">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="8ea13-131">Gli `AppAssembly`assembly specificati sono considerati oltre all'assembly specificato da.</span><span class="sxs-lookup"><span data-stu-id="8ea13-131">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="8ea13-132">Nell'esempio `Component1` seguente è un componente instradabile definito in una libreria di classi a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="8ea13-132">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="8ea13-133">Nell'esempio `AdditionalAssemblies` seguente viene restituito il supporto del `Component1`routing per:</span><span class="sxs-lookup"><span data-stu-id="8ea13-133">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="8ea13-134">< router AppAssembly = "typeof (Program). Assembly "AdditionalAssemblies =" New [] {typeof (Component1). > Assembly}...</Router></span><span class="sxs-lookup"><span data-stu-id="8ea13-134"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="8ea13-135">Parametri di route</span><span class="sxs-lookup"><span data-stu-id="8ea13-135">Route parameters</span></span>

<span data-ttu-id="8ea13-136">Il router usa parametri di route per popolare i parametri del componente corrispondenti con lo stesso nome (senza distinzione tra maiuscole e minuscole):</span><span class="sxs-lookup"><span data-stu-id="8ea13-136">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="8ea13-137">I parametri facoltativi non sono supportati per le app Blazer nell'anteprima ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="8ea13-137">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="8ea13-138">Nell' `@page` esempio precedente vengono applicate due direttive.</span><span class="sxs-lookup"><span data-stu-id="8ea13-138">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="8ea13-139">Il primo consente la navigazione al componente senza un parametro.</span><span class="sxs-lookup"><span data-stu-id="8ea13-139">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="8ea13-140">La seconda `@page` direttiva accetta il `{text}` parametro Route e assegna il valore alla `Text` proprietà.</span><span class="sxs-lookup"><span data-stu-id="8ea13-140">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="8ea13-141">Vincoli di route</span><span class="sxs-lookup"><span data-stu-id="8ea13-141">Route constraints</span></span>

<span data-ttu-id="8ea13-142">Un vincolo di route impone la corrispondenza tra i tipi in un segmento di route e un componente.</span><span class="sxs-lookup"><span data-stu-id="8ea13-142">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="8ea13-143">Nell'esempio seguente, la route per il `Users` componente corrisponde solo a se:</span><span class="sxs-lookup"><span data-stu-id="8ea13-143">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="8ea13-144">Un `Id` segmento di route è presente nell'URL della richiesta.</span><span class="sxs-lookup"><span data-stu-id="8ea13-144">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="8ea13-145">Il `Id` segmento è un numero intero`int`().</span><span class="sxs-lookup"><span data-stu-id="8ea13-145">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="8ea13-146">Sono disponibili i vincoli di route indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="8ea13-146">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="8ea13-147">Per ulteriori informazioni, vedere l'avviso sotto la tabella per i vincoli di route corrispondenti alla lingua inglese.</span><span class="sxs-lookup"><span data-stu-id="8ea13-147">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="8ea13-148">Vincolo</span><span class="sxs-lookup"><span data-stu-id="8ea13-148">Constraint</span></span> | <span data-ttu-id="8ea13-149">Esempio</span><span class="sxs-lookup"><span data-stu-id="8ea13-149">Example</span></span>           | <span data-ttu-id="8ea13-150">Esempi di corrispondenza</span><span class="sxs-lookup"><span data-stu-id="8ea13-150">Example Matches</span></span>                                                                  | <span data-ttu-id="8ea13-151">Invariante</span><span class="sxs-lookup"><span data-stu-id="8ea13-151">Invariant</span></span><br><span data-ttu-id="8ea13-152">culture</span><span class="sxs-lookup"><span data-stu-id="8ea13-152">culture</span></span><br><span data-ttu-id="8ea13-153">corrispondenti</span><span class="sxs-lookup"><span data-stu-id="8ea13-153">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="8ea13-154">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="8ea13-154">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="8ea13-155">No</span><span class="sxs-lookup"><span data-stu-id="8ea13-155">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="8ea13-156">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="8ea13-156">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="8ea13-157">Sì</span><span class="sxs-lookup"><span data-stu-id="8ea13-157">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="8ea13-158">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="8ea13-158">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="8ea13-159">Yes</span><span class="sxs-lookup"><span data-stu-id="8ea13-159">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="8ea13-160">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="8ea13-160">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="8ea13-161">Sì</span><span class="sxs-lookup"><span data-stu-id="8ea13-161">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="8ea13-162">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="8ea13-162">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="8ea13-163">Sì</span><span class="sxs-lookup"><span data-stu-id="8ea13-163">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="8ea13-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="8ea13-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="8ea13-165">No</span><span class="sxs-lookup"><span data-stu-id="8ea13-165">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="8ea13-166">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="8ea13-166">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="8ea13-167">Sì</span><span class="sxs-lookup"><span data-stu-id="8ea13-167">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="8ea13-168">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="8ea13-168">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="8ea13-169">Sì</span><span class="sxs-lookup"><span data-stu-id="8ea13-169">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="8ea13-170">I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica,</span><span class="sxs-lookup"><span data-stu-id="8ea13-170">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="8ea13-171">presupponendo che l'URL sia non localizzabile.</span><span class="sxs-lookup"><span data-stu-id="8ea13-171">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="8ea13-172">Routing con URL contenenti punti</span><span class="sxs-lookup"><span data-stu-id="8ea13-172">Routing with URLs that contain dots</span></span>

<span data-ttu-id="8ea13-173">Nelle app del lato server di Blaze la route predefinita in *_Host. cshtml* è `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="8ea13-173">In Blazor server-side apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="8ea13-174">Un URL di richiesta che contiene un punto`.`() non corrisponde alla route predefinita perché l'URL viene visualizzato per richiedere un file.</span><span class="sxs-lookup"><span data-stu-id="8ea13-174">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="8ea13-175">Un'app Blazer restituisce una risposta *404 non trovata* per un file statico che non esiste.</span><span class="sxs-lookup"><span data-stu-id="8ea13-175">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="8ea13-176">Per usare le route che contengono un punto, configurare *_Host. cshtml* con il modello di route seguente:</span><span class="sxs-lookup"><span data-stu-id="8ea13-176">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="8ea13-177">Il `"/{**path}"` modello include:</span><span class="sxs-lookup"><span data-stu-id="8ea13-177">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="8ea13-178">Double-asterisco *catch-all* Syntax (`**`) per acquisire il percorso tra più limiti di cartelle senza codificare le barre`/`().</span><span class="sxs-lookup"><span data-stu-id="8ea13-178">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="8ea13-179">Nome `path` di un parametro di route.</span><span class="sxs-lookup"><span data-stu-id="8ea13-179">A `path` route parameter name.</span></span>

<span data-ttu-id="8ea13-180">Per altre informazioni, vedere <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="8ea13-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="8ea13-181">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="8ea13-181">NavLink component</span></span>

<span data-ttu-id="8ea13-182">Usare un `NavLink` componente al posto di elementi collegamento ipertestuale`<a>`HTML () durante la creazione di collegamenti di navigazione.</span><span class="sxs-lookup"><span data-stu-id="8ea13-182">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="8ea13-183">Un `NavLink` componente si comporta come un `<a>` elemento, con la differenza che viene attivata `active` o disattivata una classe `href` CSS a seconda che corrisponda all'URL corrente.</span><span class="sxs-lookup"><span data-stu-id="8ea13-183">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="8ea13-184">La `active` classe consente a un utente di comprendere quale pagina è la pagina attiva tra i collegamenti di navigazione visualizzati.</span><span class="sxs-lookup"><span data-stu-id="8ea13-184">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="8ea13-185">Il componente `NavMenu` seguente crea una barra di navigazione [bootstrap](https://getbootstrap.com/docs/) che illustra come usare `NavLink` i componenti di:</span><span class="sxs-lookup"><span data-stu-id="8ea13-185">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="8ea13-186">Sono disponibili due `NavLinkMatch` opzioni che è possibile assegnare `Match` all'attributo dell' `<NavLink>` elemento:</span><span class="sxs-lookup"><span data-stu-id="8ea13-186">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="8ea13-187">`NavLinkMatch.All`L' oggetto`NavLink` è attivo quando corrisponde all'intero URL corrente. &ndash;</span><span class="sxs-lookup"><span data-stu-id="8ea13-187">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="8ea13-188">`NavLinkMatch.Prefix`(*impostazione predefinita*) L' oggetto`NavLink` è attivo quando corrisponde a qualsiasi prefisso dell'URL corrente. &ndash;</span><span class="sxs-lookup"><span data-stu-id="8ea13-188">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="8ea13-189">Nell' `NavLink` esempio precedente la Home `href=""` corrisponde all'URL Home e riceve solo la `active` classe CSS nell'URL del percorso di base predefinito dell'app (ad esempio, `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="8ea13-189">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="8ea13-190">Il secondo `NavLink` riceve la `active` classe quando l'utente visita un URL con un `MyComponent` prefisso, `https://localhost:5001/MyComponent` ad esempio e `https://localhost:5001/MyComponent/AnotherSegment`.</span><span class="sxs-lookup"><span data-stu-id="8ea13-190">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="8ea13-191">Gli `NavLink` attributi dei componenti aggiuntivi vengono passati al tag di ancoraggio sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="8ea13-191">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="8ea13-192">Nell'esempio seguente il `NavLink` componente include l' `target` attributo:</span><span class="sxs-lookup"><span data-stu-id="8ea13-192">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="8ea13-193">Viene eseguito il rendering del markup HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="8ea13-193">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="8ea13-194">Helper per lo stato di navigazione e URI</span><span class="sxs-lookup"><span data-stu-id="8ea13-194">URI and navigation state helpers</span></span>

<span data-ttu-id="8ea13-195">Usare `Microsoft.AspNetCore.Components.NavigationManager` per lavorare con gli URI e la C# navigazione nel codice.</span><span class="sxs-lookup"><span data-stu-id="8ea13-195">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="8ea13-196">`NavigationManager`fornisce l'evento e i metodi illustrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="8ea13-196">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="8ea13-197">Member</span><span class="sxs-lookup"><span data-stu-id="8ea13-197">Member</span></span> | <span data-ttu-id="8ea13-198">Descrizione</span><span class="sxs-lookup"><span data-stu-id="8ea13-198">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="8ea13-199">Ottiene l'URI assoluto corrente.</span><span class="sxs-lookup"><span data-stu-id="8ea13-199">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="8ea13-200">Ottiene l'URI di base (con una barra finale) che può essere anteposto ai percorsi URI relativi per produrre un URI assoluto.</span><span class="sxs-lookup"><span data-stu-id="8ea13-200">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="8ea13-201">`BaseUri` Corrisponde in genere `href` all'`<base>` attributo sull'elemento del documento in *wwwroot/index.html* (lato client Blazer) o *pages/_Host. cshtml* (lato server Blaze).</span><span class="sxs-lookup"><span data-stu-id="8ea13-201">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="8ea13-202">Passa all'URI specificato.</span><span class="sxs-lookup"><span data-stu-id="8ea13-202">Navigates to the specified URI.</span></span> <span data-ttu-id="8ea13-203">Se `forceLoad` è `true`:</span><span class="sxs-lookup"><span data-stu-id="8ea13-203">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="8ea13-204">Il routing lato client viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="8ea13-204">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="8ea13-205">Il browser è forzato a caricare la nuova pagina dal server, indipendentemente dal fatto che l'URI venga normalmente gestito dal router lato client.</span><span class="sxs-lookup"><span data-stu-id="8ea13-205">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="8ea13-206">Un evento che viene attivato quando il percorso di navigazione viene modificato.</span><span class="sxs-lookup"><span data-stu-id="8ea13-206">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="8ea13-207">Converte un URI relativo in un URI assoluto.</span><span class="sxs-lookup"><span data-stu-id="8ea13-207">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="8ea13-208">Dato un URI di base (ad esempio, un URI restituito in `GetBaseUri`precedenza da), converte un URI assoluto in un URI relativo al prefisso URI di base.</span><span class="sxs-lookup"><span data-stu-id="8ea13-208">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="8ea13-209">Quando si seleziona il pulsante, il componente seguente `Counter` passa al componente dell'app:</span><span class="sxs-lookup"><span data-stu-id="8ea13-209">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
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
