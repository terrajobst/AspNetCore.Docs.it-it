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
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="4581a-102">Novità di ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="4581a-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="4581a-103">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4581a-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="4581a-104">In questo argomento vengono descritte le nuove per ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="4581a-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="4581a-105">Download</span><span class="sxs-lookup"><span data-stu-id="4581a-105">Download</span></span>](#download)
- [<span data-ttu-id="4581a-106">Documentazione</span><span class="sxs-lookup"><span data-stu-id="4581a-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="4581a-107">Nuove funzionalità in ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="4581a-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="4581a-108">Gestione degli errori globale</span><span class="sxs-lookup"><span data-stu-id="4581a-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="4581a-109">Attributo di miglioramenti di Routing</span><span class="sxs-lookup"><span data-stu-id="4581a-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="4581a-110">Miglioramenti di pagina della Guida</span><span class="sxs-lookup"><span data-stu-id="4581a-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="4581a-111">Supporto IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="4581a-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="4581a-112">Formattatore di Media Type BSON</span><span class="sxs-lookup"><span data-stu-id="4581a-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="4581a-113">Supporto migliorato per i filtri di Async</span><span class="sxs-lookup"><span data-stu-id="4581a-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="4581a-114">Query di analisi per il Client di libreria di formattazione</span><span class="sxs-lookup"><span data-stu-id="4581a-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="4581a-115">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="4581a-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="4581a-116">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="4581a-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="4581a-117">Download</span><span class="sxs-lookup"><span data-stu-id="4581a-117">Download</span></span>

<span data-ttu-id="4581a-118">Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet.</span><span class="sxs-lookup"><span data-stu-id="4581a-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="4581a-119">Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica.</span><span class="sxs-lookup"><span data-stu-id="4581a-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="4581a-120">Il pacchetto di ASP.NET Web API 2.1 RTM più recente è la seguente versione: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="4581a-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="4581a-121">È possibile installare o aggiornare i pacchetti tramite [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="4581a-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="4581a-122">Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.</span><span class="sxs-lookup"><span data-stu-id="4581a-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="4581a-123">È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="4581a-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="4581a-124">Documentazione</span><span class="sxs-lookup"><span data-stu-id="4581a-124">Documentation</span></span>

<span data-ttu-id="4581a-125">Esercitazioni e altre informazioni su ASP.NET Web API 2.1 RTM sono disponibili dal sito web ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="4581a-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="4581a-126">Nuove funzionalità in ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="4581a-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="4581a-127">Gestione degli errori globale</span><span class="sxs-lookup"><span data-stu-id="4581a-127">Global Error Handling</span></span>

<span data-ttu-id="4581a-128">Tutte le eccezioni non gestite possono ora essere registrate tramite un meccanismo centrale e il comportamento per le eccezioni non gestite può essere personalizzato.</span><span class="sxs-lookup"><span data-stu-id="4581a-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="4581a-129">Il framework supporta più logger di eccezioni, che, vedere l'eccezione non gestita e informazioni sul contesto in cui si è verificato, ad esempio la richiesta in fase di elaborazione al momento.</span><span class="sxs-lookup"><span data-stu-id="4581a-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="4581a-130">Ad esempio, il codice seguente usa System.Diagnostics.TraceSource per registrare tutte le eccezioni non gestite:</span><span class="sxs-lookup"><span data-stu-id="4581a-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="4581a-131">È inoltre possibile sostituire il gestore eccezioni predefinito, si verifica in modo che è possibile personalizzare completamente il messaggio di risposta HTTP che viene inviato quando un'eccezione non gestita.</span><span class="sxs-lookup"><span data-stu-id="4581a-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="4581a-132">È stato fornito un [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) che registra le eccezioni gestite tutti tramite il framework ELMAH più diffuso.</span><span class="sxs-lookup"><span data-stu-id="4581a-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="4581a-133">Attributo di miglioramenti di Routing</span><span class="sxs-lookup"><span data-stu-id="4581a-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="4581a-134">Attributo routing ora supporta i vincoli, abilitare il controllo delle versioni e la selezione di route basata su intestazione.</span><span class="sxs-lookup"><span data-stu-id="4581a-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="4581a-135">Inoltre, molti aspetti delle route di attributo sono ora personalizzabili tramite il **IDirectRouteFactory** interfaccia e **RouteFactoryAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="4581a-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="4581a-136">Il prefisso della route è estensibile tramite il **IRoutePrefix** interfaccia e **RoutePrefixAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="4581a-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="4581a-137">È stato fornito un [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) che usa i vincoli per filtrare i controller in modo dinamico da un'intestazione HTTP 'api-version'.</span><span class="sxs-lookup"><span data-stu-id="4581a-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="4581a-138">Miglioramenti di pagina della Guida</span><span class="sxs-lookup"><span data-stu-id="4581a-138">Help Page Improvements</span></span>

<span data-ttu-id="4581a-139">Web API 2.1 include i miglioramenti seguenti a [pagine della Guida API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="4581a-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="4581a-140">Documentazione di singole proprietà di parametri o tipi restituiti di azioni.</span><span class="sxs-lookup"><span data-stu-id="4581a-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="4581a-141">Documentazione di annotazioni del modello di dati.</span><span class="sxs-lookup"><span data-stu-id="4581a-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="4581a-142">Anche la progettazione dell'interfaccia utente delle pagine della Guida è stata aggiornata per supportare queste modifiche.</span><span class="sxs-lookup"><span data-stu-id="4581a-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="4581a-143">Supporto IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="4581a-143">IgnoreRoute Support</span></span>

<span data-ttu-id="4581a-144">Web API 2.1 supporta ignorando i modelli URL nel routing API Web, tramite un set di **IgnoreRoute** metodi di estensione in **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="4581a-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="4581a-145">Questi metodi causano API Web ignorare qualsiasi URL che corrispondono a un modello specificato e consentono all'host di applicare un'elaborazione aggiuntiva se appropriato.</span><span class="sxs-lookup"><span data-stu-id="4581a-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="4581a-146">Nell'esempio seguente vengono ignorati gli URI che iniziano con un &quot;contenuto&quot; segmento:</span><span class="sxs-lookup"><span data-stu-id="4581a-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="4581a-147">Formattatore di Media Type BSON</span><span class="sxs-lookup"><span data-stu-id="4581a-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="4581a-148">Web API ora supporta il [BSON](http://bsonspec.org/) formato wire, sia sul client e nel server.</span><span class="sxs-lookup"><span data-stu-id="4581a-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="4581a-149">Per abilitare BSON sul lato server, aggiungere il **BsonMediaTypeFormatter** alla raccolta di formattatori:</span><span class="sxs-lookup"><span data-stu-id="4581a-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="4581a-150">Ecco come un client .NET può utilizzare il formato BSON:</span><span class="sxs-lookup"><span data-stu-id="4581a-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="4581a-151">È stato fornito un [esempio](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) cui vengono visualizzati sul lato client e server.</span><span class="sxs-lookup"><span data-stu-id="4581a-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="4581a-152">Per ulteriori informazioni, vedere [supporto BSON in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="4581a-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="4581a-153">Supporto migliorato per i filtri di Async</span><span class="sxs-lookup"><span data-stu-id="4581a-153">Better Support for Async Filters</span></span>

<span data-ttu-id="4581a-154">API Web supporta ora un modo semplice per creare filtri che eseguire in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="4581a-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="4581a-155">Questa funzionalità è utile è il filtro deve eseguire un'azione asincrona, ad esempio l'accesso a un database.</span><span class="sxs-lookup"><span data-stu-id="4581a-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="4581a-156">In precedenza, per creare un filtro async, era necessario implementare l'interfaccia di filtro direttamente, in quanto le classi di base del filtro esposti solo metodi sincroni.</span><span class="sxs-lookup"><span data-stu-id="4581a-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="4581a-157">Ora è possibile eseguire l'override virtuale `On*Async` metodi del filtro di classe base.</span><span class="sxs-lookup"><span data-stu-id="4581a-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="4581a-158">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4581a-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="4581a-159">Il **AuthorizationFilterAttribute**, **ActionFilterAttribute**, e **ExceptionFilterAttribute** tutte classi supportano asincrono in Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="4581a-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="4581a-160">Query di analisi per il Client di libreria di formattazione</span><span class="sxs-lookup"><span data-stu-id="4581a-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="4581a-161">In precedenza, **System.Net.Http.Formatting** l'analisi e le query dell'URI per il codice lato server di aggiornamento supportati, ma la libreria portabile equivalente manca questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="4581a-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="4581a-162">2.1 API Web, un'applicazione client può ora facilmente analizzare e aggiornare una stringa di query.</span><span class="sxs-lookup"><span data-stu-id="4581a-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="4581a-163">Nell'esempio seguente viene illustrato come analizzare, modificare e generare le query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="4581a-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="4581a-164">(Gli esempi mostrano un'applicazione console per motivi di semplicità).</span><span class="sxs-lookup"><span data-stu-id="4581a-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="4581a-165">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="4581a-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="4581a-166">In questa sezione vengono descritti problemi noti e modifiche di rilievo in ASP.NET Web API 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="4581a-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="4581a-167">Routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="4581a-167">Attribute Routing</span></span>

<span data-ttu-id="4581a-168">Ambiguità di corrispondenze di routing attributo ora segnalare un errore anziché scelta la prima corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="4581a-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="4581a-169">Route di attributi vietate l'utilizzo di *{controller}* parametro e dall'utilizzo di *{action}* parametro route inserito in azioni.</span><span class="sxs-lookup"><span data-stu-id="4581a-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="4581a-170">Questi parametri sarebbero molto probabilmente causare ambiguità.</span><span class="sxs-lookup"><span data-stu-id="4581a-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="4581a-171">Lo scaffolding di API Web MVC e in un progetto con risultati pacchetti 5.1 nei 5.0 pacchetti per quelle che non esistono già nel progetto</span><span class="sxs-lookup"><span data-stu-id="4581a-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="4581a-172">L'aggiornamento di pacchetti NuGet per RTM di ASP.NET Web API 2.1 non aggiornare gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET o il modello di progetto applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4581a-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="4581a-173">Usano la versione precedente dei pacchetti di runtime ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="4581a-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="4581a-174">Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (5.0.0.0) dei pacchetti richiesti, se non sono già disponibili nei progetti.</span><span class="sxs-lookup"><span data-stu-id="4581a-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="4581a-175">Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti nei progetti più recenti.</span><span class="sxs-lookup"><span data-stu-id="4581a-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="4581a-176">Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento di pacchetti a Web API 2.1 o ASP.NET MVC 5.1, assicurarsi che le versioni di API Web e MVC sono coerenti.</span><span class="sxs-lookup"><span data-stu-id="4581a-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="4581a-177">Ridenominazione di tipo</span><span class="sxs-lookup"><span data-stu-id="4581a-177">Type Renames</span></span>

<span data-ttu-id="4581a-178">Alcuni dei tipi utilizzati per l'estensibilità di routing di attributo sono state rinominate dalla versione RC di RTM 2.1.</span><span class="sxs-lookup"><span data-stu-id="4581a-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="4581a-179">Nome di tipo precedente (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="4581a-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="4581a-180">Nuovo tipo di nome (RTM 2.1)</span><span class="sxs-lookup"><span data-stu-id="4581a-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="4581a-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="4581a-181">IDirectRouteProvider</span></span> | <span data-ttu-id="4581a-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="4581a-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="4581a-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="4581a-183">RouteProviderAttribute</span></span> | <span data-ttu-id="4581a-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="4581a-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="4581a-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="4581a-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="4581a-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="4581a-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="4581a-187">Filtri eccezioni non annullare il wrapping di eccezioni di aggregazione generate con le azioni asincrone</span><span class="sxs-lookup"><span data-stu-id="4581a-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="4581a-188">In precedenza, se un'azione asincrona ha generato un **AggregateException**, un filtro eccezioni sarebbe di scartare l'eccezione, e **OnException** otterrebbe l'eccezione di base.</span><span class="sxs-lookup"><span data-stu-id="4581a-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="4581a-189">2.1, il filtro di eccezione non annullare il wrapping, e **OnException** Ottiene originale **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="4581a-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="4581a-190">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="4581a-190">Bug Fixes</span></span>

<span data-ttu-id="4581a-191">Questa versione include inoltre diverse correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="4581a-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="4581a-192">È possibile trovare l'elenco completo di seguito:</span><span class="sxs-lookup"><span data-stu-id="4581a-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="4581a-193">5.1.0 pacchetto</span><span class="sxs-lookup"><span data-stu-id="4581a-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="4581a-194">5.1.1 pacchetto</span><span class="sxs-lookup"><span data-stu-id="4581a-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="4581a-195">Il 5.1.2 pacchetto contiene gli aggiornamenti di IntelliSense ma non correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="4581a-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
