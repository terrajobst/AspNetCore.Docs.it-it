---
title: Gestire gli errori in ASP.NET Core
author: rick-anderson
description: Informazioni su come gestire gli errori nelle app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: fundamentals/error-handling
ms.openlocfilehash: bff526e196ecc378d4687e1c38188977aeeccfd9
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589880"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="54b88-103">Gestire gli errori in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54b88-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="54b88-104">Di [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="54b88-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="54b88-105">Questo articolo illustra gli approcci comuni per la gestione degli errori nelle app Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54b88-105">This article covers common approaches to handling errors in ASP.NET Core web apps.</span></span> <span data-ttu-id="54b88-106">Vedere <xref:web-api/handle-errors> per le API Web.</span><span class="sxs-lookup"><span data-stu-id="54b88-106">See <xref:web-api/handle-errors> for web APIs.</span></span>

<span data-ttu-id="54b88-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="54b88-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span> <span data-ttu-id="54b88-108">([Come scaricare](xref:index#how-to-download-a-sample)). L'articolo include istruzioni su come impostare le direttive per il preprocessore (`#if`, `#endif`, `#define`) nell'app di esempio per abilitare diversi scenari.</span><span class="sxs-lookup"><span data-stu-id="54b88-108">([How to download](xref:index#how-to-download-a-sample).) The article includes instructions about how to set preprocessor directives (`#if`, `#endif`, `#define`) in the sample app to enable different scenarios.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="54b88-109">Pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="54b88-109">Developer Exception Page</span></span>

<span data-ttu-id="54b88-110">In *Pagina delle eccezioni per gli sviluppatori* sono disponibili informazioni dettagliate sulle eccezioni delle richieste.</span><span class="sxs-lookup"><span data-stu-id="54b88-110">The *Developer Exception Page* displays detailed information about request exceptions.</span></span> <span data-ttu-id="54b88-111">La pagina è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="54b88-111">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="54b88-112">Aggiungere codice al metodo `Startup.Configure` per abilitare la pagina quando l'app è in esecuzione nell'[ambiente](xref:fundamentals/environments) di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="54b88-112">Add code to the `Startup.Configure` method to enable the page when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

<span data-ttu-id="54b88-113">Posizionare la chiamata a <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> prima di qualsiasi middleware in cui si vogliono rilevare le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="54b88-113">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> before any middleware that you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="54b88-114">Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="54b88-114">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="54b88-115">per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="54b88-115">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="54b88-116">Per altre informazioni sulla configurazione di ambienti, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="54b88-116">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="54b88-117">La pagina include le informazioni seguenti sull'eccezione e sulla richiesta:</span><span class="sxs-lookup"><span data-stu-id="54b88-117">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="54b88-118">Analisi dello stack</span><span class="sxs-lookup"><span data-stu-id="54b88-118">Stack trace</span></span>
* <span data-ttu-id="54b88-119">Parametri della stringa di query (se presenti)</span><span class="sxs-lookup"><span data-stu-id="54b88-119">Query string parameters (if any)</span></span>
* <span data-ttu-id="54b88-120">Cookie (se presenti)</span><span class="sxs-lookup"><span data-stu-id="54b88-120">Cookies (if any)</span></span>
* <span data-ttu-id="54b88-121">Intestazioni</span><span class="sxs-lookup"><span data-stu-id="54b88-121">Headers</span></span>

<span data-ttu-id="54b88-122">Per visualizzare la pagina delle eccezioni per gli sviluppatori nell'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), usare la direttiva del preprocessore `DevEnvironment` e selezionare **Trigger an exception** (Attiva un'eccezione) nella home page.</span><span class="sxs-lookup"><span data-stu-id="54b88-122">To see the Developer Exception Page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `DevEnvironment` preprocessor directive and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-page"></a><span data-ttu-id="54b88-123">Pagina del gestore di eccezioni</span><span class="sxs-lookup"><span data-stu-id="54b88-123">Exception handler page</span></span>

<span data-ttu-id="54b88-124">Per configurare una pagina di gestione degli errori personalizzata per l'ambiente di produzione, usare il middleware di gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="54b88-124">To configure a custom error handling page for the Production environment, use the Exception Handling Middleware.</span></span> <span data-ttu-id="54b88-125">Il middleware:</span><span class="sxs-lookup"><span data-stu-id="54b88-125">The middleware:</span></span>

* <span data-ttu-id="54b88-126">Intercetta e registra le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="54b88-126">Catches and logs exceptions.</span></span>
* <span data-ttu-id="54b88-127">Esegue nuovamente la richiesta in una pipeline alternativa per la pagina o il controller indicati.</span><span class="sxs-lookup"><span data-stu-id="54b88-127">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="54b88-128">La richiesta non viene eseguita nuovamente se la risposta è stata avviata.</span><span class="sxs-lookup"><span data-stu-id="54b88-128">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="54b88-129">Nell'esempio seguente <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> aggiunge il middleware di gestione delle eccezioni in ambienti non di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="54b88-129">In the following example, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

<span data-ttu-id="54b88-130">Il modello di app Razor Pages fornisce una pagina di errore ( *.cshtml*) e una classe <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="54b88-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the *Pages* folder.</span></span> <span data-ttu-id="54b88-131">Per un'app MVC, il modello di progetto include un metodo di azione Error e una visualizzazione degli errori.</span><span class="sxs-lookup"><span data-stu-id="54b88-131">For an MVC app, the project template includes an Error action method and an Error view.</span></span> <span data-ttu-id="54b88-132">Ecco il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="54b88-132">Here's the action method:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="54b88-133">Non decorare il metodo dell'azione di gestione degli errori con attributi di metodo HTTP, ad esempio `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="54b88-133">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="54b88-134">I verbi espliciti impediscono ad alcune richieste di raggiungere il metodo.</span><span class="sxs-lookup"><span data-stu-id="54b88-134">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="54b88-135">Consentire l'accesso anonimo al metodo in modo che gli utenti non autenticati possano ricevere la visualizzazione degli errori.</span><span class="sxs-lookup"><span data-stu-id="54b88-135">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

### <a name="access-the-exception"></a><span data-ttu-id="54b88-136">Accedere all'eccezione</span><span class="sxs-lookup"><span data-stu-id="54b88-136">Access the exception</span></span>

<span data-ttu-id="54b88-137">Usare <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> per accedere all'eccezione e al percorso della richiesta originale in un controller o in una pagina del gestore degli errori:</span><span class="sxs-lookup"><span data-stu-id="54b88-137">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception and the original request path in an error handler controller or page:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> <span data-ttu-id="54b88-138">**Non** fornire informazioni sugli errori sensibili ai client.</span><span class="sxs-lookup"><span data-stu-id="54b88-138">Do **not** serve sensitive error information to clients.</span></span> <span data-ttu-id="54b88-139">Fornire informazioni sugli errori rappresenta un rischio di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="54b88-139">Serving errors is a security risk.</span></span>

<span data-ttu-id="54b88-140">Per visualizzare la pagina di gestione delle eccezioni nell'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), usare le direttive del preprocessore `ProdEnvironment` e `ErrorHandlerPage` e selezionare **Trigger an exception** (Attiva un'eccezione) nella home page.</span><span class="sxs-lookup"><span data-stu-id="54b88-140">To see the exception handling page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerPage` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-lambda"></a><span data-ttu-id="54b88-141">Espressione lambda per il gestore di eccezioni</span><span class="sxs-lookup"><span data-stu-id="54b88-141">Exception handler lambda</span></span>

<span data-ttu-id="54b88-142">Un'alternativa a una [pagina di gestione delle eccezioni personalizzata](#exception-handler-page) consiste nel fornire un'espressione lambda a <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="54b88-142">An alternative to a [custom exception handler page](#exception-handler-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="54b88-143">L'uso di un'espressione lambda consente l'accesso all'errore prima di restituire la risposta.</span><span class="sxs-lookup"><span data-stu-id="54b88-143">Using a lambda allows access to the error before returning the response.</span></span>

<span data-ttu-id="54b88-144">Di seguito è riportato un esempio dell'uso di un'espressione lambda per la gestione delle eccezioni:</span><span class="sxs-lookup"><span data-stu-id="54b88-144">Here's an example of using a lambda for exception handling:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> <span data-ttu-id="54b88-145">**Non** fornire informazioni sugli errori sensibili da <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> ai client.</span><span class="sxs-lookup"><span data-stu-id="54b88-145">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="54b88-146">Fornire informazioni sugli errori rappresenta un rischio di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="54b88-146">Serving errors is a security risk.</span></span>

<span data-ttu-id="54b88-147">Per visualizzare il risultato dell'espressione lambda di gestione delle eccezioni nell'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), usare le direttive del preprocessore `ProdEnvironment` e `ErrorHandlerLambda` e selezionare **Trigger an exception** (Attiva un'eccezione) nella home page.</span><span class="sxs-lookup"><span data-stu-id="54b88-147">To see the result of the exception handling lambda in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerLambda` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="usestatuscodepages"></a><span data-ttu-id="54b88-148">UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="54b88-148">UseStatusCodePages</span></span>

