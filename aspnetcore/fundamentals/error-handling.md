---
title: Gestione degli errori in ASP.NET Core
author: ardalis
description: Viene illustrato come gestire gli errori nelle applicazioni ASP.NET Core
keywords: ASP.NET Core, gestione degli errori, la gestione delle eccezioni
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 93f0724dbe98316e2b5a0af0ac1760c3aac2f1d0
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="d9f08-104">Introduzione alla gestione degli errori in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9f08-104">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="d9f08-105">Da [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="d9f08-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="d9f08-106">Questo articolo descrive appoaches comuni di gestione degli errori nelle applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d9f08-106">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="d9f08-107">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="d9f08-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a><span data-ttu-id="d9f08-108">La pagina di eccezione per sviluppatori</span><span class="sxs-lookup"><span data-stu-id="d9f08-108">The developer exception page</span></span>

<span data-ttu-id="d9f08-109">Per configurare un'app per visualizzare una pagina che visualizza informazioni dettagliate sulle eccezioni, installare il `Microsoft.AspNetCore.Diagnostics` NuGet pacchetto e aggiungere una riga per il [configurare metodo nella classe di avvio](startup.md):</span><span class="sxs-lookup"><span data-stu-id="d9f08-109">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="d9f08-110">Inserire `UseDeveloperExceptionPage` prima middleware che si desidera intercettare le eccezioni, ad esempio `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="d9f08-110">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="d9f08-111">Abilitare la pagina di eccezione developer **solo quando l'app è in esecuzione nell'ambiente di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="d9f08-111">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="d9f08-112">Non si desidera condividere informazioni dettagliate sulle eccezioni pubblicamente quando viene eseguita l'app nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="d9f08-112">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="d9f08-113">[Ulteriori informazioni sulla configurazione di ambienti](environments.md).</span><span class="sxs-lookup"><span data-stu-id="d9f08-113">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="d9f08-114">Per visualizzare la pagina di eccezione per sviluppatori, eseguire l'applicazione di esempio con l'ambiente impostato su `Development`e aggiungere `?throw=true` all'URL di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="d9f08-114">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="d9f08-115">La pagina include diverse schede con le informazioni sull'eccezione e la richiesta.</span><span class="sxs-lookup"><span data-stu-id="d9f08-115">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="d9f08-116">La prima scheda include un'analisi dello stack.</span><span class="sxs-lookup"><span data-stu-id="d9f08-116">The first tab includes a stack trace.</span></span> 

