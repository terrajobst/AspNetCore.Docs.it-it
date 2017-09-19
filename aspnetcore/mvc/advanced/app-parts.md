---
title: Parti dell'applicazione in ASP.NET Core
author: ardalis
description: "Informazioni sull'utilizzo di parti dell'applicazione, che sono abstrations sulle risorse di un'app, configurare l'app per individuare o evitare di caricare le funzionalità da un assembly."
keywords: ASP.NET Core, parte dell'applicazione, da parte di app
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899963613a98
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 77d3a58d58493bf1b0b760ab9037d2778ba23441
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="6b7d3-104">Parti dell'applicazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b7d3-104">Application Parts in ASP.NET Core</span></span>

[<span data-ttu-id="6b7d3-105">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="6b7d3-105">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample)

<span data-ttu-id="6b7d3-106">Un *parte dell'applicazione* è un'astrazione sulle risorse di un'applicazione, da cui MVC funzionalità quali controller, i componenti di visualizzazione, o gli helper di tag possono essere individuati.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-106">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="6b7d3-107">Un esempio di una parte dell'applicazione è un AssemblyPart, che incapsula un riferimento all'assembly ed espone i tipi e i riferimenti di compilazione.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-107">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="6b7d3-108">*Funzionalità provider* funzionano con parti dell'applicazione per popolare le funzionalità di un'applicazione ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-108">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="6b7d3-109">Nel caso di utilizzo principale di parti dell'applicazione è consentire di configurare l'app per individuare (o evitare di caricamento) funzionalità MVC da un assembly.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-109">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="6b7d3-110">Introduzione a parti dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="6b7d3-110">Introducing Application Parts</span></span>

