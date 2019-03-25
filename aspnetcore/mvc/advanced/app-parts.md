---
title: Parti dell'applicazione in ASP.NET Core
author: ardalis
description: Informazioni sull'uso delle parti dell'applicazione, che sono astrazioni relative alle risorse di un'app, per rilevare o evitare di caricare funzionalità da un assembly.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 67bd40adef4cdb0bd781f70114d3954cd9a8ed09
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320095"
---
# <a name="application-parts-in-aspnet-core"></a>Parti dell'applicazione in ASP.NET Core

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

Una *parte dell'applicazione* è un'astrazione relativa alle risorse di un'applicazione che consente il rilevamento di funzionalità MVC quali controller, componenti della visualizzazione o helper tag. Un esempio di parte dell'applicazione è AssemblyPart, che incapsula un riferimento all'assembly ed espone tipi e riferimenti di compilazione. I *provider di funzionalità* vengono eseguiti con le parti dell'applicazione per popolare le funzionalità di un'app ASP.NET Core MVC. Il caso d'uso principale delle parti dell'applicazione è la configurazione dell'app in modo che rilevi (o eviti di caricare) funzionalità MVC da un assembly.

## <a name="introducing-application-parts"></a>Introduzione alle parti dell'applicazione

Le app MVC caricano le loro funzionalità dalle [parti dell'applicazione](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). In particolare la classe [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) rappresenta una parte dell'applicazione supportata da un assembly. Queste classi consentono di trovare e caricare funzionalità di MVC quali controller, componenti di visualizzazione, helper tag e origini di compilazione Razor. [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) gestisce il rilevamento delle parti dell'applicazione e dei provider di funzionalità disponibili per l'app MVC. È possibile interagire con `ApplicationPartManager` in `Startup` quando si configura MVC:

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

Per impostazione predefinita MVC esegue la ricerca nell'albero dipendenze e rileva i controller (anche in altri assembly). Per caricare un assembly arbitrario (ad esempio da un plug-in a cui non si fa riferimento in fase di compilazione), è possibile usare una parte dell'applicazione.

Le parti dell'applicazione possono essere usate per *evitare* la ricerca di controller in un assembly o un percorso specifico. È possibile controllare quali parti o assembly sono disponibili per l'app modificando la raccolta `ApplicationParts` di `ApplicationPartManager`. L'ordine delle voci nella raccolta `ApplicationParts` non è importante. È invece importante configurare completamente `ApplicationPartManager` prima di usarlo per configurare i servizi nel contenitore. Ad esempio, è necessario configurare completamente `ApplicationPartManager` prima di chiamare `AddControllersAsServices`. In caso contrario, i controller inclusi in parti dell'applicazione aggiunte dopo questa chiamata al metodo vengono ignorati (non vengono registrati come servizi) e questo può determinare errori di funzionamento dell'applicazione.

Se è presente un assembly contenente controller che non si vuole usare, rimuoverlo da `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Oltre all'assembly del progetto e agli assembly dipendenti, per impostazione predefinita `ApplicationPartManager` include parti per `Microsoft.AspNetCore.Mvc.TagHelpers` e `Microsoft.AspNetCore.Mvc.Razor`.

## <a name="application-feature-providers"></a>Provider di funzionalità dell'applicazione

I provider di funzionalità dell'applicazione esaminano le parti dell'applicazione e offrono funzionalità per tali parti. Sono disponibili provider di funzionalità predefiniti per le seguenti funzionalità MVC:

* [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Riferimento ai metadati](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Helper tag](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Componenti di visualizzazione](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

I provider di funzionalità ereditano da `IApplicationFeatureProvider<T>`, dove `T` è il tipo della funzionalità. È possibile implementare provider di funzionalità personalizzati per i tipi di funzionalità MVC elencati in precedenza. L'ordine dei provider di funzionalità nella raccolta `ApplicationPartManager.FeatureProviders` può essere importante, perché i provider in un determinato punto della raccolta possono rispondere alle azioni eseguite dai provider che li precedono.

### <a name="sample-generic-controller-feature"></a>Esempio: Funzionalità controller generico

Per impostazione predefinita, ASP.NET Core MVC ignora i controller generici (ad esempio `SomeController<T>`). Questo esempio usa come provider di funzionalità un controller che viene eseguito dopo il provider predefinito e aggiunge istanze controller generico per un elenco di tipi specificato (definito in `EntityTypes.Types`):

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Tipi di entità:

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Il provider di funzionalità viene aggiunto in `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Per impostazione predefinita i nomi di controller generico usati per il routing hanno il formato *GenericController`1[Widget]* anziché il formato *Widget*. L'attributo seguente viene usato per modificare il nome, in modo che corrisponda al tipo generico usato dal controller:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

La classe `GenericController`:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Risultato quando viene richiesta una route corrispondente:

![Esempio di output dell'app: "Hello from a generic Sprocket controller".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Esempio: Visualizzare funzionalità disponibili

È possibile eseguire l'iterazione tra le funzionalità popolate disponibili per l'app richiedendo `ApplicationPartManager` mediante l'[inserimento di dipendenze](../../fundamentals/dependency-injection.md) e usando tale elemento per popolare istanze delle funzionalità appropriate:

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Output di esempio:

![Output di esempio dall'app di esempio](app-parts/_static/available-features.png)