![Analisi dello stack](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="d9f08-118">La scheda successiva mostra la query parametri stringa, se presente.</span><span class="sxs-lookup"><span data-stu-id="d9f08-118">The next tab shows the query string parameters, if any.</span></span>

![Parametri di stringa di query](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="d9f08-120">Questa richiesta non dispone di tutti i cookie, ma in caso contrario, verranno visualizzate sul **cookie** scheda. È possibile visualizzare le intestazioni che sono state passate l'ultima scheda.</span><span class="sxs-lookup"><span data-stu-id="d9f08-120">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Intestazioni](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="d9f08-122">Configurazione di un'eccezione personalizzata la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="d9f08-122">Configuring a custom exception handling page</span></span>

<span data-ttu-id="d9f08-123">È consigliabile configurare una pagina di gestore di eccezioni da utilizzare quando l'app non è in esecuzione `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="d9f08-123">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="d9f08-124">In un'applicazione MVC, non in modo esplicito decorare il metodo di azione del gestore di errori con gli attributi del metodo HTTP, ad esempio `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="d9f08-124">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="d9f08-125">Utilizzando i verbi espliciti potrebbe impedire alcune richieste raggiungano il metodo.</span><span class="sxs-lookup"><span data-stu-id="d9f08-125">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="d9f08-126">Configurazione delle tabelle codici di stato</span><span class="sxs-lookup"><span data-stu-id="d9f08-126">Configuring status code pages</span></span>

<span data-ttu-id="d9f08-127">Per impostazione predefinita, l'applicazione non fornirà una tabella codici di stato per i codici di stato HTTP, ad esempio 500 (errore Server interno) o 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="d9f08-127">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="d9f08-128">È possibile configurare il `StatusCodePagesMiddleware` aggiungendo una riga per il `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="d9f08-128">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="d9f08-129">Per impostazione predefinita, questo middleware aggiunge gestori di solo testo semplici per i codici di stato comuni, ad esempio 404:</span><span class="sxs-lookup"><span data-stu-id="d9f08-129">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![pagina 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="d9f08-131">Il middleware supporta diversi metodi di estensione differente.</span><span class="sxs-lookup"><span data-stu-id="d9f08-131">The middleware supports several different extension methods.</span></span> <span data-ttu-id="d9f08-132">Uno accetta un'espressione lambda, un altro accetta una stringa di formato e tipo di contenuto.</span><span class="sxs-lookup"><span data-stu-id="d9f08-132">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="d9f08-133">Sono disponibili metodi di estensione di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="d9f08-133">There are also redirect extension methods.</span></span> <span data-ttu-id="d9f08-134">Uno invia al client un codice di 302 stato e uno restituisce al client il codice di stato originale, ma esegue anche il gestore per l'URL di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="d9f08-134">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="d9f08-135">Se è necessario disabilitare i codici di stato per determinate richieste, è possibile farlo:</span><span class="sxs-lookup"><span data-stu-id="d9f08-135">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="d9f08-136">Codice di gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="d9f08-136">Exception-handling code</span></span>

<span data-ttu-id="d9f08-137">Codice in pagine di gestione delle eccezioni può generare eccezioni.</span><span class="sxs-lookup"><span data-stu-id="d9f08-137">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="d9f08-138">È spesso consigliabile per le pagine di errore di produzione costituita da contenuto esclusivamente statico.</span><span class="sxs-lookup"><span data-stu-id="d9f08-138">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="d9f08-139">Inoltre, tenere presente che dopo le intestazioni di risposta sono state inviate, è possibile modificare il codice di stato della risposta, né le pagine di eccezione o gestori di eseguire.</span><span class="sxs-lookup"><span data-stu-id="d9f08-139">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="d9f08-140">La risposta deve essere completata o interrotta la connessione.</span><span class="sxs-lookup"><span data-stu-id="d9f08-140">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="d9f08-141">Gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="d9f08-141">Server exception handling</span></span>

<span data-ttu-id="d9f08-142">Oltre alla logica dell'app, gestione delle eccezioni di [server](servers/index.md) ospita l'app esegue alcune eccezioni.</span><span class="sxs-lookup"><span data-stu-id="d9f08-142">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="d9f08-143">Se il server rileva un'eccezione prima che le intestazioni vengono inviate, il server invia una risposta 500 Errore interno del Server senza il corpo.</span><span class="sxs-lookup"><span data-stu-id="d9f08-143">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="d9f08-144">Se il server memorizza nella cache dopo l'invio delle intestazioni di un'eccezione, il server chiude la connessione.</span><span class="sxs-lookup"><span data-stu-id="d9f08-144">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="d9f08-145">Le richieste che non sono gestite dalla tua app vengono gestite dal server.</span><span class="sxs-lookup"><span data-stu-id="d9f08-145">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="d9f08-146">Qualsiasi eccezione che si verifica viene gestita dall'eccezione del server la gestione.</span><span class="sxs-lookup"><span data-stu-id="d9f08-146">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="d9f08-147">Qualsiasi configurato pagine di errore personalizzate o middleware di gestione delle eccezioni o i filtri non influisce su tale comportamento.</span><span class="sxs-lookup"><span data-stu-id="d9f08-147">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="d9f08-148">Gestione delle eccezioni di avvio</span><span class="sxs-lookup"><span data-stu-id="d9f08-148">Startup exception handling</span></span>

<span data-ttu-id="d9f08-149">Il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="d9f08-149">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="d9f08-150">È possibile [configurare il comportamento dell'host in risposta agli errori durante l'avvio](hosting.md#detailed-errors) utilizzando `captureStartupErrors` e `detailedErrors` chiave.</span><span class="sxs-lookup"><span data-stu-id="d9f08-150">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="d9f08-151">Hosting può mostrare solo una pagina di errore per un errore di avvio acquisita se l'errore si verifica dopo host binding di porta e indirizzo.</span><span class="sxs-lookup"><span data-stu-id="d9f08-151">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="d9f08-152">Se un'associazione non riesce per qualsiasi motivo, il livello di hosting registra un'eccezione critica, l'arresto del processo dotnet, e non viene visualizzata alcuna pagina di errore.</span><span class="sxs-lookup"><span data-stu-id="d9f08-152">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="d9f08-153">Gestione degli errori di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d9f08-153">ASP.NET MVC error handling</span></span>

<span data-ttu-id="d9f08-154">[MVC](../mvc/index.md) App disponibili alcune opzioni aggiuntive per la gestione degli errori, ad esempio la configurazione dei filtri di eccezione e l'esecuzione di convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="d9f08-154">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="d9f08-155">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="d9f08-155">Exception Filters</span></span>

<span data-ttu-id="d9f08-156">I filtri eccezioni possono essere configurati a livello globale o in base al controller o per ogni azione in un'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="d9f08-156">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="d9f08-157">Questi filtri di gestire qualsiasi eccezione non gestita che si verifica durante l'esecuzione di un'azione del controller o un altro filtro e non vengono chiamati in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="d9f08-157">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="d9f08-158">Ulteriori informazioni sui filtri di eccezioni in [filtri](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="d9f08-158">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="d9f08-159">I filtri eccezioni sono ideali per l'intercettazione delle eccezioni che si verificano all'interno di azioni di MVC, ma non sono quanto più flessibili middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="d9f08-159">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="d9f08-160">Preferiti middleware nel caso generale e utilizzare i filtri in cui è necessario solo per eseguire la gestione degli errori *in modo diverso* in base a quale azione MVC è stato scelto.</span><span class="sxs-lookup"><span data-stu-id="d9f08-160">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="d9f08-161">Lo stato del modello di gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="d9f08-161">Handling Model State Errors</span></span>

<span data-ttu-id="d9f08-162">[La convalida del modello](../mvc/models/validation.md) si verifica prima di ogni azione del controller richiamato ed è responsabilità del metodo di azione per controllare `ModelState.IsValid` e rispondere nel modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="d9f08-162">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="d9f08-163">Alcune applicazioni è possibile scegliere di seguire una convenzione standard per la gestione di errori di convalida del modello, nel qual caso un [filtro](../mvc/controllers/filters.md) può essere una posizione appropriata per implementare questi criteri.</span><span class="sxs-lookup"><span data-stu-id="d9f08-163">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="d9f08-164">È consigliabile testare il comportamento delle azioni con gli stati di modello non valido.</span><span class="sxs-lookup"><span data-stu-id="d9f08-164">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="d9f08-165">Altre informazioni, vedere [logica del controller di test](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="d9f08-165">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



