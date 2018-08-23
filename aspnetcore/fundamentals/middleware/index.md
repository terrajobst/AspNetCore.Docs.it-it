---
title: Middleware di ASP.NET Core
author: rick-anderson
description: Informazioni sul middelware di ASP.NET Core e la pipeline delle richieste.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 9ba77561ab4f6a8668c480d6e81f2ce7e0193c73
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870947"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="23705-103">Middleware di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23705-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="23705-104">[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="23705-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="23705-105">Il middleware è un software che viene assemblato in una pipeline dell'app per gestire richieste e risposte.</span><span class="sxs-lookup"><span data-stu-id="23705-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="23705-106">Ogni componente:</span><span class="sxs-lookup"><span data-stu-id="23705-106">Each component:</span></span>

* <span data-ttu-id="23705-107">Sceglie se passare la richiesta al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="23705-108">Può eseguire le operazioni prima e dopo aver chiamato il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="23705-109">Per compilare la pipeline delle richieste vengono usati i delegati di richiesta.</span><span class="sxs-lookup"><span data-stu-id="23705-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="23705-110">I delegati di richiesta gestiscono ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="23705-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="23705-111">I delegati di richiesta vengono configurati tramite i metodi di estensione <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> e <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="23705-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="23705-112">È possibile specificare un singolo delegato di richiesta inline come metodo anonimo (chiamato middleware inline) o definirlo in una classe riutilizzabile.</span><span class="sxs-lookup"><span data-stu-id="23705-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="23705-113">Le classi riutilizzabili o i metodi anonimi inline costituiscono il *middleware* o sono noti anche come *componenti middleware*.</span><span class="sxs-lookup"><span data-stu-id="23705-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="23705-114">Ogni componente middleware è responsabile della chiamata del componente seguente nella pipeline o del corto circuito della pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="23705-115">In <xref:migration/http-modules> sono descritte le differenze tra le pipeline delle richieste in ASP.NET Core e in ASP.NET 4.x e sono riportati altri esempi di middleware.</span><span class="sxs-lookup"><span data-stu-id="23705-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="23705-116">Creare una pipeline middleware con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="23705-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="23705-117">La pipeline delle richieste ASP.NET Core è costituita da una sequenza di delegati di richiesta, chiamati uno dopo l'altro.</span><span class="sxs-lookup"><span data-stu-id="23705-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="23705-118">Il diagramma seguente illustra il concetto.</span><span class="sxs-lookup"><span data-stu-id="23705-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="23705-119">Il thread di esecuzione seguente le frecce nere.</span><span class="sxs-lookup"><span data-stu-id="23705-119">The thread of execution follows the black arrows.</span></span>