<span data-ttu-id="54b88-149">Per impostazione predefinita, un'app ASP.NET Core non offre una tabella codici di stato per i codici di stato HTTP, ad esempio *404 - Non trovato*.</span><span class="sxs-lookup"><span data-stu-id="54b88-149">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="54b88-150">L'app restituisce un codice di stato e un corpo della risposta vuoto.</span><span class="sxs-lookup"><span data-stu-id="54b88-150">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="54b88-151">Per specificare le tabelle codici di stato, usare il middleware delle tabelle codici di stato.</span><span class="sxs-lookup"><span data-stu-id="54b88-151">To provide status code pages, use Status Code Pages middleware.</span></span>

<span data-ttu-id="54b88-152">Il middleware è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="54b88-152">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="54b88-153">Per abilitare i gestori di solo testo predefiniti per i codici di stato di errore comuni, chiamare <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> nel metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="54b88-153">To enable default text-only handlers for common error status codes, call <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> in the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="54b88-154">Chiamare `UseStatusCodePages` prima del middleware di gestione delle richieste (ad esempio, middleware dei file statici e middleware MVC).</span><span class="sxs-lookup"><span data-stu-id="54b88-154">Call `UseStatusCodePages` before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="54b88-155">Di seguito è riportato un esempio del testo visualizzato dai gestori predefiniti:</span><span class="sxs-lookup"><span data-stu-id="54b88-155">Here's an example of text displayed by the default handlers:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="54b88-156">Per visualizzare uno dei vari formati di tabella codici di stato nell'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), usare una delle direttive del preprocessore che iniziano con `StatusCodePages` e selezionare **Trigger a 404** (Genera un 404) nella home page.</span><span class="sxs-lookup"><span data-stu-id="54b88-156">To see one of the various status code page formats in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use one of the preprocessor directives that begin with `StatusCodePages`, and select **Trigger a 404** on the home page.</span></span>

