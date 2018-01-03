---
title: Helper di Tag di ancoraggio | Documenti Microsoft
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag di ancoraggio
keywords: Helper per tag di ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: b2bdf8b2b297a66b08445d99afbc5f43d2e37ef6
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="b0055-104">Helper di Tag di ancoraggio</span><span class="sxs-lookup"><span data-stu-id="b0055-104">Anchor Tag Helper</span></span>

<span data-ttu-id="b0055-105">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="b0055-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="b0055-106">L'Helper di Tag di ancoraggio migliora il punto di ancoraggio HTML (`<a ... ></a>`) tag aggiungendo nuovi attributi.</span><span class="sxs-lookup"><span data-stu-id="b0055-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="b0055-107">Il collegamento generato (sul `href` tag) viene creato utilizzando i nuovi attributi.</span><span class="sxs-lookup"><span data-stu-id="b0055-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="b0055-108">Tale URL può includere un protocollo facoltativi, ad esempio https.</span><span class="sxs-lookup"><span data-stu-id="b0055-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="b0055-109">Negli esempi in questo documento viene usato il controller di voce riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="b0055-109">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="b0055-110">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="b0055-110">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="b0055-111">Attributi Helper di Tag di ancoraggio</span><span class="sxs-lookup"><span data-stu-id="b0055-111">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="b0055-112">ASP controller</span><span class="sxs-lookup"><span data-stu-id="b0055-112">asp-controller</span></span>

<span data-ttu-id="b0055-113">`asp-controller`viene utilizzato per associare il controller verrà utilizzato per generare l'URL.</span><span class="sxs-lookup"><span data-stu-id="b0055-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="b0055-114">Il controller specificato deve essere presente nel progetto corrente.</span><span class="sxs-lookup"><span data-stu-id="b0055-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="b0055-115">Il codice seguente vengono elencati tutti gli altoparlanti:</span><span class="sxs-lookup"><span data-stu-id="b0055-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="b0055-116">Il markup generato sarà:</span><span class="sxs-lookup"><span data-stu-id="b0055-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="b0055-117">Se il `asp-controller` specificato e `asp-action` non lo è, il valore predefinito `asp-action` sarà metodo controller predefinito per la visualizzazione attualmente in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b0055-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="b0055-118">Che nell'esempio precedente, se viene `asp-action` viene lasciata fuori, e viene generato questo Helper di Tag di ancoraggio da *HomeController*del `Index` vista (**/Home**), il markup generato sarà:</span><span class="sxs-lookup"><span data-stu-id="b0055-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="b0055-119">azione di ASP</span><span class="sxs-lookup"><span data-stu-id="b0055-119">asp-action</span></span>

<span data-ttu-id="b0055-120">`asp-action`è il nome del metodo di azione nel controller che verranno inclusi nell'oggetto generato `href`.</span><span class="sxs-lookup"><span data-stu-id="b0055-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="b0055-121">Ad esempio, il codice seguente imposta generato `href` in modo che punti alla pagina di dettaglio del relatore:</span><span class="sxs-lookup"><span data-stu-id="b0055-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="b0055-122">Il markup generato sarà:</span><span class="sxs-lookup"><span data-stu-id="b0055-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="b0055-123">Se non `asp-controller` attributo viene specificato, verrà utilizzato il controller predefinito chiamando la visualizzazione di cui eseguire la vista corrente.</span><span class="sxs-lookup"><span data-stu-id="b0055-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="b0055-124">Se l'attributo `asp-action` è `Index`, nessuna azione viene aggiunto all'URL, determinando il valore predefinito `Index` metodo chiamato.</span><span class="sxs-lookup"><span data-stu-id="b0055-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="b0055-125">L'azione specificata (o per impostazione predefinita), deve esistere nel controller a cui fa riferimento `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="b0055-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="b0055-126">pagina ASP</span><span class="sxs-lookup"><span data-stu-id="b0055-126">asp-page</span></span>

<span data-ttu-id="b0055-127">Utilizzare il `asp-page` attributo in un tag di ancoraggio per impostare il relativo URL in modo che punti a una pagina specifica.</span><span class="sxs-lookup"><span data-stu-id="b0055-127">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="b0055-128">Prefisso per il nome di pagina con una barra "/" crea l'URL.</span><span class="sxs-lookup"><span data-stu-id="b0055-128">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="b0055-129">Nell'esempio seguente l'URL punta alla pagina "Voce" nella directory corrente.</span><span class="sxs-lookup"><span data-stu-id="b0055-129">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="b0055-130">Il `asp-page` attributo nell'esempio di codice precedente viene eseguito il rendering di output HTML nella visualizzazione è simile al frammento riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b0055-130">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
``

The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes. However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="b0055-131">Il `asp-route-id` produce il seguente output:</span><span class="sxs-lookup"><span data-stu-id="b0055-131">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="b0055-132">Utilizzare il `asp-page` attributo nelle pagine Razor, l'URL deve essere un percorso relativo, ad esempio `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="b0055-132">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="b0055-133">I percorsi relativi nel `asp-page` attributo non sono disponibili nelle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="b0055-133">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="b0055-134">Utilizzare la sintassi "/" per le visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="b0055-134">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="b0055-135">ASP - route-{value}</span><span class="sxs-lookup"><span data-stu-id="b0055-135">asp-route-{value}</span></span>

