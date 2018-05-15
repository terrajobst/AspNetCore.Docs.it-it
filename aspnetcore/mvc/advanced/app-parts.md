---
title: Parti dell'applicazione in ASP.NET Core
author: ardalis
description: Informazioni sull'uso delle parti dell'applicazione, che sono astrazioni relative alle risorse di un'app, per rilevare o evitare di caricare funzionalità da un assembly.
manager: wpickett
ms.author: riande
ms.date: 01/04/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 8f7aeadc7a1218bf203575add8c82c95faf137b4
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="c236f-103">Parti dell'applicazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c236f-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="c236f-104">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c236f-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c236f-105">Una *parte dell'applicazione* è un'astrazione relativa alle risorse di un'applicazione che consente il rilevamento di funzionalità MVC quali controller, componenti della visualizzazione o helper tag.</span><span class="sxs-lookup"><span data-stu-id="c236f-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="c236f-106">Un esempio di parte dell'applicazione è AssemblyPart, che incapsula un riferimento all'assembly ed espone tipi e riferimenti di compilazione.</span><span class="sxs-lookup"><span data-stu-id="c236f-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="c236f-107">I *provider di funzionalità* vengono eseguiti con le parti dell'applicazione per popolare le funzionalità di un'app ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c236f-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="c236f-108">Il caso d'uso principale delle parti dell'applicazione è la configurazione dell'app in modo che rilevi (o eviti di caricare) funzionalità MVC da un assembly.</span><span class="sxs-lookup"><span data-stu-id="c236f-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="c236f-109">Introduzione alle parti dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="c236f-109">Introducing Application Parts</span></span>