## <a name="usestatuscodepages-with-format-string"></a><span data-ttu-id="54b88-157">UseStatusCodePages con stringa di formato</span><span class="sxs-lookup"><span data-stu-id="54b88-157">UseStatusCodePages with format string</span></span>

<span data-ttu-id="54b88-158">Per personalizzare il tipo di contenuto della risposta e il testo, usare l'overload di <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> che accetta un tipo di contenuto e una stringa di formato:</span><span class="sxs-lookup"><span data-stu-id="54b88-158">To customize the response content type and text, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a content type and format string:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a><span data-ttu-id="54b88-159">UseStatusCodePages con espressione lambda</span><span class="sxs-lookup"><span data-stu-id="54b88-159">UseStatusCodePages with lambda</span></span>

<span data-ttu-id="54b88-160">Per specificare la gestione degli errori personalizzata e il codice per la scrittura delle risposte, usare l'overload di <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> che accetta un'espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="54b88-160">To specify custom error-handling and response-writing code, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirects"></a><span data-ttu-id="54b88-161">UseStatusCodePagesWithRedirects</span><span class="sxs-lookup"><span data-stu-id="54b88-161">UseStatusCodePagesWithRedirects</span></span>

<span data-ttu-id="54b88-162">Metodo di estensione <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="54b88-162">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> extension method:</span></span>

* <span data-ttu-id="54b88-163">Invia un codice di stato *302 - Trovato* al client.</span><span class="sxs-lookup"><span data-stu-id="54b88-163">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="54b88-164">Reindirizza il client al percorso specificato nel modello di URL.</span><span class="sxs-lookup"><span data-stu-id="54b88-164">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="54b88-165">Il modello di URL può includere un segnaposto `{0}` per il codice di stato, come illustrato nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="54b88-165">The URL template can include a `{0}` placeholder for the status code, as shown in the example.</span></span> <span data-ttu-id="54b88-166">Se il modello di URL inizia con una tilde (~), la tilde viene sostituita con il valore `PathBase` dell'app.</span><span class="sxs-lookup"><span data-stu-id="54b88-166">If the URL template starts with a tilde (~), the tilde is replaced by the app's `PathBase`.</span></span> <span data-ttu-id="54b88-167">Se si punta a un endpoint all'interno dell'app, creare una visualizzazione MVC o una pagina Razor per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="54b88-167">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="54b88-168">Per un esempio di Razor Pages, vedere *StatusCode.cshtml* nell'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="54b88-168">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="54b88-169">Questo metodo viene usato comunemente quando l'app:</span><span class="sxs-lookup"><span data-stu-id="54b88-169">This method is commonly used when the app:</span></span>

