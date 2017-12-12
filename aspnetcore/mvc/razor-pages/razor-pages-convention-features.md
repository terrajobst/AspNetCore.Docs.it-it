---
title: "Razor pagine route e app convenzione le funzionalità ASP.NET Core"
author: guardrex
description: Individuare la route e app modello provider convenzione che consentono di routing di controllo pagina, l'individuazione e l'elaborazione.
keywords: Componenti di base di ASP.NET, pagine Razor, convenzioni, AddFolderRouteModelConvention, AddPageRouteModelConvention, AddPageRoute, AddFolderApplicationModelConvention, AddPageApplicationModelConvention, ConfigureFilter, filtri
ms.author: riande
manager: wpickett
ms.date: 10/23/2017
ms.topic: article
ms.assetid: 6b60514c-81ad-485b-bb22-9b71416dff08
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 81fe5198e25c4275f5cf0a123536a9130be8c1d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Razor pagine route e app convenzione le funzionalità ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Informazioni su come usare pagina route e app provider convenzione funzionalità di modello per controllare la pagina routing, l'individuazione e l'elaborazione in App pagine Razor. Quando è necessario configurare le route di una pagina personalizzata per le singole pagine, configurare il routing alle pagine con il [AddPageRoute convenzione](#configure-a-page-route) descritto più avanti in questo argomento.

Utilizzare il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) per esplorare le funzionalità descritte in questo argomento.

