---
title: Middleware ASP.NET Core
author: rick-anderson
description: Informazioni su ASP.NET Core middleware e la pipeline della richiesta.
ms.author: riande
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: 84f386db4ab96a82011ee2fc0b6c20a1a05b5e4b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="95114-103">Nozioni fondamentali di Middleware di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95114-103">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="95114-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="95114-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="95114-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="95114-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="95114-106">Che cos'è middleware?</span><span class="sxs-lookup"><span data-stu-id="95114-106">What is middleware?</span></span>

<span data-ttu-id="95114-107">Middleware è un software che viene assemblata in una pipeline dell'applicazione per gestire le richieste e risposte.</span><span class="sxs-lookup"><span data-stu-id="95114-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="95114-108">Ogni componente:</span><span class="sxs-lookup"><span data-stu-id="95114-108">Each component:</span></span>

* <span data-ttu-id="95114-109">Sceglie se passare la richiesta al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="95114-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="95114-110">Puoi eseguire lavoro prima e dopo aver richiamato il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="95114-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="95114-111">Richiesta delegati vengono utilizzati per compilare la pipeline della richiesta.</span><span class="sxs-lookup"><span data-stu-id="95114-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="95114-112">I delegati di richiesta di gestire ciascuna richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="95114-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="95114-113">Richiedere i delegati vengono configurati utilizzando [eseguire](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mappa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), e [utilizzare](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="95114-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="95114-114">Un delegato di singola richiesta può essere specificato nella riga come un metodo anonimo (denominato middleware in linea) o può essere definito in una classe riutilizzabile.</span><span class="sxs-lookup"><span data-stu-id="95114-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="95114-115">Queste classi riutilizzabili e i metodi anonimi in linea sono *middleware*, o *componenti middleware*.</span><span class="sxs-lookup"><span data-stu-id="95114-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="95114-116">Ogni componente del middleware nella pipeline delle richieste è responsabile per richiamare il componente successivo nella pipeline o la catena di corto circuito se appropriato.</span><span class="sxs-lookup"><span data-stu-id="95114-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="95114-117">[Moduli HTTP con la migrazione al Middleware](../migration/http-modules.md) viene illustrata la differenza tra le versioni precedente e pipeline di richiesta in ASP.NET Core e fornisce altri esempi di middleware.</span><span class="sxs-lookup"><span data-stu-id="95114-117">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="95114-118">Creazione di una pipeline middleware con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="95114-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="95114-119">La pipeline delle richieste ASP.NET Core è costituito da una sequenza di delegati di richiesta, chiamata uno dopo l'altro, perché questo diagramma mostra (il thread di esecuzione segue frecce nere):</span><span class="sxs-lookup"><span data-stu-id="95114-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Modello di elaborazione di richiesta che mostra una richiesta in arrivo, l'elaborazione tramite tre middlewares e la risposta e l'applicazione.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="95114-123">Delegati possono eseguire operazioni prima e dopo che il delegato successivo.</span><span class="sxs-lookup"><span data-stu-id="95114-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="95114-124">Un delegato può anche decidere di non passare una richiesta per il delegato successivo, che viene chiamato la pipeline di richieste di corto circuito.</span><span class="sxs-lookup"><span data-stu-id="95114-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="95114-125">Corto circuito è spesso consigliabile in quanto evita di lavoro non necessario.</span><span class="sxs-lookup"><span data-stu-id="95114-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="95114-126">Ad esempio, il middleware di file statici può restituire una richiesta di un file statico e il resto della pipeline di corto circuito.</span><span class="sxs-lookup"><span data-stu-id="95114-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="95114-127">Gestione delle eccezioni delegati devono essere chiamato nelle prime fasi della pipeline, in modo che può intercettare le eccezioni generate in fasi successive della pipeline.</span><span class="sxs-lookup"><span data-stu-id="95114-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="95114-128">L'app di ASP.NET Core più semplice possibile imposta un delegato singola richiesta che gestisce tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="95114-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="95114-129">In questo caso non include una pipeline della richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="95114-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="95114-130">Al contrario, una singola funzione anonima viene chiamata in risposta a ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="95114-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="95114-131">Il primo [app. Eseguire](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegato termina la pipeline.</span><span class="sxs-lookup"><span data-stu-id="95114-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="95114-132">È possibile concatenare più delegati richiesta insieme a [app. Utilizzare](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="95114-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="95114-133">Il `next` parametro rappresenta il delegato successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="95114-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="95114-134">(Tenere presente che è possibile corto circuito di pipeline da *non* la chiamata di *Avanti* parametro.) In genere è possibile eseguire azioni prima e dopo che il delegato successivo, come illustrato in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="95114-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="95114-135">Non chiamare `next.Invoke` dopo aver inviata la risposta al client.</span><span class="sxs-lookup"><span data-stu-id="95114-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="95114-136">Passa a `HttpResponse` dopo la risposta è stato avviato, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="95114-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="95114-137">Ad esempio, le modifiche, ad esempio l'impostazione di intestazioni, codice di stato e così via, genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="95114-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="95114-138">Scrivere il corpo della risposta dopo la chiamata `next`:</span><span class="sxs-lookup"><span data-stu-id="95114-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="95114-139">Può causare una violazione del protocollo.</span><span class="sxs-lookup"><span data-stu-id="95114-139">May cause a protocol violation.</span></span> <span data-ttu-id="95114-140">Ad esempio, la scrittura di più le `content-length`.</span><span class="sxs-lookup"><span data-stu-id="95114-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="95114-141">Potrebbe danneggiare il formato del corpo.</span><span class="sxs-lookup"><span data-stu-id="95114-141">May corrupt the body format.</span></span> <span data-ttu-id="95114-142">Ad esempio, la scrittura di un piè di pagina HTML in un file CSS.</span><span class="sxs-lookup"><span data-stu-id="95114-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="95114-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) è un suggerimento utile per indicare se le intestazioni sono state inviate e/o ha scritto il corpo.</span><span class="sxs-lookup"><span data-stu-id="95114-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="95114-144">Ordinazioni</span><span class="sxs-lookup"><span data-stu-id="95114-144">Ordering</span></span>

<span data-ttu-id="95114-145">L'ordine in cui vengono aggiunti i componenti middleware di `Configure` metodo definisce l'ordine in cui si sta richiamate su richieste e l'ordine inverso per la risposta.</span><span class="sxs-lookup"><span data-stu-id="95114-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="95114-146">Questo ordinamento è fondamentale per la sicurezza, prestazioni e funzionalità.</span><span class="sxs-lookup"><span data-stu-id="95114-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="95114-147">Il metodo Configure (mostrato sotto) consente di aggiungere i componenti middleware seguenti:</span><span class="sxs-lookup"><span data-stu-id="95114-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="95114-148">Gestione degli errori di eccezione /</span><span class="sxs-lookup"><span data-stu-id="95114-148">Exception/error handling</span></span>
2. <span data-ttu-id="95114-149">Server di file statici</span><span class="sxs-lookup"><span data-stu-id="95114-149">Static file server</span></span>
3. <span data-ttu-id="95114-150">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="95114-150">Authentication</span></span>
4. <span data-ttu-id="95114-151">MVC</span><span class="sxs-lookup"><span data-stu-id="95114-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95114-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95114-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95114-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95114-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="95114-154">Nel codice precedente, `UseExceptionHandler` è il primo componente middleware aggiunto alla pipeline, pertanto, consente di rilevare le eccezioni che si verificano nelle chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="95114-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="95114-155">Il middleware del file statico viene chiamato nelle prime fasi della pipeline, pertanto può gestire le richieste e di corto circuito senza passare attraverso i componenti rimanenti.</span><span class="sxs-lookup"><span data-stu-id="95114-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="95114-156">Fornisce il middleware di file statici **non** controlli di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="95114-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="95114-157">Qualsiasi file forniti, tra cui quelle contenute *wwwroot*, sono disponibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="95114-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="95114-158">Vedere [utilizzo di file statici](xref:fundamentals/static-files) per un approccio proteggere i file statici.</span><span class="sxs-lookup"><span data-stu-id="95114-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95114-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95114-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="95114-160">Se la richiesta non è gestita dal middleware del file statico, viene passato al middleware di identità (`app.UseAuthentication`), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="95114-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="95114-161">Identità non di corto circuito le richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="95114-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="95114-162">Anche se le richieste vengono autenticate identità, autorizzazione (e rifiuto) si verifica solo dopo aver MVC consente di selezionare una pagina Razor o controller e un'azione specifica.</span><span class="sxs-lookup"><span data-stu-id="95114-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95114-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95114-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="95114-164">Se la richiesta non è gestita dal middleware del file statico, viene passato al middleware di identità (`app.UseIdentity`), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="95114-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="95114-165">Identità non di corto circuito le richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="95114-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="95114-166">Anche se le richieste vengono autenticate identità, autorizzazione (e rifiuto) si verifica solo dopo aver MVC consente di selezionare un controller specifico e l'azione.</span><span class="sxs-lookup"><span data-stu-id="95114-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="95114-167">Nell'esempio seguente viene illustrato un tipo di middleware ordine in cui le richieste di file statici vengono gestite dal middleware del file statico prima il middleware di compressione di risposta.</span><span class="sxs-lookup"><span data-stu-id="95114-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="95114-168">File statici non vengono compressi con questo tipo di ordinamento del middleware.</span><span class="sxs-lookup"><span data-stu-id="95114-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="95114-169">Le risposte MVC [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) possono essere compressi.</span><span class="sxs-lookup"><span data-stu-id="95114-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="95114-170">Utilizzare, eseguire ed eseguire il mapping</span><span class="sxs-lookup"><span data-stu-id="95114-170">Use, Run, and Map</span></span>

<span data-ttu-id="95114-171">Configurare la pipeline HTTP usando `Use`, `Run`, e `Map`.</span><span class="sxs-lookup"><span data-stu-id="95114-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="95114-172">Il `Use` metodo possibile di corto circuito la pipeline (ovvero, se non si chiamerà un `next` delegato richiesta).</span><span class="sxs-lookup"><span data-stu-id="95114-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="95114-173">`Run`è una convenzione e possono esporre alcuni componenti middleware `Run[Middleware]` metodi che eseguono alla fine della pipeline.</span><span class="sxs-lookup"><span data-stu-id="95114-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="95114-174">`Map*`le estensioni vengono utilizzate come una convenzione per la diramazione la pipeline.</span><span class="sxs-lookup"><span data-stu-id="95114-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="95114-175">[Mappa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branch, la pipeline di richieste in base alle corrispondenze del percorso di richiesta specificata.</span><span class="sxs-lookup"><span data-stu-id="95114-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="95114-176">Se il percorso della richiesta inizia con il percorso specificato, viene eseguito il ramo.</span><span class="sxs-lookup"><span data-stu-id="95114-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="95114-177">La tabella seguente mostra le richieste e risposte da `http://localhost:1234` utilizzando il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="95114-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="95114-178">Richiesta</span><span class="sxs-lookup"><span data-stu-id="95114-178">Request</span></span> | <span data-ttu-id="95114-179">Risposta</span><span class="sxs-lookup"><span data-stu-id="95114-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="95114-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="95114-180">localhost:1234</span></span> | <span data-ttu-id="95114-181">Hello dal delegato non mappa.</span><span class="sxs-lookup"><span data-stu-id="95114-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="95114-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="95114-182">localhost:1234/map1</span></span> | <span data-ttu-id="95114-183">Eseguire il mapping di Test 1</span><span class="sxs-lookup"><span data-stu-id="95114-183">Map Test 1</span></span> |
| <span data-ttu-id="95114-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="95114-184">localhost:1234/map2</span></span> | <span data-ttu-id="95114-185">Test mappa 2</span><span class="sxs-lookup"><span data-stu-id="95114-185">Map Test 2</span></span> |
| <span data-ttu-id="95114-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="95114-186">localhost:1234/map3</span></span> | <span data-ttu-id="95114-187">Hello dal delegato non mappa.</span><span class="sxs-lookup"><span data-stu-id="95114-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="95114-188">Quando `Map` è utilizzato, ai segmenti di percorso corrispondenti vengono rimossi dal `HttpRequest.Path` e accodato a `HttpRequest.PathBase` per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="95114-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="95114-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branch, la pipeline di richieste in base al risultato del predicato specificato.</span><span class="sxs-lookup"><span data-stu-id="95114-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="95114-190">Un predicato di tipo `Func<HttpContext, bool>` può essere usato per eseguire il mapping delle richieste in un nuovo ramo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="95114-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="95114-191">Nell'esempio seguente, un predicato viene usato per rilevare la presenza di una variabile di stringa di query `branch`:</span><span class="sxs-lookup"><span data-stu-id="95114-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="95114-192">La tabella seguente mostra le richieste e risposte da `http://localhost:1234` utilizzando il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="95114-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="95114-193">Richiesta</span><span class="sxs-lookup"><span data-stu-id="95114-193">Request</span></span> | <span data-ttu-id="95114-194">Risposta</span><span class="sxs-lookup"><span data-stu-id="95114-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="95114-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="95114-195">localhost:1234</span></span> | <span data-ttu-id="95114-196">Hello dal delegato non mappa.</span><span class="sxs-lookup"><span data-stu-id="95114-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="95114-197">localhost:1234 /? ramo = master</span><span class="sxs-lookup"><span data-stu-id="95114-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="95114-198">Ramo utilizzato = master</span><span class="sxs-lookup"><span data-stu-id="95114-198">Branch used = master</span></span>|

<span data-ttu-id="95114-199">`Map`supporta la nidificazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95114-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="95114-200">`Map`può anche corrispondere più segmenti in una sola volta, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95114-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="95114-201">Middleware incorporato</span><span class="sxs-lookup"><span data-stu-id="95114-201">Built-in middleware</span></span>

<span data-ttu-id="95114-202">ASP.NET Core viene fornito con i componenti middleware seguenti, nonché una descrizione dell'ordine in cui deve essere aggiunto:</span><span class="sxs-lookup"><span data-stu-id="95114-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="95114-203">Middleware</span><span class="sxs-lookup"><span data-stu-id="95114-203">Middleware</span></span> | <span data-ttu-id="95114-204">Descrizione</span><span class="sxs-lookup"><span data-stu-id="95114-204">Description</span></span> | <span data-ttu-id="95114-205">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="95114-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="95114-206">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="95114-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="95114-207">Fornisce supporto per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="95114-207">Provides authentication support.</span></span> | <span data-ttu-id="95114-208">Prima di `HttpContext.User` è necessaria.</span><span class="sxs-lookup"><span data-stu-id="95114-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="95114-209">Terminal per i callback di OAuth.</span><span class="sxs-lookup"><span data-stu-id="95114-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="95114-210">CORS</span><span class="sxs-lookup"><span data-stu-id="95114-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="95114-211">Configura la condivisione delle risorse Multiorigine.</span><span class="sxs-lookup"><span data-stu-id="95114-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="95114-212">Prima di componenti che utilizzano CORS.</span><span class="sxs-lookup"><span data-stu-id="95114-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="95114-213">Diagnostica</span><span class="sxs-lookup"><span data-stu-id="95114-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="95114-214">Configura la diagnostica.</span><span class="sxs-lookup"><span data-stu-id="95114-214">Configures diagnostics.</span></span> | <span data-ttu-id="95114-215">Prima di componenti che generano errori.</span><span class="sxs-lookup"><span data-stu-id="95114-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="95114-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="95114-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="95114-217">Inoltra elaborate intestazioni nella richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="95114-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="95114-218">Prima di componenti che utilizzano i campi aggiornati (esempi: schema, Host, ClientIP, metodo).</span><span class="sxs-lookup"><span data-stu-id="95114-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="95114-219">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="95114-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="95114-220">Fornisce supporto per la memorizzazione nella cache le risposte.</span><span class="sxs-lookup"><span data-stu-id="95114-220">Provides support for caching responses.</span></span> | <span data-ttu-id="95114-221">Prima di componenti che richiedono la memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="95114-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="95114-222">Compressione di risposta</span><span class="sxs-lookup"><span data-stu-id="95114-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="95114-223">Fornisce il supporto per la compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="95114-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="95114-224">Prima di componenti che richiedono la compressione.</span><span class="sxs-lookup"><span data-stu-id="95114-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="95114-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="95114-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="95114-226">Fornisce il supporto di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="95114-226">Provides localization support.</span></span> | <span data-ttu-id="95114-227">Prima di componenti sensibili localizzazione.</span><span class="sxs-lookup"><span data-stu-id="95114-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="95114-228">Routing</span><span class="sxs-lookup"><span data-stu-id="95114-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="95114-229">Definisce e vincola una route richiesta.</span><span class="sxs-lookup"><span data-stu-id="95114-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="95114-230">Terminale per route corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="95114-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="95114-231">Sessione</span><span class="sxs-lookup"><span data-stu-id="95114-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="95114-232">Fornisce il supporto per la gestione delle sessioni utente.</span><span class="sxs-lookup"><span data-stu-id="95114-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="95114-233">Prima di componenti che richiedono una sessione.</span><span class="sxs-lookup"><span data-stu-id="95114-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="95114-234">File statici</span><span class="sxs-lookup"><span data-stu-id="95114-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="95114-235">Fornisce il supporto per la gestione di file statici e di esplorazione directory.</span><span class="sxs-lookup"><span data-stu-id="95114-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="95114-236">Terminal se corrisponde a una richiesta di file.</span><span class="sxs-lookup"><span data-stu-id="95114-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="95114-237">La riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="95114-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="95114-238">Fornisce supporto per la riscrittura degli URL e reindirizzamento delle richieste.</span><span class="sxs-lookup"><span data-stu-id="95114-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="95114-239">Prima di componenti che utilizzano l'URL.</span><span class="sxs-lookup"><span data-stu-id="95114-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="95114-240">Oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="95114-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="95114-241">Abilita il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="95114-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="95114-242">Prima di componenti necessari per accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="95114-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="95114-243">Middleware di scrittura</span><span class="sxs-lookup"><span data-stu-id="95114-243">Writing middleware</span></span>

<span data-ttu-id="95114-244">Middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="95114-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="95114-245">Si consideri il middleware seguente, che imposta le impostazioni cultura per la richiesta corrente dalla stringa di query:</span><span class="sxs-lookup"><span data-stu-id="95114-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="95114-246">Nota: Il codice di esempio precedente viene utilizzato per illustrare la creazione di un componente del middleware.</span><span class="sxs-lookup"><span data-stu-id="95114-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="95114-247">Vedere [ globalizzazione e localizzazione](xref:fundamentals/localization) per il supporto di localizzazione predefinite di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="95114-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="95114-248">È possibile testare il middleware passando le impostazioni cultura, ad esempio `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="95114-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="95114-249">Il codice seguente sposta il middleware di delegato a una classe:</span><span class="sxs-lookup"><span data-stu-id="95114-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="95114-250">Il seguente metodo di estensione espone il middleware tramite [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="95114-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="95114-251">Il codice seguente viene chiamato il middleware da `Configure`:</span><span class="sxs-lookup"><span data-stu-id="95114-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="95114-252">Middleware deve seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/) esponendo le relative dipendenze nel relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="95114-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="95114-253">Middleware viene costruito una volta per ogni *durata applicazione*.</span><span class="sxs-lookup"><span data-stu-id="95114-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="95114-254">Vedere *dipendenze richiesta* sotto se è necessario condividere servizi con middleware all'interno di una richiesta.</span><span class="sxs-lookup"><span data-stu-id="95114-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="95114-255">I componenti middleware per risolvere le dipendenze dall'inserimento di dipendenze mediante i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="95114-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="95114-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)può anche accettare parametri aggiuntivi direttamente.</span><span class="sxs-lookup"><span data-stu-id="95114-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="95114-257">Le dipendenze per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="95114-257">Per-request dependencies</span></span>

<span data-ttu-id="95114-258">Poiché middleware viene creato all'avvio dell'app, non per singola richiesta, *con ambito* servizi di durata utilizzati dal middleware costruttori non vengono condivise con altri tipi di dipendenza inserito durante ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="95114-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="95114-259">Se è necessario condividere una *con ambito* tra del middleware e altri tipi di servizio, aggiungere questi servizi per il `Invoke` firma del metodo.</span><span class="sxs-lookup"><span data-stu-id="95114-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="95114-260">Il `Invoke` metodo può accettare parametri aggiuntivi che vengono popolati dall'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="95114-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="95114-261">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95114-261">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a><span data-ttu-id="95114-262">Risorse</span><span class="sxs-lookup"><span data-stu-id="95114-262">Resources</span></span>

* [<span data-ttu-id="95114-263">Codice di esempio utilizzati in questo documento</span><span class="sxs-lookup"><span data-stu-id="95114-263">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="95114-264">Migrazione di moduli HTTP in middleware</span><span class="sxs-lookup"><span data-stu-id="95114-264">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="95114-265">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="95114-265">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="95114-266">Funzionalità di richiesta</span><span class="sxs-lookup"><span data-stu-id="95114-266">Request Features</span></span>](request-features.md)
