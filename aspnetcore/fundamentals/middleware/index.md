---
title: Middleware di ASP.NET Core
author: rick-anderson
description: Informazioni sul middelware di ASP.NET Core e la pipeline delle richieste.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: e6dc76b7cb80e0dfda102df5aefb5d9ce9b821ed
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055806"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="f42f4-103">Middleware di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f42f4-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="f42f4-104">[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f42f4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f42f4-105">Il middleware è un software che viene assemblato in una pipeline dell'app per gestire richieste e risposte.</span><span class="sxs-lookup"><span data-stu-id="f42f4-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="f42f4-106">Ogni componente:</span><span class="sxs-lookup"><span data-stu-id="f42f4-106">Each component:</span></span>

* <span data-ttu-id="f42f4-107">Sceglie se passare la richiesta al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="f42f4-108">Può eseguire le operazioni prima e dopo aver chiamato il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="f42f4-109">Per compilare la pipeline delle richieste vengono usati i delegati di richiesta.</span><span class="sxs-lookup"><span data-stu-id="f42f4-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="f42f4-110">I delegati di richiesta gestiscono ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f42f4-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="f42f4-111">I delegati di richiesta vengono configurati tramite i metodi di estensione <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> e <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="f42f4-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="f42f4-112">È possibile specificare un singolo delegato di richiesta inline come metodo anonimo (chiamato middleware inline) o definirlo in una classe riutilizzabile.</span><span class="sxs-lookup"><span data-stu-id="f42f4-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="f42f4-113">Le classi riutilizzabili o i metodi anonimi inline costituiscono il *middleware* o sono noti anche come *componenti middleware*.</span><span class="sxs-lookup"><span data-stu-id="f42f4-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="f42f4-114">Ogni componente middleware è responsabile della chiamata del componente seguente nella pipeline o del corto circuito della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="f42f4-115">In <xref:migration/http-modules> sono descritte le differenze tra le pipeline delle richieste in ASP.NET Core e in ASP.NET 4.x e sono riportati altri esempi di middleware.</span><span class="sxs-lookup"><span data-stu-id="f42f4-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="f42f4-116">Creare una pipeline middleware con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="f42f4-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="f42f4-117">La pipeline delle richieste ASP.NET Core è costituita da una sequenza di delegati di richiesta, chiamati uno dopo l'altro.</span><span class="sxs-lookup"><span data-stu-id="f42f4-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="f42f4-118">Il diagramma seguente illustra il concetto.</span><span class="sxs-lookup"><span data-stu-id="f42f4-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="f42f4-119">Il thread di esecuzione seguente le frecce nere.</span><span class="sxs-lookup"><span data-stu-id="f42f4-119">The thread of execution follows the black arrows.</span></span>