| Funzionalità | Di seguito viene illustrato il codice di esempio... |
| -------- | --------------------------- |
| [Convenzioni di modello di route e app](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Aggiunta di un modello di route e l'intestazione per le pagine di un'app. |
| [Convenzioni di azione route di pagina](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Aggiunta di un modello di route per le pagine in una cartella e a una singola pagina. |
| [Convenzioni della pagina modello azione](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (classe di filtro, espressione lambda o filtro factory)</li></ul> | Aggiunta di un'intestazione per le pagine in una cartella, aggiungere un'intestazione a pagina singola e la configurazione un [factory filtro](xref:mvc/controllers/filters#ifilterfactory) per aggiungere un'intestazione per le pagine di un'app. |
| [Provider del modello app pagina predefinito](#replace-the-default-page-app-model-provider) | Sostituire il provider del modello di pagina predefinito per modificare le convenzioni di denominazione di gestore. |

## <a name="add-route-and-app-model-conventions"></a>Aggiungere le convenzioni di modello di route e app

Aggiungere un delegato per [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) per aggiungere le convenzioni modello di route e app che si applicano alle pagine Razor.

**Aggiungere una convenzione del modello di route per tutte le pagine**

Utilizzare [convenzioni](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) per creare e aggiungere un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) alla raccolta di [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) istanze che vengono applicate durante il modello di route e di pagina costruzione.

L'app di esempio aggiunge un `{globalTemplate?}` modello di route per tutte le pagine nell'app:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> Il `Order` proprietà per il `AttributeRouteModel` è impostato su `0` (zero). In questo modo si garantisce che il modello viene assegnato priorità per la prima posizione del valore di dati route quando viene fornito un valore singolo di route. Ad esempio, l'esempio aggiunge un `{aboutTemplate?}` modello di route, più avanti in questo argomento. Il `{aboutTemplate?}` modello viene assegnato un `Order` di `1`. Quando la pagina di informazioni su è richiesta in `/About/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 0`) e non `RouteData.Values["aboutTemplate"]` (`Order = 1`) a causa dell'impostazione di `Order` proprietà.

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

Richiesta sulla pagina dell'esempio `localhost:5000/About/GlobalRouteValue` ed esaminare il risultato:

![La pagina di informazioni su è richiesta con un segmento di route di GlobalRouteValue. La pagina mostra che il valore di dati di route viene acquisito nel metodo OnGet della pagina.](razor-pages-convention-features/_static/about-page-global-template.png)

**Aggiungere una convenzione del modello di applicazione a tutte le pagine**

Utilizzare [convenzioni](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) per creare e aggiungere un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) alla raccolta di [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) istanze che vengono applicate durante la route e di pagina costruzione del modello.

Per dimostrare questo e altri convenzioni più avanti in questo argomento, l'app di esempio include un `AddHeaderAttribute` classe. Il costruttore della classe accetta un `name` stringa e un `values` matrice di stringhe. Questi valori vengono utilizzati nel relativo `OnResultExecuting` per impostare un'intestazione di risposta. La classe completa è illustrata nella [pagina convenzioni azione modello](#page-model-action-conventions) sezione più avanti in questo argomento.

L'applicazione di esempio utilizza il `AddHeaderAttribute` classe per aggiungere un'intestazione, `GlobalHeader`, per tutte le pagine nell'app:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

Richiesta sulla pagina dell'esempio `localhost:5000/About` ed esaminare le intestazioni per visualizzare i risultati:

![Intestazioni di risposta della pagina informazioni su mostrano che è stato aggiunto il GlobalHeader.](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>Convenzioni di azione route di pagina

Il provider del modello di route predefinito che deriva da [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) richiama le convenzioni sono progettate per fornire punti di estendibilità per la configurazione delle route di pagina.

**Convenzione del modello di route cartella**

Utilizzare [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) per creare e aggiungere un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) che richiama un'azione sul [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) per tutte le pagine nel cartella specificata.

L'applicazione di esempio utilizza `AddFolderRouteModelConvention` per aggiungere un `{otherPagesTemplate?}` il modello di route per le pagine di *OtherPages* cartella:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> Il `Order` proprietà per il `AttributeRouteModel` è impostato su `1`. Ciò garantisce che il modello per `{globalTemplate?}` (set di precedenza in questo argomento) è la priorità per i dati della route primo valore di posizione quando viene fornito un valore singolo di route. Se la pagina Page1 viene richiesto a `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 0`) e non `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) a causa dell'impostazione di `Order` proprietà.

Richiesta di pagina Page1 dell'esempio `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` ed esaminare il risultato:

![È richiesto un segmento di route di GlobalRouteValue e OtherPagesRouteValue Page1 nella cartella OtherPages. La pagina mostra che i valori di dati della route vengono acquisiti nel metodo OnGet della pagina.](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**Convenzione del modello di route pagina**

Utilizzare [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) per creare e aggiungere un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) che richiama un'azione sul [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) per la pagina con l'oggetto specificato nome.

L'applicazione di esempio utilizza `AddPageRouteModelConvention` per aggiungere un `{aboutTemplate?}` modello di route per la pagina informazioni su:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> Il `Order` proprietà per il `AttributeRouteModel` è impostato su `1`. Ciò garantisce che il modello per `{globalTemplate?}` (set di precedenza in questo argomento) è la priorità per i dati della route primo valore di posizione quando viene fornito un valore singolo di route. Se la pagina di informazioni su è richiesta in `/About/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 0`) e non `RouteData.Values["aboutTemplate"]` (`Order = 1`) a causa dell'impostazione di `Order` proprietà.

Richiesta sulla pagina dell'esempio `localhost:5000/About/GlobalRouteValue/AboutRouteValue` ed esaminare il risultato:

![Informazioni sulla pagina viene richiesta con segmenti di route per GlobalRouteValue e AboutRouteValue. La pagina mostra che i valori di dati della route vengono acquisiti nel metodo OnGet della pagina.](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Configurare una route di pagina

Utilizzare [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) per configurare una route a una pagina in corrispondenza del percorso della pagina specificata. Collegamenti generati alla pagina di utilizzano la route specificata. `AddPageRoute`Usa `AddPageRouteModelConvention` per stabilire la route.

L'app di esempio viene creata una route per `/TheContactPage` per *Contact.cshtml*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

La pagina di contatto può anche essere raggiunto al `/Contact` tramite la route predefinita.

Consente di route personalizzato dell'applicazione di esempio per la pagina di contatto per un parametro facoltativo `text` segmento route (`{text?}`). La pagina include inoltre il segmento facoltativo nel relativo `@page` direttiva nel caso in cui il visitatore accede alla pagina nel relativo `/Contact` route:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

Si noti che l'URL generato per il **contatto** collegamento nella pagina sottoposta a rendering riflette la route aggiornata:

![Collegamento di contatto di app di esempio nella barra di spostamento](razor-pages-convention-features/_static/contact-link.png)

![Esaminare il collegamento di contatto nell'HTML indica che href è impostata su ' / TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

Visitare la pagina di contatto nel relativo route normale, `/Contact`, o le route personalizzata `/TheContactPage`. Se si fornisce un ulteriore `text` segmento di route, la pagina viene illustrato il segmento codificata in formato HTML che forniscono:

![Esempio di browser Edge consentono di fornire un segmento di route facoltativo 'text' di 'Testo' nell'URL. La pagina viene illustrato il valore del segmento 'text'.](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenzioni della pagina modello azione

Il provider del modello di pagina predefinito che implementa [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) richiama le convenzioni sono progettate per fornire punti di estendibilità per la configurazione di modelli di pagina. Queste convenzioni sono utili quando la creazione e modifica di individuazione di pagina e le caratteristiche di elaborazione.

Per esempi di questa sezione, l'app di esempio utilizza un `AddHeaderAttribute` (classe), ovvero un [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), che applica un'intestazione di risposta:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

Utilizza le convenzioni, nell'esempio viene illustrato come applicare l'attributo a tutte le pagine in una cartella e a una singola pagina.

**Convenzione del modello di cartella app**

Utilizzare [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) per creare e aggiungere un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) che richiama un'azione su [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) nelle istanze di tutte le pagine nella cartella specificata.

Nell'esempio viene illustrato l'utilizzo di `AddFolderApplicationModelConvention` aggiungendo un'intestazione, `OtherPagesHeader`, alle pagine all'interno di *OtherPages* cartella dell'app:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

Richiesta di pagina Page1 dell'esempio `localhost:5000/OtherPages/Page1` ed esaminare le intestazioni per visualizzare i risultati:

![Intestazioni di risposta della pagina OtherPages/Page1 mostrano che è stato aggiunto il OtherPagesHeader.](razor-pages-convention-features/_static/page1-otherpages-header.png)

**Convenzione del modello di app di pagina**

Utilizzare [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) per creare e aggiungere un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) che richiama un'azione sul [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) per la pagina con il nome speciifed.

Nell'esempio viene illustrato l'utilizzo di `AddPageApplicationModelConvention` aggiungendo un'intestazione, `AboutHeader`, alla pagina di informazioni su:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

Richiesta sulla pagina dell'esempio `localhost:5000/About` ed esaminare le intestazioni per visualizzare i risultati:

![Intestazioni di risposta della pagina informazioni su mostrano che è stato aggiunto il AboutHeader.](razor-pages-convention-features/_static/about-page-about-header.png)

**Configurare un filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) Configura applicare il filtro specificato. È possibile implementare una classe di filtro, ma l'app di esempio viene illustrato come implementare un filtro in un'espressione lambda, che viene implementata background come una factory che restituisce un filtro:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

Viene utilizzato il modello di app di pagina per verificare il percorso relativo per i segmenti che portano alla pagina Pagina2 il *OtherPages* cartella. Se la condizione viene superato, viene aggiunta un'intestazione. In caso contrario, il `EmptyFilter` viene applicato.

`EmptyFilter`è un [filtro azione](xref:mvc/controllers/filters#action-filters). Poiché i filtri azione vengono ignorati da pagine Razor, il `EmptyFilter` ops no come previsto se il percorso non contiene `OtherPages/Page2`.

Richiesta di pagina Pagina2 dell'esempio `localhost:5000/OtherPages/Page2` ed esaminare le intestazioni per visualizzare i risultati:

![Il OtherPagesPage2Header viene aggiunto alla risposta per Page2.](razor-pages-convention-features/_static/page2-filter-header.png)

**Configurare una factory di filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configura la factory specificata per applicare [filtri](xref:mvc/controllers/filters) a tutte le pagine Razor.

L'app di esempio viene fornito un esempio dell'utilizzo di un [factory filtro](xref:mvc/controllers/filters#ifilterfactory) aggiungendo un'intestazione, `FilterFactoryHeader`, con due valori per le pagine dell'app:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Richiesta sulla pagina dell'esempio `localhost:5000/About` ed esaminare le intestazioni per visualizzare i risultati:

![Intestazioni di risposta della pagina informazioni su mostrano che sono stati aggiunti due intestazioni FilterFactoryHeader.](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Sostituire il provider del modello di app pagina predefinito

Pagine Razor utilizza il `IPageApplicationModelProvider` interfaccia per creare un [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). È possibile ereditare dal provider di modello predefinito per la logica di implementazione per l'elaborazione e l'individuazione di gestore. L'implementazione predefinita ([origine riferimento](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) stabilisce le convenzioni per *senza nome* e *denominato* gestore di denominazione, come descritto di seguito.

**Valore predefinito senza nome metodi del gestore**

I metodi del gestore per i verbi HTTP (i metodi del gestore "senza nome") seguono una convenzione: `On<HTTP verb>[Async]` (aggiunta `Async` è facoltativo ma consigliato per i metodi asincroni).

| Metodo del gestore senza nome     | Operazione                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inizializzare lo stato della pagina.     |
| `OnPost`/`OnPostAsync`     | Gestire le richieste POST.          |
| `OnDelete`/`OnDeleteAsync` | Gestire le richieste DELETE &#8224;. |
| `OnPut`/`OnPutAsync`       | Gestire le richieste PUT &#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Gestire le richieste PATCH &#8224;.  |

&#8224; Utilizzato per le chiamate API per la pagina.

**I metodi del gestore denominato predefinito**

I metodi del gestore forniti dallo sviluppatore ("denominato" i metodi del gestore) seguono una convenzione simile. Il nome del gestore viene visualizzato dopo il verbo HTTP o tra il verbo HTTP e `Async`: `On<HTTP verb><handler name>[Async]` (aggiunta `Async` è facoltativo ma consigliato per i metodi asincroni). Ad esempio, i metodi che elaborano i messaggi potrebbero richiedere la denominazione nella tabella seguente.

| Esempio denominato metodo del gestore             | Operazione di esempio        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Ottenere un messaggio.        |
| `OnPostMessage`/`OnPostMessageAsync`     | INVIARE un messaggio.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | ELIMINARE un messaggio di &#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | Inserire un messaggio di &#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | PATCH di un messaggio di &#8224;.  |

&#8224; Utilizzato per le chiamate API per la pagina.

**Personalizzare i nomi di gestori (metodo)**

Si supponga che si vuole modificare il modo in cui sono denominati metodi denominati e non del gestore. Uno schema di denominazione alternativo è evitare di avviare i nomi di metodo con "On" e utilizzare il primo segmento di word per determinare il verbo HTTP. È possibile apportare altre modifiche, ad esempio la conversione i verbi da eliminare, inserire e applicare PATCH ai POST. Tale sistema fornisce i nomi di metodo illustrati nella tabella seguente.

| Metodo del gestore                       | Operazione                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inizializzare lo stato della pagina.     |
| `Post`/`PostAsync`                   | Gestire le richieste POST.          |
| `Delete`/`DeleteAsync`               | Gestire le richieste DELETE &#8224;. |
| `Put`/`PutAsync`                     | Gestire le richieste PUT &#8224;.    |
| `Patch`/`PatchAsync`                 | Gestire le richieste PATCH &#8224;.  |
| `GetMessage`                         | Ottenere un messaggio.              |
| `PostMessage`/`PostMessageAsync`     | INVIARE un messaggio.                |
| `DeleteMessage`/`DeleteMessageAsync` | INVIARE un messaggio da eliminare.      |
| `PutMessage`/`PutMessageAsync`       | INVIARE un messaggio da inserire.         |
| `PatchMessage`/`PatchMessageAsync`   | INVIARE un messaggio alla patch.       |

&#8224; Utilizzato per le chiamate API per la pagina.

Per stabilire questo schema, ereditare il `DefaultPageApplicationModelProvider` classe ed eseguire l'override di [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) metodo per fornire la logica personalizzata per la risoluzione [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) i nomi dei gestori. L'app di esempio viene illustrato come questa operazione viene eseguita relativo `CustomPageApplicationModelProvider` classe:

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Caratteristiche della classe:

* La classe eredita da `DefaultPageApplicationModelProvider`.
* Il `TryParseHandlerMethod` elabora un gestore per determinare il verbo HTTP (`httpMethod`) e nome del gestore denominata (`handlerName`) quando si crea il `PageHandlerModel`.
  * Un `Async` suffisso viene ignorato, se presente.
  * Maiuscole e minuscole viene usata per analizzare il verbo HTTP dal nome del metodo.
  * Quando il nome del metodo (senza `Async`) è uguale al nome del verbo HTTP, non vi è alcun gestore denominata. Il `handlerName` è impostato su `null`, ed è il nome del metodo `Get`, `Post`, `Delete`, `Put`, o `Patch`.
  * Quando il nome del metodo (senza `Async`) è più lungo rispetto al nome del verbo HTTP, non vi è un gestore denominato. `handlerName` viene impostato su `<method name (less 'Async', if present)>`. Ad esempio, un nome di gestore di "GetMessage" rendimento sia "GetMessage" e "GetMessageAsync".
  * Verbi PATCH HTTP, PUT e DELETE vengono convertiti in POST.

Registrare il `CustomPageApplicationModelProvider` nel `Startup` classe:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

Il file code-behind *Index.cshtml.cs* viene illustrato come le convenzioni di denominazione del metodo normale gestore vengono modificate per le pagine nell'app. Le normali "On" denominazione di prefisso utilizzata con pagine Razor viene rimosso. Il metodo che inizializza lo stato della pagina è ora denominato `Get`. È possibile visualizzare questa convenzione utilizzata nell'intera app se si apre un file code-behind per una delle pagine.

Ognuno degli altri metodi iniziare con il verbo HTTP che descrive l'elaborazione. I due metodi che iniziano con `Delete` normalmente verrebbe considerato come verbi HTTP DELETE, ma la logica `TryParseHandlerMethod` imposta in modo esplicito il verbo POST per entrambi i gestori.

Si noti che `Async` è facoltativo tra `DeleteAllMessages` e `DeleteMessageAsync`. In entrambi i metodi asincroni, ma è possibile scegliere di utilizzare il `Async` o non in forma suffissa; si consiglia di procedere. `DeleteAllMessages`è qui usato a scopo dimostrativo, ma è consigliabile assegnare un nome tale metodo `DeleteAllMessagesAsync`. Non influisce l'elaborazione di implementazione dell'esempio ma utilizza il `Async` alle chiamate il fatto che è un metodo asincrono in forma suffissa.

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Notare i nomi dei gestori forniti in *cshtml* corrisponde la `DeleteAllMessages` e `DeleteMessageAsync` i metodi del gestore:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async`nel nome del metodo di gestore `DeleteMessageAsync` sono inseriti la disconnessione dal `TryParseHandlerMethod` per la corrispondenza della richiesta POST al metodo del gestore. Il `asp-page-handler` nome di `DeleteMessage` corrisponde al metodo del gestore `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtri MVC e il filtro di pagina (IPageFilter)

MVC [filtri azione](xref:mvc/controllers/filters#action-filters) vengono ignorate da pagine Razor, poiché le pagine Razor utilizzare i metodi del gestore. Sono disponibili per l'utilizzo di altri tipi di filtri MVC: [autorizzazione](xref:mvc/controllers/filters#authorization-filters), [eccezione](xref:mvc/controllers/filters#exception-filters), [risorse](xref:mvc/controllers/filters#resource-filters), e [risultato](xref:mvc/controllers/filters#result-filters). Per ulteriori informazioni, vedere il [filtri](xref:mvc/controllers/filters) argomento.

Il filtro di pagina ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) è un filtro che si applica a pagine Razor. Racchiuso tra l'esecuzione di un metodo del gestore di pagina. Consente di elaborare il codice personalizzato nelle fasi di esecuzione del metodo di gestore di pagina. Di seguito è riportato un esempio delle app di esempio:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

Questo filtro verifica la presenza di un `globalTemplate` distribuire "ReplacementValue" valore di "TriggerValue" e lo scambio.

Il `ReplaceRouteValueFilter` attributo può essere applicato direttamente a un `PageModel` nel code-behind:

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

Richiedere la pagina Page3 dall'applicazione di esempio con in `localhost:5000/OtherPages/Page3/TriggerValue`. Si noti come il filtro sostituisce il valore di route:

![La richiesta di OtherPages/Page3 ottenendo un TriggerValue route segmento nel filtro sostituendo il valore di route con ReplacementValue.](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>Vedere anche

* [Convenzioni di autorizzazione pagine Razor](xref:security/authorization/razor-pages-authorization)
