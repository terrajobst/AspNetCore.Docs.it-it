---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: What ' s New in API Web ASP.NET 2.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 6c5a73b95955663d76238e88009ca700f9ace010
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831529"
---
<a name="whats-new-in-aspnet-web-api-21"></a>What ' s New in API Web ASP.NET 2.1
====================
da [Microsoft](https://github.com/microsoft)

Questo argomento descrive cosa sono le novità di ASP.NET Web API 2.1.

- [Download](#download)
- [Documentazione](#documentation)
- [Nuove funzionalità nell'API Web ASP.NET 2.1](#new-features)

    - [Gestione degli errori globali](#global-error)
    - [Miglioramenti di Routing dell'attributo](#attribute-routing)
    - [Miglioramenti alla pagina della Guida](#help-page)
    - [Supporto IgnoreRoute](#ignoreroute)
    - [Formattatore di Media Type BSON](#bson)
    - [Supporto migliorato per i filtri asincroni](#async-filters)
    - [Query di analisi per il Client di libreria di formattazione](#query-parsing)
- [Problemi noti e modifiche di rilievo](#known-issues)
- [Correzioni di bug](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in NuGet gallery. Seguono tutti i pacchetti di runtime di [Versionamento semantico](http://semver.org/) specifica. Il pacchetto di ASP.NET Web API 2.1 RTM più recente è la seguente versione: "5.1.2". È possibile installare o aggiornare i pacchetti attraverso [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare ai pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni sulla versione RTM di ASP.NET Web API 2.1 sono disponibili dal sito web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Nuove funzionalità nell'API Web ASP.NET 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Gestione degli errori globali

Tutte le eccezioni non gestite possono ora essere registrate tramite un meccanismo centrale e il comportamento per le eccezioni non gestite può essere personalizzato.

Il framework supporta più logger di eccezioni, quale visualizzare le informazioni sul contesto in cui si è verificato, ad esempio la richiesta in fase di elaborazione al momento e l'eccezione non gestita.

Ad esempio, il codice seguente usa System.Diagnostics.TraceSource per registrare tutte le eccezioni non gestite:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

È inoltre possibile sostituire il gestore eccezioni predefinito, in modo che sia possibile personalizzare completamente il messaggio di risposta HTTP che viene inviato quando un'eccezione non gestita si verifica.

Microsoft ha fornito una [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) che registra le eccezioni non tutte gestite tramite il framework ELMAH più diffusi.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Miglioramenti di Routing dell'attributo

A questo punto il routing con attributi supporta i vincoli, abilitare il controllo delle versioni e la selezione delle route basata su intestazione. Inoltre, molti aspetti della route con attributi sono ora personalizzabili tramite il **IDirectRouteFactory** interfaccia e **RouteFactoryAttribute** classe. Il prefisso della route è ora estendibile tramite le **IRoutePrefix** interfaccia e **RoutePrefixAttribute** classe.

Microsoft ha fornito una [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) che usa i vincoli per filtrare in modo dinamico i controller da un'intestazione HTTP 'api-version'.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Miglioramenti alla pagina della Guida

API Web 2.1 include i miglioramenti seguenti per [pagine della Guida dell'API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Documentazione di singole proprietà di parametri o tipi restituiti delle azioni.
- Documentazione di annotazioni del modello dati.

È stata aggiornata anche la progettazione dell'interfaccia utente delle pagine della Guida, per supportare queste modifiche.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Supporto IgnoreRoute

Web API 2.1 supporta ignorando i modelli di URL nel routing con API Web, tramite un set di **IgnoreRoute** metodi di estensione sul **HttpRouteCollection**. Questi metodi causano l'API Web ignora eventuali URL che corrispondono a un modello specificato e consentono all'host applicare un'elaborazione aggiuntiva se appropriato.

Nell'esempio seguente ignora gli URI che iniziano con un &quot;contenuto&quot; segmento:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Formattatore di Media Type BSON

Web API ora supporta il [BSON](http://bsonspec.org/) formato wire, sia nel client e nel server.

Per abilitare BSON sul lato server, aggiungere il **BsonMediaTypeFormatter** alla raccolta di formattatori:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Ecco come un client .NET può utilizzare il formato BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Microsoft ha fornito una [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) che mostri lato client e server.

Per altre informazioni, vedere [supporto di BSON nel 2.1 di API Web](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Supporto migliorato per i filtri asincroni

API Web supporta ora un modo semplice per creare filtri che eseguire in modo asincrono. Questa funzionalità è utile è il filtro deve eseguire un'azione asincrona, ad esempio l'accesso a un database. In precedenza, per creare un filtro asincrono, era necessario implementare l'interfaccia di filtro, in quanto le classi di base di filtro esposti solo metodi sincroni. A questo punto è possibile eseguire l'override virtuale `On*Async` classe di base di metodi del filtro.

Ad esempio:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

Il **AuthorizationFilterAttribute**, **ActionFilterAttribute**, e **ExceptionFilterAttribute** tutte le classi supportano async in Web API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Query di analisi per il Client di libreria di formattazione

In precedenza **Formatting** l'analisi e le query dell'URI per il codice lato server di aggiornamento supportati, ma la libreria portabile equivalente è priva di questa funzionalità. 2.1 di API Web, un'applicazione client può ora facilmente analizzare e aggiornare una stringa di query.

Negli esempi seguenti viene illustrato come analizzare, modificare e generare le query dell'URI. (Gli esempi illustrano un'applicazione console per motivi di semplicità).

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo in ASP.NET Web API 2.1 RTM.

### <a name="attribute-routing"></a>Routing con attributi

Ambiguità corrisponda al routing attributo segnalano ora un errore anziché scelta la prima corrispondenza.

Le route con attributi non possono utilizzare il *{controller}* parametro e dall'utilizzo di *{action}* parametro sulle route inserito in azioni. Questi parametri molto probabile che potrebbero causare ambiguità.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Lo scaffolding di MVC o Web API in un progetto con risultati pacchetti 5.1 nei 5.0 pacchetti per quelle che non esistono già nel progetto

Aggiornamento di pacchetti NuGet per la versione RTM di ASP.NET Web API 2.1 non aggiorna il modello di progetto applicazione Web ASP.NET o gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET. Usano la versione precedente dei pacchetti di runtime ASP.NET (versione=5.0.0.0). Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (versione=5.0.0.0) dei pacchetti necessari, se non sono già disponibili nei progetti. Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti più recenti nei progetti.

Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento dei pacchetti in Web API 2.1 o ASP.NET MVC 5.1, assicurarsi che le versioni dell'API Web e MVC sono coerenti.

### <a name="type-renames"></a>Operazioni di ridenominazione di tipo

Alcuni dei tipi usati per l'estensibilità di routing di attributi sono stati rinominati dalla versione RC a RTM la 2.1.

| Vecchio nome del tipo (2.1 RC) | Nuovo tipo di nome (RTM 2.1) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>I filtri eccezioni non annullare il wrapping di eccezioni di aggregazione generate nelle azioni asincrone

In precedenza, se un'azione asincrona ha generato un **AggregateException**, un filtro eccezioni sarebbe unwrap l'eccezione, e **OnException** otterrebbe l'eccezione di base. 2.1, il filtro eccezioni non annullare il wrapping, e **OnException** Ottiene originale **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correzioni di bug

Questa versione include anche diverse correzioni di bug. È possibile trovare l'elenco completo di seguito:

- [5.1.0 pacchetto](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pacchetto](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 il pacchetto contiene gli aggiornamenti di IntelliSense, ma non le correzioni di bug.