* <span data-ttu-id="54b88-170">Deve reindirizzare il client a un endpoint diverso, in genere nei casi in cui un'altra app elabora l'errore.</span><span class="sxs-lookup"><span data-stu-id="54b88-170">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="54b88-171">Per le app Web, la barra degli indirizzi del browser del client riflette l'endpoint reindirizzato.</span><span class="sxs-lookup"><span data-stu-id="54b88-171">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="54b88-172">Non deve conservare e restituire il codice di stato originale con la risposta di reindirizzamento iniziale.</span><span class="sxs-lookup"><span data-stu-id="54b88-172">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

## <a name="usestatuscodepageswithreexecute"></a><span data-ttu-id="54b88-173">UseStatusCodePagesWithReExecute</span><span class="sxs-lookup"><span data-stu-id="54b88-173">UseStatusCodePagesWithReExecute</span></span>

<span data-ttu-id="54b88-174">Metodo di estensione <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="54b88-174">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> extension method:</span></span>

* <span data-ttu-id="54b88-175">Restituisce il codice di stato originale al client.</span><span class="sxs-lookup"><span data-stu-id="54b88-175">Returns the original status code to the client.</span></span>
* <span data-ttu-id="54b88-176">Genera il corpo della risposta eseguendo nuovamente la pipeline delle richieste con un percorso alternativo.</span><span class="sxs-lookup"><span data-stu-id="54b88-176">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

<span data-ttu-id="54b88-177">Se si punta a un endpoint all'interno dell'app, creare una visualizzazione MVC o una pagina Razor per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="54b88-177">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="54b88-178">Per un esempio di Razor Pages, vedere *StatusCode.cshtml* nell'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="54b88-178">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="54b88-179">Questo metodo viene usato comunemente quando l'app deve:</span><span class="sxs-lookup"><span data-stu-id="54b88-179">This method is commonly used when the app should:</span></span>

* <span data-ttu-id="54b88-180">Elaborare la richiesta senza il reindirizzamento a un endpoint diverso.</span><span class="sxs-lookup"><span data-stu-id="54b88-180">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="54b88-181">Per le app Web, la barra degli indirizzi del browser del client riflette l'endpoint richiesto in origine.</span><span class="sxs-lookup"><span data-stu-id="54b88-181">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="54b88-182">Conservare e restituire il codice di stato originale con la risposta.</span><span class="sxs-lookup"><span data-stu-id="54b88-182">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="54b88-183">I modelli di URL e di stringa di query potrebbero includere un segnaposto (`{0}`) per il codice di stato.</span><span class="sxs-lookup"><span data-stu-id="54b88-183">The URL and query string templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="54b88-184">Il modello di URL deve iniziare con una barra (`/`).</span><span class="sxs-lookup"><span data-stu-id="54b88-184">The URL template must start with a slash (`/`).</span></span> <span data-ttu-id="54b88-185">Quando si usa un segnaposto nel percorso, verificare che l'endpoint (pagina o controller) possa elaborare il segmento del percorso.</span><span class="sxs-lookup"><span data-stu-id="54b88-185">When using a placeholder in the path, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="54b88-186">Ad esempio, una pagina Razor per gli errori deve accettare il valore del segmento di percorso facoltativo con la direttiva `@page`:</span><span class="sxs-lookup"><span data-stu-id="54b88-186">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="54b88-187">L'endpoint che elabora l'errore può ottenere l'URL originale che ha generato l'errore, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="54b88-187">The endpoint that processes the error can get the original URL that generated the error, as shown in the following example:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a><span data-ttu-id="54b88-188">Disabilitare le tabelle codici di stato</span><span class="sxs-lookup"><span data-stu-id="54b88-188">Disable status code pages</span></span>

<span data-ttu-id="54b88-189">Per disabilitare le tabelle codici di stato per un controller MVC o un metodo di azione, usare l'attributo [[SkipStatusCodePages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute).</span><span class="sxs-lookup"><span data-stu-id="54b88-189">To disable status code pages for an MVC controller or action method, use the [[SkipStatusCodePages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) attribute.</span></span>

