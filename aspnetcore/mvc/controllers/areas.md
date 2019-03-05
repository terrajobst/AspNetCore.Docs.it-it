---
title: Aree in ASP.NET Core
author: rick-anderson
description: Informazioni sulle aree, una funzionalità di ASP.NET MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833527"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="f60e1-103">Aree in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f60e1-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="f60e1-104">Di [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f60e1-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f60e1-105">Le aree sono una funzionalità di ASP.NET che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni).</span><span class="sxs-lookup"><span data-stu-id="f60e1-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="f60e1-106">Usando le aree si crea una gerarchia per il routing aggiungendo un altro parametro di route, `area`, a `controller` e `action` o a una pagina Razor `page`.</span><span class="sxs-lookup"><span data-stu-id="f60e1-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="f60e1-107">Le aree consentono di suddividere un'app Web ASP.NET Core in gruppi funzionali più piccoli, ognuno con un proprio set di pagine Razor, controller, visualizzazioni e modelli.</span><span class="sxs-lookup"><span data-stu-id="f60e1-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="f60e1-108">Un'area è in effetti una struttura all'interno di un'app.</span><span class="sxs-lookup"><span data-stu-id="f60e1-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="f60e1-109">In un progetto Web ASP.NET Core i componenti logici come pagine, modello, controller e visualizzazione si trovano in cartelle diverse.</span><span class="sxs-lookup"><span data-stu-id="f60e1-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="f60e1-110">Il runtime di ASP.NET Core crea una relazione tra questi componenti usando convenzioni di denominazione.</span><span class="sxs-lookup"><span data-stu-id="f60e1-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="f60e1-111">Per un'app di grandi dimensioni può risultare utile suddividere l'app in aree di funzionalità di alto livello distinte.</span><span class="sxs-lookup"><span data-stu-id="f60e1-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="f60e1-112">È il caso, ad esempio, di un'app di e-commerce con più business unit, ad esempio per il completamento della transazione, la fatturazione e la ricerca.</span><span class="sxs-lookup"><span data-stu-id="f60e1-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="f60e1-113">Ognuna di queste business unit avrà la propria area in cui contenere visualizzazioni, controller, pagine Razor e modelli.</span><span class="sxs-lookup"><span data-stu-id="f60e1-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="f60e1-114">In un progetto è consigliabile usare le aree quando:</span><span class="sxs-lookup"><span data-stu-id="f60e1-114">Consider using Areas in an project when:</span></span>

* <span data-ttu-id="f60e1-115">L'app è costituita da più componenti funzionali di alto livello che possono rimanere logicamente separati.</span><span class="sxs-lookup"><span data-stu-id="f60e1-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="f60e1-116">Si vuole suddividere l'app in modo da poter lavorare su ogni area funzionale in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="f60e1-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="f60e1-117">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f60e1-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="f60e1-118">Il download di esempio fornisce un'app di base per testare le aree.</span><span class="sxs-lookup"><span data-stu-id="f60e1-118">The download sample provides a basic app for testing areas.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="f60e1-119">Aree per i controller con visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="f60e1-119">Areas for controllers with views</span></span>

