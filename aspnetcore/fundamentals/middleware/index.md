---
title: Middleware di ASP.NET Core
author: rick-anderson
description: Informazioni sul middelware di ASP.NET Core e la pipeline delle richieste.
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: bac121441d6856ca79affe1a3130e5cbc76debd9
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665389"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="6019b-103">Middleware di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6019b-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="6019b-104">[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6019b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6019b-105">Il middleware è un software che viene assemblato in una pipeline dell'app per gestire richieste e risposte.</span><span class="sxs-lookup"><span data-stu-id="6019b-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="6019b-106">Ogni componente:</span><span class="sxs-lookup"><span data-stu-id="6019b-106">Each component:</span></span>

* <span data-ttu-id="6019b-107">Sceglie se passare la richiesta al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="6019b-108">Può eseguire le operazioni prima e dopo il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="6019b-109">Per compilare la pipeline delle richieste vengono usati i delegati di richiesta.</span><span class="sxs-lookup"><span data-stu-id="6019b-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="6019b-110">I delegati di richiesta gestiscono ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6019b-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="6019b-111">I delegati di richiesta vengono configurati tramite i metodi di estensione <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> e <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="6019b-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="6019b-112">È possibile specificare un singolo delegato di richiesta inline come metodo anonimo (chiamato middleware inline) o definirlo in una classe riutilizzabile.</span><span class="sxs-lookup"><span data-stu-id="6019b-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="6019b-113">Le classi riutilizzabili o i metodi anonimi inline costituiscono il *middleware* o sono noti anche come *componenti middleware*.</span><span class="sxs-lookup"><span data-stu-id="6019b-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="6019b-114">Ogni componente middleware è responsabile della chiamata del componente seguente nella pipeline o del corto circuito della pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="6019b-115">Quando un middleware esegue un corto circuito, viene denominato *middleware terminale* perché impedisce all'ulteriore middleware di elaborare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="6019b-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="6019b-116">In <xref:migration/http-modules> sono descritte le differenze tra le pipeline delle richieste in ASP.NET Core e in ASP.NET 4.x e sono riportati altri esempi di middleware.</span><span class="sxs-lookup"><span data-stu-id="6019b-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="6019b-117">Creare una pipeline middleware con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="6019b-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="6019b-118">La pipeline delle richieste ASP.NET Core è costituita da una sequenza di delegati di richiesta, chiamati uno dopo l'altro.</span><span class="sxs-lookup"><span data-stu-id="6019b-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="6019b-119">Il diagramma seguente illustra il concetto.</span><span class="sxs-lookup"><span data-stu-id="6019b-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="6019b-120">Il thread di esecuzione seguente le frecce nere.</span><span class="sxs-lookup"><span data-stu-id="6019b-120">The thread of execution follows the black arrows.</span></span>