![Modello di elaborazione delle richieste che visualizza una richiesta in arrivo, l'elaborazione tramite tre middleware e la risposta che esce dall'app.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="23705-123">I delegati possono eseguire le operazioni prima del delegato successivo e dopo di esso.</span><span class="sxs-lookup"><span data-stu-id="23705-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="23705-124">Un delegato può anche decidere di non passare una richiesta al delegato successivo e creare un cosiddetto *corto circuito della pipeline delle richieste*.</span><span class="sxs-lookup"><span data-stu-id="23705-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="23705-125">Il corto circuito è spesso opportuno poiché evita l'esecuzione di operazioni non necessarie.</span><span class="sxs-lookup"><span data-stu-id="23705-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="23705-126">Ad esempio, il middleware dei file statici può restituire una richiesta di un file statico ed effettuare il corto circuito della pipeline rimanente.</span><span class="sxs-lookup"><span data-stu-id="23705-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="23705-127">I delegati che gestiscono le eccezioni vengono chiamati nella prima parte della pipeline, in modo che possano intercettare le eccezioni che si verificano nelle parti successive della pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="23705-128">L'app di ASP.NET Core più semplice imposta un delegato di richiesta singolo che gestisce tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="23705-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="23705-129">In questo caso non è inclusa una pipeline di richieste effettiva.</span><span class="sxs-lookup"><span data-stu-id="23705-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="23705-130">Al contrario, viene chiamata una singola funzione anonima in risposta a ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="23705-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="23705-131">Il primo delegato <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> termina la pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="23705-132">È possibile concatenare più delegati di richiesta insieme con <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="23705-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="23705-133">Il parametro `next` rappresenta il delegato successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="23705-134">È possibile eseguire il corto circuito della pipeline *non* chiamando il parametro *next*.</span><span class="sxs-lookup"><span data-stu-id="23705-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="23705-135">In genere è possibile eseguire un'azione prima e dopo il delegato successivo, come illustra l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="23705-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="23705-136">Non chiamare `next.Invoke` dopo aver inviato la risposta al client.</span><span class="sxs-lookup"><span data-stu-id="23705-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="23705-137">Le modifiche apportate a <xref:Microsoft.AspNetCore.Http.HttpResponse> dopo l'avvio della risposta generano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="23705-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="23705-138">Ad esempio, le modifiche come l'impostazione delle intestazioni e di un codice di stato generano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="23705-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="23705-139">Scrivere nel corpo della risposta dopo aver chiamato `next`:</span><span class="sxs-lookup"><span data-stu-id="23705-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="23705-140">Può causare una violazione del protocollo.</span><span class="sxs-lookup"><span data-stu-id="23705-140">May cause a protocol violation.</span></span> <span data-ttu-id="23705-141">Ad esempio, scrivere un contenuto che supera il valore `Content-Length` specificato.</span><span class="sxs-lookup"><span data-stu-id="23705-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="23705-142">Può danneggiare il formato del corpo.</span><span class="sxs-lookup"><span data-stu-id="23705-142">May corrupt the body format.</span></span> <span data-ttu-id="23705-143">Ad esempio, scrivere un piè di pagina HTML in un file CSS.</span><span class="sxs-lookup"><span data-stu-id="23705-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="23705-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> è un hint utile per indicare se le intestazioni sono state inviate o se è stato scritto contenuto nel corpo.</span><span class="sxs-lookup"><span data-stu-id="23705-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="23705-145">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="23705-145">Order</span></span>

<span data-ttu-id="23705-146">L'ordine in cui vengono aggiunti i componenti middleware nel metodo `Startup.Configure` definisce l'ordine in cui i componenti middleware vengono richiamati per le richieste e l'ordine inverso per la risposta.</span><span class="sxs-lookup"><span data-stu-id="23705-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="23705-147">Questo ordinamento è fondamentale per la sicurezza, le prestazioni e la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="23705-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="23705-148">Il metodo `Configure` seguente aggiunge i componenti middleware seguenti:</span><span class="sxs-lookup"><span data-stu-id="23705-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="23705-149">Gestione errori/eccezioni</span><span class="sxs-lookup"><span data-stu-id="23705-149">Exception/error handling</span></span>
2. <span data-ttu-id="23705-150">File server statico</span><span class="sxs-lookup"><span data-stu-id="23705-150">Static file server</span></span>
3. <span data-ttu-id="23705-151">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="23705-151">Authentication</span></span>
4. <span data-ttu-id="23705-152">MVC</span><span class="sxs-lookup"><span data-stu-id="23705-152">MVC</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="23705-153">Nel codice precedente, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> è il primo componente middleware aggiunto alla pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-153">In the code above, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="23705-154">Pertanto, il middleware del gestore di eccezioni intercetta le eccezioni che si verificano nelle chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="23705-154">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="23705-155">Il middleware dei file statici viene chiamato nella prima parte della pipeline in moda che possa gestire le richieste ed eseguire un corto circuito senza passare attraverso i componenti rimanenti.</span><span class="sxs-lookup"><span data-stu-id="23705-155">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="23705-156">Il middleware dei file statici **non** offre controlli di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="23705-156">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="23705-157">I file serviti dal sever, inclusi i file in *wwwroot*, sono disponibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="23705-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="23705-158">Vedere <xref:fundamentals/static-files> per un approccio alla protezione dei file statici.</span><span class="sxs-lookup"><span data-stu-id="23705-158">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="23705-159">Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware di autenticazione (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="23705-159">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="23705-160">L'autenticazione non esegue il corto circuito di richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="23705-160">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="23705-161">Sebbene il middleware di autenticazione esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona una pagina Razor o un controller specifico e un'azione.</span><span class="sxs-lookup"><span data-stu-id="23705-161">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="23705-162">Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware dell'identità (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="23705-162">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="23705-163">Identity non esegue il corto circuito di richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="23705-163">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="23705-164">Sebbene Identity esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona un controller specifico e un'azione.</span><span class="sxs-lookup"><span data-stu-id="23705-164">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="23705-165">L'esempio seguente illustra un ordinamento del middleware nel quale le richieste dei file statici vengono gestite dal middleware dei file statici prima del middleware di compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="23705-165">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="23705-166">I file statici non vengono compressi con questo ordine di middleware.</span><span class="sxs-lookup"><span data-stu-id="23705-166">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="23705-167">Le risposte MVC da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> possono essere compresse.</span><span class="sxs-lookup"><span data-stu-id="23705-167">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="23705-168">Use, Run e Map</span><span class="sxs-lookup"><span data-stu-id="23705-168">Use, Run, and Map</span></span>

<span data-ttu-id="23705-169">Configurare la pipeline HTTP usando `Use`,`Run` e `Map`.</span><span class="sxs-lookup"><span data-stu-id="23705-169">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="23705-170">Il metodo `Use` può eseguire il corto circuito della pipeline, ovvero non chiamare un delegato di richiesta `next`.</span><span class="sxs-lookup"><span data-stu-id="23705-170">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="23705-171">`Run` è una convenzione e alcuni componenti del middleware possono esporre metodi `Run[Middleware]` che vengono eseguiti al termine della pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-171">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="23705-172">Le estensioni <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> vengono usate come convenzione per la diramazione della pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-172"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="23705-173">`Map*` crea un ramo nella pipeline delle richieste in base alle corrispondenze del percorso della richiesta specificato.</span><span class="sxs-lookup"><span data-stu-id="23705-173">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="23705-174">Se il percorso della richiesta inizia con il percorso specificato, il ramo viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="23705-174">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="23705-175">La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente.</span><span class="sxs-lookup"><span data-stu-id="23705-175">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="23705-176">Richiesta</span><span class="sxs-lookup"><span data-stu-id="23705-176">Request</span></span>             | <span data-ttu-id="23705-177">Risposta</span><span class="sxs-lookup"><span data-stu-id="23705-177">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="23705-178">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="23705-178">localhost:1234</span></span>      | <span data-ttu-id="23705-179">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="23705-179">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="23705-180">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="23705-180">localhost:1234/map1</span></span> | <span data-ttu-id="23705-181">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="23705-181">Map Test 1</span></span>                   |
| <span data-ttu-id="23705-182">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="23705-182">localhost:1234/map2</span></span> | <span data-ttu-id="23705-183">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="23705-183">Map Test 2</span></span>                   |
| <span data-ttu-id="23705-184">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="23705-184">localhost:1234/map3</span></span> | <span data-ttu-id="23705-185">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="23705-185">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="23705-186">Quando si usa `Map`, i segmenti di percorso corrispondenti vengono rimossi da `HttpRequest.Path` e aggiunti a `HttpRequest.PathBase` per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="23705-186">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="23705-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crea un ramo nella pipeline delle richieste in base al risultato del predicato specificato.</span><span class="sxs-lookup"><span data-stu-id="23705-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="23705-188">È possibile usare qualsiasi predicato di tipo `Func<HttpContext, bool>` per mappare le richieste a un nuovo ramo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="23705-188">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="23705-189">Nell'esempio seguente viene usato un predicato per rilevare la presenza di una variabile di stringa di query `branch`:</span><span class="sxs-lookup"><span data-stu-id="23705-189">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="23705-190">La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente.</span><span class="sxs-lookup"><span data-stu-id="23705-190">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="23705-191">Richiesta</span><span class="sxs-lookup"><span data-stu-id="23705-191">Request</span></span>                       | <span data-ttu-id="23705-192">Risposta</span><span class="sxs-lookup"><span data-stu-id="23705-192">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="23705-193">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="23705-193">localhost:1234</span></span>                | <span data-ttu-id="23705-194">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="23705-194">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="23705-195">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="23705-195">localhost:1234/?branch=master</span></span> | <span data-ttu-id="23705-196">Ramo usato = master</span><span class="sxs-lookup"><span data-stu-id="23705-196">Branch used = master</span></span>         |

<span data-ttu-id="23705-197">`Map` supporta l'annidamento, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="23705-197">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="23705-198">`Map` può anche trovare la corrispondenza di più segmenti contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="23705-198">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="23705-199">Middleware incorporato</span><span class="sxs-lookup"><span data-stu-id="23705-199">Built-in middleware</span></span>

<span data-ttu-id="23705-200">ASP.NET Core include i componenti middleware seguenti.</span><span class="sxs-lookup"><span data-stu-id="23705-200">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="23705-201">Nella colonna *Ordinamento* sono disponibili note sul posizionamento del middleware nella pipeline delle richieste e indicazioni sulle condizioni nelle quali il middleware può terminare la richiesta e impedire l'elaborazione di una richiesta da altro middleware.</span><span class="sxs-lookup"><span data-stu-id="23705-201">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="23705-202">Middleware</span><span class="sxs-lookup"><span data-stu-id="23705-202">Middleware</span></span> | <span data-ttu-id="23705-203">Descrizione</span><span class="sxs-lookup"><span data-stu-id="23705-203">Description</span></span> | <span data-ttu-id="23705-204">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="23705-204">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="23705-205">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="23705-205">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="23705-206">Offre il supporto dell'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="23705-206">Provides authentication support.</span></span> | <span data-ttu-id="23705-207">Prima di `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="23705-207">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="23705-208">Terminale per i callback OAuth.</span><span class="sxs-lookup"><span data-stu-id="23705-208">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="23705-209">CORS</span><span class="sxs-lookup"><span data-stu-id="23705-209">CORS</span></span>](xref:security/cors) | <span data-ttu-id="23705-210">Configura la condivisione di risorse tra le origini (CORS).</span><span class="sxs-lookup"><span data-stu-id="23705-210">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="23705-211">Prima dei componenti che usano CORS.</span><span class="sxs-lookup"><span data-stu-id="23705-211">Before components that use CORS.</span></span> |
| [<span data-ttu-id="23705-212">Diagnostica</span><span class="sxs-lookup"><span data-stu-id="23705-212">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="23705-213">Configura la diagnostica.</span><span class="sxs-lookup"><span data-stu-id="23705-213">Configures diagnostics.</span></span> | <span data-ttu-id="23705-214">Prima dei componenti che generano errori.</span><span class="sxs-lookup"><span data-stu-id="23705-214">Before components that generate errors.</span></span> |
| [<span data-ttu-id="23705-215">Intestazioni inoltrate</span><span class="sxs-lookup"><span data-stu-id="23705-215">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="23705-216">Inoltra le intestazioni proxy nella richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="23705-216">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="23705-217">Prima dei componenti che usano i campi aggiornati, ad esempio schema, host, IP client, metodo.</span><span class="sxs-lookup"><span data-stu-id="23705-217">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="23705-218">Override del metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="23705-218">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="23705-219">Consente a una richiesta POST in arrivo di eseguire l'override del metodo.</span><span class="sxs-lookup"><span data-stu-id="23705-219">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="23705-220">Prima dei componenti che usano il metodo aggiornato.</span><span class="sxs-lookup"><span data-stu-id="23705-220">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="23705-221">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="23705-221">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="23705-222">Reindirizzare tutte le richieste HTTP a HTTPS (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="23705-222">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="23705-223">Prima dei componenti che usano l'URL.</span><span class="sxs-lookup"><span data-stu-id="23705-223">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="23705-224">Protocollo HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="23705-224">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="23705-225">Middleware di ottimizzazione della sicurezza che aggiunge un'intestazione della risposta speciale (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="23705-225">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="23705-226">Prima dell'invio delle risposte e dopo i componenti che modificano le richieste, ad esempio intestazioni inoltrate o riscrittura dell'URL.</span><span class="sxs-lookup"><span data-stu-id="23705-226">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="23705-227">MVC</span><span class="sxs-lookup"><span data-stu-id="23705-227">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="23705-228">Elabora le richieste con MVC/Razor Pages (ASP.NET Core 2.0 o versione successiva).</span><span class="sxs-lookup"><span data-stu-id="23705-228">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="23705-229">Terminale se una richiesta corrisponde a una route.</span><span class="sxs-lookup"><span data-stu-id="23705-229">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="23705-230">OWIN</span><span class="sxs-lookup"><span data-stu-id="23705-230">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="23705-231">Interoperabilità con app, server e middleware basati su OWIN.</span><span class="sxs-lookup"><span data-stu-id="23705-231">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="23705-232">Terminale se la richiesta viene elaborata completamente dal middleware OWIN.</span><span class="sxs-lookup"><span data-stu-id="23705-232">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="23705-233">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="23705-233">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="23705-234">Offre il supporto per la memorizzazione delle risposte nella cache.</span><span class="sxs-lookup"><span data-stu-id="23705-234">Provides support for caching responses.</span></span> | <span data-ttu-id="23705-235">Prima dei componenti che richiedono la memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="23705-235">Before components that require caching.</span></span> |
| [<span data-ttu-id="23705-236">Compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="23705-236">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="23705-237">Offre il supporto per la compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="23705-237">Provides support for compressing responses.</span></span> | <span data-ttu-id="23705-238">Prima dei componenti che richiedono la compressione.</span><span class="sxs-lookup"><span data-stu-id="23705-238">Before components that require compression.</span></span> |
| [<span data-ttu-id="23705-239">Localizzazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="23705-239">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="23705-240">Offre il supporto per la localizzazione.</span><span class="sxs-lookup"><span data-stu-id="23705-240">Provides localization support.</span></span> | <span data-ttu-id="23705-241">Prima dei componenti sensibili alla localizzazione.</span><span class="sxs-lookup"><span data-stu-id="23705-241">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="23705-242">Routing</span><span class="sxs-lookup"><span data-stu-id="23705-242">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="23705-243">Definisce e vincola le route di richiesta.</span><span class="sxs-lookup"><span data-stu-id="23705-243">Defines and constrains request routes.</span></span> | <span data-ttu-id="23705-244">Terminale per le route corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="23705-244">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="23705-245">Sessione</span><span class="sxs-lookup"><span data-stu-id="23705-245">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="23705-246">Offre il supporto per la gestione delle sessioni utente.</span><span class="sxs-lookup"><span data-stu-id="23705-246">Provides support for managing user sessions.</span></span> | <span data-ttu-id="23705-247">Prima dei componenti che richiedono la Sessione.</span><span class="sxs-lookup"><span data-stu-id="23705-247">Before components that require Session.</span></span> |
| [<span data-ttu-id="23705-248">File statici</span><span class="sxs-lookup"><span data-stu-id="23705-248">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="23705-249">Offre il supporto per la gestione di file statici e l'esplorazione directory.</span><span class="sxs-lookup"><span data-stu-id="23705-249">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="23705-250">Terminale se una richiesta corrisponde a un file.</span><span class="sxs-lookup"><span data-stu-id="23705-250">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="23705-251">Riscrittura dell'URL</span><span class="sxs-lookup"><span data-stu-id="23705-251">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="23705-252">Offre il supporto per la riscrittura degli URL e il reindirizzamento delle richieste.</span><span class="sxs-lookup"><span data-stu-id="23705-252">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="23705-253">Prima dei componenti che usano l'URL.</span><span class="sxs-lookup"><span data-stu-id="23705-253">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="23705-254">Oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="23705-254">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="23705-255">Abilita il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="23705-255">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="23705-256">Prima dei componenti necessari per accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="23705-256">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="23705-257">Scrivere il middleware</span><span class="sxs-lookup"><span data-stu-id="23705-257">Write middleware</span></span>

<span data-ttu-id="23705-258">Il middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="23705-258">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="23705-259">Si consideri il middleware seguente, che specifica le impostazioni cultura per la richiesta corrente da una stringa di query:</span><span class="sxs-lookup"><span data-stu-id="23705-259">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="23705-260">Il codice di esempio precedente viene usato per illustrare la creazione di un componente middleware.</span><span class="sxs-lookup"><span data-stu-id="23705-260">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="23705-261">Per il supporto di localizzazione incorporato di ASP.NET Core, vedere <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="23705-261">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="23705-262">È possibile testare il middleware passando le impostazioni cultura, ad esempio `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="23705-262">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="23705-263">Il codice seguente sposta il delegato middleware in una classe:</span><span class="sxs-lookup"><span data-stu-id="23705-263">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="23705-264">Il nome del metodo `Task` del middleware deve essere `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="23705-264">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="23705-265">In ASP.NET Core 2.0 o versioni successive il nome può essere `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="23705-265">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="23705-266">Il metodo di estensione seguente espone il middleware tramite <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="23705-266">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="23705-267">Il codice seguente chiama il middleware da `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="23705-267">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="23705-268">Il middleware deve seguire il [principio delle dipendenze esplicite](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) esponendo le dipendenze nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="23705-268">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="23705-269">Il middleware viene costruito una volta per ogni *durata applicazione*.</span><span class="sxs-lookup"><span data-stu-id="23705-269">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="23705-270">Se è necessario condividere servizi con il middleware all'interno di una richiesta, vedere la sezione [Dipendenze per richiesta](#per-request-dependencies).</span><span class="sxs-lookup"><span data-stu-id="23705-270">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="23705-271">I componenti middleware possono risolvere le dipendenze dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection) mediante i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="23705-271">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="23705-272">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) può anche accettare direttamente parametri aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="23705-272">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="23705-273">Dipendenze per richiesta</span><span class="sxs-lookup"><span data-stu-id="23705-273">Per-request dependencies</span></span>

<span data-ttu-id="23705-274">Poiché il middleware viene creato all'avvio dell'app e non per richiesta, i servizi di durata *con ambito* usati dai costruttori del middleware non vengono condivisi con altri tipi di inserimento di dipendenze durante ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="23705-274">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="23705-275">Se è necessario condividere un servizio *con ambito* tra il proprio middleware e altri tipi, aggiungere i servizi alla firma del metodo `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="23705-275">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="23705-276">Il metodo `Invoke` può accettare parametri aggiuntivi popolati dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="23705-276">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="23705-277">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="23705-277">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
