---
title: Gestione degli errori in ASP.NET Core
author: ardalis
description: Viene illustrato come gestire gli errori nelle applicazioni ASP.NET Core
keywords: ASP.NET Core, gestione degli errori, gestione delle eccezioni,
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5898892c63e978adfabf9939394fef4ea1848d49
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="0bb2a-104">Introduzione alla gestione degli errori in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0bb2a-104">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="0bb2a-105">Da [Steve Smith](http://ardalis.com) e [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="0bb2a-105">By [Steve Smith](http://ardalis.com) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="0bb2a-106">Questo articolo descrive appoaches comuni di gestione degli errori nelle applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-106">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="0bb2a-107">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="0bb2a-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a><span data-ttu-id="0bb2a-108">La pagina di eccezione per sviluppatori</span><span class="sxs-lookup"><span data-stu-id="0bb2a-108">The developer exception page</span></span>

<span data-ttu-id="0bb2a-109">Per configurare un'app per visualizzare una pagina che visualizza informazioni dettagliate sulle eccezioni, installare il `Microsoft.AspNetCore.Diagnostics` NuGet pacchetto e aggiungere una riga per il [configurare metodo nella classe di avvio](startup.md):</span><span class="sxs-lookup"><span data-stu-id="0bb2a-109">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

<span data-ttu-id="0bb2a-110">[!code-csharp[Principale](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="0bb2a-110">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span></span>

<span data-ttu-id="0bb2a-111">Inserire `UseDeveloperExceptionPage` prima middleware che si desidera intercettare le eccezioni, ad esempio `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-111">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="0bb2a-112">Abilitare la pagina di eccezione developer **solo quando l'app è in esecuzione nell'ambiente di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-112">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="0bb2a-113">Non si desidera condividere informazioni dettagliate sulle eccezioni pubblicamente quando viene eseguita l'app nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="0bb2a-114">[Ulteriori informazioni sulla configurazione di ambienti](environments.md).</span><span class="sxs-lookup"><span data-stu-id="0bb2a-114">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="0bb2a-115">Per visualizzare la pagina di eccezione per sviluppatori, eseguire l'applicazione di esempio con l'ambiente impostato su `Development`e aggiungere `?throw=true` all'URL di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-115">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="0bb2a-116">La pagina include diverse schede con le informazioni sull'eccezione e la richiesta.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-116">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="0bb2a-117">La prima scheda include un'analisi dello stack.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-117">The first tab includes a stack trace.</span></span> 

![Analisi dello stack](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="0bb2a-119">La scheda successiva mostra la query parametri stringa, se presente.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-119">The next tab shows the query string parameters, if any.</span></span>

![Parametri di stringa di query](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="0bb2a-121">Questa richiesta non dispone di tutti i cookie, ma in caso contrario, verranno visualizzate sul **cookie** scheda.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-121">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab.</span></span> <span data-ttu-id="0bb2a-122">È possibile visualizzare le intestazioni che sono state passate l'ultima scheda.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-122">You can see the headers that were passed in the last tab.</span></span>

![Intestazioni](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="0bb2a-124">Configurazione di un'eccezione personalizzata la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="0bb2a-124">Configuring a custom exception handling page</span></span>

<span data-ttu-id="0bb2a-125">È consigliabile configurare una pagina di gestore di eccezioni da utilizzare quando l'app non è in esecuzione `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-125">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

<span data-ttu-id="0bb2a-126">[!code-csharp[Principale](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span><span class="sxs-lookup"><span data-stu-id="0bb2a-126">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span></span>

<span data-ttu-id="0bb2a-127">In un'applicazione MVC, non in modo esplicito decorare il metodo di azione del gestore di errori con gli attributi del metodo HTTP, ad esempio `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-127">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="0bb2a-128">Utilizzando i verbi espliciti potrebbe impedire alcune richieste raggiungano il metodo.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-128">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="0bb2a-129">Configurazione delle tabelle codici di stato</span><span class="sxs-lookup"><span data-stu-id="0bb2a-129">Configuring status code pages</span></span>

<span data-ttu-id="0bb2a-130">Per impostazione predefinita, l'applicazione non fornirà una tabella codici di stato per i codici di stato HTTP, ad esempio 500 (errore Server interno) o 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="0bb2a-130">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="0bb2a-131">È possibile configurare il `StatusCodePagesMiddleware` aggiungendo una riga per il `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="0bb2a-131">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="0bb2a-132">Per impostazione predefinita, questo middleware aggiunge gestori di solo testo semplici per i codici di stato comuni, ad esempio 404:</span><span class="sxs-lookup"><span data-stu-id="0bb2a-132">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![pagina 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="0bb2a-134">Il middleware supporta diversi metodi di estensione differente.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-134">The middleware supports several different extension methods.</span></span> <span data-ttu-id="0bb2a-135">Uno accetta un'espressione lambda, un altro accetta una stringa di formato e tipo di contenuto.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-135">One takes a lambda expression, another takes a content type and format string.</span></span>

<span data-ttu-id="0bb2a-136">[!code-csharp[Principale](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span><span class="sxs-lookup"><span data-stu-id="0bb2a-136">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="0bb2a-137">Sono disponibili metodi di estensione di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-137">There are also redirect extension methods.</span></span> <span data-ttu-id="0bb2a-138">Uno invia al client un codice di 302 stato e uno restituisce al client il codice di stato originale, ma esegue anche il gestore per l'URL di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-138">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

<span data-ttu-id="0bb2a-139">[!code-csharp[Principale](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span><span class="sxs-lookup"><span data-stu-id="0bb2a-139">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="0bb2a-140">Se è necessario disabilitare i codici di stato per determinate richieste, è possibile farlo:</span><span class="sxs-lookup"><span data-stu-id="0bb2a-140">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="0bb2a-141">Codice di gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="0bb2a-141">Exception-handling code</span></span>

<span data-ttu-id="0bb2a-142">Codice in pagine di gestione delle eccezioni può generare eccezioni.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-142">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="0bb2a-143">È spesso consigliabile per le pagine di errore di produzione costituita da contenuto esclusivamente statico.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-143">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="0bb2a-144">Inoltre, tenere presente che dopo le intestazioni di risposta sono state inviate, è possibile modificare il codice di stato della risposta, né le pagine di eccezione o gestori di eseguire.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-144">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="0bb2a-145">La risposta deve essere completata o interrotta la connessione.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-145">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="0bb2a-146">Gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="0bb2a-146">Server exception handling</span></span>

<span data-ttu-id="0bb2a-147">Oltre alla logica dell'app, gestione delle eccezioni di [server](servers/index.md) ospita l'applicazione eseguirà alcune eccezioni.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-147">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app will perform some exception handling.</span></span> <span data-ttu-id="0bb2a-148">Se il server rileva un'eccezione prima che le intestazioni inviate che invia una risposta 500 Errore interno del Server senza il corpo.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-148">If the server catches an exception before the headers have been sent it sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="0bb2a-149">Se rileva un'eccezione dopo l'invio delle intestazioni, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-149">If it catches an exception after the headers have been sent, it closes the connection.</span></span> <span data-ttu-id="0bb2a-150">Le richieste che non vengono gestite dalla tua app verranno gestite dal server e qualsiasi eccezione si verifichi verrà gestiti dall'eccezione del server la gestione.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-150">Requests that are not handled by your app will be handled by the server, and any exception that occurs will be handled by the server's exception handling.</span></span> <span data-ttu-id="0bb2a-151">Le pagine di errore personalizzate o eccezioni middleware o è stato configurato per l'applicazione di filtri non influiranno questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-151">Any custom error pages or exception handling middleware or filters you have configured for your app will not affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="0bb2a-152">Gestione delle eccezioni di avvio</span><span class="sxs-lookup"><span data-stu-id="0bb2a-152">Startup exception handling</span></span>

<span data-ttu-id="0bb2a-153">Il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-153">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="0bb2a-154">Le eccezioni che si verificano durante l'avvio dell'app possono influire su comportamento del server.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-154">Exceptions that occur during app startup can impact server behavior.</span></span> <span data-ttu-id="0bb2a-155">Ad esempio, se si verifica un'eccezione prima di chiamare `KestrelServerOptions.UseHttps`, il livello di hosting intercetta l'eccezione, verrà avviato il server e visualizza una pagina di errore sulla porta non SSL.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-155">For example, if an exception happens before you call `KestrelServerOptions.UseHttps`, the hosting layer catches the exception, starts the server, and displays an error page on the non-SSL port.</span></span> <span data-ttu-id="0bb2a-156">Se si verifica un'eccezione dopo l'esecuzione di tale riga, la pagina di errore viene invece fornita tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-156">If an exception happens after that line executes, the error page is served over HTTPS instead.</span></span>

<span data-ttu-id="0bb2a-157">È possibile [configurare il comportamento in risposta agli errori dell'host durante l'avvio](hosting.md#configuring-a-host) utilizzando `CaptureStartupErrors` e `detailedErrors` chiave.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-157">You can [configure how the host will behave in response to errors during startup](hosting.md#configuring-a-host) using `CaptureStartupErrors` and the `detailedErrors` key.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="0bb2a-158">Gestione degli errori di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="0bb2a-158">ASP.NET MVC error handling</span></span>

<span data-ttu-id="0bb2a-159">[MVC](../mvc/index.md) App disponibili alcune opzioni aggiuntive per la gestione degli errori, ad esempio la configurazione dei filtri di eccezione e l'esecuzione di convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-159">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="0bb2a-160">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="0bb2a-160">Exception Filters</span></span>

<span data-ttu-id="0bb2a-161">I filtri eccezioni possono essere configurati a livello globale o in base al controller o per ogni azione in un'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-161">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="0bb2a-162">Questi filtri di gestire qualsiasi eccezione non gestita che si verifica durante l'esecuzione di un'azione del controller o un altro filtro e non vengono chiamati in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-162">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="0bb2a-163">Ulteriori informazioni sui filtri di eccezioni in [filtri](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="0bb2a-163">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="0bb2a-164">I filtri eccezioni sono ideali per l'intercettazione delle eccezioni che si verificano all'interno di azioni di MVC, ma non sono quanto più flessibili middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-164">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="0bb2a-165">Preferiti middleware nel caso generale e utilizzare i filtri in cui è necessario solo per eseguire la gestione degli errori *in modo diverso* in base a quale azione MVC è stato scelto.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-165">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="0bb2a-166">Lo stato del modello di gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="0bb2a-166">Handling Model State Errors</span></span>

<span data-ttu-id="0bb2a-167">[La convalida del modello](../mvc/models/validation.md) si verifica prima di ogni azione del controller richiamato ed è responsabilità del metodo di azione per controllare `ModelState.IsValid` e rispondere nel modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-167">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="0bb2a-168">Alcune applicazioni è possibile scegliere di seguire una convenzione standard per la gestione di errori di convalida del modello, nel qual caso un [filtro](../mvc/controllers/filters.md) può essere una posizione appropriata per implementare questi criteri.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-168">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="0bb2a-169">È consigliabile testare il comportamento delle azioni con gli stati di modello non valido.</span><span class="sxs-lookup"><span data-stu-id="0bb2a-169">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="0bb2a-170">Altre informazioni, vedere [logica del controller di test](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="0bb2a-170">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



