---
title: Helper di Tag di ancoraggio | Documenti Microsoft
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag di ancoraggio
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 74609b515936ec7da8bfc133c27cb69f51311924
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="c95ce-103">Helper di Tag di ancoraggio</span><span class="sxs-lookup"><span data-stu-id="c95ce-103">Anchor Tag Helper</span></span>

<span data-ttu-id="c95ce-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="c95ce-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="c95ce-105">L'Helper di Tag di ancoraggio migliora il punto di ancoraggio HTML (`<a ... ></a>`) tag aggiungendo nuovi attributi.</span><span class="sxs-lookup"><span data-stu-id="c95ce-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="c95ce-106">Il collegamento generato (sul `href` tag) viene creato utilizzando i nuovi attributi.</span><span class="sxs-lookup"><span data-stu-id="c95ce-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="c95ce-107">Tale URL può includere un protocollo facoltativi, ad esempio https.</span><span class="sxs-lookup"><span data-stu-id="c95ce-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="c95ce-108">Negli esempi in questo documento viene usato il controller di voce riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="c95ce-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="c95ce-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="c95ce-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="c95ce-110">Attributi Helper di Tag di ancoraggio</span><span class="sxs-lookup"><span data-stu-id="c95ce-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="c95ce-111">ASP controller</span><span class="sxs-lookup"><span data-stu-id="c95ce-111">asp-controller</span></span>

<span data-ttu-id="c95ce-112">`asp-controller`viene utilizzato per associare il controller verrà utilizzato per generare l'URL.</span><span class="sxs-lookup"><span data-stu-id="c95ce-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="c95ce-113">Il controller specificato deve essere presente nel progetto corrente.</span><span class="sxs-lookup"><span data-stu-id="c95ce-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="c95ce-114">Il codice seguente vengono elencati tutti gli altoparlanti:</span><span class="sxs-lookup"><span data-stu-id="c95ce-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="c95ce-115">Il markup generato sarà:</span><span class="sxs-lookup"><span data-stu-id="c95ce-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="c95ce-116">Se il `asp-controller` specificato e `asp-action` non è, il valore predefinito `asp-action` sarà metodo controller predefinito per la visualizzazione attualmente in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c95ce-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="c95ce-117">Che nell'esempio precedente, se viene `asp-action` viene lasciata fuori, e viene generato questo Helper di Tag di ancoraggio da *HomeController*del `Index` vista (**/Home**), il markup generato sarà:</span><span class="sxs-lookup"><span data-stu-id="c95ce-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="c95ce-118">azione di ASP</span><span class="sxs-lookup"><span data-stu-id="c95ce-118">asp-action</span></span>

<span data-ttu-id="c95ce-119">`asp-action`è il nome del metodo di azione nel controller che verranno inclusi nell'oggetto generato `href`.</span><span class="sxs-lookup"><span data-stu-id="c95ce-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="c95ce-120">Ad esempio, il codice seguente imposta generato `href` in modo che punti alla pagina di dettaglio del relatore:</span><span class="sxs-lookup"><span data-stu-id="c95ce-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="c95ce-121">Il markup generato sarà:</span><span class="sxs-lookup"><span data-stu-id="c95ce-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="c95ce-122">Se non `asp-controller` attributo viene specificato, verrà utilizzato il controller predefinito chiamando la visualizzazione di cui eseguire la vista corrente.</span><span class="sxs-lookup"><span data-stu-id="c95ce-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="c95ce-123">Se l'attributo `asp-action` è `Index`, nessuna azione viene aggiunto all'URL, determinando il valore predefinito `Index` metodo chiamato.</span><span class="sxs-lookup"><span data-stu-id="c95ce-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="c95ce-124">L'azione specificata (o per impostazione predefinita), deve esistere nel controller a cui fa riferimento `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="c95ce-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="c95ce-125">pagina ASP</span><span class="sxs-lookup"><span data-stu-id="c95ce-125">asp-page</span></span>

<span data-ttu-id="c95ce-126">Utilizzare il `asp-page` attributo in un tag di ancoraggio per impostare il relativo URL in modo che punti a una pagina specifica.</span><span class="sxs-lookup"><span data-stu-id="c95ce-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="c95ce-127">Prefisso per il nome di pagina con una barra "/" crea l'URL.</span><span class="sxs-lookup"><span data-stu-id="c95ce-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="c95ce-128">Nell'esempio seguente l'URL punta alla pagina "Voce" nella directory corrente.</span><span class="sxs-lookup"><span data-stu-id="c95ce-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="c95ce-129">Il `asp-page` attributo nell'esempio di codice precedente viene eseguito il rendering di output HTML nella visualizzazione è simile al frammento riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c95ce-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="c95ce-130">Il `asp-page` attributo si escludono a vicenda con la `asp-route`, `asp-controller`, e `asp-action` gli attributi.</span><span class="sxs-lookup"><span data-stu-id="c95ce-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="c95ce-131">Tuttavia, `asp-page` può essere utilizzato con `asp-route-id` per controllare il routing, come illustrato nell'esempio di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c95ce-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="c95ce-132">Il `asp-route-id` produce il seguente output:</span><span class="sxs-lookup"><span data-stu-id="c95ce-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="c95ce-133">Utilizzare il `asp-page` attributo nelle pagine Razor, l'URL deve essere un percorso relativo, ad esempio `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="c95ce-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="c95ce-134">I percorsi relativi nel `asp-page` attributo non sono disponibili nelle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="c95ce-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="c95ce-135">Utilizzare la sintassi "/" per le visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="c95ce-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="c95ce-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="c95ce-136">asp-route-{value}</span></span>

