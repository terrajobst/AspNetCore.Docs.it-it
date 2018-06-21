---
title: Middleware di ASP.NET Core
author: rick-anderson
description: Informazioni sul middelware di ASP.NET Core e la pipeline delle richieste.
ms.author: riande
ms.date: 01/22/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: d22c7208390ed2de2ca31ead46ecb21bc41671bf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279584"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="75ad2-103">Middleware di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75ad2-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="75ad2-104">[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="75ad2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="75ad2-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="75ad2-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="75ad2-106">Che cos'è il middleware?</span><span class="sxs-lookup"><span data-stu-id="75ad2-106">What is middleware?</span></span>

<span data-ttu-id="75ad2-107">Il middleware è un software che viene assemblato in una pipeline dell'applicazione per gestire richieste e risposte.</span><span class="sxs-lookup"><span data-stu-id="75ad2-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="75ad2-108">Ogni componente:</span><span class="sxs-lookup"><span data-stu-id="75ad2-108">Each component:</span></span>

* <span data-ttu-id="75ad2-109">Sceglie se passare la richiesta al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="75ad2-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="75ad2-110">Può eseguire le operazioni prima e dopo aver chiamato il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="75ad2-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="75ad2-111">Per compilare la pipeline delle richieste vengono usati i delegati di richiesta.</span><span class="sxs-lookup"><span data-stu-id="75ad2-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="75ad2-112">I delegati di richiesta gestiscono ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="75ad2-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="75ad2-113">I delegati di richiesta vengono configurati tramite i metodi di estensione [Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) e [Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="75ad2-113">Request delegates are configured using [Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions), and [Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="75ad2-114">È possibile specificare un singolo delegato di richiesta inline come metodo anonimo (chiamato middleware inline) o definirlo in una classe riutilizzabile.</span><span class="sxs-lookup"><span data-stu-id="75ad2-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="75ad2-115">Le classi riutilizzabili o i metodi anonimi inline costituiscono il *middleware* o sono *componenti middleware*.</span><span class="sxs-lookup"><span data-stu-id="75ad2-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="75ad2-116">Ogni componente del middleware è responsabile della chiamata del componente seguente nella pipeline o del corto circuito della catena, se necessario.</span><span class="sxs-lookup"><span data-stu-id="75ad2-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="75ad2-117">In [Eseguire la migrazione di moduli HTTP in middleware](xref:migration/http-modules) sono descritte le differenze tra le pipeline delle richieste in ASP.NET Core e in ASP.NET 4.x e sono riportati altri esempi di middleware.</span><span class="sxs-lookup"><span data-stu-id="75ad2-117">[Migrate HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="75ad2-118">Creazione di una pipeline middleware con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="75ad2-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="75ad2-119">La pipeline delle richieste ASP.NET Core è costituita da una sequenza di delegati di richiesta, chiamati uno dopo l'altro, come illustrato nella figura seguente (le frecce nere indicano il thread di esecuzione):</span><span class="sxs-lookup"><span data-stu-id="75ad2-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Modello di elaborazione di richiesta che visualizza una richiesta in arrivo, l'elaborazione tramite tre middleware e la risposta che esce dall'applicazione.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="75ad2-123">Ogni delegato può eseguire operazioni prima e dopo il delegato successivo.</span><span class="sxs-lookup"><span data-stu-id="75ad2-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="75ad2-124">Un delegato può anche decidere di non passare una richiesta al delegato successivo e creare un corto circuito della pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="75ad2-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="75ad2-125">Il corto circuito è spesso opportuno poiché evita l'esecuzione di operazioni non necessarie.</span><span class="sxs-lookup"><span data-stu-id="75ad2-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="75ad2-126">Ad esempio, il middleware dei file statici può restituire una richiesta di un file statico ed effettuare il corto circuito della pipeline rimanente.</span><span class="sxs-lookup"><span data-stu-id="75ad2-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="75ad2-127">I delegati che gestiscono le eccezioni devono essere chiamati nella prima parte della pipeline in modo che possano individuare le eccezioni che si verificano nelle parti successive della pipeline.</span><span class="sxs-lookup"><span data-stu-id="75ad2-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="75ad2-128">L'app di ASP.NET Core più semplice imposta un delegato di richiesta singolo che gestisce tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="75ad2-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="75ad2-129">In questo caso non è inclusa una pipeline di richieste effettiva.</span><span class="sxs-lookup"><span data-stu-id="75ad2-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="75ad2-130">Al contrario, viene chiamata una singola funzione anonima in risposta a ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="75ad2-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="75ad2-131">Il primo delegato [app.Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions) termina la pipeline.</span><span class="sxs-lookup"><span data-stu-id="75ad2-131">The first [app.Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="75ad2-132">È possibile concatenare più delegati di richiesta insieme a [app.Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="75ad2-132">You can chain multiple request delegates together with [app.Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="75ad2-133">Il parametro `next` rappresenta il delegato successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="75ad2-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="75ad2-134">(Tenere presente che è possibile eseguire il corto circuito della pipeline *non* chiamando il parametro *successivo*). In genere è possibile eseguire un'azione prima e dopo il delegato successivo, come illustra l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="75ad2-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="75ad2-135">Non chiamare `next.Invoke` dopo aver inviato la risposta al client.</span><span class="sxs-lookup"><span data-stu-id="75ad2-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="75ad2-136">Le modifiche apportate a `HttpResponse` dopo l'avvio della risposta generano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="75ad2-137">Ad esempio, le modifiche come l'impostazione delle intestazioni, del codice di stato e così via, generano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="75ad2-138">Scrivere nel corpo della risposta dopo aver chiamato `next`:</span><span class="sxs-lookup"><span data-stu-id="75ad2-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="75ad2-139">Può causare una violazione del protocollo.</span><span class="sxs-lookup"><span data-stu-id="75ad2-139">May cause a protocol violation.</span></span> <span data-ttu-id="75ad2-140">Ad esempio, scrivere un contenuto che supera il valore `content-length` specificato.</span><span class="sxs-lookup"><span data-stu-id="75ad2-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="75ad2-141">Può danneggiare il formato del corpo.</span><span class="sxs-lookup"><span data-stu-id="75ad2-141">May corrupt the body format.</span></span> <span data-ttu-id="75ad2-142">Ad esempio, scrivere un piè di pagina HTML in un file CSS.</span><span class="sxs-lookup"><span data-stu-id="75ad2-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="75ad2-143">[HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) è un suggerimento utile per indicare se le intestazioni sono state inviate e/o se è stato scritto un contenuto nel corpo.</span><span class="sxs-lookup"><span data-stu-id="75ad2-143">[HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="75ad2-144">Ordinazioni</span><span class="sxs-lookup"><span data-stu-id="75ad2-144">Ordering</span></span>

<span data-ttu-id="75ad2-145">L'ordine in cui vengono aggiunti i componenti middleware nel metodo `Configure` definisce l'ordine in cui sono chiamati nelle richieste e l'ordine inverso per la risposta.</span><span class="sxs-lookup"><span data-stu-id="75ad2-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="75ad2-146">Questo ordinamento è fondamentale per la sicurezza, le prestazioni e la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="75ad2-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="75ad2-147">Il metodo Configure (illustrato di seguito) aggiunge i componenti middleware seguenti:</span><span class="sxs-lookup"><span data-stu-id="75ad2-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="75ad2-148">Gestione errori/eccezioni</span><span class="sxs-lookup"><span data-stu-id="75ad2-148">Exception/error handling</span></span>
2. <span data-ttu-id="75ad2-149">File server statico</span><span class="sxs-lookup"><span data-stu-id="75ad2-149">Static file server</span></span>
3. <span data-ttu-id="75ad2-150">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="75ad2-150">Authentication</span></span>
4. <span data-ttu-id="75ad2-151">MVC</span><span class="sxs-lookup"><span data-stu-id="75ad2-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="75ad2-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="75ad2-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="75ad2-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="75ad2-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="75ad2-154">Nel codice precedente poiché `UseExceptionHandler` è il primo componente middleware aggiunto alla pipeline, il componente consente di rilevare le eccezioni che si verificano nelle chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="75ad2-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="75ad2-155">Il middleware dei file statici viene chiamato nella prima parte della pipeline in moda che possa gestire le richieste ed eseguire un corto circuito senza passare attraverso i componenti rimanenti.</span><span class="sxs-lookup"><span data-stu-id="75ad2-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="75ad2-156">Il middleware dei file statici **non** offre controlli di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="75ad2-157">I file serviti dal sever, inclusi i file in *wwwroot*, sono disponibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="75ad2-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="75ad2-158">Vedere [File statici](xref:fundamentals/static-files) per un approccio alla protezione dei file statici.</span><span class="sxs-lookup"><span data-stu-id="75ad2-158">See [Static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="75ad2-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="75ad2-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="75ad2-160">Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware Identity (`app.UseAuthentication`), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="75ad2-161">Identity non esegue il corto circuito di richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="75ad2-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="75ad2-162">Sebbene Identity esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona una pagina Razor o un controller specifico e un'azione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="75ad2-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="75ad2-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="75ad2-164">Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware Identity (`app.UseIdentity`), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="75ad2-165">Identity non esegue il corto circuito di richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="75ad2-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="75ad2-166">Sebbene Identity esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona un controller specifico e un'azione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="75ad2-167">L'esempio seguente illustra un ordinamento del middleware nel quale le richieste dei file statici vengono gestite dal middleware dei file statici prima del middleware di compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="75ad2-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="75ad2-168">I file statici non vengono compressi con questo tipo di ordinamento del middleware.</span><span class="sxs-lookup"><span data-stu-id="75ad2-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="75ad2-169">Le risposte MVC da [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) possono essere compresse.</span><span class="sxs-lookup"><span data-stu-id="75ad2-169">The MVC responses from [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="75ad2-170">Use, Run e Map</span><span class="sxs-lookup"><span data-stu-id="75ad2-170">Use, Run, and Map</span></span>

<span data-ttu-id="75ad2-171">Configurare la pipeline HTTP usando `Use`,`Run` e `Map`.</span><span class="sxs-lookup"><span data-stu-id="75ad2-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="75ad2-172">Il metodo `Use` può eseguire il corto circuito della pipeline, ovvero non chiamare un delegato di richiesta `next`.</span><span class="sxs-lookup"><span data-stu-id="75ad2-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="75ad2-173">`Run` è una convenzione e alcuni componenti del middleware possono esporre metodi `Run[Middleware]` che vengono eseguiti al termine della pipeline.</span><span class="sxs-lookup"><span data-stu-id="75ad2-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="75ad2-174">Le estensioni `Map*` vengono usate come convenzione per la diramazione della pipeline.</span><span class="sxs-lookup"><span data-stu-id="75ad2-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="75ad2-175">[Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) crea un ramo nella pipeline delle richieste in base alle corrispondenze del percorso della richiesta specificato.</span><span class="sxs-lookup"><span data-stu-id="75ad2-175">[Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="75ad2-176">Se il percorso della richiesta inizia con il percorso specificato, il ramo viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="75ad2-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="75ad2-177">La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="75ad2-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="75ad2-178">Richiesta</span><span class="sxs-lookup"><span data-stu-id="75ad2-178">Request</span></span> | <span data-ttu-id="75ad2-179">Risposta</span><span class="sxs-lookup"><span data-stu-id="75ad2-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="75ad2-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="75ad2-180">localhost:1234</span></span> | <span data-ttu-id="75ad2-181">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="75ad2-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="75ad2-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="75ad2-182">localhost:1234/map1</span></span> | <span data-ttu-id="75ad2-183">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="75ad2-183">Map Test 1</span></span> |
| <span data-ttu-id="75ad2-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="75ad2-184">localhost:1234/map2</span></span> | <span data-ttu-id="75ad2-185">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="75ad2-185">Map Test 2</span></span> |
| <span data-ttu-id="75ad2-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="75ad2-186">localhost:1234/map3</span></span> | <span data-ttu-id="75ad2-187">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="75ad2-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="75ad2-188">Quando si usa `Map`, i segmenti di percorso corrispondenti vengono rimossi da `HttpRequest.Path` e aggiunti a `HttpRequest.PathBase` per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="75ad2-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="75ad2-189">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crea un ramo nella pipeline delle richieste in base al risultato del predicato specificato.</span><span class="sxs-lookup"><span data-stu-id="75ad2-189">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="75ad2-190">È possibile usare qualsiasi predicato di tipo `Func<HttpContext, bool>` per mappare le richieste a un nuovo ramo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="75ad2-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="75ad2-191">Nell'esempio seguente viene usato un predicato per rilevare la presenza di una variabile di stringa di query `branch`:</span><span class="sxs-lookup"><span data-stu-id="75ad2-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="75ad2-192">La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="75ad2-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="75ad2-193">Richiesta</span><span class="sxs-lookup"><span data-stu-id="75ad2-193">Request</span></span> | <span data-ttu-id="75ad2-194">Risposta</span><span class="sxs-lookup"><span data-stu-id="75ad2-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="75ad2-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="75ad2-195">localhost:1234</span></span> | <span data-ttu-id="75ad2-196">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="75ad2-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="75ad2-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="75ad2-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="75ad2-198">Ramo usato = master</span><span class="sxs-lookup"><span data-stu-id="75ad2-198">Branch used = master</span></span>|

<span data-ttu-id="75ad2-199">`Map` supporta l'annidamento, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75ad2-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="75ad2-200">`Map` può anche trovare la corrispondenza di più segmenti contemporaneamente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75ad2-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="75ad2-201">Middleware incorporato</span><span class="sxs-lookup"><span data-stu-id="75ad2-201">Built-in middleware</span></span>

<span data-ttu-id="75ad2-202">ASP.NET Core viene dato con i componenti middleware seguenti e una descrizione dell'ordine in cui devono essere aggiunti:</span><span class="sxs-lookup"><span data-stu-id="75ad2-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="75ad2-203">Middleware</span><span class="sxs-lookup"><span data-stu-id="75ad2-203">Middleware</span></span> | <span data-ttu-id="75ad2-204">Descrizione</span><span class="sxs-lookup"><span data-stu-id="75ad2-204">Description</span></span> | <span data-ttu-id="75ad2-205">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="75ad2-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="75ad2-206">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="75ad2-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="75ad2-207">Offre il supporto dell'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-207">Provides authentication support.</span></span> | <span data-ttu-id="75ad2-208">Prima di `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="75ad2-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="75ad2-209">Terminale per i callback OAuth.</span><span class="sxs-lookup"><span data-stu-id="75ad2-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="75ad2-210">CORS</span><span class="sxs-lookup"><span data-stu-id="75ad2-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="75ad2-211">Configura la condivisione di risorse tra le origini (CORS).</span><span class="sxs-lookup"><span data-stu-id="75ad2-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="75ad2-212">Prima dei componenti che usano CORS.</span><span class="sxs-lookup"><span data-stu-id="75ad2-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="75ad2-213">Diagnostica</span><span class="sxs-lookup"><span data-stu-id="75ad2-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="75ad2-214">Configura la diagnostica.</span><span class="sxs-lookup"><span data-stu-id="75ad2-214">Configures diagnostics.</span></span> | <span data-ttu-id="75ad2-215">Prima dei componenti che generano errori.</span><span class="sxs-lookup"><span data-stu-id="75ad2-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="75ad2-216">Intestazioni inoltrate</span><span class="sxs-lookup"><span data-stu-id="75ad2-216">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="75ad2-217">Inoltra le intestazioni proxy nella richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="75ad2-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="75ad2-218">Prima dei componenti che usano i campi aggiornati, ad esempio schema, host, IP client, metodo.</span><span class="sxs-lookup"><span data-stu-id="75ad2-218">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="75ad2-219">Override del metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="75ad2-219">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="75ad2-220">Consente a una richiesta POST in arrivo di eseguire l'override del metodo.</span><span class="sxs-lookup"><span data-stu-id="75ad2-220">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="75ad2-221">Prima dei componenti che usano il metodo aggiornato.</span><span class="sxs-lookup"><span data-stu-id="75ad2-221">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="75ad2-222">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="75ad2-222">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="75ad2-223">Reindirizzare tutte le richieste HTTP a HTTPS (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="75ad2-223">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="75ad2-224">Prima dei componenti che usano l'URL.</span><span class="sxs-lookup"><span data-stu-id="75ad2-224">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="75ad2-225">Protocollo HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="75ad2-225">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="75ad2-226">Middleware di ottimizzazione della sicurezza che aggiunge un'intestazione della risposta speciale (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="75ad2-226">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="75ad2-227">Prima dell'invio delle risposte e dopo i componenti che modificano le richieste, ad esempio intestazioni inoltrate o riscrittura dell'URL.</span><span class="sxs-lookup"><span data-stu-id="75ad2-227">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="75ad2-228">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="75ad2-228">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="75ad2-229">Offre il supporto per la memorizzazione delle risposte nella cache.</span><span class="sxs-lookup"><span data-stu-id="75ad2-229">Provides support for caching responses.</span></span> | <span data-ttu-id="75ad2-230">Prima dei componenti che richiedono la memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="75ad2-230">Before components that require caching.</span></span> |
| [<span data-ttu-id="75ad2-231">Compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="75ad2-231">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="75ad2-232">Offre il supporto per la compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="75ad2-232">Provides support for compressing responses.</span></span> | <span data-ttu-id="75ad2-233">Prima dei componenti che richiedono la compressione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-233">Before components that require compression.</span></span> |
| [<span data-ttu-id="75ad2-234">Localizzazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="75ad2-234">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="75ad2-235">Offre il supporto per la localizzazione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-235">Provides localization support.</span></span> | <span data-ttu-id="75ad2-236">Prima dei componenti sensibili alla localizzazione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-236">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="75ad2-237">Routing</span><span class="sxs-lookup"><span data-stu-id="75ad2-237">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="75ad2-238">Definisce e vincola le route di richiesta.</span><span class="sxs-lookup"><span data-stu-id="75ad2-238">Defines and constrains request routes.</span></span> | <span data-ttu-id="75ad2-239">Terminale per le route corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="75ad2-239">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="75ad2-240">Sessione</span><span class="sxs-lookup"><span data-stu-id="75ad2-240">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="75ad2-241">Offre il supporto per la gestione delle sessioni utente.</span><span class="sxs-lookup"><span data-stu-id="75ad2-241">Provides support for managing user sessions.</span></span> | <span data-ttu-id="75ad2-242">Prima dei componenti che richiedono la Sessione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-242">Before components that require Session.</span></span> |
| [<span data-ttu-id="75ad2-243">File statici</span><span class="sxs-lookup"><span data-stu-id="75ad2-243">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="75ad2-244">Offre il supporto per la gestione di file statici e l'esplorazione directory.</span><span class="sxs-lookup"><span data-stu-id="75ad2-244">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="75ad2-245">Terminale se una richiesta corrisponde ai file.</span><span class="sxs-lookup"><span data-stu-id="75ad2-245">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="75ad2-246">Riscrittura dell'URL</span><span class="sxs-lookup"><span data-stu-id="75ad2-246">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="75ad2-247">Offre il supporto per la riscrittura degli URL e il reindirizzamento delle richieste.</span><span class="sxs-lookup"><span data-stu-id="75ad2-247">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="75ad2-248">Prima dei componenti che usano l'URL.</span><span class="sxs-lookup"><span data-stu-id="75ad2-248">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="75ad2-249">Oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="75ad2-249">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="75ad2-250">Abilita il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="75ad2-250">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="75ad2-251">Prima dei componenti necessari per accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="75ad2-251">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="75ad2-252">Scrittura di middleware</span><span class="sxs-lookup"><span data-stu-id="75ad2-252">Writing middleware</span></span>

<span data-ttu-id="75ad2-253">Il middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="75ad2-253">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="75ad2-254">Si consideri il middleware seguente, che specifica le impostazioni cultura per la richiesta corrente dalla stringa di query:</span><span class="sxs-lookup"><span data-stu-id="75ad2-254">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="75ad2-255">Nota: il codice di esempio precedente viene usato per illustrare la creazione di un componente middleware.</span><span class="sxs-lookup"><span data-stu-id="75ad2-255">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="75ad2-256">Vedere [Globalizzazione e localizzazione](xref:fundamentals/localization) per il supporto di localizzazione incorporato di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75ad2-256">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="75ad2-257">È possibile testare il middleware passando le impostazioni cultura, ad esempio `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="75ad2-257">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="75ad2-258">Il codice seguente sposta il delegato middleware in una classe:</span><span class="sxs-lookup"><span data-stu-id="75ad2-258">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> <span data-ttu-id="75ad2-259">In ASP.NET Core 1. x il nome del metodo `Task` del middleware deve essere `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="75ad2-259">In ASP.NET Core 1.x, the middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="75ad2-260">In ASP.NET Core 2.0 o versioni successive il nome può essere `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="75ad2-260">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

<span data-ttu-id="75ad2-261">Il metodo di estensione seguente espone il middleware tramite [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="75ad2-261">The following extension method exposes the middleware through [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="75ad2-262">Il codice seguente chiama il middleware da `Configure`:</span><span class="sxs-lookup"><span data-stu-id="75ad2-262">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="75ad2-263">Il middleware deve seguire il [principio delle dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/) esponendo le dipendenze nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="75ad2-263">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="75ad2-264">Il middleware viene costruito una volta per ogni *durata applicazione*.</span><span class="sxs-lookup"><span data-stu-id="75ad2-264">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="75ad2-265">Se è necessario condividere servizi con il middleware all'interno di una richiesta, vedere *Dipendenze per richiesta* di seguito.</span><span class="sxs-lookup"><span data-stu-id="75ad2-265">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="75ad2-266">I componenti middleware possono risolvere le dipendenze dall'inserimento di dipendenze mediante i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="75ad2-266">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="75ad2-267">[`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) può anche accettare direttamente parametri aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="75ad2-267">[`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="75ad2-268">Dipendenze per richiesta</span><span class="sxs-lookup"><span data-stu-id="75ad2-268">Per-request dependencies</span></span>

<span data-ttu-id="75ad2-269">Poiché il middleware viene creato all'avvio dell'app e non per richiesta, i servizi di durata *con ambito* usati dai costruttori del middleware non vengono condivisi con altri tipi di inserimento di dipendenze durante ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="75ad2-269">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="75ad2-270">Se è necessario condividere un servizio *con ambito* tra il proprio middleware e altri tipi, aggiungere i servizi alla firma del metodo `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="75ad2-270">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="75ad2-271">Il metodo `Invoke` può accettare parametri aggiuntivi popolati dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="75ad2-271">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="75ad2-272">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75ad2-272">For example:</span></span>

```csharp
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

## <a name="additional-resources"></a><span data-ttu-id="75ad2-273">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="75ad2-273">Additional resources</span></span>

* [<span data-ttu-id="75ad2-274">Eseguire la migrazione di moduli HTTP in middleware</span><span class="sxs-lookup"><span data-stu-id="75ad2-274">Migrate HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="75ad2-275">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="75ad2-275">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="75ad2-276">Funzionalità di richiesta</span><span class="sxs-lookup"><span data-stu-id="75ad2-276">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="75ad2-277">Attivazione del middleware basata sulla factory</span><span class="sxs-lookup"><span data-stu-id="75ad2-277">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="75ad2-278">Attivazione del middleware con un contenitore di terze parti</span><span class="sxs-lookup"><span data-stu-id="75ad2-278">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
