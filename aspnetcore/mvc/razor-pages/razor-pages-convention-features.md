---
title: Funzionalità di convenzione di route e app per Razor Pages in ASP.NET Core
author: guardrex
description: Informazioni su come le funzionalità di convenzione di route e del provider di modello di app consentono di controllare routing, rilevamento ed elaborazione delle pagine.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 0d8dc4e236d82de6c59add8aa949c28e9435f8fa
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Funzionalità di convenzione di route e app per Razor Pages in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Informazioni su come usare le funzionalità di [convenzione di route e del provider di modello di app](xref:mvc/controllers/application-model#conventions) della pagina per controllare routing, rilevamento ed elaborazione nelle app Razor Pages. Quando è necessario configurare la route di una pagina personalizzata per le singole pagine, configurare il routing alle pagine con la [convenzione AddPageRoute](#configure-a-page-route) descritta più avanti in questo argomento.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

::: moniker range="= aspnetcore-2.0"
| Funzionalità | L'esempio illustra come eseguire le seguenti operazioni: |
| -------- | --------------------------- |
| [Convenzioni del modello](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Aggiungere un modello e un'intestazione di route alle pagine di un'app. |
| [Convenzioni per le azioni di route di pagina](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Aggiungere un modello di route alle pagine in una cartella e a una pagina singola. |
| [Convenzioni per le azioni del modello di pagina](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (classe di filtro, espressione lambda o factory di filtro)</li></ul> | Aggiungere un'intestazione alle pagine in una cartella, aggiungere un'intestazione a una pagina singola e configurare una [factory di filtro](xref:mvc/controllers/filters#ifilterfactory) per aggiungere un'intestazione alle pagine di un'app. |
| [Provider di modello di app della pagina predefinito](#replace-the-default-page-app-model-provider) | Sostituire il provider di modello di pagina predefinito per modificare le convenzioni dei nomi del gestore. |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Funzionalità | L'esempio illustra come eseguire le seguenti operazioni: |
| -------- | --------------------------- |
| [Convenzioni del modello](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Aggiungere un modello e un'intestazione di route alle pagine di un'app. |
| [Convenzioni per le azioni di route di pagina](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Aggiungere un modello di route alle pagine in una cartella e a una pagina singola. |
| [Convenzioni per le azioni del modello di pagina](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (classe di filtro, espressione lambda o factory di filtro)</li></ul> | Aggiungere un'intestazione alle pagine in una cartella, aggiungere un'intestazione a una pagina singola e configurare una [factory di filtro](xref:mvc/controllers/filters#ifilterfactory) per aggiungere un'intestazione alle pagine di un'app. |
| [Provider di modello di app della pagina predefinito](#replace-the-default-page-app-model-provider) | Sostituire il provider di modello di pagina predefinito per modificare le convenzioni dei nomi del gestore. |
::: moniker-end

Le convenzioni di Razor Pages vengono aggiunte e configurate usando il metodo di estensione [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) in [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) nella raccolta di servizi nella classe `Startup`. Gli esempi di convenzione seguenti sono illustrati più avanti in questo argomento:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="model-conventions"></a>Convenzioni del modello

Aggiungere un delegato per [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) per aggiungere le [convenzioni del modello](xref:mvc/controllers/application-model#conventions) che si applicano a Razor Pages.

**Aggiungere una convenzione del modello di route a tutte le pagine**

Usare [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) per creare e aggiungere un'interfaccia [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) alla raccolta di istanze [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) che vengono applicate durante la costruzione del modello di route della pagina.

L'app di esempio aggiunge un modello di route `{globalTemplate?}` a tutte le pagine nell'app:

[!code-csharp[](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> La proprietà `Order` per `AttributeRouteModel` è impostata su `-1`. In questo modo si garantisce che al modello venga assegnata la priorità per la prima posizione del valore di dati della route quando viene specificato un valore singolo di route e continui ad avere la priorità sulle ruote di Razor Pages generate automaticamente. Ad esempio, l'esempio aggiunge un modello di route `{aboutTemplate?}` più avanti in questo argomento. Al modello `{aboutTemplate?}` viene assegnato un `Order` di `1`. Quando viene richiesta la pagina di informazioni in `/About/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = -1`) e non in `RouteData.Values["aboutTemplate"]` (`Order = 1`) a causa dell'impostazione della proprietà `Order`.

Le opzioni di Razor Pages, come l'aggiunta di [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), vengono integrate quando MVC è aggiunto alla raccolta di servizi in `Startup.ConfigureServices`. Per un esempio completo, vedere [l'app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample/).

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About/GlobalRouteValue` ed esaminare il risultato:

![La pagina di informazioni viene richiesta con un segmento di route di GlobalRouteValue. La pagina sottoposta a rendering indica che il valore di dati della route viene acquisito nel metodo OnGet della pagina.](razor-pages-convention-features/_static/about-page-global-template.png)

**Aggiungere una convenzione del modello di app a tutte le pagine**

Usare [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) per creare e aggiungere un'interfaccia [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) alla raccolta di istanze [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) che vengono applicate durante la costruzione del modello di app della pagina.

Per dimostrare questa e altre convenzioni più avanti in questo argomento, l'app di esempio include una classe `AddHeaderAttribute`. Il costruttore della classe accetta una stringa `name` e una matrice di stringhe `values`. Questi valori vengono usati nel relativo metodo `OnResultExecuting` per impostare un'intestazione di risposta. La classe completa è illustrata nella sezione [Convenzioni per le azioni del modello di pagina](#page-model-action-conventions) più avanti in questo argomento.

L'app di esempio usa la classe `AddHeaderAttribute` per aggiungere un'intestazione, `GlobalHeader`, a tutte le pagine nell'app:

[!code-csharp[](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:

![Le intestazioni di risposta della pagina di informazioni indicano che è stato aggiunto l'elemento GlobalHeader.](razor-pages-convention-features/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"
**Aggiungere una convenzione del modello di gestore a tutte le pagine**

[!INCLUDE[](~/includes/2.1.md)]

Usare [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) per creare e aggiungere un'interfaccia [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) alla raccolta di istanze [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) che vengono applicate durante la costruzione del modello di gestore della pagina.

```csharp
public class GlobalPageHandlerModelConvention 
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

`Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```
::: moniker-end

## <a name="page-route-action-conventions"></a>Convenzioni per le azioni di route di pagina

Il provider di modello di route predefinito che deriva da [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) chiama le convenzioni progettate per offrire punti di estendibilità per la configurazione delle route di pagina.

**Convenzione per il modello di route cartella**

Usare il metodo [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) per creare e aggiungere un'interfaccia [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) che chiama un'azione sulla classe [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) per tutte le pagine nella cartella specificata.

L'app di esempio usa `AddFolderRouteModelConvention` per aggiungere un modello di route `{otherPagesTemplate?}` alle pagine nella cartella *OtherPages*:

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> La proprietà `Order` per `AttributeRouteModel` è impostata su `1`. In questo modo si garantisce che al modello per `{globalTemplate?}` (impostato in precedenza in questo argomento) venga assegnata la priorità per la prima posizione del valore di dati della route quando viene specificato un valore singolo di route. Se viene richiesta la pagina Page1 in `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = -1`) e non in `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) a causa dell'impostazione della proprietà `Order`.

Richiedere la pagina Page1 dell'esempio in `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` ed esaminare il risultato:

![La pagina Page1 nella cartella OtherPages viene richiesta con un segmento di route di GlobalRouteValue e OtherPagesRouteValue. La pagina sottoposta a rendering indica che i valori di dati della route vengono acquisiti nel metodo OnGet della pagina.](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**Convenzione per il modello di route pagina**

Usare il metodo [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) per creare e aggiungere un'interfaccia [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) che chiama un'azione sulla classe [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) per la pagine con il nome specificato.

L'app di esempio usa `AddPageRouteModelConvention` per aggiungere un modello di route `{aboutTemplate?}` alla pagina di informazioni:

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> La proprietà `Order` per `AttributeRouteModel` è impostata su `1`. In questo modo si garantisce che al modello per `{globalTemplate?}` (impostato in precedenza in questo argomento) venga assegnata la priorità per la prima posizione del valore di dati della route quando viene specificato un valore singolo di route. Se viene richiesta la pagina di informazioni in `/About/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = -1`) e non in `RouteData.Values["aboutTemplate"]` (`Order = 1`) a causa dell'impostazione della proprietà `Order`.

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About/GlobalRouteValue/AboutRouteValue` ed esaminare il risultato:

![La pagina di informazioni viene richiesta con segmenti di route per GlobalRouteValue e AboutRouteValue. La pagina sottoposta a rendering indica che i valori di dati della route vengono acquisiti nel metodo OnGet della pagina.](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Configurare una route di pagina

Usare il metodo [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) per configurare una route a una pagina in corrispondenza del percorso della pagina specificato. I collegamenti generati alla pagina usano la route specificata. `AddPageRoute` usa `AddPageRouteModelConvention` per stabilire la route.

L'app di esempio crea una route a `/TheContactPage` per *Contact.cshtml*:

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

La pagina di contatto può anche essere raggiunta in corrispondenza di `/Contact` tramite la route predefinita.

La route personalizzata dell'app di esempio alla pagina di contatto consente un segmento di route `text` facoltativo (`{text?}`). La pagina include anche tale segmento facoltativo nella relativa istruzione `@page` nel caso in cui il visitatore acceda alla pagina in corrispondenza della relativa route `/Contact`:

[!code-cshtml[](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

Si noti che l'URL generato per il collegamento **Contatto** nella pagina sottoposta a rendering riflette la route aggiornata:

![Collegamento alla pagina di contatto dell'app di esempio nella barra di spostamento](razor-pages-convention-features/_static/contact-link.png)

![L'esame del collegamento alla pagina di contatto nell'HTML sottoposto a rendering indica che href è impostato su '/TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

Visitare la pagina di contatto nella route normale, `/Contact`, o nella route personalizzata `/TheContactPage`. Se si specifica un segmento di route `text` aggiuntivo, la pagina visualizza il segmento codificato in formato HTML specificato dall'utente:

![Esempio di browser Microsoft Edge in cui viene specificato un segmento di route facoltativo 'text' corrispondente a 'TextValue' nell'URL. La pagina sottoposta a rendering visualizza il valore del segmento 'text'.](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenzioni per le azioni del modello di pagina

Il provider di modello di pagina predefinito che implementa [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) chiama le convenzioni progettate per offrire punti di estendibilità per la configurazione dei modelli di pagina. Queste convenzioni sono utili durante la compilazione e la modifica delle funzionalità di individuazione e rilevamento delle pagine.

Per gli esempi di questa sezione, l'app di esempio usa una classe `AddHeaderAttribute`, ovvero una classe [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), che applica un'intestazione di risposta:

[!code-csharp[](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

Tramite le convenzioni, nell'esempio viene illustrato come applicare l'attributo a tutte le pagine in una cartella e a una singola pagina.

**Convenzione per il modello di app cartella**

Usare il metodo [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) per creare e aggiungere un'interfaccia [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) che chiama un'azione sulle istanze [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) per tutte le pagine nella cartella specificata.

Nell'esempio viene illustrato l'uso di `AddFolderApplicationModelConvention` aggiungendo un'intestazione, `OtherPagesHeader`, alle pagine all'interno della cartella *OtherPages* dell'app:

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

Richiedere la pagina Page1 dell'esempio in `localhost:5000/OtherPages/Page1` ed esaminare le intestazioni per visualizzare il risultato:

![Le intestazioni di risposta della pagina OtherPages/Page1 indicano che è stato aggiunto l'elemento OtherPagesHeader.](razor-pages-convention-features/_static/page1-otherpages-header.png)

**Convenzione per il modello di app cartella**

Usare il metodo [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) per creare e aggiungere un'interfaccia [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) che chiama un'azione sulla classe [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) per la pagina con il nome specificato.

Nell'esempio viene illustrato l'uso di `AddPageApplicationModelConvention` aggiungendo un'intestazione, `AboutHeader`, alla pagina di informazioni:

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:

![Le intestazioni di risposta della pagina di informazioni indicano che è stato aggiunto l'elemento AboutHeader.](razor-pages-convention-features/_static/about-page-about-header.png)

**Configurare un filtro**

Il metodo [ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configura il filtro specificato per l'applicazione. È possibile implementare una classe di filtro, ma l'app di esempio illustra come implementare un filtro in un'espressione lambda, che viene implementata in background come una factory che restituisce un filtro:

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

Viene usato il modello di app della pagina per verificare il percorso relativo per i segmenti che portano alla pagina Page2 nella cartella *OtherPages*. Se la condizione viene superata, viene aggiunta un'intestazione. In caso contrario, viene applicato `EmptyFilter`.

`EmptyFilter` è un [filtro azione](xref:mvc/controllers/filters#action-filters). Poiché i filtri azione vengono ignorati da Razor Pages, `EmptyFilter` non esegue nessuna operazione come previsto se il percorso non contiene `OtherPages/Page2`.

Richiedere la pagina Page2 dell'esempio in `localhost:5000/OtherPages/Page2` ed esaminare le intestazioni per visualizzare il risultato:

![L'elemento OtherPagesPage2Header viene aggiunto alla risposta per Page2.](razor-pages-convention-features/_static/page2-filter-header.png)

**Configurare una factory di filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configura la factory specificata per applicare [filtri](xref:mvc/controllers/filters) all'intera Razor Pages.

L'app di esempio illustra un esempio dell'uso di una [factory di filtro](xref:mvc/controllers/filters#ifilterfactory) aggiungendo un'intestazione, `FilterFactoryHeader`, con due valori per le pagine dell'app:

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:

![Le intestazioni di risposta della pagina informazioni indicano che sono state aggiunte due intestazioni FilterFactoryHeader.](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Sostituire il provider di modello di app della pagina predefinito

Razor Pages usa l'interfaccia `IPageApplicationModelProvider` per creare una classe [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). È possibile ereditare dal provider di modello predefinito per offrire la propria logica di implementazione per l'individuazione e l'elaborazione del gestore. L'implementazione predefinita ([origine riferimento](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) stabilisce le convenzioni per la denominazione del gestore *senza nome* e *denominato*, come descritto di seguito.

**Metodi del gestore senza nome predefinito**

I metodi del gestore per i verbi HTTP (metodi del gestore "senza nome") seguono una convenzione: `On<HTTP verb>[Async]` (l'aggiunta di `Async` è facoltativa ma consigliata per i metodi asincroni).

| Metodo del gestore senza nome     | Operazione                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inizializzare lo stato della pagina.     |
| `OnPost`/`OnPostAsync`     | Gestire le richieste POST.          |
| `OnDelete`/`OnDeleteAsync` | Gestire le richieste DELETE&#8224;. |
| `OnPut`/`OnPutAsync`       | Gestire le richieste PUT&#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Gestire le richieste PATCH&#8224;.  |

&#8224;Usato per le chiamate API alla pagina.

**Metodi del gestore denominato predefinito**

I metodi del gestore specificati dallo sviluppatore (metodi del gestore "denominati") seguono una convenzione simile. Il nome del gestore viene visualizzato dopo il verbo HTTP o tra il verbo HTTP e `Async`: `On<HTTP verb><handler name>[Async]` (l'aggiunta di `Async` è facoltativa ma consigliata per i metodi asincroni). Ad esempio, i metodi che elaborano i messaggi potrebbero assumere la denominazione indicata nella tabella seguente.

| Metodo del gestore denominato di esempio             | Operazione di esempio        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Ottenere un messaggio.        |
| `OnPostMessage`/`OnPostMessageAsync`     | POST per un messaggio.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | DELETE per un messaggio&#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | PUT per un messaggio&#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | PATCH per un messaggio&#8224;.  |

&#8224;Usato per le chiamate API alla pagina.

**Personalizzare i nomi dei metodi dei gestori**

Si supponga di voler modificare il modo in cui vengono denominati i metodi con e senza nome gestore. Uno schema di denominazione alternativo consiste nell'evitare che i nomi di metodo inizino con"On" e nell'usare il primo segmento della parola per determinare il verbo HTTP. È possibile apportare altre modifiche, ad esempio convertire i verbi per DELETE, PUT, e PATCH in POST. Questo sistema offre i nomi di metodo illustrati nella tabella seguente.

| Metodo gestore                       | Operazione                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inizializzare lo stato della pagina.     |
| `Post`/`PostAsync`                   | Gestire le richieste POST.          |
| `Delete`/`DeleteAsync`               | Gestire le richieste DELETE&#8224;. |
| `Put`/`PutAsync`                     | Gestire le richieste PUT&#8224;.    |
| `Patch`/`PatchAsync`                 | Gestire le richieste PATCH&#8224;.  |
| `GetMessage`                         | Ottenere un messaggio.              |
| `PostMessage`/`PostMessageAsync`     | POST per un messaggio.                |
| `DeleteMessage`/`DeleteMessageAsync` | POST per un messaggio da eliminare.      |
| `PutMessage`/`PutMessageAsync`       | POST per un messaggio da inserire.         |
| `PatchMessage`/`PatchMessageAsync`   | POST per un messaggio a cui applicare la patch.       |

&#8224;Usato per le chiamate API alla pagina.

Per stabilire questo schema, ereditare dalla classe `DefaultPageApplicationModelProvider` ed eseguire l'override del metodo [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) per specificare la logica personalizzata per risolvere i nomi del gestore della classe [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). L'app di esempio illustra come questa operazione viene eseguita nella relativa classe `CustomPageApplicationModelProvider`:

[!code-csharp[](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Le caratteristiche della classe sono le seguenti:

* La classe eredita da `DefaultPageApplicationModelProvider`.
* L'elemento `TryParseHandlerMethod` elabora un gestore per determinare il verbo HTTP (`httpMethod`) il nome del gestore denominato (`handlerName`) quando si crea l'elemento `PageHandlerModel`.
  * Se presente, l'eventuale suffisso `Async` viene ignorato.
  * Viene applicata la distinzione tra maiuscole e minuscole per analizzare il verbo HTTP dal nome del metodo.
  * Quando il nome del metodo (senza `Async`) è uguale al nome del verbo HTTP, non vi è alcun gestore denominato. L'elemento `handlerName` è impostato su `null` e il nome del metodo è `Get`, `Post`, `Delete`, `Put` o `Patch`.
  * Quando il nome del metodo (senza `Async`) è più lungo del nome del verbo HTTP, è presente un gestore denominato. `handlerName` viene impostato su `<method name (less 'Async', if present)>`. Ad esempio, sia "GetMessage" che "GetMessageAsync" generano un nome gestore di "GetMessage".
  * I verbi HTTP DELETE, PUT e PATCH vengono convertiti in POST.

Registrare l'elemento `CustomPageApplicationModelProvider` nella classe `Startup`:

[!code-csharp[](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

Il modello di pagina in *Index.cshtml.cs* illustra come le convenzioni di denominazione del metodo del gestore normale vengono modificate per le pagine nell'app. La denominazione di prefisso "On" normale usata con Razor Pages viene rimossa. Il metodo che inizializza lo stato della pagina è ora denominato `Get`. L'uso di questa convenzione può essere visualizzato nell'intera app se si apre qualsiasi modello di pagina per qualsiasi pagina.

Ognuno degli altri metodi inizia con il verbo HTTP che ne descrive l'elaborazione. I due metodi che iniziano con `Delete` normalmente verrebbero considerati come verbi HTTP DELETE, ma la logica in `TryParseHandlerMethod` imposta in modo esplicito il verbo POST per entrambi i gestori.

Si noti che `Async` è facoltativo tra `DeleteAllMessages` e `DeleteMessageAsync`. Entrambi i metodi sono asincroni, ma è possibile scegliere di usare o meno il suffisso `Async`; è consigliabile usarlo. `DeleteAllMessages` è qui usato a scopo dimostrativo, ma è consigliabile denominare tale metodo `DeleteAllMessagesAsync`. Non influisce sull'elaborazione di implementazione dell'esempio, ma l'uso del suffisso `Async` indica che si tratta di un metodo asincrono.

[!code-csharp[](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Si noti che i nomi dei gestori specificati in *cshtml* corrispondono ai metodi del gestore `DeleteAllMessages` e `DeleteMessageAsync`:

[!code-cshtml[](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` nel nome del metodo del gestore `DeleteMessageAsync` è stato escluso dall'elemento `TryParseHandlerMethod` per la corrispondenza della richiesta POST al metodo del gestore. Il nome `asp-page-handler` di `DeleteMessage` corrisponde al metodo del gestore `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtri MVC e il filtro di pagina (IPageFilter)

I [filtri azione](xref:mvc/controllers/filters#action-filters) MVC vengono ignorati da Razor Pages, poiché vengono usati i metodi del gestore. Sono disponibili altri tipi di filtri MVC: [autorizzazione](xref:mvc/controllers/filters#authorization-filters), [eccezione](xref:mvc/controllers/filters#exception-filters), [risorse](xref:mvc/controllers/filters#resource-filters) e [risultati](xref:mvc/controllers/filters#result-filters). Per altre informazioni, vedere l'argomento [Filtri](xref:mvc/controllers/filters).

Il filtro di pagina ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) è un filtro che si applica a Razor Pages. Per altre informazioni, vedere [Modalità di filtro per pagine Razor](xref:mvc/razor-pages/filter).

## <a name="see-also"></a>Vedere anche

* [Convenzioni di autorizzazione di Razor Pages](xref:security/authorization/razor-pages-authorization)
