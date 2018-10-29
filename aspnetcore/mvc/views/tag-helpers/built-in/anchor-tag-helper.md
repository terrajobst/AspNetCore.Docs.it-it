---
title: Helper tag di ancoraggio in ASP.NET Core
author: pkellner
description: Informazioni sugli attributi dell'helper tag di ancoraggio ASP.NET Core e sul ruolo di ogni attributo per l'estensione del comportamento del tag di ancoraggio HTML.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 01c5833210b73dafb763602d363afcf9e7bc0122
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206276"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="8951b-103">Helper tag di ancoraggio in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8951b-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="8951b-104">Di [Peter Kellner](http://peterkellner.net) e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="8951b-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="8951b-105">L'[helper tag](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) di ancoraggio migliora il tag di ancoraggio HTML standard (`<a ... ></a>`) con l'aggiunta di nuovi attributi.</span><span class="sxs-lookup"><span data-stu-id="8951b-105">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="8951b-106">Per convenzione, i nomi di attributo hanno il prefisso `asp-`.</span><span class="sxs-lookup"><span data-stu-id="8951b-106">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="8951b-107">Il valore dell'attributo `href` dell'elemento di ancoraggio visualizzato dipende dai valori degli attributi `asp-`.</span><span class="sxs-lookup"><span data-stu-id="8951b-107">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="8951b-108">Per una panoramica degli helper tag, vedere <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="8951b-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="8951b-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8951b-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8951b-110">*SpeakerController* viene usato negli esempi in tutto il documento:</span><span class="sxs-lookup"><span data-stu-id="8951b-110">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="8951b-111">Segue un inventario degli attributi `asp-`.</span><span class="sxs-lookup"><span data-stu-id="8951b-111">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="8951b-112">asp-controller</span><span class="sxs-lookup"><span data-stu-id="8951b-112">asp-controller</span></span>

<span data-ttu-id="8951b-113">L'attributo [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) assegna il controller usato per generare l'URL.</span><span class="sxs-lookup"><span data-stu-id="8951b-113">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="8951b-114">Il markup seguente elenca tutti gli altoparlanti:</span><span class="sxs-lookup"><span data-stu-id="8951b-114">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="8951b-115">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-115">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="8951b-116">Se si specifica l'attributo `asp-controller` e l'attributo `asp-action` viene omesso, il valore `asp-action` predefinito è l'azione del controller associata alla visualizzazione di esecuzione corrente.</span><span class="sxs-lookup"><span data-stu-id="8951b-116">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="8951b-117">Se `asp-action` viene omesso dal markup precedente e viene usato l'helper tag di ancoraggio nella visualizzazione *Index* di *HomeController* (*/Home*), il codice HTML generato è:</span><span class="sxs-lookup"><span data-stu-id="8951b-117">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="8951b-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="8951b-118">asp-action</span></span>

<span data-ttu-id="8951b-119">Il valore dell'attributo [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) rappresenta il nome dell'azione del controller incluso nell'attributo `href` generato.</span><span class="sxs-lookup"><span data-stu-id="8951b-119">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="8951b-120">Il markup seguente imposta il valore dell'attributo `href` generato sulla pagina delle valutazioni degli altoparlanti:</span><span class="sxs-lookup"><span data-stu-id="8951b-120">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="8951b-121">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-121">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="8951b-122">Se non viene specificato alcun attributo `asp-controller` viene usato il controller predefinito per chiamare la visualizzazione che esegue la visualizzazione corrente.</span><span class="sxs-lookup"><span data-stu-id="8951b-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="8951b-123">Se il valore dell'attributo `asp-action` è `Index` non viene accodata alcuna azione all'URL, con conseguente chiamata dell'azione `Index` predefinita.</span><span class="sxs-lookup"><span data-stu-id="8951b-123">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="8951b-124">L'azione specificata (o usata per impostazione predefinita) deve esistere nel controller a cui si fa riferimento in `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="8951b-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="8951b-125">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="8951b-125">asp-route-{value}</span></span>

<span data-ttu-id="8951b-126">L'attributo [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) abilita un prefisso di route con caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="8951b-126">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="8951b-127">Qualsiasi valore che sostituisce il segnaposto `{value}` viene interpretato come potenziale parametro di route.</span><span class="sxs-lookup"><span data-stu-id="8951b-127">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="8951b-128">Se non viene trovata una route predefinita, il prefisso di route viene accodato come parametro di richiesta e relativo valore all'attributo `href` generato.</span><span class="sxs-lookup"><span data-stu-id="8951b-128">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="8951b-129">In caso contrario viene sostituito nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="8951b-129">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="8951b-130">Esaminare l'azione del controller seguente:</span><span class="sxs-lookup"><span data-stu-id="8951b-130">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="8951b-131">Con un modello di route predefinito definito in *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="8951b-131">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="8951b-132">La visualizzazione MVC usa il modello, fornito dall'azione, come segue:</span><span class="sxs-lookup"><span data-stu-id="8951b-132">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="8951b-133">È stata trovata una corrispondenza per il segnaposto `{id?}` della route predefinita.</span><span class="sxs-lookup"><span data-stu-id="8951b-133">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="8951b-134">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-134">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="8951b-135">Si supponga che il prefisso di route non faccia parte del modello di routing corrispondente, come nel caso della visualizzazione MVC seguente:</span><span class="sxs-lookup"><span data-stu-id="8951b-135">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="8951b-136">Il codice HTML seguente viene generato perché `speakerid` non è stato trovato nella route corrispondente:</span><span class="sxs-lookup"><span data-stu-id="8951b-136">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="8951b-137">Se `asp-controller` o `asp-action` non vengono specificati, viene eseguita la stessa elaborazione predefinita usata nell'attributo `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="8951b-137">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="8951b-138">asp-route</span><span class="sxs-lookup"><span data-stu-id="8951b-138">asp-route</span></span>

