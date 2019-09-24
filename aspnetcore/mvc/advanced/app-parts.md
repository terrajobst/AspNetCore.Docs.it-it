---
title: Parti dell'applicazione in ASP.NET Core
author: rick-anderson
description: Condividi controller, Visualizza, Razor Pages e altro con le parti dell'applicazione in ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4b4c8c554a7045a180b56cf9998ab1a8496cde1b
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207345"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a>Condividi controller, visualizzazioni, Razor Pages e altro con le parti dell'applicazione in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([procedura per il download](xref:index#how-to-download-a-sample))

Una *parte dell'applicazione* è un'astrazione sulle risorse di un'app. Le parti dell'applicazione consentono ASP.NET Core di individuare controller, componenti di visualizzazione, helper tag, Razor Pages, origini di compilazione Razor e altro ancora. [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) è una parte dell'applicazione. `AssemblyPart`Incapsula un riferimento all'assembly ed espone i tipi e i riferimenti di compilazione.

I *provider di funzionalità* funzionano con le parti dell'applicazione per popolare le funzionalità di un'app ASP.NET Core. Il caso d'uso principale per le parti dell'applicazione consiste nel configurare un'app per individuare (o evitare il caricamento) ASP.NET Core funzionalità da un assembly. Ad esempio, potrebbe essere necessario condividere le funzionalità comuni tra più app. Con le parti dell'applicazione è possibile condividere un assembly (DLL) contenente controller, visualizzazioni, Razor Pages, origini di compilazione Razor, helper tag e altro ancora con più app. La condivisione di un assembly è preferibile alla duplicazione del codice in più progetti.

ASP.NET Core le app caricano <xref:System.Web.WebPages.ApplicationPart>le funzionalità da. La <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> classe rappresenta una parte dell'applicazione supportata da un assembly.

## <a name="load-aspnet-core-features"></a>Carica ASP.NET Core funzionalità

Usare le `ApplicationPart` classi `AssemblyPart` e per individuare e caricare ASP.NET Core funzionalità (controller, componenti di visualizzazione e così via). Consente di tenere traccia delle parti dell'applicazione e dei provider di funzionalità disponibili. <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> `ApplicationPartManager`è configurato in `Startup.ConfigureServices`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

Il codice seguente offre un approccio alternativo alla configurazione `ApplicationPartManager` di `AssemblyPart`utilizzando:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

I due esempi `SharedController` di codice precedenti caricano da un assembly. L' `SharedController` oggetto non è presente nel progetto dell'applicazione. Vedere il download dell'esempio di [soluzione WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .

### <a name="include-views"></a>Includi visualizzazioni

Per includere le visualizzazioni nell'assembly:

* Aggiungere il markup seguente al file di progetto condiviso:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> Aggiungere<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>alla:

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>Impedisci il caricamento delle risorse

Le parti dell'applicazione possono essere utilizzate per *evitare* il caricamento di risorse in un assembly o in un percorso specifico. Aggiungere o rimuovere membri della <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> raccolta per nascondere o rendere disponibili risorse. L'ordine delle voci nella raccolta `ApplicationParts` non è importante. Configurare prima `ApplicationPartManager` di usarlo per configurare i servizi nel contenitore. Configurare `ApplicationPartManager` , ad esempio, prima di `AddControllersAsServices`richiamare. `Remove` Chiamare`ApplicationParts` sulla raccolta per rimuovere una risorsa.

Il codice seguente usa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> per rimuovere `MyDependentLibrary` dall'app:[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

Include `ApplicationPartManager` le parti per:

* Assembly dell'app e assembly dipendenti.
* `Microsoft.AspNetCore.Mvc.TagHelpers`.
* `Microsoft.AspNetCore.Mvc.Razor`.

## <a name="application-feature-providers"></a>Provider di funzionalità dell'applicazione

I provider di funzionalità dell'applicazione esaminano le parti dell'applicazione e forniscono funzionalità per tali parti. Sono disponibili provider di funzionalità predefiniti per le funzionalità di ASP.NET Core seguenti:

* [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Helper tag](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Componenti di visualizzazione](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

I provider di funzionalità ereditano da <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, dove `T` è il tipo della funzionalità. I provider di funzionalità possono essere implementati per uno qualsiasi dei tipi di funzionalità elencati in precedenza. L'ordine dei provider di funzionalità in `ApplicationPartManager.FeatureProviders` può influisca sul comportamento in fase di esecuzione. I provider aggiunti successivamente possono rispondere alle azioni eseguite dai provider aggiunti precedentemente.

### <a name="generic-controller-feature"></a>Funzionalità controller generico

ASP.NET Core ignora i [controller generici](/dotnet/csharp/programming-guide/generics/generic-classes). Un controller generico ha un parametro di tipo (ad esempio `MyController<T>`,). Nell'esempio seguente vengono aggiunte istanze del controller generico per un elenco di tipi specificato:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

I tipi sono definiti in `EntityTypes.Types`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

Il provider di funzionalità viene aggiunto in `Startup`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

I nomi di controller generici usati per il routing hanno il formato *GenericController ' 1 [widget]* anziché *widget*. L'attributo seguente modifica il nome in modo che corrisponda al tipo generico utilizzato dal controller:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

La classe `GenericController`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Ad esempio, la richiesta di un URL `https://localhost:5001/Sprocket` di restituisce la risposta seguente:

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>Visualizzare funzionalità disponibili

Le funzionalità disponibili per un'app possono essere enumerate richiedendo un `ApplicationPartManager` inserimento delle [dipendenze](../../fundamentals/dependency-injection.md):

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

L' [esempio di download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) usa il codice precedente per visualizzare le funzionalità dell'app:

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