<span data-ttu-id="c236f-110">Le app MVC caricano le loro funzionalità dalle [parti dell'applicazione](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="c236f-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="c236f-111">In particolare la classe [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) rappresenta una parte dell'applicazione supportata da un assembly.</span><span class="sxs-lookup"><span data-stu-id="c236f-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="c236f-112">Queste classi consentono di trovare e caricare funzionalità di MVC quali controller, componenti di visualizzazione, helper tag e origini di compilazione Razor.</span><span class="sxs-lookup"><span data-stu-id="c236f-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="c236f-113">[ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) gestisce il rilevamento delle parti dell'applicazione e dei provider di funzionalità disponibili per l'app MVC.</span><span class="sxs-lookup"><span data-stu-id="c236f-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="c236f-114">È possibile interagire con `ApplicationPartManager` in `Startup` quando si configura MVC:</span><span class="sxs-lookup"><span data-stu-id="c236f-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

<span data-ttu-id="c236f-115">Per impostazione predefinita MVC esegue la ricerca nell'albero dipendenze e rileva i controller (anche in altri assembly).</span><span class="sxs-lookup"><span data-stu-id="c236f-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="c236f-116">Per caricare un assembly arbitrario (ad esempio da un plug-in a cui non si fa riferimento in fase di compilazione), è possibile usare una parte dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c236f-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="c236f-117">Le parti dell'applicazione possono essere usate per *evitare* la ricerca di controller in un assembly o un percorso specifico.</span><span class="sxs-lookup"><span data-stu-id="c236f-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="c236f-118">È possibile controllare quali parti o assembly sono disponibili per l'app modificando la raccolta `ApplicationParts` di `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="c236f-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="c236f-119">L'ordine delle voci nella raccolta `ApplicationParts` non è importante.</span><span class="sxs-lookup"><span data-stu-id="c236f-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="c236f-120">È invece importante configurare completamente `ApplicationPartManager` prima di usarlo per configurare i servizi nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="c236f-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="c236f-121">Ad esempio, è necessario configurare completamente `ApplicationPartManager` prima di chiamare `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="c236f-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="c236f-122">In caso contrario, i controller inclusi in parti dell'applicazione aggiunte dopo questa chiamata al metodo vengono ignorati (non vengono registrati come servizi) e questo può determinare errori di funzionamento dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c236f-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="c236f-123">Se è presente un assembly contenente controller che non si vuole usare, rimuoverlo da `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="c236f-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="c236f-124">Oltre all'assembly del progetto e agli assembly dipendenti, per impostazione predefinita `ApplicationPartManager` include parti per `Microsoft.AspNetCore.Mvc.TagHelpers` e `Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="c236f-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="c236f-125">Provider di funzionalità dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="c236f-125">Application Feature Providers</span></span>

<span data-ttu-id="c236f-126">I provider di funzionalità dell'applicazione esaminano le parti dell'applicazione e offrono funzionalità per tali parti.</span><span class="sxs-lookup"><span data-stu-id="c236f-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="c236f-127">Sono disponibili provider di funzionalità predefiniti per le seguenti funzionalità MVC:</span><span class="sxs-lookup"><span data-stu-id="c236f-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="c236f-128">Controller</span><span class="sxs-lookup"><span data-stu-id="c236f-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="c236f-129">Riferimento ai metadati</span><span class="sxs-lookup"><span data-stu-id="c236f-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="c236f-130">Helper tag</span><span class="sxs-lookup"><span data-stu-id="c236f-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="c236f-131">Componenti di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="c236f-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="c236f-132">I provider di funzionalità ereditano da `IApplicationFeatureProvider<T>`, dove `T` è il tipo della funzionalità.</span><span class="sxs-lookup"><span data-stu-id="c236f-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="c236f-133">È possibile implementare provider di funzionalità personalizzati per i tipi di funzionalità MVC elencati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c236f-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="c236f-134">L'ordine dei provider di funzionalità nella raccolta `ApplicationPartManager.FeatureProviders` può essere importante, perché i provider in un determinato punto della raccolta possono rispondere alle azioni eseguite dai provider che li precedono.</span><span class="sxs-lookup"><span data-stu-id="c236f-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="c236f-135">Esempio: Funzionalità controller generico</span><span class="sxs-lookup"><span data-stu-id="c236f-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="c236f-136">Per impostazione predefinita, ASP.NET Core MVC ignora i controller generici (ad esempio `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="c236f-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="c236f-137">Questo esempio usa come provider di funzionalità un controller che viene eseguito dopo il provider predefinito e aggiunge istanze controller generico per un elenco di tipi specificato (definito in `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="c236f-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="c236f-138">Tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="c236f-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="c236f-139">Il provider di funzionalità viene aggiunto in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c236f-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="c236f-140">Per impostazione predefinita i nomi di controller generico usati per il routing hanno il formato *GenericController\`1[Widget]* anziché il formato *Widget*.</span><span class="sxs-lookup"><span data-stu-id="c236f-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="c236f-141">L'attributo seguente viene usato per modificare il nome, in modo che corrisponda al tipo generico usato dal controller:</span><span class="sxs-lookup"><span data-stu-id="c236f-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="c236f-142">La classe `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="c236f-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="c236f-143">Risultato quando viene richiesta una route corrispondente:</span><span class="sxs-lookup"><span data-stu-id="c236f-143">The result, when a matching route is requested:</span></span>

![Esempio di output dell'app: "Hello from a generic Sprocket controller".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="c236f-145">Esempio: Visualizzare funzionalità disponibili</span><span class="sxs-lookup"><span data-stu-id="c236f-145">Sample: Display available features</span></span>

<span data-ttu-id="c236f-146">È possibile eseguire l'iterazione tra le funzionalità popolate disponibili per l'app richiedendo `ApplicationPartManager` mediante l'[inserimento di dipendenze](../../fundamentals/dependency-injection.md) e usando tale elemento per popolare istanze delle funzionalità appropriate:</span><span class="sxs-lookup"><span data-stu-id="c236f-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="c236f-147">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="c236f-147">Example output:</span></span>

![Output di esempio dall'app di esempio](app-parts/_static/available-features.png)
