---
title: Eseguire la migrazione di moduli e gestori HTTP in middleware di ASP.NET Core
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 84381210910c66a7d121120b8c6b0f046cae8c4f
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207810"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="addc6-102">Eseguire la migrazione di moduli e gestori HTTP in middleware di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="addc6-102">Migrate HTTP handlers and modules to ASP.NET Core middleware</span></span>

<span data-ttu-id="addc6-103">Questo articolo illustra come eseguire la migrazione ASP.NET esistenti [moduli HTTP e i gestori da System. webServer](/iis/configuration/system.webserver/) ad ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="addc6-103">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="addc6-104">I moduli e gestori Rivisitazione della</span><span class="sxs-lookup"><span data-stu-id="addc6-104">Modules and handlers revisited</span></span>

<span data-ttu-id="addc6-105">Prima di procedere al middleware di ASP.NET Core, è opportuno innanzitutto riepilogare come funzionano i gestori e moduli HTTP:</span><span class="sxs-lookup"><span data-stu-id="addc6-105">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Gestore di moduli](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="addc6-107">**I gestori sono:**</span><span class="sxs-lookup"><span data-stu-id="addc6-107">**Handlers are:**</span></span>

* <span data-ttu-id="addc6-108">Le classi che implementano [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="addc6-108">Classes that implement [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span></span>

* <span data-ttu-id="addc6-109">Consente di gestire le richieste con un nome file specificato o l'estensione, ad esempio *.report*</span><span class="sxs-lookup"><span data-stu-id="addc6-109">Used to handle requests with a given file name or extension, such as *.report*</span></span>

* <span data-ttu-id="addc6-110">[Configurata](/iis/configuration/system.webserver/handlers/) in *Web. config*</span><span class="sxs-lookup"><span data-stu-id="addc6-110">[Configured](/iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="addc6-111">**I moduli sono:**</span><span class="sxs-lookup"><span data-stu-id="addc6-111">**Modules are:**</span></span>

* <span data-ttu-id="addc6-112">Le classi che implementano [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="addc6-112">Classes that implement [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span></span>

* <span data-ttu-id="addc6-113">Richiamato per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="addc6-113">Invoked for every request</span></span>

* <span data-ttu-id="addc6-114">In grado di corto circuito (interrompere ulteriore elaborazione di una richiesta)</span><span class="sxs-lookup"><span data-stu-id="addc6-114">Able to short-circuit (stop further processing of a request)</span></span>

* <span data-ttu-id="addc6-115">Possibilità di aggiungere alla risposta HTTP, o crearne di propri</span><span class="sxs-lookup"><span data-stu-id="addc6-115">Able to add to the HTTP response, or create their own</span></span>

* <span data-ttu-id="addc6-116">[Configurata](/iis/configuration/system.webserver/modules/) in *Web. config*</span><span class="sxs-lookup"><span data-stu-id="addc6-116">[Configured](/iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="addc6-117">**L'ordine in cui i moduli elaborino le richieste in ingresso è determinato da:**</span><span class="sxs-lookup"><span data-stu-id="addc6-117">**The order in which modules process incoming requests is determined by:**</span></span>

1. <span data-ttu-id="addc6-118">Il [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx), ovvero un eventi della serie generato da ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Ogni modulo è possibile creare un gestore per uno o più eventi.</span><span class="sxs-lookup"><span data-stu-id="addc6-118">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

2. <span data-ttu-id="addc6-119">Per lo stesso evento, l'ordine in cui configurarli nel *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="addc6-119">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="addc6-120">Oltre ai moduli, è possibile aggiungere gestori per gli eventi del ciclo di vita per le *Global.asax.cs* file.</span><span class="sxs-lookup"><span data-stu-id="addc6-120">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="addc6-121">Dopo i gestori nei moduli configurati, eseguire questi gestori.</span><span class="sxs-lookup"><span data-stu-id="addc6-121">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="addc6-122">Da gestori e moduli in middleware</span><span class="sxs-lookup"><span data-stu-id="addc6-122">From handlers and modules to middleware</span></span>

<span data-ttu-id="addc6-123">**Middleware sono più semplici rispetto a gestori e moduli HTTP:**</span><span class="sxs-lookup"><span data-stu-id="addc6-123">**Middleware are simpler than HTTP modules and handlers:**</span></span>

* <span data-ttu-id="addc6-124">I moduli, gestori *Global.asax.cs*, *Web. config* (ad eccezione di configurazione di IIS) e il ciclo di vita dell'applicazione sono stati eliminati</span><span class="sxs-lookup"><span data-stu-id="addc6-124">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

* <span data-ttu-id="addc6-125">I ruoli di entrambi i moduli e gestori sono stati eseguiti al middleware</span><span class="sxs-lookup"><span data-stu-id="addc6-125">The roles of both modules and handlers have been taken over by middleware</span></span>

* <span data-ttu-id="addc6-126">Middleware vengono configurati utilizzando codice anziché in *Web. config*</span><span class="sxs-lookup"><span data-stu-id="addc6-126">Middleware are configured using code rather than in *Web.config*</span></span>

* <span data-ttu-id="addc6-127">[Creazione di rami pipeline](xref:fundamentals/middleware/index#use-run-and-map) consente di inviare richieste al middleware specifico, basato sull'URL non solo ma anche in intestazioni della richiesta, le stringhe di query, e così via.</span><span class="sxs-lookup"><span data-stu-id="addc6-127">[Pipeline branching](xref:fundamentals/middleware/index#use-run-and-map) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="addc6-128">**Middleware sono molto simili ai moduli:**</span><span class="sxs-lookup"><span data-stu-id="addc6-128">**Middleware are very similar to modules:**</span></span>

* <span data-ttu-id="addc6-129">Richiamato in linea di principio per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="addc6-129">Invoked in principle for every request</span></span>

* <span data-ttu-id="addc6-130">In grado di corto circuito da una richiesta, [non trasmettere la richiesta al middleware successivo](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="addc6-130">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

* <span data-ttu-id="addc6-131">In grado di creare le proprie risposte HTTP</span><span class="sxs-lookup"><span data-stu-id="addc6-131">Able to create their own HTTP response</span></span>

<span data-ttu-id="addc6-132">**Middleware e i moduli vengono elaborati in un ordine diverso:**</span><span class="sxs-lookup"><span data-stu-id="addc6-132">**Middleware and modules are processed in a different order:**</span></span>

* <span data-ttu-id="addc6-133">Ordine del middleware è in base all'ordine in cui viene inserito nel pipeline delle richieste, mentre l'ordine dei moduli si basa principalmente sulla [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx) eventi</span><span class="sxs-lookup"><span data-stu-id="addc6-133">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

* <span data-ttu-id="addc6-134">Ordine del middleware per le risposte è l'opposto rispetto a quello per le richieste, mentre l'ordine dei moduli è lo stesso per le richieste e risposte</span><span class="sxs-lookup"><span data-stu-id="addc6-134">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

* <span data-ttu-id="addc6-135">Vedere [creare una pipeline middleware con IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="addc6-135">See [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Middleware](http-modules/_static/middleware.png)

<span data-ttu-id="addc6-137">Si noti come nell'immagine precedente, il middleware di autenticazione bloccata la richiesta.</span><span class="sxs-lookup"><span data-stu-id="addc6-137">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="addc6-138">Migrazione del codice modulo al middleware</span><span class="sxs-lookup"><span data-stu-id="addc6-138">Migrating module code to middleware</span></span>

<span data-ttu-id="addc6-139">Un modulo HTTP esistente avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="addc6-139">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="addc6-140">Come illustrato nel [Middleware](xref:fundamentals/middleware/index) pagina, un middleware di ASP.NET Core è una classe che espone un `Invoke` prendendo metodo un `HttpContext` e restituendo un `Task`.</span><span class="sxs-lookup"><span data-stu-id="addc6-140">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="addc6-141">Il middleware nuovo avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="addc6-141">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="addc6-142">Il modello di middleware precedente è stato creato nella sezione [scrittura di middleware](xref:fundamentals/middleware/write).</span><span class="sxs-lookup"><span data-stu-id="addc6-142">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/write).</span></span>

<span data-ttu-id="addc6-143">Il *MyMiddlewareExtensions* rende più semplice configurare il middleware nella classe helper di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="addc6-143">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="addc6-144">Il `UseMyMiddleware` metodo aggiunge una classe middleware alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="addc6-144">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="addc6-145">I servizi richiesti dal middleware ottengano inseriti nel costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="addc6-145">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="addc6-146">Il modulo potrebbe infatti terminare una richiesta, se l'utente non è stata autorizzata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="addc6-146">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="addc6-147">Un tipo di middleware viene gestita tramite la chiamata non `Invoke` il middleware successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="addc6-147">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="addc6-148">Tenere presente che questo non termina completamente la richiesta, poiché il middleware precedente verrà ancora richiamata quando la risposta arrivi attraverso la pipeline.</span><span class="sxs-lookup"><span data-stu-id="addc6-148">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="addc6-149">Quando si esegue la migrazione delle funzionalità del modulo per il middleware di nuovo, è possibile che il codice non viene compilato perché il `HttpContext` classe ha subito modifiche significative in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="addc6-149">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="addc6-150">[In un secondo momento](#migrating-to-the-new-httpcontext), scoprirai come eseguire la migrazione al nuovo ASP.NET Core HttpContext.</span><span class="sxs-lookup"><span data-stu-id="addc6-150">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="addc6-151">La migrazione, l'inserimento modulo nella pipeline delle richieste</span><span class="sxs-lookup"><span data-stu-id="addc6-151">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="addc6-152">In genere vengono aggiunti i moduli HTTP per la richiesta di pipeline tramite *Web. config*:</span><span class="sxs-lookup"><span data-stu-id="addc6-152">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="addc6-153">This per convertire [aggiungere il middleware di nuovo](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) alla pipeline delle richieste nel `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="addc6-153">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="addc6-154">Punto nella pipeline in cui si inserisce il middleware di nuovo varia a seconda dell'evento che gestito come modulo (`BeginRequest`, `EndRequest`e così via) e l'ordine nell'elenco dei moduli nel *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="addc6-154">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="addc6-155">Come già non indicato, non contiene alcun ciclo di vita dell'applicazione ASP.NET Core e l'ordine in cui le risposte vengono elaborate dal middleware differente da quello usato dai moduli.</span><span class="sxs-lookup"><span data-stu-id="addc6-155">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="addc6-156">Ciò potrebbe rendere la decisione di ordinamento più impegnativo.</span><span class="sxs-lookup"><span data-stu-id="addc6-156">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="addc6-157">Se l'ordinamento diventa un problema, è possibile suddividere un modulo in più componenti middleware che possono essere ordinati in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="addc6-157">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="addc6-158">Migrazione del codice del gestore in middleware</span><span class="sxs-lookup"><span data-stu-id="addc6-158">Migrating handler code to middleware</span></span>

<span data-ttu-id="addc6-159">Un gestore HTTP simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="addc6-159">An HTTP handler looks something like this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="addc6-160">Nel progetto ASP.NET Core, è necessario convertire in un middleware simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="addc6-160">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="addc6-161">Questo middleware è molto simile al middleware corrispondente ai moduli.</span><span class="sxs-lookup"><span data-stu-id="addc6-161">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="addc6-162">L'unica vera differenza è che seguito non vi è alcuna chiamata a `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="addc6-162">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="addc6-163">Che ha senso, perché il gestore di è alla fine della pipeline delle richieste, pertanto sarà presente alcun middleware successivo da richiamare.</span><span class="sxs-lookup"><span data-stu-id="addc6-163">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="addc6-164">La migrazione, l'inserimento del gestore nella pipeline delle richieste</span><span class="sxs-lookup"><span data-stu-id="addc6-164">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="addc6-165">Configurazione di un gestore HTTP viene eseguita in *Web. config* e ha un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="addc6-165">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="addc6-166">È possibile convertire questo aggiungendo il middleware di gestore di nuovo alla pipeline delle richieste nel `Startup` (classe), simile al middleware convertito da moduli.</span><span class="sxs-lookup"><span data-stu-id="addc6-166">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="addc6-167">Il problema con questo approccio è che lo invierà tutte le richieste per il middleware di gestore di nuovo.</span><span class="sxs-lookup"><span data-stu-id="addc6-167">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="addc6-168">Tuttavia, è necessario solo alle richieste con una determinata estensione di raggiungere il middleware.</span><span class="sxs-lookup"><span data-stu-id="addc6-168">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="addc6-169">Che consente la stessa funzionalità che è necessario con il gestore HTTP.</span><span class="sxs-lookup"><span data-stu-id="addc6-169">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="addc6-170">Una soluzione consiste nel creare un ramo della pipeline per le richieste con una determinata estensione, usando il `MapWhen` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="addc6-170">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="addc6-171">A tale scopo, lo stesso `Configure` metodo in cui si aggiunge l'altro middleware:</span><span class="sxs-lookup"><span data-stu-id="addc6-171">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="addc6-172">`MapWhen` utilizza questi parametri:</span><span class="sxs-lookup"><span data-stu-id="addc6-172">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="addc6-173">Un'espressione lambda che accetta il `HttpContext` e restituisce `true` se la richiesta deve passare al ramo.</span><span class="sxs-lookup"><span data-stu-id="addc6-173">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="addc6-174">Ciò significa che è possibile creare una diramazione delle richieste non solo in base all'estensione, ma anche le intestazioni di richiesta parametri della stringa di query, e così via.</span><span class="sxs-lookup"><span data-stu-id="addc6-174">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="addc6-175">Un'espressione lambda che accetta un `IApplicationBuilder` e aggiunge il middleware di tutti per il ramo.</span><span class="sxs-lookup"><span data-stu-id="addc6-175">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="addc6-176">Ciò significa che è possibile aggiungere altro middleware per il ramo davanti il middleware di gestore.</span><span class="sxs-lookup"><span data-stu-id="addc6-176">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="addc6-177">Middleware aggiunto alla pipeline prima che il ramo verrà richiamato in tutte le richieste; il ramo avrà alcun effetto su di essi.</span><span class="sxs-lookup"><span data-stu-id="addc6-177">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="addc6-178">Le opzioni del middleware adottando il modello di opzioni di caricamento</span><span class="sxs-lookup"><span data-stu-id="addc6-178">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="addc6-179">Alcuni moduli e gestori hanno opzioni di configurazione che vengono archiviate nel *Web. config*. Tuttavia, in ASP.NET Core viene usato un nuovo modello di configurazione al posto di *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="addc6-179">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="addc6-180">Il nuovo [sistema di configurazione](xref:fundamentals/configuration/index) fornisce le seguenti opzioni per risolvere il problema:</span><span class="sxs-lookup"><span data-stu-id="addc6-180">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="addc6-181">Inserire direttamente le opzioni nel middleware, come illustrato nella [nella sezione successiva](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="addc6-181">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="addc6-182">Usare la [modello di opzioni](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="addc6-182">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1. <span data-ttu-id="addc6-183">Creare una classe che contenga le opzioni del middleware, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="addc6-183">Create a class to hold your middleware options, for example:</span></span>

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. <span data-ttu-id="addc6-184">I valori delle opzioni di Store</span><span class="sxs-lookup"><span data-stu-id="addc6-184">Store the option values</span></span>

   <span data-ttu-id="addc6-185">Il sistema di configurazione consente di archiviare i valori delle opzioni in qualsiasi punto desiderato.</span><span class="sxs-lookup"><span data-stu-id="addc6-185">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="addc6-186">Tuttavia, i siti più utilizzare *appSettings. JSON*, pertanto è possibile adottare un tale approccio:</span><span class="sxs-lookup"><span data-stu-id="addc6-186">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   <span data-ttu-id="addc6-187">*MyMiddlewareOptionsSection* seguito è riportato un nome di sezione.</span><span class="sxs-lookup"><span data-stu-id="addc6-187">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="addc6-188">Non deve necessariamente corrispondere al nome della classe di opzioni.</span><span class="sxs-lookup"><span data-stu-id="addc6-188">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="addc6-189">Associare i valori delle opzioni con la classe di opzioni</span><span class="sxs-lookup"><span data-stu-id="addc6-189">Associate the option values with the options class</span></span>

    <span data-ttu-id="addc6-190">Il modello di opzioni Usa i framework di inserimento delle dipendenze di ASP.NET Core per associare il tipo di opzioni (ad esempio `MyMiddlewareOptions`) con un `MyMiddlewareOptions` oggetto con le opzioni effettive.</span><span class="sxs-lookup"><span data-stu-id="addc6-190">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="addc6-191">Aggiornamento di `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="addc6-191">Update your `Startup` class:</span></span>

   1. <span data-ttu-id="addc6-192">Se si usa *appSettings. JSON*, aggiungerlo al generatore di configurazione nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="addc6-192">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. <span data-ttu-id="addc6-193">Configurare il servizio di opzioni:</span><span class="sxs-lookup"><span data-stu-id="addc6-193">Configure the options service:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. <span data-ttu-id="addc6-194">Associare le opzioni disponibili con la classe di opzioni:</span><span class="sxs-lookup"><span data-stu-id="addc6-194">Associate your options with your options class:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. <span data-ttu-id="addc6-195">Inserire le opzioni nel costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="addc6-195">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="addc6-196">È simile all'inserimento di opzioni in un controller.</span><span class="sxs-lookup"><span data-stu-id="addc6-196">This is similar to injecting options into a controller.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   <span data-ttu-id="addc6-197">Il [UseMiddleware](#http-modules-usemiddleware) metodo di estensione che aggiunge il middleware per la `IApplicationBuilder` si occupa di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="addc6-197">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

   <span data-ttu-id="addc6-198">Non è limitato a `IOptions` oggetti.</span><span class="sxs-lookup"><span data-stu-id="addc6-198">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="addc6-199">Qualsiasi altro oggetto che richiede il middleware può essere inserito in questo modo.</span><span class="sxs-lookup"><span data-stu-id="addc6-199">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="addc6-200">Il caricamento di opzioni del middleware tramite l'inserimento diretto</span><span class="sxs-lookup"><span data-stu-id="addc6-200">Loading middleware options through direct injection</span></span>

<span data-ttu-id="addc6-201">Il modello di opzioni presenta il vantaggio che crea loose accoppiamento tra i valori delle opzioni e i relativi utenti.</span><span class="sxs-lookup"><span data-stu-id="addc6-201">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="addc6-202">Una volta associato a una classe di opzioni i valori effettivi di opzioni, nessun'altra classe può accedere alle opzioni tramite il framework di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="addc6-202">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="addc6-203">Non è necessario passare intorno ai valori di opzioni.</span><span class="sxs-lookup"><span data-stu-id="addc6-203">There's no need to pass around options values.</span></span>

<span data-ttu-id="addc6-204">Questo modo si ottengono tuttavia se si desidera usare il middleware stesso due volte, con opzioni diverse.</span><span class="sxs-lookup"><span data-stu-id="addc6-204">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="addc6-205">Ad esempio un middleware di autorizzazione usato in diversi rami che consente diversi ruoli.</span><span class="sxs-lookup"><span data-stu-id="addc6-205">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="addc6-206">È possibile associare due oggetti opzioni diverse con la classe di uno opzioni.</span><span class="sxs-lookup"><span data-stu-id="addc6-206">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="addc6-207">La soluzione consiste nell'ottenere gli oggetti di opzioni con i valori di opzioni effettivo nel `Startup` classe e passarle direttamente a ogni istanza del proprio middleware.</span><span class="sxs-lookup"><span data-stu-id="addc6-207">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1. <span data-ttu-id="addc6-208">Aggiungere una seconda chiave da *appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="addc6-208">Add a second key to *appsettings.json*</span></span>

   <span data-ttu-id="addc6-209">Per aggiungere un secondo set di opzioni per la *appSettings. JSON* file, usare una nuova chiave per identificarlo in modo univoco:</span><span class="sxs-lookup"><span data-stu-id="addc6-209">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. <span data-ttu-id="addc6-210">Recuperare i valori delle opzioni e passarle al middleware.</span><span class="sxs-lookup"><span data-stu-id="addc6-210">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="addc6-211">Il `Use...` metodo di estensione (che aggiunge il middleware alla pipeline) è una posizione logica per passare i valori delle opzioni:</span><span class="sxs-lookup"><span data-stu-id="addc6-211">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. <span data-ttu-id="addc6-212">Abilitare il middleware di accettare un parametro di opzioni.</span><span class="sxs-lookup"><span data-stu-id="addc6-212">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="addc6-213">Fornire un overload del `Use...` metodo di estensione (che accetta il parametro options e lo passa al `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="addc6-213">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="addc6-214">Quando si `UseMiddleware` viene chiamato con parametri, passa i parametri al costruttore del middleware quando si crea un'istanza dell'oggetto di middleware.</span><span class="sxs-lookup"><span data-stu-id="addc6-214">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   <span data-ttu-id="addc6-215">Si noti come si esegue il wrapping l'oggetto di opzioni in un `OptionsWrapper` oggetto.</span><span class="sxs-lookup"><span data-stu-id="addc6-215">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="addc6-216">Ciò implementa `IOptions`, come previsto dal costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="addc6-216">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="addc6-217">Eseguire la migrazione a nuovo HttpContext</span><span class="sxs-lookup"><span data-stu-id="addc6-217">Migrating to the new HttpContext</span></span>

<span data-ttu-id="addc6-218">Illustrato in precedenza che il `Invoke` metodo nel proprio middleware accetta un parametro di tipo `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="addc6-218">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="addc6-219">`HttpContext` è stato modificato in modo significativo in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="addc6-219">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="addc6-220">In questa sezione spiega come traslare le proprietà più comunemente utilizzate del [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) al nuovo `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="addc6-220">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="addc6-221">HttpContext</span><span class="sxs-lookup"><span data-stu-id="addc6-221">HttpContext</span></span>

<span data-ttu-id="addc6-222">**HttpContext. Items** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-222">**HttpContext.Items** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="addc6-223">**ID univoco della richiesta (nessuna controparte System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="addc6-223">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="addc6-224">Fornisce un id univoco per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="addc6-224">Gives you a unique id for each request.</span></span> <span data-ttu-id="addc6-225">Molto utili da includere nei log.</span><span class="sxs-lookup"><span data-stu-id="addc6-225">Very useful to include in your logs.</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="addc6-226">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="addc6-226">HttpContext.Request</span></span>

<span data-ttu-id="addc6-227">**HttpContext.Request.HttpMethod** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-227">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="addc6-228">**HttpContext.Request.QueryString** translates to:</span><span class="sxs-lookup"><span data-stu-id="addc6-228">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="addc6-229">**HttpContext.Request.Url** e **HttpContext.Request.RawUrl** Traduci da:</span><span class="sxs-lookup"><span data-stu-id="addc6-229">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="addc6-230">**HttpContext.Request.IsSecureConnection** translates to:</span><span class="sxs-lookup"><span data-stu-id="addc6-230">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="addc6-231">**HttpContext.Request.UserHostAddress** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-231">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="addc6-232">**HttpContext.Request.Cookies** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-232">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="addc6-233">**HttpContext.Request.RequestContext.RouteData** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-233">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="addc6-234">**HttpContext.Request.Headers** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-234">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="addc6-235">**HttpContext.Request.UserAgent** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-235">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="addc6-236">**HttpContext.Request.UrlReferrer** translates to:</span><span class="sxs-lookup"><span data-stu-id="addc6-236">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="addc6-237">**HttpContext.Request.ContentType** translates to:</span><span class="sxs-lookup"><span data-stu-id="addc6-237">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="addc6-238">**HttpContext.Request.Form** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-238">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="addc6-239">Leggere i valori del form solo se il tipo di contenuto sub *x-www-form-urlencoded* oppure *form-data*.</span><span class="sxs-lookup"><span data-stu-id="addc6-239">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="addc6-240">**HttpContext.Request.InputStream** translates to:</span><span class="sxs-lookup"><span data-stu-id="addc6-240">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="addc6-241">Usare questo codice solo in un middleware di tipo gestore, alla fine di una pipeline.</span><span class="sxs-lookup"><span data-stu-id="addc6-241">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="addc6-242">È possibile leggere il corpo non elaborato, come illustrato in precedenza una sola volta per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="addc6-242">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="addc6-243">Tentativo di leggere il corpo dopo la prima lettura middleware leggerà un corpo vuoto.</span><span class="sxs-lookup"><span data-stu-id="addc6-243">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="addc6-244">Ciò non si applica alla lettura di un modulo come illustrato in precedenza, perché l'operazione viene eseguita da un buffer.</span><span class="sxs-lookup"><span data-stu-id="addc6-244">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="addc6-245">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="addc6-245">HttpContext.Response</span></span>

<span data-ttu-id="addc6-246">**HttpContext.Response.Status** e **HttpContext.Response.StatusDescription** Traduci da:</span><span class="sxs-lookup"><span data-stu-id="addc6-246">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="addc6-247">**HttpContext.Response.ContentEncoding** e **HttpContext.Response.ContentType** Traduci da:</span><span class="sxs-lookup"><span data-stu-id="addc6-247">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="addc6-248">**HttpContext.Response.ContentType** sul proprio anche si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-248">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="addc6-249">**HttpContext.Response.Output** si traduce in:</span><span class="sxs-lookup"><span data-stu-id="addc6-249">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="addc6-250">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="addc6-250">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="addc6-251">Servizio backup di un file viene illustrata [qui](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="addc6-251">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="addc6-252">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="addc6-252">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="addc6-253">L'invio delle intestazioni di risposta è ulteriormente complicata dal fatto che se si impostano dopo qualsiasi elemento è stato scritto nel corpo della risposta, non verrà inviati.</span><span class="sxs-lookup"><span data-stu-id="addc6-253">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="addc6-254">La soluzione consiste nell'impostare un metodo di callback che verrà chiamato a destra prima della scrittura nei avviata risposta.</span><span class="sxs-lookup"><span data-stu-id="addc6-254">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="addc6-255">Questa operazione viene eseguita meglio all'inizio del `Invoke` metodo nel proprio middleware.</span><span class="sxs-lookup"><span data-stu-id="addc6-255">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="addc6-256">È questo metodo di callback che consente di impostare le intestazioni della risposta.</span><span class="sxs-lookup"><span data-stu-id="addc6-256">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="addc6-257">Il codice seguente imposta un metodo di callback chiamato `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="addc6-257">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="addc6-258">Il `SetHeaders` metodo di callback sarà analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="addc6-258">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="addc6-259">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="addc6-259">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="addc6-260">I cookie verranno trasmessi al browser in un *Set-Cookie* intestazione della risposta.</span><span class="sxs-lookup"><span data-stu-id="addc6-260">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="addc6-261">Di conseguenza, l'invio di cookie richiede il callback stesso come quelle utilizzate per l'invio delle intestazioni di risposta:</span><span class="sxs-lookup"><span data-stu-id="addc6-261">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="addc6-262">Il `SetCookies` metodo di callback sarebbe simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="addc6-262">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="addc6-263">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="addc6-263">Additional resources</span></span>

* [<span data-ttu-id="addc6-264">Panoramica di moduli HTTP e gestori HTTP</span><span class="sxs-lookup"><span data-stu-id="addc6-264">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="addc6-265">Configurazione</span><span class="sxs-lookup"><span data-stu-id="addc6-265">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="addc6-266">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="addc6-266">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="addc6-267">Middleware</span><span class="sxs-lookup"><span data-stu-id="addc6-267">Middleware</span></span>](xref:fundamentals/middleware/index)
