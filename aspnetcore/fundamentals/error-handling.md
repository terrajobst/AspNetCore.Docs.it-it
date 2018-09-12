---
title: Gestire gli errori in ASP.NET Core
author: ardalis
description: Informazioni su come gestire gli errori nelle app ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: 7ea944bc423001aa47ce684443b96104cf9174bf
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312247"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="61ea1-103">Gestire gli errori in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61ea1-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="61ea1-104">Di [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="61ea1-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="61ea1-105">Questo articolo descrive gli approcci comuni per la gestione degli errori nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61ea1-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="61ea1-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="61ea1-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="61ea1-107">Pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="61ea1-107">The Developer Exception Page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="61ea1-108">Per configurare un'app in modo che sia visualizzata una pagina contenente informazioni dettagliate sulle eccezioni, usare la *pagina delle eccezioni per gli sviluppatori*.</span><span class="sxs-lookup"><span data-stu-id="61ea1-108">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="61ea1-109">La pagina è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="61ea1-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="61ea1-110">Aggiungere una riga al metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="61ea1-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="61ea1-111">Per configurare un'app in modo che sia visualizzata una pagina contenente informazioni dettagliate sulle eccezioni, usare la *pagina delle eccezioni per gli sviluppatori*.</span><span class="sxs-lookup"><span data-stu-id="61ea1-111">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="61ea1-112">La pagina è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="61ea1-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="61ea1-113">Aggiungere una riga al metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="61ea1-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="61ea1-114">Per configurare un'app in modo che sia visualizzata una pagina contenente informazioni dettagliate sulle eccezioni, usare la *pagina delle eccezioni per gli sviluppatori*.</span><span class="sxs-lookup"><span data-stu-id="61ea1-114">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="61ea1-115">La pagina diventa disponibile aggiungendo una riferimento al pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="61ea1-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="61ea1-116">Aggiungere una riga al metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="61ea1-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="61ea1-117">Posizionare la chiamata a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) davanti al middleware in cui si vogliono rilevare le eccezioni, ad esempio `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="61ea1-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

> [!WARNING]
> <span data-ttu-id="61ea1-118">Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="61ea1-118">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="61ea1-119">per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="61ea1-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="61ea1-120">[Altre informazioni sulla configurazione degli ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="61ea1-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="61ea1-121">Per visualizzare la pagina delle eccezioni per gli sviluppatori, eseguire l'app di esempio con l'ambiente impostato su `Development` e aggiungere `?throw=true` all'URL di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="61ea1-121">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="61ea1-122">La pagina include diverse schede con informazioni sull'eccezione e sulla richiesta.</span><span class="sxs-lookup"><span data-stu-id="61ea1-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="61ea1-123">La prima scheda include un'analisi dello stack:</span><span class="sxs-lookup"><span data-stu-id="61ea1-123">The first tab includes a stack trace:</span></span>

