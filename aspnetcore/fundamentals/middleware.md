---
title: Middleware ASP.NET Core
author: rick-anderson
description: Informazioni su ASP.NET Core middleware e la pipeline della richiesta.
keywords: ASP.NET Core, Middleware, pipeline, delegato
ms.author: riande
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: 730b4c281a766059b16ca1c36bbeb9611b979b72
ms.sourcegitcommit: 0f23400cae837e90927043aa0dfd6c31108a4e2c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="2c458-104">Nozioni fondamentali di Middleware di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c458-104">ASP.NET Core Middleware Fundamentals</span></span>

<a name=fundamentals-middleware></a>

<span data-ttu-id="2c458-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2c458-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2c458-106">[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2c458-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="2c458-107">Che cos'è middleware</span><span class="sxs-lookup"><span data-stu-id="2c458-107">What is middleware</span></span>

<span data-ttu-id="2c458-108">Middleware è un software che viene assemblata in una pipeline dell'applicazione per gestire le richieste e risposte.</span><span class="sxs-lookup"><span data-stu-id="2c458-108">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="2c458-109">Ogni componente:</span><span class="sxs-lookup"><span data-stu-id="2c458-109">Each component:</span></span>

* <span data-ttu-id="2c458-110">Sceglie se passare la richiesta al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c458-110">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="2c458-111">Puoi eseguire lavoro prima e dopo aver richiamato il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c458-111">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="2c458-112">Richiesta delegati vengono utilizzati per compilare la pipeline della richiesta.</span><span class="sxs-lookup"><span data-stu-id="2c458-112">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="2c458-113">I delegati di richiesta di gestire ciascuna richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2c458-113">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="2c458-114">Richiedere i delegati vengono configurati utilizzando [eseguire](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mappa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), e [utilizzare](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="2c458-114">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="2c458-115">Un delegato di singola richiesta può essere specificato nella riga come un metodo anonimo (denominato middleware in linea) o può essere definito in una classe riutilizzabile.</span><span class="sxs-lookup"><span data-stu-id="2c458-115">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="2c458-116">Queste classi riutilizzabili e i metodi anonimi in linea sono *middleware*, o *componenti middleware*.</span><span class="sxs-lookup"><span data-stu-id="2c458-116">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="2c458-117">Ogni componente del middleware nella pipeline delle richieste è responsabile per richiamare il componente successivo nella pipeline o la catena di corto circuito se appropriato.</span><span class="sxs-lookup"><span data-stu-id="2c458-117">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="2c458-118">[Moduli HTTP con la migrazione al Middleware](../migration/http-modules.md) viene illustrata la differenza tra le versioni precedente e pipeline di richiesta in ASP.NET Core e fornisce altri esempi di middleware.</span><span class="sxs-lookup"><span data-stu-id="2c458-118">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="2c458-119">Creazione di una pipeline middleware con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="2c458-119">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="2c458-120">La pipeline delle richieste ASP.NET Core è costituito da una sequenza di delegati di richiesta, chiamata uno dopo l'altro, perché questo diagramma mostra (il thread di esecuzione segue frecce nere):</span><span class="sxs-lookup"><span data-stu-id="2c458-120">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Modello di elaborazione di richiesta che mostra una richiesta in arrivo, l'elaborazione tramite tre middlewares e la risposta e l'applicazione.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="2c458-124">Delegati possono eseguire operazioni prima e dopo che il delegato successivo.</span><span class="sxs-lookup"><span data-stu-id="2c458-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="2c458-125">Un delegato può anche decidere di non passare una richiesta per il delegato successivo, che viene chiamato la pipeline di richieste di corto circuito.</span><span class="sxs-lookup"><span data-stu-id="2c458-125">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="2c458-126">Corto circuito è spesso consigliabile in quanto evita di lavoro non necessario.</span><span class="sxs-lookup"><span data-stu-id="2c458-126">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="2c458-127">Ad esempio, il middleware di file statici può restituire una richiesta di un file statico e il resto della pipeline di corto circuito.</span><span class="sxs-lookup"><span data-stu-id="2c458-127">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="2c458-128">Gestione delle eccezioni delegati devono essere chiamato nelle prime fasi della pipeline, in modo che può intercettare le eccezioni generate in fasi successive della pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c458-128">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="2c458-129">L'app di ASP.NET Core più semplice possibile imposta un delegato singola richiesta che gestisce tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="2c458-129">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="2c458-130">In questo caso non include una pipeline della richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="2c458-130">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="2c458-131">Al contrario, una singola funzione anonima viene chiamata in risposta a ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2c458-131">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="2c458-132">Il primo [app. Eseguire](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegato termina la pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c458-132">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="2c458-133">È possibile concatenare più delegati richiesta insieme a [app. Utilizzare](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="2c458-133">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="2c458-134">Il `next` parametro rappresenta il delegato successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c458-134">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="2c458-135">(Tenere presente che è possibile corto circuito di pipeline da *non* la chiamata di *Avanti* parametro.) In genere è possibile eseguire azioni prima e dopo che il delegato successivo, come illustrato in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="2c458-135">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="2c458-136">Non chiamare `next.Invoke` dopo aver inviata la risposta al client.</span><span class="sxs-lookup"><span data-stu-id="2c458-136">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="2c458-137">Passa a `HttpResponse` dopo la risposta è stato avviato, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2c458-137">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="2c458-138">Ad esempio, le modifiche, ad esempio l'impostazione di intestazioni, codice di stato e così via, genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2c458-138">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="2c458-139">Scrivere il corpo della risposta dopo la chiamata `next`:</span><span class="sxs-lookup"><span data-stu-id="2c458-139">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="2c458-140">Può causare una violazione del protocollo.</span><span class="sxs-lookup"><span data-stu-id="2c458-140">May cause a protocol violation.</span></span> <span data-ttu-id="2c458-141">Ad esempio, la scrittura di più le `content-length`.</span><span class="sxs-lookup"><span data-stu-id="2c458-141">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="2c458-142">Potrebbe danneggiare il formato del corpo.</span><span class="sxs-lookup"><span data-stu-id="2c458-142">May corrupt the body format.</span></span> <span data-ttu-id="2c458-143">Ad esempio, la scrittura di un piè di pagina HTML in un file CSS.</span><span class="sxs-lookup"><span data-stu-id="2c458-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="2c458-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) è un suggerimento utile per indicare se le intestazioni sono state inviate e/o ha scritto il corpo.</span><span class="sxs-lookup"><span data-stu-id="2c458-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="2c458-145">Ordinazioni</span><span class="sxs-lookup"><span data-stu-id="2c458-145">Ordering</span></span>

<span data-ttu-id="2c458-146">L'ordine in cui vengono aggiunti i componenti middleware di `Configure` metodo definisce l'ordine in cui vengono richiamati per le richieste e l'ordine inverso per la risposta.</span><span class="sxs-lookup"><span data-stu-id="2c458-146">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="2c458-147">Questo ordinamento è fondamentale per la sicurezza, prestazioni e funzionalità.</span><span class="sxs-lookup"><span data-stu-id="2c458-147">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="2c458-148">Il metodo Configure (mostrato sotto) consente di aggiungere i componenti middleware seguenti:</span><span class="sxs-lookup"><span data-stu-id="2c458-148">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="2c458-149">Gestione degli errori di eccezione /</span><span class="sxs-lookup"><span data-stu-id="2c458-149">Exception/error handling</span></span>
2. <span data-ttu-id="2c458-150">Server di file statici</span><span class="sxs-lookup"><span data-stu-id="2c458-150">Static file server</span></span>
3. <span data-ttu-id="2c458-151">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="2c458-151">Authentication</span></span>
4. <span data-ttu-id="2c458-152">MVC</span><span class="sxs-lookup"><span data-stu-id="2c458-152">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2c458-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2c458-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2c458-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2c458-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="2c458-155">Nel codice precedente, `UseExceptionHandler` è il primo componente middleware aggiunto alla pipeline, pertanto, consente di rilevare le eccezioni che si verificano nelle chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="2c458-155">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="2c458-156">Il middleware del file statico viene chiamato nelle prime fasi della pipeline, pertanto può gestire le richieste e di corto circuito senza passare attraverso i componenti rimanenti.</span><span class="sxs-lookup"><span data-stu-id="2c458-156">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="2c458-157">Fornisce il middleware di file statici **non** controlli di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="2c458-157">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="2c458-158">Qualsiasi file forniti, tra cui quelle contenute *wwwroot*, sono disponibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="2c458-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="2c458-159">Vedere [utilizzo di file statici](xref:fundamentals/static-files) per un approccio proteggere i file statici.</span><span class="sxs-lookup"><span data-stu-id="2c458-159">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2c458-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2c458-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="2c458-161">Se la richiesta non è gestita dal middleware del file statico, viene passato al middleware di identità (`app.UseAuthentication`), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2c458-161">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="2c458-162">Identità non di corto circuito le richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="2c458-162">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="2c458-163">Anche se le richieste vengono autenticate identità, autorizzazione (e rifiuto) si verifica solo dopo aver MVC consente di selezionare una pagina Razor o controller e un'azione specifica.</span><span class="sxs-lookup"><span data-stu-id="2c458-163">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2c458-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2c458-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2c458-165">Se la richiesta non è gestita dal middleware del file statico, viene passato al middleware di identità (`app.UseIdentity`), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2c458-165">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="2c458-166">Identità non di corto circuito le richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="2c458-166">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="2c458-167">Anche se le richieste vengono autenticate identità, autorizzazione (e rifiuto) si verifica solo dopo aver MVC consente di selezionare un controller specifico e l'azione.</span><span class="sxs-lookup"><span data-stu-id="2c458-167">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="2c458-168">Nell'esempio seguente viene illustrato un tipo di middleware ordine in cui le richieste di file statici vengono gestite dal middleware del file statico prima il middleware di compressione di risposta.</span><span class="sxs-lookup"><span data-stu-id="2c458-168">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="2c458-169">File statici non vengono compressi con questo tipo di ordinamento del middleware.</span><span class="sxs-lookup"><span data-stu-id="2c458-169">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="2c458-170">Le risposte MVC [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) possono essere compressi.</span><span class="sxs-lookup"><span data-stu-id="2c458-170">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name=middleware-run-map-use></a>

### <a name="use-run-and-map"></a><span data-ttu-id="2c458-171">Utilizzare, eseguire ed eseguire il mapping</span><span class="sxs-lookup"><span data-stu-id="2c458-171">Use, Run, and Map</span></span>

<span data-ttu-id="2c458-172">Configurare la pipeline HTTP usando `Use`, `Run`, e `Map`.</span><span class="sxs-lookup"><span data-stu-id="2c458-172">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="2c458-173">Il `Use` metodo possibile di corto circuito la pipeline (ovvero, se non chiama un `next` delegato richiesta).</span><span class="sxs-lookup"><span data-stu-id="2c458-173">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="2c458-174">`Run`è una convenzione e possono esporre alcuni componenti middleware `Run[Middleware]` metodi che eseguono alla fine della pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c458-174">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="2c458-175">`Map*`le estensioni vengono utilizzate come una convenzione per la diramazione la pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c458-175">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="2c458-176">[Mappa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branch, la pipeline di richieste in base alle corrispondenze del percorso di richiesta specificata.</span><span class="sxs-lookup"><span data-stu-id="2c458-176">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="2c458-177">Se il percorso della richiesta inizia con il percorso specificato, viene eseguito il ramo.</span><span class="sxs-lookup"><span data-stu-id="2c458-177">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="2c458-178">La tabella seguente mostra le richieste e risposte da `http://localhost:1234` utilizzando il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="2c458-178">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="2c458-179">Richiesta</span><span class="sxs-lookup"><span data-stu-id="2c458-179">Request</span></span> | <span data-ttu-id="2c458-180">Risposta</span><span class="sxs-lookup"><span data-stu-id="2c458-180">Response</span></span> |
| --- | --- |
| <span data-ttu-id="2c458-181">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="2c458-181">localhost:1234</span></span> | <span data-ttu-id="2c458-182">Hello dal delegato non mappa.</span><span class="sxs-lookup"><span data-stu-id="2c458-182">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="2c458-183">localhost:1234 / map1</span><span class="sxs-lookup"><span data-stu-id="2c458-183">localhost:1234/map1</span></span> | <span data-ttu-id="2c458-184">Eseguire il mapping di Test 1</span><span class="sxs-lookup"><span data-stu-id="2c458-184">Map Test 1</span></span> |
| <span data-ttu-id="2c458-185">localhost:1234 / map2</span><span class="sxs-lookup"><span data-stu-id="2c458-185">localhost:1234/map2</span></span> | <span data-ttu-id="2c458-186">Test mappa 2</span><span class="sxs-lookup"><span data-stu-id="2c458-186">Map Test 2</span></span> |
| <span data-ttu-id="2c458-187">localhost:1234 / map3</span><span class="sxs-lookup"><span data-stu-id="2c458-187">localhost:1234/map3</span></span> | <span data-ttu-id="2c458-188">Hello dal delegato non mappa.</span><span class="sxs-lookup"><span data-stu-id="2c458-188">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="2c458-189">Quando `Map` è utilizzato, ai segmenti di percorso corrispondenti vengono rimossi dal `HttpRequest.Path` e accodato a `HttpRequest.PathBase` per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="2c458-189">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="2c458-190">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branch, la pipeline di richieste in base al risultato del predicato specificato.</span><span class="sxs-lookup"><span data-stu-id="2c458-190">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="2c458-191">Un predicato di tipo `Func<HttpContext, bool>` può essere usato per eseguire il mapping delle richieste in un nuovo ramo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c458-191">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="2c458-192">Nell'esempio seguente, un predicato viene usato per rilevare la presenza di una variabile di stringa di query `branch`:</span><span class="sxs-lookup"><span data-stu-id="2c458-192">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="2c458-193">La tabella seguente mostra le richieste e risposte da `http://localhost:1234` utilizzando il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="2c458-193">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="2c458-194">Richiesta</span><span class="sxs-lookup"><span data-stu-id="2c458-194">Request</span></span> | <span data-ttu-id="2c458-195">Risposta</span><span class="sxs-lookup"><span data-stu-id="2c458-195">Response</span></span> |
| --- | --- |
| <span data-ttu-id="2c458-196">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="2c458-196">localhost:1234</span></span> | <span data-ttu-id="2c458-197">Hello dal delegato non mappa.</span><span class="sxs-lookup"><span data-stu-id="2c458-197">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="2c458-198">localhost:1234 /? ramo = master</span><span class="sxs-lookup"><span data-stu-id="2c458-198">localhost:1234/?branch=master</span></span> | <span data-ttu-id="2c458-199">Ramo utilizzato = master</span><span class="sxs-lookup"><span data-stu-id="2c458-199">Branch used = master</span></span>|

<span data-ttu-id="2c458-200">`Map`supporta la nidificazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2c458-200">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="2c458-201">`Map`può anche corrispondere più segmenti in una sola volta, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2c458-201">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="2c458-202">Middleware incorporato</span><span class="sxs-lookup"><span data-stu-id="2c458-202">Built-in middleware</span></span>

<span data-ttu-id="2c458-203">ASP.NET Core viene fornito con i componenti middleware seguenti:</span><span class="sxs-lookup"><span data-stu-id="2c458-203">ASP.NET Core ships with the following middleware components:</span></span>

| <span data-ttu-id="2c458-204">Middleware</span><span class="sxs-lookup"><span data-stu-id="2c458-204">Middleware</span></span> | <span data-ttu-id="2c458-205">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2c458-205">Description</span></span> |
| ----- | ------- |
| [<span data-ttu-id="2c458-206">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="2c458-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="2c458-207">Fornisce supporto per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2c458-207">Provides authentication support.</span></span> |
| [<span data-ttu-id="2c458-208">CORS</span><span class="sxs-lookup"><span data-stu-id="2c458-208">CORS</span></span>](xref:security/cors) | <span data-ttu-id="2c458-209">Configura la condivisione delle risorse Multiorigine.</span><span class="sxs-lookup"><span data-stu-id="2c458-209">Configures Cross-Origin Resource Sharing.</span></span> |
| [<span data-ttu-id="2c458-210">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="2c458-210">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="2c458-211">Fornisce supporto per la memorizzazione nella cache le risposte.</span><span class="sxs-lookup"><span data-stu-id="2c458-211">Provides support for caching responses.</span></span> |
| [<span data-ttu-id="2c458-212">Compressione di risposta</span><span class="sxs-lookup"><span data-stu-id="2c458-212">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="2c458-213">Fornisce il supporto per la compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="2c458-213">Provides support for compressing responses.</span></span> |
| [<span data-ttu-id="2c458-214">Routing</span><span class="sxs-lookup"><span data-stu-id="2c458-214">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="2c458-215">Definisce e vincola una route richiesta.</span><span class="sxs-lookup"><span data-stu-id="2c458-215">Defines and constrains request routes.</span></span> |
| [<span data-ttu-id="2c458-216">Sessione</span><span class="sxs-lookup"><span data-stu-id="2c458-216">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="2c458-217">Fornisce il supporto per la gestione delle sessioni utente.</span><span class="sxs-lookup"><span data-stu-id="2c458-217">Provides support for managing user sessions.</span></span> |
| [<span data-ttu-id="2c458-218">File statici</span><span class="sxs-lookup"><span data-stu-id="2c458-218">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="2c458-219">Fornisce il supporto per la gestione di file statici e di esplorazione directory.</span><span class="sxs-lookup"><span data-stu-id="2c458-219">Provides support for serving static files and directory browsing.</span></span> |
| [<span data-ttu-id="2c458-220">Middleware di riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="2c458-220">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="2c458-221">Fornisce supporto per la riscrittura degli URL e reindirizzamento delle richieste.</span><span class="sxs-lookup"><span data-stu-id="2c458-221">Provides support for rewriting URLs and redirecting requests.</span></span> |

<a name=middleware-writing-middleware></a>

## <a name="writing-middleware"></a><span data-ttu-id="2c458-222">Middleware di scrittura</span><span class="sxs-lookup"><span data-stu-id="2c458-222">Writing middleware</span></span>

<span data-ttu-id="2c458-223">Middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="2c458-223">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="2c458-224">Si consideri il middleware seguente, che imposta le impostazioni cultura per la richiesta corrente dalla stringa di query:</span><span class="sxs-lookup"><span data-stu-id="2c458-224">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="2c458-225">Nota: Il codice di esempio precedente viene utilizzato per illustrare la creazione di un componente del middleware.</span><span class="sxs-lookup"><span data-stu-id="2c458-225">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="2c458-226">Vedere [ globalizzazione e localizzazione](xref:fundamentals/localization) per il supporto di localizzazione predefinite di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c458-226">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="2c458-227">È possibile testare il middleware passando le impostazioni cultura, ad esempio `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="2c458-227">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="2c458-228">Il codice seguente sposta il middleware di delegato a una classe:</span><span class="sxs-lookup"><span data-stu-id="2c458-228">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="2c458-229">Il seguente metodo di estensione espone il middleware tramite [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="2c458-229">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="2c458-230">Il codice seguente viene chiamato il middleware da `Configure`:</span><span class="sxs-lookup"><span data-stu-id="2c458-230">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="2c458-231">Middleware deve seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/) esponendo le relative dipendenze nel relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="2c458-231">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="2c458-232">Middleware viene costruito una volta per ogni *durata applicazione*.</span><span class="sxs-lookup"><span data-stu-id="2c458-232">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="2c458-233">Vedere *dipendenze richiesta* sotto se è necessario condividere servizi con middleware all'interno di una richiesta.</span><span class="sxs-lookup"><span data-stu-id="2c458-233">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="2c458-234">I componenti middleware per risolvere le dipendenze dall'inserimento di dipendenze mediante i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="2c458-234">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="2c458-235">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)può anche accettare parametri aggiuntivi direttamente.</span><span class="sxs-lookup"><span data-stu-id="2c458-235">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="2c458-236">Le dipendenze per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="2c458-236">Per-request dependencies</span></span>

<span data-ttu-id="2c458-237">Poiché middleware viene creato all'avvio dell'app, non per singola richiesta, *con ambito* servizi di durata utilizzati dal middleware costruttori non vengono condivise con altri tipi di dipendenza inserito durante ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="2c458-237">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="2c458-238">Se è necessario condividere una *con ambito* tra del middleware e altri tipi di servizio, aggiungere questi servizi per il `Invoke` firma del metodo.</span><span class="sxs-lookup"><span data-stu-id="2c458-238">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="2c458-239">Il `Invoke` metodo può accettare parametri aggiuntivi che vengono popolati dall'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="2c458-239">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="2c458-240">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2c458-240">For example:</span></span>

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

## <a name="resources"></a><span data-ttu-id="2c458-241">Risorse</span><span class="sxs-lookup"><span data-stu-id="2c458-241">Resources</span></span>

* [<span data-ttu-id="2c458-242">Codice di esempio utilizzati in questo documento</span><span class="sxs-lookup"><span data-stu-id="2c458-242">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="2c458-243">Migrazione di moduli HTTP in middleware</span><span class="sxs-lookup"><span data-stu-id="2c458-243">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="2c458-244">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="2c458-244">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="2c458-245">Funzionalità di richiesta</span><span class="sxs-lookup"><span data-stu-id="2c458-245">Request Features</span></span>](request-features.md)
