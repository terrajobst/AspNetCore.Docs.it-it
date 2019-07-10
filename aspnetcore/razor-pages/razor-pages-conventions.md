---
title: Convenzioni di route e app per Razor Pages in ASP.NET Core
author: guardrex
description: Informazioni su come le convenzioni di route e del provider di modello di app consentono di controllare routing, individuazione ed elaborazione delle pagine.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 59c8af648b50deb51f3762c14348d08acd48886e
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724455"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Convenzioni di route e app per Razor Pages in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Informazioni su come usare le [convenzioni di route e del provider di modello di app](xref:mvc/controllers/application-model#conventions) della pagina per controllare routing, individuazione ed elaborazione nelle app Razor Pages.

Quando è necessario configurare la route di una pagina personalizzata per le singole pagine, configurare il routing alle pagine con la [convenzione AddPageRoute](#configure-a-page-route) descritta più avanti in questo argomento.

Per specificare una route di pagina, aggiungere segmenti di route o aggiungere parametri a una route, usare la pagina `@page` direttiva. Per altre informazioni, vedere [route personalizzata](xref:razor-pages/index#custom-routes).

Sono presenti parole riservate non possono essere usati come segmenti di route o i nomi dei parametri. Per altre informazioni, vedere [Routing: Routing nomi riservati](xref:fundamentals/routing#reserved-routing-names).

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))

| Scenario | L'esempio illustra come eseguire le seguenti operazioni: |
| -------- | --------------------------- |
| [Convenzioni del modello](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Aggiungere un modello e un'intestazione di route alle pagine di un'app. |
| [Convenzioni per le azioni di route di pagina](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Aggiungere un modello di route alle pagine in una cartella e a una pagina singola. |
| [Convenzioni per le azioni del modello di pagina](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (classe di filtro, espressione lambda o factory di filtro)</li></ul> | Aggiungere un'intestazione alle pagine in una cartella, aggiungere un'intestazione a una pagina singola e configurare una [factory di filtro](xref:mvc/controllers/filters#ifilterfactory) per aggiungere un'intestazione alle pagine di un'app. |

Convenzioni di pagine Razor vengono aggiunti e configurati tramite il <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> nella raccolta di servizio nel `Startup` classe. Gli esempi di convenzione seguenti sono illustrati più avanti in questo argomento:

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

## <a name="route-order"></a>Ordine della route

Le route specificano un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per l'elaborazione (route corrispondente).

| Ordinamento            | Comportamento |
| :--------------: | -------- |
| -1               | La route viene elaborata prima che vengano elaborate altre route. |
| 0                | Non è specificato l'ordine (valore predefinito). Non assegnare `Order` (`Order = null`) per impostazione predefinita la route `Order` su 0 (zero) per l'elaborazione. |
| 1, 2, &hellip; n | Specifica l'ordine di elaborazione delle route. |

Per convenzione, viene stabilita l'elaborazione delle route:

* Le route vengono elaborate in ordine sequenziale (-1, 0, 1, 2, &hellip; n).
* Quando le route hanno gli stessi `Order`, più route specifica esiste una corrispondenza per prime, seguite dalle route meno specifiche.
* Quando le route con lo stesso `Order` e lo stesso numero di parametri corrisponde un URL della richiesta, le route vengono elaborate nell'ordine in cui vengono aggiunti al <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Evitare se possibile, a seconda di un ordine di elaborazione delle route stabilita. In genere, il routing seleziona la route corretta con un URL corrispondente. Se è necessario impostare route `Order` le proprietà per instradare le richieste correttamente, lo schema di routing dell'app è probabilmente creare confusione ai client e fragile mantenere. Cercare di semplificare lo schema di routing dell'app. L'app di esempio richiede una route esplicita l'elaborazione dell'ordine per illustrare diversi scenari di routing usando una singola app. Tuttavia, è consigliabile tentare di evitare la pratica di route impostazione `Order` nelle app di produzione.

Il routing di Razor Pages e il routing del controller MVC condividono un'implementazione. Informazioni sull'ordine della route negli argomenti di MVC sono disponibile all'indirizzo [Routing ad azioni del controller: Ordinamento delle route con attributi](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Convenzioni del modello

Aggiungere un delegato per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> per aggiungere [modellare convenzioni](xref:mvc/controllers/application-model#conventions) che si applicano a Razor Pages.

### <a name="add-a-route-model-convention-to-all-pages"></a>Aggiungere una convenzione del modello di route a tutte le pagine

Uso <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> alla raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> costruzione del modello istanze che vengono applicate durante la route di pagina.

L'app di esempio aggiunge un modello di route `{globalTemplate?}` a tutte le pagine nell'app:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

La proprietà <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> è impostata su `1`. In questo modo la route seguente comportamento nell'app di esempio di corrispondenza:

* Un modello di route per `TheContactPage/{text?}` viene aggiunto più avanti in questo argomento. La route di pagina di contatto dispone di un ordine predefinito `null` (`Order = 0`), in modo che corrisponda alla prima il `{globalTemplate?}` del modello di route.
* Un `{aboutTemplate?}` del modello di route viene aggiunto più avanti in questo argomento. Al modello `{aboutTemplate?}` viene assegnato un `Order` di `2`. Quando viene richiesta la pagina di informazioni in `/About/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 1`) e non in `RouteData.Values["aboutTemplate"]` (`Order = 2`) a causa dell'impostazione della proprietà `Order`.
* Un `{otherPagesTemplate?}` del modello di route viene aggiunto più avanti in questo argomento. Al modello `{otherPagesTemplate?}` viene assegnato un `Order` di `2`. Quando una o più pagine nel *Pages/OtherPages* cartella viene richiesta con un parametro di route (ad esempio, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 1`) e non `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) a causa dell'impostazione di `Order` proprietà.

Laddove possibile, non vengono impostate le `Order`, in modo da `Order = 0`. Si basano sul routing per selezionare la route corretta.

Opzioni di pagine Razor, ad esempio l'aggiunta <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, vengono aggiunti quando MVC viene aggiunto alla raccolta di servizio nel `Startup.ConfigureServices`. Per un esempio completo, vedere [l'app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About/GlobalRouteValue` ed esaminare il risultato:

![La pagina di informazioni viene richiesta con un segmento di route di GlobalRouteValue. La pagina sottoposta a rendering indica che il valore di dati della route viene acquisito nel metodo OnGet della pagina.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Aggiungere una convenzione del modello di app per tutte le pagine

Uso <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> alla raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> istanze che vengono applicate durante la costruzione del modello di app di pagina.

Per dimostrare questa e altre convenzioni più avanti in questo argomento, l'app di esempio include una classe `AddHeaderAttribute`. Il costruttore della classe accetta una stringa `name` e una matrice di stringhe `values`. Questi valori vengono usati nel relativo metodo `OnResultExecuting` per impostare un'intestazione di risposta. La classe completa è illustrata nella sezione [Convenzioni per le azioni del modello di pagina](#page-model-action-conventions) più avanti in questo argomento.

L'app di esempio usa la classe `AddHeaderAttribute` per aggiungere un'intestazione, `GlobalHeader`, a tutte le pagine nell'app:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:

![Le intestazioni di risposta della pagina di informazioni indicano che è stato aggiunto l'elemento GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Aggiungere una convenzione del modello di gestore a tutte le pagine

Uso <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> alla raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> istanze che vengono applicate durante la costruzione del modello di gestore di pagina.

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>Convenzioni per le azioni di route di pagina

Il provider di modello di route predefinito che deriva da <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> chiama le convenzioni progettate per offrire punti di estendibilità per la configurazione delle route di pagina.

### <a name="folder-route-model-convention"></a>Convenzione del modello di route cartella

Uso <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> che chiama un'azione su di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> per tutte le pagine nella cartella specificata.

L'app di esempio usa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> per aggiungere un modello di route `{otherPagesTemplate?}` alle pagine nella cartella *OtherPages*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

La proprietà <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> è impostata su `2`. Ciò garantisce che il modello per `{globalTemplate?}` (in precedenza nell'argomento consentono di impostare `1`) viene assegnata la priorità per i dati della route prima posizione del valore quando viene specificato un valore singolo di route. Se una pagina nel *Pages/OtherPages* cartella viene richiesta con un valore di parametro di route (ad esempio, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 1`) e non `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) a causa dell'impostazione di `Order` proprietà.

Laddove possibile, non vengono impostate le `Order`, in modo da `Order = 0`. Si basano sul routing per selezionare la route corretta.

Richiedere la pagina Page1 dell'esempio in `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` ed esaminare il risultato:

![La pagina Page1 nella cartella OtherPages viene richiesta con un segmento di route di GlobalRouteValue e OtherPagesRouteValue. La pagina sottoposta a rendering indica che i valori di dati della route vengono acquisiti nel metodo OnGet della pagina.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Convenzione del modello di route pagina

Uso <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> che chiama un'azione su di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> per la pagina con il nome specificato.

L'app di esempio usa `AddPageRouteModelConvention` per aggiungere un modello di route `{aboutTemplate?}` alla pagina di informazioni:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

La proprietà <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> è impostata su `2`. Ciò garantisce che il modello per `{globalTemplate?}` (in precedenza nell'argomento consentono di impostare `1`) viene assegnata la priorità per i dati della route prima posizione del valore quando viene specificato un valore singolo di route. Se la pagina di informazioni viene richiesta con un valore di parametro di route in corrispondenza `/About/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 1`) e non `RouteData.Values["aboutTemplate"]` (`Order = 2`) a causa dell'impostazione di `Order` proprietà.

Laddove possibile, non vengono impostate le `Order`, in modo da `Order = 0`. Si basano sul routing per selezionare la route corretta.

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About/GlobalRouteValue/AboutRouteValue` ed esaminare il risultato:

![La pagina di informazioni viene richiesta con segmenti di route per GlobalRouteValue e AboutRouteValue. La pagina sottoposta a rendering indica che i valori di dati della route vengono acquisiti nel metodo OnGet della pagina.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Usare un trasformatore di parametri per personalizzare le route di pagina

Le route di pagina generate da ASP.NET Core possono essere personalizzate usando un trasformatore di parametro. Un trasformatore di parametri implementa `IOutboundParameterTransformer` e trasforma il valore dei parametri. Ad esempio, un trasformatore di parametri `SlugifyParameterTransformer` personalizzato cambia il valore di route `SubscriptionManagement` in `subscription-management`.

Il `PageRouteTransformerConvention` convenzione del modello di route pagina un trasformatore parametro si applica ai segmenti di nome file e cartelle delle route di pagina generata automaticamente in un'app. Ad esempio, le pagine Razor file alla */Pages/SubscriptionManagement/ViewAll.cshtml* avrebbe relativa route riscrivere `/SubscriptionManagement/ViewAll` a `/subscription-management/view-all`.

`PageRouteTransformerConvention` Trasforma solo segmenti generati automaticamente di una route di pagina provenienti dalla cartella di Razor Pages e nome file. Non trasforma i segmenti di route aggiunti con il `@page` direttiva. La convenzione anche non trasforma route aggiunte da <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.

Il `PageRouteTransformerConvention` viene registrata come un'opzione in `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add(
                    new PageRouteTransformerConvention(
                        new SlugifyParameterTransformer()));
            });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

## <a name="configure-a-page-route"></a>Configurare una route di pagina

Usare <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> per configurare una route a una pagina nel percorso di pagina specificato. I collegamenti generati alla pagina usano la route specificata. `AddPageRoute` usa `AddPageRouteModelConvention` per stabilire la route.

L'app di esempio crea una route a `/TheContactPage` per *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

La pagina di contatto può anche essere raggiunta in corrispondenza di `/Contact` tramite la route predefinita.

La route personalizzata dell'app di esempio alla pagina di contatto consente un segmento di route `text` facoltativo (`{text?}`). La pagina include anche tale segmento facoltativo nella relativa istruzione `@page` nel caso in cui il visitatore acceda alla pagina in corrispondenza della relativa route `/Contact`:

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

Si noti che l'URL generato per il collegamento **Contatto** nella pagina sottoposta a rendering riflette la route aggiornata:

![Collegamento alla pagina di contatto dell'app di esempio nella barra di spostamento](razor-pages-conventions/_static/contact-link.png)

![L'esame del collegamento alla pagina di contatto nell'HTML sottoposto a rendering indica che href è impostato su '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

Visitare la pagina di contatto nella route normale, `/Contact`, o nella route personalizzata `/TheContactPage`. Se si specifica un segmento di route `text` aggiuntivo, la pagina visualizza il segmento codificato in formato HTML specificato dall'utente:

![Esempio di browser Microsoft Edge in cui viene specificato un segmento di route facoltativo 'text' corrispondente a 'TextValue' nell'URL. La pagina sottoposta a rendering visualizza il valore del segmento 'text'.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenzioni per le azioni del modello di pagina

Il provider di modello di pagina predefinito che implementa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> chiama le convenzioni progettate per offrire punti di estendibilità per la configurazione di modelli di pagina. Queste convenzioni sono utili durante la compilazione e la modifica degli scenari di individuazione ed elaborazione delle pagine.

Per gli esempi in questa sezione, l'app di esempio Usa un' `AddHeaderAttribute` classe, ovvero un <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, che applica un'intestazione di risposta:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

Tramite le convenzioni, nell'esempio viene illustrato come applicare l'attributo a tutte le pagine in una cartella e a una singola pagina.

**Convenzione per il modello di app cartella**

Uso <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> che chiama un'azione su <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> istanze per tutte le pagine nella cartella specificata.

Nell'esempio viene illustrato l'uso di `AddFolderApplicationModelConvention` aggiungendo un'intestazione, `OtherPagesHeader`, alle pagine all'interno della cartella *OtherPages* dell'app:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

Richiedere la pagina Page1 dell'esempio in `localhost:5000/OtherPages/Page1` ed esaminare le intestazioni per visualizzare il risultato:

![Le intestazioni di risposta della pagina OtherPages/Page1 indicano che è stato aggiunto l'elemento OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Convenzione per il modello di app cartella**

Uso <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> che chiama un'azione su di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> per la pagina con il nome specificato.

Nell'esempio viene illustrato l'uso di `AddPageApplicationModelConvention` aggiungendo un'intestazione, `AboutHeader`, alla pagina di informazioni:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:

![Le intestazioni di risposta della pagina di informazioni indicano che è stato aggiunto l'elemento AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

**Configurare un filtro**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Consente di configurare il filtro specificato da applicare. È possibile implementare una classe di filtro, ma l'app di esempio illustra come implementare un filtro in un'espressione lambda, che viene implementata in background come una factory che restituisce un filtro:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

Viene usato il modello di app della pagina per verificare il percorso relativo per i segmenti che portano alla pagina Page2 nella cartella *OtherPages*. Se la condizione viene superata, viene aggiunta un'intestazione. In caso contrario, viene applicato `EmptyFilter`.

`EmptyFilter` è un [filtro azione](xref:mvc/controllers/filters#action-filters). Poiché i filtri azione vengono ignorati da Razor Pages, il `EmptyFilter` come previsto se non contiene il percorso non ha alcun effetto `OtherPages/Page2`.

Richiedere la pagina Page2 dell'esempio in `localhost:5000/OtherPages/Page2` ed esaminare le intestazioni per visualizzare il risultato:

![L'elemento OtherPagesPage2Header viene aggiunto alla risposta per Page2.](razor-pages-conventions/_static/page2-filter-header.png)

**Configurare una factory di filtro**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Configura la factory specificata per applicare [filtri](xref:mvc/controllers/filters) a tutte le pagine Razor.

L'app di esempio illustra un esempio dell'uso di una [factory di filtro](xref:mvc/controllers/filters#ifilterfactory) aggiungendo un'intestazione, `FilterFactoryHeader`, con due valori per le pagine dell'app:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:

![Le intestazioni di risposta della pagina informazioni indicano che sono state aggiunte due intestazioni FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtri MVC e il filtro di pagina (IPageFilter)

I [filtri azione](xref:mvc/controllers/filters#action-filters) MVC vengono ignorati da Razor Pages, poiché vengono usati i metodi del gestore. Sono disponibili è possibile utilizzare altri tipi di filtri MVC: [Authorization](xref:mvc/controllers/filters#authorization-filters), [eccezioni](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), e [risultato](xref:mvc/controllers/filters#result-filters). Per altre informazioni, vedere l'argomento [Filtri](xref:mvc/controllers/filters).

Il filtro di pagina (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) è un filtro che si applica a Razor Pages. Per altre informazioni, vedere [Modalità di filtro per pagine Razor](xref:razor-pages/filter).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
