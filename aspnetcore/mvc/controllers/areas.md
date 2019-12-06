---
title: Aree in ASP.NET Core
author: rick-anderson
description: Informazioni sulle aree, una funzionalità di ASP.NET MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni).
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 7e02a21361e0e2148b29a3ae0f1ba25e68239e13
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881123"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="bf8e0-103">Aree in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf8e0-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="bf8e0-104">Di [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bf8e0-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bf8e0-105">Le aree sono una funzionalità di ASP.NET che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="bf8e0-106">Usando le aree si crea una gerarchia per il routing aggiungendo un altro parametro di route, `area`, a `controller` e `action` o a una pagina Razor `page`.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="bf8e0-107">Le aree consentono di suddividere un'app Web ASP.NET Core in gruppi funzionali più piccoli, ognuno con un proprio set di pagine Razor, controller, visualizzazioni e modelli.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="bf8e0-108">Un'area è in effetti una struttura all'interno di un'app.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="bf8e0-109">In un progetto Web ASP.NET Core i componenti logici come pagine, modello, controller e visualizzazione si trovano in cartelle diverse.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="bf8e0-110">Il runtime di ASP.NET Core crea una relazione tra questi componenti usando convenzioni di denominazione.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="bf8e0-111">Per un'app di grandi dimensioni può risultare utile suddividere l'app in aree di funzionalità di alto livello distinte.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="bf8e0-112">È il caso, ad esempio, di un'app di e-commerce con più business unit, ad esempio per il completamento della transazione, la fatturazione e la ricerca.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="bf8e0-113">Ognuna di queste business unit avrà la propria area in cui contenere visualizzazioni, controller, pagine Razor e modelli.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="bf8e0-114">In un progetto è consigliabile usare le aree quando:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="bf8e0-115">L'app è costituita da più componenti funzionali di alto livello che possono rimanere logicamente separati.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="bf8e0-116">Si vuole suddividere l'app in modo da poter lavorare su ogni area funzionale in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="bf8e0-117">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="bf8e0-118">Il download di esempio fornisce un'app di base per testare le aree.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="bf8e0-119">Se si usa Razor Pages, vedere [Aree con pagine Razor](#areas-with-razor-pages) in questo documento.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="bf8e0-120">Aree per i controller con visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="bf8e0-120">Areas for controllers with views</span></span>

<span data-ttu-id="bf8e0-121">Una tipica app Web ASP.NET Core che usa aree, controller e visualizzazioni contiene quanto segue:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="bf8e0-122">Una [struttura di cartelle dell'area](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="bf8e0-123">Controller con l'attributo [`[Area]`](#attribute) per associare il controller all'area:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-123">Controllers with the [`[Area]`](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="bf8e0-124">La [route di area aggiunta all'avvio](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="bf8e0-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="bf8e0-125">Struttura di cartelle dell'area</span><span class="sxs-lookup"><span data-stu-id="bf8e0-125">Area folder structure</span></span>

<span data-ttu-id="bf8e0-126">Si consideri un'applicazione che ha due gruppi logici, *Prodotti* e *Servizi*.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="bf8e0-127">Usando le aree, la struttura delle cartelle sarebbe simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="bf8e0-128">Nome progetto</span><span class="sxs-lookup"><span data-stu-id="bf8e0-128">Project name</span></span>
  * <span data-ttu-id="bf8e0-129">Aree</span><span class="sxs-lookup"><span data-stu-id="bf8e0-129">Areas</span></span>
    * <span data-ttu-id="bf8e0-130">Products</span><span class="sxs-lookup"><span data-stu-id="bf8e0-130">Products</span></span>
      * <span data-ttu-id="bf8e0-131">Controllers</span><span class="sxs-lookup"><span data-stu-id="bf8e0-131">Controllers</span></span>
        * <span data-ttu-id="bf8e0-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="bf8e0-132">HomeController.cs</span></span>
        * <span data-ttu-id="bf8e0-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="bf8e0-133">ManageController.cs</span></span>
      * <span data-ttu-id="bf8e0-134">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="bf8e0-134">Views</span></span>
        * <span data-ttu-id="bf8e0-135">Home page di</span><span class="sxs-lookup"><span data-stu-id="bf8e0-135">Home</span></span>
          * <span data-ttu-id="bf8e0-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="bf8e0-136">Index.cshtml</span></span>
        * <span data-ttu-id="bf8e0-137">Gestisci</span><span class="sxs-lookup"><span data-stu-id="bf8e0-137">Manage</span></span>
          * <span data-ttu-id="bf8e0-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="bf8e0-138">Index.cshtml</span></span>
          * <span data-ttu-id="bf8e0-139">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="bf8e0-139">About.cshtml</span></span>
    * <span data-ttu-id="bf8e0-140">Servizi</span><span class="sxs-lookup"><span data-stu-id="bf8e0-140">Services</span></span>
      * <span data-ttu-id="bf8e0-141">Controllers</span><span class="sxs-lookup"><span data-stu-id="bf8e0-141">Controllers</span></span>
        * <span data-ttu-id="bf8e0-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="bf8e0-142">HomeController.cs</span></span>
      * <span data-ttu-id="bf8e0-143">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="bf8e0-143">Views</span></span>
        * <span data-ttu-id="bf8e0-144">Home page di</span><span class="sxs-lookup"><span data-stu-id="bf8e0-144">Home</span></span>
          * <span data-ttu-id="bf8e0-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="bf8e0-145">Index.cshtml</span></span>

<span data-ttu-id="bf8e0-146">Anche se il layout precedente è tipico quando si usano le aree, per usare questa struttura di cartelle sono necessari solo i file delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="bf8e0-147">L'individuazione delle visualizzazioni cercherà un file di visualizzazione area corrispondente in quest'ordine:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
```

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="bf8e0-148">Associare il controller a un'area</span><span class="sxs-lookup"><span data-stu-id="bf8e0-148">Associate the controller with an Area</span></span>

<span data-ttu-id="bf8e0-149">I controller di area sono identificati dall'attributo [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute):</span><span class="sxs-lookup"><span data-stu-id="bf8e0-149">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="bf8e0-150">Aggiungere una route di area</span><span class="sxs-lookup"><span data-stu-id="bf8e0-150">Add Area route</span></span>

<span data-ttu-id="bf8e0-151">Le route di area usano in genere il routing convenzionale anziché il routing con attributi.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-151">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="bf8e0-152">Il routing convenzionale dipende dall'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-152">Conventional routing is order-dependent.</span></span> <span data-ttu-id="bf8e0-153">In generale, le route con aree devono essere posizionate prima delle altre nella tabella di route poiché sono più specifiche rispetto alle route senza un'area.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-153">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="bf8e0-154">`{area:...}` può essere usato come token nei modelli di route se lo spazio URL è uniforme in tutte le aree:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-154">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="bf8e0-155">Nel codice precedente, `exists` applica il vincolo che la route deve corrispondere a un'area.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-155">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="bf8e0-156">L'uso di `{area:...}` è il meccanismo meno complesso per l'aggiunta del routing alle aree.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-156">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="bf8e0-157">Il codice seguente usa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> per creare due route di area denominate:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-157">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="bf8e0-158">Quando si usa `MapAreaRoute` con ASP.NET Core 2.2, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-158">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="bf8e0-159">Per altre informazioni, vedere [Aree](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-159">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="bf8e0-160">Generazione di collegamenti con le aree MVC</span><span class="sxs-lookup"><span data-stu-id="bf8e0-160">Link generation with MVC areas</span></span>

<span data-ttu-id="bf8e0-161">Il codice seguente dal [download di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) mostra la generazione di collegamenti con l'area specificata:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-161">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="bf8e0-162">I collegamenti generati con il codice precedente sono validi ovunque nell'app.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-162">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="bf8e0-163">Il download di esempio include una [visualizzazione parziale](xref:mvc/views/partial) che contiene i collegamenti precedenti e gli stessi collegamenti senza specificare l'area.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-163">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="bf8e0-164">La visualizzazione parziale viene referenziata nel [file di layout](xref:mvc/views/layout), in modo che ogni pagina nell'app mostri i collegamenti generati.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-164">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="bf8e0-165">I collegamenti generati senza specificare l'area sono validi solo quando sono referenziati da una pagina nella stessa area e controller.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-165">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="bf8e0-166">Quando l'area o il controller non sono specificati, il routing dipende dai valori di *ambiente*.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-166">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="bf8e0-167">I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-167">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="bf8e0-168">In molti casi per l'app di esempio, l'uso dei valori di ambiente genera collegamenti non corretti.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-168">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="bf8e0-169">Per altre informazioni, vedere [Routing ad azioni del controller](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-169">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="bf8e0-170">Layout condiviso per le aree tramite il file _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="bf8e0-170">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="bf8e0-171">Per condividere un layout comune per l'intera app, spostare *_ViewStart.cshtml* nella cartella radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-171">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="bf8e0-172">_ViewImports.cshtml</span><span class="sxs-lookup"><span data-stu-id="bf8e0-172">_ViewImports.cshtml</span></span>

<span data-ttu-id="bf8e0-173">Nella posizione standard */Views/_ViewImports.cshtml* non si applica alle aree.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-173">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="bf8e0-174">Per usare gli [helper tag](xref:mvc/views/tag-helpers/intro) comuni, `@using` o `@inject` nella propria area, assicurarsi che un file *_ViewImports.cshtml* appropriato venga [applicato alle visualizzazioni dell'area](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-174">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="bf8e0-175">Se si vuole lo stesso comportamento in tutte le visualizzazioni, spostare */Views/_ViewImports.cshtml* nella radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-175">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="bf8e0-176">Modificare la cartella dell'area predefinita in cui sono archiviate le visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="bf8e0-176">Change default area folder where views are stored</span></span>

<span data-ttu-id="bf8e0-177">Il codice seguente modifica la cartella dell'area predefinita da `"Areas"` a `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-177">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="bf8e0-178">Aree con pagine Razor</span><span class="sxs-lookup"><span data-stu-id="bf8e0-178">Areas with Razor Pages</span></span>

<span data-ttu-id="bf8e0-179">Per le aree con pagine Razor è richiesta una cartella *Areas/<area name>/Pages* nella radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-179">Areas with Razor Pages require an *Areas/<area name>/Pages* folder in the root of the app.</span></span> <span data-ttu-id="bf8e0-180">La struttura di cartelle seguente viene usata con l'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span><span class="sxs-lookup"><span data-stu-id="bf8e0-180">The following folder structure is used with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="bf8e0-181">Nome progetto</span><span class="sxs-lookup"><span data-stu-id="bf8e0-181">Project name</span></span>
  * <span data-ttu-id="bf8e0-182">Aree</span><span class="sxs-lookup"><span data-stu-id="bf8e0-182">Areas</span></span>
    * <span data-ttu-id="bf8e0-183">Products</span><span class="sxs-lookup"><span data-stu-id="bf8e0-183">Products</span></span>
      * <span data-ttu-id="bf8e0-184">Pages</span><span class="sxs-lookup"><span data-stu-id="bf8e0-184">Pages</span></span>
        * <span data-ttu-id="bf8e0-185">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="bf8e0-185">_ViewImports</span></span>
        * <span data-ttu-id="bf8e0-186">Informazioni su</span><span class="sxs-lookup"><span data-stu-id="bf8e0-186">About</span></span>
        * <span data-ttu-id="bf8e0-187">Indice</span><span class="sxs-lookup"><span data-stu-id="bf8e0-187">Index</span></span>
    * <span data-ttu-id="bf8e0-188">Servizi</span><span class="sxs-lookup"><span data-stu-id="bf8e0-188">Services</span></span>
      * <span data-ttu-id="bf8e0-189">Pages</span><span class="sxs-lookup"><span data-stu-id="bf8e0-189">Pages</span></span>
        * <span data-ttu-id="bf8e0-190">Gestisci</span><span class="sxs-lookup"><span data-stu-id="bf8e0-190">Manage</span></span>
          * <span data-ttu-id="bf8e0-191">Informazioni su</span><span class="sxs-lookup"><span data-stu-id="bf8e0-191">About</span></span>
          * <span data-ttu-id="bf8e0-192">Indice</span><span class="sxs-lookup"><span data-stu-id="bf8e0-192">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="bf8e0-193">Generazione del collegamento con pagine Razor e aree</span><span class="sxs-lookup"><span data-stu-id="bf8e0-193">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="bf8e0-194">Il codice seguente dal [download di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) mostra la generazione di collegamenti con l'area specificata (ad esempio, `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="bf8e0-194">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="bf8e0-195">I collegamenti generati con il codice precedente sono validi ovunque nell'app.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-195">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="bf8e0-196">Il download di esempio include una [visualizzazione parziale](xref:mvc/views/partial) che contiene i collegamenti precedenti e gli stessi collegamenti senza specificare l'area.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-196">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="bf8e0-197">La visualizzazione parziale viene referenziata nel [file di layout](xref:mvc/views/layout), in modo che ogni pagina nell'app mostri i collegamenti generati.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-197">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="bf8e0-198">I collegamenti generati senza specificare l'area sono validi solo in caso di riferimento da una pagina nella stessa area.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-198">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="bf8e0-199">Quando l'area non è specificata, il routing dipende dai valori di *ambiente*.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-199">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="bf8e0-200">I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-200">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="bf8e0-201">In molti casi per l'app di esempio, l'uso dei valori di ambiente genera collegamenti non corretti.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-201">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="bf8e0-202">Si considerino, ad esempio, i collegamenti generati dal codice seguente:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-202">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="bf8e0-203">Per il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-203">For the preceding code:</span></span>

* <span data-ttu-id="bf8e0-204">Il collegamento generato da `<a asp-page="/Manage/About">` è corretto solo quando l'ultima richiesta riguarda una pagina nell'area `Services`.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-204">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="bf8e0-205">Ad esempio, `/Services/Manage/`, `/Services/Manage/Index` o `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-205">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="bf8e0-206">Il collegamento generato da `<a asp-page="/About">` è corretto solo quando l'ultima richiesta riguarda una pagina in `/Home`.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-206">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="bf8e0-207">Il codice deriva dal [download di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-207">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="bf8e0-208">Importare lo spazio dei nomi e gli helper tag con il file _ViewImports</span><span class="sxs-lookup"><span data-stu-id="bf8e0-208">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="bf8e0-209">È possibile aggiungere un file *_ViewImports.cshtml* nella cartella *Pages* di ogni area per importare lo spazio dei nomi e gli helper tag in ogni pagina Razor nella cartella.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-209">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="bf8e0-210">Prendere in considerazione l'area *Services* del codice di esempio, che non contiene un file *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-210">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="bf8e0-211">Il markup seguente mostra la pagina Razor */Services/Manage/About*:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-211">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="bf8e0-212">Nel markup precedente:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-212">In the preceding markup:</span></span>

* <span data-ttu-id="bf8e0-213">Il nome di dominio completo deve essere usato per specificare il modello (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-213">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="bf8e0-214">[Gli helper tag](xref:mvc/views/tag-helpers/intro) sono abilitati da `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="bf8e0-214">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="bf8e0-215">Nel download di esempio, l'area Products contiene il file *_ViewImports.cshtml* seguente:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-215">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="bf8e0-216">Il markup seguente mostra la pagina Razor */Products/About*:</span><span class="sxs-lookup"><span data-stu-id="bf8e0-216">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="bf8e0-217">Nel file precedente lo spazio dei nomi e la direttiva `@addTagHelper` vengono importati dal file *Areas/Products/Pages/_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-217">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="bf8e0-218">Per altre informazioni, vedere [Gestione dell'ambito dell'helper tag](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) e [Importazione delle direttive condivise](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="bf8e0-218">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="bf8e0-219">Layout condiviso per le aree di pagine Razor</span><span class="sxs-lookup"><span data-stu-id="bf8e0-219">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="bf8e0-220">Per condividere un layout comune per l'intera app, spostare *_ViewStart.cshtml* nella cartella radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-220">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="bf8e0-221">Pubblicazione di aree</span><span class="sxs-lookup"><span data-stu-id="bf8e0-221">Publishing Areas</span></span>

<span data-ttu-id="bf8e0-222">Tutti i file \*.cshtml e i file all'interno della directory *wwwroot* vengono pubblicati nell'output quando `<Project Sdk="Microsoft.NET.Sdk.Web">` è incluso nel file \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="bf8e0-222">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
