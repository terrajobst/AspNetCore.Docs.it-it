---
title: Parti dell'applicazione in ASP.NET Core
author: rick-anderson
description: Condividi controller, Visualizza, Razor Pages e altro con le parti dell'applicazione in ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: ad0372f25377115e6fc7c8ea42db75de56b3e6d2
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187004"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="beb85-103">Condividi controller, visualizzazioni, Razor Pages e altro con le parti dell'applicazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="beb85-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>
=======

<!-- DO NOT MAKE CHANGES BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12376 Merges -->

<span data-ttu-id="beb85-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="beb85-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="beb85-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="beb85-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="beb85-106">Una *parte dell'applicazione* è un'astrazione sulle risorse di un'app.</span><span class="sxs-lookup"><span data-stu-id="beb85-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="beb85-107">Le parti dell'applicazione consentono ASP.NET Core di individuare controller, componenti di visualizzazione, helper tag, Razor Pages, origini di compilazione Razor e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="beb85-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="beb85-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) è una parte dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="beb85-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="beb85-109">`AssemblyPart`Incapsula un riferimento all'assembly ed espone i tipi e i riferimenti di compilazione.</span><span class="sxs-lookup"><span data-stu-id="beb85-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="beb85-110">I *provider di funzionalità* funzionano con le parti dell'applicazione per popolare le funzionalità di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="beb85-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="beb85-111">Il caso d'uso principale per le parti dell'applicazione consiste nel configurare un'app per individuare (o evitare il caricamento) ASP.NET Core funzionalità da un assembly.</span><span class="sxs-lookup"><span data-stu-id="beb85-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="beb85-112">Ad esempio, potrebbe essere necessario condividere le funzionalità comuni tra più app.</span><span class="sxs-lookup"><span data-stu-id="beb85-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="beb85-113">Con le parti dell'applicazione è possibile condividere un assembly (DLL) contenente controller, visualizzazioni, Razor Pages, origini di compilazione Razor, helper tag e altro ancora con più app.</span><span class="sxs-lookup"><span data-stu-id="beb85-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="beb85-114">La condivisione di un assembly è preferibile alla duplicazione del codice in più progetti.</span><span class="sxs-lookup"><span data-stu-id="beb85-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="beb85-115">ASP.NET Core le app caricano <xref:System.Web.WebPages.ApplicationPart>le funzionalità da.</span><span class="sxs-lookup"><span data-stu-id="beb85-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="beb85-116">La <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> classe rappresenta una parte dell'applicazione supportata da un assembly.</span><span class="sxs-lookup"><span data-stu-id="beb85-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="beb85-117">Carica ASP.NET Core funzionalità</span><span class="sxs-lookup"><span data-stu-id="beb85-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="beb85-118">Usare le `ApplicationPart` classi `AssemblyPart` e per individuare e caricare ASP.NET Core funzionalità (controller, componenti di visualizzazione e così via).</span><span class="sxs-lookup"><span data-stu-id="beb85-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="beb85-119">Consente di tenere traccia delle parti dell'applicazione e dei provider di funzionalità disponibili. <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager></span><span class="sxs-lookup"><span data-stu-id="beb85-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="beb85-120">`ApplicationPartManager`è configurato in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="beb85-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="beb85-121">Il codice seguente offre un approccio alternativo alla configurazione `ApplicationPartManager` di `AssemblyPart`utilizzando:</span><span class="sxs-lookup"><span data-stu-id="beb85-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="beb85-122">I due esempi `SharedController` di codice precedenti caricano da un assembly.</span><span class="sxs-lookup"><span data-stu-id="beb85-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="beb85-123">L' `SharedController` oggetto non è presente nel progetto di applicazione.</span><span class="sxs-lookup"><span data-stu-id="beb85-123">The `SharedController` is not in the applications project.</span></span> <span data-ttu-id="beb85-124">Vedere il download dell'esempio di [soluzione WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="beb85-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="beb85-125">Includi visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="beb85-125">Include views</span></span>

<span data-ttu-id="beb85-126">Per includere le visualizzazioni nell'assembly:</span><span class="sxs-lookup"><span data-stu-id="beb85-126">To include views in the assembly:</span></span>

* <span data-ttu-id="beb85-127">Aggiungere il markup seguente al file di progetto condiviso:</span><span class="sxs-lookup"><span data-stu-id="beb85-127">Add the following markup to the shared project file:</span></span>

  ```csproj
    <ItemGroup>
      <EmbeddedResource Include = "Views\**\*.cshtml" />
    </ ItemGroup >
  ```

* <span data-ttu-id="beb85-128"><xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> Aggiungere<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>alla:</span><span class="sxs-lookup"><span data-stu-id="beb85-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="beb85-129">Impedisci il caricamento delle risorse</span><span class="sxs-lookup"><span data-stu-id="beb85-129">Prevent loading resources</span></span>

