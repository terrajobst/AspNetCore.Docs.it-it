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
ms.openlocfilehash: 12c34b7a9521835533998c5609870bc712a6d48c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="application-parts-in-aspnet-core"></a>Parti dell'applicazione in ASP.NET Core

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Un *parte dell'applicazione* è un'astrazione sulle risorse di un'applicazione, da cui MVC funzionalità quali controller, i componenti di visualizzazione, o gli helper di tag possono essere individuati. Un esempio di una parte dell'applicazione è un AssemblyPart, che incapsula un riferimento all'assembly ed espone i tipi e i riferimenti di compilazione. *Funzionalità provider* funzionano con parti dell'applicazione per popolare le funzionalità di un'applicazione ASP.NET MVC di base. Nel caso di utilizzo principale di parti dell'applicazione è consentire di configurare l'app per individuare (o evitare di caricamento) funzionalità MVC da un assembly.

## <a name="introducing-application-parts"></a>Introduzione a parti dell'applicazione

Le applicazioni MVC caricare le funzionalità da [parti dell'applicazione](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). In particolare, il [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) classe rappresenta una parte di applicazione che è supportata da un assembly. È possibile utilizzare queste classi per individuare e caricare le funzionalità di MVC, ad esempio controller, Visualizza i componenti, gli helper di tag e origini di compilazione razor. Il [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) responsabile della gestione di parti dell'applicazione e un provider di funzionalità disponibili per l'applicazione MVC. È possibile interagire con il `ApplicationPartManager` in `Startup` quando si configura MVC:

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

Per impostazione predefinita, MVC verrà ricerca l'albero delle dipendenze e individuare i controller (anche in altri assembly). Per caricare un assembly arbitrario (ad esempio, da un plug-in che non fa riferimento in fase di compilazione), è possibile utilizzare una parte dell'applicazione.

È possibile utilizzare parti dell'applicazione per *evitare* cercando controller in un particolare assembly o un percorso. È possibile controllare quali parti o gli assembly sono disponibili per l'applicazione modificando il `ApplicationParts` insieme il `ApplicationPartManager`. L'ordine delle voci di `ApplicationParts` raccolta non è importante. È importante configurare completamente il `ApplicationPartManager` prima di utilizzarlo per configurare i servizi nel contenitore. Ad esempio, è necessario configurare completamente il `ApplicationPartManager` prima di richiamare `AddControllersAsServices`. In caso contrario, significa che i controller di parti dell'applicazione aggiunti dopo che chiamata al metodo non sarà interessata (non viene registrato come servizi) che potrebbe essere bevavior non corretto dell'applicazione.

Se si dispone di un assembly che contiene i controller non si desidera utilizzare, rimuoverlo dal `ApplicationPartManager`:

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

Oltre agli assembly del progetto e i relativi assembly dipendenti, la `ApplicationPartManager` includerà parti per `Microsoft.AspNetCore.Mvc.TagHelpers` e `Microsoft.AspNetCore.Mvc.Razor` per impostazione predefinita.

## <a name="application-feature-providers"></a>Provider di funzionalità dell'applicazione

Provider di funzionalità dell'applicazione esaminare parti dell'applicazione e forniscono funzionalità per tali parti. Sono disponibili provider di funzionalità predefinite per le seguenti funzionalità MVC:

* [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Riferimento ai metadati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Helper tag](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Visualizza i componenti](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Provider di funzionalità ereditare `IApplicationFeatureProvider<T>`, dove `T` è il tipo della funzionalità. È possibile implementare la propria funzionalità i provider per uno qualsiasi dei tipi di funzionalità del MVC elencati in precedenza. L'ordine dei provider di funzionalità di `ApplicationPartManager.FeatureProviders` raccolta può essere importante, poiché i provider successive possano rispondere alle azioni eseguite dal provider precedenti.

### <a name="sample-generic-controller-feature"></a>Esempio: Funzionalità controller generico

Per impostazione predefinita, componenti di base di ASP.NET MVC Ignora controller generico (ad esempio, `SomeController<T>`). In questo esempio utilizza un provider di funzionalità di controller che viene eseguita quando il provider predefinito e aggiunge istanze controller generico per un elenco specificato di tipi (definito in `EntityTypes.Types`):

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

I tipi di entità:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Il provider di funzionalità viene aggiunta in `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Per impostazione predefinita, i nomi di controller generico utilizzati per il routing sarà nel formato *GenericController'1 [Widget]* anziché *Widget*. L'attributo seguente viene utilizzato per modificare il nome in modo che corrisponda al tipo generico utilizzato dal controller:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

La `GenericController` classe:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Di conseguenza, quando viene richiesta una route corrispondente:

![Esempio di output dall'app di esempio legge, 'Hello da un controller Sproket generico'.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Esempio: Le funzionalità disponibili visualizzazione

È possibile scorrere le funzionalità popolate disponibili per l'app richiedendo un `ApplicationPartManager` tramite [inserimento di dipendenze](../../fundamentals/dependency-injection.md) e utilizzarlo per popolare le istanze delle funzionalità appropriate:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Output di esempio:

![Output di esempio di app di esempio](app-parts/_static/available-features.png)
