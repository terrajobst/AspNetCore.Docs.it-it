---
title: Helper tag di ancoraggio in ASP.NET Core
author: pkellner
description: Descrive l'uso dell'helper tag di ancoraggio
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="b6551-103">Helper tag di ancoraggio</span><span class="sxs-lookup"><span data-stu-id="b6551-103">Anchor Tag Helper</span></span>

<span data-ttu-id="b6551-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="b6551-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="b6551-105">L'helper tag di ancoraggio migliora il tag di ancoraggio HTML (`<a ... ></a>`) con l'aggiunta di nuovi attributi.</span><span class="sxs-lookup"><span data-stu-id="b6551-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="b6551-106">Il collegamento generato (sul tag `href`) viene creato usando i nuovi attributi.</span><span class="sxs-lookup"><span data-stu-id="b6551-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="b6551-107">Tale URL può includere un protocollo facoltativo, ad esempio https.</span><span class="sxs-lookup"><span data-stu-id="b6551-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="b6551-108">Il controller altoparlante seguente viene usato negli esempi di questo documento.</span><span class="sxs-lookup"><span data-stu-id="b6551-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="b6551-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="b6551-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="b6551-110">Attributi dell'helper tag di ancoraggio</span><span class="sxs-lookup"><span data-stu-id="b6551-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="b6551-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="b6551-111">asp-controller</span></span>

<span data-ttu-id="b6551-112">`asp-controller` viene usato per associare il controller che verrà usato per generare l'URL.</span><span class="sxs-lookup"><span data-stu-id="b6551-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="b6551-113">I controller specificati devono esistere nel progetto corrente.</span><span class="sxs-lookup"><span data-stu-id="b6551-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="b6551-114">Il codice seguente elenca tutti gli altoparlanti:</span><span class="sxs-lookup"><span data-stu-id="b6551-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="b6551-115">Il markup generato è il seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="b6551-116">Se `asp-controller` è specificato e `asp-action` non è specificato, l'elemento `asp-action` predefinito è il metodo controller predefinito della visualizzazione attualmente in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b6551-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="b6551-117">Nell'esempio precedente, se `asp-action` viene escluso e questo helper tag di ancoraggio viene generato dalla vista `Index` di *HomeController* (**/Home**), il markup generato sarà il seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="b6551-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="b6551-118">asp-action</span></span>

<span data-ttu-id="b6551-119">`asp-action` è il nome del metodo di azione nel controller che verrà incluso nell'elemento `href` generato.</span><span class="sxs-lookup"><span data-stu-id="b6551-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="b6551-120">Ad esempio, il codice seguente imposta l'elemento `href` generato in modo che faccia riferimento alla pagina dei dettagli dell'altoparlante:</span><span class="sxs-lookup"><span data-stu-id="b6551-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="b6551-121">Il markup generato è il seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="b6551-122">Se non è specificato nessun attributo `asp-controller` viene usato il controller predefinito per chiamare la visualizzazione che esegue la visualizzazione corrente.</span><span class="sxs-lookup"><span data-stu-id="b6551-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="b6551-123">Se l'attributo `asp-action` è `Index` non viene accodata nessuna azione all'URL e di conseguenza viene chiamato il metodo `Index` predefinito.</span><span class="sxs-lookup"><span data-stu-id="b6551-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="b6551-124">L'azione specificata (o usata per impostazione predefinita) deve esistere nel controller a cui si fa riferimento in `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="b6551-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="b6551-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="b6551-125">asp-page</span></span>

<span data-ttu-id="b6551-126">Usare l'attributo `asp-page` in un tag di ancoraggio per impostarne l'URL in modo che indichi una pagina specifica.</span><span class="sxs-lookup"><span data-stu-id="b6551-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="b6551-127">L'URL viene creato facendo precedere al nome della pagina un carattere barra "/".</span><span class="sxs-lookup"><span data-stu-id="b6551-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="b6551-128">L'URL dell'esempio seguente fa riferimento alla pagina "Speaker" nella directory corrente.</span><span class="sxs-lookup"><span data-stu-id="b6551-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="b6551-129">L'attributo `asp-page` nell'esempio di codice precedente esegue il rendering dell'output HTML nella visualizzazione, con risultati simili al frammento seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="b6551-130">L'attributo `asp-page` e gli attributi `asp-route`, `asp-controller` e `asp-action` si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="b6551-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="b6551-131">`asp-page` può tuttavia essere usato con `asp-route-id` per il controllo del routing, come visualizzato nell'esempio di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="b6551-132">`asp-route-id` produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="b6551-133">Per l'uso dell'attributo `asp-page` in Razor Pages gli URL devono essere percorsi relativi, ad esempio `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="b6551-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="b6551-134">I percorsi relativi nell'attributo `asp-page` non sono disponibili nelle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="b6551-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="b6551-135">Per le visualizzazioni MVC usare la sintassi "/".</span><span class="sxs-lookup"><span data-stu-id="b6551-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="b6551-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="b6551-136">asp-route-{value}</span></span>

