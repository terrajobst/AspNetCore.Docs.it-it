---
title: Utilizzo del modello di applicazione
author: ardalis
description: 
keywords: ASP.NET Core,ASP.NET Core MVC, il modello di applicazione
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4eb7e52f-5665-41a4-a3e3-e348d07337f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: 3c35184921dbe26cde100fd3d5124e38ea0d06cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="working-with-the-application-model"></a>Utilizzo del modello di applicazione

Da [Steve Smith](https://ardalis.com/)

ASP.NET MVC di base definisce un *modello applicativo* che rappresentano i componenti di un'applicazione MVC. È possibile leggere e modificare questo modello per modificare il comportano di elementi MVC. Per impostazione predefinita, MVC segue alcune convenzioni per determinare le classi che vengono considerate i controller, i metodi in tali classi sono azioni e il comportamento di routing e parametri. È possibile personalizzare questo comportamento in base alle esigenze dell'applicazione creando le convenzioni e applicarli a livello globale o come attributi.

## <a name="models-and-providers"></a>I modelli e i provider

Il modello di applicazione ASP.NET MVC di base includono sia astratte interfacce e classi di implementazione concreta che descrivono un'applicazione MVC. Questo modello è il risultato dell'individuazione dei controller, azioni, i parametri di azione, route e filtri in base alle convenzioni predefinite dell'app MVC. Quando si lavora con il modello di applicazione, è possibile modificare l'applicazione per seguire le convenzioni di diverse da quello predefinito MVC. I parametri, nomi, route e i filtri vengono tutti utilizzati come dati di configurazione per le azioni e controller.

Il modello di applicazione MVC ASP.NET Core presenta la struttura seguente:

* ApplicationModel
    * Controller (ControllerModel)
        * Azioni (ActionModel)
            * Parametri (ParameterModel)

Ogni livello del modello ha accesso a un comune `Properties` possono accedere e sovrascrivere i valori di proprietà impostati dai livelli superiori nella gerarchia di raccolta e i livelli inferiori. Le proprietà vengono rese persistenti il `ActionDescriptor.Properties` quando vengono create le azioni. Quindi quando una richiesta viene gestita, le proprietà una convenzione di aggiunte o modificate sono accessibili tramite `ActionContext.ActionDescriptor.Properties`. Utilizzo delle proprietà è un ottimo modo per configurare i filtri, raccoglitori di modelli e così via nei singoli per ogni azione.

> [!NOTE]
> Il `ActionDescriptor.Properties` raccolta non è thread-safe (per operazioni di scrittura) una volta terminata l'avvio dell'app. Convenzioni sono il modo migliore per aggiungere in modo sicuro i dati a questa raccolta.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

Componenti di base di ASP.NET MVC carica il modello di applicazione utilizzando un modello di provider, definito dal [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interfaccia. In questa sezione vengono illustrati alcuni dei dettagli relativi all'implementazione interna come funzioni questo provider. Si tratta di un argomento avanzato, la maggior parte delle applicazioni che utilizzano il modello di applicazione necessario farlo quando si lavora con le convenzioni.

Le implementazioni del `IApplicationModelProvider` interfaccia "wrap" un altro, con ogni chiamata di implementazione `OnProvidersExecuting` in ordine crescente in base relativa `Order` proprietà. Il `OnProvidersExecuted` quindi viene chiamato il metodo in ordine inverso. Il framework definisce diversi provider:

Primo (`Order=-1000`):

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

Quindi (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> L'ordine in cui due provider con lo stesso valore per `Order` vengono chiamati è definito e pertanto non essere affidabile.

> [!NOTE]
> `IApplicationModelProvider`è un concetto avanzato per gli autori di framework estendere. In generale, le app devono utilizzare le convenzioni e Framework devono utilizzare provider. La differenza principale è che i provider eseguiti sempre prima di convenzioni.

Il `DefaultApplicationModelProvider` stabilisce molti dei comportamenti predefinito utilizzati da ASP.NET MVC di base. Le responsabilità includono:

* Aggiunta di filtri globali al contesto
* Aggiunta di controller nel contesto
* Aggiunta di metodi pubblici controller come azioni
* Aggiunta di parametri di metodo di azione al contesto
* L'applicazione di route e gli altri attributi

Alcuni comportamenti predefiniti vengono implementati mediante la `DefaultApplicationModelProvider`. Questo provider è responsabile della costruzione di [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), che a sua volta fa riferimento a [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), e [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) istanze. La `DefaultApplicationModelProvider` classe è un dettaglio di implementazione interno del framework che possono e verrà modificato in futuro. 

Il `AuthorizationApplicationModelProvider` è responsabile dell'applicazione il comportamento associato il `AuthorizeFilter` e `AllowAnonymousFilter` gli attributi. [Altre informazioni su questi attributi](xref:security/authorization/simple).

Il `CorsApplicationModelProvider` implementa comportamento associato il `IEnableCorsAttribute` e `IDisableCorsAttribute`e `DisableCorsAuthorizationFilter`. [Altre informazioni su CORS](xref:security/cors).

## <a name="conventions"></a>Convenzioni

Il modello di applicazione definisce astrazioni convenzione che forniscono un modo più semplice per personalizzare il comportamento dei modelli dell'override l'intero modello o un provider. Le astrazioni sono il modo consigliato per modificare il comportamento dell'app. Le convenzioni forniscono un modo scrivere codice che verrà applicate in modo dinamico le personalizzazioni. Mentre [filtri](xref:mvc/controllers/filters) consentono di modificare il comportamento del framework, le personalizzazioni consentono di controllare la modalità di collegati l'intera app.

Le convenzioni seguenti sono disponibili:

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

Vengono applicate le convenzioni aggiungendo le opzioni di MVC o implementando `Attribute`s e li applica al controller, azioni o i parametri dell'azione (simile a [ `Filters` ](xref:mvc/controllers/filters)). A differenza dei filtri, le convenzioni vengono eseguite solo quando si avvia l'applicazione, non come parte di ogni richiesta.

### <a name="sample-modifying-the-applicationmodel"></a>Esempio: Modifica di ApplicationModel

La convenzione seguente viene utilizzata per aggiungere una proprietà del modello di applicazione. 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Convenzioni di modello di applicazione vengono applicate come opzioni quando MVC viene aggiunto `ConfigureServices` in `Startup`.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Le proprietà sono accessibili dal `ActionDescriptor` insieme di proprietà all'interno di azioni del controller:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Esempio: Modifica la descrizione ControllerModel

Come nell'esempio precedente, è inoltre possibile modificare il modello di controller per includere le proprietà personalizzate. Questi verranno eseguire l'override di proprietà esistenti con lo stesso nome specificato nel modello di applicazione. L'attributo di convenzione seguente aggiunge una descrizione a livello di controller:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Questa convenzione viene applicata come un attributo in un controller.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

La proprietà "descrizione" avviene nello stesso modo come negli esempi precedenti.

### <a name="sample-modifying-the-actionmodel-description"></a>Esempio: Modifica la descrizione ActionModel

Per singole azioni, l'override del comportamento già applicato a livello di applicazione o un controller, è possibile utilizzare una convenzione attributo separato.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Questa applicazione a un'azione nel controller dell'esempio precedente viene illustrato come viene eseguito l'override della convenzione a livello di controller:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Esempio: Modifica di ParameterModel

La convenzione seguente può essere applicata a parametri di azione per modificare i relativi `BindingInfo`. La convenzione seguente richiede che il parametro sia un parametro di route. altri possibili origini di associazione (ad esempio, i valori di stringa di query) vengono ignorati.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

L'attributo può essere applicato a qualsiasi parametro di azione:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Esempio: Modifica del nome ActionModel

La convenzione seguente modifica il `ActionModel` per aggiornare il *nome* dell'azione a cui viene applicato. Il nuovo nome viene fornito come parametro all'attributo. Questo nuovo nome viene utilizzato dal routing, in modo influisce negativamente la route utilizzata per raggiungere questo metodo di azione.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Questo attributo viene applicato a un metodo di azione di `HomeController`:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

Anche se è il nome del metodo `SomeName`, l'attributo sostituisce la convenzione MVC di utilizzare il nome del metodo e sostituisce il nome dell'azione con `MyCoolAction`. Pertanto, la route utilizzata per raggiungere questa azione è `/Home/MyCoolAction`.

> [!NOTE]
> In questo esempio è sostanzialmente come l'utilizzo predefinito [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) attributo.

### <a name="sample-custom-routing-convention"></a>Esempio: Convenzione di Routing personalizzato

È possibile utilizzare un `IApplicationModelConvention` per personalizzare il funzionamento del routing. Ad esempio, la convenzione seguente verrà incorporare spazi dei nomi dei controller i cicli di lavorazione, sostituendo `.` nello spazio dei nomi con `/` nella route:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

La convenzione viene aggiunto come un'opzione di avvio.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> È possibile aggiungere le convenzioni per il [middleware](xref:fundamentals/middleware) accedendo `MvcOptions` utilizzando`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

In questo esempio si applica questa convenzione alle route che non si utilizzano l'attributo di routing in cui il controller è "Namespace" nel nome. Il controller seguente illustra questa convenzione:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Utilizzo di modelli di applicazione in WebApiCompatShim

Componenti di base di ASP.NET MVC viene utilizzato un diverso set di convenzioni di ASP.NET Web API 2. Utilizza convenzioni personalizzate, è possibile modificare il comportamento di un'app ASP.NET MVC di base per essere coerente con quello di un'app Web API. Viene fornito a Microsoft di [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) per questo scopo specifico.

> [!NOTE]
> Altre informazioni, vedere [la migrazione da ASP.NET Web API](xref:migration/webapi).

Per usare lo Shim di compatibilità di Web API, è necessario aggiungere il pacchetto al progetto e quindi aggiungere le convenzioni per MVC chiamando `AddWebApiConventions` in `Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

Le convenzioni dello shim vengono applicate solo alle parti dell'app che hanno alcuni attributi applicate ad essi. Per controllare che le convenzioni modificate dalle convenzioni del shim devono disporre di controller, vengono utilizzati i quattro attributi seguenti:

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Convenzioni di azione

Il `UseWebApiActionConventionsAttribute` viene utilizzato per eseguire il mapping alle azioni in base al nome di metodo HTTP (ad esempio, `Get` consente il mapping a `HttpGet`). Si applica solo alle azioni che non utilizzano l'attributo di routing.

### <a name="overloading"></a>Overload

Il `UseWebApiOverloadingAttribute` viene utilizzato per applicare il `WebApiOverloadingApplicationModelConvention` convenzione. Questa convenzione viene aggiunto un `OverloadActionConstraint` per il processo di selezione di azione, che limita azioni candidato a quelli per cui la richiesta soddisfa tutti i parametri obbligatori.

### <a name="parameter-conventions"></a>Convenzioni dei parametri

Il `UseWebApiParameterConventionsAttribute` viene utilizzato per applicare il `WebApiParameterConventionsApplicationModelConvention` convenzione azione. Questa convenzione specifica che i tipi semplici usati come parametri di azione associati dall'URI per impostazione predefinita, mentre i tipi complessi sono associati dal corpo della richiesta.

### <a name="routes"></a>Route

Il `UseWebApiRoutesAttribute` controlli se il `WebApiApplicationModelConvention` convenzione controller viene applicato. Quando abilitata, questa convenzione viene utilizzata per aggiungere supporto [aree](xref:mvc/controllers/areas) alla route.

Oltre a un set di convenzioni, il pacchetto di compatibilità include un `System.Web.Http.ApiController` classe base che sostituisce quella fornita dall'API Web. In questo modo i controller per l'API Web e che eredita dal relativo `ApiController` a funzionare come sono stati progettati, durante l'esecuzione in ASP.NET MVC di base. Questa classe di controller di base è decorata con tutte le `UseWebApi*` gli attributi elencati in precedenza. Il `ApiController` espone proprietà, metodi e tipi di risultati che sono compatibili con quelle disponibili nell'API Web.

## <a name="using-apiexplorer-to-document-your-app"></a>Utilizzo di ApiExplorer per documentare l'App

Il modello di applicazione espone un [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) proprietà ogni livello può essere usato per attraversare la struttura dell'applicazione. Questo può essere utilizzato per [generare pagine della Guida per le API Web usando strumenti come Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger). Il `ApiExplorer` proprietà espone un `IsVisible` proprietà che è possibile impostare per specificare quali parti del modello dell'applicazione devono essere esposti. È possibile configurare questa impostazione utilizzando una convenzione:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Usando questo approccio (e le convenzioni aggiuntive se necessario), è possibile attivare o disattivare la visibilità di API a qualsiasi livello all'interno dell'app. 
