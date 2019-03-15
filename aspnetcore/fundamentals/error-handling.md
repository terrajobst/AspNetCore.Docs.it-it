---
title: Gestire gli errori in ASP.NET Core
author: tdykstra
description: Informazioni su come gestire gli errori nelle app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665363"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="4eb4a-103">Gestire gli errori in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4eb4a-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="4eb4a-104">Di [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4eb4a-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4eb4a-105">Questo articolo descrive gli approcci comuni per la gestione degli errori nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="4eb4a-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4eb4a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="4eb4a-107">Pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="4eb4a-107">Developer Exception Page</span></span>

<span data-ttu-id="4eb4a-108">Per configurare un'app in modo da visualizzare una pagina contenente informazioni dettagliate sulle eccezioni delle richieste, usare la *pagina delle eccezioni per gli sviluppatori*.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-108">To configure an app to display a page that shows detailed information about request exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="4eb4a-109">La pagina è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4eb4a-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="4eb4a-110">Aggiungere una riga al metodo `Startup.Configure` quando l'app è in esecuzione nell'[ambiente](xref:fundamentals/environments) di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-110">Add a line to the `Startup.Configure` method when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

<span data-ttu-id="4eb4a-111">Posizionare la chiamata a <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> davanti al middleware in cui si vogliono rilevare le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="4eb4a-112">Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="4eb4a-113">per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="4eb4a-114">Per altre informazioni sulla configurazione di ambienti, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="4eb4a-115">Per visualizzare la pagina delle eccezioni per gli sviluppatori, eseguire l'app di esempio con l'ambiente impostato su `Development` e aggiungere `?throw=true` all'URL di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="4eb4a-116">La pagina include le informazioni seguenti sull'eccezione e sulla richiesta:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="4eb4a-117">Analisi dello stack</span><span class="sxs-lookup"><span data-stu-id="4eb4a-117">Stack trace</span></span>
* <span data-ttu-id="4eb4a-118">Parametri della stringa di query (se presenti)</span><span class="sxs-lookup"><span data-stu-id="4eb4a-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="4eb4a-119">Cookie (se presenti)</span><span class="sxs-lookup"><span data-stu-id="4eb4a-119">Cookies (if any)</span></span>
* <span data-ttu-id="4eb4a-120">Intestazioni</span><span class="sxs-lookup"><span data-stu-id="4eb4a-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="4eb4a-121">Configurare una pagina di gestione delle eccezioni personalizzata</span><span class="sxs-lookup"><span data-stu-id="4eb4a-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="4eb4a-122">Quando l'app non è in esecuzione nell'ambiente di sviluppo, chiamare il metodo di estensione <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> per aggiungere il middleware di gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="4eb4a-123">Il middleware:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-123">The middleware:</span></span>

* <span data-ttu-id="4eb4a-124">Intercetta le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-124">Catches exceptions.</span></span>
* <span data-ttu-id="4eb4a-125">Registra le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-125">Logs exceptions.</span></span>
* <span data-ttu-id="4eb4a-126">Esegue nuovamente la richiesta in una pipeline alternativa per la pagina o il controller indicati.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="4eb4a-127">La richiesta non viene eseguita nuovamente se la risposta è stata avviata.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="4eb4a-128">Nell'esempio seguente dall'app di esempio, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> aggiunge il middleware di gestione delle eccezioni in ambienti non di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="4eb4a-129">Il metodo di estensione specifica una pagina di errore o un controller nell'endpoint `/Error` per le richieste rieseguite dopo l'intercettazione e la registrazione delle eccezioni:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