<span data-ttu-id="54b88-190">Per disabilitare le tabelle codici di stato per richieste specifiche in un metodo del gestore Razor Pages o in un controller MVC, usare <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span><span class="sxs-lookup"><span data-stu-id="54b88-190">To disable status code pages for specific requests in a Razor Pages handler method or in an MVC controller, use <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="54b88-191">Codice di gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="54b88-191">Exception-handling code</span></span>

<span data-ttu-id="54b88-192">Il codice in pagine di gestione delle eccezioni può generare eccezioni.</span><span class="sxs-lookup"><span data-stu-id="54b88-192">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="54b88-193">È spesso consigliabile che le pagine di errore di produzione contengano esclusivamente contenuto statico.</span><span class="sxs-lookup"><span data-stu-id="54b88-193">It's often a good idea for production error pages to consist of purely static content.</span></span>

### <a name="response-headers"></a><span data-ttu-id="54b88-194">Intestazioni della risposta</span><span class="sxs-lookup"><span data-stu-id="54b88-194">Response headers</span></span>

<span data-ttu-id="54b88-195">Dopo l'invio delle intestazioni per una risposta:</span><span class="sxs-lookup"><span data-stu-id="54b88-195">Once the headers for a response are sent:</span></span>

* <span data-ttu-id="54b88-196">L'app non può modificare il codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="54b88-196">The app can't change the response's status code.</span></span>
* <span data-ttu-id="54b88-197">Non è possibile eseguire pagine o gestori delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="54b88-197">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="54b88-198">La risposta deve essere completata o la connessione deve essere interrotta.</span><span class="sxs-lookup"><span data-stu-id="54b88-198">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="54b88-199">Gestione delle eccezioni del server</span><span class="sxs-lookup"><span data-stu-id="54b88-199">Server exception handling</span></span>