<span data-ttu-id="b6551-137">`asp-route-` è un prefisso di route con caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="b6551-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="b6551-138">Qualsiasi valore inserito dopo il trattino finale viene interpretato come un potenziale parametro di route.</span><span class="sxs-lookup"><span data-stu-id="b6551-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="b6551-139">Se non viene trovata una route predefinita, il prefisso di route viene accodato come parametro di richiesta e relativo valore all'elemento href generato.</span><span class="sxs-lookup"><span data-stu-id="b6551-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="b6551-140">In caso contrario viene sostituito nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="b6551-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="b6551-141">Se ad esempio è presente un metodo del controller definito come segue:</span><span class="sxs-lookup"><span data-stu-id="b6551-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="b6551-142">E il modello di route predefinito in *Startup.cs* è definito come segue:</span><span class="sxs-lookup"><span data-stu-id="b6551-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="b6551-143">Il file con estensione **cshtml** contenente l'helper tag di ancoraggio necessario per usare il parametro modello **speaker** passato dal controller alla vista ha la struttura seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="b6551-144">Il codice HTML generato risultante sarà il seguente, perché nella route predefinita è stato rilevato **id**.</span><span class="sxs-lookup"><span data-stu-id="b6551-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="b6551-145">Se il prefisso di route non è incluso nel modello di routing rilevato, come ad esempio nel file con estensione **cshtml** seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="b6551-146">Il codice HTML generato risultante sarà il seguente, perché **speakerid** non è stato rilevato nella route corrispondente:</span><span class="sxs-lookup"><span data-stu-id="b6551-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="b6551-147">Se `asp-controller` o `asp-action` non vengono specificati, viene eseguita la stessa elaborazione predefinita usata nell'attributo `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="b6551-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="b6551-148">asp-route</span><span class="sxs-lookup"><span data-stu-id="b6551-148">asp-route</span></span>

<span data-ttu-id="b6551-149">`asp-route` consente di creare un URL che esegue il collegamento diretto a una route con nome.</span><span class="sxs-lookup"><span data-stu-id="b6551-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="b6551-150">Mediante gli attributi di routing è possibile assegnare un nome a una route come visualizzato in `SpeakerController` e usarla nel metodo `Evaluations` corrispondente.</span><span class="sxs-lookup"><span data-stu-id="b6551-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="b6551-151">`Name = "speakerevals"` richiede all'helper tag di ancoraggio di generare una route diretta a tale metodo del controller usando l'URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="b6551-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="b6551-152">Se è specificato `asp-controller` o `asp-action` in aggiunta a `asp-route`, la route generata potrebbe non essere quella prevista.</span><span class="sxs-lookup"><span data-stu-id="b6551-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="b6551-153">Evitare l'uso di `asp-route` con `asp-controller` o `asp-action` per non creare un conflitto di route.</span><span class="sxs-lookup"><span data-stu-id="b6551-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="b6551-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="b6551-154">asp-all-route-data</span></span>

<span data-ttu-id="b6551-155">`asp-all-route-data` consente la creazione di un dizionario di coppie di valori chiave in cui la chiave è il nome del parametro e il valore è il valore associato a tale chiave.</span><span class="sxs-lookup"><span data-stu-id="b6551-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="b6551-156">Come illustrato nell'esempio seguente, viene creato un dizionario inline e i dati vengono passati alla visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="b6551-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="b6551-157">In alternativa è possibile passare i dati con il modello.</span><span class="sxs-lookup"><span data-stu-id="b6551-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="b6551-158">Il codice precedente genera l'URL seguente: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="b6551-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="b6551-159">Quando si fa clic sul collegamento viene chiamato il metodo del controller `EvaluationsCurrent`.</span><span class="sxs-lookup"><span data-stu-id="b6551-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="b6551-160">Il metodo viene chiamato perché il controller presenta due parametri stringa che corrispondono a ciò che è stato creato dal dizionario `asp-all-route-data`.</span><span class="sxs-lookup"><span data-stu-id="b6551-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="b6551-161">Se una o più chiavi nel dizionario corrispondono a parametri della route, tali valori vengono sostituiti nella route come previsto e gli altri valori non corrispondenti vengono generati come parametri di richiesta.</span><span class="sxs-lookup"><span data-stu-id="b6551-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="b6551-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="b6551-162">asp-fragment</span></span>