![Modello di elaborazione delle richieste che visualizza una richiesta in arrivo, l'elaborazione tramite tre middleware e la risposta che esce dall'app.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="6019b-124">I delegati possono eseguire le operazioni prima del delegato successivo e dopo di esso.</span><span class="sxs-lookup"><span data-stu-id="6019b-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="6019b-125">I delegati che gestiscono le eccezioni devono essere chiamati nella prima parte della pipeline in modo che possano individuare le eccezioni che si verificano nelle parti successive della pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="6019b-126">L'app di ASP.NET Core più semplice imposta un delegato di richiesta singolo che gestisce tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="6019b-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="6019b-127">In questo caso non è inclusa una pipeline di richieste effettiva.</span><span class="sxs-lookup"><span data-stu-id="6019b-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="6019b-128">Al contrario, viene chiamata una singola funzione anonima in risposta a ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6019b-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="6019b-129">Il primo delegato <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> termina la pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-129">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="6019b-130">È possibile concatenare più delegati di richiesta insieme con <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="6019b-130">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="6019b-131">Il parametro `next` rappresenta il delegato successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-131">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="6019b-132">È possibile eseguire il corto circuito della pipeline *non* chiamando il parametro *next*.</span><span class="sxs-lookup"><span data-stu-id="6019b-132">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="6019b-133">In genere è possibile eseguire un'azione prima e dopo il delegato successivo, come illustra l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="6019b-133">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

<span data-ttu-id="6019b-134">Quando un delegato non passa una richiesta al delegato successivo, si crea un cosiddetto *corto circuito della pipeline delle richieste*.</span><span class="sxs-lookup"><span data-stu-id="6019b-134">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="6019b-135">Il corto circuito è spesso opportuno poiché evita l'esecuzione di operazioni non necessarie.</span><span class="sxs-lookup"><span data-stu-id="6019b-135">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="6019b-136">Ad esempio, il [middleware dei file statici](xref:fundamentals/static-files) può operare come *middleware terminale* elaborando una richiesta di un file statico ed effettuando il corto circuito della pipeline rimanente.</span><span class="sxs-lookup"><span data-stu-id="6019b-136">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="6019b-137">Il middleware aggiunto alla pipeline prima del middleware che termina l'ulteriore elaborazione elabora comunque il codice dopo le istruzioni `next.Invoke`.</span><span class="sxs-lookup"><span data-stu-id="6019b-137">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="6019b-138">Vedere comunque l'avviso seguente sul tentativo di scrivere una risposta che è già stata inviata.</span><span class="sxs-lookup"><span data-stu-id="6019b-138">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="6019b-139">Non chiamare `next.Invoke` dopo aver inviato la risposta al client.</span><span class="sxs-lookup"><span data-stu-id="6019b-139">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="6019b-140">Le modifiche apportate a <xref:Microsoft.AspNetCore.Http.HttpResponse> dopo l'avvio della risposta generano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="6019b-140">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="6019b-141">Ad esempio, le modifiche come l'impostazione delle intestazioni e di un codice di stato generano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="6019b-141">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="6019b-142">Scrivere nel corpo della risposta dopo aver chiamato `next`:</span><span class="sxs-lookup"><span data-stu-id="6019b-142">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="6019b-143">Può causare una violazione del protocollo.</span><span class="sxs-lookup"><span data-stu-id="6019b-143">May cause a protocol violation.</span></span> <span data-ttu-id="6019b-144">Ad esempio, scrivere un contenuto che supera il valore `Content-Length` specificato.</span><span class="sxs-lookup"><span data-stu-id="6019b-144">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="6019b-145">Può danneggiare il formato del corpo.</span><span class="sxs-lookup"><span data-stu-id="6019b-145">May corrupt the body format.</span></span> <span data-ttu-id="6019b-146">Ad esempio, scrivere un piè di pagina HTML in un file CSS.</span><span class="sxs-lookup"><span data-stu-id="6019b-146">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="6019b-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> è un hint utile per indicare se le intestazioni sono state inviate o se è stato scritto contenuto nel corpo.</span><span class="sxs-lookup"><span data-stu-id="6019b-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="6019b-148">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="6019b-148">Order</span></span>

<span data-ttu-id="6019b-149">L'ordine in cui vengono aggiunti i componenti middleware nel metodo `Startup.Configure` definisce l'ordine in cui i componenti middleware vengono richiamati per le richieste e l'ordine inverso per la risposta.</span><span class="sxs-lookup"><span data-stu-id="6019b-149">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="6019b-150">Questo ordinamento è fondamentale per la sicurezza, le prestazioni e la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="6019b-150">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="6019b-151">Il metodo `Startup.Configure` seguente aggiunge componenti del middleware per gli scenari di app comuni:</span><span class="sxs-lookup"><span data-stu-id="6019b-151">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="6019b-152">Gestione errori/eccezioni</span><span class="sxs-lookup"><span data-stu-id="6019b-152">Exception/error handling</span></span>
1. <span data-ttu-id="6019b-153">Protocollo HTTP Strict Transport Security</span><span class="sxs-lookup"><span data-stu-id="6019b-153">HTTP Strict Transport Security Protocol</span></span>
1. <span data-ttu-id="6019b-154">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="6019b-154">HTTPS redirection</span></span>
1. <span data-ttu-id="6019b-155">File server statico</span><span class="sxs-lookup"><span data-stu-id="6019b-155">Static file server</span></span>
1. <span data-ttu-id="6019b-156">Applicazione dei criteri per i cookie</span><span class="sxs-lookup"><span data-stu-id="6019b-156">Cookie policy enforcement</span></span>
1. <span data-ttu-id="6019b-157">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="6019b-157">Authentication</span></span>
1. <span data-ttu-id="6019b-158">Sessione</span><span class="sxs-lookup"><span data-stu-id="6019b-158">Session</span></span>
1. <span data-ttu-id="6019b-159">MVC</span><span class="sxs-lookup"><span data-stu-id="6019b-159">MVC</span></span>

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
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. <span data-ttu-id="6019b-160">Gestione errori/eccezioni</span><span class="sxs-lookup"><span data-stu-id="6019b-160">Exception/error handling</span></span>
1. <span data-ttu-id="6019b-161">File statici</span><span class="sxs-lookup"><span data-stu-id="6019b-161">Static files</span></span>
1. <span data-ttu-id="6019b-162">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="6019b-162">Authentication</span></span>
1. <span data-ttu-id="6019b-163">Sessione</span><span class="sxs-lookup"><span data-stu-id="6019b-163">Session</span></span>
1. <span data-ttu-id="6019b-164">MVC</span><span class="sxs-lookup"><span data-stu-id="6019b-164">MVC</span></span>

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

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="6019b-165">Nell'esempio di codice precedente, ogni metodo di estensione del middleware viene esposto in <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> tramite lo spazio dei nomi <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="6019b-165">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="6019b-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> è il primo componente middleware aggiunto alla pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="6019b-167">Pertanto, il middleware del gestore di eccezioni intercetta le eccezioni che si verificano nelle chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="6019b-167">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="6019b-168">Il middleware dei file statici viene chiamato nella prima parte della pipeline in moda che possa gestire le richieste ed eseguire un corto circuito senza passare attraverso i componenti rimanenti.</span><span class="sxs-lookup"><span data-stu-id="6019b-168">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="6019b-169">Il middleware dei file statici **non** offre controlli di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="6019b-169">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="6019b-170">I file serviti dal sever, inclusi i file in *wwwroot*, sono disponibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="6019b-170">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="6019b-171">Vedere <xref:fundamentals/static-files> per un approccio alla protezione dei file statici.</span><span class="sxs-lookup"><span data-stu-id="6019b-171">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6019b-172">Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware di autenticazione (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="6019b-172">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="6019b-173">L'autenticazione non esegue il corto circuito di richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="6019b-173">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="6019b-174">Sebbene il middleware di autenticazione esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona una pagina Razor o un controller specifico e un'azione.</span><span class="sxs-lookup"><span data-stu-id="6019b-174">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6019b-175">Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware dell'identità (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>) che esegue l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="6019b-175">If the request isn't handled by Static File Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="6019b-176">Identity non esegue il corto circuito di richieste non autenticate.</span><span class="sxs-lookup"><span data-stu-id="6019b-176">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="6019b-177">Sebbene Identity esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona un controller specifico e un'azione.</span><span class="sxs-lookup"><span data-stu-id="6019b-177">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="6019b-178">L'esempio seguente illustra un ordinamento del middleware nel quale le richieste dei file statici vengono gestite dal middleware dei file statici prima del middleware di compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="6019b-178">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="6019b-179">I file statici non vengono compressi con questo ordine di middleware.</span><span class="sxs-lookup"><span data-stu-id="6019b-179">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="6019b-180">Le risposte MVC da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> possono essere compresse.</span><span class="sxs-lookup"><span data-stu-id="6019b-180">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="6019b-181">Use, Run e Map</span><span class="sxs-lookup"><span data-stu-id="6019b-181">Use, Run, and Map</span></span>

<span data-ttu-id="6019b-182">Configurare la pipeline HTTP usando `Use`,`Run` e `Map`.</span><span class="sxs-lookup"><span data-stu-id="6019b-182">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="6019b-183">Il metodo `Use` può eseguire il corto circuito della pipeline, ovvero non chiamare un delegato di richiesta `next`.</span><span class="sxs-lookup"><span data-stu-id="6019b-183">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="6019b-184">`Run` è una convenzione e alcuni componenti del middleware possono esporre metodi `Run[Middleware]` che vengono eseguiti al termine della pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-184">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="6019b-185">Le estensioni <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> vengono usate come convenzione per la diramazione della pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-185"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="6019b-186">`Map*` crea un ramo nella pipeline delle richieste in base alle corrispondenze del percorso della richiesta specificato.</span><span class="sxs-lookup"><span data-stu-id="6019b-186">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="6019b-187">Se il percorso della richiesta inizia con il percorso specificato, il ramo viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="6019b-187">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="6019b-188">La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente.</span><span class="sxs-lookup"><span data-stu-id="6019b-188">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="6019b-189">Richiesta</span><span class="sxs-lookup"><span data-stu-id="6019b-189">Request</span></span>             | <span data-ttu-id="6019b-190">Risposta</span><span class="sxs-lookup"><span data-stu-id="6019b-190">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="6019b-191">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="6019b-191">localhost:1234</span></span>      | <span data-ttu-id="6019b-192">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="6019b-192">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="6019b-193">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="6019b-193">localhost:1234/map1</span></span> | <span data-ttu-id="6019b-194">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="6019b-194">Map Test 1</span></span>                   |
| <span data-ttu-id="6019b-195">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="6019b-195">localhost:1234/map2</span></span> | <span data-ttu-id="6019b-196">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="6019b-196">Map Test 2</span></span>                   |
| <span data-ttu-id="6019b-197">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="6019b-197">localhost:1234/map3</span></span> | <span data-ttu-id="6019b-198">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="6019b-198">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="6019b-199">Quando si usa `Map`, i segmenti di percorso corrispondenti vengono rimossi da `HttpRequest.Path` e aggiunti a `HttpRequest.PathBase` per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6019b-199">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="6019b-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crea un ramo nella pipeline delle richieste in base al risultato del predicato specificato.</span><span class="sxs-lookup"><span data-stu-id="6019b-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="6019b-201">È possibile usare qualsiasi predicato di tipo `Func<HttpContext, bool>` per mappare le richieste a un nuovo ramo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="6019b-201">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="6019b-202">Nell'esempio seguente viene usato un predicato per rilevare la presenza di una variabile di stringa di query `branch`:</span><span class="sxs-lookup"><span data-stu-id="6019b-202">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="6019b-203">La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente.</span><span class="sxs-lookup"><span data-stu-id="6019b-203">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="6019b-204">Richiesta</span><span class="sxs-lookup"><span data-stu-id="6019b-204">Request</span></span>                       | <span data-ttu-id="6019b-205">Risposta</span><span class="sxs-lookup"><span data-stu-id="6019b-205">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="6019b-206">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="6019b-206">localhost:1234</span></span>                | <span data-ttu-id="6019b-207">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="6019b-207">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="6019b-208">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="6019b-208">localhost:1234/?branch=master</span></span> | <span data-ttu-id="6019b-209">Ramo usato = master</span><span class="sxs-lookup"><span data-stu-id="6019b-209">Branch used = master</span></span>         |

<span data-ttu-id="6019b-210">`Map` supporta l'annidamento, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6019b-210">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="6019b-211">`Map` può anche trovare la corrispondenza di più segmenti contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="6019b-211">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="6019b-212">Middleware incorporato</span><span class="sxs-lookup"><span data-stu-id="6019b-212">Built-in middleware</span></span>

<span data-ttu-id="6019b-213">ASP.NET Core include i componenti middleware seguenti.</span><span class="sxs-lookup"><span data-stu-id="6019b-213">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="6019b-214">Nella colonna *Ordinamento* sono disponibili note sul posizionamento del middleware nella pipeline di elaborazione delle richieste e indicazioni sulle condizioni nelle quali il middleware può terminare l'elaborazione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="6019b-214">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="6019b-215">Quando un middleware esegue un corto circuito della pipeline di elaborazione delle richieste e impedisce al middleware a valle di elaborare una richiesta, viene denominato *middleware terminale*.</span><span class="sxs-lookup"><span data-stu-id="6019b-215">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="6019b-216">Per altre informazioni sul corto circuito, vedere la sezione [Creare una pipeline middleware con IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="6019b-216">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="6019b-217">Middleware</span><span class="sxs-lookup"><span data-stu-id="6019b-217">Middleware</span></span> | <span data-ttu-id="6019b-218">Description</span><span class="sxs-lookup"><span data-stu-id="6019b-218">Description</span></span> | <span data-ttu-id="6019b-219">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="6019b-219">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="6019b-220">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="6019b-220">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="6019b-221">Offre il supporto dell'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="6019b-221">Provides authentication support.</span></span> | <span data-ttu-id="6019b-222">Prima di `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="6019b-222">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="6019b-223">Terminale per i callback OAuth.</span><span class="sxs-lookup"><span data-stu-id="6019b-223">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="6019b-224">Criteri per i cookie</span><span class="sxs-lookup"><span data-stu-id="6019b-224">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="6019b-225">Registra il consenso degli utenti per l'archiviazione delle informazioni personali e applica gli standard minimi per i campi dei cookie, come `secure` e `SameSite`.</span><span class="sxs-lookup"><span data-stu-id="6019b-225">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="6019b-226">Prima del middleware che emette i cookie.</span><span class="sxs-lookup"><span data-stu-id="6019b-226">Before middleware that issues cookies.</span></span> <span data-ttu-id="6019b-227">Esempi: Autenticazione, Sessione, MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="6019b-227">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="6019b-228">CORS</span><span class="sxs-lookup"><span data-stu-id="6019b-228">CORS</span></span>](xref:security/cors) | <span data-ttu-id="6019b-229">Configura la condivisione di risorse tra le origini (CORS).</span><span class="sxs-lookup"><span data-stu-id="6019b-229">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="6019b-230">Prima dei componenti che usano CORS.</span><span class="sxs-lookup"><span data-stu-id="6019b-230">Before components that use CORS.</span></span> |
| [<span data-ttu-id="6019b-231">Gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="6019b-231">Exception Handling</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="6019b-232">Gestisce le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="6019b-232">Handles exceptions.</span></span> | <span data-ttu-id="6019b-233">Prima dei componenti che generano errori.</span><span class="sxs-lookup"><span data-stu-id="6019b-233">Before components that generate errors.</span></span> |
| [<span data-ttu-id="6019b-234">Intestazioni inoltrate</span><span class="sxs-lookup"><span data-stu-id="6019b-234">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="6019b-235">Inoltra le intestazioni proxy nella richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="6019b-235">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="6019b-236">Prima dei componenti che usano i campi aggiornati.</span><span class="sxs-lookup"><span data-stu-id="6019b-236">Before components that consume the updated fields.</span></span> <span data-ttu-id="6019b-237">Esempi: schema, host, IP client, metodo.</span><span class="sxs-lookup"><span data-stu-id="6019b-237">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="6019b-238">Controllo integrità</span><span class="sxs-lookup"><span data-stu-id="6019b-238">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="6019b-239">Controlla l'integrità di un'app ASP.NET Core e le relative dipendenze, come il controllo della disponibilità del database.</span><span class="sxs-lookup"><span data-stu-id="6019b-239">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="6019b-240">Terminale se una richiesta corrisponde a un endpoint di controllo di integrità.</span><span class="sxs-lookup"><span data-stu-id="6019b-240">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="6019b-241">Override del metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="6019b-241">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="6019b-242">Consente a una richiesta POST in arrivo di eseguire l'override del metodo.</span><span class="sxs-lookup"><span data-stu-id="6019b-242">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="6019b-243">Prima dei componenti che usano il metodo aggiornato.</span><span class="sxs-lookup"><span data-stu-id="6019b-243">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="6019b-244">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="6019b-244">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="6019b-245">Reindirizzare tutte le richieste HTTP a HTTPS (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="6019b-245">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="6019b-246">Prima dei componenti che usano l'URL.</span><span class="sxs-lookup"><span data-stu-id="6019b-246">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="6019b-247">Protocollo HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="6019b-247">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="6019b-248">Middleware di ottimizzazione della sicurezza che aggiunge un'intestazione della risposta speciale (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="6019b-248">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="6019b-249">Prima dell'invio delle risposte e dopo i componenti che modificano le richieste.</span><span class="sxs-lookup"><span data-stu-id="6019b-249">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="6019b-250">Esempi: intestazioni inoltrate, riscrittura dell'URL.</span><span class="sxs-lookup"><span data-stu-id="6019b-250">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="6019b-251">MVC</span><span class="sxs-lookup"><span data-stu-id="6019b-251">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="6019b-252">Elabora le richieste con MVC/Razor Pages (ASP.NET Core 2.0 o versione successiva).</span><span class="sxs-lookup"><span data-stu-id="6019b-252">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="6019b-253">Terminale se una richiesta corrisponde a una route.</span><span class="sxs-lookup"><span data-stu-id="6019b-253">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="6019b-254">OWIN</span><span class="sxs-lookup"><span data-stu-id="6019b-254">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="6019b-255">Interoperabilità con app, server e middleware basati su OWIN.</span><span class="sxs-lookup"><span data-stu-id="6019b-255">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="6019b-256">Terminale se la richiesta viene elaborata completamente dal middleware OWIN.</span><span class="sxs-lookup"><span data-stu-id="6019b-256">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="6019b-257">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="6019b-257">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="6019b-258">Offre il supporto per la memorizzazione delle risposte nella cache.</span><span class="sxs-lookup"><span data-stu-id="6019b-258">Provides support for caching responses.</span></span> | <span data-ttu-id="6019b-259">Prima dei componenti che richiedono la memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="6019b-259">Before components that require caching.</span></span> |
| [<span data-ttu-id="6019b-260">Compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="6019b-260">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="6019b-261">Offre il supporto per la compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="6019b-261">Provides support for compressing responses.</span></span> | <span data-ttu-id="6019b-262">Prima dei componenti che richiedono la compressione.</span><span class="sxs-lookup"><span data-stu-id="6019b-262">Before components that require compression.</span></span> |
| [<span data-ttu-id="6019b-263">Localizzazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="6019b-263">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="6019b-264">Offre il supporto per la localizzazione.</span><span class="sxs-lookup"><span data-stu-id="6019b-264">Provides localization support.</span></span> | <span data-ttu-id="6019b-265">Prima dei componenti sensibili alla localizzazione.</span><span class="sxs-lookup"><span data-stu-id="6019b-265">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="6019b-266">Routing</span><span class="sxs-lookup"><span data-stu-id="6019b-266">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="6019b-267">Definisce e vincola le route di richiesta.</span><span class="sxs-lookup"><span data-stu-id="6019b-267">Defines and constrains request routes.</span></span> | <span data-ttu-id="6019b-268">Terminale per le route corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="6019b-268">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="6019b-269">Sessione</span><span class="sxs-lookup"><span data-stu-id="6019b-269">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="6019b-270">Offre il supporto per la gestione delle sessioni utente.</span><span class="sxs-lookup"><span data-stu-id="6019b-270">Provides support for managing user sessions.</span></span> | <span data-ttu-id="6019b-271">Prima dei componenti che richiedono la Sessione.</span><span class="sxs-lookup"><span data-stu-id="6019b-271">Before components that require Session.</span></span> |
| [<span data-ttu-id="6019b-272">File statici</span><span class="sxs-lookup"><span data-stu-id="6019b-272">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="6019b-273">Offre il supporto per la gestione di file statici e l'esplorazione directory.</span><span class="sxs-lookup"><span data-stu-id="6019b-273">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="6019b-274">Terminale se una richiesta corrisponde a un file.</span><span class="sxs-lookup"><span data-stu-id="6019b-274">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="6019b-275">Riscrittura dell'URL</span><span class="sxs-lookup"><span data-stu-id="6019b-275">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="6019b-276">Offre il supporto per la riscrittura degli URL e il reindirizzamento delle richieste.</span><span class="sxs-lookup"><span data-stu-id="6019b-276">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="6019b-277">Prima dei componenti che usano l'URL.</span><span class="sxs-lookup"><span data-stu-id="6019b-277">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="6019b-278">Oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="6019b-278">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="6019b-279">Abilita il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6019b-279">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="6019b-280">Prima dei componenti necessari per accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6019b-280">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="6019b-281">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6019b-281">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