<span data-ttu-id="c95ce-137">`asp-route-`è un prefisso di route con caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="c95ce-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="c95ce-138">Qualsiasi valore inserito dopo il trattino finale verrà interpretato come un parametro di route possibili.</span><span class="sxs-lookup"><span data-stu-id="c95ce-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="c95ce-139">Se non viene trovata una route predefinita, per href generato come valore di parametro di richiesta e verrà aggiunto il prefisso della route.</span><span class="sxs-lookup"><span data-stu-id="c95ce-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="c95ce-140">In caso contrario verrà sostituito nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="c95ce-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="c95ce-141">Presupponendo che sia un metodo del controller definiti come segue:</span><span class="sxs-lookup"><span data-stu-id="c95ce-141">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="c95ce-142">E che il modello di route predefiniti definito nel *Startup.cs* come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c95ce-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="c95ce-143">Il **cshtml** file che contiene l'Helper di Tag di ancoraggio necessarie per l'utilizzo di **altoparlante** parametro di modello passato dal controller per la visualizzazione è indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c95ce-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="c95ce-144">Il codice HTML generato verrà quindi modificato come segue perché **id** è stato trovato nella route predefinita.</span><span class="sxs-lookup"><span data-stu-id="c95ce-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="c95ce-145">Se il prefisso di route non fa parte del modello di routing trovato, che si verifica con i seguenti **cshtml** file:</span><span class="sxs-lookup"><span data-stu-id="c95ce-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="c95ce-146">Il codice HTML generato verrà quindi modificato come segue perché **speakerid** non è stato trovato nella route corrispondenza:</span><span class="sxs-lookup"><span data-stu-id="c95ce-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="c95ce-147">Se il valore `asp-controller` o `asp-action` non vengono specificati, viene seguito l'elaborazione predefinita stesso in cui è nel `asp-route` attributo.</span><span class="sxs-lookup"><span data-stu-id="c95ce-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="c95ce-148">route di ASP</span><span class="sxs-lookup"><span data-stu-id="c95ce-148">asp-route</span></span>

<span data-ttu-id="c95ce-149">`asp-route`fornisce un modo per creare un URL che colleghi direttamente a una route denominata.</span><span class="sxs-lookup"><span data-stu-id="c95ce-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="c95ce-150">Gli attributi di routing una route può essere denominata come illustrato nel `SpeakerController` e utilizzato nel relativo `Evaluations` metodo.</span><span class="sxs-lookup"><span data-stu-id="c95ce-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="c95ce-151">`Name = "speakerevals"`indica l'Helper di Tag di ancoraggio per generare una route direttamente a tale metodo controller utilizzando l'URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="c95ce-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="c95ce-152">Se `asp-controller` o `asp-action` è specificato in aggiunta al `asp-route`, la route generata potrebbe non essere quello previsto.</span><span class="sxs-lookup"><span data-stu-id="c95ce-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="c95ce-153">`asp-route`non devono essere usate con uno degli attributi `asp-controller` o `asp-action` per evitare un conflitto di route.</span><span class="sxs-lookup"><span data-stu-id="c95ce-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="c95ce-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="c95ce-154">asp-all-route-data</span></span>

<span data-ttu-id="c95ce-155">`asp-all-route-data`Consente di creare un dizionario di coppie chiave-valore in cui la chiave è il nome del parametro e il valore è il valore associato a tale chiave.</span><span class="sxs-lookup"><span data-stu-id="c95ce-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="c95ce-156">Come nell'esempio seguente viene illustrato, viene creato un dizionario inline e i dati vengono passati alla visualizzazione razor.</span><span class="sxs-lookup"><span data-stu-id="c95ce-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="c95ce-157">In alternativa, i dati potrebbero essere anche passati con il modello.</span><span class="sxs-lookup"><span data-stu-id="c95ce-157">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="c95ce-158">Il codice precedente genera il seguente URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="c95ce-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="c95ce-159">Quando viene selezionato il collegamento, il metodo controller `EvaluationsCurrent` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="c95ce-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="c95ce-160">Viene chiamato perché tale controller presenta due parametri di stringa che corrispondono a ciò che è stato creato dal `asp-all-route-data` dizionario.</span><span class="sxs-lookup"><span data-stu-id="c95ce-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="c95ce-161">Se le chiavi nella corrispondenza dizionario parametri della route, tali valori saranno sostituiti nella route come appropriato e gli altri valori non corrispondenti verranno generati come parametri di richiesta.</span><span class="sxs-lookup"><span data-stu-id="c95ce-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="c95ce-162">frammento di ASP</span><span class="sxs-lookup"><span data-stu-id="c95ce-162">asp-fragment</span></span>

