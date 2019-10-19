---
title: Routing ASP.NET Core Blazor
author: guardrex
description: Informazioni su come instradare le richieste nelle app e sul componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/routing
ms.openlocfilehash: d4b76c00f79f333884fa7e30b27eadc6e36de287
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589933"
---
# <a name="aspnet-core-opno-locblazor-routing"></a><span data-ttu-id="b32a3-103">Routing ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="b32a3-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="b32a3-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b32a3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="b32a3-105">Informazioni su come instradare le richieste e su come usare il componente `NavLink` per creare collegamenti di navigazione nelle app Blazor.</span><span class="sxs-lookup"><span data-stu-id="b32a3-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="b32a3-106">Integrazione di routing endpoint ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b32a3-106">ASP.NET Core endpoint routing integration</span></span>

Blazor<span data-ttu-id="b32a3-107"> server è integrato nel [Routing ASP.NET Core endpoint](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="b32a3-107"> Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="b32a3-108">Un'app ASP.NET Core è configurata in modo da accettare le connessioni in ingresso per i componenti interattivi con `MapBlazorHub` in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b32a3-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="b32a3-109">La configurazione più tipica consiste nel routing di tutte le richieste a una pagina Razor, che funge da host per la parte lato server dell'app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="b32a3-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="b32a3-110">Per convenzione, la pagina *host* è in genere denominata *_Host. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b32a3-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="b32a3-111">La route specificata nel file host viene chiamata route di *fallback* perché funziona con una priorità bassa nella corrispondenza della route.</span><span class="sxs-lookup"><span data-stu-id="b32a3-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="b32a3-112">La route di fallback viene considerata quando altre route non corrispondono.</span><span class="sxs-lookup"><span data-stu-id="b32a3-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="b32a3-113">Ciò consente all'app di usare altri controller e pagine senza interferire con l'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="b32a3-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="b32a3-114">Modelli di route</span><span class="sxs-lookup"><span data-stu-id="b32a3-114">Route templates</span></span>

<span data-ttu-id="b32a3-115">Il componente `Router` consente il routing a ogni componente con una route specificata.</span><span class="sxs-lookup"><span data-stu-id="b32a3-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="b32a3-116">Il componente `Router` viene visualizzato nel file *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="b32a3-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="b32a3-117">Quando viene compilato un file *Razor* con una direttiva `@page`, alla classe generata viene fornito un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specificando il modello di route.</span><span class="sxs-lookup"><span data-stu-id="b32a3-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="b32a3-118">In fase di esecuzione, il componente `RouteView`:</span><span class="sxs-lookup"><span data-stu-id="b32a3-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="b32a3-119">Riceve il `RouteData` dall'`Router` insieme a tutti i parametri desiderati.</span><span class="sxs-lookup"><span data-stu-id="b32a3-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="b32a3-120">Esegue il rendering del componente specificato con il layout (o un layout predefinito facoltativo) utilizzando i parametri specificati.</span><span class="sxs-lookup"><span data-stu-id="b32a3-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="b32a3-121">Facoltativamente, è possibile specificare un parametro `DefaultLayout` con una classe layout da utilizzare per i componenti che non specificano un layout.</span><span class="sxs-lookup"><span data-stu-id="b32a3-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="b32a3-122">I modelli Blazor predefiniti specificano il componente `MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="b32a3-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="b32a3-123">*MainLayout. Razor* si trova nella cartella *condivisa* del progetto modello.</span><span class="sxs-lookup"><span data-stu-id="b32a3-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="b32a3-124">Per ulteriori informazioni sui layout, vedere <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="b32a3-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="b32a3-125">È possibile applicare più modelli di route a un componente.</span><span class="sxs-lookup"><span data-stu-id="b32a3-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="b32a3-126">Il componente seguente risponde alle richieste per `/BlazorRoute` e `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="b32a3-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="b32a3-127">Per risolvere correttamente gli URL, l'app deve includere un tag `<base>` nel file *wwwroot/index.html* (Blazor webassembly) o nel file *pages/_Host. cshtml* (server Blazor) con il percorso di base dell'app specificato nell'attributo `href` (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="b32a3-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="b32a3-128">Per ulteriori informazioni, vedere <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="b32a3-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="b32a3-129">Fornire contenuto personalizzato quando il contenuto non è stato trovato</span><span class="sxs-lookup"><span data-stu-id="b32a3-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="b32a3-130">Il componente `Router` consente all'app di specificare contenuto personalizzato se non viene trovato contenuto per la route richiesta.</span><span class="sxs-lookup"><span data-stu-id="b32a3-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="b32a3-131">Nel file *app. Razor* impostare il contenuto personalizzato nel parametro del modello di `NotFound` del componente `Router`:</span><span class="sxs-lookup"><span data-stu-id="b32a3-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="b32a3-132">Il contenuto dei tag `<NotFound>` può includere elementi arbitrari, ad esempio altri componenti interattivi.</span><span class="sxs-lookup"><span data-stu-id="b32a3-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="b32a3-133">Per applicare un layout predefinito a `NotFound` contenuto, vedere <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="b32a3-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="b32a3-134">Routing a componenti da più assembly</span><span class="sxs-lookup"><span data-stu-id="b32a3-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="b32a3-135">Usare il parametro `AdditionalAssemblies` per specificare gli assembly aggiuntivi da considerare per il componente `Router` durante la ricerca di componenti instradabili.</span><span class="sxs-lookup"><span data-stu-id="b32a3-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="b32a3-136">Gli assembly specificati vengono considerati oltre all'assembly specificato `AppAssembly`.</span><span class="sxs-lookup"><span data-stu-id="b32a3-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="b32a3-137">Nell'esempio seguente `Component1` è un componente instradabile definito in una libreria di classi a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="b32a3-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="b32a3-138">Nell'esempio di `AdditionalAssemblies` seguente viene restituito il supporto del routing per `Component1`:</span><span class="sxs-lookup"><span data-stu-id="b32a3-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="b32a3-139">Parametri di route</span><span class="sxs-lookup"><span data-stu-id="b32a3-139">Route parameters</span></span>

<span data-ttu-id="b32a3-140">Il router usa parametri di route per popolare i parametri del componente corrispondenti con lo stesso nome (senza distinzione tra maiuscole e minuscole):</span><span class="sxs-lookup"><span data-stu-id="b32a3-140">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="b32a3-141">I parametri facoltativi non sono supportati per le app Blazor in ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="b32a3-141">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0.</span></span> <span data-ttu-id="b32a3-142">Nell'esempio precedente vengono applicate due direttive `@page`.</span><span class="sxs-lookup"><span data-stu-id="b32a3-142">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="b32a3-143">Il primo consente la navigazione al componente senza un parametro.</span><span class="sxs-lookup"><span data-stu-id="b32a3-143">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="b32a3-144">La seconda direttiva `@page` accetta il parametro di route `{text}` e assegna il valore alla proprietà `Text`.</span><span class="sxs-lookup"><span data-stu-id="b32a3-144">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="b32a3-145">Vincoli di route</span><span class="sxs-lookup"><span data-stu-id="b32a3-145">Route constraints</span></span>

<span data-ttu-id="b32a3-146">Un vincolo di route impone la corrispondenza tra i tipi in un segmento di route e un componente.</span><span class="sxs-lookup"><span data-stu-id="b32a3-146">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="b32a3-147">Nell'esempio seguente, la route per il componente `Users` corrisponde solo a se:</span><span class="sxs-lookup"><span data-stu-id="b32a3-147">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="b32a3-148">Un segmento di route `Id` è presente nell'URL della richiesta.</span><span class="sxs-lookup"><span data-stu-id="b32a3-148">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="b32a3-149">Il segmento `Id` è un numero intero (`int`).</span><span class="sxs-lookup"><span data-stu-id="b32a3-149">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="b32a3-150">Sono disponibili i vincoli di route indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="b32a3-150">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="b32a3-151">Per ulteriori informazioni, vedere l'avviso sotto la tabella per i vincoli di route corrispondenti alla lingua inglese.</span><span class="sxs-lookup"><span data-stu-id="b32a3-151">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="b32a3-152">Vincolo</span><span class="sxs-lookup"><span data-stu-id="b32a3-152">Constraint</span></span> | <span data-ttu-id="b32a3-153">Esempio</span><span class="sxs-lookup"><span data-stu-id="b32a3-153">Example</span></span>           | <span data-ttu-id="b32a3-154">Esempi di corrispondenza</span><span class="sxs-lookup"><span data-stu-id="b32a3-154">Example Matches</span></span>                                                                  | <span data-ttu-id="b32a3-155">Invariante</span><span class="sxs-lookup"><span data-stu-id="b32a3-155">Invariant</span></span><br><span data-ttu-id="b32a3-156">impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="b32a3-156">culture</span></span><br><span data-ttu-id="b32a3-157">corrispondenti</span><span class="sxs-lookup"><span data-stu-id="b32a3-157">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="b32a3-158">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="b32a3-158">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="b32a3-159">No</span><span class="sxs-lookup"><span data-stu-id="b32a3-159">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="b32a3-160">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="b32a3-160">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="b32a3-161">Yes</span><span class="sxs-lookup"><span data-stu-id="b32a3-161">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="b32a3-162">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="b32a3-162">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="b32a3-163">Yes</span><span class="sxs-lookup"><span data-stu-id="b32a3-163">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="b32a3-164">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="b32a3-164">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="b32a3-165">Yes</span><span class="sxs-lookup"><span data-stu-id="b32a3-165">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="b32a3-166">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="b32a3-166">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="b32a3-167">Yes</span><span class="sxs-lookup"><span data-stu-id="b32a3-167">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="b32a3-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="b32a3-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="b32a3-169">No</span><span class="sxs-lookup"><span data-stu-id="b32a3-169">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="b32a3-170">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="b32a3-170">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="b32a3-171">Yes</span><span class="sxs-lookup"><span data-stu-id="b32a3-171">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="b32a3-172">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="b32a3-172">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="b32a3-173">Yes</span><span class="sxs-lookup"><span data-stu-id="b32a3-173">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="b32a3-174">I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica,</span><span class="sxs-lookup"><span data-stu-id="b32a3-174">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="b32a3-175">presupponendo che l'URL sia non localizzabile.</span><span class="sxs-lookup"><span data-stu-id="b32a3-175">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="b32a3-176">Routing con URL contenenti punti</span><span class="sxs-lookup"><span data-stu-id="b32a3-176">Routing with URLs that contain dots</span></span>

<span data-ttu-id="b32a3-177">Nelle app Blazor server la route predefinita in *_Host. cshtml* è `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="b32a3-177">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="b32a3-178">Un URL di richiesta che contiene un punto (`.`) non corrisponde alla route predefinita perché l'URL viene visualizzato per richiedere un file.</span><span class="sxs-lookup"><span data-stu-id="b32a3-178">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="b32a3-179">Un'app Blazor restituisce una risposta *404 non trovata* per un file statico che non esiste.</span><span class="sxs-lookup"><span data-stu-id="b32a3-179">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="b32a3-180">Per usare le route che contengono un punto, configurare *_Host. cshtml* con il modello di route seguente:</span><span class="sxs-lookup"><span data-stu-id="b32a3-180">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="b32a3-181">Il modello di `"/{**path}"` include:</span><span class="sxs-lookup"><span data-stu-id="b32a3-181">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="b32a3-182">Sintassi Double-asterisco *catch-all* (`**`) per acquisire il percorso tra più limiti di cartella senza barre di codifica (`/`).</span><span class="sxs-lookup"><span data-stu-id="b32a3-182">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="b32a3-183">Nome del parametro di route `path`.</span><span class="sxs-lookup"><span data-stu-id="b32a3-183">A `path` route parameter name.</span></span>

<span data-ttu-id="b32a3-184">Per ulteriori informazioni, vedere <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="b32a3-184">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="b32a3-185">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="b32a3-185">NavLink component</span></span>

<span data-ttu-id="b32a3-186">Usare un componente `NavLink` al posto degli elementi collegamento ipertestuale HTML (`<a>`) durante la creazione di collegamenti di navigazione.</span><span class="sxs-lookup"><span data-stu-id="b32a3-186">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="b32a3-187">Un componente `NavLink` si comporta come un elemento `<a>`, con la differenza che viene attivata o disattivata una classe `active` CSS a seconda che la `href` corrisponda all'URL corrente.</span><span class="sxs-lookup"><span data-stu-id="b32a3-187">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="b32a3-188">La classe `active` consente agli utenti di comprendere quale pagina è la pagina attiva tra i collegamenti di navigazione visualizzati.</span><span class="sxs-lookup"><span data-stu-id="b32a3-188">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="b32a3-189">Il componente `NavMenu` seguente crea una barra di navigazione [bootstrap](https://getbootstrap.com/docs/) che illustra come usare `NavLink` componenti:</span><span class="sxs-lookup"><span data-stu-id="b32a3-189">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="b32a3-190">Sono disponibili due opzioni di `NavLinkMatch` che è possibile assegnare all'attributo `Match` dell'elemento `<NavLink>`:</span><span class="sxs-lookup"><span data-stu-id="b32a3-190">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="b32a3-191">`NavLinkMatch.All` &ndash; il `NavLink` è attivo quando corrisponde all'intero URL corrente.</span><span class="sxs-lookup"><span data-stu-id="b32a3-191">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="b32a3-192">`NavLinkMatch.Prefix` (*impostazione predefinita*) &ndash; il `NavLink` è attivo quando corrisponde a qualsiasi prefisso dell'URL corrente.</span><span class="sxs-lookup"><span data-stu-id="b32a3-192">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="b32a3-193">Nell'esempio precedente, il `href=""` Home `NavLink` corrisponde all'URL Home e riceve solo la classe `active` CSS nell'URL del percorso di base predefinito dell'app, ad esempio `https://localhost:5001/`.</span><span class="sxs-lookup"><span data-stu-id="b32a3-193">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="b32a3-194">Il secondo `NavLink` riceve la classe `active` quando l'utente visita un URL con un prefisso `MyComponent` (ad esempio, `https://localhost:5001/MyComponent` e `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="b32a3-194">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="b32a3-195">Gli attributi aggiuntivi del componente `NavLink` vengono passati al tag di ancoraggio sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="b32a3-195">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="b32a3-196">Nell'esempio seguente il componente `NavLink` include l'attributo `target`:</span><span class="sxs-lookup"><span data-stu-id="b32a3-196">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="b32a3-197">Viene eseguito il rendering del markup HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="b32a3-197">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="b32a3-198">Helper per lo stato di navigazione e URI</span><span class="sxs-lookup"><span data-stu-id="b32a3-198">URI and navigation state helpers</span></span>

<span data-ttu-id="b32a3-199">Usare `Microsoft.AspNetCore.Components.NavigationManager` per lavorare con gli URI e la C# navigazione nel codice.</span><span class="sxs-lookup"><span data-stu-id="b32a3-199">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="b32a3-200">`NavigationManager` fornisce l'evento e i metodi illustrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="b32a3-200">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="b32a3-201">Member</span><span class="sxs-lookup"><span data-stu-id="b32a3-201">Member</span></span> | <span data-ttu-id="b32a3-202">Descrizione</span><span class="sxs-lookup"><span data-stu-id="b32a3-202">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="b32a3-203">Ottiene l'URI assoluto corrente.</span><span class="sxs-lookup"><span data-stu-id="b32a3-203">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="b32a3-204">Ottiene l'URI di base (con una barra finale) che può essere anteposto ai percorsi URI relativi per produrre un URI assoluto.</span><span class="sxs-lookup"><span data-stu-id="b32a3-204">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="b32a3-205">In genere, `BaseUri` corrisponde all'attributo `href` sull'elemento `<base>` del documento in *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="b32a3-205">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="b32a3-206">Passa all'URI specificato.</span><span class="sxs-lookup"><span data-stu-id="b32a3-206">Navigates to the specified URI.</span></span> <span data-ttu-id="b32a3-207">Se `forceLoad` è `true`:</span><span class="sxs-lookup"><span data-stu-id="b32a3-207">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="b32a3-208">Il routing lato client viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="b32a3-208">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="b32a3-209">Il browser è forzato a caricare la nuova pagina dal server, indipendentemente dal fatto che l'URI venga normalmente gestito dal router lato client.</span><span class="sxs-lookup"><span data-stu-id="b32a3-209">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="b32a3-210">Un evento che viene attivato quando il percorso di navigazione viene modificato.</span><span class="sxs-lookup"><span data-stu-id="b32a3-210">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="b32a3-211">Converte un URI relativo in un URI assoluto.</span><span class="sxs-lookup"><span data-stu-id="b32a3-211">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="b32a3-212">Dato un URI di base (ad esempio, un URI restituito in precedenza da `GetBaseUri`), converte un URI assoluto in un URI relativo al prefisso URI di base.</span><span class="sxs-lookup"><span data-stu-id="b32a3-212">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="b32a3-213">Quando si seleziona il pulsante, il componente seguente passa al componente `Counter` dell'app:</span><span class="sxs-lookup"><span data-stu-id="b32a3-213">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