<span data-ttu-id="6b7d3-111">Le applicazioni MVC caricare le funzionalità da [parti dell'applicazione](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="6b7d3-111">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="6b7d3-112">In particolare, il [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) classe rappresenta una parte di applicazione che è supportata da un assembly.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-112">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that is backed by an assembly.</span></span> <span data-ttu-id="6b7d3-113">È possibile utilizzare queste classi per individuare e caricare le funzionalità di MVC, ad esempio controller, Visualizza i componenti, gli helper di tag e origini di compilazione razor.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-113">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="6b7d3-114">Il [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) responsabile della gestione di parti dell'applicazione e un provider di funzionalità disponibili per l'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-114">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="6b7d3-115">È possibile interagire con il `ApplicationPartManager` in `Startup` quando si configura MVC:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-115">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

<span data-ttu-id="6b7d3-116">Per impostazione predefinita, MVC verrà ricerca l'albero delle dipendenze e individuare i controller (anche in altri assembly).</span><span class="sxs-lookup"><span data-stu-id="6b7d3-116">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="6b7d3-117">Per caricare un assembly arbitrario (ad esempio, da un plug-in che non fa riferimento in fase di compilazione), è possibile utilizzare una parte dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-117">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="6b7d3-118">È possibile utilizzare parti dell'applicazione per *evitare* cercando controller in un particolare assembly o un percorso.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-118">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="6b7d3-119">È possibile controllare quali parti o gli assembly sono disponibili per l'applicazione modificando il `ApplicationParts` insieme il `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-119">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="6b7d3-120">L'ordine delle voci di `ApplicationParts` raccolta non è importante.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-120">The order of the entries in the `ApplicationParts` collection is not important.</span></span> <span data-ttu-id="6b7d3-121">È importante configurare completamente il `ApplicationPartManager` prima di utilizzarlo per configurare i servizi nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-121">It is important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="6b7d3-122">Ad esempio, è necessario configurare completamente il `ApplicationPartManager` prima di richiamare `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-122">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="6b7d3-123">In caso contrario, significa che i controller di parti dell'applicazione aggiunti dopo che chiamata al metodo non sarà interessata (non viene registrato come servizi) che potrebbe essere bevavior non corretto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-123">Failing to do so, will mean that controllers in application parts added after that method call will not be affected (will not get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="6b7d3-124">Se si dispone di un assembly che contiene i controller non si desidera utilizzare, rimuoverlo dal `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-124">If you have an assembly that contains controllers you do not want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="6b7d3-125">Oltre agli assembly del progetto e i relativi assembly dipendenti, la `ApplicationPartManager` includerà parti per `Microsoft.AspNetCore.Mvc.TagHelpers` e `Microsoft.AspNetCore.Mvc.Razor` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-125">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="6b7d3-126">Provider di funzionalità dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="6b7d3-126">Application Feature Providers</span></span>

<span data-ttu-id="6b7d3-127">Provider di funzionalità dell'applicazione esaminare parti dell'applicazione e forniscono funzionalità per tali parti.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-127">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="6b7d3-128">Sono disponibili provider di funzionalità predefinite per le seguenti funzionalità MVC:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-128">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="6b7d3-129">Controller</span><span class="sxs-lookup"><span data-stu-id="6b7d3-129">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="6b7d3-130">Riferimento ai metadati</span><span class="sxs-lookup"><span data-stu-id="6b7d3-130">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="6b7d3-131">Helper tag</span><span class="sxs-lookup"><span data-stu-id="6b7d3-131">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="6b7d3-132">Visualizza i componenti</span><span class="sxs-lookup"><span data-stu-id="6b7d3-132">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="6b7d3-133">Provider di funzionalità ereditare `IApplicationFeatureProvider<T>`, dove `T` è il tipo della funzionalità.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-133">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="6b7d3-134">È possibile implementare la propria funzionalità i provider per uno qualsiasi dei tipi di funzionalità del MVC elencati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-134">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="6b7d3-135">L'ordine dei provider di funzionalità di `ApplicationPartManager.FeatureProviders` raccolta può essere importante, poiché i provider successive possano rispondere alle azioni eseguite dal provider precedenti.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-135">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="6b7d3-136">Esempio: Funzionalità controller generico</span><span class="sxs-lookup"><span data-stu-id="6b7d3-136">Sample: Generic controller feature</span></span>

<span data-ttu-id="6b7d3-137">Per impostazione predefinita, componenti di base di ASP.NET MVC Ignora controller generico (ad esempio, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="6b7d3-137">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="6b7d3-138">In questo esempio utilizza un provider di funzionalità di controller che viene eseguita quando il provider predefinito e aggiunge istanze controller generico per un elenco specificato di tipi (definito in `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="6b7d3-138">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="6b7d3-139">I tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-139">The entity types:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="6b7d3-140">Il provider di funzionalità viene aggiunta in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-140">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="6b7d3-141">Per impostazione predefinita, i nomi di controller generico utilizzati per il routing sarà nel formato *GenericController'1 [Widget]* anziché *Widget*.</span><span class="sxs-lookup"><span data-stu-id="6b7d3-141">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="6b7d3-142">L'attributo seguente viene utilizzato per modificare il nome in modo che corrisponda al tipo generico utilizzato dal controller:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-142">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="6b7d3-143">La `GenericController` classe:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-143">The `GenericController` class:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="6b7d3-144">Di conseguenza, quando viene richiesta una route corrispondente:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-144">The result, when a matching route is requested:</span></span>

![Esempio di output dall'app di esempio legge, 'Hello da un controller Sproket generico'.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="6b7d3-146">Esempio: Le funzionalità disponibili visualizzazione</span><span class="sxs-lookup"><span data-stu-id="6b7d3-146">Sample: Display available features</span></span>

<span data-ttu-id="6b7d3-147">È possibile scorrere le funzionalità popolate disponibili per l'app richiedendo un `ApplicationPartManager` tramite [inserimento di dipendenze](../../fundamentals/dependency-injection.md) e utilizzarlo per popolare le istanze delle funzionalità appropriate:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-147">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="6b7d3-148">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="6b7d3-148">Example output:</span></span>

![Output di esempio di app di esempio](app-parts/_static/available-features.png)