<span data-ttu-id="b0055-136">`asp-route-`è un prefisso di route con caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="b0055-136">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="b0055-137">Qualsiasi valore inserito dopo il trattino finale verrà interpretato come un parametro di route possibili.</span><span class="sxs-lookup"><span data-stu-id="b0055-137">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="b0055-138">Se una route predefinita non viene trovata, verrà aggiunto il prefisso della route per href generato come valore di parametro di richiesta e.</span><span class="sxs-lookup"><span data-stu-id="b0055-138">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="b0055-139">In caso contrario verrà sostituito nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="b0055-139">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="b0055-140">Presupponendo che sia un metodo del controller definiti come segue:</span><span class="sxs-lookup"><span data-stu-id="b0055-140">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="b0055-141">E che il modello di route predefiniti definito nel *Startup.cs* come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b0055-141">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="b0055-142">Il **cshtml** file che contiene l'Helper di Tag di ancoraggio necessarie per l'utilizzo di **altoparlante** parametro di modello passato dal controller per la visualizzazione è indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b0055-142">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="b0055-143">Il codice HTML generato verrà quindi modificato come segue perché **id** è stato trovato nella route predefinita.</span><span class="sxs-lookup"><span data-stu-id="b0055-143">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="b0055-144">Se il prefisso di route non fa parte di trovare il modello di routing, che si verifica con i seguenti **cshtml** file:</span><span class="sxs-lookup"><span data-stu-id="b0055-144">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="b0055-145">Il codice HTML generato verrà quindi modificato come segue perché **speakerid** non è stato trovato nella route corrispondenza:</span><span class="sxs-lookup"><span data-stu-id="b0055-145">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="b0055-146">Se il valore `asp-controller` o `asp-action` non vengono specificati, viene seguito l'elaborazione predefinita stesso in cui è nel `asp-route` attributo.</span><span class="sxs-lookup"><span data-stu-id="b0055-146">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="b0055-147">route di ASP</span><span class="sxs-lookup"><span data-stu-id="b0055-147">asp-route</span></span>

<span data-ttu-id="b0055-148">`asp-route`fornisce un modo per creare un URL che colleghi direttamente a una route denominata.</span><span class="sxs-lookup"><span data-stu-id="b0055-148">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="b0055-149">Gli attributi di routing una route può essere denominata come illustrato nel `SpeakerController` e utilizzato nel relativo `Evaluations` metodo.</span><span class="sxs-lookup"><span data-stu-id="b0055-149">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="b0055-150">`Name = "speakerevals"`indica l'Helper di Tag di ancoraggio per generare una route direttamente a tale metodo controller utilizzando l'URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="b0055-150">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="b0055-151">Se `asp-controller` o `asp-action` è specificato in aggiunta al `asp-route`, la route generata potrebbe non essere quello previsto.</span><span class="sxs-lookup"><span data-stu-id="b0055-151">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="b0055-152">`asp-route`non deve essere utilizzato con uno degli attributi `asp-controller` o `asp-action` per evitare un conflitto di route.</span><span class="sxs-lookup"><span data-stu-id="b0055-152">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="b0055-153">dati della route di all ASP</span><span class="sxs-lookup"><span data-stu-id="b0055-153">asp-all-route-data</span></span>

<span data-ttu-id="b0055-154">`asp-all-route-data`Consente di creare un dizionario di coppie chiave-valore in cui la chiave è il nome del parametro e il valore è il valore associato a tale chiave.</span><span class="sxs-lookup"><span data-stu-id="b0055-154">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="b0055-155">Come nell'esempio seguente viene illustrato, viene creato un dizionario inline e i dati vengono passati alla visualizzazione razor.</span><span class="sxs-lookup"><span data-stu-id="b0055-155">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="b0055-156">In alternativa, i dati potrebbero essere anche passati con il modello.</span><span class="sxs-lookup"><span data-stu-id="b0055-156">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="b0055-157">Il codice precedente genera il seguente URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="b0055-157">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="b0055-158">Quando viene selezionato il collegamento, il metodo controller `EvaluationsCurrent` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="b0055-158">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="b0055-159">Viene chiamato perché tale controller presenta due parametri di stringa che corrispondono a ciò che è stato creato dal `asp-all-route-data` dizionario.</span><span class="sxs-lookup"><span data-stu-id="b0055-159">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="b0055-160">Se le chiavi nella corrispondenza dizionario parametri della route, tali valori saranno sostituiti nella route come appropriato e gli altri valori non corrispondenti verranno generati come parametri di richiesta.</span><span class="sxs-lookup"><span data-stu-id="b0055-160">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="b0055-161">frammento di ASP</span><span class="sxs-lookup"><span data-stu-id="b0055-161">asp-fragment</span></span>

