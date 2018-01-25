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
<a name="configuring-aspnet-web-api-2"></a><span data-ttu-id="c08ff-102">Configurazione di ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c08ff-102">Configuring ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c08ff-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c08ff-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c08ff-104">In questo argomento viene descritto come configurare l'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c08ff-104">This topic describes how to configure ASP.NET Web API.</span></span>

- [<span data-ttu-id="c08ff-105">Impostazioni di configurazione</span><span class="sxs-lookup"><span data-stu-id="c08ff-105">Configuration Settings</span></span>](#settings)
- [<span data-ttu-id="c08ff-106">Configurazione di API Web con Hosting ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c08ff-106">Configuring Web API with ASP.NET Hosting</span></span>](#webhost)
- [<span data-ttu-id="c08ff-107">Configurazione di Web API con self-Hosting OWIN</span><span class="sxs-lookup"><span data-stu-id="c08ff-107">Configuring Web API with OWIN Self-Hosting</span></span>](#selfhost)
- [<span data-ttu-id="c08ff-108">Servizi API Web globale</span><span class="sxs-lookup"><span data-stu-id="c08ff-108">Global Web API Services</span></span>](#services)
- [<span data-ttu-id="c08ff-109">Configurazione di Controller</span><span class="sxs-lookup"><span data-stu-id="c08ff-109">Per-Controller Configuration</span></span>](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a><span data-ttu-id="c08ff-110">Impostazioni di configurazione</span><span class="sxs-lookup"><span data-stu-id="c08ff-110">Configuration Settings</span></span>

<span data-ttu-id="c08ff-111">Le impostazioni di configurazione Web API sono definite nel [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="c08ff-111">Web API configuration setttings are defined in the [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) class.</span></span>

| <span data-ttu-id="c08ff-112">Member</span><span class="sxs-lookup"><span data-stu-id="c08ff-112">Member</span></span> | <span data-ttu-id="c08ff-113">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c08ff-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c08ff-114">**DependencyResolver**</span><span class="sxs-lookup"><span data-stu-id="c08ff-114">**DependencyResolver**</span></span> | <span data-ttu-id="c08ff-115">Consente l'inserimento di dipendenze per i controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-115">Enables dependency injection for controllers.</span></span> <span data-ttu-id="c08ff-116">Vedere [mediante il Resolver di dipendenza di API Web](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-116">See [Using the Web API Dependency Resolver](dependency-injection.md).</span></span> |
| <span data-ttu-id="c08ff-117">**Filtri**</span><span class="sxs-lookup"><span data-stu-id="c08ff-117">**Filters**</span></span> | <span data-ttu-id="c08ff-118">Filtri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="c08ff-118">Action filters.</span></span> |
| <span data-ttu-id="c08ff-119">**Formattatori**</span><span class="sxs-lookup"><span data-stu-id="c08ff-119">**Formatters**</span></span> | <span data-ttu-id="c08ff-120">[Formattatori di Media type](../formats-and-model-binding/media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-120">[Media-type formatters](../formats-and-model-binding/media-formatters.md).</span></span> |
| <span data-ttu-id="c08ff-121">**IncludeErrorDetailPolicy**</span><span class="sxs-lookup"><span data-stu-id="c08ff-121">**IncludeErrorDetailPolicy**</span></span> | <span data-ttu-id="c08ff-122">Specifica se il server deve includere dettagli dell'errore, ad esempio i messaggi di eccezione e delle tracce dello stack, nei messaggi di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c08ff-122">Specifies whether the server should include error details, such as exception messages and stack traces, in HTTP response messages.</span></span> <span data-ttu-id="c08ff-123">Vedere [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span><span class="sxs-lookup"><span data-stu-id="c08ff-123">See [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span></span> |
| <span data-ttu-id="c08ff-124">**Initializer**</span><span class="sxs-lookup"><span data-stu-id="c08ff-124">**Initializer**</span></span> | <span data-ttu-id="c08ff-125">Una funzione che esegue l'inizializzazione finale del **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="c08ff-125">A function that performs final initialization of the **HttpConfiguration**.</span></span> |
| <span data-ttu-id="c08ff-126">**MessageHandlers**</span><span class="sxs-lookup"><span data-stu-id="c08ff-126">**MessageHandlers**</span></span> | <span data-ttu-id="c08ff-127">[Gestori di messaggi HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-127">[HTTP message handlers](http-message-handlers.md).</span></span> |
| <span data-ttu-id="c08ff-128">**ParameterBindingRules**</span><span class="sxs-lookup"><span data-stu-id="c08ff-128">**ParameterBindingRules**</span></span> | <span data-ttu-id="c08ff-129">Raccolta di regole per l'associazione di parametri in azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-129">A collection of rules for binding parameters on controller actions.</span></span> |
| <span data-ttu-id="c08ff-130">**Proprietà**</span><span class="sxs-lookup"><span data-stu-id="c08ff-130">**Properties**</span></span> | <span data-ttu-id="c08ff-131">Un contenitore di proprietà generiche.</span><span class="sxs-lookup"><span data-stu-id="c08ff-131">A generic property bag.</span></span> |
| <span data-ttu-id="c08ff-132">**Route**</span><span class="sxs-lookup"><span data-stu-id="c08ff-132">**Routes**</span></span> | <span data-ttu-id="c08ff-133">L'insieme di route.</span><span class="sxs-lookup"><span data-stu-id="c08ff-133">The collection of routes.</span></span> <span data-ttu-id="c08ff-134">Vedere [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-134">See [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="c08ff-135">**Servizi**</span><span class="sxs-lookup"><span data-stu-id="c08ff-135">**Services**</span></span> | <span data-ttu-id="c08ff-136">La raccolta di servizi.</span><span class="sxs-lookup"><span data-stu-id="c08ff-136">The collection of services.</span></span> <span data-ttu-id="c08ff-137">Vedere [servizi](#services).</span><span class="sxs-lookup"><span data-stu-id="c08ff-137">See [Services](#services).</span></span> |


## <a name="prerequisites"></a><span data-ttu-id="c08ff-138">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c08ff-138">Prerequisites</span></span>

<span data-ttu-id="c08ff-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="c08ff-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition.</span></span>

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a><span data-ttu-id="c08ff-140">Configurazione di API Web con Hosting ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c08ff-140">Configuring Web API with ASP.NET Hosting</span></span>

<span data-ttu-id="c08ff-141">In un'applicazione ASP.NET, configurare l'API Web chiamando [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) nel **applicazione\_avviare** metodo.</span><span class="sxs-lookup"><span data-stu-id="c08ff-141">In an ASP.NET application, configure Web API by calling [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in the **Application\_Start** method.</span></span> <span data-ttu-id="c08ff-142">Il **configura** metodo accetta un delegato con un solo parametro di tipo **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="c08ff-142">The **Configure** method takes a delegate with a single parameter of type **HttpConfiguration**.</span></span> <span data-ttu-id="c08ff-143">Eseguire tutte le configurazione all'interno del delegato.</span><span class="sxs-lookup"><span data-stu-id="c08ff-143">Perform all of your configuation inside the delegate.</span></span>

<span data-ttu-id="c08ff-144">Di seguito è riportato un esempio di utilizzo di un delegato anonimo:</span><span class="sxs-lookup"><span data-stu-id="c08ff-144">Here is an example using an anonymous delegate:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="c08ff-145">In Visual Studio 2017, il modello di progetto "Applicazione Web ASP.NET" imposta automaticamente il codice di configurazione, se si seleziona "API Web" nel **nuovo progetto ASP.NET** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="c08ff-145">In Visual Studio 2017, the "ASP.NET Web Application" project template automatically sets up the configuration code, if you select "Web API" in the **New ASP.NET Project** dialog.</span></span>

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

<span data-ttu-id="c08ff-146">Il modello di progetto crea un file denominato WebApiConfig.cs all'interno dell'App\_cartella di avvio.</span><span class="sxs-lookup"><span data-stu-id="c08ff-146">The project template creates a file named WebApiConfig.cs inside the App\_Start folder.</span></span> <span data-ttu-id="c08ff-147">Questo file di codice definisce il delegato in cui è necessario inserire il codice di configurazione Web API.</span><span class="sxs-lookup"><span data-stu-id="c08ff-147">This code file defines the delegate where you should put your Web API configuration code.</span></span>

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

<span data-ttu-id="c08ff-148">Il modello di progetto aggiunge anche il codice che chiama il delegato da **applicazione\_avviare**.</span><span class="sxs-lookup"><span data-stu-id="c08ff-148">The project template also adds the code that calls the delegate from **Application\_Start**.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a><span data-ttu-id="c08ff-149">Configurazione di Web API con self-Hosting OWIN</span><span class="sxs-lookup"><span data-stu-id="c08ff-149">Configuring Web API with OWIN Self-Hosting</span></span>

<span data-ttu-id="c08ff-150">Se si è Self-hosting con OWIN, creare un nuovo **HttpConfiguration** istanza.</span><span class="sxs-lookup"><span data-stu-id="c08ff-150">If you are self-hosting with OWIN, create a new **HttpConfiguration** instance.</span></span> <span data-ttu-id="c08ff-151">Eseguire tutte le configurazioni in questa istanza e quindi passare l'istanza di **Owin.UseWebApi** metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="c08ff-151">Perform any configuration on this instance, and then pass the instance to the **Owin.UseWebApi** extension method.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="c08ff-152">L'esercitazione [OWIN utilizzare Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) viene illustrata la procedura.</span><span class="sxs-lookup"><span data-stu-id="c08ff-152">The tutorial [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) shows the complete steps.</span></span>

<a id="services"></a>
## <a name="global-web-api-services"></a><span data-ttu-id="c08ff-153">Servizi API Web globale</span><span class="sxs-lookup"><span data-stu-id="c08ff-153">Global Web API Services</span></span>

<span data-ttu-id="c08ff-154">Il **HttpConfiguration.Services** raccolta contiene un set di servizi globali utilizzate per eseguire varie attività, ad esempio la negoziazione di selezione e il contenuto di controller API Web.</span><span class="sxs-lookup"><span data-stu-id="c08ff-154">The **HttpConfiguration.Services** collection contains a set of global services that Web API uses to perform various tasks, such as controller selection and content negotiation.</span></span>

> [!NOTE]
> <span data-ttu-id="c08ff-155">Il **servizi** raccolta non è un meccanismo generico per l'aggiunta di individuazione o la dipendenza del servizio.</span><span class="sxs-lookup"><span data-stu-id="c08ff-155">The **Services** collection is not a general-purpose mechanism for service discovery or dependency injection.</span></span> <span data-ttu-id="c08ff-156">Archivia solo i tipi di servizio che sono noti al framework Web API.</span><span class="sxs-lookup"><span data-stu-id="c08ff-156">It only stores service types that are known to the Web API framework.</span></span>


<span data-ttu-id="c08ff-157">Il **servizi** raccolta viene inizializzata con un set predefinito di servizi ed è possibile fornire implementazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="c08ff-157">The **Services** collection is initialized with a default set of services, and you can provide your own custom implementations.</span></span> <span data-ttu-id="c08ff-158">Alcuni servizi supportano più istanze, mentre altri può disporre di una sola istanza.</span><span class="sxs-lookup"><span data-stu-id="c08ff-158">Some services support multiple instances, while others can have only one instance.</span></span> <span data-ttu-id="c08ff-159">(Tuttavia, è anche possibile fornire servizi a livello di controller, vedere [configurazione Controller](#percontrollerconfig).</span><span class="sxs-lookup"><span data-stu-id="c08ff-159">(However, you can also provide services at the controller level; see [Per-Controller Configuration](#percontrollerconfig).</span></span>

<span data-ttu-id="c08ff-160">Servizi a istanza singola</span><span class="sxs-lookup"><span data-stu-id="c08ff-160">Single-Instance Services</span></span>


| <span data-ttu-id="c08ff-161">Service</span><span class="sxs-lookup"><span data-stu-id="c08ff-161">Service</span></span> | <span data-ttu-id="c08ff-162">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c08ff-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c08ff-163">**IActionValueBinder**</span><span class="sxs-lookup"><span data-stu-id="c08ff-163">**IActionValueBinder**</span></span> | <span data-ttu-id="c08ff-164">Ottiene un'associazione per un parametro.</span><span class="sxs-lookup"><span data-stu-id="c08ff-164">Gets a binding for a parameter.</span></span> |
| <span data-ttu-id="c08ff-165">**IApiExplorer**</span><span class="sxs-lookup"><span data-stu-id="c08ff-165">**IApiExplorer**</span></span> | <span data-ttu-id="c08ff-166">Ottiene le descrizioni delle API esposte dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c08ff-166">Gets descriptions of the APIs exposed by the application.</span></span> <span data-ttu-id="c08ff-167">Vedere [la creazione di una pagina della Guida per un'API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-167">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="c08ff-168">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="c08ff-168">**IAssembliesResolver**</span></span> | <span data-ttu-id="c08ff-169">Ottiene un elenco di assembly per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c08ff-169">Gets a list of the assemblies for the application.</span></span> <span data-ttu-id="c08ff-170">Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-170">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="c08ff-171">**IBodyModelValidator**</span><span class="sxs-lookup"><span data-stu-id="c08ff-171">**IBodyModelValidator**</span></span> | <span data-ttu-id="c08ff-172">Convalida un modello che viene letto dal corpo della richiesta da un formattatore di media type.</span><span class="sxs-lookup"><span data-stu-id="c08ff-172">Validates a model that is read from the request body by a media-type formatter.</span></span> |
| <span data-ttu-id="c08ff-173">**IContentNegotiator**</span><span class="sxs-lookup"><span data-stu-id="c08ff-173">**IContentNegotiator**</span></span> | <span data-ttu-id="c08ff-174">Esegue la negoziazione del contenuto.</span><span class="sxs-lookup"><span data-stu-id="c08ff-174">Performs content negotiation.</span></span> |
| <span data-ttu-id="c08ff-175">**IDocumentationProvider**</span><span class="sxs-lookup"><span data-stu-id="c08ff-175">**IDocumentationProvider**</span></span> | <span data-ttu-id="c08ff-176">Fornisce la documentazione per le API.</span><span class="sxs-lookup"><span data-stu-id="c08ff-176">Provides documentation for APIs.</span></span> <span data-ttu-id="c08ff-177">Il valore predefinito è **null**.</span><span class="sxs-lookup"><span data-stu-id="c08ff-177">The default is **null**.</span></span> <span data-ttu-id="c08ff-178">Vedere [la creazione di una pagina della Guida per un'API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-178">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="c08ff-179">**IHostBufferPolicySelector**</span><span class="sxs-lookup"><span data-stu-id="c08ff-179">**IHostBufferPolicySelector**</span></span> | <span data-ttu-id="c08ff-180">Indica se l'host deve memorizzare nel buffer il corpo di entità dei messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="c08ff-180">Indicates whether the host should buffer HTTP message entity bodies.</span></span> |
| <span data-ttu-id="c08ff-181">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="c08ff-181">**IHttpActionInvoker**</span></span> | <span data-ttu-id="c08ff-182">Richiama un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-182">Invokes a controller action.</span></span> <span data-ttu-id="c08ff-183">Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-183">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="c08ff-184">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="c08ff-184">**IHttpActionSelector**</span></span> | <span data-ttu-id="c08ff-185">Seleziona un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-185">Selects a controller action.</span></span> <span data-ttu-id="c08ff-186">Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-186">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="c08ff-187">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="c08ff-187">**IHttpControllerActivator**</span></span> | <span data-ttu-id="c08ff-188">Attiva un controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-188">Activates a controller.</span></span> <span data-ttu-id="c08ff-189">Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-189">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="c08ff-190">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="c08ff-190">**IHttpControllerSelector**</span></span> | <span data-ttu-id="c08ff-191">Seleziona un controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-191">Selects a controller.</span></span> <span data-ttu-id="c08ff-192">Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-192">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="c08ff-193">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="c08ff-193">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="c08ff-194">Fornisce un elenco dei tipi di controller API Web nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c08ff-194">Provides a list of the Web API controller types in the application.</span></span> <span data-ttu-id="c08ff-195">Vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-195">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="c08ff-196">**ITraceManager**</span><span class="sxs-lookup"><span data-stu-id="c08ff-196">**ITraceManager**</span></span> | <span data-ttu-id="c08ff-197">Inizializza il framework di traccia.</span><span class="sxs-lookup"><span data-stu-id="c08ff-197">Initializes the tracing framework.</span></span> <span data-ttu-id="c08ff-198">Vedere [traccia ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-198">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="c08ff-199">**ITraceWriter**</span><span class="sxs-lookup"><span data-stu-id="c08ff-199">**ITraceWriter**</span></span> | <span data-ttu-id="c08ff-200">Fornisce un writer di traccia.</span><span class="sxs-lookup"><span data-stu-id="c08ff-200">Provides a trace writer.</span></span> <span data-ttu-id="c08ff-201">Il valore predefinito è un writer di traccia "no-op".</span><span class="sxs-lookup"><span data-stu-id="c08ff-201">The default is a "no-op" trace writer.</span></span> <span data-ttu-id="c08ff-202">Vedere [traccia ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c08ff-202">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="c08ff-203">**IModelValidatorCache**</span><span class="sxs-lookup"><span data-stu-id="c08ff-203">**IModelValidatorCache**</span></span> | <span data-ttu-id="c08ff-204">Fornisce una cache di validator del modello.</span><span class="sxs-lookup"><span data-stu-id="c08ff-204">Provides a cache of model validators.</span></span> |

<span data-ttu-id="c08ff-205">Servizi di più istanze</span><span class="sxs-lookup"><span data-stu-id="c08ff-205">Multiple-Instance Services</span></span>


| <span data-ttu-id="c08ff-206">Service</span><span class="sxs-lookup"><span data-stu-id="c08ff-206">Service</span></span> | <span data-ttu-id="c08ff-207">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c08ff-207">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c08ff-208">**IFilterProvider**</span><span class="sxs-lookup"><span data-stu-id="c08ff-208">**IFilterProvider**</span></span> | <span data-ttu-id="c08ff-209">Restituisce un elenco di filtri per un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-209">Returns a list of filters for a controller action.</span></span> |
| <span data-ttu-id="c08ff-210">**ModelBinderProvider**</span><span class="sxs-lookup"><span data-stu-id="c08ff-210">**ModelBinderProvider**</span></span> | <span data-ttu-id="c08ff-211">Restituisce un raccoglitore di modelli per un determinato tipo.</span><span class="sxs-lookup"><span data-stu-id="c08ff-211">Returns a model binder for a given type.</span></span> |
| <span data-ttu-id="c08ff-212">**ModelMetadataProvider**</span><span class="sxs-lookup"><span data-stu-id="c08ff-212">**ModelMetadataProvider**</span></span> | <span data-ttu-id="c08ff-213">Fornisce i metadati per un modello.</span><span class="sxs-lookup"><span data-stu-id="c08ff-213">Provides metadata for a model.</span></span> |
| <span data-ttu-id="c08ff-214">**ModelValidatorProvider**</span><span class="sxs-lookup"><span data-stu-id="c08ff-214">**ModelValidatorProvider**</span></span> | <span data-ttu-id="c08ff-215">Fornisce un validator per un modello.</span><span class="sxs-lookup"><span data-stu-id="c08ff-215">Provides a validator for a model.</span></span> |
| <span data-ttu-id="c08ff-216">**ValueProviderFactory**</span><span class="sxs-lookup"><span data-stu-id="c08ff-216">**ValueProviderFactory**</span></span> | <span data-ttu-id="c08ff-217">Crea un provider di valori.</span><span class="sxs-lookup"><span data-stu-id="c08ff-217">Creates a value provider.</span></span> <span data-ttu-id="c08ff-218">Per ulteriori informazioni, vedere del Mike stallo post di blog [come creare un provider di valori personalizzati in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span><span class="sxs-lookup"><span data-stu-id="c08ff-218">For more information, see Mike Stall's blog post [How to create a custom value provider in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span></span> |<span data-ttu-id="c08ff-219">.</span><span class="sxs-lookup"><span data-stu-id="c08ff-219">.</span></span>

<span data-ttu-id="c08ff-220">Per aggiungere un'implementazione personalizzata a un servizio multi-istanza, chiamare **Aggiungi** o **inserire** sul **servizi** raccolta:</span><span class="sxs-lookup"><span data-stu-id="c08ff-220">To add a custom implementation to a multi-instance service, call **Add** or **Insert** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="c08ff-221">Per sostituire un servizio a istanza singola con un'implementazione personalizzata, chiamare **sostituire** sul **servizi** raccolta:</span><span class="sxs-lookup"><span data-stu-id="c08ff-221">To replace a single-instance service with a custom implementation, call **Replace** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a><span data-ttu-id="c08ff-222">Configurazione di Controller</span><span class="sxs-lookup"><span data-stu-id="c08ff-222">Per-Controller Configuration</span></span>

<span data-ttu-id="c08ff-223">È possibile sostituire le impostazioni seguenti in base al controller:</span><span class="sxs-lookup"><span data-stu-id="c08ff-223">You can override the following settings on a per-controller basis:</span></span>

- <span data-ttu-id="c08ff-224">Formattatori di Media type</span><span class="sxs-lookup"><span data-stu-id="c08ff-224">Media-type formatters</span></span>
- <span data-ttu-id="c08ff-225">Regole di associazione di parametro</span><span class="sxs-lookup"><span data-stu-id="c08ff-225">Parameter binding rules</span></span>
- <span data-ttu-id="c08ff-226">Servizi</span><span class="sxs-lookup"><span data-stu-id="c08ff-226">Services</span></span>

<span data-ttu-id="c08ff-227">A tale scopo, definire un attributo personalizzato che implementa il **IControllerConfiguration** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="c08ff-227">To do so, define a custom attribute that implements the **IControllerConfiguration** interface.</span></span> <span data-ttu-id="c08ff-228">Quindi, applicare l'attributo nel controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-228">Then apply the attribute to the controller.</span></span>

<span data-ttu-id="c08ff-229">Nell'esempio seguente sostituisce i formattatori di media type predefinito con un formattatore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c08ff-229">The following example replaces the default media-type formatters with a custom formatter.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="c08ff-230">Il **IControllerConfiguration.Initialize** metodo accetta due parametri:</span><span class="sxs-lookup"><span data-stu-id="c08ff-230">The **IControllerConfiguration.Initialize** method takes two parameters:</span></span>

- <span data-ttu-id="c08ff-231">Un **HttpControllerSettings** oggetto</span><span class="sxs-lookup"><span data-stu-id="c08ff-231">An **HttpControllerSettings** object</span></span>
- <span data-ttu-id="c08ff-232">Un **HttpControllerDescriptor** oggetto</span><span class="sxs-lookup"><span data-stu-id="c08ff-232">An **HttpControllerDescriptor** object</span></span>

<span data-ttu-id="c08ff-233">Il **HttpControllerDescriptor** contiene una descrizione del controller, che è possibile esaminare per scopi informativi (ad esempio, per distinguere tra i due controller).</span><span class="sxs-lookup"><span data-stu-id="c08ff-233">The **HttpControllerDescriptor** contains a description of the controller, which you can examine for informational purposes (say, to distinguish between two controllers).</span></span>

<span data-ttu-id="c08ff-234">Utilizzare il **HttpControllerSettings** oggetto per configurare il controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-234">Use the **HttpControllerSettings** object to configure the controller.</span></span> <span data-ttu-id="c08ff-235">Questo oggetto contiene il subset di parametri di configurazione che è possibile eseguire l'override in una base per ogni controller.</span><span class="sxs-lookup"><span data-stu-id="c08ff-235">This object contains the subset of configuration parameters that you can override on a per-controller basis.</span></span> <span data-ttu-id="c08ff-236">Non modificare le impostazioni predefinite per globale **HttpConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="c08ff-236">Any settings that you don't change default to the global **HttpConfiguration** object.</span></span>