<span data-ttu-id="c95ce-163">`asp-fragment`definisce un frammento di URL da aggiungere all'URL.</span><span class="sxs-lookup"><span data-stu-id="c95ce-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="c95ce-164">L'Helper di Tag di ancoraggio aggiungerà il cancelletto (#).</span><span class="sxs-lookup"><span data-stu-id="c95ce-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="c95ce-165">Se si crea un tag:</span><span class="sxs-lookup"><span data-stu-id="c95ce-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="c95ce-166">L'URL generato sarà: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="c95ce-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="c95ce-167">Tag di hash sono utili quando si compilano applicazioni sul lato client.</span><span class="sxs-lookup"><span data-stu-id="c95ce-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="c95ce-168">Possono essere utilizzati per contrassegnare e la ricerca in JavaScript, ad esempio semplice.</span><span class="sxs-lookup"><span data-stu-id="c95ce-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="c95ce-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="c95ce-169">asp-area</span></span>

<span data-ttu-id="c95ce-170">`asp-area`Imposta il nome dell'area che ASP.NET Core viene utilizzato per impostare la route appropriata.</span><span class="sxs-lookup"><span data-stu-id="c95ce-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="c95ce-171">Di seguito sono riportati esempi di come l'attributo area causa una modifica del mapping delle route.</span><span class="sxs-lookup"><span data-stu-id="c95ce-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="c95ce-172">Impostazione `asp-area` ai blog prefissi di directory `Areas/Blogs` a tutte le route delle controller associato e le viste per il tag di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="c95ce-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="c95ce-173">Nome progetto</span><span class="sxs-lookup"><span data-stu-id="c95ce-173">Project name</span></span>
  * <span data-ttu-id="c95ce-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="c95ce-174">wwwroot</span></span>
  * <span data-ttu-id="c95ce-175">Aree</span><span class="sxs-lookup"><span data-stu-id="c95ce-175">Areas</span></span>
    * <span data-ttu-id="c95ce-176">Blog</span><span class="sxs-lookup"><span data-stu-id="c95ce-176">Blogs</span></span>
      * <span data-ttu-id="c95ce-177">Controller</span><span class="sxs-lookup"><span data-stu-id="c95ce-177">Controllers</span></span>
        * <span data-ttu-id="c95ce-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="c95ce-178">HomeController.cs</span></span>
      * <span data-ttu-id="c95ce-179">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="c95ce-179">Views</span></span>
        * <span data-ttu-id="c95ce-180">Home</span><span class="sxs-lookup"><span data-stu-id="c95ce-180">Home</span></span>
          * <span data-ttu-id="c95ce-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c95ce-181">Index.cshtml</span></span>
          * <span data-ttu-id="c95ce-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="c95ce-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="c95ce-183">Controller</span><span class="sxs-lookup"><span data-stu-id="c95ce-183">Controllers</span></span>

<span data-ttu-id="c95ce-184">Specificare un tag area valido, ad esempio ```area="Blogs"``` quando si fa riferimento il ```AboutBlog.cshtml``` file sarà simile al seguente utilizzando l'Helper di Tag di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="c95ce-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="c95ce-185">Il codice HTML generato includerà il segmento di aree e verrà modificato come segue:</span><span class="sxs-lookup"><span data-stu-id="c95ce-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="c95ce-186">Per le aree MVC funzionare in un'applicazione web, il modello di route deve includere un riferimento all'area, se esiste.</span><span class="sxs-lookup"><span data-stu-id="c95ce-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="c95ce-187">Modello, che è il secondo parametro del `routes.MapRoute` chiamata al metodo, verrà visualizzato come:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="c95ce-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="c95ce-188">protocollo di ASP</span><span class="sxs-lookup"><span data-stu-id="c95ce-188">asp-protocol</span></span>

<span data-ttu-id="c95ce-189">Il `asp-protocol` consente di specificare un protocollo (ad esempio `https`) nell'indirizzo URL.</span><span class="sxs-lookup"><span data-stu-id="c95ce-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="c95ce-190">Un esempio di Helper di Tag di ancoraggio che include il protocollo apparirà come segue:</span><span class="sxs-lookup"><span data-stu-id="c95ce-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="c95ce-191">e genererà HTML come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c95ce-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="c95ce-192">Nell'esempio il dominio è localhost, ma l'Helper di Tag di ancoraggio utilizza dominio pubblico del sito Web durante la generazione dell'URL.</span><span class="sxs-lookup"><span data-stu-id="c95ce-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c95ce-193">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c95ce-193">Additional resources</span></span>

* [<span data-ttu-id="c95ce-194">Aree</span><span class="sxs-lookup"><span data-stu-id="c95ce-194">Areas</span></span>](xref:mvc/controllers/areas)
