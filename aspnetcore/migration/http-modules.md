---
title: La migrazione di gestori HTTP e moduli al middleware di ASP.NET Core.
author: rick-anderson
description: 
ms.author: tdykstra
manager: wpickett
ms.date: 12/07/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/http-modules
ms.openlocfilehash: a38ddc64583de05b4088cd31d48fbd7ee949d4e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="72cf4-102">La migrazione di gestori HTTP e moduli al middleware di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72cf4-102">Migrating HTTP handlers and modules to ASP.NET Core middleware</span></span> 

<span data-ttu-id="72cf4-103">Da [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="72cf4-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="72cf4-104">In questo articolo viene illustrato come eseguire la migrazione di ASP.NET esistente [moduli e i gestori da System. webServer](https://docs.microsoft.com/iis/configuration/system.webserver/) per ASP.NET Core [middleware](../fundamentals/middleware.md).</span><span class="sxs-lookup"><span data-stu-id="72cf4-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) to ASP.NET Core [middleware](../fundamentals/middleware.md).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="72cf4-105">I moduli e i gestori aggiornamento</span><span class="sxs-lookup"><span data-stu-id="72cf4-105">Modules and handlers revisited</span></span>

<span data-ttu-id="72cf4-106">Prima di procedere al middleware di ASP.NET Core, verrà innanzitutto riepilogo funzionano dei gestori e moduli HTTP:</span><span class="sxs-lookup"><span data-stu-id="72cf4-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Gestore di moduli](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="72cf4-108">**I gestori sono:**</span><span class="sxs-lookup"><span data-stu-id="72cf4-108">**Handlers are:**</span></span>

   * <span data-ttu-id="72cf4-109">Le classi che implementano [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="72cf4-109">Classes that implement [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="72cf4-110">Utilizzato per gestire le richieste con un nome file specificato o l'estensione, ad esempio *.report*</span><span class="sxs-lookup"><span data-stu-id="72cf4-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="72cf4-111">[Configurato](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web. config*</span><span class="sxs-lookup"><span data-stu-id="72cf4-111">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="72cf4-112">**I moduli sono:**</span><span class="sxs-lookup"><span data-stu-id="72cf4-112">**Modules are:**</span></span>

   * <span data-ttu-id="72cf4-113">Le classi che implementano [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="72cf4-113">Classes that implement [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="72cf4-114">Viene richiamata per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="72cf4-114">Invoked for every request</span></span>

   * <span data-ttu-id="72cf4-115">In grado di corto circuito (interrompere ulteriore elaborazione di una richiesta)</span><span class="sxs-lookup"><span data-stu-id="72cf4-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="72cf4-116">In grado di aggiungere alla risposta HTTP o crearne di propri</span><span class="sxs-lookup"><span data-stu-id="72cf4-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="72cf4-117">[Configurato](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web. config*</span><span class="sxs-lookup"><span data-stu-id="72cf4-117">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="72cf4-118">**L'ordine in cui i moduli di elaborare le richieste in ingresso è determinato da:**</span><span class="sxs-lookup"><span data-stu-id="72cf4-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="72cf4-119">Il [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx), ovvero gli eventi una serie viene generato da ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)e così via. Ogni modulo è possibile creare un gestore per uno o più eventi.</span><span class="sxs-lookup"><span data-stu-id="72cf4-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="72cf4-120">Per lo stesso evento, l'ordine in cui la configurazione di tali in *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="72cf4-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="72cf4-121">Oltre a moduli, è possibile aggiungere i gestori per gli eventi del ciclo di vita per il *Global.asax.cs* file.</span><span class="sxs-lookup"><span data-stu-id="72cf4-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="72cf4-122">Questi gestori vengono eseguiti dopo i gestori di moduli configurati.</span><span class="sxs-lookup"><span data-stu-id="72cf4-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="72cf4-123">Da gestori e i moduli al middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="72cf4-124">**Middleware sono più semplici rispetto a gestori e i moduli HTTP:**</span><span class="sxs-lookup"><span data-stu-id="72cf4-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="72cf4-125">I moduli, i gestori, *Global.asax.cs*, *Web. config* (ad eccezione di configurazione di IIS) e il ciclo di vita dell'applicazione sono stati eliminati</span><span class="sxs-lookup"><span data-stu-id="72cf4-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="72cf4-126">I ruoli di moduli e i gestori sono stati eseguiti dai middleware</span><span class="sxs-lookup"><span data-stu-id="72cf4-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="72cf4-127">Middleware vengono configurati utilizzando codice anziché in *Web. config*</span><span class="sxs-lookup"><span data-stu-id="72cf4-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="72cf4-128">[Creazione di rami pipeline](../fundamentals/middleware.md#middleware-run-map-use) consente di inviare richieste al middleware specifico, dipende non solo l'URL ma anche delle intestazioni di richiesta, le stringhe di query, ecc.</span><span class="sxs-lookup"><span data-stu-id="72cf4-128">[Pipeline branching](../fundamentals/middleware.md#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="72cf4-129">**Middleware sono molto simili ai moduli:**</span><span class="sxs-lookup"><span data-stu-id="72cf4-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="72cf4-130">Richiamato in linea di massima per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="72cf4-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="72cf4-131">In grado di corto circuito, una richiesta da [non passare la richiesta al middleware successivo](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="72cf4-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="72cf4-132">In grado di creare i propri risposta HTTP</span><span class="sxs-lookup"><span data-stu-id="72cf4-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="72cf4-133">**Middleware e i moduli vengono elaborati in un ordine diverso:**</span><span class="sxs-lookup"><span data-stu-id="72cf4-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="72cf4-134">Ordine di middleware è in base all'ordine in cui viene inserito nella pipeline delle richieste, mentre l'ordine dei moduli si basa principalmente sulla [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx) eventi</span><span class="sxs-lookup"><span data-stu-id="72cf4-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="72cf4-135">Ordine del middleware per le risposte è l'opposto rispetto a quello per le richieste, mentre l'ordine dei moduli è lo stesso per le richieste e risposte</span><span class="sxs-lookup"><span data-stu-id="72cf4-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="72cf4-136">Vedere [creazione di una pipeline middleware con IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="72cf4-136">See [Creating a middleware pipeline with IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Middleware](http-modules/_static/middleware.png)

<span data-ttu-id="72cf4-138">Si noti come nell'immagine precedente, il middleware di autenticazione short-circuited la richiesta.</span><span class="sxs-lookup"><span data-stu-id="72cf4-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="72cf4-139">Migrazione del codice modulo al middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-139">Migrating module code to middleware</span></span>

<span data-ttu-id="72cf4-140">Un modulo HTTP esistente avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="72cf4-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="72cf4-141">Come illustrato nel [Middleware](../fundamentals/middleware.md) pagina, un middleware di ASP.NET Core è una classe che espone un `Invoke` acquisire metodo un `HttpContext` e restituendo un `Task`.</span><span class="sxs-lookup"><span data-stu-id="72cf4-141">As shown in the [Middleware](../fundamentals/middleware.md) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="72cf4-142">Il middleware nuovo sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="72cf4-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="72cf4-143">Il modello di middleware precedente è stato effettuato dalla sezione su [scrittura middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span><span class="sxs-lookup"><span data-stu-id="72cf4-143">The above middleware template was taken from the section on [writing middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span></span>

<span data-ttu-id="72cf4-144">Il *MyMiddlewareExtensions* classe helper rende più semplice configurare il middleware del `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="72cf4-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="72cf4-145">Il `UseMyMiddleware` metodo aggiunge la classe middleware alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="72cf4-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="72cf4-146">Servizi richiesti dal middleware ottengano inseriti nel costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="72cf4-147">Il modulo potrebbe terminare una richiesta, ad esempio, se l'utente non autorizzato:</span><span class="sxs-lookup"><span data-stu-id="72cf4-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="72cf4-148">Un tipo di middleware viene gestita tramite la chiamata non `Invoke` nel middleware successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="72cf4-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="72cf4-149">Tenere presente che questo non termina completamente la richiesta, in quanto middlewares precedente ancora da richiamare quando la risposta procede attraverso la pipeline.</span><span class="sxs-lookup"><span data-stu-id="72cf4-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="72cf4-150">Quando si esegue la migrazione di funzionalità del modulo per il middleware di nuovo, è possibile che il codice non viene compilato perché la `HttpContext` classe è stata modificata in modo significativo in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72cf4-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="72cf4-151">[In un secondo momento](#migrating-to-the-new-httpcontext), si vedrà come eseguire la migrazione alla nuova ASP.NET Core HttpContext.</span><span class="sxs-lookup"><span data-stu-id="72cf4-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="72cf4-152">Inserimento di modulo migrazione nella pipeline delle richieste</span><span class="sxs-lookup"><span data-stu-id="72cf4-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="72cf4-153">I moduli HTTP vengono in genere aggiunti per la richiesta di pipeline utilizzando *Web. config*:</span><span class="sxs-lookup"><span data-stu-id="72cf4-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="72cf4-154">Il problema, convertire [aggiunta del nuovo middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) alla pipeline delle richieste nel `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="72cf4-154">Convert this by [adding your new middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="72cf4-155">La posizione esatta nella pipeline di punto di inserimento del nuovo middleware dipende dall'evento che gestito come modulo (`BeginRequest`, `EndRequest`e così via) e il relativo ordine nell'elenco dei moduli in *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="72cf4-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="72cf4-156">Come già non indicato, vi è alcun ciclo di vita delle applicazioni in ASP.NET Core e l'ordine in cui vengono elaborate le risposte dal middleware è diverso da quello usato dai moduli.</span><span class="sxs-lookup"><span data-stu-id="72cf4-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="72cf4-157">Questo potrebbe rendere la decisione di ordinamento più complessa.</span><span class="sxs-lookup"><span data-stu-id="72cf4-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="72cf4-158">Se l'ordinamento diventa un problema, è possibile suddividere un modulo in più componenti middleware che possono essere ordinati in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="72cf4-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="72cf4-159">Migrazione del codice del gestore al middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="72cf4-160">Un gestore HTTP è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="72cf4-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="72cf4-161">Nel progetto ASP.NET Core, è necessario convertire in un tipo di middleware simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="72cf4-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="72cf4-162">Questo middleware è molto simile per il middleware corrispondente ai moduli.</span><span class="sxs-lookup"><span data-stu-id="72cf4-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="72cf4-163">L'unica differenza è che qui non è presente alcuna chiamata a `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="72cf4-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="72cf4-164">Che è utile, perché il gestore non è alla fine della pipeline di richieste, in modo che vi sia alcun middleware successivo da richiamare.</span><span class="sxs-lookup"><span data-stu-id="72cf4-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="72cf4-165">La migrazione di inserimento gestore nella pipeline delle richieste</span><span class="sxs-lookup"><span data-stu-id="72cf4-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="72cf4-166">Configurazione di un gestore HTTP viene eseguita in *Web. config* e simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="72cf4-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="72cf4-167">È possibile convertire questo valore tramite l'aggiunta del nuovo middleware gestore alla pipeline delle richieste nel `Startup` (classe), simile al middleware convertito da moduli.</span><span class="sxs-lookup"><span data-stu-id="72cf4-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="72cf4-168">Il problema con questo approccio è che invierebbe tutte le richieste per il nuovo middleware gestore.</span><span class="sxs-lookup"><span data-stu-id="72cf4-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="72cf4-169">Tuttavia, si desidera solo le richieste con una determinata estensione per raggiungere il middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="72cf4-170">Che fornisce la stessa funzionalità che si aveva con il gestore HTTP.</span><span class="sxs-lookup"><span data-stu-id="72cf4-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="72cf4-171">Una soluzione consiste nel creare un ramo della pipeline per le richieste con una determinata estensione utilizzando il `MapWhen` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="72cf4-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="72cf4-172">A tale scopo, lo stesso `Configure` metodo in cui si aggiunge il middleware di altri:</span><span class="sxs-lookup"><span data-stu-id="72cf4-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="72cf4-173">`MapWhen`utilizza questi parametri:</span><span class="sxs-lookup"><span data-stu-id="72cf4-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="72cf4-174">Un'espressione lambda che accetta il `HttpContext` e restituisce `true` se la richiesta deve essere inoltrato al ramo.</span><span class="sxs-lookup"><span data-stu-id="72cf4-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="72cf4-175">Ciò significa che è possibile creare rami di richieste di base non solo l'estensione, ma anche le intestazioni di richiesta, i parametri di stringa di query e così via.</span><span class="sxs-lookup"><span data-stu-id="72cf4-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="72cf4-176">Un'espressione lambda che accetta un `IApplicationBuilder` e aggiunge il middleware di tutti per il ramo.</span><span class="sxs-lookup"><span data-stu-id="72cf4-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="72cf4-177">Ciò significa che è possibile aggiungere altro middleware per il ramo davanti del middleware di gestore.</span><span class="sxs-lookup"><span data-stu-id="72cf4-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="72cf4-178">Middleware aggiunto alla pipeline prima che il ramo verrà richiamato in tutte le richieste; il ramo avrà alcun effetto su di essi.</span><span class="sxs-lookup"><span data-stu-id="72cf4-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="72cf4-179">Il caricamento di opzioni del middleware usando il modello di opzioni</span><span class="sxs-lookup"><span data-stu-id="72cf4-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="72cf4-180">Alcuni gestori e i moduli sono disponibili opzioni di configurazione che vengono archiviate in *Web. config*. Tuttavia, in ASP.NET Core viene utilizzato un nuovo modello di configurazione al posto di *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="72cf4-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="72cf4-181">Il nuovo [sistema di configurazione](xref:fundamentals/configuration/index) fornisce le seguenti opzioni per risolvere il problema:</span><span class="sxs-lookup"><span data-stu-id="72cf4-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="72cf4-182">Inserire direttamente le opzioni in middleware, come illustrato nel [nella sezione successiva](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="72cf4-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="72cf4-183">Utilizzare il [modello opzioni](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="72cf4-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1.  <span data-ttu-id="72cf4-184">Creare una classe che contenga le opzioni del middleware, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="72cf4-184">Create a class to hold your middleware options, for example:</span></span>

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  <span data-ttu-id="72cf4-185">Archiviare i valori di opzione</span><span class="sxs-lookup"><span data-stu-id="72cf4-185">Store the option values</span></span>

    <span data-ttu-id="72cf4-186">Il sistema di configurazione consente di archiviare i valori di opzione in qualsiasi punto desiderato.</span><span class="sxs-lookup"><span data-stu-id="72cf4-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="72cf4-187">Tuttavia, più siti utilizzare *appSettings. JSON*, pertanto l'utente verrà reindirizzato a tale approccio:</span><span class="sxs-lookup"><span data-stu-id="72cf4-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    <span data-ttu-id="72cf4-188">*MyMiddlewareOptionsSection* qui è un nome di sezione.</span><span class="sxs-lookup"><span data-stu-id="72cf4-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="72cf4-189">Non deve essere identico al nome della classe di opzioni.</span><span class="sxs-lookup"><span data-stu-id="72cf4-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="72cf4-190">Associare i valori dell'opzione con la classe di opzioni</span><span class="sxs-lookup"><span data-stu-id="72cf4-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="72cf4-191">Il modello di opzioni Usa framework della ASP.NET Core dipendenza attacco intrusivo nel codice per associare il tipo di opzioni (ad esempio `MyMiddlewareOptions`) con un `MyMiddlewareOptions` oggetto con le opzioni.</span><span class="sxs-lookup"><span data-stu-id="72cf4-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="72cf4-192">Aggiornamento del `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="72cf4-192">Update your `Startup` class:</span></span>

    1.  <span data-ttu-id="72cf4-193">Se si utilizza *appSettings. JSON*, aggiungerlo al generatore di configurazione nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="72cf4-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  <span data-ttu-id="72cf4-194">Configurare il servizio di opzioni:</span><span class="sxs-lookup"><span data-stu-id="72cf4-194">Configure the options service:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  <span data-ttu-id="72cf4-195">Associare le opzioni disponibili con la classe di opzioni:</span><span class="sxs-lookup"><span data-stu-id="72cf4-195">Associate your options with your options class:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  <span data-ttu-id="72cf4-196">Inserire le opzioni nel costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="72cf4-197">È simile per l'inserimento di opzioni in un controller.</span><span class="sxs-lookup"><span data-stu-id="72cf4-197">This is similar to injecting options into a controller.</span></span>

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  <span data-ttu-id="72cf4-198">Il [UseMiddleware](#http-modules-usemiddleware) metodo di estensione che aggiunge il middleware per la `IApplicationBuilder` si occupa di inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="72cf4-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

  <span data-ttu-id="72cf4-199">Questo non è limitato a `IOptions` oggetti.</span><span class="sxs-lookup"><span data-stu-id="72cf4-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="72cf4-200">In questo modo può essere inserito da qualsiasi altro oggetto che richiede il middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="72cf4-201">Il caricamento di opzioni del middleware tramite inserimento diretto</span><span class="sxs-lookup"><span data-stu-id="72cf4-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="72cf4-202">Il modello di opzioni presenta il vantaggio che crea separato accoppiamento tra i valori delle opzioni e utenti.</span><span class="sxs-lookup"><span data-stu-id="72cf4-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="72cf4-203">Una volta associato a una classe di opzioni i valori delle opzioni effettivo, qualsiasi altra classe può accedere alle opzioni tramite il framework di inserimento di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="72cf4-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="72cf4-204">Non è necessario passare intorno ai valori di opzioni.</span><span class="sxs-lookup"><span data-stu-id="72cf4-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="72cf4-205">Questo modo si ottengono tuttavia se si desidera utilizzare il middleware stesso due volte, con opzioni diverse.</span><span class="sxs-lookup"><span data-stu-id="72cf4-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="72cf4-206">Ad esempio un'autorizzazione middleware utilizzato in diversi rami che consente di ruoli diversi.</span><span class="sxs-lookup"><span data-stu-id="72cf4-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="72cf4-207">È possibile associare i due oggetti diverse opzioni con la classe di opzioni di uno.</span><span class="sxs-lookup"><span data-stu-id="72cf4-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="72cf4-208">La soluzione consiste nell'ottenere gli oggetti di opzioni con i valori effettivi opzioni la `Startup` classe e passare direttamente in ogni istanza del middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1.  <span data-ttu-id="72cf4-209">Aggiungere una seconda chiave da *appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="72cf4-209">Add a second key to *appsettings.json*</span></span>

    <span data-ttu-id="72cf4-210">Per aggiungere un secondo set di opzioni per il *appSettings. JSON* file, utilizzare una nuova chiave per identificarla in modo univoco:</span><span class="sxs-lookup"><span data-stu-id="72cf4-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  <span data-ttu-id="72cf4-211">Recuperare i valori delle opzioni e passarle al middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="72cf4-212">Il `Use...` metodo di estensione (che aggiunge il middleware alla pipeline) è un punto logico per passare i valori di opzione:</span><span class="sxs-lookup"><span data-stu-id="72cf4-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  <span data-ttu-id="72cf4-213">Abilitare il middleware accettare un parametro di opzioni.</span><span class="sxs-lookup"><span data-stu-id="72cf4-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="72cf4-214">Fornire un overload di `Use...` metodo di estensione (che accetta il parametro di opzioni e lo passa al `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="72cf4-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="72cf4-215">Quando `UseMiddleware` viene chiamato con parametri, passa i parametri del costruttore del middleware quando viene creata un'istanza dell'oggetto middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    <span data-ttu-id="72cf4-216">Si noti come questo include l'oggetto di opzioni in un `OptionsWrapper` oggetto.</span><span class="sxs-lookup"><span data-stu-id="72cf4-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="72cf4-217">Implementa `IOptions`, come previsto dal costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="72cf4-218">La migrazione a nuovi HttpContext</span><span class="sxs-lookup"><span data-stu-id="72cf4-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="72cf4-219">Illustrato in precedenza che il `Invoke` del middleware metodo accetta un parametro di tipo `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="72cf4-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="72cf4-220">`HttpContext`è stato modificato in modo significativo in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72cf4-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="72cf4-221">In questa sezione viene illustrato come convertire le proprietà utilizzate più di frequente di [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) al nuovo `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="72cf4-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="72cf4-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="72cf4-222">HttpContext</span></span>

<span data-ttu-id="72cf4-223">**HttpContext. Items** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="72cf4-224">**ID richiesta univoco (non controparte System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="72cf4-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="72cf4-225">Fornisce un id univoco per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="72cf4-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="72cf4-226">Molto utile da includere nei file registro.</span><span class="sxs-lookup"><span data-stu-id="72cf4-226">Very useful to include in your logs.</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="72cf4-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="72cf4-227">HttpContext.Request</span></span>

<span data-ttu-id="72cf4-228">**HttpContext.Request.HttpMethod** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="72cf4-229">**HttpContext.Request.QueryString** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="72cf4-230">**HttpContext.Request.Url** e **HttpContext.Request.RawUrl** Traduci in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="72cf4-231">**HttpContext.Request.IsSecureConnection** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="72cf4-232">**HttpContext.Request.UserHostAddress** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="72cf4-233">**HttpContext.Request.Cookies** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="72cf4-234">**HttpContext.Request.RequestContext.RouteData** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="72cf4-235">**HttpContext.Request.Headers** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="72cf4-236">**HttpContext.Request.UserAgent** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="72cf4-237">**HttpContext.Request.UrlReferrer** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="72cf4-238">**HttpContext.Request.ContentType** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="72cf4-239">**HttpContext.Request.Form** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="72cf4-240">Leggere i valori del form solo se il tipo di contenuto sub *x-www-form-urlencoded* o *form-data*.</span><span class="sxs-lookup"><span data-stu-id="72cf4-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="72cf4-241">**HttpContext.Request.InputStream** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="72cf4-242">Utilizzare questo codice solo in un middleware di tipo di gestore, alla fine di una pipeline.</span><span class="sxs-lookup"><span data-stu-id="72cf4-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="72cf4-243">È possibile leggere il corpo non elaborato, come illustrato in precedenza una sola volta per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="72cf4-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="72cf4-244">Tentativo di leggere il corpo dopo la prima lettura middleware leggerà un corpo vuoto.</span><span class="sxs-lookup"><span data-stu-id="72cf4-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="72cf4-245">Questo non si applica alla lettura di un modulo come illustrato in precedenza, perché questa operazione viene effettuata da un buffer.</span><span class="sxs-lookup"><span data-stu-id="72cf4-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="72cf4-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="72cf4-246">HttpContext.Response</span></span>

<span data-ttu-id="72cf4-247">**HttpContext.Response.Status** e **HttpContext.Response.StatusDescription** Traduci in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="72cf4-248">**HttpContext.Response.ContentEncoding** e **HttpContext.Response.ContentType** Traduci in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="72cf4-249">**HttpContext.Response.ContentType** sul proprio anche viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="72cf4-250">**HttpContext.Response.Output** viene convertito in:</span><span class="sxs-lookup"><span data-stu-id="72cf4-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="72cf4-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="72cf4-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="72cf4-252">Disporre di un file è descritto [qui](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="72cf4-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="72cf4-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="72cf4-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="72cf4-254">L'invio delle intestazioni di risposta è complicato dal fatto che se si imposta dopo qualsiasi elemento è stato scritto per il corpo della risposta, non verrà inviati.</span><span class="sxs-lookup"><span data-stu-id="72cf4-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="72cf4-255">La soluzione consiste nell'impostare un metodo di callback che verrà chiamato destra prima della scrittura dell'inizio della risposta.</span><span class="sxs-lookup"><span data-stu-id="72cf4-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="72cf4-256">Questo è più adatto all'inizio del `Invoke` metodo del middleware.</span><span class="sxs-lookup"><span data-stu-id="72cf4-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="72cf4-257">È il metodo di callback che imposta le intestazioni di risposta.</span><span class="sxs-lookup"><span data-stu-id="72cf4-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="72cf4-258">Il codice seguente imposta un metodo di callback chiamato `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="72cf4-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="72cf4-259">Il `SetHeaders` metodo di callback è analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="72cf4-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="72cf4-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="72cf4-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="72cf4-261">Si spostano i cookie nel browser in un *Set-Cookie* intestazione della risposta.</span><span class="sxs-lookup"><span data-stu-id="72cf4-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="72cf4-262">Di conseguenza, inviare cookie richiede il callback stesso utilizzato per l'invio delle intestazioni di risposta:</span><span class="sxs-lookup"><span data-stu-id="72cf4-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="72cf4-263">Il `SetCookies` metodo di callback avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="72cf4-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="72cf4-264">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="72cf4-264">Additional Resources</span></span>

* [<span data-ttu-id="72cf4-265">Panoramica di moduli HTTP e i gestori HTTP</span><span class="sxs-lookup"><span data-stu-id="72cf4-265">HTTP Handlers and HTTP Modules Overview</span></span>](https://docs.microsoft.com/iis/configuration/system.webserver/)

* [<span data-ttu-id="72cf4-266">Configurazione</span><span class="sxs-lookup"><span data-stu-id="72cf4-266">Configuration</span></span>](xref:fundamentals/configuration/index)

* [<span data-ttu-id="72cf4-267">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="72cf4-267">Application Startup</span></span>](../fundamentals/startup.md)

* [<span data-ttu-id="72cf4-268">Middleware</span><span class="sxs-lookup"><span data-stu-id="72cf4-268">Middleware</span></span>](../fundamentals/middleware.md)