![Modello di elaborazione delle richieste che visualizza una richiesta in arrivo, l'elaborazione tramite tre middleware e la risposta che esce dall'app.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="f42f4-123">I delegati possono eseguire le operazioni prima del delegato successivo e dopo di esso.</span><span class="sxs-lookup"><span data-stu-id="f42f4-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="f42f4-124">Un delegato può anche decidere di non passare una richiesta al delegato successivo e creare un cosiddetto *corto circuito della pipeline delle richieste*.</span><span class="sxs-lookup"><span data-stu-id="f42f4-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="f42f4-125">Il corto circuito è spesso opportuno poiché evita l'esecuzione di operazioni non necessarie.</span><span class="sxs-lookup"><span data-stu-id="f42f4-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="f42f4-126">Ad esempio, il middleware dei file statici può restituire una richiesta di un file statico ed effettuare il corto circuito della pipeline rimanente.</span><span class="sxs-lookup"><span data-stu-id="f42f4-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="f42f4-127">I delegati che gestiscono le eccezioni vengono chiamati nella prima parte della pipeline, in modo che possano intercettare le eccezioni che si verificano nelle parti successive della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="f42f4-128">L'app di ASP.NET Core più semplice imposta un delegato di richiesta singolo che gestisce tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="f42f4-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="f42f4-129">In questo caso non è inclusa una pipeline di richieste effettiva.</span><span class="sxs-lookup"><span data-stu-id="f42f4-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="f42f4-130">Al contrario, viene chiamata una singola funzione anonima in risposta a ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f42f4-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="f42f4-131">Il primo delegato <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> termina la pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="f42f4-132">È possibile concatenare più delegati di richiesta insieme con <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="f42f4-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="f42f4-133">Il parametro `next` rappresenta il delegato successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="f42f4-134">È possibile eseguire il corto circuito della pipeline *non* chiamando il parametro *next*.</span><span class="sxs-lookup"><span data-stu-id="f42f4-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="f42f4-135">In genere è possibile eseguire un'azione prima e dopo il delegato successivo, come illustra l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f42f4-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="f42f4-136">Non chiamare `next.Invoke` dopo aver inviato la risposta al client.</span><span class="sxs-lookup"><span data-stu-id="f42f4-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="f42f4-137">Le modifiche apportate a <xref:Microsoft.AspNetCore.Http.HttpResponse> dopo l'avvio della risposta generano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="f42f4-138">Ad esempio, le modifiche come l'impostazione delle intestazioni e di un codice di stato generano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="f42f4-139">Scrivere nel corpo della risposta dopo aver chiamato `next`:</span><span class="sxs-lookup"><span data-stu-id="f42f4-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="f42f4-140">Può causare una violazione del protocollo.</span><span class="sxs-lookup"><span data-stu-id="f42f4-140">May cause a protocol violation.</span></span> <span data-ttu-id="f42f4-141">Ad esempio, scrivere un contenuto che supera il valore `Content-Length` specificato.</span><span class="sxs-lookup"><span data-stu-id="f42f4-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="f42f4-142">Può danneggiare il formato del corpo.</span><span class="sxs-lookup"><span data-stu-id="f42f4-142">May corrupt the body format.</span></span> <span data-ttu-id="f42f4-143">Ad esempio, scrivere un piè di pagina HTML in un file CSS.</span><span class="sxs-lookup"><span data-stu-id="f42f4-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="f42f4-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> è un hint utile per indicare se le intestazioni sono state inviate o se è stato scritto contenuto nel corpo.</span><span class="sxs-lookup"><span data-stu-id="f42f4-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="f42f4-145">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="f42f4-145">Order</span></span>

<span data-ttu-id="f42f4-146">L'ordine in cui vengono aggiunti i componenti middleware nel metodo `Startup.Configure` definisce l'ordine in cui i componenti middleware vengono richiamati per le richieste e l'ordine inverso per la risposta.</span><span class="sxs-lookup"><span data-stu-id="f42f4-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="f42f4-147">Questo ordinamento è fondamentale per la sicurezza, le prestazioni e la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="f42f4-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="f42f4-148">Il metodo `Configure` seguente aggiunge i componenti middleware seguenti:</span><span class="sxs-lookup"><span data-stu-id="f42f4-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="f42f4-149">Gestione errori/eccezioni</span><span class="sxs-lookup"><span data-stu-id="f42f4-149">Exception/error handling</span></span>
2. <span data-ttu-id="f42f4-150">File server statico</span><span class="sxs-lookup"><span data-stu-id="f42f4-150">Static file server</span></span>
3. <span data-ttu-id="f42f4-151">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="f42f4-151">Authentication</span></span>
4. <span data-ttu-id="f42f4-152">MVC</span><span class="sxs-lookup"><span data-stu-id="f42f4-152">MVC</span></span>

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

<span data-ttu-id="f42f4-153">Nell'esempio di codice precedente, ogni metodo di estensione del middleware viene esposto in <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> tramite lo spazio dei nomi <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="f42f4-153">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="f42f4-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> è il primo componente middleware aggiunto alla pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="f42f4-155">Pertanto, il middleware del gestore di eccezioni intercetta le eccezioni che si verificano nelle chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="f42f4-155">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="f42f4-156">Il middleware dei file statici viene chiamato nella prima parte della pipeline in moda che possa gestire le richieste ed eseguire un corto circuito senza passare attraverso i componenti rimanenti.</span><span class="sxs-lookup"><span data-stu-id="f42f4-156">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="f42f4-157">Il middleware dei file statici **non** offre controlli di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-157">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="f42f4-158">I file serviti dal sever, inclusi i file in *wwwroot*, sono disponibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="f42f4-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="f42f4-159">Vedere <xref:fundamentals/static-files> per un approccio alla protezione dei file statici.</span><span class="sxs-lookup"><span data-stu-id="f42f4-159">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f42f4-160">Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware di autenticazione (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-160">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="f42f4-161">L'autenticazione non esegue il corto circuito di richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="f42f4-161">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="f42f4-162">Sebbene il middleware di autenticazione esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona una pagina Razor o un controller specifico e un'azione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-162">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f42f4-163">Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware dell'identità (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-163">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="f42f4-164">Identity non esegue il corto circuito di richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="f42f4-164">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="f42f4-165">Sebbene Identity esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona un controller specifico e un'azione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-165">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="f42f4-166">L'esempio seguente illustra un ordinamento del middleware nel quale le richieste dei file statici vengono gestite dal middleware dei file statici prima del middleware di compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="f42f4-166">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="f42f4-167">I file statici non vengono compressi con questo ordine di middleware.</span><span class="sxs-lookup"><span data-stu-id="f42f4-167">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="f42f4-168">Le risposte MVC da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> possono essere compresse.</span><span class="sxs-lookup"><span data-stu-id="f42f4-168">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="f42f4-169">Use, Run e Map</span><span class="sxs-lookup"><span data-stu-id="f42f4-169">Use, Run, and Map</span></span>

<span data-ttu-id="f42f4-170">Configurare la pipeline HTTP usando `Use`,`Run` e `Map`.</span><span class="sxs-lookup"><span data-stu-id="f42f4-170">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="f42f4-171">Il metodo `Use` può eseguire il corto circuito della pipeline, ovvero non chiamare un delegato di richiesta `next`.</span><span class="sxs-lookup"><span data-stu-id="f42f4-171">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="f42f4-172">`Run` è una convenzione e alcuni componenti del middleware possono esporre metodi `Run[Middleware]` che vengono eseguiti al termine della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-172">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="f42f4-173">Le estensioni <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> vengono usate come convenzione per la diramazione della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-173"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="f42f4-174">`Map*` crea un ramo nella pipeline delle richieste in base alle corrispondenze del percorso della richiesta specificato.</span><span class="sxs-lookup"><span data-stu-id="f42f4-174">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="f42f4-175">Se il percorso della richiesta inizia con il percorso specificato, il ramo viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="f42f4-175">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="f42f4-176">La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente.</span><span class="sxs-lookup"><span data-stu-id="f42f4-176">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="f42f4-177">Richiesta</span><span class="sxs-lookup"><span data-stu-id="f42f4-177">Request</span></span>             | <span data-ttu-id="f42f4-178">Risposta</span><span class="sxs-lookup"><span data-stu-id="f42f4-178">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="f42f4-179">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="f42f4-179">localhost:1234</span></span>      | <span data-ttu-id="f42f4-180">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="f42f4-180">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="f42f4-181">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="f42f4-181">localhost:1234/map1</span></span> | <span data-ttu-id="f42f4-182">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="f42f4-182">Map Test 1</span></span>                   |
| <span data-ttu-id="f42f4-183">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="f42f4-183">localhost:1234/map2</span></span> | <span data-ttu-id="f42f4-184">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="f42f4-184">Map Test 2</span></span>                   |
| <span data-ttu-id="f42f4-185">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="f42f4-185">localhost:1234/map3</span></span> | <span data-ttu-id="f42f4-186">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="f42f4-186">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="f42f4-187">Quando si usa `Map`, i segmenti di percorso corrispondenti vengono rimossi da `HttpRequest.Path` e aggiunti a `HttpRequest.PathBase` per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="f42f4-187">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="f42f4-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crea un ramo nella pipeline delle richieste in base al risultato del predicato specificato.</span><span class="sxs-lookup"><span data-stu-id="f42f4-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="f42f4-189">È possibile usare qualsiasi predicato di tipo `Func<HttpContext, bool>` per mappare le richieste a un nuovo ramo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f42f4-189">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="f42f4-190">Nell'esempio seguente viene usato un predicato per rilevare la presenza di una variabile di stringa di query `branch`:</span><span class="sxs-lookup"><span data-stu-id="f42f4-190">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="f42f4-191">La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente.</span><span class="sxs-lookup"><span data-stu-id="f42f4-191">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="f42f4-192">Richiesta</span><span class="sxs-lookup"><span data-stu-id="f42f4-192">Request</span></span>                       | <span data-ttu-id="f42f4-193">Risposta</span><span class="sxs-lookup"><span data-stu-id="f42f4-193">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="f42f4-194">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="f42f4-194">localhost:1234</span></span>                | <span data-ttu-id="f42f4-195">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="f42f4-195">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="f42f4-196">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="f42f4-196">localhost:1234/?branch=master</span></span> | <span data-ttu-id="f42f4-197">Ramo usato = master</span><span class="sxs-lookup"><span data-stu-id="f42f4-197">Branch used = master</span></span>         |

<span data-ttu-id="f42f4-198">`Map` supporta l'annidamento, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f42f4-198">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="f42f4-199">`Map` può anche trovare la corrispondenza di più segmenti contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="f42f4-199">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="f42f4-200">Middleware incorporato</span><span class="sxs-lookup"><span data-stu-id="f42f4-200">Built-in middleware</span></span>

<span data-ttu-id="f42f4-201">ASP.NET Core include i componenti middleware seguenti.</span><span class="sxs-lookup"><span data-stu-id="f42f4-201">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="f42f4-202">Nella colonna *Ordinamento* sono disponibili note sul posizionamento del middleware nella pipeline delle richieste e indicazioni sulle condizioni nelle quali il middleware può terminare la richiesta e impedire l'elaborazione di una richiesta da altro middleware.</span><span class="sxs-lookup"><span data-stu-id="f42f4-202">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="f42f4-203">Middleware</span><span class="sxs-lookup"><span data-stu-id="f42f4-203">Middleware</span></span> | <span data-ttu-id="f42f4-204">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f42f4-204">Description</span></span> | <span data-ttu-id="f42f4-205">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="f42f4-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="f42f4-206">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="f42f4-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="f42f4-207">Offre il supporto dell'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-207">Provides authentication support.</span></span> | <span data-ttu-id="f42f4-208">Prima di `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="f42f4-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="f42f4-209">Terminale per i callback OAuth.</span><span class="sxs-lookup"><span data-stu-id="f42f4-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="f42f4-210">CORS</span><span class="sxs-lookup"><span data-stu-id="f42f4-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="f42f4-211">Configura la condivisione di risorse tra le origini (CORS).</span><span class="sxs-lookup"><span data-stu-id="f42f4-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="f42f4-212">Prima dei componenti che usano CORS.</span><span class="sxs-lookup"><span data-stu-id="f42f4-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="f42f4-213">Diagnostica</span><span class="sxs-lookup"><span data-stu-id="f42f4-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="f42f4-214">Configura la diagnostica.</span><span class="sxs-lookup"><span data-stu-id="f42f4-214">Configures diagnostics.</span></span> | <span data-ttu-id="f42f4-215">Prima dei componenti che generano errori.</span><span class="sxs-lookup"><span data-stu-id="f42f4-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="f42f4-216">Intestazioni inoltrate</span><span class="sxs-lookup"><span data-stu-id="f42f4-216">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="f42f4-217">Inoltra le intestazioni proxy nella richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="f42f4-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="f42f4-218">Prima dei componenti che usano i campi aggiornati, ad esempio schema, host, IP client, metodo.</span><span class="sxs-lookup"><span data-stu-id="f42f4-218">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="f42f4-219">Override del metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="f42f4-219">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="f42f4-220">Consente a una richiesta POST in arrivo di eseguire l'override del metodo.</span><span class="sxs-lookup"><span data-stu-id="f42f4-220">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="f42f4-221">Prima dei componenti che usano il metodo aggiornato.</span><span class="sxs-lookup"><span data-stu-id="f42f4-221">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="f42f4-222">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="f42f4-222">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="f42f4-223">Reindirizzare tutte le richieste HTTP a HTTPS (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="f42f4-223">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="f42f4-224">Prima dei componenti che usano l'URL.</span><span class="sxs-lookup"><span data-stu-id="f42f4-224">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="f42f4-225">Protocollo HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="f42f4-225">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="f42f4-226">Middleware di ottimizzazione della sicurezza che aggiunge un'intestazione della risposta speciale (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="f42f4-226">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="f42f4-227">Prima dell'invio delle risposte e dopo i componenti che modificano le richieste, ad esempio intestazioni inoltrate o riscrittura dell'URL.</span><span class="sxs-lookup"><span data-stu-id="f42f4-227">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="f42f4-228">MVC</span><span class="sxs-lookup"><span data-stu-id="f42f4-228">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="f42f4-229">Elabora le richieste con MVC/Razor Pages (ASP.NET Core 2.0 o versione successiva).</span><span class="sxs-lookup"><span data-stu-id="f42f4-229">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="f42f4-230">Terminale se una richiesta corrisponde a una route.</span><span class="sxs-lookup"><span data-stu-id="f42f4-230">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="f42f4-231">OWIN</span><span class="sxs-lookup"><span data-stu-id="f42f4-231">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="f42f4-232">Interoperabilità con app, server e middleware basati su OWIN.</span><span class="sxs-lookup"><span data-stu-id="f42f4-232">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="f42f4-233">Terminale se la richiesta viene elaborata completamente dal middleware OWIN.</span><span class="sxs-lookup"><span data-stu-id="f42f4-233">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="f42f4-234">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="f42f4-234">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="f42f4-235">Offre il supporto per la memorizzazione delle risposte nella cache.</span><span class="sxs-lookup"><span data-stu-id="f42f4-235">Provides support for caching responses.</span></span> | <span data-ttu-id="f42f4-236">Prima dei componenti che richiedono la memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="f42f4-236">Before components that require caching.</span></span> |
| [<span data-ttu-id="f42f4-237">Compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="f42f4-237">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="f42f4-238">Offre il supporto per la compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="f42f4-238">Provides support for compressing responses.</span></span> | <span data-ttu-id="f42f4-239">Prima dei componenti che richiedono la compressione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-239">Before components that require compression.</span></span> |
| [<span data-ttu-id="f42f4-240">Localizzazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="f42f4-240">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="f42f4-241">Offre il supporto per la localizzazione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-241">Provides localization support.</span></span> | <span data-ttu-id="f42f4-242">Prima dei componenti sensibili alla localizzazione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-242">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="f42f4-243">Routing</span><span class="sxs-lookup"><span data-stu-id="f42f4-243">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="f42f4-244">Definisce e vincola le route di richiesta.</span><span class="sxs-lookup"><span data-stu-id="f42f4-244">Defines and constrains request routes.</span></span> | <span data-ttu-id="f42f4-245">Terminale per le route corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="f42f4-245">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="f42f4-246">Sessione</span><span class="sxs-lookup"><span data-stu-id="f42f4-246">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="f42f4-247">Offre il supporto per la gestione delle sessioni utente.</span><span class="sxs-lookup"><span data-stu-id="f42f4-247">Provides support for managing user sessions.</span></span> | <span data-ttu-id="f42f4-248">Prima dei componenti che richiedono la Sessione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-248">Before components that require Session.</span></span> |
| [<span data-ttu-id="f42f4-249">File statici</span><span class="sxs-lookup"><span data-stu-id="f42f4-249">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="f42f4-250">Offre il supporto per la gestione di file statici e l'esplorazione directory.</span><span class="sxs-lookup"><span data-stu-id="f42f4-250">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="f42f4-251">Terminale se una richiesta corrisponde a un file.</span><span class="sxs-lookup"><span data-stu-id="f42f4-251">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="f42f4-252">Riscrittura dell'URL</span><span class="sxs-lookup"><span data-stu-id="f42f4-252">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="f42f4-253">Offre il supporto per la riscrittura degli URL e il reindirizzamento delle richieste.</span><span class="sxs-lookup"><span data-stu-id="f42f4-253">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="f42f4-254">Prima dei componenti che usano l'URL.</span><span class="sxs-lookup"><span data-stu-id="f42f4-254">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="f42f4-255">Oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="f42f4-255">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="f42f4-256">Abilita il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f42f4-256">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="f42f4-257">Prima dei componenti necessari per accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f42f4-257">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="f42f4-258">Scrivere il middleware</span><span class="sxs-lookup"><span data-stu-id="f42f4-258">Write middleware</span></span>

<span data-ttu-id="f42f4-259">Il middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="f42f4-259">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="f42f4-260">Si consideri il middleware seguente, che specifica le impostazioni cultura per la richiesta corrente da una stringa di query:</span><span class="sxs-lookup"><span data-stu-id="f42f4-260">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="f42f4-261">Il codice di esempio precedente viene usato per illustrare la creazione di un componente middleware.</span><span class="sxs-lookup"><span data-stu-id="f42f4-261">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="f42f4-262">Per il supporto di localizzazione incorporato di ASP.NET Core, vedere <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="f42f4-262">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="f42f4-263">È possibile testare il middleware passando le impostazioni cultura, ad esempio `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="f42f4-263">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="f42f4-264">Il codice seguente sposta il delegato middleware in una classe:</span><span class="sxs-lookup"><span data-stu-id="f42f4-264">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f42f4-265">Il nome del metodo `Task` del middleware deve essere `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="f42f4-265">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="f42f4-266">In ASP.NET Core 2.0 o versioni successive il nome può essere `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="f42f4-266">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="f42f4-267">Il metodo di estensione seguente espone il middleware tramite <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="f42f4-267">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="f42f4-268">Il codice seguente chiama il middleware da `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f42f4-268">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f42f4-269">Il middleware deve seguire il [principio delle dipendenze esplicite](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) esponendo le dipendenze nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="f42f4-269">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="f42f4-270">Il middleware viene costruito una volta per ogni *durata applicazione*.</span><span class="sxs-lookup"><span data-stu-id="f42f4-270">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="f42f4-271">Se è necessario condividere servizi con il middleware all'interno di una richiesta, vedere la sezione [Dipendenze per richiesta](#per-request-dependencies).</span><span class="sxs-lookup"><span data-stu-id="f42f4-271">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="f42f4-272">I componenti middleware possono risolvere le dipendenze dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection) mediante i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="f42f4-272">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="f42f4-273">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) può anche accettare direttamente parametri aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="f42f4-273">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="f42f4-274">Dipendenze per richiesta</span><span class="sxs-lookup"><span data-stu-id="f42f4-274">Per-request dependencies</span></span>

<span data-ttu-id="f42f4-275">Poiché il middleware viene creato all'avvio dell'app e non per richiesta, i servizi di durata *con ambito* usati dai costruttori del middleware non vengono condivisi con altri tipi di inserimento di dipendenze durante ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="f42f4-275">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="f42f4-276">Se è necessario condividere un servizio *con ambito* tra il proprio middleware e altri tipi, aggiungere i servizi alla firma del metodo `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="f42f4-276">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="f42f4-277">Il metodo `Invoke` può accettare parametri aggiuntivi popolati dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="f42f4-277">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="f42f4-278">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f42f4-278">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