<span data-ttu-id="8951b-139">L'attributo [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) viene usato per la creazione di un URL collegato direttamente a una route denominata.</span><span class="sxs-lookup"><span data-stu-id="8951b-139">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="8951b-140">Mediante gli [attributi di routing](xref:mvc/controllers/routing#attribute-routing) è possibile assegnare un nome a una route come mostrato in `SpeakerController` e usarla nell'azione `Evaluations` corrispondente:</span><span class="sxs-lookup"><span data-stu-id="8951b-140">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="8951b-141">Nel markup seguente l'attributo `asp-route` fa riferimento alla route denominata:</span><span class="sxs-lookup"><span data-stu-id="8951b-141">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="8951b-142">L'helper tag di ancoraggio genera una route direttamente per tale azione del controller usando l'URL */Speaker/Evaluations*.</span><span class="sxs-lookup"><span data-stu-id="8951b-142">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="8951b-143">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-143">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="8951b-144">Se è specificato `asp-controller` o `asp-action` in aggiunta a `asp-route`, la route generata potrebbe non essere quella prevista.</span><span class="sxs-lookup"><span data-stu-id="8951b-144">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="8951b-145">Per evitare un conflitto di route, è consigliabile non usare `asp-route` con gli attributi `asp-controller` o `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="8951b-145">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="8951b-146">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="8951b-146">asp-all-route-data</span></span>

<span data-ttu-id="8951b-147">L'attributo [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) supporta la creazione di un dizionario di coppie chiave-valore.</span><span class="sxs-lookup"><span data-stu-id="8951b-147">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="8951b-148">La chiave è il nome del parametro e il valore è il valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="8951b-148">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="8951b-149">Nell'esempio seguente viene inizializzato un dizionario che viene poi passato a una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="8951b-149">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="8951b-150">In alternativa è possibile passare i dati con il modello.</span><span class="sxs-lookup"><span data-stu-id="8951b-150">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="8951b-151">Il codice precedente genera il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="8951b-151">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="8951b-152">Il dizionario `asp-all-route-data` viene reso flat per produrre una stringa di query corrispondente ai requisiti dell'azione `Evaluations` in overload:</span><span class="sxs-lookup"><span data-stu-id="8951b-152">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="8951b-153">In presenza di chiavi nel dizionario corrispondenti ai parametri di route, questi valori vengono sostituiti nella route come appropriato.</span><span class="sxs-lookup"><span data-stu-id="8951b-153">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="8951b-154">Gli altri valori non corrispondenti vengono generati come parametri di richiesta.</span><span class="sxs-lookup"><span data-stu-id="8951b-154">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="8951b-155">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="8951b-155">asp-fragment</span></span>

<span data-ttu-id="8951b-156">L'attributo [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) definisce un frammento di URL da accodare all'URL.</span><span class="sxs-lookup"><span data-stu-id="8951b-156">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="8951b-157">L'helper tag di ancoraggio aggiunge il carattere hash (#).</span><span class="sxs-lookup"><span data-stu-id="8951b-157">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="8951b-158">Considerare il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="8951b-158">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="8951b-159">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-159">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="8951b-160">I tag hash sono utili per la compilazione di app sul lato client.</span><span class="sxs-lookup"><span data-stu-id="8951b-160">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="8951b-161">Ad esempio possono essere usati per semplificare l'uso di contrassegni e la ricerca in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8951b-161">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="8951b-162">asp-area</span><span class="sxs-lookup"><span data-stu-id="8951b-162">asp-area</span></span>

