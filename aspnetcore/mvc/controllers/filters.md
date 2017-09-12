---
title: Filtri
author: ardalis
description: Informazioni su come * filtri * lavoro e sul loro utilizzo in ASP.NET MVC di base.
keywords: ASP.NET Core, filtri, filtri mvc, filtri azione, i filtri di autorizzazione, filtri delle risorse, filtri dei risultati, i filtri eccezioni
ms.author: tdykstra
manager: wpickett
ms.date: 12/12/2016
ms.topic: article
ms.assetid: 531bda08-aa5b-4471-8f08-96add22c8683
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/filters
ms.openlocfilehash: b96a70a2446cab7b1af9bd689469584868980595
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="filters"></a>Filtri

Da [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)

*I filtri* in ASP.NET MVC di base consentono di eseguire codice prima o dopo determinate fasi della pipeline di elaborazione della richiesta.

 I filtri predefiniti handle di attività, ad esempio l'autorizzazione (impedire l'accesso alle risorse di per che un utente non è autorizzato), assicurando che tutte le richieste di utilizzano HTTPS e risposta, la memorizzazione nella cache (corto circuito la pipeline di richieste per restituire una risposta memorizzata nella cache). 

È possibile creare filtri personalizzati per gestire i problemi di montaggio incrociato per l'applicazione. Ogni volta che si desidera evitare la duplicazione di codice tra le azioni, i filtri sono la soluzione. Ad esempio, è possibile consolidare il codice in un filtro eccezioni di gestione degli errori.

