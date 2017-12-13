---
title: Registrazione ad alte prestazioni con LoggerMessage in ASP.NET Core
author: guardrex
description: "Informazioni su come usare le funzionalità di LoggerMessage per creare delegati memorizzabile nella cache che richiedono meno allocazioni degli oggetti rispetto ai metodi di estensione logger per scenari di registrazione ad alte prestazioni."
ms.author: riande
manager: wpickett
ms.date: 11/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: defba75c6c9ea13d24af4cd8515d82d9e7cf9853
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="1e114-103">Registrazione ad alte prestazioni con LoggerMessage in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e114-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="1e114-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1e114-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1e114-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) funzionalità creano delegati memorizzabile nella cache che richiedono un minor numero di allocazioni di oggetti e una riduzione del carico di elaborazione di [metodi di estensione logger](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), ad esempio `LogInformation`, `LogDebug`e `LogError`.</span><span class="sxs-lookup"><span data-stu-id="1e114-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="1e114-106">Per gli scenari di registrazione ad alte prestazioni, utilizzare il `LoggerMessage` modello.</span><span class="sxs-lookup"><span data-stu-id="1e114-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="1e114-107">`LoggerMessage`offre i vantaggi delle prestazioni seguenti sui metodi di estensione Logger:</span><span class="sxs-lookup"><span data-stu-id="1e114-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="1e114-108">Metodi di estensione logger richiedono tipi di valore "boxing" (conversione), ad esempio `int`, in `object`.</span><span class="sxs-lookup"><span data-stu-id="1e114-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="1e114-109">Il `LoggerMessage` modello evita la conversione boxing usando statico `Action` campi e metodi di estensione con parametri fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="1e114-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="1e114-110">Metodi di estensione logger necessario analizzare il modello di messaggio (stringa di formato denominata) ogni volta che viene scritto un messaggio di log.</span><span class="sxs-lookup"><span data-stu-id="1e114-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="1e114-111">`LoggerMessage`richiede solo l'analisi di un modello di una volta quando viene definito il messaggio.</span><span class="sxs-lookup"><span data-stu-id="1e114-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="1e114-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1e114-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1e114-113">Viene illustrato l'app di esempio `LoggerMessage` funzionalità a un'offerta di base di sistema di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="1e114-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="1e114-114">L'applicazione aggiunge ed Elimina le offerte utilizzando un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="1e114-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="1e114-115">Se si verificano queste operazioni, messaggi di log sono generati mediante la `LoggerMessage` modello.</span><span class="sxs-lookup"><span data-stu-id="1e114-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="1e114-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="1e114-116">LoggerMessage.Define</span></span>

