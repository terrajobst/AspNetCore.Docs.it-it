---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: What ' s New in API Web ASP.NET 2.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7952614456b1de24e4c618b9e7ba8448b2a01741
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838403"
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="e1cc0-102">What ' s New in API Web ASP.NET 2.1</span><span class="sxs-lookup"><span data-stu-id="e1cc0-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="e1cc0-103">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e1cc0-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="e1cc0-104">Questo argomento descrive cosa sono le novità di ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="e1cc0-105">Download</span><span class="sxs-lookup"><span data-stu-id="e1cc0-105">Download</span></span>](#download)
- [<span data-ttu-id="e1cc0-106">Documentazione</span><span class="sxs-lookup"><span data-stu-id="e1cc0-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="e1cc0-107">Nuove funzionalità nell'API Web ASP.NET 2.1</span><span class="sxs-lookup"><span data-stu-id="e1cc0-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="e1cc0-108">Gestione degli errori globali</span><span class="sxs-lookup"><span data-stu-id="e1cc0-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="e1cc0-109">Miglioramenti di Routing dell'attributo</span><span class="sxs-lookup"><span data-stu-id="e1cc0-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="e1cc0-110">Miglioramenti alla pagina della Guida</span><span class="sxs-lookup"><span data-stu-id="e1cc0-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="e1cc0-111">Supporto IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="e1cc0-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="e1cc0-112">Formattatore di Media Type BSON</span><span class="sxs-lookup"><span data-stu-id="e1cc0-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="e1cc0-113">Supporto migliorato per i filtri asincroni</span><span class="sxs-lookup"><span data-stu-id="e1cc0-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="e1cc0-114">Query di analisi per il Client di libreria di formattazione</span><span class="sxs-lookup"><span data-stu-id="e1cc0-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="e1cc0-115">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="e1cc0-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="e1cc0-116">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="e1cc0-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="e1cc0-117">Download</span><span class="sxs-lookup"><span data-stu-id="e1cc0-117">Download</span></span>

<span data-ttu-id="e1cc0-118">Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in NuGet gallery.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="e1cc0-119">Seguono tutti i pacchetti di runtime di [Versionamento semantico](http://semver.org/) specifica.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="e1cc0-120">Il pacchetto di ASP.NET Web API 2.1 RTM più recente è la seguente versione: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="e1cc0-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="e1cc0-121">È possibile installare o aggiornare i pacchetti attraverso [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="e1cc0-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="e1cc0-122">Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="e1cc0-123">È possibile installare o aggiornare ai pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="e1cc0-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="e1cc0-124">Documentazione</span><span class="sxs-lookup"><span data-stu-id="e1cc0-124">Documentation</span></span>

<span data-ttu-id="e1cc0-125">Esercitazioni e altre informazioni sulla versione RTM di ASP.NET Web API 2.1 sono disponibili dal sito web ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="e1cc0-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="e1cc0-126">Nuove funzionalità nell'API Web ASP.NET 2.1</span><span class="sxs-lookup"><span data-stu-id="e1cc0-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="e1cc0-127">Gestione degli errori globali</span><span class="sxs-lookup"><span data-stu-id="e1cc0-127">Global Error Handling</span></span>

<span data-ttu-id="e1cc0-128">Tutte le eccezioni non gestite possono ora essere registrate tramite un meccanismo centrale e il comportamento per le eccezioni non gestite può essere personalizzato.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="e1cc0-129">Il framework supporta più logger di eccezioni, quale visualizzare le informazioni sul contesto in cui si è verificato, ad esempio la richiesta in fase di elaborazione al momento e l'eccezione non gestita.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="e1cc0-130">Ad esempio, il codice seguente usa System.Diagnostics.TraceSource per registrare tutte le eccezioni non gestite:</span><span class="sxs-lookup"><span data-stu-id="e1cc0-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="e1cc0-131">È inoltre possibile sostituire il gestore eccezioni predefinito, in modo che sia possibile personalizzare completamente il messaggio di risposta HTTP che viene inviato quando un'eccezione non gestita si verifica.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="e1cc0-132">Microsoft ha fornito una [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) che registra le eccezioni non tutte gestite tramite il framework ELMAH più diffusi.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="e1cc0-133">Miglioramenti di Routing dell'attributo</span><span class="sxs-lookup"><span data-stu-id="e1cc0-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="e1cc0-134">A questo punto il routing con attributi supporta i vincoli, abilitare il controllo delle versioni e la selezione delle route basata su intestazione.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="e1cc0-135">Inoltre, molti aspetti della route con attributi sono ora personalizzabili tramite il **IDirectRouteFactory** interfaccia e **RouteFactoryAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="e1cc0-136">Il prefisso della route è ora estendibile tramite le **IRoutePrefix** interfaccia e **RoutePrefixAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="e1cc0-137">Microsoft ha fornito una [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) che usa i vincoli per filtrare in modo dinamico i controller da un'intestazione HTTP 'api-version'.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="e1cc0-138">Miglioramenti alla pagina della Guida</span><span class="sxs-lookup"><span data-stu-id="e1cc0-138">Help Page Improvements</span></span>

<span data-ttu-id="e1cc0-139">API Web 2.1 include i miglioramenti seguenti per [pagine della Guida dell'API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="e1cc0-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="e1cc0-140">Documentazione di singole proprietà di parametri o tipi restituiti delle azioni.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="e1cc0-141">Documentazione di annotazioni del modello dati.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="e1cc0-142">È stata aggiornata anche la progettazione dell'interfaccia utente delle pagine della Guida, per supportare queste modifiche.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="e1cc0-143">Supporto IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="e1cc0-143">IgnoreRoute Support</span></span>

<span data-ttu-id="e1cc0-144">Web API 2.1 supporta ignorando i modelli di URL nel routing con API Web, tramite un set di **IgnoreRoute** metodi di estensione sul **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="e1cc0-145">Questi metodi causano l'API Web ignora eventuali URL che corrispondono a un modello specificato e consentono all'host applicare un'elaborazione aggiuntiva se appropriato.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="e1cc0-146">Nell'esempio seguente ignora gli URI che iniziano con un &quot;contenuto&quot; segmento:</span><span class="sxs-lookup"><span data-stu-id="e1cc0-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="e1cc0-147">Formattatore di Media Type BSON</span><span class="sxs-lookup"><span data-stu-id="e1cc0-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="e1cc0-148">Web API ora supporta il [BSON](http://bsonspec.org/) formato wire, sia nel client e nel server.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="e1cc0-149">Per abilitare BSON sul lato server, aggiungere il **BsonMediaTypeFormatter** alla raccolta di formattatori:</span><span class="sxs-lookup"><span data-stu-id="e1cc0-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="e1cc0-150">Ecco come un client .NET può utilizzare il formato BSON:</span><span class="sxs-lookup"><span data-stu-id="e1cc0-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="e1cc0-151">Microsoft ha fornito una [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) che mostri lato client e server.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="e1cc0-152">Per altre informazioni, vedere [supporto di BSON nel 2.1 di API Web](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="e1cc0-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="e1cc0-153">Supporto migliorato per i filtri asincroni</span><span class="sxs-lookup"><span data-stu-id="e1cc0-153">Better Support for Async Filters</span></span>

<span data-ttu-id="e1cc0-154">API Web supporta ora un modo semplice per creare filtri che eseguire in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="e1cc0-155">Questa funzionalità è utile è il filtro deve eseguire un'azione asincrona, ad esempio l'accesso a un database.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="e1cc0-156">In precedenza, per creare un filtro asincrono, era necessario implementare l'interfaccia di filtro, in quanto le classi di base di filtro esposti solo metodi sincroni.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="e1cc0-157">A questo punto è possibile eseguire l'override virtuale `On*Async` classe di base di metodi del filtro.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="e1cc0-158">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e1cc0-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="e1cc0-159">Il **AuthorizationFilterAttribute**, **ActionFilterAttribute**, e **ExceptionFilterAttribute** tutte le classi supportano async in Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="e1cc0-160">Query di analisi per il Client di libreria di formattazione</span><span class="sxs-lookup"><span data-stu-id="e1cc0-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="e1cc0-161">In precedenza **Formatting** l'analisi e le query dell'URI per il codice lato server di aggiornamento supportati, ma la libreria portabile equivalente è priva di questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="e1cc0-162">2.1 di API Web, un'applicazione client può ora facilmente analizzare e aggiornare una stringa di query.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="e1cc0-163">Negli esempi seguenti viene illustrato come analizzare, modificare e generare le query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="e1cc0-164">(Gli esempi illustrano un'applicazione console per motivi di semplicità).</span><span class="sxs-lookup"><span data-stu-id="e1cc0-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="e1cc0-165">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="e1cc0-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="e1cc0-166">In questa sezione vengono descritti problemi noti e modifiche di rilievo in ASP.NET Web API 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="e1cc0-167">Routing con attributi</span><span class="sxs-lookup"><span data-stu-id="e1cc0-167">Attribute Routing</span></span>

<span data-ttu-id="e1cc0-168">Ambiguità corrisponda al routing attributo segnalano ora un errore anziché scelta la prima corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="e1cc0-169">Le route con attributi non possono utilizzare il *{controller}* parametro e dall'utilizzo di *{action}* parametro sulle route inserito in azioni.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="e1cc0-170">Questi parametri molto probabile che potrebbero causare ambiguità.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="e1cc0-171">Lo scaffolding di MVC o Web API in un progetto con risultati pacchetti 5.1 nei 5.0 pacchetti per quelle che non esistono già nel progetto</span><span class="sxs-lookup"><span data-stu-id="e1cc0-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="e1cc0-172">Aggiornamento di pacchetti NuGet per la versione RTM di ASP.NET Web API 2.1 non aggiorna il modello di progetto applicazione Web ASP.NET o gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="e1cc0-173">Usano la versione precedente dei pacchetti di runtime ASP.NET (versione=5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="e1cc0-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="e1cc0-174">Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (versione=5.0.0.0) dei pacchetti necessari, se non sono già disponibili nei progetti.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="e1cc0-175">Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti più recenti nei progetti.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="e1cc0-176">Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento dei pacchetti in Web API 2.1 o ASP.NET MVC 5.1, assicurarsi che le versioni dell'API Web e MVC sono coerenti.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="e1cc0-177">Operazioni di ridenominazione di tipo</span><span class="sxs-lookup"><span data-stu-id="e1cc0-177">Type Renames</span></span>

<span data-ttu-id="e1cc0-178">Alcuni dei tipi usati per l'estensibilità di routing di attributi sono stati rinominati dalla versione RC a RTM la 2.1.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="e1cc0-179">Vecchio nome del tipo (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="e1cc0-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="e1cc0-180">Nuovo tipo di nome (RTM 2.1)</span><span class="sxs-lookup"><span data-stu-id="e1cc0-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="e1cc0-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="e1cc0-181">IDirectRouteProvider</span></span> | <span data-ttu-id="e1cc0-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="e1cc0-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="e1cc0-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="e1cc0-183">RouteProviderAttribute</span></span> | <span data-ttu-id="e1cc0-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="e1cc0-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="e1cc0-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="e1cc0-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="e1cc0-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="e1cc0-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="e1cc0-187">I filtri eccezioni non annullare il wrapping di eccezioni di aggregazione generate nelle azioni asincrone</span><span class="sxs-lookup"><span data-stu-id="e1cc0-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="e1cc0-188">In precedenza, se un'azione asincrona ha generato un **AggregateException**, un filtro eccezioni sarebbe unwrap l'eccezione, e **OnException** otterrebbe l'eccezione di base.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="e1cc0-189">2.1, il filtro eccezioni non annullare il wrapping, e **OnException** Ottiene originale **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="e1cc0-190">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="e1cc0-190">Bug Fixes</span></span>

<span data-ttu-id="e1cc0-191">Questa versione include anche diverse correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="e1cc0-192">È possibile trovare l'elenco completo di seguito:</span><span class="sxs-lookup"><span data-stu-id="e1cc0-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="e1cc0-193">5.1.0 pacchetto</span><span class="sxs-lookup"><span data-stu-id="e1cc0-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="e1cc0-194">5.1.1 pacchetto</span><span class="sxs-lookup"><span data-stu-id="e1cc0-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="e1cc0-195">5.1.2 il pacchetto contiene gli aggiornamenti di IntelliSense, ma non le correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="e1cc0-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
