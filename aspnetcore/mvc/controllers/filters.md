---
title: Filtri in ASP.NET Core
author: ardalis
description: Informazioni su come funzionano i filtri e su come usarli in ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2018
uid: mvc/controllers/filters
ms.openlocfilehash: d25976af860ed8733c0548a4102e0095969b2ba6
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861849"
---
# <a name="filters-in-aspnet-core"></a>Filtri in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)

I *filtri* in ASP.NET Core MVC consentono di eseguire codice prima o dopo fasi specifiche della pipeline di elaborazione della richiesta.

> [!IMPORTANT]
> Questo argomento **non** si applica a Razor Pages. ASP.NET Core 2.1 e le versioni successive supportano [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) per Razor Pages. Per altre informazioni, vedere [Modalità di filtro per pagine Razor](xref:razor-pages/filter).

 I filtri predefiniti gestiscono attività, ad esempio:

 * Autorizzazione (impedire l'accesso alle risorse per cui un utente non è autorizzato).
 * Assicurarsi che tutte le richieste utilizzino HTTPS.
 * Memorizzazione nella cache delle risposte (blocco della pipeline delle richieste per restituire una risposta memorizzata nella cache). 

I filtri personalizzati possono essere creati per gestire problemi relativi a più settori. I filtri possono evitare la duplicazione di codice tra le azioni. Ad esempio, un filtro eccezioni per la gestione degli errori potrebbe consolidare la gestione degli errori.

[Visualizzare o scaricare un esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Funzionamento dei filtri

I filtri vengono eseguiti all'interno della *pipeline della chiamata di azione MVC*, talvolta definita la *pipeline filtro*.  La pipeline filtro viene eseguita dopo che MVC ha selezionato l'azione da eseguire.

![La richiesta viene elaborata tramite altro middleware, il middleware di routing, la selezione azione e la pipeline della chiamata di azione MVC. L'elaborazione della richiesta torna alla selezione azione, al middleware di routing e a diversi altri middleware prima di diventare la risposta che viene inviata al client.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Tipi di filtro

Ogni tipo di filtro viene eseguito in una fase diversa della pipeline filtro.

* I [filtri autorizzazione](#authorization-filters) vengono eseguiti per primi e consentono di determinare se l'utente corrente è autorizzato per la richiesta. Possono bloccare la pipeline se una richiesta non è autorizzata. 

* I [filtri risorse](#resource-filters) sono i primi a gestire una richiesta dopo l'autorizzazione.  Possono eseguire codice prima del resto della pipeline filtro e dopo che il resto della pipeline è stato completato. Risultano utili per implementare la memorizzazione nella cache o in caso contrario per bloccare la pipeline filtro per motivi di prestazioni. Vengono eseguiti prima dell'associazione di modelli, pertanto possono influenzare l'associazione di modelli.

* I [filtri azione](#action-filters) possono eseguire codice immediatamente prima e dopo la chiamata di un metodo di azione singolo. Possono essere usati per modificare gli argomenti passati a un'azione e il risultato restituito dall'azione.

* I [filtri eccezioni](#exception-filters) vengono usati per applicare i criteri globali a eccezioni non gestite che si verificano prima che qualsiasi elemento sia stato scritto nel corpo della risposta.

* I [filtri risultato](#result-filters) possono eseguire codice immediatamente prima e dopo l'esecuzione di risultati di azioni singole. Vengono eseguiti solo quando il metodo di azione è stato eseguito correttamente. Sono utili per la logica che deve racchiudere l'esecuzione del formattatore o di una vista.

Il diagramma seguente illustra come i tipi di filtro interagiscono nella pipeline filtro.

![La richiesta passa attraverso i filtri autorizzazione, i filtri risorse, l'associazione di modelli, i filtri azione, l'esecuzione dell'azione e la conversione del risultato dell'azione, i filtri eccezioni, i filtri risultato e l'esecuzione del risultato. All'uscita della pipeline, la richiesta viene elaborata solo dai filtri risultato e dai filtri risorse prima di diventare una risposta inviata al client.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementazione

I filtri supportano sia implementazioni sincrone che asincrone tramite definizioni di interfaccia diverse. 

I filtri sincroni che possono eseguire codice sia prima che dopo la pipeline definiscono i metodi On*fase*Executing e On*fase*Executed. Ad esempio, `OnActionExecuting` viene chiamato prima che venga chiamato il metodo di azione, e `OnActionExecuted` viene chiamato dopo la restituzione del metodo di azione.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

I filtri asincroni definiscono un singolo metodo On*fase*ExecutionAsync. Questo metodo accetta un delegato *TipoFiltro*ExecutionDelegate che esegue la fase della pipeline filtro. Ad esempio, `ActionExecutionDelegate` chiama il metodo di azione o il filtro di azione successivo ed è possibile eseguire codice sia prima che dopo la chiamata del metodo.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

È possibile implementare interfacce per più fasi di filtro in una singola classe. Ad esempio, la classe [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) implementa `IActionFilter`, `IResultFilter` nonché i relativi equivalenti asincroni.

> [!NOTE]
> Implementare la versione sincrona **o** la versione asincrona di un'interfaccia di filtro, non entrambe. Il framework controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama. In caso contrario, chiama i metodi dell'interfaccia sincrona. Se si implementano entrambe le interfacce in una classe, viene chiamato solo il metodo asincrono. Quando si usano classi astratte come [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) viene eseguito l'override solo dei metodi sincroni o del metodo asincrono per ogni tipo di filtro.

### <a name="ifilterfactory"></a>IFilterFactory

[IFilterFactory](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory) implementa [IFilterMetadata](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifiltermetadata). Pertanto, un'istanza di `IFilterFactory` può essere usata come un'istanza di `IFilterMetadata` in un punto qualsiasi della pipeline filtro. Quando il framework si prepara per richiamare il filtro, tenta di eseguirne il cast su un'istanza di `IFilterFactory`. Se l'esecuzione del cast ha esito positivo, viene chiamato il metodo [CreateInstance](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory.createinstance) per creare l'istanza di `IFilterMetadata` che verrà richiamata. In questo modo viene specificata una struttura flessibile, poiché non è necessario impostare in modo esplicito la pipeline filtro all'avvio dell'app.

Un altro approccio alla creazione di filtri consiste nell'implementare `IFilterFactory` nelle implementazioni dell'attributo:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Attributi filtro predefiniti

Il framework include filtri basati su attributi predefiniti che è possibile creare in una sottoclasse e personalizzare. Ad esempio, il filtro risultato seguente aggiunge un'intestazione alla risposta.

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Gli attributi consentono ai filtri di accettare argomenti, come illustrato nell'esempio precedente. Questo attributo può essere aggiunto a un controller o a un metodo di azione ed è possibile specificare il nome e il valore dell'intestazione HTTP:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Il risultato dell'azione `Index` viene illustrato in seguito. Le intestazioni della risposta sono visualizzate in basso a destra.

![Strumenti di sviluppo di Microsoft Edge che illustrano le intestazioni della risposta, tra cui l'autore Steve Smith @ardalis](filters/_static/add-header.png)

Molte delle interfacce del filtro presentano attributi corrispondenti che possono essere usati come classi di base per implementazioni personalizzate.

Attributi dei filtri:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

Gli attributi `TypeFilterAttribute` e `ServiceFilterAttribute` sono descritti [più avanti nell'articolo](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Ambiti dei filtri e ordine di esecuzione

È possibile aggiungere un filtro alla pipeline in uno dei tre *ambiti*. È possibile aggiungere un filtro a un metodo di azione particolare o a una classe controller usando un attributo. Oppure è possibile registrare un filtro a livello globale per tutti i controller e le azioni. I filtri sono aggiunti a livello globale quando vengono aggiunti alla raccolta `MvcOptions.Filters` in `ConfigureServices`:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Ordine di esecuzione predefinito

Quando sono presenti più filtri per una particolare fase della pipeline, l'ambito determina l'ordine di esecuzione predefinito dei filtri.  I filtri globali racchiudono i filtri di classe, che a loro volta racchiudono i filtri dei metodi. Questo tipo di nidificazione, in cui ogni ambito è racchiuso da un altro, a volte viene definito a [Matrioska](https://wikipedia.org/wiki/Matryoshka_doll). In genere è possibile ottenere il comportamento di override che si vuole senza dover determinare in modo esplicito l'ordinamento.

In seguito a questa nidificazione, il codice *after* dei filtri viene eseguito in ordine inverso rispetto al codice *before*. La sequenza è simile alla seguente:

* Il codice *before* dei filtri applicati a livello globale
  * Il codice *before* dei filtri applicati ai controller
    * Il codice *before* dei filtri applicati ai metodi di azione
    * Il codice *after* dei filtri applicati ai metodi di azione
  * Il codice *after* dei filtri applicati ai controller
* Il codice *after* dei filtri applicati a livello globale
  
Di seguito è riportato un esempio che illustra l'ordine nel quale i metodi dei filtri vengono chiamati per i filtri di azione sincroni.

| Sequence | Ambito del filtro | Metodo del filtro |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Controller | `OnActionExecuting` |
| 3 | Metodo | `OnActionExecuting` |
| 4 | Metodo | `OnActionExecuted` |
| 5 | Controller | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Questa sequenza mostra che:

* Il filtro del metodo è annidato all'interno del filtro del controller.
* Il filtro del controller è annidato all'interno del filtro globale. 

In altre parole, se si è all'interno di un metodo On*fase*ExecutionAsync di un filtro asincrono, tutti i filtri con un ambito più ridotto vengono eseguiti mentre il codice è nello stack.

> [!NOTE]
> Ogni controller che eredita dalla classe di base `Controller` include i metodi `OnActionExecuting` e `OnActionExecuted`. Tali metodi racchiudono i filtri che vengono eseguiti per una determinata azione: `OnActionExecuting` viene chiamato prima di tutti i filtri, `OnActionExecuted` viene chiamato dopo tutti i filtri.

### <a name="overriding-the-default-order"></a>Override dell'ordine predefinito

È possibile eseguire l'override della sequenza di esecuzione predefinita implementando `IOrderedFilter`. Questa interfaccia espone una proprietà `Order` che ha la precedenza sull'ambito per determinare l'ordine di esecuzione. Un filtro con un valore `Order` più basso avrà il relativo codice *before* eseguito prima di quello di un filtro con un valore di `Order` più alto. Un filtro con un valore di `Order` più basso avrà il relativo codice *after* eseguito dopo quello di un filtro con un valore di `Order` più alto. È possibile impostare la proprietà `Order` tramite un parametro di costruttore:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Se si hanno gli stessi tre filtri azione illustrati nell'esempio precedente ma la proprietà `Order` del controllo e dei filtri globali impostata rispettivamente su 1 e 2, l'ordine di esecuzione viene rovesciato.

| Sequence | Ambito del filtro | Proprietà`Order`  | Metodo del filtro |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Metodo | 0 | `OnActionExecuting` |
| 2 | Controller | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Controller | 1  | `OnActionExecuted` |
| 6 | Metodo | 0  | `OnActionExecuted` |

La proprietà `Order` prevale sull'ambito nel determinare l'ordine in cui verranno eseguiti i filtri. I filtri vengono ordinati prima in base all'ordine, poi viene usato l'ambito per interrompere i collegamenti. Tutti i filtri predefiniti implementano `IOrderedFilter` e impostano il valore predefinito di `Order` su 0. Per i filtri predefiniti l'ambito determina l'ordine, a meno che non si imposti `Order` su un valore diverso da zero.

## <a name="cancellation-and-short-circuiting"></a>Annullamento e blocco

È possibile bloccare la pipeline filtro in qualsiasi momento impostando la proprietà `Result` sul parametro `context` specificato nel metodo di filtro. Ad esempio, il filtro risorse seguente impedisce l'esecuzione del resto della pipeline.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

Nel codice seguente sia il filtro `ShortCircuitingResourceFilter` che il filtro `AddHeader` hanno come destinazione il metodo di azione `SomeResource`. `ShortCircuitingResourceFilter`:

* Viene eseguito per primo perché è un filtro risorsa e `AddHeader` è un filtro azione.
* Blocca il resto della pipeline.

Pertanto il filtro `AddHeader` non viene mai eseguito per l'azione `SomeResource`. Questo comportamento sarebbe lo stesso se entrambi i filtri venissero applicati a livello di metodo di azione, a condizione che il filtro `ShortCircuitingResourceFilter` venga eseguito prima. `ShortCircuitingResourceFilter` viene eseguito per primo per il tipo di filtro o per l'uso esplicito della proprietà `Order`.

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Inserimento di dipendenze

È possibile aggiungere filtri per tipo o per istanza. Se si aggiunge un'istanza, tale istanza verrà usata per ogni richiesta. Se si aggiunge un tipo, il filtro sarà attivato dal tipo, vale a dire che verrà creata un'istanza per ogni richiesta e che tutte le dipendenze del costruttore verranno popolate dall'[inserimento di dipendenze](../../fundamentals/dependency-injection.md). L'aggiunta di un filtro in base al tipo equivale a `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

I filtri implementati come attributi e aggiunti direttamente alle classi controller o ai metodi di azione non possono avere dipendenze costruttore specificate dall'[inserimento di dipendenze](../../fundamentals/dependency-injection.md). Questo avviene perché quando vengono applicati gli attributi, è necessario che vengano specificati i relativi parametri del costruttore. Questa è una limitazione del funzionamento degli attributi.

Se i filtri hanno dipendenze cui è necessario accedere dall'inserimento di dipendenze, esistono diversi approcci supportati. È possibile applicare il filtro a una classe o a un metodo di azione usando uno degli elementi seguenti:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` implementato nell'attributo

> [!NOTE]
> Una delle dipendenze che può essere utile ottenere dall'inserimento di dipendenze è un logger. Tuttavia, è consigliabile evitare di creare e usare i filtri esclusivamente per scopi di registrazione, poiché è possibile che le [funzionalità di registrazione del framework predefinite](xref:fundamentals/logging/index) offrano già le informazioni necessarie. Se si intende aggiungere la registrazione ai filtri, è consigliabile che riguardi problemi di dominio aziendale o comportamenti specifici del filtro, anziché azioni MVC o altri eventi di framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

I tipi di implementazione del filtro per i servizi sono registrati nell'inserimento di dipendenze. `ServiceFilterAttribute` recupera un'istanza del filtro dall'inserimento di dipendenze. Aggiungere `ServiceFilterAttribute` al contenitore in `Startup.ConfigureServices` e farvi riferimento in un attributo `[ServiceFilter]`:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Quando si usa `ServiceFilterAttribute`, l'impostazione di `IsReusable` indica che l'istanza del filtro *potrebbe* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata. Il framework non offre alcuna garanzia che venga creata una singola istanza del filtro o che il filtro non venga richiesto di nuovo dal contenitore di inserimento delle dipendenze in un momento successivo. Evitare di usare `IsReusable` quando si usa un filtro che dipende da servizi con una durata diversa da singleton.

Se si usa `ServiceFilterAttribute` senza registrare il tipo di filtro, viene generata un'eccezione:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` implementa `IFilterFactory`. `IFilterFactory` espone il metodo `CreateInstance` per creare un'istanza `IFilterMetadata`. Il metodo `CreateInstance` carica il tipo specificato dal contenitore dei servizi (DI).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` è simile a `ServiceFilterAttribute`, ma il relativo tipo non viene risolto direttamente dal contenitore dell'inserimento di dipendenze. Viene creata un'istanza del tipo tramite `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

A causa di questa differenza:

* Non è necessario che i tipi a cui viene fatto riferimento tramite `TypeFilterAttribute` siano già registrati nel contenitore.  Le loro dipendenze vengono evase dal contenitore. 
* In via facoltativa, `TypeFilterAttribute` può anche accettare gli argomenti del costruttore per il tipo.

Quando si usa `TypeFilterAttribute`, l'impostazione di `IsReusable` indica che l'istanza del filtro *potrebbe* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata. Il framework non fornisce alcuna garanzia che venga creata una singola istanza del filtro. Evitare di usare `IsReusable` quando si usa un filtro che dipende da servizi con una durata diversa da singleton.

Nell'esempio riportato di seguito viene illustrato come passare argomenti a un tipo usando `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

### <a name="ifilterfactory-implemented-on-your-attribute"></a>IFilterFactory implementato nell'attributo

Se si dispone di un filtro che:

* Non richiede argomenti.
* Prevede dipendenze del costruttore che devono essere evase dal DI.

Sulle classi e sui metodi è possibile usare il proprio attributo denominato invece di `[TypeFilter(typeof(FilterType))]`. Nel filtro seguente viene illustrato come eseguire tale implementazione:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Il filtro può essere applicato a classi o metodi usando la sintassi `[SampleActionFilter]`, anziché dover usare `[TypeFilter]` o `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtri autorizzazione

*Filtri autorizzazione*:

* Controllano l'accesso ai metodi di azione.
* Sono i primi filtri ad essere eseguiti all'interno della pipeline filtro. 
* Dispongono di un metodo precedente, ma non di un metodo successivo. 

È consigliabile scrivere un filtro autorizzazione personalizzato solo se si sta scrivendo il proprio framework di autorizzazione. Invece di scrivere un filtro personalizzato, configurare criteri di autorizzazione o scrivere criteri di autorizzazione personalizzati. L'implementazione dei filtri predefinita si limita a chiamare il sistema di autorizzazioni.

Non generare eccezioni nei filtri autorizzazione perché l'eccezione non verrebbe gestita in quanto i filtri autorizzazione non gestiscono le eccezioni. È consigliabile emettere una richiesta di verifica quando si verifica un'eccezione.

Altre informazioni sull'[autorizzazione](xref:security/authorization/introduction).

## <a name="resource-filters"></a>Filtri risorse

* Implementano l'interfaccia `IResourceFilter` o `IAsyncResourceFilter`.
* La loro esecuzione racchiude la maggior parte della pipeline filtro. 
* Solo i [filtri autorizzazione](#authorization-filters) vengono eseguiti prima dei filtri risorse.

I filtri risorse sono utili per bloccare la maggior parte dell'esecuzione di una richiesta. Ad esempio, un filtro di memorizzazione nella cache può evitare il resto della pipeline se la risposta è memorizzata nella cache.

Il [filtro risorse di blocco](#short-circuiting-resource-filter) illustrato in precedenza è un esempio di filtro risorse. Un altro esempio è [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

* Impedisce all'associazione di modelli di accedere ai dati del modulo. 
* È utile per i caricamenti di file di grandi dimensioni e se si vuole impedire che il modulo venga letto in memoria.

## <a name="action-filters"></a>Filtri azione

*Filtri azione*:

* Implementano l'interfaccia `IActionFilter` o `IAsyncActionFilter`.
* La loro esecuzione circonda l'esecuzione dei metodi di azione.

Di seguito è riportato un filtro azione di esempio:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) specifica le proprietà seguenti:

* `ActionArguments`: consente di modificare gli input per l'azione.
* `Controller`: consente di modificare l'istanza del controller. 
* `Result`: consente di bloccare l'esecuzione del metodo di azione e i filtri azione successivi. Anche la generazione di un'eccezione impedisce l'esecuzione del metodo di azione e dei filtri successivi, ma ciò viene considerato un errore anziché un risultato positivo.

[ActionExecutingContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) specifica `Controller` e `Result` oltre alle proprietà seguenti:

* `Canceled`: è true se l'esecuzione dell'azione è stata bloccata da un altro filtro.
* `Exception`: è non Null se l'azione o un filtro azione successivo ha generato un'eccezione. Se si imposta la proprietà su Null, l'eccezione viene gestita e la proprietà `Result` viene eseguita come se fosse stata restituita normalmente dal metodo di azione.

Per un oggetto `IAsyncActionFilter`, una chiamata a `ActionExecutionDelegate`:

* Esegue qualsiasi filtro azione successivo e il metodo di azione.
* Restituisce `ActionExecutedContext`. 

Per bloccare l'esecuzione, assegnare `ActionExecutingContext.Result` a un'istanza di risultato e non chiamare `ActionExecutionDelegate`.

Il framework specifica una classe astratta `ActionFilterAttribute` che è possibile includere in una sottoclasse. 

È possibile usare un filtro azione per convalidare lo stato del modello e perché vengano restituiti errori se lo stato non è valido:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

Il metodo `OnActionExecuted` viene eseguito dopo il metodo di azione ed è quindi possibile visualizzare e modificare i risultati dell'azione mediante la proprietà `ActionExecutedContext.Result`. La proprietà `ActionExecutedContext.Canceled` viene impostata su true se l'esecuzione dell'azione è stata bloccata da un altro filtro. La proprietà `ActionExecutedContext.Exception` viene impostata su un valore non Null se l'azione o un filtro azione successivo ha generato un'eccezione. Impostazione di `ActionExecutedContext.Exception` su null:

* Gestisce un'eccezione in modo efficace.
* L'oggetto `ActionExectedContext.Result` viene eseguito come se fosse stato restituito normalmente dal metodo di azione.

## <a name="exception-filters"></a>Filtri eccezioni

I *filtri eccezioni* implementano l'interfaccia `IExceptionFilter` o `IAsyncExceptionFilter`. Possono essere usati per implementare i criteri di gestione degli errori comuni per le app. 

Il filtro eccezioni di esempio seguente usa una visualizzazione errori sviluppatore personalizzata per visualizzare i dettagli sulle eccezioni che si verificano quando l'app è in fase di sviluppo:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Filtri eccezioni:

* Non dispone di eventi precedenti o successivi. 
* Implementano `OnException` o `OnExceptionAsync`. 
* Gestiscono le eccezioni non gestite che si verificano nella creazione del controller, nell'[associazione di modelli](../models/model-binding.md), nei filtri azione o nei metodi di azione. 
* Non rilevano le eccezioni che si verificano nei filtri risorse, nei filtri risultato o nell'esecuzione dei risultati MVC.

Per gestire un'eccezione, impostare la proprietà `ExceptionContext.ExceptionHandled` su true o scrivere una risposta. In questo modo si arresta la propagazione dell'eccezione. Un filtro eccezioni non può modificare un'eccezione in una "operazione riuscita". Solo un filtro azione può farlo.

> [!NOTE]
> In ASP.NET Core 1.1 se si imposta `ExceptionHandled` su true **e** si scrive una risposta, questa non viene inviata. In questo scenario ASP.NET Core 1.0 invia la risposta, mentre ASP.NET Core 1.1.2 torna al comportamento della versione 1.0. Per altre informazioni, vedere il problema [n. 5594](https://github.com/aspnet/Mvc/issues/5594) nel repository GitHub. 

Filtri eccezioni:

* Sono ideali per le eccezioni di registrazione che si verificano all'interno di azioni MVC.
* Non sono flessibili quanto il middleware di gestione degli errori. 

Scegliere il middleware per la gestione delle eccezioni. Usare invece i filtri solo quando è necessario gestire gli errori *in modo diverso* in base all'azione MVC scelta. Ad esempio, l'app può avere metodi di azione sia per gli endpoint dell'API che per le visualizzazioni/HTML. Gli endpoint dell'API possono restituire informazioni sull'errore come JSON, mentre le azioni basate sulla visualizzazione possono restituire una pagina di errore in formato HTML.

L'oggetto `ExceptionFilterAttribute` può essere sottoclassato. 

## <a name="result-filters"></a>Filtri risultato

* Implementano l'interfaccia `IResultFilter` o `IAsyncResultFilter`.
* La loro esecuzione circonda l'esecuzione dei risultati dell'azione. 

Di seguito è riportato un esempio di un filtro risultato che aggiunge un'intestazione HTTP.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Il tipo di risultato eseguito dipende dall'azione in questione. Un'azione MVC che restituisce una visualizzazione include tutte le elaborazioni razor in quanto parte dell'elemento `ViewResult` in esecuzione. Un metodo API può eseguire la serializzazione in quanto parte dell'esecuzione del risultato. Altre informazioni sui [risultati dell'azione](actions.md)

I filtri risultato vengono eseguiti solo per i risultati corretti, ovvero quando l'azione o i filtri azione producono un risultato dell'azione. I filtri risultato non vengono eseguiti quando i filtri eccezioni gestiscono un'eccezione.

Il metodo `OnResultExecuting` può interrompere l'esecuzione del risultato dell'azione e i filtri risultato successivi se si imposta `ResultExecutingContext.Cancel` su true. In questo caso è consigliabile scrivere nell'oggetto risposta per evitare di generare una risposta vuota. La generazione di un'eccezione:

* Impedisce l'esecuzione del risultato dell'azione e dei filtri successivi.
* È considerata un errore anziché un risultato positivo.

Quando viene eseguito il metodo `OnResultExecuted`, molto probabilmente la risposta è stata inviata al client e non può essere più modificata, a meno che non sia stata generata un'eccezione. La proprietà `ResultExecutedContext.Canceled` viene impostata su true se l'esecuzione del risultato dell'azione è stata bloccata da un altro filtro.

La proprietà `ResultExecutedContext.Exception` viene impostata su un valore non Null se il risultato dell'azione o un filtro risultato successivo ha generato un'eccezione. Se si imposta `Exception` su Null l'eccezione viene gestita e si evita che venga generata nuovamente da MVC in un punto successivo della pipeline. Quando si gestisce un'eccezione in un filtro risultato, non è possibile scrivere dati nella risposta. Se il risultato dell'azione genera un'eccezione durante l'esecuzione e le intestazioni sono già state scaricate nel client, non esiste alcun meccanismo affidabile per inviare un codice di errore.

Per un oggetto `IAsyncResultFilter`, una chiamata a `await next` in `ResultExecutionDelegate` esegue tutti i filtri risultato successivi e il risultato dell'azione. Per bloccare questa elaborazione, impostare `ResultExecutingContext.Cancel` su true e non chiamare `ResultExectionDelegate`.

Il framework specifica una classe astratta `ResultFilterAttribute` che è possibile includere in una sottoclasse. La classe [AddHeaderAttribute](#add-header-attribute) illustrata in precedenza è un esempio di un attributo del filtro risultato.

## <a name="using-middleware-in-the-filter-pipeline"></a>Uso di middleware nella pipeline filtro

I filtri risorse funzionano come [middleware](xref:fundamentals/middleware/index) in quanto racchiudono l'esecuzione di tutto ciò che viene dopo nella pipeline. Ma i filtri sono diversi dal middleware in quanto fanno parte di MVC, il che significa che hanno accesso al contesto e ai costrutti MVC.

In ASP.NET Core 1.1 è possibile usare un middleware nella pipeline filtro. Questa scelta è consigliabile se si ha un componente del middleware che richiede l'accesso ai dati di route MVC, o che deve essere eseguito solo per determinati controller o azioni.

Per usare un middleware come filtro, creare un tipo con un metodo `Configure` che specifica il middleware che si vuole inserire nella pipeline filtro. Di seguito è riportato un esempio che usa il middleware di localizzazione per stabilire le impostazioni cultura correnti per una richiesta:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

È quindi possibile usare `MiddlewareFilterAttribute` per eseguire il middleware per un'azione o un controller selezionato o a livello globale:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

I filtri middleware vengono eseguiti nella stessa fase della pipeline filtro come filtri risorse, prima dell'associazione di modelli e dopo il resto della pipeline.

## <a name="next-actions"></a>Azioni successive

Per sperimentare i filtri, [scaricare, testare e modificare l'esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