<span data-ttu-id="1e114-117">[Definire (LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) crea un `Action` delegato per la registrazione di un messaggio.</span><span class="sxs-lookup"><span data-stu-id="1e114-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="1e114-118">`Define`gli overload consentono il passaggio di parametri di tipo fino a sei a una stringa di formato denominato (modello).</span><span class="sxs-lookup"><span data-stu-id="1e114-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

## <a name="loggermessagedefinescope"></a><span data-ttu-id="1e114-119">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="1e114-119">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="1e114-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) crea un `Func` delegato per la definizione di un [log ambito](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="1e114-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="1e114-121">`DefineScope`gli overload è possibile passare un massimo di tre parametri di tipo in una stringa di formato denominato (modello).</span><span class="sxs-lookup"><span data-stu-id="1e114-121">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

## <a name="message-template-named-format-string"></a><span data-ttu-id="1e114-122">Modello di messaggio (denominato stringa di formato)</span><span class="sxs-lookup"><span data-stu-id="1e114-122">Message template (named format string)</span></span>

<span data-ttu-id="1e114-123">La stringa fornita per il `Define` e `DefineScope` metodi è un modello e non una stringa interpolata.</span><span class="sxs-lookup"><span data-stu-id="1e114-123">The string provided to the `Define` and `DefineScope` methods is a template and not an interpolated string.</span></span> <span data-ttu-id="1e114-124">I segnaposti vengono sostituiti nell'ordine che siano specificati i tipi.</span><span class="sxs-lookup"><span data-stu-id="1e114-124">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="1e114-125">Nomi di segnaposto nel modello devono essere descrittivi e coerente tra modelli.</span><span class="sxs-lookup"><span data-stu-id="1e114-125">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="1e114-126">Vengono utilizzati come nomi di proprietà all'interno di dati strutturati di log.</span><span class="sxs-lookup"><span data-stu-id="1e114-126">They serve as property names within structured log data.</span></span> <span data-ttu-id="1e114-127">È consigliabile [Pascal maiuscole e minuscole](/dotnet/standard/design-guidelines/capitalization-conventions) per i nomi di segnaposto.</span><span class="sxs-lookup"><span data-stu-id="1e114-127">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="1e114-128">Ad esempio, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="1e114-128">For example, `{Count}`, `{FirstName}`.</span></span>

## <a name="implementing-loggermessagedefine"></a><span data-ttu-id="1e114-129">Implementazione LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="1e114-129">Implementing LoggerMessage.Define</span></span>

<span data-ttu-id="1e114-130">Ogni messaggio di log è un `Action` contenuti in un campo statico creato da `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="1e114-130">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="1e114-131">Ad esempio, l'app di esempio crea un campo per descrivere un messaggio di log per una richiesta GET per la pagina di indice (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1e114-131">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="1e114-132">Per il `Action`, specificare:</span><span class="sxs-lookup"><span data-stu-id="1e114-132">For the `Action`, specify:</span></span>

* <span data-ttu-id="1e114-133">Il livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="1e114-133">The log level.</span></span>
* <span data-ttu-id="1e114-134">Un identificatore dell'evento univoco ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) con il nome del metodo di estensione statici.</span><span class="sxs-lookup"><span data-stu-id="1e114-134">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="1e114-135">Il modello di messaggio (denominato stringa di formato).</span><span class="sxs-lookup"><span data-stu-id="1e114-135">The message template (named format string).</span></span> 

<span data-ttu-id="1e114-136">Una richiesta per la pagina di indice dei set di app di esempio di:</span><span class="sxs-lookup"><span data-stu-id="1e114-136">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="1e114-137">Accedere a livello di `Information`.</span><span class="sxs-lookup"><span data-stu-id="1e114-137">Log level to `Information`.</span></span>
* <span data-ttu-id="1e114-138">Id evento `1` con il nome del `IndexPageRequested` metodo.</span><span class="sxs-lookup"><span data-stu-id="1e114-138">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="1e114-139">Modello di messaggio (stringa di formato denominato) in una stringa.</span><span class="sxs-lookup"><span data-stu-id="1e114-139">Message template (named format string) to a string.</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="1e114-140">Registrazione strutturati archivi è possono utilizzare il nome dell'evento quando è stato fornito con l'id evento di arricchire la registrazione.</span><span class="sxs-lookup"><span data-stu-id="1e114-140">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="1e114-141">Ad esempio, [Serilog](https://github.com/serilog/serilog-extensions-logging) utilizza il nome dell'evento.</span><span class="sxs-lookup"><span data-stu-id="1e114-141">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="1e114-142">Il `Action` viene richiamato tramite un metodo di estensione fortemente tipizzata.</span><span class="sxs-lookup"><span data-stu-id="1e114-142">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="1e114-143">Il `IndexPageRequested` metodo registra un messaggio per una richiesta GET pagina di indice nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="1e114-143">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="1e114-144">`IndexPageRequested`viene chiamato sul logger nel `OnGetAsync` metodo *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1e114-144">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="1e114-145">Esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="1e114-145">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="1e114-146">Per passare parametri a un messaggio di log, è possibile definire fino a sei tipi quando si crea il campo statico.</span><span class="sxs-lookup"><span data-stu-id="1e114-146">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="1e114-147">L'app di esempio accede a una stringa durante l'aggiunta di un'offerta definendo un `string` tipo per il `Action` campo:</span><span class="sxs-lookup"><span data-stu-id="1e114-147">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="1e114-148">Modello di messaggio di log del delegato riceve i valori segnaposto dai tipi forniti.</span><span class="sxs-lookup"><span data-stu-id="1e114-148">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="1e114-149">L'app di esempio definisce un delegato per l'aggiunta di un'offerta dove il parametro offerta è un `string`:</span><span class="sxs-lookup"><span data-stu-id="1e114-149">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="1e114-150">Il metodo di estensione statici per l'aggiunta di un'offerta, `QuoteAdded`, riceve il valore dell'argomento offerta e lo passa al `Action` delegato:</span><span class="sxs-lookup"><span data-stu-id="1e114-150">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="1e114-151">Nel file code-behind della pagina di indice (*Pages/Index.cshtml.cs*), `QuoteAdded` viene chiamata per registrare il messaggio:</span><span class="sxs-lookup"><span data-stu-id="1e114-151">In the Index page's code-behind file (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="1e114-152">Esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="1e114-152">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="1e114-153">L'applicazione di esempio implementa un `try` &ndash; `catch` modello per l'eliminazione di offerta.</span><span class="sxs-lookup"><span data-stu-id="1e114-153">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="1e114-154">Per un'operazione di eliminazione ha esito positivo, viene registrato un messaggio informativo.</span><span class="sxs-lookup"><span data-stu-id="1e114-154">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="1e114-155">Un messaggio di errore viene registrato per un'operazione di eliminazione quando viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="1e114-155">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="1e114-156">Il messaggio di log per l'operazione di eliminazione l'esito negativo include la traccia dello stack dell'eccezione (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1e114-156">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="1e114-157">Si noti come l'eccezione viene passato al delegato in `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="1e114-157">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="1e114-158">Nell'indice pagina code-behind, l'eliminazione ha esito positivo offerta chiama il `QuoteDeleted` metodo il logger.</span><span class="sxs-lookup"><span data-stu-id="1e114-158">In the Index page code-behind, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="1e114-159">Quando non viene trovata un'offerta per l'eliminazione, un `ArgumentNullException` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="1e114-159">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="1e114-160">L'eccezione viene intercettato dal `try` &ndash; `catch` istruzione e registrato chiamando il `QuoteDeleteFailed` il logger nel metodo il `catch` blocco (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1e114-160">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="1e114-161">Quando viene eliminata correttamente una virgoletta, esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="1e114-161">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="1e114-162">Quando offerta eliminazione non riesce, esaminare l'output della console dell'app.</span><span class="sxs-lookup"><span data-stu-id="1e114-162">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="1e114-163">Si noti che l'eccezione è incluso nel messaggio di log:</span><span class="sxs-lookup"><span data-stu-id="1e114-163">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="implementing-loggermessagedefinescope"></a><span data-ttu-id="1e114-164">Implementazione LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="1e114-164">Implementing LoggerMessage.DefineScope</span></span>

<span data-ttu-id="1e114-165">Definire un [log ambito](xref:fundamentals/logging/index#log-scopes) da applicare a una serie di messaggi di log utilizzando il [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) metodo.</span><span class="sxs-lookup"><span data-stu-id="1e114-165">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="1e114-166">L'applicazione di esempio dispone di un **Cancella tutto** pulsante per l'eliminazione di tutte le virgolette nel database.</span><span class="sxs-lookup"><span data-stu-id="1e114-166">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="1e114-167">Le virgolette vengono eliminate, rimuoverle uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="1e114-167">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="1e114-168">Ogni volta che viene eliminata un'offerta, il `QuoteDeleted` metodo viene chiamato sul logger.</span><span class="sxs-lookup"><span data-stu-id="1e114-168">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="1e114-169">Un ambito di log viene aggiunto a questi messaggi di log.</span><span class="sxs-lookup"><span data-stu-id="1e114-169">A log scope is added to these log messages.</span></span>

<span data-ttu-id="1e114-170">Abilitare `IncludeScopes` nelle opzioni di logger di console:</span><span class="sxs-lookup"><span data-stu-id="1e114-170">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="1e114-171">Impostazione `IncludeScopes` nelle applicazioni ASP.NET Core 2.0 per abilitare gli ambiti di log è necessaria.</span><span class="sxs-lookup"><span data-stu-id="1e114-171">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="1e114-172">Impostazione `IncludeScopes` tramite *appsettings* i file di configurazione è una funzionalità che ha pianificato per la versione di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="1e114-172">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="1e114-173">L'app di esempio cancella altri provider e aggiunge i filtri per ridurre l'output di registrazione.</span><span class="sxs-lookup"><span data-stu-id="1e114-173">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="1e114-174">Questo rende più semplice visualizzare i messaggi di log dell'esempio che illustrano `LoggerMessage` funzionalità.</span><span class="sxs-lookup"><span data-stu-id="1e114-174">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="1e114-175">Per creare un ambito di log, aggiungere un campo per contenere un `Func` delegato per l'ambito.</span><span class="sxs-lookup"><span data-stu-id="1e114-175">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="1e114-176">L'app di esempio crea un campo denominato `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1e114-176">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="1e114-177">Utilizzare `DefineScope` per creare il delegato.</span><span class="sxs-lookup"><span data-stu-id="1e114-177">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="1e114-178">Fino a tre tipi possono essere specificati per l'utilizzo come argomenti di modello quando viene richiamato il delegato.</span><span class="sxs-lookup"><span data-stu-id="1e114-178">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="1e114-179">L'app di esempio utilizza un modello di messaggio che include il numero di virgolette eliminate (un `int` tipo):</span><span class="sxs-lookup"><span data-stu-id="1e114-179">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="1e114-180">Fornire un metodo di estensione statici per il messaggio di log.</span><span class="sxs-lookup"><span data-stu-id="1e114-180">Provide a static extension method for the log message.</span></span> <span data-ttu-id="1e114-181">Includere i parametri di tipo per le proprietà denominate che vengono visualizzati nel modello di messaggio.</span><span class="sxs-lookup"><span data-stu-id="1e114-181">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="1e114-182">L'app di esempio accetta un `count` di offerta da eliminare e restituisce `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="1e114-182">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="1e114-183">Esegue il wrapping di ambito chiama l'estensione di registrazione in un `using` blocco:</span><span class="sxs-lookup"><span data-stu-id="1e114-183">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="1e114-184">Esaminare i messaggi di log di output della console dell'app.</span><span class="sxs-lookup"><span data-stu-id="1e114-184">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="1e114-185">Il risultato seguente mostra tre offerte eliminate con il messaggio di ambito di log incluso:</span><span class="sxs-lookup"><span data-stu-id="1e114-185">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="see-also"></a><span data-ttu-id="1e114-186">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="1e114-186">See also</span></span>

* [<span data-ttu-id="1e114-187">Registrazione</span><span class="sxs-lookup"><span data-stu-id="1e114-187">Logging</span></span>](xref:fundamentals/logging/index)
