---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: "Novità di ASP.NET Web API 2.1 | Documenti Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>Novità di ASP.NET Web API 2.1
====================
da [Microsoft](https://github.com/microsoft)

In questo argomento vengono descritte le nuove per ASP.NET Web API 2.1.

- [Download](#download)
- [Documentazione](#documentation)
- [Nuove funzionalità in ASP.NET Web API 2.1](#new-features)

    - [Gestione degli errori globale](#global-error)
    - [Attributo di miglioramenti di Routing](#attribute-routing)
    - [Miglioramenti di pagina della Guida](#help-page)
    - [Supporto IgnoreRoute](#ignoreroute)
    - [Formattatore di Media Type BSON](#bson)
    - [Supporto migliorato per i filtri di Async](#async-filters)
    - [Query di analisi per il Client di libreria di formattazione](#query-parsing)
- [Problemi noti e modifiche di rilievo](#known-issues)
- [Correzioni di bug](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet. Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica. Il pacchetto di ASP.NET Web API 2.1 RTM più recente è la seguente versione: "5.1.2". È possibile installare o aggiornare i pacchetti tramite [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET Web API 2.1 RTM sono disponibili dal sito web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Nuove funzionalità in ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Gestione degli errori globale

Tutte le eccezioni non gestite possono ora essere registrate tramite un meccanismo centrale e il comportamento per le eccezioni non gestite può essere personalizzato.

Il framework supporta più logger di eccezioni, che, vedere l'eccezione non gestita e informazioni sul contesto in cui si è verificato, ad esempio la richiesta in fase di elaborazione al momento.

Ad esempio, il codice seguente usa System.Diagnostics.TraceSource per registrare tutte le eccezioni non gestite:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

È inoltre possibile sostituire il gestore eccezioni predefinito, si verifica in modo che è possibile personalizzare completamente il messaggio di risposta HTTP che viene inviato quando un'eccezione non gestita.

È stato fornito un [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) che registra le eccezioni gestite tutti tramite il framework ELMAH più diffuso.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Attributo di miglioramenti di Routing

Attributo routing ora supporta i vincoli, abilitare il controllo delle versioni e la selezione di route basata su intestazione. Inoltre, molti aspetti delle route di attributo sono ora personalizzabili tramite il **IDirectRouteFactory** interfaccia e **RouteFactoryAttribute** classe. Il prefisso della route è estensibile tramite il **IRoutePrefix** interfaccia e **RoutePrefixAttribute** classe.

È stato fornito un [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) che usa i vincoli per filtrare i controller in modo dinamico da un'intestazione HTTP 'api-version'.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Miglioramenti di pagina della Guida

Web API 2.1 include i miglioramenti seguenti a [pagine della Guida API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Documentazione di singole proprietà di parametri o tipi restituiti di azioni.
- Documentazione di annotazioni del modello di dati.

Anche la progettazione dell'interfaccia utente delle pagine della Guida è stata aggiornata per supportare queste modifiche.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Supporto IgnoreRoute

Web API 2.1 supporta ignorando i modelli URL nel routing API Web, tramite un set di **IgnoreRoute** metodi di estensione in **HttpRouteCollection**. Questi metodi causano API Web ignorare qualsiasi URL che corrispondono a un modello specificato e consentono all'host di applicare un'elaborazione aggiuntiva se appropriato.

Nell'esempio seguente vengono ignorati gli URI che iniziano con un &quot;contenuto&quot; segmento:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Formattatore di Media Type BSON

Web API ora supporta il [BSON](http://bsonspec.org/) formato wire, sia sul client e nel server.

Per abilitare BSON sul lato server, aggiungere il **BsonMediaTypeFormatter** alla raccolta di formattatori:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Ecco come un client .NET può utilizzare il formato BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

È stato fornito un [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) cui vengono visualizzati sul lato client e server.

Per ulteriori informazioni, vedere [supporto BSON in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Supporto migliorato per i filtri di Async

API Web supporta ora un modo semplice per creare filtri che eseguire in modo asincrono. Questa funzionalità è utile è il filtro deve eseguire un'azione asincrona, ad esempio l'accesso a un database. In precedenza, per creare un filtro async, era necessario implementare l'interfaccia di filtro direttamente, in quanto le classi di base del filtro esposti solo metodi sincroni. Ora è possibile eseguire l'override virtuale `On*Async` metodi del filtro di classe base.

Ad esempio:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

Il **AuthorizationFilterAttribute**, **ActionFilterAttribute**, e **ExceptionFilterAttribute** tutte classi supportano asincrono in Web API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Query di analisi per il Client di libreria di formattazione

In precedenza, **System.Net.Http.Formatting** l'analisi e le query dell'URI per il codice lato server di aggiornamento supportati, ma la libreria portabile equivalente manca questa funzionalità. 2.1 API Web, un'applicazione client può ora facilmente analizzare e aggiornare una stringa di query.

Nell'esempio seguente viene illustrato come analizzare, modificare e generare le query dell'URI. (Gli esempi mostrano un'applicazione console per motivi di semplicità).

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo in ASP.NET Web API 2.1 RTM.

### <a name="attribute-routing"></a>Routing degli attributi

Ambiguità di corrispondenze di routing attributo ora segnalare un errore anziché scelta la prima corrispondenza.

Route di attributi vietate l'utilizzo di *{controller}* parametro e dall'utilizzo di *{action}* parametro route inserito in azioni. Questi parametri sarebbero molto probabilmente causare ambiguità.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Lo scaffolding di API Web MVC e in un progetto con risultati pacchetti 5.1 nei 5.0 pacchetti per quelle che non esistono già nel progetto

L'aggiornamento di pacchetti NuGet per RTM di ASP.NET Web API 2.1 non aggiornare gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET o il modello di progetto applicazione Web ASP.NET. Usano la versione precedente dei pacchetti di runtime ASP.NET (5.0.0.0). Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (5.0.0.0) dei pacchetti richiesti, se non sono già disponibili nei progetti. Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti nei progetti più recenti.

Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento di pacchetti a Web API 2.1 o ASP.NET MVC 5.1, assicurarsi che le versioni di API Web e MVC sono coerenti.

### <a name="type-renames"></a>Ridenominazione di tipo

Alcuni dei tipi utilizzati per l'estensibilità di routing di attributo sono state rinominate dalla versione RC di RTM 2.1.

| Nome di tipo precedente (2.1 RC) | Nuovo tipo di nome (RTM 2.1) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Filtri eccezioni non annullare il wrapping di eccezioni di aggregazione generate con le azioni asincrone

In precedenza, se un'azione asincrona ha generato un **AggregateException**, un filtro eccezioni sarebbe di scartare l'eccezione, e **OnException** otterrebbe l'eccezione di base. 2.1, il filtro di eccezione non annullare il wrapping, e **OnException** Ottiene originale **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correzioni di bug

Questa versione include inoltre diverse correzioni di bug. È possibile trovare l'elenco completo di seguito:

- [5.1.0 pacchetto](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pacchetto](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

Il 5.1.2 pacchetto contiene gli aggiornamenti di IntelliSense ma non correzioni di bug.