<span data-ttu-id="54b88-200">Oltre alla logica di gestione delle eccezioni nell'app, l'[implementazione del server HTTP](xref:fundamentals/servers/index) può gestire alcune eccezioni.</span><span class="sxs-lookup"><span data-stu-id="54b88-200">In addition to the exception handling logic in your app, the [HTTP server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="54b88-201">Se rileva un'eccezione prima dell'invio delle intestazioni di risposta, il server invia una risposta *500 - Errore interno del server* senza corpo.</span><span class="sxs-lookup"><span data-stu-id="54b88-201">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="54b88-202">Se il server rileva un'eccezione dopo l'invio delle intestazioni di risposta, il server chiude la connessione.</span><span class="sxs-lookup"><span data-stu-id="54b88-202">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="54b88-203">Le richieste che non sono gestite dall'app vengono gestite dal server.</span><span class="sxs-lookup"><span data-stu-id="54b88-203">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="54b88-204">Qualsiasi eccezione che si verifica quando il server sta gestendo la richiesta viene gestita dalla gestione delle eccezioni del server.</span><span class="sxs-lookup"><span data-stu-id="54b88-204">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="54b88-205">Eventuali pagine di errore personalizzate dell'app, middleware di gestione delle eccezioni e filtri non hanno effetto su questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="54b88-205">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="54b88-206">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="54b88-206">Startup exception handling</span></span>

<span data-ttu-id="54b88-207">Solo il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="54b88-207">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="54b88-208">L'host può essere configurato per [acquisire gli errori di avvio](xref:fundamentals/host/web-host#capture-startup-errors) e [acquisire errori dettagliati](xref:fundamentals/host/web-host#detailed-errors).</span><span class="sxs-lookup"><span data-stu-id="54b88-208">The host can be configured to [capture startup errors](xref:fundamentals/host/web-host#capture-startup-errors) and [capture detailed errors](xref:fundamentals/host/web-host#detailed-errors).</span></span>

<span data-ttu-id="54b88-209">Il livello di hosting può visualizzare una pagina di errore per un errore di avvio acquisito solo se l'errore si verifica dopo l'associazione indirizzo host/porta.</span><span class="sxs-lookup"><span data-stu-id="54b88-209">The hosting layer can show an error page for a captured startup error only if the error occurs after host address/port binding.</span></span> <span data-ttu-id="54b88-210">Se si verifica un errore di associazione:</span><span class="sxs-lookup"><span data-stu-id="54b88-210">If binding fails:</span></span>

* <span data-ttu-id="54b88-211">Il livello di hosting registra un'eccezione critica.</span><span class="sxs-lookup"><span data-stu-id="54b88-211">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="54b88-212">Si verifica un arresto anomalo del processo di dotnet.</span><span class="sxs-lookup"><span data-stu-id="54b88-212">The dotnet process crashes.</span></span>
* <span data-ttu-id="54b88-213">Non viene visualizzata alcuna pagina di errore quando il server HTTP è [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="54b88-213">No error page is displayed when the HTTP server is [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="54b88-214">Se durante l'esecuzione in [IIS](/iis) (o servizio app di Azure) o in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) il processo non può essere avviato, viene restituito l'errore *502.5 Errore del processo* dal [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="54b88-214">When running on [IIS](/iis) (or Azure App Service) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="54b88-215">Per ulteriori informazioni, vedere <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="54b88-215">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

## <a name="database-error-page"></a><span data-ttu-id="54b88-216">Pagina di errore di database</span><span class="sxs-lookup"><span data-stu-id="54b88-216">Database error page</span></span>

<span data-ttu-id="54b88-217">Il middleware della pagina di errore del database acquisisce le eccezioni correlate al database che possono essere risolte tramite Entity Framework migrazioni.</span><span class="sxs-lookup"><span data-stu-id="54b88-217">Database Error Page Middleware captures database-related exceptions that can be resolved by using Entity Framework migrations.</span></span> <span data-ttu-id="54b88-218">Quando si verificano queste eccezioni, viene generata una risposta HTML con i dettagli delle azioni possibili per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="54b88-218">When these exceptions occur, an HTML response with details of possible actions to resolve the issue is generated.</span></span> <span data-ttu-id="54b88-219">Questa pagina deve essere abilitata solo nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="54b88-219">This page should be enabled only in the Development environment.</span></span> <span data-ttu-id="54b88-220">Abilitare la pagina aggiungendo codice `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="54b88-220">Enable the page by adding code to `Startup.Configure`:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

<!-- FUTURE UPDATE: On the next topic overhaul/release update, add API crosslink to this section for xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage* when available via the API docs. -->

## <a name="exception-filters"></a><span data-ttu-id="54b88-221">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="54b88-221">Exception filters</span></span>

<span data-ttu-id="54b88-222">Nelle app MVC i filtri delle eccezioni possono essere configurati a livello globale o per ogni controller o azione singolarmente.</span><span class="sxs-lookup"><span data-stu-id="54b88-222">In MVC apps, exception filters can be configured globally or on a per-controller or per-action basis.</span></span> <span data-ttu-id="54b88-223">Nelle app Razor Pages è possibile configurarli a livello globale o per ogni modello di pagina.</span><span class="sxs-lookup"><span data-stu-id="54b88-223">In Razor Pages apps, they can be configured globally or per page model.</span></span> <span data-ttu-id="54b88-224">Questi filtri gestiscono tutte le eccezioni non gestite che si verificano durante l'esecuzione di un'azione del controller o di un altro filtro</span><span class="sxs-lookup"><span data-stu-id="54b88-224">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="54b88-225">Per ulteriori informazioni, vedere <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="54b88-225">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="54b88-226">I filtri delle eccezioni sono utili per intercettare le eccezioni che si verificano all'interno di azioni MVC, ma non sono flessibili come il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="54b88-226">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="54b88-227">È consigliabile usare il middleware.</span><span class="sxs-lookup"><span data-stu-id="54b88-227">We recommend using the middleware.</span></span> <span data-ttu-id="54b88-228">Usare i filtri solo quando è necessario eseguire la gestione degli errori in modo diverso in base all'azione MVC scelta.</span><span class="sxs-lookup"><span data-stu-id="54b88-228">Use filters only where you need to perform error handling differently based on which MVC action is chosen.</span></span>

## <a name="model-state-errors"></a><span data-ttu-id="54b88-229">Errori di stato del modello</span><span class="sxs-lookup"><span data-stu-id="54b88-229">Model state errors</span></span>

<span data-ttu-id="54b88-230">Per informazioni su come gestire gli errori dello stato del modello, vedere [Associazione di modelli](xref:mvc/models/model-binding) e [Convalida del modello](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="54b88-230">For information about how to handle model state errors, see [Model binding](xref:mvc/models/model-binding) and [Model validation](xref:mvc/models/validation).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="54b88-231">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="54b88-231">Additional resources</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