<span data-ttu-id="4eb4a-130">Il modello di app Razor Pages fornisce una pagina di errore (*.cshtml*) e una classe <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) nella cartella Pages.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="4eb4a-131">In un'app MVC, il metodo del gestore errori seguente è incluso nel modello di app MVC e viene visualizzato nel controller Home:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="4eb4a-132">Non decorare il metodo dell'azione di gestione degli errori con attributi di metodo HTTP, ad esempio `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="4eb4a-133">I verbi espliciti impediscono ad alcune richieste di raggiungere il metodo.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="4eb4a-134">Consentire l'accesso anonimo al metodo in modo che gli utenti non autenticati possano ricevere la visualizzazione degli errori.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="access-the-exception"></a><span data-ttu-id="4eb4a-135">Accedere all'eccezione</span><span class="sxs-lookup"><span data-stu-id="4eb4a-135">Access the exception</span></span>

<span data-ttu-id="4eb4a-136">Usare <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> per accedere all'eccezione o al percorso della richiesta originale in un controller o in una pagina:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception or the original request path in a controller or page:</span></span>

* <span data-ttu-id="4eb4a-137">Il percorso è disponibile dalla proprietà <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path>.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-137">The path is available from the <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path> property.</span></span>
* <span data-ttu-id="4eb4a-138">Leggere <xref:System.Exception?displayProperty=fullName> dalla proprietà [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) ereditata.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-138">Read the <xref:System.Exception?displayProperty=fullName> from the inherited [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) property.</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> <span data-ttu-id="4eb4a-139">**Non** fornire informazioni sugli errori sensibili da <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> ai client.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-139">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="4eb4a-140">Fornire informazioni sugli errori rappresenta un rischio di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-140">Serving errors is a security risk.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="4eb4a-141">Configurare codice di gestione delle eccezioni personalizzato</span><span class="sxs-lookup"><span data-stu-id="4eb4a-141">Configure custom exception handling code</span></span>

<span data-ttu-id="4eb4a-142">Un'alternativa al fornire un endpoint per gli errori con una [pagina di gestione delle eccezioni personalizzata](#configure-a-custom-exception-handling-page) consiste nel fornire un funzione lambda a <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-142">An alternative to serving an endpoint for errors with a [custom exception handling page](#configure-a-custom-exception-handling-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="4eb4a-143">L'uso di una funzione lambda con <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> consente l'accesso all'errore prima di restituire la risposta.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-143">Using a lambda with <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> allows access to the error before returning the response.</span></span>

<span data-ttu-id="4eb4a-144">L'app di esempio illustra il codice di gestione delle eccezioni personalizzato in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-144">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="4eb4a-145">Generare un'eccezione tramite il collegamento **Throw Exception** (Genera eccezione) nella pagina Index.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-145">Trigger an exception with the **Throw Exception** link on the Index page.</span></span> <span data-ttu-id="4eb4a-146">Viene eseguita la funzione lambda seguente:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-146">The following lambda runs:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> <span data-ttu-id="4eb4a-147">**Non** fornire informazioni sugli errori sensibili da <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> ai client.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-147">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="4eb4a-148">Fornire informazioni sugli errori rappresenta un rischio di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-148">Serving errors is a security risk.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="4eb4a-149">Configurare le tabelle codici di stato</span><span class="sxs-lookup"><span data-stu-id="4eb4a-149">Configure status code pages</span></span>

<span data-ttu-id="4eb4a-150">Per impostazione predefinita, un'app ASP.NET Core non offre una tabella codici di stato per i codici di stato HTTP, ad esempio *404 - Non trovato*.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-150">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="4eb4a-151">L'app restituisce un codice di stato e un corpo della risposta vuoto.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-151">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="4eb4a-152">Per specificare le tabelle codici di stato, usare il middleware delle tabelle codici di stato.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-152">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="4eb4a-153">Il middleware è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4eb4a-153">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4eb4a-154">Aggiungere una riga al metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-154">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="4eb4a-155">Chiamare il metodo <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> prima del middleware di gestione delle richieste (ad esempio, middleware dei file statici e middleware MVC).</span><span class="sxs-lookup"><span data-stu-id="4eb4a-155">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="4eb4a-156">Per impostazione predefinita, il middleware delle pagine dei codici di stato aggiunge gestori di solo testo per i codici di stato comuni, ad esempio *404 - Non trovato*:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-156">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="4eb4a-157">Il middleware supporta diversi metodi di estensione che consentono di personalizzarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-157">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="4eb4a-158">Un overload di <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> accetta un'espressione lambda, che è possibile usare per elaborare la logica di gestione degli errori personalizzata e per scrivere manualmente la risposta:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-158">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="4eb4a-159">Un overload di <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> accetta un tipo di contenuto e una stringa di formato, utilizzabili per personalizzare il tipo di contenuto e il testo della risposta:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-159">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="4eb4a-160">Reindirizzare e rieseguire i metodi di estensione</span><span class="sxs-lookup"><span data-stu-id="4eb4a-160">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="4eb4a-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="4eb4a-162">Invia un codice di stato *302 - Trovato* al client.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="4eb4a-163">Reindirizza il client al percorso specificato nel modello di URL.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="4eb4a-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> viene in genere usato quando l'app:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="4eb4a-165">Deve reindirizzare il client a un endpoint diverso, in genere nei casi in cui un'altra app elabora l'errore.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-165">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="4eb4a-166">Per le app Web, la barra degli indirizzi del browser del client riflette l'endpoint reindirizzato.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-166">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="4eb4a-167">Non deve conservare e restituire il codice di stato originale con la risposta di reindirizzamento iniziale.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-167">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="4eb4a-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="4eb4a-169">Restituisce il codice di stato originale al client.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-169">Returns the original status code to the client.</span></span>
* <span data-ttu-id="4eb4a-170">Genera il corpo della risposta eseguendo nuovamente la pipeline delle richieste con un percorso alternativo.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-170">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="4eb4a-171"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> viene in genere usato quando l'app deve:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-171"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="4eb4a-172">Elaborare la richiesta senza il reindirizzamento a un endpoint diverso.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-172">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="4eb4a-173">Per le app Web, la barra degli indirizzi del browser del client riflette l'endpoint richiesto in origine.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-173">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="4eb4a-174">Conservare e restituire il codice di stato originale con la risposta.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-174">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="4eb4a-175">I modelli possono includere un segnaposto (`{0}`) per il codice di stato.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-175">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="4eb4a-176">Il modello deve iniziare con una barra rovesciata (`/`).</span><span class="sxs-lookup"><span data-stu-id="4eb4a-176">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="4eb4a-177">Quando si usa un segnaposto, verificare che l'endpoint (pagina o controller) possa elaborare il segmento del percorso.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-177">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="4eb4a-178">Ad esempio, una pagina Razor per gli errori deve accettare il valore del segmento di percorso facoltativo con la direttiva `@page`:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-178">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="4eb4a-179">Le tabelle codici di stato possono essere disabilitate per richieste specifiche in un metodo del gestore Razor Pages o in un controller MVC.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-179">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="4eb4a-180">Per disabilitare le tabelle codici di stato, provare a recuperare <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> dalla raccolta [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) della richiesta e disabilitare la funzionalità, se disponibile:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-180">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="4eb4a-181">Per usare un overload <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> che punta a un endpoint all'interno dell'app, creare una visualizzazione MVC o una pagina Razor per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-181">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="4eb4a-182">Ad esempio, il modello di app Razor Pages produce la pagina e la classe di modelli di pagina seguenti:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-182">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="4eb4a-183">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-183">*Error.cshtml*:</span></span>

::: moniker range=">= aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

<span data-ttu-id="4eb4a-184">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-184">*Error.cshtml.cs*:</span></span>

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="4eb4a-185">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-185">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a><span data-ttu-id="4eb4a-186">Codice di gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="4eb4a-186">Exception-handling code</span></span>

<span data-ttu-id="4eb4a-187">Il codice in pagine di gestione delle eccezioni può generare eccezioni.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-187">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="4eb4a-188">È spesso consigliabile che le pagine di errore di produzione contengano esclusivamente contenuto statico.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-188">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="4eb4a-189">Inoltre, tenere presente che dopo l'invio delle intestazioni di risposta:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-189">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="4eb4a-190">L'app non può modificare il codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-190">The app can't change the response's status code.</span></span>
* <span data-ttu-id="4eb4a-191">Non è possibile eseguire pagine o gestori delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-191">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="4eb4a-192">La risposta deve essere completata o la connessione deve essere interrotta.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-192">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="4eb4a-193">Gestione delle eccezioni del server</span><span class="sxs-lookup"><span data-stu-id="4eb4a-193">Server exception handling</span></span>

<span data-ttu-id="4eb4a-194">Oltre alla logica di gestione delle eccezioni nell'app, l'[implementazione del server](xref:fundamentals/servers/index) può gestire alcune eccezioni.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-194">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="4eb4a-195">Se rileva un'eccezione prima dell'invio delle intestazioni di risposta, il server invia una risposta *500 - Errore interno del server* senza corpo.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-195">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="4eb4a-196">Se il server rileva un'eccezione dopo l'invio delle intestazioni di risposta, il server chiude la connessione.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-196">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="4eb4a-197">Le richieste che non sono gestite dall'app vengono gestite dal server.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-197">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="4eb4a-198">Qualsiasi eccezione che si verifica quando il server sta gestendo la richiesta viene gestita dalla gestione delle eccezioni del server.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-198">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="4eb4a-199">Eventuali pagine di errore personalizzate dell'app, middleware di gestione delle eccezioni e filtri non hanno effetto su questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-199">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="4eb4a-200">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="4eb4a-200">Startup exception handling</span></span>

<span data-ttu-id="4eb4a-201">Solo il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-201">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="4eb4a-202">Con [Host Web](xref:fundamentals/host/web-host) è possibile [configurare il comportamento dell'host in risposta agli errori durante l'avvio](xref:fundamentals/host/web-host#detailed-errors) usando `captureStartupErrors` e le chiavi `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-202">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="4eb4a-203">L'hosting può visualizzare solo una pagina di errore per un errore di avvio acquisito se l'errore si verifica dopo l'associazione indirizzo host/porta.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-203">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="4eb4a-204">Se qualsiasi associazione non riesce per qualsiasi motivo:</span><span class="sxs-lookup"><span data-stu-id="4eb4a-204">If any binding fails for any reason:</span></span>

