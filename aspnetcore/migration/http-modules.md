---
title: Eseguire la migrazione di moduli e gestori HTTP in middleware di ASP.NET Core
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 9dd28b86966912cce87166feb37e65adf3dd6dcb
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902671"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="c95cd-102">Eseguire la migrazione di moduli e gestori HTTP in middleware di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c95cd-102">Migrate HTTP handlers and modules to ASP.NET Core middleware</span></span>

<span data-ttu-id="c95cd-103">Da [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="c95cd-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="c95cd-104">Questo articolo illustra come eseguire la migrazione ASP.NET esistenti [moduli HTTP e i gestori da System. webServer](/iis/configuration/system.webserver/) ad ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="c95cd-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="c95cd-105">I moduli e gestori Rivisitazione della</span><span class="sxs-lookup"><span data-stu-id="c95cd-105">Modules and handlers revisited</span></span>

<span data-ttu-id="c95cd-106">Prima di procedere al middleware di ASP.NET Core, è opportuno innanzitutto riepilogare come funzionano i gestori e moduli HTTP:</span><span class="sxs-lookup"><span data-stu-id="c95cd-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Gestore di moduli](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="c95cd-108">**I gestori sono:**</span><span class="sxs-lookup"><span data-stu-id="c95cd-108">**Handlers are:**</span></span>

   * <span data-ttu-id="c95cd-109">Le classi che implementano [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="c95cd-109">Classes that implement [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="c95cd-110">Consente di gestire le richieste con un nome file specificato o l'estensione, ad esempio *.report*</span><span class="sxs-lookup"><span data-stu-id="c95cd-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="c95cd-111">[Configurata](/iis/configuration/system.webserver/handlers/) in *Web. config*</span><span class="sxs-lookup"><span data-stu-id="c95cd-111">[Configured](/iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="c95cd-112">**I moduli sono:**</span><span class="sxs-lookup"><span data-stu-id="c95cd-112">**Modules are:**</span></span>

   * <span data-ttu-id="c95cd-113">Le classi che implementano [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="c95cd-113">Classes that implement [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="c95cd-114">Richiamato per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="c95cd-114">Invoked for every request</span></span>

   * <span data-ttu-id="c95cd-115">In grado di corto circuito (interrompere ulteriore elaborazione di una richiesta)</span><span class="sxs-lookup"><span data-stu-id="c95cd-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="c95cd-116">Possibilità di aggiungere alla risposta HTTP, o crearne di propri</span><span class="sxs-lookup"><span data-stu-id="c95cd-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="c95cd-117">[Configurata](/iis/configuration/system.webserver/modules/) in *Web. config*</span><span class="sxs-lookup"><span data-stu-id="c95cd-117">[Configured](/iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="c95cd-118">**L'ordine in cui i moduli elaborino le richieste in ingresso è determinato da:**</span><span class="sxs-lookup"><span data-stu-id="c95cd-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="c95cd-119">Il [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx), ovvero un eventi della serie generato da ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)e così via. Ogni modulo è possibile creare un gestore per uno o più eventi.</span><span class="sxs-lookup"><span data-stu-id="c95cd-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="c95cd-120">Per lo stesso evento, l'ordine in cui configurarli nel *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="c95cd-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="c95cd-121">Oltre ai moduli, è possibile aggiungere gestori per gli eventi del ciclo di vita per le *Global.asax.cs* file.</span><span class="sxs-lookup"><span data-stu-id="c95cd-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="c95cd-122">Dopo i gestori nei moduli configurati, eseguire questi gestori.</span><span class="sxs-lookup"><span data-stu-id="c95cd-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="c95cd-123">Da gestori e moduli in middleware</span><span class="sxs-lookup"><span data-stu-id="c95cd-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="c95cd-124">**Middleware sono più semplici rispetto a gestori e moduli HTTP:**</span><span class="sxs-lookup"><span data-stu-id="c95cd-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="c95cd-125">I moduli, gestori *Global.asax.cs*, *Web. config* (ad eccezione di configurazione di IIS) e il ciclo di vita dell'applicazione sono stati eliminati</span><span class="sxs-lookup"><span data-stu-id="c95cd-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="c95cd-126">I ruoli di entrambi i moduli e gestori sono stati eseguiti al middleware</span><span class="sxs-lookup"><span data-stu-id="c95cd-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="c95cd-127">Middleware vengono configurati utilizzando codice anziché in *Web. config*</span><span class="sxs-lookup"><span data-stu-id="c95cd-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="c95cd-128">[Creazione di rami pipeline](xref:fundamentals/middleware/index#use-run-and-map) consente di inviare richieste al middleware specifico, basato sull'URL non solo ma anche in intestazioni della richiesta, le stringhe di query, e così via.</span><span class="sxs-lookup"><span data-stu-id="c95cd-128">[Pipeline branching](xref:fundamentals/middleware/index#use-run-and-map) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="c95cd-129">**Middleware sono molto simili ai moduli:**</span><span class="sxs-lookup"><span data-stu-id="c95cd-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="c95cd-130">Richiamato in linea di principio per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="c95cd-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="c95cd-131">In grado di corto circuito da una richiesta, [non trasmettere la richiesta al middleware successivo](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="c95cd-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="c95cd-132">In grado di creare le proprie risposte HTTP</span><span class="sxs-lookup"><span data-stu-id="c95cd-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="c95cd-133">**Middleware e i moduli vengono elaborati in un ordine diverso:**</span><span class="sxs-lookup"><span data-stu-id="c95cd-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="c95cd-134">Ordine del middleware è in base all'ordine in cui viene inserito nel pipeline delle richieste, mentre l'ordine dei moduli si basa principalmente sulla [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx) eventi</span><span class="sxs-lookup"><span data-stu-id="c95cd-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="c95cd-135">Ordine del middleware per le risposte è l'opposto rispetto a quello per le richieste, mentre l'ordine dei moduli è lo stesso per le richieste e risposte</span><span class="sxs-lookup"><span data-stu-id="c95cd-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="c95cd-136">Vedere [creare una pipeline middleware con IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="c95cd-136">See [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Middleware](http-modules/_static/middleware.png)

<span data-ttu-id="c95cd-138">Si noti come nell'immagine precedente, il middleware di autenticazione bloccata la richiesta.</span><span class="sxs-lookup"><span data-stu-id="c95cd-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="c95cd-139">Migrazione del codice modulo al middleware</span><span class="sxs-lookup"><span data-stu-id="c95cd-139">Migrating module code to middleware</span></span>

<span data-ttu-id="c95cd-140">Un modulo HTTP esistente avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c95cd-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="c95cd-141">Come illustrato nel [Middleware](xref:fundamentals/middleware/index) pagina, un middleware di ASP.NET Core è una classe che espone un `Invoke` prendendo metodo un `HttpContext` e restituendo un `Task`.</span><span class="sxs-lookup"><span data-stu-id="c95cd-141">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="c95cd-142">Il middleware nuovo avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c95cd-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="c95cd-143">Il modello di middleware precedente è stato creato nella sezione [scrittura di middleware](xref:fundamentals/middleware/index#write-middleware).</span><span class="sxs-lookup"><span data-stu-id="c95cd-143">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/index#write-middleware).</span></span>

<span data-ttu-id="c95cd-144">Il *MyMiddlewareExtensions* rende più semplice configurare il middleware nella classe helper di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="c95cd-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="c95cd-145">Il `UseMyMiddleware` metodo aggiunge una classe middleware alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="c95cd-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="c95cd-146">I servizi richiesti dal middleware ottengano inseriti nel costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="c95cd-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="c95cd-147">Il modulo potrebbe infatti terminare una richiesta, se l'utente non è stata autorizzata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c95cd-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="c95cd-148">Un tipo di middleware viene gestita tramite la chiamata non `Invoke` il middleware successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="c95cd-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="c95cd-149">Tenere presente che questo non termina completamente la richiesta, poiché il middleware precedente verrà ancora richiamata quando la risposta arrivi attraverso la pipeline.</span><span class="sxs-lookup"><span data-stu-id="c95cd-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="c95cd-150">Quando si esegue la migrazione delle funzionalità del modulo per il middleware di nuovo, è possibile che il codice non viene compilato perché il `HttpContext` classe ha subito modifiche significative in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c95cd-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="c95cd-151">[In un secondo momento](#migrating-to-the-new-httpcontext), scoprirai come eseguire la migrazione al nuovo ASP.NET Core HttpContext.</span><span class="sxs-lookup"><span data-stu-id="c95cd-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="c95cd-152">La migrazione, l'inserimento modulo nella pipeline delle richieste</span><span class="sxs-lookup"><span data-stu-id="c95cd-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="c95cd-153">In genere vengono aggiunti i moduli HTTP per la richiesta di pipeline tramite *Web. config*:</span><span class="sxs-lookup"><span data-stu-id="c95cd-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="c95cd-154">This per convertire [aggiungere il middleware di nuovo](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) alla pipeline delle richieste nel `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="c95cd-154">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="c95cd-155">Punto nella pipeline in cui si inserisce il middleware di nuovo varia a seconda dell'evento che gestito come modulo (`BeginRequest`, `EndRequest`e così via) e l'ordine nell'elenco dei moduli nel *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="c95cd-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="c95cd-156">Come già non indicato, non contiene alcun ciclo di vita dell'applicazione ASP.NET Core e l'ordine in cui le risposte vengono elaborate dal middleware differente da quello usato dai moduli.</span><span class="sxs-lookup"><span data-stu-id="c95cd-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="c95cd-157">Ciò potrebbe rendere la decisione di ordinamento più impegnativo.</span><span class="sxs-lookup"><span data-stu-id="c95cd-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="c95cd-158">Se l'ordinamento diventa un problema, è possibile suddividere un modulo in più componenti middleware che possono essere ordinati in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="c95cd-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="c95cd-159">Migrazione del codice del gestore in middleware</span><span class="sxs-lookup"><span data-stu-id="c95cd-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="c95cd-160">Un gestore HTTP simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c95cd-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="c95cd-161">Nel progetto ASP.NET Core, è necessario convertire in un middleware simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c95cd-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="c95cd-162">Questo middleware è molto simile al middleware corrispondente ai moduli.</span><span class="sxs-lookup"><span data-stu-id="c95cd-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="c95cd-163">L'unica vera differenza è che seguito non vi è alcuna chiamata a `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="c95cd-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="c95cd-164">Che ha senso, perché il gestore di è alla fine della pipeline delle richieste, pertanto sarà presente alcun middleware successivo da richiamare.</span><span class="sxs-lookup"><span data-stu-id="c95cd-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="c95cd-165">La migrazione, l'inserimento del gestore nella pipeline delle richieste</span><span class="sxs-lookup"><span data-stu-id="c95cd-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="c95cd-166">Configurazione di un gestore HTTP viene eseguita in *Web. config* e ha un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c95cd-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="c95cd-167">È possibile convertire questo aggiungendo il middleware di gestore di nuovo alla pipeline delle richieste nel `Startup` (classe), simile al middleware convertito da moduli.</span><span class="sxs-lookup"><span data-stu-id="c95cd-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="c95cd-168">Il problema con questo approccio è che lo invierà tutte le richieste per il middleware di gestore di nuovo.</span><span class="sxs-lookup"><span data-stu-id="c95cd-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="c95cd-169">Tuttavia, è necessario solo alle richieste con una determinata estensione di raggiungere il middleware.</span><span class="sxs-lookup"><span data-stu-id="c95cd-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="c95cd-170">Che consente la stessa funzionalità che è necessario con il gestore HTTP.</span><span class="sxs-lookup"><span data-stu-id="c95cd-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="c95cd-171">Una soluzione consiste nel creare un ramo della pipeline per le richieste con una determinata estensione, usando il `MapWhen` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="c95cd-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="c95cd-172">A tale scopo, lo stesso `Configure` metodo in cui si aggiunge l'altro middleware:</span><span class="sxs-lookup"><span data-stu-id="c95cd-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="c95cd-173">`MapWhen` utilizza questi parametri:</span><span class="sxs-lookup"><span data-stu-id="c95cd-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="c95cd-174">Un'espressione lambda che accetta il `HttpContext` e restituisce `true` se la richiesta deve passare al ramo.</span><span class="sxs-lookup"><span data-stu-id="c95cd-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="c95cd-175">Ciò significa che è possibile creare una diramazione delle richieste non solo in base all'estensione, ma anche le intestazioni di richiesta parametri della stringa di query, e così via.</span><span class="sxs-lookup"><span data-stu-id="c95cd-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="c95cd-176">Un'espressione lambda che accetta un `IApplicationBuilder` e aggiunge il middleware di tutti per il ramo.</span><span class="sxs-lookup"><span data-stu-id="c95cd-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="c95cd-177">Ciò significa che è possibile aggiungere altro middleware per il ramo davanti il middleware di gestore.</span><span class="sxs-lookup"><span data-stu-id="c95cd-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="c95cd-178">Middleware aggiunto alla pipeline prima che il ramo verrà richiamato in tutte le richieste; il ramo avrà alcun effetto su di essi.</span><span class="sxs-lookup"><span data-stu-id="c95cd-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="c95cd-179">Le opzioni del middleware adottando il modello di opzioni di caricamento</span><span class="sxs-lookup"><span data-stu-id="c95cd-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="c95cd-180">Alcuni moduli e gestori hanno opzioni di configurazione che vengono archiviate nel *Web. config*. Tuttavia, in ASP.NET Core viene usato un nuovo modello di configurazione al posto di *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="c95cd-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="c95cd-181">Il nuovo [sistema di configurazione](xref:fundamentals/configuration/index) fornisce le seguenti opzioni per risolvere il problema:</span><span class="sxs-lookup"><span data-stu-id="c95cd-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="c95cd-182">Inserire direttamente le opzioni nel middleware, come illustrato nella [nella sezione successiva](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="c95cd-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="c95cd-183">Usare la [modello di opzioni](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="c95cd-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1. <span data-ttu-id="c95cd-184">Creare una classe che contenga le opzioni del middleware, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c95cd-184">Create a class to hold your middleware options, for example:</span></span>

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. <span data-ttu-id="c95cd-185">I valori delle opzioni di Store</span><span class="sxs-lookup"><span data-stu-id="c95cd-185">Store the option values</span></span>

   <span data-ttu-id="c95cd-186">Il sistema di configurazione consente di archiviare i valori delle opzioni in qualsiasi punto desiderato.</span><span class="sxs-lookup"><span data-stu-id="c95cd-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="c95cd-187">Tuttavia, i siti più utilizzare *appSettings. JSON*, pertanto è possibile adottare un tale approccio:</span><span class="sxs-lookup"><span data-stu-id="c95cd-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   <span data-ttu-id="c95cd-188">*MyMiddlewareOptionsSection* seguito è riportato un nome di sezione.</span><span class="sxs-lookup"><span data-stu-id="c95cd-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="c95cd-189">Non deve necessariamente corrispondere al nome della classe di opzioni.</span><span class="sxs-lookup"><span data-stu-id="c95cd-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="c95cd-190">Associare i valori delle opzioni con la classe di opzioni</span><span class="sxs-lookup"><span data-stu-id="c95cd-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="c95cd-191">Il modello di opzioni Usa i framework di inserimento delle dipendenze di ASP.NET Core per associare il tipo di opzioni (ad esempio `MyMiddlewareOptions`) con un `MyMiddlewareOptions` oggetto con le opzioni effettive.</span><span class="sxs-lookup"><span data-stu-id="c95cd-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="c95cd-192">Aggiornamento di `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="c95cd-192">Update your `Startup` class:</span></span>

   1. <span data-ttu-id="c95cd-193">Se si usa *appSettings. JSON*, aggiungerlo al generatore di configurazione nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="c95cd-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. <span data-ttu-id="c95cd-194">Configurare il servizio di opzioni:</span><span class="sxs-lookup"><span data-stu-id="c95cd-194">Configure the options service:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. <span data-ttu-id="c95cd-195">Associare le opzioni disponibili con la classe di opzioni:</span><span class="sxs-lookup"><span data-stu-id="c95cd-195">Associate your options with your options class:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. <span data-ttu-id="c95cd-196">Inserire le opzioni nel costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="c95cd-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="c95cd-197">È simile all'inserimento di opzioni in un controller.</span><span class="sxs-lookup"><span data-stu-id="c95cd-197">This is similar to injecting options into a controller.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   <span data-ttu-id="c95cd-198">Il [UseMiddleware](#http-modules-usemiddleware) metodo di estensione che aggiunge il middleware per la `IApplicationBuilder` si occupa di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c95cd-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

   <span data-ttu-id="c95cd-199">Non è limitato a `IOptions` oggetti.</span><span class="sxs-lookup"><span data-stu-id="c95cd-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="c95cd-200">Qualsiasi altro oggetto che richiede il middleware può essere inserito in questo modo.</span><span class="sxs-lookup"><span data-stu-id="c95cd-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="c95cd-201">Il caricamento di opzioni del middleware tramite l'inserimento diretto</span><span class="sxs-lookup"><span data-stu-id="c95cd-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="c95cd-202">Il modello di opzioni presenta il vantaggio che crea loose accoppiamento tra i valori delle opzioni e i relativi utenti.</span><span class="sxs-lookup"><span data-stu-id="c95cd-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="c95cd-203">Una volta associato a una classe di opzioni i valori effettivi di opzioni, nessun'altra classe può accedere alle opzioni tramite il framework di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c95cd-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="c95cd-204">Non è necessario passare intorno ai valori di opzioni.</span><span class="sxs-lookup"><span data-stu-id="c95cd-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="c95cd-205">Questo modo si ottengono tuttavia se si desidera usare il middleware stesso due volte, con opzioni diverse.</span><span class="sxs-lookup"><span data-stu-id="c95cd-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="c95cd-206">Ad esempio un middleware di autorizzazione usato in diversi rami che consente diversi ruoli.</span><span class="sxs-lookup"><span data-stu-id="c95cd-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="c95cd-207">È possibile associare due oggetti opzioni diverse con la classe di uno opzioni.</span><span class="sxs-lookup"><span data-stu-id="c95cd-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="c95cd-208">La soluzione consiste nell'ottenere gli oggetti di opzioni con i valori di opzioni effettivo nel `Startup` classe e passarle direttamente a ogni istanza del proprio middleware.</span><span class="sxs-lookup"><span data-stu-id="c95cd-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1. <span data-ttu-id="c95cd-209">Aggiungere una seconda chiave da *appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="c95cd-209">Add a second key to *appsettings.json*</span></span>

   <span data-ttu-id="c95cd-210">Per aggiungere un secondo set di opzioni per la *appSettings. JSON* file, usare una nuova chiave per identificarlo in modo univoco:</span><span class="sxs-lookup"><span data-stu-id="c95cd-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. <span data-ttu-id="c95cd-211">Recuperare i valori delle opzioni e passarle al middleware.</span><span class="sxs-lookup"><span data-stu-id="c95cd-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="c95cd-212">Il `Use...` metodo di estensione (che aggiunge il middleware alla pipeline) è una posizione logica per passare i valori delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="c95cd-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. <span data-ttu-id="c95cd-213">Abilitare il middleware di accettare un parametro di opzioni.</span><span class="sxs-lookup"><span data-stu-id="c95cd-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="c95cd-214">Fornire un overload del `Use...` metodo di estensione (che accetta il parametro options e lo passa al `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="c95cd-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="c95cd-215">Quando si `UseMiddleware` viene chiamato con parametri, passa i parametri al costruttore del middleware quando si crea un'istanza dell'oggetto di middleware.</span><span class="sxs-lookup"><span data-stu-id="c95cd-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   <span data-ttu-id="c95cd-216">Si noti come si esegue il wrapping l'oggetto di opzioni in un `OptionsWrapper` oggetto.</span><span class="sxs-lookup"><span data-stu-id="c95cd-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="c95cd-217">Ciò implementa `IOptions`, come previsto dal costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="c95cd-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="c95cd-218">Eseguire la migrazione a nuovo HttpContext</span><span class="sxs-lookup"><span data-stu-id="c95cd-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="c95cd-219">Illustrato in precedenza che il `Invoke` metodo nel proprio middleware accetta un parametro di tipo `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="c95cd-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="c95cd-220">`HttpContext` è stato modificato in modo significativo in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c95cd-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="c95cd-221">In questa sezione spiega come traslare le proprietà più comunemente utilizzate del [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) al nuovo `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="c95cd-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="c95cd-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="c95cd-222">HttpContext</span></span>

<span data-ttu-id="c95cd-223">**HttpContext. Items** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="c95cd-224">**ID univoco della richiesta (nessuna controparte System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="c95cd-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="c95cd-225">Fornisce un id univoco per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="c95cd-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="c95cd-226">Molto utili da includere nei log.</span><span class="sxs-lookup"><span data-stu-id="c95cd-226">Very useful to include in your logs.</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="c95cd-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="c95cd-227">HttpContext.Request</span></span>

<span data-ttu-id="c95cd-228">**HttpContext.Request.HttpMethod** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="c95cd-229">**HttpContext.Request.QueryString** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="c95cd-230">**HttpContext.Request.Url** e **HttpContext.Request.RawUrl** Traduci da:</span><span class="sxs-lookup"><span data-stu-id="c95cd-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="c95cd-231">**HttpContext.Request.IsSecureConnection** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="c95cd-232">**HttpContext.Request.UserHostAddress** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="c95cd-233">**HttpContext.Request.Cookies** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="c95cd-234">**HttpContext.Request.RequestContext.RouteData** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="c95cd-235">**HttpContext.Request.Headers** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="c95cd-236">**HttpContext.Request.UserAgent** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="c95cd-237">**HttpContext.Request.UrlReferrer** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="c95cd-238">**HttpContext.Request.ContentType** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="c95cd-239">**HttpContext.Request.Form** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="c95cd-240">Leggere i valori del form solo se il tipo di contenuto sub *x-www-form-urlencoded* oppure *form-data*.</span><span class="sxs-lookup"><span data-stu-id="c95cd-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="c95cd-241">**HttpContext.Request.InputStream** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="c95cd-242">Usare questo codice solo in un middleware di tipo gestore, alla fine di una pipeline.</span><span class="sxs-lookup"><span data-stu-id="c95cd-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="c95cd-243">È possibile leggere il corpo non elaborato, come illustrato in precedenza una sola volta per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="c95cd-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="c95cd-244">Tentativo di leggere il corpo dopo la prima lettura middleware leggerà un corpo vuoto.</span><span class="sxs-lookup"><span data-stu-id="c95cd-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="c95cd-245">Ciò non si applica alla lettura di un modulo come illustrato in precedenza, perché l'operazione viene eseguita da un buffer.</span><span class="sxs-lookup"><span data-stu-id="c95cd-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="c95cd-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="c95cd-246">HttpContext.Response</span></span>

<span data-ttu-id="c95cd-247">**HttpContext.Response.Status** e **HttpContext.Response.StatusDescription** Traduci da:</span><span class="sxs-lookup"><span data-stu-id="c95cd-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="c95cd-248">**HttpContext.Response.ContentEncoding** e **HttpContext.Response.ContentType** Traduci da:</span><span class="sxs-lookup"><span data-stu-id="c95cd-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="c95cd-249">**HttpContext.Response.ContentType** sul proprio anche si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="c95cd-250">**HttpContext.Response.Output** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="c95cd-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="c95cd-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="c95cd-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="c95cd-252">Servizio backup di un file viene illustrata [qui](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="c95cd-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="c95cd-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="c95cd-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="c95cd-254">L'invio delle intestazioni di risposta è ulteriormente complicata dal fatto che se si impostano dopo qualsiasi elemento è stato scritto nel corpo della risposta, non verrà inviati.</span><span class="sxs-lookup"><span data-stu-id="c95cd-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="c95cd-255">La soluzione consiste nell'impostare un metodo di callback che verrà chiamato a destra prima della scrittura nei avviata risposta.</span><span class="sxs-lookup"><span data-stu-id="c95cd-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="c95cd-256">Questa operazione viene eseguita meglio all'inizio del `Invoke` metodo nel proprio middleware.</span><span class="sxs-lookup"><span data-stu-id="c95cd-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="c95cd-257">È questo metodo di callback che consente di impostare le intestazioni della risposta.</span><span class="sxs-lookup"><span data-stu-id="c95cd-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="c95cd-258">Il codice seguente imposta un metodo di callback chiamato `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="c95cd-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="c95cd-259">Il `SetHeaders` metodo di callback sarà analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="c95cd-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="c95cd-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="c95cd-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="c95cd-261">I cookie verranno trasmessi al browser in un *Set-Cookie* intestazione della risposta.</span><span class="sxs-lookup"><span data-stu-id="c95cd-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="c95cd-262">Di conseguenza, l'invio di cookie richiede il callback stesso come quelle utilizzate per l'invio delle intestazioni di risposta:</span><span class="sxs-lookup"><span data-stu-id="c95cd-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="c95cd-263">Il `SetCookies` metodo di callback sarebbe simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c95cd-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="c95cd-264">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c95cd-264">Additional resources</span></span>

* [<span data-ttu-id="c95cd-265">Panoramica di moduli HTTP e gestori HTTP</span><span class="sxs-lookup"><span data-stu-id="c95cd-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="c95cd-266">Configurazione</span><span class="sxs-lookup"><span data-stu-id="c95cd-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="c95cd-267">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="c95cd-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="c95cd-268">Middleware</span><span class="sxs-lookup"><span data-stu-id="c95cd-268">Middleware</span></span>](xref:fundamentals/middleware/index)