<span data-ttu-id="f60e1-120">Una tipica app Web ASP.NET Core che usa aree, controller e visualizzazioni contiene quanto segue:</span><span class="sxs-lookup"><span data-stu-id="f60e1-120">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="f60e1-121">Una [struttura di cartelle dell'area](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="f60e1-121">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="f60e1-122">Controller decorati con l'attributo [&lbrack;Area&rbrack;](#attribute) per associare il controller all'area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="f60e1-122">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span></span>
* <span data-ttu-id="f60e1-123">La [route di area aggiunta all'avvio](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="f60e1-123">The [area route added to startup](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span></span>

## <a name="area-folder-structure"></a><span data-ttu-id="f60e1-124">Struttura di cartelle dell'area</span><span class="sxs-lookup"><span data-stu-id="f60e1-124">Area folder structure</span></span>
<span data-ttu-id="f60e1-125">Si consideri un'applicazione che ha due gruppi logici, *Prodotti* e *Servizi*.</span><span class="sxs-lookup"><span data-stu-id="f60e1-125">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="f60e1-126">Usando le aree, la struttura delle cartelle sarebbe simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="f60e1-126">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="f60e1-127">Nome progetto</span><span class="sxs-lookup"><span data-stu-id="f60e1-127">Project name</span></span>
  * <span data-ttu-id="f60e1-128">Aree</span><span class="sxs-lookup"><span data-stu-id="f60e1-128">Areas</span></span>
    * <span data-ttu-id="f60e1-129">Prodotti</span><span class="sxs-lookup"><span data-stu-id="f60e1-129">Products</span></span>
      * <span data-ttu-id="f60e1-130">Controllers</span><span class="sxs-lookup"><span data-stu-id="f60e1-130">Controllers</span></span>
        * <span data-ttu-id="f60e1-131">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="f60e1-131">HomeController.cs</span></span>
        * <span data-ttu-id="f60e1-132">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="f60e1-132">ManageController.cs</span></span>
      * <span data-ttu-id="f60e1-133">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="f60e1-133">Views</span></span>
        * <span data-ttu-id="f60e1-134">Home</span><span class="sxs-lookup"><span data-stu-id="f60e1-134">Home</span></span>
          * <span data-ttu-id="f60e1-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f60e1-135">Index.cshtml</span></span>
        * <span data-ttu-id="f60e1-136">Gestisci</span><span class="sxs-lookup"><span data-stu-id="f60e1-136">Manage</span></span>
          * <span data-ttu-id="f60e1-137">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f60e1-137">Index.cshtml</span></span>
          * <span data-ttu-id="f60e1-138">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="f60e1-138">About.cshtml</span></span>
    * <span data-ttu-id="f60e1-139">Servizi</span><span class="sxs-lookup"><span data-stu-id="f60e1-139">Services</span></span>
      * <span data-ttu-id="f60e1-140">Controllers</span><span class="sxs-lookup"><span data-stu-id="f60e1-140">Controllers</span></span>
        * <span data-ttu-id="f60e1-141">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="f60e1-141">HomeController.cs</span></span>
      * <span data-ttu-id="f60e1-142">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="f60e1-142">Views</span></span>
        * <span data-ttu-id="f60e1-143">Home</span><span class="sxs-lookup"><span data-stu-id="f60e1-143">Home</span></span>
          * <span data-ttu-id="f60e1-144">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f60e1-144">Index.cshtml</span></span>

<span data-ttu-id="f60e1-145">Anche se il layout precedente è tipico quando si usano le aree, per usare questa struttura di cartelle sono necessari solo i file delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="f60e1-145">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="f60e1-146">L'individuazione delle visualizzazioni cercherà un file di visualizzazione area corrispondente in quest'ordine:</span><span class="sxs-lookup"><span data-stu-id="f60e1-146">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="f60e1-147">Il percorso delle cartelle non di visualizzazioni, come *Controller* e *Modelli* **non** è rilevante.</span><span class="sxs-lookup"><span data-stu-id="f60e1-147">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="f60e1-148">Ad esempio, le cartelle *Controller* e *Modelli* non sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="f60e1-148">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="f60e1-149">Il contenuto di *Controller* e *Modelli* è codice che viene compilato in un file DLL.</span><span class="sxs-lookup"><span data-stu-id="f60e1-149">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="f60e1-150">Il contenuto di *Visualizzazioni* non viene compilato finché non viene effettuata una richiesta a tale visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f60e1-150">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="f60e1-151">Associare il controller a un'area</span><span class="sxs-lookup"><span data-stu-id="f60e1-151">Associate the controller with an Area</span></span>

<span data-ttu-id="f60e1-152">I controller di area sono identificati dall'attributo [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute):</span><span class="sxs-lookup"><span data-stu-id="f60e1-152">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="f60e1-153">Aggiungere una route di area</span><span class="sxs-lookup"><span data-stu-id="f60e1-153">Add Area route</span></span>

<span data-ttu-id="f60e1-154">Le route di area usano in genere il routing convenzionale anziché il routing con attributi.</span><span class="sxs-lookup"><span data-stu-id="f60e1-154">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="f60e1-155">Il routing convenzionale dipende dall'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="f60e1-155">Conventional routing is order-dependent.</span></span> <span data-ttu-id="f60e1-156">In generale, le route con aree devono essere posizionate prima delle altre nella tabella di route poiché sono più specifiche rispetto alle route senza un'area.</span><span class="sxs-lookup"><span data-stu-id="f60e1-156">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="f60e1-157">`{area:...}` può essere usato come token nei modelli di route se lo spazio URL è uniforme in tutte le aree:</span><span class="sxs-lookup"><span data-stu-id="f60e1-157">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="f60e1-158">Nel codice precedente, `exists` applica il vincolo che la route deve corrispondere a un'area.</span><span class="sxs-lookup"><span data-stu-id="f60e1-158">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="f60e1-159">L'uso di `{area:...}` è il meccanismo meno complesso per l'aggiunta del routing alle aree.</span><span class="sxs-lookup"><span data-stu-id="f60e1-159">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="f60e1-160">Il codice seguente usa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> per creare due route di area denominate:</span><span class="sxs-lookup"><span data-stu-id="f60e1-160">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="f60e1-161">Quando si usa `MapAreaRoute` con ASP.NET Core 2.2, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="f60e1-161">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="f60e1-162">Per altre informazioni, vedere [Aree](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="f60e1-162">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-areas"></a><span data-ttu-id="f60e1-163">Generazione di collegamenti con le aree</span><span class="sxs-lookup"><span data-stu-id="f60e1-163">Link Generation with Areas</span></span>

<span data-ttu-id="f60e1-164">Il codice seguente dal [download di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) mostra la generazione di collegamenti con l'area specificata:</span><span class="sxs-lookup"><span data-stu-id="f60e1-164">The following code from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="f60e1-165">I collegamenti generati con il codice precedente sono validi ovunque nell'app.</span><span class="sxs-lookup"><span data-stu-id="f60e1-165">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="f60e1-166">Il download di esempio include una [visualizzazione parziale](xref:mvc/views/partial) che contiene i collegamenti precedenti e gli stessi collegamenti senza specificare l'area.</span><span class="sxs-lookup"><span data-stu-id="f60e1-166">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="f60e1-167">La visualizzazione parziale viene referenziata nel [file di layout](), in modo che ogni pagina nell'app mostri i collegamenti generati.</span><span class="sxs-lookup"><span data-stu-id="f60e1-167">The partial view is referenced in the [layout file](), so every page in the app displays the generated links.</span></span> <span data-ttu-id="f60e1-168">I collegamenti generati senza specificare l'area sono validi solo quando sono referenziati da una pagina nella stessa area e controller.</span><span class="sxs-lookup"><span data-stu-id="f60e1-168">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="f60e1-169">Quando l'area o il controller non sono specificati, il routing dipende dai valori di *ambiente*.</span><span class="sxs-lookup"><span data-stu-id="f60e1-169">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="f60e1-170">I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento.</span><span class="sxs-lookup"><span data-stu-id="f60e1-170">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="f60e1-171">In molti casi per l'app di esempio, l'uso dei valori di ambiente genera collegamenti non corretti.</span><span class="sxs-lookup"><span data-stu-id="f60e1-171">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="f60e1-172">Per altre informazioni, vedere [Routing ad azioni del controller](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="f60e1-172">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="f60e1-173">Layout condiviso per le aree tramite il file _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="f60e1-173">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="f60e1-174">Per condividere un layout comune per l'intera app, spostare *_ViewStart.cshtml* nella cartella radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f60e1-174">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="f60e1-175">Modificare la cartella dell'area predefinita in cui sono archiviate le visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="f60e1-175">Change default area folder where views are stored</span></span>

<span data-ttu-id="f60e1-176">Il codice seguente modifica la cartella dell'area predefinita da `"Areas"` a `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="f60e1-176">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a><span data-ttu-id="f60e1-177">Pubblicazione di aree</span><span class="sxs-lookup"><span data-stu-id="f60e1-177">Publishing Areas</span></span>

<span data-ttu-id="f60e1-178">Tutti i file `*.cshtml` e `wwwroot/**` vengono pubblicati per l'output quando `<Project Sdk="Microsoft.NET.Sdk.Web">` viene incluso nel file con estensione *csproj*.</span><span class="sxs-lookup"><span data-stu-id="f60e1-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
