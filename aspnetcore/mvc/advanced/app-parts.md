---
title: Parti dell'applicazione in ASP.NET Core
author: rick-anderson
description: Condividi controller, Visualizza, Razor Pages e altro con le parti dell'applicazione in ASP.NET Core
ms.author: riande
ms.date: 11/10/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 92c408adfad8af3dfd2572b625ae28ef24f64948
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905756"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="7725b-103">Condividi controller, visualizzazioni, Razor Pages e altro con le parti dell'applicazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7725b-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7725b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7725b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7725b-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7725b-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7725b-106">Una *parte dell'applicazione* è un'astrazione sulle risorse di un'app.</span><span class="sxs-lookup"><span data-stu-id="7725b-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="7725b-107">Le parti dell'applicazione consentono ASP.NET Core di individuare controller, componenti di visualizzazione, helper tag, Razor Pages, origini di compilazione Razor e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="7725b-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="7725b-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) è una parte dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7725b-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="7725b-109">`AssemblyPart` incapsula un riferimento all'assembly ed espone i tipi e i riferimenti di compilazione.</span><span class="sxs-lookup"><span data-stu-id="7725b-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="7725b-110">I *provider di funzionalità* funzionano con le parti dell'applicazione per popolare le funzionalità di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7725b-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="7725b-111">Il caso d'uso principale per le parti dell'applicazione consiste nel configurare un'app per individuare (o evitare il caricamento) ASP.NET Core funzionalità da un assembly.</span><span class="sxs-lookup"><span data-stu-id="7725b-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="7725b-112">Ad esempio, potrebbe essere necessario condividere le funzionalità comuni tra più app.</span><span class="sxs-lookup"><span data-stu-id="7725b-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="7725b-113">Con le parti dell'applicazione è possibile condividere un assembly (DLL) contenente controller, visualizzazioni, Razor Pages, origini di compilazione Razor, helper tag e altro ancora con più app.</span><span class="sxs-lookup"><span data-stu-id="7725b-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="7725b-114">La condivisione di un assembly è preferibile alla duplicazione del codice in più progetti.</span><span class="sxs-lookup"><span data-stu-id="7725b-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="7725b-115">ASP.NET Core le app caricano le funzionalità da <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="7725b-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="7725b-116">La classe <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> rappresenta una parte dell'applicazione supportata da un assembly.</span><span class="sxs-lookup"><span data-stu-id="7725b-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="7725b-117">Carica ASP.NET Core funzionalità</span><span class="sxs-lookup"><span data-stu-id="7725b-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="7725b-118">Usare le classi `ApplicationPart` e `AssemblyPart` per individuare e caricare funzionalità di ASP.NET Core (controller, componenti di visualizzazione e così via).</span><span class="sxs-lookup"><span data-stu-id="7725b-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="7725b-119">Il <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tiene traccia delle parti dell'applicazione e dei provider di funzionalità disponibili.</span><span class="sxs-lookup"><span data-stu-id="7725b-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="7725b-120">`ApplicationPartManager` è configurato in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7725b-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="7725b-121">Il codice seguente offre un approccio alternativo alla configurazione di `ApplicationPartManager` usando `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="7725b-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="7725b-122">I due esempi di codice precedenti caricano il `SharedController` da un assembly.</span><span class="sxs-lookup"><span data-stu-id="7725b-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="7725b-123">Il `SharedController` non è presente nel progetto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7725b-123">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="7725b-124">Vedere il download dell'esempio di [soluzione WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="7725b-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="7725b-125">Includi visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="7725b-125">Include views</span></span>

<span data-ttu-id="7725b-126">Per includere le visualizzazioni nell'assembly:</span><span class="sxs-lookup"><span data-stu-id="7725b-126">To include views in the assembly:</span></span>

* <span data-ttu-id="7725b-127">Aggiungere il markup seguente al file di progetto condiviso:</span><span class="sxs-lookup"><span data-stu-id="7725b-127">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="7725b-128">Aggiungere il <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> al <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span><span class="sxs-lookup"><span data-stu-id="7725b-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="7725b-129">Impedisci il caricamento delle risorse</span><span class="sxs-lookup"><span data-stu-id="7725b-129">Prevent loading resources</span></span>

<span data-ttu-id="7725b-130">Le parti dell'applicazione possono essere utilizzate per *evitare* il caricamento di risorse in un assembly o in un percorso specifico.</span><span class="sxs-lookup"><span data-stu-id="7725b-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="7725b-131">Aggiungere o rimuovere membri della raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> per nascondere o rendere disponibili risorse.</span><span class="sxs-lookup"><span data-stu-id="7725b-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="7725b-132">L'ordine delle voci nella raccolta `ApplicationParts` non è importante.</span><span class="sxs-lookup"><span data-stu-id="7725b-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="7725b-133">Configurare il `ApplicationPartManager` prima di usarlo per configurare i servizi nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="7725b-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="7725b-134">Ad esempio, configurare il `ApplicationPartManager` prima di richiamare `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="7725b-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="7725b-135">Chiamare `Remove` sulla raccolta `ApplicationParts` per rimuovere una risorsa.</span><span class="sxs-lookup"><span data-stu-id="7725b-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="7725b-136">Il codice seguente usa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> per rimuovere `MyDependentLibrary` dall'app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="7725b-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="7725b-137">Il `ApplicationPartManager` include le parti per:</span><span class="sxs-lookup"><span data-stu-id="7725b-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="7725b-138">Assembly dell'app e assembly dipendenti.</span><span class="sxs-lookup"><span data-stu-id="7725b-138">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="7725b-139">`Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="7725b-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="7725b-140">`Microsoft.AspNetCore.Mvc.Razor`</span><span class="sxs-lookup"><span data-stu-id="7725b-140">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="7725b-141">Provider di funzionalità dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="7725b-141">Application feature providers</span></span>

<span data-ttu-id="7725b-142">I provider di funzionalità dell'applicazione esaminano le parti dell'applicazione e forniscono funzionalità per tali parti.</span><span class="sxs-lookup"><span data-stu-id="7725b-142">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="7725b-143">Sono disponibili provider di funzionalità predefiniti per le funzionalità di ASP.NET Core seguenti:</span><span class="sxs-lookup"><span data-stu-id="7725b-143">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="7725b-144">Controller</span><span class="sxs-lookup"><span data-stu-id="7725b-144">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="7725b-145">Helper tag</span><span class="sxs-lookup"><span data-stu-id="7725b-145">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="7725b-146">Componenti di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="7725b-146">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="7725b-147">I provider di funzionalità ereditano da <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, dove `T` è il tipo della funzionalità.</span><span class="sxs-lookup"><span data-stu-id="7725b-147">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="7725b-148">I provider di funzionalità possono essere implementati per uno qualsiasi dei tipi di funzionalità elencati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="7725b-148">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="7725b-149">L'ordine dei provider di funzionalità nella `ApplicationPartManager.FeatureProviders` può influisca sul comportamento in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7725b-149">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="7725b-150">I provider aggiunti successivamente possono rispondere alle azioni eseguite dai provider aggiunti precedentemente.</span><span class="sxs-lookup"><span data-stu-id="7725b-150">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="7725b-151">Funzionalità controller generico</span><span class="sxs-lookup"><span data-stu-id="7725b-151">Generic controller feature</span></span>

<span data-ttu-id="7725b-152">ASP.NET Core ignora i [controller generici](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="7725b-152">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="7725b-153">Un controller generico ha un parametro di tipo, ad esempio `MyController<T>`.</span><span class="sxs-lookup"><span data-stu-id="7725b-153">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="7725b-154">Nell'esempio seguente vengono aggiunte istanze del controller generico per un elenco di tipi specificato:</span><span class="sxs-lookup"><span data-stu-id="7725b-154">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="7725b-155">I tipi sono definiti in `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="7725b-155">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="7725b-156">Il provider di funzionalità viene aggiunto in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7725b-156">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="7725b-157">I nomi di controller generici usati per il routing hanno il formato *GenericController ' 1 [widget]* anziché *widget*.</span><span class="sxs-lookup"><span data-stu-id="7725b-157">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="7725b-158">L'attributo seguente modifica il nome in modo che corrisponda al tipo generico utilizzato dal controller:</span><span class="sxs-lookup"><span data-stu-id="7725b-158">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="7725b-159">La classe `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="7725b-159">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="7725b-160">Ad esempio, la richiesta di un URL di `https://localhost:5001/Sprocket` restituisce la risposta seguente:</span><span class="sxs-lookup"><span data-stu-id="7725b-160">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="7725b-161">Visualizzare funzionalità disponibili</span><span class="sxs-lookup"><span data-stu-id="7725b-161">Display available features</span></span>

<span data-ttu-id="7725b-162">Le funzionalità disponibili per un'app possono essere enumerate richiedendo un `ApplicationPartManager` tramite l' [inserimento di dipendenze](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="7725b-162">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="7725b-163">L' [esempio di download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) usa il codice precedente per visualizzare le funzionalità dell'app:</span><span class="sxs-lookup"><span data-stu-id="7725b-163">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7725b-164">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7725b-164">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7725b-165">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7725b-165">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7725b-166">Una *parte dell'applicazione* è un'astrazione sulle risorse di un'app.</span><span class="sxs-lookup"><span data-stu-id="7725b-166">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="7725b-167">Le parti dell'applicazione consentono ASP.NET Core di individuare controller, componenti di visualizzazione, helper tag, Razor Pages, origini di compilazione Razor e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="7725b-167">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="7725b-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) è una parte dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7725b-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="7725b-169">`AssemblyPart` incapsula un riferimento all'assembly ed espone i tipi e i riferimenti di compilazione.</span><span class="sxs-lookup"><span data-stu-id="7725b-169">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="7725b-170">I *provider di funzionalità* funzionano con le parti dell'applicazione per popolare le funzionalità di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7725b-170">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="7725b-171">Il caso d'uso principale per le parti dell'applicazione consiste nel configurare un'app per individuare (o evitare il caricamento) ASP.NET Core funzionalità da un assembly.</span><span class="sxs-lookup"><span data-stu-id="7725b-171">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="7725b-172">Ad esempio, potrebbe essere necessario condividere le funzionalità comuni tra più app.</span><span class="sxs-lookup"><span data-stu-id="7725b-172">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="7725b-173">Con le parti dell'applicazione è possibile condividere un assembly (DLL) contenente controller, visualizzazioni, Razor Pages, origini di compilazione Razor, helper tag e altro ancora con più app.</span><span class="sxs-lookup"><span data-stu-id="7725b-173">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="7725b-174">La condivisione di un assembly è preferibile alla duplicazione del codice in più progetti.</span><span class="sxs-lookup"><span data-stu-id="7725b-174">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="7725b-175">ASP.NET Core le app caricano le funzionalità da <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="7725b-175">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="7725b-176">La classe <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> rappresenta una parte dell'applicazione supportata da un assembly.</span><span class="sxs-lookup"><span data-stu-id="7725b-176">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="7725b-177">Carica ASP.NET Core funzionalità</span><span class="sxs-lookup"><span data-stu-id="7725b-177">Load ASP.NET Core features</span></span>

<span data-ttu-id="7725b-178">Usare le classi `ApplicationPart` e `AssemblyPart` per individuare e caricare funzionalità di ASP.NET Core (controller, componenti di visualizzazione e così via).</span><span class="sxs-lookup"><span data-stu-id="7725b-178">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="7725b-179">Il <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tiene traccia delle parti dell'applicazione e dei provider di funzionalità disponibili.</span><span class="sxs-lookup"><span data-stu-id="7725b-179">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="7725b-180">`ApplicationPartManager` è configurato in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7725b-180">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="7725b-181">Il codice seguente offre un approccio alternativo alla configurazione di `ApplicationPartManager` usando `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="7725b-181">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="7725b-182">I due esempi di codice precedenti caricano il `SharedController` da un assembly.</span><span class="sxs-lookup"><span data-stu-id="7725b-182">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="7725b-183">Il `SharedController` non è presente nel progetto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7725b-183">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="7725b-184">Vedere il download dell'esempio di [soluzione WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="7725b-184">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="7725b-185">Includi visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="7725b-185">Include views</span></span>

<span data-ttu-id="7725b-186">Per includere le visualizzazioni nell'assembly:</span><span class="sxs-lookup"><span data-stu-id="7725b-186">To include views in the assembly:</span></span>

* <span data-ttu-id="7725b-187">Aggiungere il markup seguente al file di progetto condiviso:</span><span class="sxs-lookup"><span data-stu-id="7725b-187">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="7725b-188">Aggiungere il <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> al <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span><span class="sxs-lookup"><span data-stu-id="7725b-188">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="7725b-189">Impedisci il caricamento delle risorse</span><span class="sxs-lookup"><span data-stu-id="7725b-189">Prevent loading resources</span></span>

<span data-ttu-id="7725b-190">Le parti dell'applicazione possono essere utilizzate per *evitare* il caricamento di risorse in un assembly o in un percorso specifico.</span><span class="sxs-lookup"><span data-stu-id="7725b-190">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="7725b-191">Aggiungere o rimuovere membri della raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> per nascondere o rendere disponibili risorse.</span><span class="sxs-lookup"><span data-stu-id="7725b-191">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="7725b-192">L'ordine delle voci nella raccolta `ApplicationParts` non è importante.</span><span class="sxs-lookup"><span data-stu-id="7725b-192">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="7725b-193">Configurare il `ApplicationPartManager` prima di usarlo per configurare i servizi nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="7725b-193">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="7725b-194">Ad esempio, configurare il `ApplicationPartManager` prima di richiamare `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="7725b-194">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="7725b-195">Chiamare `Remove` sulla raccolta `ApplicationParts` per rimuovere una risorsa.</span><span class="sxs-lookup"><span data-stu-id="7725b-195">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="7725b-196">Il codice seguente usa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> per rimuovere `MyDependentLibrary` dall'app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="7725b-196">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="7725b-197">Il `ApplicationPartManager` include le parti per:</span><span class="sxs-lookup"><span data-stu-id="7725b-197">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="7725b-198">Assembly dell'app e assembly dipendenti.</span><span class="sxs-lookup"><span data-stu-id="7725b-198">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="7725b-199">`Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="7725b-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="7725b-200">`Microsoft.AspNetCore.Mvc.Razor`</span><span class="sxs-lookup"><span data-stu-id="7725b-200">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="7725b-201">Provider di funzionalità dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="7725b-201">Application feature providers</span></span>

<span data-ttu-id="7725b-202">I provider di funzionalità dell'applicazione esaminano le parti dell'applicazione e forniscono funzionalità per tali parti.</span><span class="sxs-lookup"><span data-stu-id="7725b-202">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="7725b-203">Sono disponibili provider di funzionalità predefiniti per le funzionalità di ASP.NET Core seguenti:</span><span class="sxs-lookup"><span data-stu-id="7725b-203">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="7725b-204">Controller</span><span class="sxs-lookup"><span data-stu-id="7725b-204">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="7725b-205">Helper tag</span><span class="sxs-lookup"><span data-stu-id="7725b-205">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="7725b-206">Componenti di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="7725b-206">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="7725b-207">I provider di funzionalità ereditano da <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, dove `T` è il tipo della funzionalità.</span><span class="sxs-lookup"><span data-stu-id="7725b-207">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="7725b-208">I provider di funzionalità possono essere implementati per uno qualsiasi dei tipi di funzionalità elencati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="7725b-208">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="7725b-209">L'ordine dei provider di funzionalità nella `ApplicationPartManager.FeatureProviders` può influisca sul comportamento in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7725b-209">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="7725b-210">I provider aggiunti successivamente possono rispondere alle azioni eseguite dai provider aggiunti precedentemente.</span><span class="sxs-lookup"><span data-stu-id="7725b-210">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="7725b-211">Funzionalità controller generico</span><span class="sxs-lookup"><span data-stu-id="7725b-211">Generic controller feature</span></span>

<span data-ttu-id="7725b-212">ASP.NET Core ignora i [controller generici](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="7725b-212">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="7725b-213">Un controller generico ha un parametro di tipo, ad esempio `MyController<T>`.</span><span class="sxs-lookup"><span data-stu-id="7725b-213">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="7725b-214">Nell'esempio seguente vengono aggiunte istanze del controller generico per un elenco di tipi specificato:</span><span class="sxs-lookup"><span data-stu-id="7725b-214">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="7725b-215">I tipi sono definiti in `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="7725b-215">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="7725b-216">Il provider di funzionalità viene aggiunto in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7725b-216">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="7725b-217">I nomi di controller generici usati per il routing hanno il formato *GenericController ' 1 [widget]* anziché *widget*.</span><span class="sxs-lookup"><span data-stu-id="7725b-217">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="7725b-218">L'attributo seguente modifica il nome in modo che corrisponda al tipo generico utilizzato dal controller:</span><span class="sxs-lookup"><span data-stu-id="7725b-218">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="7725b-219">La classe `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="7725b-219">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="7725b-220">Ad esempio, la richiesta di un URL di `https://localhost:5001/Sprocket` restituisce la risposta seguente:</span><span class="sxs-lookup"><span data-stu-id="7725b-220">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="7725b-221">Visualizzare funzionalità disponibili</span><span class="sxs-lookup"><span data-stu-id="7725b-221">Display available features</span></span>

<span data-ttu-id="7725b-222">Le funzionalità disponibili per un'app possono essere enumerate richiedendo un `ApplicationPartManager` tramite l' [inserimento di dipendenze](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="7725b-222">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="7725b-223">L' [esempio di download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) usa il codice precedente per visualizzare le funzionalità dell'app:</span><span class="sxs-lookup"><span data-stu-id="7725b-223">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end