<span data-ttu-id="b6551-163">`asp-fragment` definisce un frammento di URL da accodare all'URL.</span><span class="sxs-lookup"><span data-stu-id="b6551-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="b6551-164">L'helper tag di ancoraggio aggiunge il carattere hash (#).</span><span class="sxs-lookup"><span data-stu-id="b6551-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="b6551-165">Se si crea un tag:</span><span class="sxs-lookup"><span data-stu-id="b6551-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="b6551-166">L'URL generato è: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="b6551-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="b6551-167">I tag hash sono utili per la compilazione di applicazioni lato client.</span><span class="sxs-lookup"><span data-stu-id="b6551-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="b6551-168">Ad esempio possono essere usati per semplificare l'uso di contrassegni e la ricerca in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b6551-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="b6551-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="b6551-169">asp-area</span></span>

<span data-ttu-id="b6551-170">`asp-area` imposta il nome di area usato da ASP.NET Core per impostare la route appropriata.</span><span class="sxs-lookup"><span data-stu-id="b6551-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="b6551-171">Gli esempi che seguono visualizzano come l'attributo di area determina la modifica del mapping delle route.</span><span class="sxs-lookup"><span data-stu-id="b6551-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="b6551-172">Se si imposta `asp-area` su Blogs, la directory `Areas/Blogs` viene aggiunta come prefisso alle route dei controller e delle visualizzazioni associate di questo tag di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="b6551-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="b6551-173">Nome progetto</span><span class="sxs-lookup"><span data-stu-id="b6551-173">Project name</span></span>
  * <span data-ttu-id="b6551-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="b6551-174">wwwroot</span></span>
  * <span data-ttu-id="b6551-175">Aree</span><span class="sxs-lookup"><span data-stu-id="b6551-175">Areas</span></span>
    * <span data-ttu-id="b6551-176">Blog</span><span class="sxs-lookup"><span data-stu-id="b6551-176">Blogs</span></span>
      * <span data-ttu-id="b6551-177">Controller</span><span class="sxs-lookup"><span data-stu-id="b6551-177">Controllers</span></span>
        * <span data-ttu-id="b6551-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="b6551-178">HomeController.cs</span></span>
      * <span data-ttu-id="b6551-179">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="b6551-179">Views</span></span>
        * <span data-ttu-id="b6551-180">Home</span><span class="sxs-lookup"><span data-stu-id="b6551-180">Home</span></span>
          * <span data-ttu-id="b6551-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="b6551-181">Index.cshtml</span></span>
          * <span data-ttu-id="b6551-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="b6551-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="b6551-183">Controller</span><span class="sxs-lookup"><span data-stu-id="b6551-183">Controllers</span></span>

<span data-ttu-id="b6551-184">Se viene specificato un tag di area valido, ad esempio ```area="Blogs"```, per il riferimento al file ```AboutBlog.cshtml``` si ottiene un risultato simile al seguente usando l'helper tag di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="b6551-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="b6551-185">Il codice HTML generato include il segmento delle aree e ha l'aspetto seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="b6551-186">Per garantire il funzionamento delle aree MVC in un'applicazione Web, il modello di route deve includere un riferimento all'area, se esistente.</span><span class="sxs-lookup"><span data-stu-id="b6551-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="b6551-187">Tale modello, che è il secondo parametro della chiamata del metodo `routes.MapRoute`, viene visualizzato come segue: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="b6551-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="b6551-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="b6551-188">asp-protocol</span></span>

<span data-ttu-id="b6551-189">`asp-protocol` consente di specificare un protocollo (ad esempio `https`) nell'URL.</span><span class="sxs-lookup"><span data-stu-id="b6551-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="b6551-190">Un esempio di helper tag di ancoraggio che include il protocollo è il seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="b6551-191">e genera il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="b6551-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="b6551-192">Nell'esempio il dominio è localhost, ma l'helper tag di ancoraggio usa il dominio pubblico del sito Web per generare l'URL.</span><span class="sxs-lookup"><span data-stu-id="b6551-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6551-193">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b6551-193">Additional resources</span></span>

* [<span data-ttu-id="b6551-194">Aree</span><span class="sxs-lookup"><span data-stu-id="b6551-194">Areas</span></span>](xref:mvc/controllers/areas)
