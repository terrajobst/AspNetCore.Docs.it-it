---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configurazione di ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a>Configurazione di ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

In questo argomento viene descritto come configurare l'API Web ASP.NET.

- [Impostazioni di configurazione](#settings)
- [Configurazione di API Web con Hosting ASP.NET](#webhost)
- [Configurazione di Web API con self-Hosting OWIN](#selfhost)
- [Servizi API Web globale](#services)
- [Configurazione di Controller](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Impostazioni di configurazione

Le impostazioni di configurazione Web API sono definite nel [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) classe.

| Member | Descrizione |
| --- | --- |
| **DependencyResolver** | Consente l'inserimento di dipendenze per i controller. Vedere [mediante il Resolver di dipendenza di API Web](dependency-injection.md). |
| **Filtri** | Filtri dell'azione. |
| **Formattatori** | [Formattatori di Media type](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Specifica se il server deve includere dettagli dell'errore, ad esempio i messaggi di eccezione e delle tracce dello stack, nei messaggi di risposta HTTP. Vedere [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Initializer** | Una funzione che esegue l'inizializzazione finale del **HttpConfiguration**. |
| **MessageHandlers** | [Gestori di messaggi HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Raccolta di regole per l'associazione di parametri in azioni del controller. |
| **Proprietà** | Un contenitore di proprietà generiche. |
| **Route** | L'insieme di route. Vedere [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Servizi** | La raccolta di servizi. Vedere [servizi](#services). |


## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configurazione di API Web con Hosting ASP.NET

In un'applicazione ASP.NET, configurare l'API Web chiamando [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) nel **applicazione\_avviare** metodo. Il **configura** metodo accetta un delegato con un solo parametro di tipo **HttpConfiguration**. Eseguire tutte le configurazione all'interno del delegato.

Di seguito è riportato un esempio di utilizzo di un delegato anonimo:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

In Visual Studio 2017, il modello di progetto "Applicazione Web ASP.NET" imposta automaticamente il codice di configurazione, se si seleziona "API Web" nel **nuovo progetto ASP.NET** finestra di dialogo.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Il modello di progetto crea un file denominato WebApiConfig.cs all'interno dell'App\_cartella di avvio. Questo file di codice definisce il delegato in cui è necessario inserire il codice di configurazione Web API.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Il modello di progetto aggiunge anche il codice che chiama il delegato da **applicazione\_avviare**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configurazione di Web API con self-Hosting OWIN

Se si è Self-hosting con OWIN, creare un nuovo **HttpConfiguration** istanza. Eseguire tutte le configurazioni in questa istanza e quindi passare l'istanza di **Owin.UseWebApi** metodo di estensione.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

L'esercitazione [OWIN utilizzare Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) viene illustrata la procedura.

<a id="services"></a>
## <a name="global-web-api-services"></a>Servizi API Web globale

Il **HttpConfiguration.Services** raccolta contiene un set di servizi globali utilizzate per eseguire varie attività, ad esempio la negoziazione di selezione e il contenuto di controller API Web.

> [!NOTE]
> Il **servizi** raccolta non è un meccanismo generico per l'aggiunta di individuazione o la dipendenza del servizio. Archivia solo i tipi di servizio che sono noti al framework Web API.


Il **servizi** raccolta viene inizializzata con un set predefinito di servizi ed è possibile fornire implementazioni personalizzate. Alcuni servizi supportano più istanze, mentre altri può disporre di una sola istanza. (Tuttavia, è anche possibile fornire servizi a livello di controller, vedere [configurazione Controller](#percontrollerconfig).

Servizi a istanza singola


| Service | Descrizione |
| --- | --- |
| **IActionValueBinder** | Ottiene un'associazione per un parametro. |
| **IApiExplorer** | Ottiene le descrizioni delle API esposte dall'applicazione. Vedere [la creazione di una pagina della Guida per un'API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Ottiene un elenco di assembly per l'applicazione. Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Convalida un modello che viene letto dal corpo della richiesta da un formattatore di media type. |
| **IContentNegotiator** | Esegue la negoziazione del contenuto. |
| **IDocumentationProvider** | Fornisce la documentazione per le API. Il valore predefinito è **null**. Vedere [la creazione di una pagina della Guida per un'API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indica se l'host deve memorizzare nel buffer il corpo di entità dei messaggi HTTP. |
| **IHttpActionInvoker** | Richiama un'azione del controller. Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Seleziona un'azione del controller. Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Attiva un controller. Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Seleziona un controller. Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Fornisce un elenco dei tipi di controller API Web nell'applicazione. Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inizializza il framework di traccia. Vedere [traccia ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Fornisce un writer di traccia. Il valore predefinito è un writer di traccia "no-op". Vedere [traccia ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Fornisce una cache di validator del modello. |

Servizi di più istanze


| Service | Descrizione |
| --- | --- |
| **IFilterProvider** | Restituisce un elenco di filtri per un'azione del controller. |
| **ModelBinderProvider** | Restituisce un raccoglitore di modelli per un determinato tipo. |
| **ModelMetadataProvider** | Fornisce i metadati per un modello. |
| **ModelValidatorProvider** | Fornisce un validator per un modello. |
| **ValueProviderFactory** | Crea un provider di valori. Per ulteriori informazioni, vedere del Mike stallo post di blog [come creare un provider di valori personalizzati in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |.

Per aggiungere un'implementazione personalizzata a un servizio multi-istanza, chiamare **Aggiungi** o **inserire** sul **servizi** raccolta:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Per sostituire un servizio a istanza singola con un'implementazione personalizzata, chiamare **sostituire** sul **servizi** raccolta:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configurazione di Controller

È possibile sostituire le impostazioni seguenti in base al controller:

- Formattatori di Media type
- Regole di associazione di parametro
- Servizi

A tale scopo, definire un attributo personalizzato che implementa il **IControllerConfiguration** interfaccia. Quindi, applicare l'attributo nel controller.

Nell'esempio seguente sostituisce i formattatori di media type predefinito con un formattatore personalizzato.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

Il **IControllerConfiguration.Initialize** metodo accetta due parametri:

- Un **HttpControllerSettings** oggetto
- Un **HttpControllerDescriptor** oggetto

Il **HttpControllerDescriptor** contiene una descrizione del controller, che è possibile esaminare per scopi informativi (ad esempio, per distinguere tra i due controller).

Utilizzare il **HttpControllerSettings** oggetto per configurare il controller. Questo oggetto contiene il subset di parametri di configurazione che è possibile eseguire l'override in una base per ogni controller. Non modificare le impostazioni predefinite per globale **HttpConfiguration** oggetto.