<span data-ttu-id="beb85-130">Le parti dell'applicazione possono essere utilizzate per *evitare* il caricamento di risorse in un assembly o in un percorso specifico.</span><span class="sxs-lookup"><span data-stu-id="beb85-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="beb85-131">Aggiungere o rimuovere membri della <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> raccolta per nascondere o rendere disponibili risorse.</span><span class="sxs-lookup"><span data-stu-id="beb85-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="beb85-132">L'ordine delle voci nella raccolta `ApplicationParts` non è importante.</span><span class="sxs-lookup"><span data-stu-id="beb85-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="beb85-133">Configurare prima `ApplicationPartManager` di usarlo per configurare i servizi nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="beb85-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="beb85-134">Configurare `ApplicationPartManager` , ad esempio, prima di `AddControllersAsServices`richiamare.</span><span class="sxs-lookup"><span data-stu-id="beb85-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="beb85-135">`Remove` Chiamare`ApplicationParts` sulla raccolta per rimuovere una risorsa.</span><span class="sxs-lookup"><span data-stu-id="beb85-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="beb85-136">Il codice seguente usa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> per rimuovere `MyDependentLibrary` dall'app:[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="beb85-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="beb85-137">Include `ApplicationPartManager` le parti per:</span><span class="sxs-lookup"><span data-stu-id="beb85-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="beb85-138">Assembly di app e assembly dipendenti.</span><span class="sxs-lookup"><span data-stu-id="beb85-138">The apps assembly and dependent assemblies.</span></span>
* `Microsoft.AspNetCore.Mvc.TagHelpers`
* <span data-ttu-id="beb85-139">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="beb85-139">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="beb85-140">Provider di funzionalità dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="beb85-140">Application feature providers</span></span>

<span data-ttu-id="beb85-141">I provider di funzionalità dell'applicazione esaminano le parti dell'applicazione e forniscono funzionalità per tali parti.</span><span class="sxs-lookup"><span data-stu-id="beb85-141">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="beb85-142">Sono disponibili provider di funzionalità predefiniti per le funzionalità di ASP.NET Core seguenti:</span><span class="sxs-lookup"><span data-stu-id="beb85-142">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="beb85-143">Controller</span><span class="sxs-lookup"><span data-stu-id="beb85-143">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="beb85-144">Helper tag</span><span class="sxs-lookup"><span data-stu-id="beb85-144">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="beb85-145">Componenti di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="beb85-145">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="beb85-146">I provider di funzionalità ereditano da <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, dove `T` è il tipo della funzionalità.</span><span class="sxs-lookup"><span data-stu-id="beb85-146">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="beb85-147">I provider di funzionalità possono essere implementati per uno qualsiasi dei tipi di funzionalità elencati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="beb85-147">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="beb85-148">L'ordine dei provider di funzionalità in `ApplicationPartManager.FeatureProviders` può influisca sul comportamento in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="beb85-148">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="beb85-149">I provider aggiunti successivamente possono rispondere alle azioni eseguite dai provider aggiunti precedentemente.</span><span class="sxs-lookup"><span data-stu-id="beb85-149">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="beb85-150">Funzionalità controller generico</span><span class="sxs-lookup"><span data-stu-id="beb85-150">Generic controller feature</span></span>

<span data-ttu-id="beb85-151">ASP.NET Core ignora i [controller generici](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="beb85-151">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="beb85-152">Un controller generico ha un parametro di tipo (ad esempio `MyController<T>`,).</span><span class="sxs-lookup"><span data-stu-id="beb85-152">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="beb85-153">Nell'esempio seguente vengono aggiunte istanze del controller generico per un elenco di tipi specificato.</span><span class="sxs-lookup"><span data-stu-id="beb85-153">The following sample adds generic controller instances for a specified list of types.</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="beb85-154">I tipi sono definiti in `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="beb85-154">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="beb85-155">Il provider di funzionalità viene aggiunto in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="beb85-155">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="beb85-156">I nomi di controller generici usati per il routing hanno il formato *GenericController ' 1 [widget]* anziché *widget*.</span><span class="sxs-lookup"><span data-stu-id="beb85-156">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="beb85-157">L'attributo seguente modifica il nome in modo che corrisponda al tipo generico utilizzato dal controller:</span><span class="sxs-lookup"><span data-stu-id="beb85-157">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="beb85-158">La classe `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="beb85-158">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

### <a name="display-available-features"></a><span data-ttu-id="beb85-159">Visualizzare funzionalità disponibili</span><span class="sxs-lookup"><span data-stu-id="beb85-159">Display available features</span></span>

<span data-ttu-id="beb85-160">Le funzionalità disponibili per un'app possono essere enumerate mediante la richiesta di un `ApplicationPartManager` [inserimento delle dipendenze](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="beb85-160">The features available to an app can be enumerated by by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="beb85-161">L' [esempio di download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) usa il codice precedente per visualizzare le funzionalità dell'app.</span><span class="sxs-lookup"><span data-stu-id="beb85-161">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features.</span></span>