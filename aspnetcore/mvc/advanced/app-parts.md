---
title: Parti dell'applicazione in ASP.NET Core
author: ardalis
description: "Informazioni sull'utilizzo di parti dell'applicazione, che sono abstrations sulle risorse di un'app, configurare l'app per individuare o evitare di caricare le funzionalità da un assembly."
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 702d7773374f331b25489060b18f752186d7acea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="14759-103">Parti dell'applicazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14759-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="14759-104">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="14759-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="14759-105">Un *parte dell'applicazione* è un'astrazione sulle risorse di un'applicazione, da cui MVC funzionalità quali controller, i componenti di visualizzazione, o gli helper di tag possono essere individuati.</span><span class="sxs-lookup"><span data-stu-id="14759-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="14759-106">Un esempio di una parte dell'applicazione è un AssemblyPart, che incapsula un riferimento all'assembly ed espone i tipi e i riferimenti di compilazione.</span><span class="sxs-lookup"><span data-stu-id="14759-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="14759-107">*Funzionalità provider* funzionano con parti dell'applicazione per popolare le funzionalità di un'applicazione ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="14759-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="14759-108">Nel caso di utilizzo principale di parti dell'applicazione è consentire di configurare l'app per individuare (o evitare di caricamento) funzionalità MVC da un assembly.</span><span class="sxs-lookup"><span data-stu-id="14759-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="14759-109">Introduzione a parti dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="14759-109">Introducing Application Parts</span></span>