![Analisi dello stack](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="61ea1-125">La scheda successiva visualizza i parametri della stringa di query, se presenti:</span><span class="sxs-lookup"><span data-stu-id="61ea1-125">The next tab shows the query string parameters, if any:</span></span>

![Parametri della stringa di query](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="61ea1-127">Se la richiesta contiene cookie, verranno visualizzati nella scheda **Cookie**. Le intestazioni vengono visualizzate nell'ultima scheda:</span><span class="sxs-lookup"><span data-stu-id="61ea1-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Intestazioni](error-handling/_static/developer-exception-page-headers.png)

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="61ea1-129">Configurare una pagina di gestione delle eccezioni personalizzata</span><span class="sxs-lookup"><span data-stu-id="61ea1-129">Configure a custom exception handling page</span></span>

<span data-ttu-id="61ea1-130">Configurare una pagina di gestione delle eccezioni da usare quando l'app non è in esecuzione nell'ambiente `Development`:</span><span class="sxs-lookup"><span data-stu-id="61ea1-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="61ea1-131">In un'app Razor Pages il modello Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) offre una pagina Errore e una classe di modelli di pagina `ErrorModel` nella cartella *Pages* (Pagine).</span><span class="sxs-lookup"><span data-stu-id="61ea1-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="61ea1-132">In un'app MVC non decorare il metodo dell'azione di gestione degli errori con attributi di metodo HTTP, ad esempio `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="61ea1-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="61ea1-133">I verbi espliciti impediscono ad alcune richieste di raggiungere il metodo.</span><span class="sxs-lookup"><span data-stu-id="61ea1-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="61ea1-134">Consentire l'accesso anonimo al metodo in modo che gli utenti non autenticati possano ricevere la visualizzazione degli errori.</span><span class="sxs-lookup"><span data-stu-id="61ea1-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="61ea1-135">Ad esempio, il metodo gestore errori seguente viene fornito dal modello MVC [dotnet new](/dotnet/core/tools/dotnet-new) e visualizzato nel controller Home:</span><span class="sxs-lookup"><span data-stu-id="61ea1-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configure-status-code-pages"></a><span data-ttu-id="61ea1-136">Configurare le tabelle codici di stato</span><span class="sxs-lookup"><span data-stu-id="61ea1-136">Configure status code pages</span></span>

<span data-ttu-id="61ea1-137">Per impostazione predefinita, un'app non offre una tabella codici di stato completa per i codici di stato HTTP, ad esempio *404 Non trovato*.</span><span class="sxs-lookup"><span data-stu-id="61ea1-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="61ea1-138">Per specificare le tabelle codici di stato, usare il middleware delle tabelle codici di stato.</span><span class="sxs-lookup"><span data-stu-id="61ea1-138">To provide status code pages, use Status Code Pages Middleware.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="61ea1-139">Il middleware è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="61ea1-139">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="61ea1-140">Il middleware è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="61ea1-140">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="61ea1-141">Il middleware diventa disponibile aggiungendo un riferimento al pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="61ea1-141">The middleware is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span>

::: moniker-end

<span data-ttu-id="61ea1-142">Aggiungere una riga al metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="61ea1-142">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="61ea1-143"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> deve essere chiamato prima dei middleware di gestione richiesta nella pipeline (ad esempio, middleware dei file statici e middleware MVC).</span><span class="sxs-lookup"><span data-stu-id="61ea1-143"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> should be called before request handling middlewares in the pipeline (for example, Static Files Middleware and MVC Middleware).</span></span>

<span data-ttu-id="61ea1-144">Per impostazione predefinita, il middleware delle pagine dei codici di stato aggiunge gestori di solo testo per i codici di stato comuni, ad esempio 404:</span><span class="sxs-lookup"><span data-stu-id="61ea1-144">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![Pagina 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="61ea1-146">Il middleware supporta vari metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="61ea1-146">The middleware supports several extension methods.</span></span> <span data-ttu-id="61ea1-147">Un metodo accetta un'espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="61ea1-147">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="61ea1-148">Un altro metodo accetta un tipo di contenuto e una stringa di formato:</span><span class="sxs-lookup"><span data-stu-id="61ea1-148">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="61ea1-149">Sono disponibili anche metodi di estensione di reindirizzamento e ripetizione dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="61ea1-149">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="61ea1-150">Il metodo di reindirizzamento invia un codice di stato *302 - Trovato* al client e reindirizza il client al modello di URL di posizione specificato.</span><span class="sxs-lookup"><span data-stu-id="61ea1-150">The redirect method sends a *302 Found* status code to the client and redirects the client to the provided location URL template.</span></span> <span data-ttu-id="61ea1-151">Il modello può includere un segnaposto `{0}` per il codice di stato.</span><span class="sxs-lookup"><span data-stu-id="61ea1-151">The template may include a `{0}` placeholder for the status code.</span></span> <span data-ttu-id="61ea1-152">Gli URL che iniziano con `~` vengono fatti precedere dal percorso di base.</span><span class="sxs-lookup"><span data-stu-id="61ea1-152">URLs starting with `~` have the base path prepended.</span></span> <span data-ttu-id="61ea1-153">Un URL che non inizia con `~` viene usato così com'è.</span><span class="sxs-lookup"><span data-stu-id="61ea1-153">A URL that doesn't start with `~` is used as is.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="61ea1-154">Il metodo di riesecuzione restituisce il codice di stato originale al client e specifica che il corpo della risposta deve essere generato rieseguendo la pipeline della richiesta e usando un percorso alternativo.</span><span class="sxs-lookup"><span data-stu-id="61ea1-154">The re-execute method returns the original status code to the client and specifies that the response body should be generated by re-executing the request pipeline using an alternate path.</span></span> <span data-ttu-id="61ea1-155">Questo percorso può contenere un segnaposto `{0}` per il codice di stato:</span><span class="sxs-lookup"><span data-stu-id="61ea1-155">This path may contain a `{0}` placeholder for the status code:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="61ea1-156">Le tabelle codici di stato possono essere disabilitate per richieste specifiche in un metodo del gestore Razor Pages o in un controller MVC.</span><span class="sxs-lookup"><span data-stu-id="61ea1-156">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="61ea1-157">Per disabilitare le tabelle codici di stato, provare a recuperare [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) dalla raccolta [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) della richiesta e disabilitare la funzionalità, se disponibile:</span><span class="sxs-lookup"><span data-stu-id="61ea1-157">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="61ea1-158">Se si usa un overload `UseStatusCodePages*` che punta a un endpoint all'interno dell'app, creare una visualizzazione MVC o una pagina Razor per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="61ea1-158">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="61ea1-159">Ad esempio, il modello [dotnet new](/dotnet/core/tools/dotnet-new) per un'app Razor Pages produce la pagina e la classe di modelli di pagina seguenti:</span><span class="sxs-lookup"><span data-stu-id="61ea1-159">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="61ea1-160">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="61ea1-160">*Error.cshtml*:</span></span>

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

<span data-ttu-id="61ea1-161">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="61ea1-161">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="61ea1-162">Codice di gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="61ea1-162">Exception-handling code</span></span>

<span data-ttu-id="61ea1-163">Il codice in pagine di gestione delle eccezioni può generare eccezioni.</span><span class="sxs-lookup"><span data-stu-id="61ea1-163">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="61ea1-164">È spesso consigliabile che le pagine di errore di produzione contengano esclusivamente contenuto statico.</span><span class="sxs-lookup"><span data-stu-id="61ea1-164">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="61ea1-165">Tenere anche presente che dopo che le intestazioni per una risposta sono state inviate, non è possibile modificare il codice di stato della risposta né eseguire pagine delle eccezioni o gestori.</span><span class="sxs-lookup"><span data-stu-id="61ea1-165">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="61ea1-166">La risposta deve essere completata o la connessione deve essere interrotta.</span><span class="sxs-lookup"><span data-stu-id="61ea1-166">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="61ea1-167">Gestione delle eccezioni del server</span><span class="sxs-lookup"><span data-stu-id="61ea1-167">Server exception handling</span></span>

<span data-ttu-id="61ea1-168">Oltre alla logica di gestione delle eccezioni nell'app, il [server](xref:fundamentals/servers/index) che ospita l'app esegue una parte della gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="61ea1-168">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="61ea1-169">Se rileva un'eccezione prima dell'invio delle intestazioni, il server invia una risposta *500 Errore interno del server* senza corpo.</span><span class="sxs-lookup"><span data-stu-id="61ea1-169">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="61ea1-170">Se il server rileva un'eccezione dopo l'invio delle intestazioni, il server chiude la connessione.</span><span class="sxs-lookup"><span data-stu-id="61ea1-170">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="61ea1-171">Le richieste che non sono gestite dall'app vengono gestite dal server.</span><span class="sxs-lookup"><span data-stu-id="61ea1-171">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="61ea1-172">Tutte le eccezioni vengono gestite dalla gestione delle eccezioni del server.</span><span class="sxs-lookup"><span data-stu-id="61ea1-172">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="61ea1-173">Eventuali pagine di errore personalizzate, middleware di gestione delle eccezioni o filtri configurati non hanno effetto su questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="61ea1-173">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="61ea1-174">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="61ea1-174">Startup exception handling</span></span>

<span data-ttu-id="61ea1-175">Solo il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="61ea1-175">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="61ea1-176">Con [Host Web](xref:fundamentals/host/web-host) è possibile [configurare il comportamento dell'host in risposta agli errori durante l'avvio](xref:fundamentals/host/web-host#detailed-errors) usando `captureStartupErrors` e le chiavi `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="61ea1-176">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="61ea1-177">L'hosting può visualizzare solo una pagina di errore per un errore di avvio acquisito se l'errore si verifica dopo l'associazione indirizzo host/porta.</span><span class="sxs-lookup"><span data-stu-id="61ea1-177">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="61ea1-178">Se un'associazione ha esito negativo per qualsiasi ragione, il livello di hosting registra un'eccezione critica, il processo dotnet viene interrotto e non viene visualizzata alcuna pagina di errore quando l'app è in esecuzione nel server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="61ea1-178">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="61ea1-179">Se durante l'esecuzione su [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) non è possibile avviare il processo, viene restituito un codice *502.5 Errore del processo* dal [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="61ea1-179">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="61ea1-180">Per informazioni sulla risoluzione dei problemi di avvio durante l'hosting con IIS, vedere <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="61ea1-180">For information on troubleshooting startup issues when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="61ea1-181">Per informazioni sulla risoluzione dei problemi di avvio con il servizio app di Azure, vedere <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="61ea1-181">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="61ea1-182">Gestione degli errori di ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="61ea1-182">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="61ea1-183">Le app [MVC](xref:mvc/overview) includono alcune opzioni aggiuntive per la gestione degli errori, ad esempio la configurazione di filtri delle eccezioni e l'esecuzione della convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="61ea1-183">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="61ea1-184">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="61ea1-184">Exception filters</span></span>

<span data-ttu-id="61ea1-185">I filtri delle eccezioni possono essere configurati a livello globale o per singolo controller o azione in un'app MVC.</span><span class="sxs-lookup"><span data-stu-id="61ea1-185">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="61ea1-186">Questi filtri gestiscono tutte le eccezioni non gestite che si verificano durante l'esecuzione di un'azione del controller o di un altro filtro</span><span class="sxs-lookup"><span data-stu-id="61ea1-186">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="61ea1-187">e non vengono chiamati in altro modo.</span><span class="sxs-lookup"><span data-stu-id="61ea1-187">These filters aren't called otherwise.</span></span> <span data-ttu-id="61ea1-188">Per altre informazioni, vedere [Filtri](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="61ea1-188">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="61ea1-189">I filtri delle eccezioni sono utili per intercettare le eccezioni che si verificano all'interno di azioni MV, ma non sono flessibili come il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="61ea1-189">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="61ea1-190">In genere è preferibile usare il middleware. Usare invece i filtri solo quando è necessario gestire gli errori *in modo diverso* in base all'azione MVC scelta.</span><span class="sxs-lookup"><span data-stu-id="61ea1-190">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="61ea1-191">Gestione degli errori dello stato del modello</span><span class="sxs-lookup"><span data-stu-id="61ea1-191">Handling model state errors</span></span>

<span data-ttu-id="61ea1-192">La [convalida del modello](xref:mvc/models/validation) avviene prima di richiamare ogni azione del controller ed è responsabilità del metodo di azione controllare `ModelState.IsValid` e rispondere in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="61ea1-192">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="61ea1-193">Alcune app scelgono di seguire una convenzione standard per gestire gli errori di convalida del modello. In tal caso un [filtro](xref:mvc/controllers/filters) può essere l'elemento adatto in cui implementare i criteri.</span><span class="sxs-lookup"><span data-stu-id="61ea1-193">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="61ea1-194">È consigliabile testare il comportamento delle azioni con gli stati del modello non validi.</span><span class="sxs-lookup"><span data-stu-id="61ea1-194">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="61ea1-195">Altre informazioni sono disponibili in [Test della logica dei controller](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="61ea1-195">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61ea1-196">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="61ea1-196">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