* <span data-ttu-id="4eb4a-205">Il livello di hosting registra un'eccezione critica.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-205">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="4eb4a-206">Si verifica un arresto anomalo del processo di dotnet.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-206">The dotnet process crashes.</span></span>
* <span data-ttu-id="4eb4a-207">Non viene visualizzata alcuna pagina di errore quando l'app è in esecuzione nel server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="4eb4a-207">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="4eb4a-208">Se durante l'esecuzione su [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) il processo non può essere avviato, viene restituito un codice *502.5 Errore del processo* dal [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4eb4a-208">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="4eb4a-209">Per ulteriori informazioni, vedere <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-209">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="4eb4a-210">Per informazioni sulla risoluzione dei problemi di avvio con il servizio app di Azure, vedere <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-210">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="4eb4a-211">Gestione degli errori di ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="4eb4a-211">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="4eb4a-212">Le app [MVC](xref:mvc/overview) includono alcune opzioni aggiuntive per la gestione degli errori, ad esempio la configurazione di filtri delle eccezioni e l'esecuzione della convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-212">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="4eb4a-213">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="4eb4a-213">Exception filters</span></span>

<span data-ttu-id="4eb4a-214">I filtri delle eccezioni possono essere configurati a livello globale o per singolo controller o azione in un'app MVC.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-214">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="4eb4a-215">Questi filtri gestiscono tutte le eccezioni non gestite che si verificano durante l'esecuzione di un'azione del controller o di un altro filtro</span><span class="sxs-lookup"><span data-stu-id="4eb4a-215">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="4eb4a-216">e non vengono chiamati in altro modo.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-216">These filters aren't called otherwise.</span></span> <span data-ttu-id="4eb4a-217">Per ulteriori informazioni, vedere <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-217">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="4eb4a-218">I filtri delle eccezioni sono utili per intercettare le eccezioni che si verificano all'interno di azioni MVC, ma non sono flessibili come il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-218">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="4eb4a-219">È consigliabile usare il middleware.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-219">We recommend using the middleware.</span></span> <span data-ttu-id="4eb4a-220">Usare i filtri solo quando è necessario eseguire la gestione degli errori *in modo diverso* in base all'azione MVC scelta.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-220">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="4eb4a-221">Gestire gli errori di stato del modello</span><span class="sxs-lookup"><span data-stu-id="4eb4a-221">Handle model state errors</span></span>

<span data-ttu-id="4eb4a-222">La [convalida del modello](xref:mvc/models/validation) avviene prima di richiamare ogni azione del controller ed è responsabilità del metodo di azione controllare [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) e rispondere in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-222">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="4eb4a-223">Alcune app scelgono di seguire una convenzione standard per gestire gli errori di [convalida del modello](xref:mvc/models/validation). In tal caso un [filtro](xref:mvc/controllers/filters) può essere l'elemento adatto in cui implementare i criteri.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-223">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="4eb4a-224">È consigliabile testare il comportamento delle azioni con gli stati del modello non validi.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-224">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="4eb4a-225">Per ulteriori informazioni, vedere <xref:mvc/controllers/testing>.</span><span class="sxs-lookup"><span data-stu-id="4eb4a-225">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4eb4a-226">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4eb4a-226">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
