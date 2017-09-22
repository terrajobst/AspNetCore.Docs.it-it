---
title: Middleware ASP.NET Core
author: rick-anderson
description: Viene illustrato il middleware e la pipeline della richiesta.
keywords: ASP.NET Core, Middleware, pipeline, delegato
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: cb39d74b9293b3ab341beba08d2f0af90261ca5f
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="c3e9a-104">Nozioni fondamentali di Middleware di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3e9a-104">ASP.NET Core Middleware Fundamentals</span></span>

<a name=fundamentals-middleware></a>

<span data-ttu-id="c3e9a-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c3e9a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

[<span data-ttu-id="c3e9a-106">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="c3e9a-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)

## <a name="what-is-middleware"></a><span data-ttu-id="c3e9a-107">Che cos'è middleware</span><span class="sxs-lookup"><span data-stu-id="c3e9a-107">What is middleware</span></span>

<span data-ttu-id="c3e9a-108">Middleware è un software che viene assemblata in una pipeline dell'applicazione per gestire le richieste e risposte.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-108">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="c3e9a-109">Ogni componente:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-109">Each component:</span></span>

* <span data-ttu-id="c3e9a-110">Sceglie se passare la richiesta al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-110">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="c3e9a-111">Puoi eseguire lavoro prima e dopo aver richiamato il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-111">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="c3e9a-112">Richiesta delegati vengono utilizzati per compilare la pipeline della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-112">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="c3e9a-113">I delegati di richiesta di gestire ciascuna richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-113">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="c3e9a-114">Richiedere i delegati vengono configurati utilizzando [eseguire](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mappa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), e [utilizzare](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-114">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="c3e9a-115">Un delegato di singola richiesta può essere specificato nella riga come un metodo anonimo (denominato middleware in linea) o può essere definito in una classe riutilizzabile.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-115">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="c3e9a-116">Queste classi riutilizzabili e i metodi anonimi in linea sono *middleware*, o *componenti middleware*.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-116">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="c3e9a-117">Ogni componente del middleware nella pipeline delle richieste è responsabile per richiamare il componente successivo nella pipeline o la catena di corto circuito se appropriato.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-117">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="c3e9a-118">[Moduli HTTP con la migrazione al Middleware](../migration/http-modules.md) viene illustrata la differenza tra le versioni precedente e pipeline di richiesta in ASP.NET Core e fornisce altri esempi di middleware.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-118">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="c3e9a-119">Creazione di una pipeline middleware con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="c3e9a-119">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="c3e9a-120">La pipeline delle richieste ASP.NET Core è costituito da una sequenza di delegati di richiesta, chiamata uno dopo l'altro, perché questo diagramma mostra (il thread di esecuzione segue frecce nere):</span><span class="sxs-lookup"><span data-stu-id="c3e9a-120">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Modello di elaborazione di richiesta che mostra una richiesta in arrivo, l'elaborazione tramite tre middlewares e la risposta e l'applicazione.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="c3e9a-124">Delegati possono eseguire operazioni prima e dopo che il delegato successivo.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="c3e9a-125">Un delegato può anche decidere di non passare una richiesta per il delegato successivo, che viene chiamato la pipeline di richieste di corto circuito.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-125">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="c3e9a-126">Corto circuito è spesso utile perché consente di lavoro non necessario evitare.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-126">Short-circuiting is often desirable because it allows unnecessary work to be avoided.</span></span> <span data-ttu-id="c3e9a-127">Ad esempio, il middleware di file statici può restituire una richiesta di un file statico e il resto della pipeline di corto circuito.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-127">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="c3e9a-128">Gestione delle eccezioni delegati devono essere chiamato nelle prime fasi della pipeline, in modo che può intercettare le eccezioni generate in fasi successive della pipeline.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-128">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="c3e9a-129">L'app di ASP.NET Core più semplice possibile imposta un delegato singola richiesta che gestisce tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-129">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="c3e9a-130">In questo caso non include una pipeline della richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-130">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="c3e9a-131">Al contrario, una singola funzione anonima viene chiamata in risposta a ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-131">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="c3e9a-132">Il primo [app. Eseguire](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegato termina la pipeline.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-132">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="c3e9a-133">È possibile concatenare più delegati richiesta insieme a [app. Utilizzare](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="c3e9a-133">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="c3e9a-134">Il `next` parametro rappresenta il delegato successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-134">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="c3e9a-135">(Tenere presente che è possibile corto circuito di pipeline da *non* la chiamata di *Avanti* parametro.) In genere è possibile eseguire azioni prima e dopo che il delegato successivo, come illustrato in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-135">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="c3e9a-136">Non chiamare `next.Invoke` dopo aver inviata la risposta al client.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-136">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="c3e9a-137">Passa a `HttpResponse` dopo la risposta è stato avviato, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-137">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="c3e9a-138">Ad esempio, le modifiche, ad esempio l'impostazione di intestazioni, codice di stato e così via, genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-138">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="c3e9a-139">Scrivere il corpo della risposta dopo la chiamata `next`:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-139">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="c3e9a-140">Può causare una violazione del protocollo.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-140">May cause a protocol violation.</span></span> <span data-ttu-id="c3e9a-141">Ad esempio, la scrittura di più le `content-length`.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-141">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="c3e9a-142">Potrebbe danneggiare il formato del corpo.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-142">May corrupt the body format.</span></span> <span data-ttu-id="c3e9a-143">Ad esempio, la scrittura di un piè di pagina HTML in un file CSS.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="c3e9a-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) è un suggerimento utile per indicare se le intestazioni sono state inviate e/o ha scritto il corpo.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="c3e9a-145">Ordinazioni</span><span class="sxs-lookup"><span data-stu-id="c3e9a-145">Ordering</span></span>

<span data-ttu-id="c3e9a-146">L'ordine in cui vengono aggiunti i componenti middleware di `Configure` metodo definisce l'ordine in cui vengono richiamati per le richieste e l'ordine inverso per la risposta.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-146">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="c3e9a-147">Questo ordinamento è fondamentale per la sicurezza, prestazioni e funzionalità.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-147">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="c3e9a-148">Il metodo Configure (mostrato sotto) consente di aggiungere i componenti middleware seguenti:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-148">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="c3e9a-149">Gestione degli errori di eccezione /</span><span class="sxs-lookup"><span data-stu-id="c3e9a-149">Exception/error handling</span></span>
2. <span data-ttu-id="c3e9a-150">Server di file statici</span><span class="sxs-lookup"><span data-stu-id="c3e9a-150">Static file server</span></span>
3. <span data-ttu-id="c3e9a-151">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="c3e9a-151">Authentication</span></span>
4. <span data-ttu-id="c3e9a-152">MVC</span><span class="sxs-lookup"><span data-stu-id="c3e9a-152">MVC</span></span>

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

<span data-ttu-id="c3e9a-153">Nel codice precedente, `UseExceptionHandler` è il primo componente middleware aggiunto alla pipeline, pertanto, consente di rilevare le eccezioni che si verificano nelle chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-153">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="c3e9a-154">Il middleware del file statico viene chiamato nelle prime fasi della pipeline, pertanto può gestire le richieste e di corto circuito senza passare attraverso i componenti rimanenti.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-154">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="c3e9a-155">Fornisce il middleware di file statici **non** controlli di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-155">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="c3e9a-156">Qualsiasi file forniti, tra cui quelle contenute *wwwroot*, sono disponibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-156">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="c3e9a-157">Vedere [utilizzo di file statici](xref:fundamentals/static-files) per un approccio proteggere i file statici.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-157">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

<span data-ttu-id="c3e9a-158">Se la richiesta non è gestita dal middleware del file statico, viene passato al middleware di identità (`app.UseIdentity`), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-158">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="c3e9a-159">Identità non di corto circuito le richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-159">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="c3e9a-160">Anche se le richieste vengono autenticate identità, autorizzazione (e rifiuto) si verifica solo dopo aver MVC consente di selezionare un controller specifico e l'azione.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-160">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

<span data-ttu-id="c3e9a-161">Nell'esempio seguente viene illustrato un tipo di middleware ordine in cui le richieste di file statici vengono gestite dal middleware del file statico prima il middleware di compressione di risposta.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-161">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="c3e9a-162">File statici non vengono compressi con questo tipo di ordinamento del middleware.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-162">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="c3e9a-163">Le risposte MVC [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) possono essere compressi.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-163">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="c3e9a-164">Utilizzare, eseguire ed eseguire il mapping</span><span class="sxs-lookup"><span data-stu-id="c3e9a-164">Use, Run, and Map</span></span>

<span data-ttu-id="c3e9a-165">Configurare la pipeline HTTP usando `Use`, `Run`, e `Map`.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-165">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="c3e9a-166">Il `Use` metodo possibile di corto circuito la pipeline (ovvero, se non chiama un `next` delegato richiesta).</span><span class="sxs-lookup"><span data-stu-id="c3e9a-166">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="c3e9a-167">`Run`è una convenzione e possono esporre alcuni componenti middleware `Run[Middleware]` metodi che eseguono alla fine della pipeline.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-167">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="c3e9a-168">`Map*`le estensioni vengono utilizzate come una convenzione per la diramazione la pipeline.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-168">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="c3e9a-169">[Mappa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branch, la pipeline di richieste in base alle corrispondenze del percorso di richiesta specificata.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-169">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="c3e9a-170">Se il percorso della richiesta inizia con il percorso specificato, viene eseguito il ramo.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-170">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="c3e9a-171">La tabella seguente mostra le richieste e risposte da `http://localhost:1234` utilizzando il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-171">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="c3e9a-172">Richiesta</span><span class="sxs-lookup"><span data-stu-id="c3e9a-172">Request</span></span> | <span data-ttu-id="c3e9a-173">Risposta</span><span class="sxs-lookup"><span data-stu-id="c3e9a-173">Response</span></span> |
| --- | --- |
| <span data-ttu-id="c3e9a-174">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="c3e9a-174">localhost:1234</span></span> | <span data-ttu-id="c3e9a-175">Hello dal delegato non mappa.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-175">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="c3e9a-176">localhost:1234 / map1</span><span class="sxs-lookup"><span data-stu-id="c3e9a-176">localhost:1234/map1</span></span> | <span data-ttu-id="c3e9a-177">Eseguire il mapping di Test 1</span><span class="sxs-lookup"><span data-stu-id="c3e9a-177">Map Test 1</span></span> |
| <span data-ttu-id="c3e9a-178">localhost:1234 / map2</span><span class="sxs-lookup"><span data-stu-id="c3e9a-178">localhost:1234/map2</span></span> | <span data-ttu-id="c3e9a-179">Test mappa 2</span><span class="sxs-lookup"><span data-stu-id="c3e9a-179">Map Test 2</span></span> |
| <span data-ttu-id="c3e9a-180">localhost:1234 / map3</span><span class="sxs-lookup"><span data-stu-id="c3e9a-180">localhost:1234/map3</span></span> | <span data-ttu-id="c3e9a-181">Hello dal delegato non mappa.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-181">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="c3e9a-182">Quando `Map` è utilizzato, ai segmenti di percorso corrispondenti vengono rimossi dal `HttpRequest.Path` e accodato a `HttpRequest.PathBase` per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-182">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="c3e9a-183">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branch, la pipeline di richieste in base al risultato del predicato specificato.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-183">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="c3e9a-184">Un predicato di tipo `Func<HttpContext, bool>` può essere usato per eseguire il mapping delle richieste in un nuovo ramo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-184">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="c3e9a-185">Nell'esempio seguente, un predicato viene usato per rilevare la presenza di una variabile di stringa di query `branch`:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-185">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="c3e9a-186">La tabella seguente mostra le richieste e risposte da `http://localhost:1234` utilizzando il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-186">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="c3e9a-187">Richiesta</span><span class="sxs-lookup"><span data-stu-id="c3e9a-187">Request</span></span> | <span data-ttu-id="c3e9a-188">Risposta</span><span class="sxs-lookup"><span data-stu-id="c3e9a-188">Response</span></span> |
| --- | --- |
| <span data-ttu-id="c3e9a-189">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="c3e9a-189">localhost:1234</span></span> | <span data-ttu-id="c3e9a-190">Hello dal delegato non mappa.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-190">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="c3e9a-191">localhost:1234 /? ramo = master</span><span class="sxs-lookup"><span data-stu-id="c3e9a-191">localhost:1234/?branch=master</span></span> | <span data-ttu-id="c3e9a-192">Ramo utilizzato = master</span><span class="sxs-lookup"><span data-stu-id="c3e9a-192">Branch used = master</span></span>|

<span data-ttu-id="c3e9a-193">`Map`supporta la nidificazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-193">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="c3e9a-194">`Map`può anche corrispondere più segmenti in una sola volta, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-194">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="c3e9a-195">Middleware incorporato</span><span class="sxs-lookup"><span data-stu-id="c3e9a-195">Built-in middleware</span></span>

<span data-ttu-id="c3e9a-196">ASP.NET Core viene fornito con i componenti middleware seguenti:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-196">ASP.NET Core ships with the following middleware components:</span></span>

| <span data-ttu-id="c3e9a-197">Middleware</span><span class="sxs-lookup"><span data-stu-id="c3e9a-197">Middleware</span></span> | <span data-ttu-id="c3e9a-198">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c3e9a-198">Description</span></span> |
| ----- | ------- |
| [<span data-ttu-id="c3e9a-199">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="c3e9a-199">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="c3e9a-200">Fornisce supporto per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-200">Provides authentication support.</span></span> |
| [<span data-ttu-id="c3e9a-201">CORS</span><span class="sxs-lookup"><span data-stu-id="c3e9a-201">CORS</span></span>](xref:security/cors) | <span data-ttu-id="c3e9a-202">Configura la condivisione delle risorse Multiorigine.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-202">Configures Cross-Origin Resource Sharing.</span></span> |
| [<span data-ttu-id="c3e9a-203">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="c3e9a-203">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="c3e9a-204">Fornisce supporto per la memorizzazione nella cache le risposte.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-204">Provides support for caching responses.</span></span> |
| [<span data-ttu-id="c3e9a-205">Compressione di risposta</span><span class="sxs-lookup"><span data-stu-id="c3e9a-205">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="c3e9a-206">Fornisce il supporto per la compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-206">Provides support for compressing responses.</span></span> |
| [<span data-ttu-id="c3e9a-207">Routing</span><span class="sxs-lookup"><span data-stu-id="c3e9a-207">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="c3e9a-208">Definisce e vincola una route richiesta.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-208">Defines and constrains request routes.</span></span> |
| [<span data-ttu-id="c3e9a-209">Sessione</span><span class="sxs-lookup"><span data-stu-id="c3e9a-209">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="c3e9a-210">Fornisce il supporto per la gestione delle sessioni utente.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-210">Provides support for managing user sessions.</span></span> |
| [<span data-ttu-id="c3e9a-211">File statici</span><span class="sxs-lookup"><span data-stu-id="c3e9a-211">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="c3e9a-212">Fornisce il supporto per la gestione di file statici e di esplorazione directory.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-212">Provides support for serving static files and directory browsing.</span></span> |
| [<span data-ttu-id="c3e9a-213">Middleware di riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="c3e9a-213">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="c3e9a-214">Fornisce supporto per la riscrittura degli URL e reindirizzamento delle richieste.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-214">Provides support for rewriting URLs and redirecting requests.</span></span> |

<a name=middleware-writing-middleware></a>

## <a name="writing-middleware"></a><span data-ttu-id="c3e9a-215">Middleware di scrittura</span><span class="sxs-lookup"><span data-stu-id="c3e9a-215">Writing middleware</span></span>

<span data-ttu-id="c3e9a-216">Middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-216">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="c3e9a-217">Si consideri il middleware seguente, che imposta le impostazioni cultura per la richiesta corrente dalla stringa di query:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-217">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="c3e9a-218">Nota: Il codice di esempio precedente viene utilizzato per illustrare la creazione di un componente del middleware.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-218">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="c3e9a-219">Vedere [ globalizzazione e localizzazione](xref:fundamentals/localization) per il supporto di localizzazione predefinite di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-219">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="c3e9a-220">È possibile testare il middleware passando le impostazioni cultura, ad esempio `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-220">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="c3e9a-221">Il codice seguente sposta il middleware di delegato a una classe:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-221">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="c3e9a-222">Il seguente metodo di estensione espone il middleware tramite [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="c3e9a-222">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="c3e9a-223">Il codice seguente viene chiamato il middleware da `Configure`:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-223">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="c3e9a-224">Middleware deve seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/) esponendo le relative dipendenze nel relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-224">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="c3e9a-225">Middleware viene costruito una volta per ogni *durata applicazione*.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-225">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="c3e9a-226">Vedere *dipendenze richiesta* sotto se è necessario condividere servizi con middleware all'interno di una richiesta.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-226">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="c3e9a-227">I componenti middleware per risolvere le dipendenze dall'inserimento di dipendenze mediante i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-227">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="c3e9a-228">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)può anche accettare parametri aggiuntivi direttamente.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-228">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="c3e9a-229">Le dipendenze per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="c3e9a-229">Per-request dependencies</span></span>

<span data-ttu-id="c3e9a-230">Poiché middleware viene creato all'avvio dell'app, non per singola richiesta, *con ambito* servizi di durata utilizzati dal middleware costruttori non vengono condivise con altri tipi di dipendenza inserito durante ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-230">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="c3e9a-231">Se è necessario condividere una *con ambito* tra del middleware e altri tipi di servizio, aggiungere questi servizi per il `Invoke` firma del metodo.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-231">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="c3e9a-232">Il `Invoke` metodo può accettare parametri aggiuntivi che vengono popolati dall'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c3e9a-232">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="c3e9a-233">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c3e9a-233">For example:</span></span>

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

## <a name="resources"></a><span data-ttu-id="c3e9a-234">Risorse</span><span class="sxs-lookup"><span data-stu-id="c3e9a-234">Resources</span></span>

* [<span data-ttu-id="c3e9a-235">Codice di esempio utilizzati in questo documento</span><span class="sxs-lookup"><span data-stu-id="c3e9a-235">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="c3e9a-236">Migrazione di moduli HTTP in middleware</span><span class="sxs-lookup"><span data-stu-id="c3e9a-236">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="c3e9a-237">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="c3e9a-237">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="c3e9a-238">Funzionalità di richiesta</span><span class="sxs-lookup"><span data-stu-id="c3e9a-238">Request Features</span></span>](request-features.md)