<span data-ttu-id="14759-110">Le applicazioni MVC caricare le funzionalità da [parti dell'applicazione](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="14759-110">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="14759-111">In particolare, il [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) classe rappresenta una parte di applicazione che è supportata da un assembly.</span><span class="sxs-lookup"><span data-stu-id="14759-111">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="14759-112">È possibile utilizzare queste classi per individuare e caricare le funzionalità di MVC, ad esempio controller, Visualizza i componenti, gli helper di tag e origini di compilazione razor.</span><span class="sxs-lookup"><span data-stu-id="14759-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="14759-113">Il [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) responsabile della gestione di parti dell'applicazione e un provider di funzionalità disponibili per l'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="14759-113">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="14759-114">È possibile interagire con il `ApplicationPartManager` in `Startup` quando si configura MVC:</span><span class="sxs-lookup"><span data-stu-id="14759-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

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

<span data-ttu-id="14759-115">Per impostazione predefinita, MVC verrà ricerca l'albero delle dipendenze e individuare i controller (anche in altri assembly).</span><span class="sxs-lookup"><span data-stu-id="14759-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="14759-116">Per caricare un assembly arbitrario (ad esempio, da un plug-in che non fa riferimento in fase di compilazione), è possibile utilizzare una parte dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="14759-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="14759-117">È possibile utilizzare parti dell'applicazione per *evitare* cercando controller in un particolare assembly o un percorso.</span><span class="sxs-lookup"><span data-stu-id="14759-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="14759-118">È possibile controllare quali parti o gli assembly sono disponibili per l'applicazione modificando il `ApplicationParts` insieme il `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="14759-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="14759-119">L'ordine delle voci di `ApplicationParts` raccolta non è importante.</span><span class="sxs-lookup"><span data-stu-id="14759-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="14759-120">È importante configurare completamente il `ApplicationPartManager` prima di utilizzarlo per configurare i servizi nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="14759-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="14759-121">Ad esempio, è necessario configurare completamente il `ApplicationPartManager` prima di richiamare `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="14759-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="14759-122">In caso contrario, significa che i controller di parti dell'applicazione aggiunti dopo che non saranno interessata chiamata al metodo (non viene registrato come servizi) che potrebbe essere bevavior non corretto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="14759-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="14759-123">Se si dispone di un assembly che contiene i controller che non si desidera utilizzare, rimuoverlo dal `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="14759-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

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

<span data-ttu-id="14759-124">Oltre agli assembly del progetto e i relativi assembly dipendenti, la `ApplicationPartManager` includerà parti per `Microsoft.AspNetCore.Mvc.TagHelpers` e `Microsoft.AspNetCore.Mvc.Razor` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="14759-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="14759-125">Provider di funzionalità dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="14759-125">Application Feature Providers</span></span>

<span data-ttu-id="14759-126">Provider di funzionalità dell'applicazione esaminare parti dell'applicazione e forniscono funzionalità per tali parti.</span><span class="sxs-lookup"><span data-stu-id="14759-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="14759-127">Sono disponibili provider di funzionalità predefinite per le seguenti funzionalità MVC:</span><span class="sxs-lookup"><span data-stu-id="14759-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="14759-128">Controller</span><span class="sxs-lookup"><span data-stu-id="14759-128">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="14759-129">Riferimento ai metadati</span><span class="sxs-lookup"><span data-stu-id="14759-129">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="14759-130">Helper tag</span><span class="sxs-lookup"><span data-stu-id="14759-130">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="14759-131">Visualizza i componenti</span><span class="sxs-lookup"><span data-stu-id="14759-131">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="14759-132">Provider di funzionalità ereditare `IApplicationFeatureProvider<T>`, dove `T` è il tipo della funzionalità.</span><span class="sxs-lookup"><span data-stu-id="14759-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="14759-133">È possibile implementare la propria funzionalità i provider per uno qualsiasi dei tipi di funzionalità del MVC elencati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="14759-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="14759-134">L'ordine dei provider di funzionalità di `ApplicationPartManager.FeatureProviders` raccolta può essere importante, poiché i provider successive possano rispondere alle azioni eseguite dal provider precedenti.</span><span class="sxs-lookup"><span data-stu-id="14759-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="14759-135">Esempio: Funzionalità controller generico</span><span class="sxs-lookup"><span data-stu-id="14759-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="14759-136">Per impostazione predefinita, componenti di base di ASP.NET MVC Ignora controller generico (ad esempio, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="14759-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="14759-137">In questo esempio utilizza un provider di funzionalità di controller che viene eseguita quando il provider predefinito e aggiunge istanze controller generico per un elenco specificato di tipi (definito in `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="14759-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="14759-138">I tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="14759-138">The entity types:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="14759-139">Il provider di funzionalità viene aggiunta in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="14759-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="14759-140">Per impostazione predefinita, i nomi di controller generico utilizzati per il routing sarà nel formato *GenericController'1 [Widget]* anziché *Widget*.</span><span class="sxs-lookup"><span data-stu-id="14759-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="14759-141">L'attributo seguente viene utilizzato per modificare il nome in modo che corrisponda al tipo generico utilizzato dal controller:</span><span class="sxs-lookup"><span data-stu-id="14759-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="14759-142">La `GenericController` classe:</span><span class="sxs-lookup"><span data-stu-id="14759-142">The `GenericController` class:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="14759-143">Di conseguenza, quando viene richiesta una route corrispondente:</span><span class="sxs-lookup"><span data-stu-id="14759-143">The result, when a matching route is requested:</span></span>

![Esempio di output dall'app di esempio legge, 'Hello da un controller Sproket generico'.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="14759-145">Esempio: Le funzionalità disponibili visualizzazione</span><span class="sxs-lookup"><span data-stu-id="14759-145">Sample: Display available features</span></span>

<span data-ttu-id="14759-146">È possibile scorrere le funzionalità popolate disponibili per l'app richiedendo un `ApplicationPartManager` tramite [inserimento di dipendenze](../../fundamentals/dependency-injection.md) e utilizzarlo per popolare le istanze delle funzionalità appropriate:</span><span class="sxs-lookup"><span data-stu-id="14759-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="14759-147">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="14759-147">Example output:</span></span>

![Output di esempio di app di esempio](app-parts/_static/available-features.png)