[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Come funzionano i filtri?

I filtri vengono eseguiti all'interno di *pipeline di chiamata di azione MVC*, talvolta definito come il *pipeline filtro*.  La pipeline del filtro viene eseguito dopo MVC consente di selezionare l'azione da eseguire.

![La richiesta viene elaborata tramite l'altro Middleware, Routing Middleware, selezione di azione e la Pipeline di chiamata di azione MVC. L'elaborazione della richiesta continua attraverso la selezione di azione, Routing Middleware e vari Middleware altri prima di diventare una risposta inviata al client.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Tipi di filtro

Ogni tipo di filtro viene eseguita in una fase della pipeline filtro diversi.

* [Filtri di autorizzazione](#authorization-filters) eseguiti per primi e vengono utilizzati per determinare se l'utente corrente è autorizzato per la richiesta corrente. È possibile corto circuito la pipeline se una richiesta non è autorizzata. 

* [Filtri risorse](#resource-filters) sono i primi a gestire una richiesta l'autorizzazione.  È possibile eseguire codice prima il resto della pipeline filtro e dopo che il resto della pipeline è stata completata. Risultano utili per implementare la memorizzazione nella cache o in caso contrario la pipeline di filtro per motivi di prestazioni di corto circuito. Poiché vengono eseguiti prima dell'associazione di modelli, risultano utili per tutto ciò che è necessario per influenzare l'associazione del modello.

* [Filtri azione](#action-filters) può eseguire codice immediatamente prima e dopo la chiamata di un metodo di azione singola. Possono essere utilizzati per manipolare gli argomenti passati a un'azione e il risultato restituito dall'azione.

* [I filtri eccezioni](#exception-filters) vengono utilizzati per applicare i criteri globali per le eccezioni non gestite che si verificano prima di qualsiasi elemento è stato scritto per il corpo della risposta.

* [Risultato filtri](#result-filters) può eseguire codice immediatamente prima e dopo l'esecuzione di risultati di singola azione. Vengono eseguite solo quando il metodo di azione è stata eseguita correttamente e sono utili per la logica che è necessario racchiudere l'esecuzione del formattatore o una vista.

Il diagramma seguente illustra l'interagiscono tra questi tipi di filtro nella pipeline di filtro.

![La richiesta viene elaborata tramite filtri di autorizzazione, filtri delle risorse, associazione di modelli, filtri azione, esecuzione azione e azione di conversione di risultati, i filtri eccezioni, filtri dei risultati e risultati esecuzione. La modalità di timeout, la richiesta viene elaborata solo mediante filtri dei risultati e i filtri di risorsa prima di diventare una risposta inviata al client.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementazione

I filtri supportano implementazioni sincrone e asincrone tramite definizioni di interfaccia diversi. Scegliere variante asincrono o di sincronizzazione a seconda del tipo di attività da eseguire. 

Codice sincroni filtri che è possono eseguire sia prima e dopo la fase della pipeline è definito in*fase*durante l'esecuzione e nella*fase*eseguito metodi. Ad esempio, `OnActionExecuting` viene chiamato prima di chiamare il metodo di azione, e `OnActionExecuted` viene chiamato dopo che il metodo di azione restituisce.

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

Asincrona che definiscono un singolo in*fase*ExecutionAsync metodo. Questo metodo accetta un *FilterType*ExecutionDelegate delegato che esegue una fase della pipeline del filtro. Ad esempio, `ActionExecutionDelegate` chiama il metodo di azione e si possono eseguire codice prima e dopo che è possibile chiamare.

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

È possibile implementare le interfacce per più fasi di filtro in una singola classe. Ad esempio, il [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) implementa una classe astratta sia `IActionFilter` e `IResultFilter`, nonché i relativi equivalenti asincroni.

> [!NOTE]
> Implementare **entrambi** sincroni o la versione asincrona di un'interfaccia di filtro, non entrambi. Il framework controlla innanzitutto se il filtro implementa l'interfaccia asincrona e in tal caso, che chiama. In caso contrario, chiama i metodi dell'interfaccia sincrona. Se fosse necessario implementare entrambe le interfacce in una classe, verrà chiamato solo il metodo asincrono. Quando si utilizzano le classi astratte come [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) si eseguirà l'override di metodi sincroni o il metodo asincrono per ogni tipo di filtro.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` implementa `IFilter`. Pertanto, un `IFilterFactory` istanza può essere utilizzata come un `IFilter` istanza in un punto qualsiasi nella pipeline di filtro. Quando il framework Prepara richiamare il filtro, tenta di eseguirne il cast su un `IFilterFactory`. Se il cast ha esito positivo, il `CreateInstance` metodo viene chiamato per creare il `IFilter` istanza che verrà richiamato. Fornisce una struttura molto flessibile, poiché la pipeline del precisa di filtro non è necessario impostare in modo esplicito all'avvio dell'applicazione.

È possibile implementare `IFilterFactory` nelle implementazioni attributo come un altro approccio per la creazione di filtri:

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Attributi di filtro predefinito

Il framework include i filtri basati su attributi predefiniti che è possibile creare una sottoclasse e personalizzare. Ad esempio, il filtro di risultati seguente aggiunge un'intestazione nella risposta.

<a name=add-header-attribute></a>

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Gli attributi consentono di filtri accettare argomenti, come illustrato nell'esempio precedente. Si potrebbe aggiungere questo attributo a un metodo di azione o controller e specificare il nome e il valore dell'intestazione HTTP:

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Il risultato di `Index` di seguito è riportata l'azione, in basso a destra vengono visualizzate le intestazioni di risposta.

![Strumenti per sviluppatori di Microsoft Edge che mostra le intestazioni di risposta, tra cui autore Steve Smith@ardalis](filters/_static/add-header.png)

Molte delle interfacce del filtro presentano attributi corrispondenti che possono essere usati come classi di base per le implementazioni personalizzate.

Attributi di filtro:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute`e `ServiceFilterAttribute` sono illustrati [più avanti in questo articolo](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Gli ambiti di filtro e l'ordine di esecuzione

Un filtro può essere aggiunto alla pipeline in uno dei tre *ambiti*. È possibile aggiungere un filtro a un metodo di azione particolare o a una classe controller utilizzando un attributo. Oppure è possibile registrare un filtro a livello globale (per tutti i controller e azioni) è necessario il `MvcOptions.Filters` insieme il `ConfigureServices` metodo nel `Startup` classe:

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Ordine di esecuzione predefinito

Quando sono presenti più filtri per una particolare fase della pipeline, l'ambito determina l'ordine di esecuzione di un filtro predefinito.  Filtri globali consente di racchiudere i filtri di classe, che a sua volta, racchiudere il filtro. Si è a volte definito come la nidificazione "Bambola russo", ogni incremento nell'ambito wrapping ambito precedente, ad esempio un [bambola nidificazione](https://wikipedia.org/wiki/Matryoshka_doll). In genere possibile ottenere il comportamento di override desiderato senza la necessità di determinare in modo esplicito l'ordinamento.

In seguito a questa funzione di nidificazione, il *dopo* codice dei filtri viene eseguito in ordine inverso il *prima* codice. La sequenza è simile al seguente:

* Il *prima* codice dei filtri applicati a livello globale
  * Il *prima* codice dei filtri applicati ai controller
    * Il *prima* codice dei filtri applicati ai metodi di azione
    * Il *dopo* codice dei filtri applicati ai metodi di azione
  * Il *dopo* codice dei filtri applicati ai controller
* Il *dopo* codice dei filtri applicati a livello globale
  
Di seguito è riportato un esempio che illustra l'ordine nel quale filtro metodi vengono chiamati per i filtri di azione sincroni.

| Sequence | Ambito di filtro | Filter (metodo) |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Controller | `OnActionExecuting` |
| 3 | Metodo | `OnActionExecuting` |
| 4 | Metodo | `OnActionExecuted` |
| 5 | Controller | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Questa sequenza mostra che il filtro del metodo è annidato all'interno del filtro di controller e il filtro controller viene nidificato all'interno di filtri globali. Inserisce un altro modo, se si è all'interno di un filtro di async*fase*ExecutionAsync (metodo), tutti i filtri con un ambito maggiore eseguire mentre il codice è nello stack.

> [!NOTE]
> Ogni controller da cui eredita il `Controller` classe di base include `OnActionExecuting` e `OnActionExecuted` metodi. Questi metodi eseguono il wrapping dei filtri per una determinata azione esecuzione: `OnActionExecuting` viene chiamato prima che i filtri, e `OnActionExecuted` viene chiamato dopo che tutti i filtri.

### <a name="overriding-the-default-order"></a>L'ordine predefinito viene sottoposto a override

È possibile eseguire l'override della sequenza predefinita di esecuzione implementando `IOrderedFilter`. Questa interfaccia espone un `Order` proprietà ha la precedenza sull'ambito per determinare l'ordine di esecuzione. Un filtro con un basso `Order` valore avrà relativo *prima* codice eseguito prima che di un filtro con un valore più alto di `Order`. Un filtro con un basso `Order` valore avrà relativo *dopo* codice eseguito in seguito di un filtro con un valore più alto `Order` valore. È possibile impostare il `Order` proprietà tramite un parametro di costruttore:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Se si hanno lo stesso 3 filtri azione illustrati nell'esempio precedente ma set il `Order` proprietà globali e del controller di filtri a 1 e 2, rispettivamente, l'ordine di esecuzione devono essere considerate rovesciata.

| Sequence | Ambito di filtro | Proprietà `Order` | Filter (metodo) |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Metodo | 0 | `OnActionExecuting` |
| 2 | Controller | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Controller | 1  | `OnActionExecuted` |
| 6 | Metodo | 0  | `OnActionExecuted` |

Il `Order` proprietà supera l'ambito per determinare l'ordine in cui verranno eseguiti i filtri. I filtri vengono ordinati prima in base all'ordine, quindi l'ambito viene utilizzato per interrompere i collegamenti. Tutti i filtri incorporati implementare `IOrderedFilter` e impostare il valore predefinito `Order` valore su 0, pertanto l'ambito determina l'ordine a meno che non si imposta `Order` su un valore diverso da zero.

## <a name="cancellation-and-short-circuiting"></a>Annullamento e operazioni di corto circuito

È possibile di corto circuito di pipeline filtro in qualsiasi momento impostando la `Result` proprietà il `context` parametro fornito al metodo di filtro. Ad esempio, il filtro di risorse seguente impedisce l'esecuzione il resto della pipeline.

<a name=short-circuiting-resource-filter></a>

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

Nel codice seguente, sia il `ShortCircuitingResourceFilter` e `AddHeader` destinazione filtro il `SomeResource` metodo di azione. Tuttavia, poiché il `ShortCircuitingResourceFilter` viene eseguito per primo (perché è un filtro di risorse e `AddHeader` è un filtro azione) e provoca un corto circuito il resto della pipeline, il `AddHeader` filtro non viene mai eseguita per il `SomeResource` azione. Questo comportamento sarà lo stesso se entrambi i filtri sono stati applicati a livello di metodo di azione, fornito il `ShortCircuitingResourceFilter` è stato eseguito prima (a causa di tipo di filtro, o esplicita utilizzare `Order` proprietà, ad esempio).

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Inserimento di dipendenze

È possibile aggiungere filtri per tipo o dall'istanza. Se si aggiunge un'istanza, tale istanza verrà utilizzata per ogni richiesta. Se si aggiunge un tipo, sarà attivato dal tipo, vale a dire verrà creata un'istanza per ogni richiesta e le dipendenze di costruttore verranno popolate dalla [inserimento di dipendenze](../../fundamentals/dependency-injection.md) (DI). Aggiunta di un filtro in base al tipo equivale a `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

I filtri vengono implementati come attributi e aggiunti direttamente al controller classi o metodi di azione non possono avere dipendenze costruttore fornito da [inserimento di dipendenze](../../fundamentals/dependency-injection.md) (DI). Questo avviene perché gli attributi deve essere specificati in cui vengono applicati i parametri del costruttore. Si tratta di una limitazione del funzionano di attributi.

Se i filtri hanno dipendenze che è necessario accedere da DI, esistono diversi approcci supportati. È possibile applicare il filtro a un metodo di classe o un'azione utilizzando uno dei valori seguenti:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory`implementato su attributo

> [!NOTE]
> Una dipendenza di che si potrebbe voler ottenere da è un logger. Tuttavia, evitare di creazione e utilizzo di filtri esclusivamente per scopi di registrazione, poiché il [le funzionalità di registrazione framework incorporato](../../fundamentals/logging.md) possono offrire le informazioni necessarie. Se si intende aggiungere la registrazione per i filtri, consigliabile concentrarsi sul problemi di dominio aziendali o del comportamento specifico per il filtro, anziché azioni MVC o altri eventi di framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Oggetto `ServiceFilter` recupera un'istanza del filtro da DI. Aggiungere il filtro per il contenitore in `ConfigureServices`e di farvi riferimento in un `ServiceFilter` attributo

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Principale](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Utilizzando `ServiceFilter` senza registrare i risultati di tipo di filtro in un'eccezione:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute`implementa `IFilterFactory`, che espone un solo metodo per la creazione di un `IFilter` istanza. In caso di `ServiceFilterAttribute`, `IFilterFactory` dell'interfaccia `CreateInstance` metodo viene implementato per caricare il tipo specificato dal contenitore di servizi (DI).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute`è molto simile a `ServiceFilterAttribute` (e implementa anche `IFilterFactory`), ma il relativo tipo non viene risolto direttamente dal contenitore DI. Al contrario, viene creata un'istanza del tipo tramite `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

A causa di questa differenza, tipi di cui viene fatto riferimento tramite il `TypeFilterAttribute` non è necessario essere registrati con il contenitore (ma hanno ancora le relative dipendenze soddisfatte dal contenitore). Inoltre, `TypeFilterAttribute` può facoltativamente accettare gli argomenti del costruttore per il tipo in questione. Nell'esempio riportato di seguito viene illustrato come passare argomenti a un tipo usando `TypeFilterAttribute`:

[!code-csharp[Principale](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Se si dispone di un filtro che non richiede alcun argomento, ma che dispone di dipendenze di costruttore che devono essere immesse DI, è possibile utilizzare il propria denominato dell'attributo sulle classi e metodi invece degli `[TypeFilter(typeof(FilterType))]`). Il filtro seguente viene illustrato come possono essere implementata:

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Questo filtro può essere applicato alle classi o metodi usando il `[SampleActionFilter]` sintassi, anziché dover usare `[TypeFilter]` o `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtri di autorizzazione

*Filtri di autorizzazione* controllare l'accesso ai metodi di azione e sono i filtri prima di essere eseguita all'interno della pipeline filtro. Hanno solo un prima di metodo, a differenza della maggior parte dei filtri che supportano la prima e dopo i metodi. È consigliabile scrivere solo un filtro di autorizzazione personalizzato se si sta scrivendo un proprio framework di autorizzazione. Scegliere la configurazione dei criteri di autorizzazione o la scrittura di un criterio di autorizzazione personalizzato rispetto alla scrittura di un filtro personalizzato. L'implementazione di filtro predefinito è solo responsabile della chiamata di sistema di autorizzazioni.

Si noti che è non devono generare eccezioni all'interno di filtri di autorizzazione, poiché non gestirà l'eccezione (filtri di eccezioni non gestiscono). Invece, inviare una richiesta o individuare un altro modo.

Altre informazioni, vedere [autorizzazione](../../security/authorization/index.md).

## <a name="resource-filters"></a>Filtri di risorse

*Filtri risorse* implementare il `IResourceFilter` o `IAsyncResourceFilter` interfaccia e la loro esecuzione include la maggior parte della pipeline filtro. (Solo [filtri di autorizzazione](#authorization-filters) eseguire precedute.) I filtri di risorsa sono particolarmente utili se è necessario per la maggior parte del lavoro che sta eseguendo una richiesta di corto circuito. Ad esempio, un filtro di memorizzazione nella cache possibile evitare il resto della pipeline, se la risposta è già nella cache.

Il [breve filtro risorsa corto](#short-circuiting-resource-filter) illustrato in precedenza è un esempio di un filtro di risorse. Un altro esempio è [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), che impediscono l'associazione del modello di accedere ai dati del form. È utile nei casi in cui è noto che si desidera ricevere i caricamenti di file di grandi dimensioni e per impedire che il form viene letto in memoria.

## <a name="action-filters"></a>Filtri azione

*Filtri azione* implementare il `IActionFilter` o `IAsyncActionFilter` interfaccia e la loro esecuzione circonda l'esecuzione dei metodi di azione.

Di seguito è riportato un filtro di azione di esempio:

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

Il [ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) fornisce le proprietà seguenti:

* `ActionArguments`-Consente di modificare gli input per l'azione.
* `Controller`-Consente di modificare l'istanza del controller. 
* `Result`-Se si imposta questo provoca un corto circuito di esecuzione del metodo di azione e i filtri azione successiva. Generare un'eccezione anche impedisce l'esecuzione del metodo di azione e filtri successivi, ma viene considerato come un errore anziché un risultato positivo.

Il [ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) fornisce `Controller` e `Result` inoltre le proprietà seguenti:

* `Canceled`-sarà true se l'esecuzione dell'azione è stata esegue un corto circuito da un altro filtro.
* `Exception`-sarà non null se l'azione o un filtro azioni successive ha generato un'eccezione. Impostazione di questa proprietà su null in modo efficace 'handles' un'eccezione, e `Result` verrà eseguita come se sono stati restituito dal metodo di azione normalmente.

Per un `IAsyncActionFilter`, una chiamata al `ActionExecutionDelegate` esegue qualsiasi filtro azioni successive e il metodo di azione, restituendo un `ActionExecutedContext`. Corto circuito, assegnare `ActionExecutingContext.Result` per un risultato dell'istanza e non si chiama il `ActionExecutionDelegate`.

Il framework fornisce una classe astratta `ActionFilterAttribute` che è possibile creare una sottoclasse. 

È possibile utilizzare un filtro azione automaticamente convalidare lo stato del modello e restituisce eventuali errori, se lo stato è valido:

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

Il `OnActionExecuted` esecuzione del metodo dopo il metodo di azione e possono visualizzare e modificare i risultati dell'azione mediante il `ActionExecutedContext.Result` proprietà. `ActionExecutedContext.Canceled`verrà impostato su true se l'esecuzione dell'azione è stata esegue un corto circuito da un altro filtro. `ActionExecutedContext.Exception`essere imposterà su un valore non null se l'azione o un filtro azioni successive ha generato un'eccezione. Impostazione `ActionExecutedContext.Exception` su null in modo efficace 'handles' un'eccezione, e `ActionExectedContext.Result` quindi essere eseguita come se sono stati restituito dal metodo di azione normalmente.

## <a name="exception-filters"></a>Filtri eccezioni

*I filtri eccezioni* implementare il `IExceptionFilter` o `IAsyncExceptionFilter` interfaccia. Possono essere utilizzati per implementare i criteri per un'app di gestione degli errori comuni. 

Il filtro di eccezione di esempio seguente viene utilizzata un sviluppatore personalizzato errore per visualizzare i dettagli sulle eccezioni che si verificano quando l'applicazione è in fase di sviluppo:

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

I filtri eccezioni non dispongono di due eventi (per prima e dopo aver) - solo implementano `OnException` (o `OnExceptionAsync`). 

I filtri eccezioni gestiscono le eccezioni non gestite che si verificano nella creazione del controller, [associazione del modello](../models/model-binding.md), filtri azione, o i metodi di azione. Non rilevano le eccezioni che si verificano nei filtri delle risorse, i filtri dei risultati o esecuzione risultato MVC.

Per gestire un'eccezione, impostare il `ExceptionContext.ExceptionHandled` proprietà su true o scrivere una risposta. Questo modo si impedisce la propagazione dell'eccezione. Si noti che un filtro eccezioni non è possibile attivare un'eccezione in una "operazione completata". Solo un filtro azione possibile farlo.

> [!NOTE]
> In ASP.NET 1.1, se si imposta la risposta non viene inviata `ExceptionHandled` su true **e** scrivere una risposta. In questo scenario, versione 1.0 di ASP.NET Core invia la risposta e ASP.NET Core 1.1.2 restituirà al 1.0 comportamento. Per ulteriori informazioni, vedere [emettere &#5594;](https://github.com/aspnet/Mvc/issues/5594) nel repository GitHub. 

I filtri eccezioni sono ideali per l'intercettazione delle eccezioni che si verificano all'interno di azioni di MVC, ma non sono quanto più flessibili middleware di gestione degli errori. Preferiti middleware nel caso generale e utilizzare i filtri in cui è necessario solo per eseguire la gestione degli errori *in modo diverso* in base a quale azione MVC è stato scelto. Ad esempio, l'app potrebbe avere metodi di azione per entrambi gli endpoint dell'API e per le viste o HTML. L'endpoint dell'API potrebbe restituire informazioni sull'errore come JSON, mentre le azioni basate su Vista potrebbe restituire un errore di pagina in formato HTML.

Il framework fornisce una classe astratta `ExceptionFilterAttribute` che è possibile creare una sottoclasse. 

## <a name="result-filters"></a>Filtri dei risultati

*Risultato filtri* implementare il `IResultFilter` o `IAsyncResultFilter` interfaccia e la loro esecuzione circonda l'esecuzione di risultati dell'azione. 

Di seguito è riportato un esempio di un filtro dei risultati che aggiunge un'intestazione HTTP.

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Il tipo di risultato eseguita dipende dall'azione in questione. Include tutti razor elaborazione come parte di un'azione MVC restituzione di una vista di `ViewResult` in esecuzione. Un metodo API potrebbe eseguire alcuni serializzazione come parte dell'esecuzione del risultato. Altre informazioni, vedere [risultati dell'azione](actions.md)

Filtri dei risultati vengono eseguiti solo per i risultati corretti - quando l'azione o filtri azione producono un risultato dell'azione. Filtri dei risultati non vengono eseguiti quando i filtri eccezioni gestiscono un'eccezione.

Il `OnResultExecuting` metodo può interrompere l'esecuzione del risultato dell'azione e filtri dei risultati successivi impostando `ResultExecutingContext.Cancel` su true. In genere, è consigliabile scrivere nell'oggetto response quando corto circuito per evitare di generare una risposta vuota. Generare un'eccezione, verrà inoltre impedire l'esecuzione del risultato dell'azione e filtri successivi, ma verrà considerato come un errore anziché un risultato positivo.

Quando il `OnResultExecuted` probabilmente è stato inviato al client di esecuzione del metodo, la risposta e non può essere modificato ulteriormente (a meno che non è stata generata un'eccezione). `ResultExecutedContext.Canceled`verrà impostato su true se l'esecuzione del risultato di azione è stata esegue un corto circuito da un altro filtro.

`ResultExecutedContext.Exception`essere imposterà su un valore non null se il risultato dell'azione o un filtro dei risultati successivi ha generato un'eccezione. Impostazione `Exception` a null gestisce un'eccezione in modo efficace e impedisce la generazione dell'eccezione viene generata nuovamente da MVC in un secondo momento nella pipeline. Quando si sta gestendo un'eccezione in un filtro dei risultati, potrebbe non essere in grado di scrivere i dati alla risposta. Se il risultato dell'azione di genera un'eccezione durante un'esecuzione e le intestazioni sono già state scaricate nel client, non è presente alcun meccanismo affidabile per inviare un codice di errore.

Per un `IAsyncResultFilter` una chiamata a `await next()` sul `ResultExecutionDelegate` esegue eventuali filtri dei risultati successivi e il risultato dell'azione. Corto circuito, impostare `ResultExecutingContext.Cancel` a true e non si chiama il `ResultExectionDelegate`.

Il framework fornisce una classe astratta `ResultFilterAttribute` che è possibile creare una sottoclasse. Il [AddHeaderAttribute](#add-header-attribute) classe illustrato in precedenza è un esempio di un attributo di filtro del risultato.

## <a name="using-middleware-in-the-filter-pipeline"></a>Utilizzo di middleware nella pipeline filtro

Filtri delle risorse di lavoro come [middleware](../../fundamentals/middleware.md) in quanto essi racchiuso l'esecuzione di qualsiasi elemento incluso in un secondo momento nella pipeline. Ma filtri diversi dal middleware in quanto fanno parte di MVC, il che significa che hanno accesso al contesto MVC e costrutti.

In ASP.NET Core 1.1, è possibile utilizzare middleware nella pipeline di filtro. Si consiglia di effettuare che se si dispone di un componente del middleware che richiede l'accesso ai dati di route MVC, o che deve essere eseguito solo per determinati controller o azioni.

Per utilizzare middleware come filtro, creare un tipo con un `Configure` metodo che specifica il middleware che si desidera inserire nella pipeline di filtro. Di seguito è riportato un esempio che utilizza il middleware di localizzazione per stabilire le impostazioni cultura correnti per una richiesta:

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

È quindi possibile utilizzare il `MiddlewareFilterAttribute` per eseguire il middleware per un'azione o controller selezionato o a livello globale:

[!code-csharp[Principale](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Middleware filtri vengono eseguiti nella stessa fase della pipeline filtro come filtri delle risorse, prima di associazione del modello e dopo il resto della pipeline.

## <a name="next-actions"></a>Azioni successive

Per sperimentare i filtri, [scaricare, testare e modificare l'esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
