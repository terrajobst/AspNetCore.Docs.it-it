---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configurazione di ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 57066b8ce3254caf59cf927d16d96f8bc22a8acd
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795251"
---
<a name="configuring-aspnet-web-api-2"></a>Configurazione di ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questo argomento descrive come configurare l'API Web ASP.NET.

- [Impostazioni di configurazione](#settings)
- [Configurazione dell'API Web con Hosting di ASP.NET](#webhost)
- [Configurazione dell'API Web con self-Hosting OWIN](#selfhost)
- [Servizi API Web globale](#services)
- [Configurazione Controller](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Impostazioni di configurazione

Le impostazioni di configurazione di API Web sono definite nel [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) classe.

| Member | Descrizione |
| --- | --- |
| **DependencyResolver** | Abilita inserimento delle dipendenze per i controller. Visualizzare [utilizzando il Resolver di dipendenza di API Web](dependency-injection.md). |
| **Filtri** | Filtri dell'azione. |
| **Formattatori** | [Formattatori di Media type](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Specifica se il server deve includere dettagli dell'errore, ad esempio i messaggi di eccezione e le analisi dello stack, nei messaggi di risposta HTTP. Visualizzare [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inizializzatore** | Una funzione che esegue l'inizializzazione finale del **HttpConfiguration**. |
| **MessageHandlers** | [Gestori di messaggi HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Una raccolta di regole per l'associazione di parametri in azioni del controller. |
| **Proprietà** | Un contenitore delle proprietà generiche. |
| **Route** | Raccolta di route. Visualizzare [Routing nell'API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Servizi** | La raccolta di servizi. Visualizzare [Services](#services). |


## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configurazione dell'API Web con Hosting di ASP.NET

In un'applicazione ASP.NET, configurare Web API chiamando [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) nel **Application\_avviare** (metodo). Il **Configure** metodo accetta un delegato con un singolo parametro di tipo **HttpConfiguration**. Eseguire tutte le configurazione all'interno del delegato.

Di seguito è riportato un esempio con un delegato anonimo:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

In Visual Studio 2017, il modello di progetto "Applicazione Web ASP.NET" Configura automaticamente il codice di configurazione, se si seleziona "API Web" nel **nuovo progetto ASP.NET** finestra di dialogo.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Il modello di progetto crea un file denominato WebApiConfig.cs all'interno dell'App\_cartella di avvio. Questo file di codice definisce il delegato in cui è opportuno inserire codice di configurazione dell'API Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Il modello di progetto aggiunge anche il codice che chiama il delegato dal **Application\_avviare**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configurazione dell'API Web con self-Hosting OWIN

Se si è Self-hosting con OWIN, creare una nuova **HttpConfiguration** istanza. Eseguire eventuali configurazioni in questa istanza e quindi passare l'istanza per il **Owin.UseWebApi** metodo di estensione.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

L'esercitazione [usare OWIN per dell'API Web 2 ASP.NET](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) vengono illustrati i passaggi di completamento.

<a id="services"></a>
## <a name="global-web-api-services"></a>Servizi API Web globale

Il **HttpConfiguration.Services** raccolta contiene un set di servizi globali che consente di eseguire diverse attività, ad esempio la negoziazione del contenuto e selezione controller API Web.

> [!NOTE]
> Il **Services** raccolta non è un meccanismo per utilizzo generico per l'inserimento delle dipendenze o individuazione del servizio. Archivia solo i tipi di servizio che sono noti per il framework API Web.


Il **Services** raccolta viene inizializzata con un set predefinito di servizi ed è possibile fornire implementazioni personalizzate. Alcuni servizi supportano più istanze, mentre altri possono avere una sola istanza. (Tuttavia, è anche possibile fornire servizi a livello di controller, vedere [la configurazione per ogni Controller](#percontrollerconfig).

Servizi a istanza singola


| Service | Descrizione |
| --- | --- |
| **IActionValueBinder** | Ottiene un'associazione per un parametro. |
| **IApiExplorer** | Ottiene le descrizioni delle API esposte dall'applicazione. Visualizzare [creazione di una pagina della Guida per un'API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Ottiene un elenco degli assembly per l'applicazione. Visualizzare [Routing e selezione dell'azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Convalida un modello che viene letto dal corpo della richiesta da un formattatore di media type. |
| **IContentNegotiator** | Esegue la negoziazione del contenuto. |
| **IDocumentationProvider** | Fornisce la documentazione per le API. Il valore predefinito è **null**. Visualizzare [creazione di una pagina della Guida per un'API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indica se l'host deve memorizzare nel buffer entità corpi dei messaggi HTTP. |
| **IHttpActionInvoker** | Richiama un'azione del controller. Visualizzare [Routing e selezione dell'azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Seleziona un'azione del controller. Visualizzare [Routing e selezione dell'azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Attiva un controller. Visualizzare [Routing e selezione dell'azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Seleziona un controller. Visualizzare [Routing e selezione dell'azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Fornisce un elenco dei tipi di controller API Web nell'applicazione. Visualizzare [Routing e selezione dell'azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inizializza il framework di traccia. Visualizzare [traccia nell'API Web ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Fornisce un writer di traccia. Il valore predefinito è un writer di traccia "no-op". Visualizzare [traccia nell'API Web ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Fornisce una cache di validator del modello. |

Servizi di più istanze


|                 Service                 |                                                                                                              Descrizione                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Restituisce un elenco di filtri di un'azione del controller.                                                                                           |
|  <strong>Elemento ModelBinderProvider</strong>   |                                                                                                Restituisce un modello di associazione per un determinato tipo.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Fornisce i metadati per un modello.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Fornisce un validator per un modello.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Crea un provider di valori. Per altre informazioni, vedere del Mike stallo post di blog [come creare un provider di valori personalizzati in API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Per aggiungere un'implementazione personalizzata a un servizio a istanza multipla, chiamare **Add** o **Inserisci** sul **Services** raccolta:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Per sostituire un servizio a istanza singola con un'implementazione personalizzata, chiamare **sostituire** nel **Services** raccolta:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configurazione Controller

È possibile sostituire le impostazioni seguenti in base al controller:

- Formattatori di Media type
- Regole di associazione di parametro
- Servizi

A tale scopo, definire un attributo personalizzato che implementa il **IControllerConfiguration** interfaccia. Quindi, applicare l'attributo al controller.

Nell'esempio seguente sostituisce i formattatori di media type predefinito con un formattatore personalizzato.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

Il **IControllerConfiguration.Initialize** metodo accetta due parametri:

- Un' **HttpControllerSettings** oggetto
- Un' **HttpControllerDescriptor** oggetto

Il **HttpControllerDescriptor** contiene una descrizione del controller, che è possibile esaminare per scopi informativi (ad esempio, per distinguere tra i due controller).

Usare la **HttpControllerSettings** oggetto per configurare il controller. Questo oggetto contiene il subset di parametri di configurazione che è possibile eseguire l'override in una base per ogni controller. Eventuali impostazioni non vengono modificati per impostazione predefinita globale **HttpConfiguration** oggetto.