<span data-ttu-id="b0055-162">`asp-fragment`definisce un frammento di URL da aggiungere all'URL.</span><span class="sxs-lookup"><span data-stu-id="b0055-162">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="b0055-163">L'Helper di Tag di ancoraggio aggiungerà il cancelletto (#).</span><span class="sxs-lookup"><span data-stu-id="b0055-163">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="b0055-164">Se si crea un tag:</span><span class="sxs-lookup"><span data-stu-id="b0055-164">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="b0055-165">L'URL generato sarà: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="b0055-165">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="b0055-166">Tag di hash sono utili quando si compilano applicazioni sul lato client.</span><span class="sxs-lookup"><span data-stu-id="b0055-166">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="b0055-167">Possono essere utilizzati per contrassegnare e la ricerca in JavaScript, ad esempio semplice.</span><span class="sxs-lookup"><span data-stu-id="b0055-167">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="b0055-168">area di ASP</span><span class="sxs-lookup"><span data-stu-id="b0055-168">asp-area</span></span>

<span data-ttu-id="b0055-169">`asp-area`Imposta il nome dell'area che ASP.NET Core viene utilizzato per impostare la route appropriata.</span><span class="sxs-lookup"><span data-stu-id="b0055-169">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="b0055-170">Di seguito sono riportati esempi di come l'attributo area causa una modifica del mapping delle route.</span><span class="sxs-lookup"><span data-stu-id="b0055-170">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="b0055-171">Impostazione `asp-area` ai blog prefissi di directory `Areas/Blogs` a tutte le route delle controller associato e le viste per il tag di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="b0055-171">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="b0055-172">Nome progetto</span><span class="sxs-lookup"><span data-stu-id="b0055-172">Project name</span></span>
  * <span data-ttu-id="b0055-173">wwwroot</span><span class="sxs-lookup"><span data-stu-id="b0055-173">wwwroot</span></span>
  * <span data-ttu-id="b0055-174">Aree</span><span class="sxs-lookup"><span data-stu-id="b0055-174">Areas</span></span>
    * <span data-ttu-id="b0055-175">Blog</span><span class="sxs-lookup"><span data-stu-id="b0055-175">Blogs</span></span>
      * <span data-ttu-id="b0055-176">Controller</span><span class="sxs-lookup"><span data-stu-id="b0055-176">Controllers</span></span>
        * <span data-ttu-id="b0055-177">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="b0055-177">HomeController.cs</span></span>
      * <span data-ttu-id="b0055-178">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="b0055-178">Views</span></span>
        * <span data-ttu-id="b0055-179">Home</span><span class="sxs-lookup"><span data-stu-id="b0055-179">Home</span></span>
          * <span data-ttu-id="b0055-180">Cshtml</span><span class="sxs-lookup"><span data-stu-id="b0055-180">Index.cshtml</span></span>
          * <span data-ttu-id="b0055-181">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="b0055-181">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="b0055-182">Controller</span><span class="sxs-lookup"><span data-stu-id="b0055-182">Controllers</span></span>

<span data-ttu-id="b0055-183">Specificare un tag area valido, ad esempio ```area="Blogs"``` quando si fa riferimento il ```AboutBlog.cshtml``` file sarà simile al seguente utilizzando l'Helper di Tag di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="b0055-183">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="b0055-184">Il codice HTML generato includerà il segmento di aree e verrà modificato come segue:</span><span class="sxs-lookup"><span data-stu-id="b0055-184">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="b0055-185">Per le aree MVC funzionare in un'applicazione web, il modello di route deve includere un riferimento all'area, se esiste.</span><span class="sxs-lookup"><span data-stu-id="b0055-185">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="b0055-186">Modello, che è il secondo parametro del `routes.MapRoute` chiamata al metodo, verrà visualizzato come:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="b0055-186">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="b0055-187">protocollo di ASP</span><span class="sxs-lookup"><span data-stu-id="b0055-187">asp-protocol</span></span>

<span data-ttu-id="b0055-188">Il `asp-protocol` consente di specificare un protocollo (ad esempio `https`) nell'indirizzo URL.</span><span class="sxs-lookup"><span data-stu-id="b0055-188">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="b0055-189">Un esempio di Helper di Tag di ancoraggio che include il protocollo apparirà come segue:</span><span class="sxs-lookup"><span data-stu-id="b0055-189">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="b0055-190">e genererà HTML come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b0055-190">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="b0055-191">Nell'esempio il dominio è localhost, ma l'Helper di Tag di ancoraggio utilizza dominio pubblico del sito Web durante la generazione dell'URL.</span><span class="sxs-lookup"><span data-stu-id="b0055-191">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0055-192">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b0055-192">Additional resources</span></span>

* [<span data-ttu-id="b0055-193">Aree</span><span class="sxs-lookup"><span data-stu-id="b0055-193">Areas</span></span>](xref:mvc/controllers/areas)
