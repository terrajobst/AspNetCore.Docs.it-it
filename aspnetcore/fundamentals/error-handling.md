---
title: Gestire gli errori in ASP.NET Core
author: ardalis
description: Informazioni su come gestire gli errori nelle app ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: 126a782bfd32f9ecd0596045218371ef5ccc82f2
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894140"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="572b4-103">Gestire gli errori in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="572b4-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="572b4-104">Di [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="572b4-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="572b4-105">Questo articolo descrive gli approcci comuni per la gestione degli errori nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="572b4-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="572b4-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="572b4-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="572b4-107">Pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="572b4-107">The developer exception page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="572b4-108">Per configurare un'app in modo che sia visualizzata una pagina contenente informazioni dettagliate sulle eccezioni, usare la *pagina delle eccezioni per gli sviluppatori*.</span><span class="sxs-lookup"><span data-stu-id="572b4-108">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="572b4-109">La pagina è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="572b4-109">The page made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="572b4-110">Aggiungere una riga al metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="572b4-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="572b4-111">Per configurare un'app in modo che sia visualizzata una pagina contenente informazioni dettagliate sulle eccezioni, usare la *pagina delle eccezioni per gli sviluppatori*.</span><span class="sxs-lookup"><span data-stu-id="572b4-111">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="572b4-112">La pagina è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="572b4-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="572b4-113">Aggiungere una riga al metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="572b4-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="572b4-114">Per configurare un'app in modo che sia visualizzata una pagina contenente informazioni dettagliate sulle eccezioni, usare la *pagina delle eccezioni per gli sviluppatori*.</span><span class="sxs-lookup"><span data-stu-id="572b4-114">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="572b4-115">La pagina diventa disponibile aggiungendo una riferimento al pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="572b4-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="572b4-116">Aggiungere una riga al metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="572b4-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="572b4-117">Posizionare la chiamata a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) davanti al middleware in cui si vogliono rilevare le eccezioni, ad esempio `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="572b4-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="572b4-118">Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo**</span><span class="sxs-lookup"><span data-stu-id="572b4-118">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="572b4-119">per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="572b4-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="572b4-120">[Altre informazioni sulla configurazione degli ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="572b4-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="572b4-121">Per visualizzare la pagina delle eccezioni per gli sviluppatori, eseguire l'app di esempio con l'ambiente impostato su `Development` e aggiungere `?throw=true` all'URL di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="572b4-121">To see the developer exception page, run the sample app with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="572b4-122">La pagina include diverse schede con informazioni sull'eccezione e sulla richiesta.</span><span class="sxs-lookup"><span data-stu-id="572b4-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="572b4-123">La prima scheda include un'analisi dello stack:</span><span class="sxs-lookup"><span data-stu-id="572b4-123">The first tab includes a stack trace:</span></span>

![Analisi dello stack](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="572b4-125">La scheda successiva visualizza i parametri della stringa di query, se presenti:</span><span class="sxs-lookup"><span data-stu-id="572b4-125">The next tab shows the query string parameters, if any:</span></span>

![Parametri della stringa di query](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="572b4-127">Se la richiesta contiene cookie, verranno visualizzati nella scheda **Cookie**. Le intestazioni vengono visualizzate nell'ultima scheda:</span><span class="sxs-lookup"><span data-stu-id="572b4-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Intestazioni](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="572b4-129">Configurazione di una pagina di gestione delle eccezioni personalizzata</span><span class="sxs-lookup"><span data-stu-id="572b4-129">Configuring a custom exception handling page</span></span>

<span data-ttu-id="572b4-130">Configurare una pagina di gestione delle eccezioni da usare quando l'app non è in esecuzione nell'ambiente `Development`:</span><span class="sxs-lookup"><span data-stu-id="572b4-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="572b4-131">In un'app Razor Pages il modello Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) offre una pagina Errore e una classe di modelli di pagina `ErrorModel` nella cartella *Pages* (Pagine).</span><span class="sxs-lookup"><span data-stu-id="572b4-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="572b4-132">In un'app MVC non decorare il metodo dell'azione di gestione degli errori con attributi di metodo HTTP, ad esempio `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="572b4-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="572b4-133">I verbi espliciti impediscono ad alcune richieste di raggiungere il metodo.</span><span class="sxs-lookup"><span data-stu-id="572b4-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="572b4-134">Consentire l'accesso anonimo al metodo in modo che gli utenti non autenticati possano ricevere la visualizzazione degli errori.</span><span class="sxs-lookup"><span data-stu-id="572b4-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="572b4-135">Ad esempio, il metodo gestore errori seguente viene fornito dal modello MVC [dotnet new](/dotnet/core/tools/dotnet-new) e visualizzato nel controller Home:</span><span class="sxs-lookup"><span data-stu-id="572b4-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="572b4-136">Configurazione delle tabelle codici di stato</span><span class="sxs-lookup"><span data-stu-id="572b4-136">Configuring status code pages</span></span>

<span data-ttu-id="572b4-137">Per impostazione predefinita, un'app non offre una tabella codici di stato completa per i codici di stato HTTP, ad esempio *404 Non trovato*.</span><span class="sxs-lookup"><span data-stu-id="572b4-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="572b4-138">Per includere tabelle codici di stato, configurare il middleware delle tabelle codici di stato aggiungendo una riga al metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="572b4-138">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="572b4-139">Per impostazione predefinita, il middleware delle pagine dei codici di stato aggiunge gestori di solo testo per i codici di stato comuni, ad esempio 404:</span><span class="sxs-lookup"><span data-stu-id="572b4-139">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![Pagina 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="572b4-141">Il middleware supporta vari metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="572b4-141">The middleware supports several extension methods.</span></span> <span data-ttu-id="572b4-142">Un metodo accetta un'espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="572b4-142">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="572b4-143">Un altro metodo accetta un tipo di contenuto e una stringa di formato:</span><span class="sxs-lookup"><span data-stu-id="572b4-143">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="572b4-144">Sono disponibili anche metodi di estensione di reindirizzamento e ripetizione dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="572b4-144">There's also redirect and re-execute extension methods.</span></span> <span data-ttu-id="572b4-145">Il metodo di reindirizzamento invia un codice di stato *302 - Trovato* al client:</span><span class="sxs-lookup"><span data-stu-id="572b4-145">The redirect method sends a *302 Found* status code to the client:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="572b4-146">Il metodo di ripetizione dell'esecuzione restituisce il codice di stato originale al client, ma esegue anche il gestore per l'URL di reindirizzamento:</span><span class="sxs-lookup"><span data-stu-id="572b4-146">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="572b4-147">Le tabelle codici di stato possono essere disabilitate per richieste specifiche in un metodo del gestore Razor Pages o in un controller MVC.</span><span class="sxs-lookup"><span data-stu-id="572b4-147">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="572b4-148">Per disabilitare le tabelle codici di stato, provare a recuperare [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) dalla raccolta [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) della richiesta e disabilitare la funzionalità, se disponibile:</span><span class="sxs-lookup"><span data-stu-id="572b4-148">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="572b4-149">Se si usa un overload `UseStatusCodePages*` che punta a un endpoint all'interno dell'app, creare una visualizzazione MVC o una pagina Razor per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="572b4-149">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="572b4-150">Ad esempio, il modello [dotnet new](/dotnet/core/tools/dotnet-new) per un'app Razor Pages produce la pagina e la classe di modelli di pagina seguenti:</span><span class="sxs-lookup"><span data-stu-id="572b4-150">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="572b4-151">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="572b4-151">*Error.cshtml*:</span></span>

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

<span data-ttu-id="572b4-152">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="572b4-152">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="572b4-153">Codice di gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="572b4-153">Exception-handling code</span></span>

<span data-ttu-id="572b4-154">Il codice in pagine di gestione delle eccezioni può generare eccezioni.</span><span class="sxs-lookup"><span data-stu-id="572b4-154">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="572b4-155">È spesso consigliabile che le pagine di errore di produzione contengano esclusivamente contenuto statico.</span><span class="sxs-lookup"><span data-stu-id="572b4-155">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="572b4-156">Tenere anche presente che dopo che le intestazioni per una risposta sono state inviate, non è possibile modificare il codice di stato della risposta né eseguire pagine delle eccezioni o gestori.</span><span class="sxs-lookup"><span data-stu-id="572b4-156">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="572b4-157">La risposta deve essere completata o la connessione deve essere interrotta.</span><span class="sxs-lookup"><span data-stu-id="572b4-157">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="572b4-158">Gestione delle eccezioni del server</span><span class="sxs-lookup"><span data-stu-id="572b4-158">Server exception handling</span></span>

<span data-ttu-id="572b4-159">Oltre alla logica di gestione delle eccezioni nell'app, il [server](xref:fundamentals/servers/index) che ospita l'app esegue una parte della gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="572b4-159">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="572b4-160">Se rileva un'eccezione prima dell'invio delle intestazioni, il server invia una risposta *500 Errore interno del server* senza corpo.</span><span class="sxs-lookup"><span data-stu-id="572b4-160">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="572b4-161">Se il server rileva un'eccezione dopo l'invio delle intestazioni, il server chiude la connessione.</span><span class="sxs-lookup"><span data-stu-id="572b4-161">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="572b4-162">Le richieste che non sono gestite dall'app vengono gestite dal server.</span><span class="sxs-lookup"><span data-stu-id="572b4-162">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="572b4-163">Tutte le eccezioni vengono gestite dalla gestione delle eccezioni del server.</span><span class="sxs-lookup"><span data-stu-id="572b4-163">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="572b4-164">Eventuali pagine di errore personalizzate, middleware di gestione delle eccezioni o filtri configurati non hanno effetto su questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="572b4-164">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="572b4-165">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="572b4-165">Startup exception handling</span></span>

<span data-ttu-id="572b4-166">Solo il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="572b4-166">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="572b4-167">Con [Host Web](xref:fundamentals/host/web-host) è possibile [configurare il comportamento dell'host in risposta agli errori durante l'avvio](xref:fundamentals/host/web-host#detailed-errors) usando `captureStartupErrors` e le chiavi `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="572b4-167">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="572b4-168">L'hosting può visualizzare solo una pagina di errore per un errore di avvio acquisito se l'errore si verifica dopo l'associazione indirizzo host/porta.</span><span class="sxs-lookup"><span data-stu-id="572b4-168">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="572b4-169">Se un'associazione ha esito negativo per qualsiasi ragione, il livello di hosting registra un'eccezione critica, il processo dotnet viene interrotto e non viene visualizzata alcuna pagina di errore quando l'app è in esecuzione nel server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="572b4-169">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="572b4-170">Se durante l'esecuzione su [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) non è possibile avviare il processo, viene restituito un codice *502.5 Errore del processo* dal [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="572b4-170">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="572b4-171">Seguire i consigli sulla risoluzione dei problemi in [Risolvere i problemi di ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="572b4-171">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="572b4-172">Gestione degli errori di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="572b4-172">ASP.NET MVC error handling</span></span>

<span data-ttu-id="572b4-173">Le app [MVC](xref:mvc/overview) includono alcune opzioni aggiuntive per la gestione degli errori, ad esempio la configurazione di filtri delle eccezioni e l'esecuzione della convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="572b4-173">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="572b4-174">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="572b4-174">Exception filters</span></span>

<span data-ttu-id="572b4-175">I filtri delle eccezioni possono essere configurati a livello globale o per singolo controller o azione in un'app MVC.</span><span class="sxs-lookup"><span data-stu-id="572b4-175">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="572b4-176">Questi filtri gestiscono tutte le eccezioni non gestite che si verificano durante l'esecuzione di un'azione del controller o di un altro filtro</span><span class="sxs-lookup"><span data-stu-id="572b4-176">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="572b4-177">e non vengono chiamati in altro modo.</span><span class="sxs-lookup"><span data-stu-id="572b4-177">These filters aren't called otherwise.</span></span> <span data-ttu-id="572b4-178">Per altre informazioni, vedere [Filtri](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="572b4-178">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="572b4-179">I filtri delle eccezioni sono utili per intercettare le eccezioni che si verificano all'interno di azioni MV, ma non sono flessibili come il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="572b4-179">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="572b4-180">In genere è preferibile usare il middleware. Usare invece i filtri solo quando è necessario gestire gli errori *in modo diverso* in base all'azione MVC scelta.</span><span class="sxs-lookup"><span data-stu-id="572b4-180">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="572b4-181">Gestione degli errori dello stato del modello</span><span class="sxs-lookup"><span data-stu-id="572b4-181">Handling model state errors</span></span>

<span data-ttu-id="572b4-182">La [convalida del modello](xref:mvc/models/validation) avviene prima di richiamare ogni azione del controller ed è responsabilità del metodo di azione controllare `ModelState.IsValid` e rispondere in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="572b4-182">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="572b4-183">Alcune app scelgono di seguire una convenzione standard per gestire gli errori di convalida del modello. In tal caso un [filtro](xref:mvc/controllers/filters) può essere l'elemento adatto in cui implementare i criteri.</span><span class="sxs-lookup"><span data-stu-id="572b4-183">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="572b4-184">È consigliabile testare il comportamento delle azioni con gli stati del modello non validi.</span><span class="sxs-lookup"><span data-stu-id="572b4-184">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="572b4-185">Altre informazioni sono disponibili in [Test della logica dei controller](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="572b4-185">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="572b4-186">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="572b4-186">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/azure-apps/troubleshoot>