<span data-ttu-id="8951b-163">L'attributo [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) imposta il nome dell'area usato per impostare la route appropriata.</span><span class="sxs-lookup"><span data-stu-id="8951b-163">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="8951b-164">L'esempio seguente illustra come l'attributo di area determina la modifica del mapping delle route.</span><span class="sxs-lookup"><span data-stu-id="8951b-164">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="8951b-165">Se si imposta `asp-area` su "Blogs", la directory *Areas/Blogs* viene aggiunta come prefisso alle route dei controller e delle visualizzazioni associati per questo tag di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="8951b-165">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="8951b-166">**{Nome progetto}**</span><span class="sxs-lookup"><span data-stu-id="8951b-166">**{Project name}**</span></span>
  * <span data-ttu-id="8951b-167">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="8951b-167">**wwwroot**</span></span>
  * <span data-ttu-id="8951b-168">**Aree**</span><span class="sxs-lookup"><span data-stu-id="8951b-168">**Areas**</span></span>
    * <span data-ttu-id="8951b-169">**Blog**</span><span class="sxs-lookup"><span data-stu-id="8951b-169">**Blogs**</span></span>
      * <span data-ttu-id="8951b-170">**Controller**</span><span class="sxs-lookup"><span data-stu-id="8951b-170">**Controllers**</span></span>
        * <span data-ttu-id="8951b-171">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="8951b-171">*HomeController.cs*</span></span>
      * <span data-ttu-id="8951b-172">**Visualizzazioni**</span><span class="sxs-lookup"><span data-stu-id="8951b-172">**Views**</span></span>
        * <span data-ttu-id="8951b-173">**Home**</span><span class="sxs-lookup"><span data-stu-id="8951b-173">**Home**</span></span>
          * <span data-ttu-id="8951b-174">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8951b-174">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="8951b-175">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8951b-175">*Index.cshtml*</span></span>
        * <span data-ttu-id="8951b-176">*\_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8951b-176">*\_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="8951b-177">**Controller**</span><span class="sxs-lookup"><span data-stu-id="8951b-177">**Controllers**</span></span>

<span data-ttu-id="8951b-178">Data la gerarchia di directory precedente, il markup per fare riferimento al file *AboutBlog.cshtml* è:</span><span class="sxs-lookup"><span data-stu-id="8951b-178">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="8951b-179">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-179">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="8951b-180">Per garantire il funzionamento delle aree in un'app MVC, il modello di route deve includere un riferimento all'area, se esistente.</span><span class="sxs-lookup"><span data-stu-id="8951b-180">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="8951b-181">Tale modello è rappresentato dal secondo parametro della chiamata del metodo `routes.MapRoute` in *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="8951b-181">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*:</span></span>
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a><span data-ttu-id="8951b-182">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="8951b-182">asp-protocol</span></span>

<span data-ttu-id="8951b-183">L'attributo [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) consente di specificare un protocollo (ad esempio `https`) nell'URL.</span><span class="sxs-lookup"><span data-stu-id="8951b-183">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="8951b-184">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8951b-184">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="8951b-185">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="8951b-186">Il nome host nell'esempio è localhost, ma l'helper tag di ancoraggio usa il dominio pubblico del sito Web per generare l'URL.</span><span class="sxs-lookup"><span data-stu-id="8951b-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="8951b-187">asp-host</span><span class="sxs-lookup"><span data-stu-id="8951b-187">asp-host</span></span>

<span data-ttu-id="8951b-188">L'attributo [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) consente di specificare un nome host nell'URL.</span><span class="sxs-lookup"><span data-stu-id="8951b-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="8951b-189">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8951b-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="8951b-190">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="8951b-191">asp-page</span><span class="sxs-lookup"><span data-stu-id="8951b-191">asp-page</span></span>

<span data-ttu-id="8951b-192">L'attributo [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) viene usato con le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="8951b-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="8951b-193">Usarlo per impostare il valore dell'attributo `href` per un tag di ancoraggio su una pagina specifica.</span><span class="sxs-lookup"><span data-stu-id="8951b-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="8951b-194">L'URL viene creato anteponendo una barra ("/") al nome della pagina.</span><span class="sxs-lookup"><span data-stu-id="8951b-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="8951b-195">L'esempio seguente fa riferimento alla pagina Razor Attendee:</span><span class="sxs-lookup"><span data-stu-id="8951b-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="8951b-196">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="8951b-197">L'attributo `asp-page` e gli attributi `asp-route`, `asp-controller` e `asp-action` si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="8951b-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="8951b-198">`asp-page` può tuttavia essere usato con `asp-route-{value}` per il controllo del routing, come dimostra il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="8951b-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="8951b-199">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="8951b-200">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="8951b-200">asp-page-handler</span></span>

<span data-ttu-id="8951b-201">L'attributo [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) viene usato con le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="8951b-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="8951b-202">È progettato per il collegamento di gestori di pagine specifici.</span><span class="sxs-lookup"><span data-stu-id="8951b-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="8951b-203">Si consideri il gestore di pagine seguente:</span><span class="sxs-lookup"><span data-stu-id="8951b-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="8951b-204">Il markup associato del modello di pagina si collega al gestore di pagina `OnGetProfile`.</span><span class="sxs-lookup"><span data-stu-id="8951b-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="8951b-205">Si noti che il prefisso `On<Verb>` del nome di metodo del gestore di pagina viene omesso nel valore dell'attributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="8951b-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="8951b-206">Il suffisso `Async` verrebbe omesso anche nel caso di un metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="8951b-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="8951b-207">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="8951b-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="8951b-208">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8951b-208">Additional resources</span></span>

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
