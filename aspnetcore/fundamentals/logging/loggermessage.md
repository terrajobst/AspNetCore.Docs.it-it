---
title: Registrazione a prestazioni elevate con LoggerMessage in ASP.NET Core
author: guardrex
description: Informazioni su come usare LoggerMessage per creare delegati inseribili nella cache che richiedono un numero minore di allocazioni di oggetti per scenari di registrazione a prestazioni elevate.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/26/2019
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 56c60fe405660ff39e2696de591449c25f669de2
ms.sourcegitcommit: 0774a61a3a6c1412a7da0e7d932dc60c506441fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2019
ms.locfileid: "70059043"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="00b4c-103">Registrazione a prestazioni elevate con LoggerMessage in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00b4c-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="00b4c-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="00b4c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="00b4c-105">Le funzionalità di <xref:Microsoft.Extensions.Logging.LoggerMessage> creano delegati inseribili nella cache che richiedono un numero minore di allocazioni di oggetti e riducono il sovraccarico di calcolo rispetto ai [metodi di estensione del logger](xref:Microsoft.Extensions.Logging.LoggerExtensions), ad esempio <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> e <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-105"><xref:Microsoft.Extensions.Logging.LoggerMessage> features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](xref:Microsoft.Extensions.Logging.LoggerExtensions), such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span></span> <span data-ttu-id="00b4c-106">Per gli scenari di registrazione a prestazioni elevate, usare <xref:Microsoft.Extensions.Logging.LoggerMessage>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-106">For high-performance logging scenarios, use the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

<span data-ttu-id="00b4c-107"><xref:Microsoft.Extensions.Logging.LoggerMessage> offre i seguenti vantaggi in termini di prestazioni rispetto ai metodi di estensione del logger:</span><span class="sxs-lookup"><span data-stu-id="00b4c-107"><xref:Microsoft.Extensions.Logging.LoggerMessage> provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="00b4c-108">I metodi di estensione del logger richiedono una "conversione boxing" dei tipi di valori, ad esempio `int`, in `object`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="00b4c-109"><xref:Microsoft.Extensions.Logging.LoggerMessage> evita la conversione boxing usando campi <xref:System.Action> statici e metodi di estensione con parametri fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="00b4c-109">The <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern avoids boxing by using static <xref:System.Action> fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="00b4c-110">I metodi di estensione del logger devono analizzare il modello di messaggio (stringa di formato denominata) ogni volta che viene scritto un messaggio del log.</span><span class="sxs-lookup"><span data-stu-id="00b4c-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="00b4c-111">Solo <xref:Microsoft.Extensions.Logging.LoggerMessage> richiede una sola analisi del modello durante la definizione del messaggio.</span><span class="sxs-lookup"><span data-stu-id="00b4c-111"><xref:Microsoft.Extensions.Logging.LoggerMessage> only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="00b4c-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="00b4c-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="00b4c-113">L'app di esempio illustra le funzionalità di <xref:Microsoft.Extensions.Logging.LoggerMessage> con un sistema di verifica delle offerte di base.</span><span class="sxs-lookup"><span data-stu-id="00b4c-113">The sample app demonstrates <xref:Microsoft.Extensions.Logging.LoggerMessage> features with a basic quote tracking system.</span></span> <span data-ttu-id="00b4c-114">L'app aggiunge ed elimina le offerte usando un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="00b4c-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="00b4c-115">Durante l'esecuzione di queste operazioni, i messaggi del log vengono generati usando <xref:Microsoft.Extensions.Logging.LoggerMessage>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-115">As these operations occur, log messages are generated using the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="00b4c-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="00b4c-116">LoggerMessage.Define</span></span>

<span data-ttu-id="00b4c-117">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) crea un delegato <xref:System.Action> per la registrazione di un messaggio.</span><span class="sxs-lookup"><span data-stu-id="00b4c-117">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) creates an <xref:System.Action> delegate for logging a message.</span></span> <span data-ttu-id="00b4c-118">Gli overload <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> permettono il passaggio di un massimo di sei parametri di tipo in una stringa di formato denominata (modello).</span><span class="sxs-lookup"><span data-stu-id="00b4c-118"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="00b4c-119">La stringa specificata nel metodo <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> è un modello e non una stringa interpolata.</span><span class="sxs-lookup"><span data-stu-id="00b4c-119">The string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="00b4c-120">I segnaposto vengono inseriti nell'ordine in cui sono specificati i tipi.</span><span class="sxs-lookup"><span data-stu-id="00b4c-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="00b4c-121">I nomi dei segnaposto nel modello devono essere descrittivi e coerenti tra i modelli.</span><span class="sxs-lookup"><span data-stu-id="00b4c-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="00b4c-122">Vengono usati come nomi di proprietà all'interno dei dati di log strutturati.</span><span class="sxs-lookup"><span data-stu-id="00b4c-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="00b4c-123">Per i nomi dei segnaposto è consigliabile usare la [convenzione Pascal](/dotnet/standard/design-guidelines/capitalization-conventions).</span><span class="sxs-lookup"><span data-stu-id="00b4c-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="00b4c-124">Ad esempio, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="00b4c-125">Ogni messaggio di log è un elemento <xref:System.Action> contenuto in un campo statico creato da [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span><span class="sxs-lookup"><span data-stu-id="00b4c-125">Each log message is an <xref:System.Action> held in a static field created by [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span></span> <span data-ttu-id="00b4c-126">Ad esempio, l'app di esempio crea un campo per descrivere un messaggio di log per una richiesta GET per la pagina di indice (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="00b4c-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="00b4c-127">Per <xref:System.Action>, specificare:</span><span class="sxs-lookup"><span data-stu-id="00b4c-127">For the <xref:System.Action>, specify:</span></span>

* <span data-ttu-id="00b4c-128">Il livello del log.</span><span class="sxs-lookup"><span data-stu-id="00b4c-128">The log level.</span></span>
* <span data-ttu-id="00b4c-129">Un identificatore di evento univoco (<xref:Microsoft.Extensions.Logging.EventId>) con il nome del metodo di estensione statico.</span><span class="sxs-lookup"><span data-stu-id="00b4c-129">A unique event identifier (<xref:Microsoft.Extensions.Logging.EventId>) with the name of the static extension method.</span></span>
* <span data-ttu-id="00b4c-130">Il modello di messaggio (stringa di formato denominata).</span><span class="sxs-lookup"><span data-stu-id="00b4c-130">The message template (named format string).</span></span> 

<span data-ttu-id="00b4c-131">Una richiesta per la pagina di indice dell'app di esempio imposta:</span><span class="sxs-lookup"><span data-stu-id="00b4c-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="00b4c-132">Il livello del log su `Information`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-132">Log level to `Information`.</span></span>
* <span data-ttu-id="00b4c-133">L'ID evento su `1` con il nome del metodo `IndexPageRequested`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="00b4c-134">Il modello di messaggio (stringa di formato denominata) su una stringa.</span><span class="sxs-lookup"><span data-stu-id="00b4c-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="00b4c-135">Negli archivi di log strutturati è possibile che venga usato il nome evento quando viene specificato con l'ID evento per arricchire la registrazione.</span><span class="sxs-lookup"><span data-stu-id="00b4c-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="00b4c-136">Ad esempio, [Serilog](https://github.com/serilog/serilog-extensions-logging) usa il nome evento.</span><span class="sxs-lookup"><span data-stu-id="00b4c-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="00b4c-137"><xref:System.Action> viene richiamato attraverso un metodo di estensione fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="00b4c-137">The <xref:System.Action> is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="00b4c-138">Il metodo `IndexPageRequested` registra un messaggio per la richiesta GET di una pagina di indice nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="00b4c-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="00b4c-139">`IndexPageRequested` viene chiamato nel logger nel metodo `OnGetAsync` in *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="00b4c-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="00b4c-140">Esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="00b4c-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="00b4c-141">Per passare i parametri in un messaggio di log, definire un massimo di sei tipi durante la creazione del campo statico.</span><span class="sxs-lookup"><span data-stu-id="00b4c-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="00b4c-142">L'app di esempio registra una stringa quando viene aggiunta un'offerta definendo un tipo `string` per il campo <xref:System.Action>:</span><span class="sxs-lookup"><span data-stu-id="00b4c-142">The sample app logs a string when adding a quote by defining a `string` type for the <xref:System.Action> field:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="00b4c-143">Il modello di messaggio di log del delegato riceve i valori dei segnaposto dai tipi specificati.</span><span class="sxs-lookup"><span data-stu-id="00b4c-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="00b4c-144">L'app di esempio definisce un delegato per l'aggiunta di un'offerta dove il parametro dell'offerta è un elemento `string`:</span><span class="sxs-lookup"><span data-stu-id="00b4c-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="00b4c-145">Il metodo di estensione statico per l'aggiunta di un'offerta, `QuoteAdded`, riceve il valore dell'argomento dell'offerta e lo passa al delegato <xref:System.Action>:</span><span class="sxs-lookup"><span data-stu-id="00b4c-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the <xref:System.Action> delegate:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="00b4c-146">Nel modello della pagina di indice (*Pages/Index.cshtml.cs*) viene chiamato `QuoteAdded` per registrare il messaggio:</span><span class="sxs-lookup"><span data-stu-id="00b4c-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="00b4c-147">Esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="00b4c-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="00b4c-148">L'app di esempio implementa un modello [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) per l'eliminazione dell'offerta.</span><span class="sxs-lookup"><span data-stu-id="00b4c-148">The sample app implements a [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) pattern for quote deletion.</span></span> <span data-ttu-id="00b4c-149">Per un'operazione di eliminazione con esito positivo, viene registrato un messaggio informativo.</span><span class="sxs-lookup"><span data-stu-id="00b4c-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="00b4c-150">Per un'operazione di eliminazione con la generazione di un'eccezione, viene registrato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="00b4c-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="00b4c-151">Il messaggio di log per un'operazione di eliminazione con esito negativo include l'analisi dello stack dell'eccezione (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="00b4c-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="00b4c-152">Si noti come l'eccezione viene passata al delegato in `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="00b4c-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="00b4c-153">Nel modello di pagina per la pagina di indice l'eliminazione di un'offerta con esito positivo chiama il metodo `QuoteDeleted` nel logger.</span><span class="sxs-lookup"><span data-stu-id="00b4c-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="00b4c-154">Quando non viene trovata un'offerta per l'eliminazione, viene generata un'eccezione <xref:System.ArgumentNullException>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-154">When a quote isn't found for deletion, an <xref:System.ArgumentNullException> is thrown.</span></span> <span data-ttu-id="00b4c-155">L'eccezione viene intercettata dall'istruzione [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) e registrata chiamando il metodo `QuoteDeleteFailed` nel logger nel blocco [catch](/dotnet/csharp/language-reference/keywords/try-catch) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="00b4c-155">The exception is trapped by the [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) statement and logged by calling the `QuoteDeleteFailed` method on the logger in the [catch](/dotnet/csharp/language-reference/keywords/try-catch) block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=9,13)]

<span data-ttu-id="00b4c-156">Quando un'offerta viene eliminata, esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="00b4c-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="00b4c-157">Quando l'eliminazione di un'offerta ha esito negativo, esaminare l'output della console dell'app.</span><span class="sxs-lookup"><span data-stu-id="00b4c-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="00b4c-158">Si noti che l'eccezione è inclusa nel messaggio di log:</span><span class="sxs-lookup"><span data-stu-id="00b4c-158">Note that the exception is included in the log message:</span></span>

```console
LoggerMessageSample.Pages.IndexModel: Error: Quote delete failed (Id = 999)

System.NullReferenceException: Object reference not set to an instance of an object.
   at lambda_method(Closure , ValueBuffer )
   at System.Linq.Enumerable.SelectEnumerableIterator`2.MoveNext()
   at Microsoft.EntityFrameworkCore.InMemory.Query.Internal.InMemoryShapedQueryCompilingExpressionVisitor.AsyncQueryingEnumerable`1.AsyncEnumerator.MoveNextAsync()
   at Microsoft.EntityFrameworkCore.Query.ShapedQueryCompilingExpressionVisitor.SingleOrDefaultAsync[TSource](IAsyncEnumerable`1 asyncEnumerable, CancellationToken cancellationToken)
   at Microsoft.EntityFrameworkCore.Query.ShapedQueryCompilingExpressionVisitor.SingleOrDefaultAsync[TSource](IAsyncEnumerable`1 asyncEnumerable, CancellationToken cancellationToken)
   at LoggerMessageSample.Pages.IndexModel.OnPostDeleteQuoteAsync(Int32 id) in c:\Users\guard\Documents\GitHub\Docs\aspnetcore\fundamentals\logging\loggermessage\samples\3.x\LoggerMessageSample\Pages\Index.cshtml.cs:line 77
```

## <a name="loggermessagedefinescope"></a><span data-ttu-id="00b4c-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="00b4c-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="00b4c-160">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) crea un delegato <xref:System.Func%601> per definire un [ambito del log](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="00b4c-160">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) creates a <xref:System.Func%601> delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="00b4c-161">Gli overload <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> permettono il passaggio di un massimo di tre parametri di tipo in una stringa di formato denominata (modello).</span><span class="sxs-lookup"><span data-stu-id="00b4c-161"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="00b4c-162">Come avviene per il metodo <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*>, la stringa specificata nel metodo <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> è un modello e non una stringa interpolata.</span><span class="sxs-lookup"><span data-stu-id="00b4c-162">As is the case with the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method, the string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="00b4c-163">I segnaposto vengono inseriti nell'ordine in cui sono specificati i tipi.</span><span class="sxs-lookup"><span data-stu-id="00b4c-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="00b4c-164">I nomi dei segnaposto nel modello devono essere descrittivi e coerenti tra i modelli.</span><span class="sxs-lookup"><span data-stu-id="00b4c-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="00b4c-165">Vengono usati come nomi di proprietà all'interno dei dati di log strutturati.</span><span class="sxs-lookup"><span data-stu-id="00b4c-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="00b4c-166">Per i nomi dei segnaposto è consigliabile usare la [convenzione Pascal](/dotnet/standard/design-guidelines/capitalization-conventions).</span><span class="sxs-lookup"><span data-stu-id="00b4c-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="00b4c-167">Ad esempio, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="00b4c-168">Definire un [ambito del log](xref:fundamentals/logging/index#log-scopes) da applicare a una serie di messaggi di log usando il metodo <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method.</span></span>

<span data-ttu-id="00b4c-169">L'app di esempio include un pulsante **Clear All** (Cancella tutto) per eliminare tutte le offerte nel database.</span><span class="sxs-lookup"><span data-stu-id="00b4c-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="00b4c-170">Le offerte vengono eliminate rimuovendole una alla volta.</span><span class="sxs-lookup"><span data-stu-id="00b4c-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="00b4c-171">Ogni volta che viene eliminata un'offerta, viene chiamato il metodo `QuoteDeleted` nel logger.</span><span class="sxs-lookup"><span data-stu-id="00b4c-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="00b4c-172">Viene aggiunto un ambito di log a questi messaggi di log.</span><span class="sxs-lookup"><span data-stu-id="00b4c-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="00b4c-173">Abilitare `IncludeScopes` nella sezione del logger di console di *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="00b4c-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

<span data-ttu-id="00b4c-174">Per creare un ambito di log, aggiungere un campo per inserire un delegato <xref:System.Func%601> per l'ambito.</span><span class="sxs-lookup"><span data-stu-id="00b4c-174">To create a log scope, add a field to hold a <xref:System.Func%601> delegate for the scope.</span></span> <span data-ttu-id="00b4c-175">L'app di esempio crea un campo denominato `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="00b4c-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="00b4c-176">Usare <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> per creare il delegato.</span><span class="sxs-lookup"><span data-stu-id="00b4c-176">Use <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> to create the delegate.</span></span> <span data-ttu-id="00b4c-177">È possibile specificare un massimo di tre tipi da usare come argomenti di modello quando viene richiamato il delegato.</span><span class="sxs-lookup"><span data-stu-id="00b4c-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="00b4c-178">L'app di esempio usa un modello di messaggio che include il numero di offerte eliminate (un tipo `int`):</span><span class="sxs-lookup"><span data-stu-id="00b4c-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="00b4c-179">Specificare un metodo di estensione statico per il messaggio di log.</span><span class="sxs-lookup"><span data-stu-id="00b4c-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="00b4c-180">Includere i parametri di tipo per le proprietà denominate visualizzate nel modello di messaggio.</span><span class="sxs-lookup"><span data-stu-id="00b4c-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="00b4c-181">L'app di esempio accetta un numero `count` di offerte da eliminare e restituisce `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="00b4c-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="00b4c-182">L'ambito esegue il wrapping delle chiamate di estensione di registrazione in un blocco [using](/dotnet/csharp/language-reference/keywords/using-statement):</span><span class="sxs-lookup"><span data-stu-id="00b4c-182">The scope wraps the logging extension calls in a [using](/dotnet/csharp/language-reference/keywords/using-statement) block:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="00b4c-183">Esaminare i messaggi di log nell'output della console dell'app.</span><span class="sxs-lookup"><span data-stu-id="00b4c-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="00b4c-184">Il risultato seguente mostra tre offerte eliminate con il messaggio di ambito di log incluso:</span><span class="sxs-lookup"><span data-stu-id="00b4c-184">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="00b4c-185">Le funzionalità di <xref:Microsoft.Extensions.Logging.LoggerMessage> creano delegati inseribili nella cache che richiedono un numero minore di allocazioni di oggetti e riducono il sovraccarico di calcolo rispetto ai [metodi di estensione del logger](xref:Microsoft.Extensions.Logging.LoggerExtensions), ad esempio <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> e <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-185"><xref:Microsoft.Extensions.Logging.LoggerMessage> features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](xref:Microsoft.Extensions.Logging.LoggerExtensions), such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span></span> <span data-ttu-id="00b4c-186">Per gli scenari di registrazione a prestazioni elevate, usare <xref:Microsoft.Extensions.Logging.LoggerMessage>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-186">For high-performance logging scenarios, use the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

<span data-ttu-id="00b4c-187"><xref:Microsoft.Extensions.Logging.LoggerMessage> offre i seguenti vantaggi in termini di prestazioni rispetto ai metodi di estensione del logger:</span><span class="sxs-lookup"><span data-stu-id="00b4c-187"><xref:Microsoft.Extensions.Logging.LoggerMessage> provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="00b4c-188">I metodi di estensione del logger richiedono una "conversione boxing" dei tipi di valori, ad esempio `int`, in `object`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-188">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="00b4c-189"><xref:Microsoft.Extensions.Logging.LoggerMessage> evita la conversione boxing usando campi <xref:System.Action> statici e metodi di estensione con parametri fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="00b4c-189">The <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern avoids boxing by using static <xref:System.Action> fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="00b4c-190">I metodi di estensione del logger devono analizzare il modello di messaggio (stringa di formato denominata) ogni volta che viene scritto un messaggio del log.</span><span class="sxs-lookup"><span data-stu-id="00b4c-190">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="00b4c-191">Solo <xref:Microsoft.Extensions.Logging.LoggerMessage> richiede una sola analisi del modello durante la definizione del messaggio.</span><span class="sxs-lookup"><span data-stu-id="00b4c-191"><xref:Microsoft.Extensions.Logging.LoggerMessage> only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="00b4c-192">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="00b4c-192">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="00b4c-193">L'app di esempio illustra le funzionalità di <xref:Microsoft.Extensions.Logging.LoggerMessage> con un sistema di verifica delle offerte di base.</span><span class="sxs-lookup"><span data-stu-id="00b4c-193">The sample app demonstrates <xref:Microsoft.Extensions.Logging.LoggerMessage> features with a basic quote tracking system.</span></span> <span data-ttu-id="00b4c-194">L'app aggiunge ed elimina le offerte usando un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="00b4c-194">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="00b4c-195">Durante l'esecuzione di queste operazioni, i messaggi del log vengono generati usando <xref:Microsoft.Extensions.Logging.LoggerMessage>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-195">As these operations occur, log messages are generated using the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="00b4c-196">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="00b4c-196">LoggerMessage.Define</span></span>

<span data-ttu-id="00b4c-197">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) crea un delegato <xref:System.Action> per la registrazione di un messaggio.</span><span class="sxs-lookup"><span data-stu-id="00b4c-197">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) creates an <xref:System.Action> delegate for logging a message.</span></span> <span data-ttu-id="00b4c-198">Gli overload <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> permettono il passaggio di un massimo di sei parametri di tipo in una stringa di formato denominata (modello).</span><span class="sxs-lookup"><span data-stu-id="00b4c-198"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="00b4c-199">La stringa specificata nel metodo <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> è un modello e non una stringa interpolata.</span><span class="sxs-lookup"><span data-stu-id="00b4c-199">The string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="00b4c-200">I segnaposto vengono inseriti nell'ordine in cui sono specificati i tipi.</span><span class="sxs-lookup"><span data-stu-id="00b4c-200">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="00b4c-201">I nomi dei segnaposto nel modello devono essere descrittivi e coerenti tra i modelli.</span><span class="sxs-lookup"><span data-stu-id="00b4c-201">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="00b4c-202">Vengono usati come nomi di proprietà all'interno dei dati di log strutturati.</span><span class="sxs-lookup"><span data-stu-id="00b4c-202">They serve as property names within structured log data.</span></span> <span data-ttu-id="00b4c-203">Per i nomi dei segnaposto è consigliabile usare la [convenzione Pascal](/dotnet/standard/design-guidelines/capitalization-conventions).</span><span class="sxs-lookup"><span data-stu-id="00b4c-203">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="00b4c-204">Ad esempio, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-204">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="00b4c-205">Ogni messaggio di log è un elemento <xref:System.Action> contenuto in un campo statico creato da [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span><span class="sxs-lookup"><span data-stu-id="00b4c-205">Each log message is an <xref:System.Action> held in a static field created by [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span></span> <span data-ttu-id="00b4c-206">Ad esempio, l'app di esempio crea un campo per descrivere un messaggio di log per una richiesta GET per la pagina di indice (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="00b4c-206">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="00b4c-207">Per <xref:System.Action>, specificare:</span><span class="sxs-lookup"><span data-stu-id="00b4c-207">For the <xref:System.Action>, specify:</span></span>

* <span data-ttu-id="00b4c-208">Il livello del log.</span><span class="sxs-lookup"><span data-stu-id="00b4c-208">The log level.</span></span>
* <span data-ttu-id="00b4c-209">Un identificatore di evento univoco (<xref:Microsoft.Extensions.Logging.EventId>) con il nome del metodo di estensione statico.</span><span class="sxs-lookup"><span data-stu-id="00b4c-209">A unique event identifier (<xref:Microsoft.Extensions.Logging.EventId>) with the name of the static extension method.</span></span>
* <span data-ttu-id="00b4c-210">Il modello di messaggio (stringa di formato denominata).</span><span class="sxs-lookup"><span data-stu-id="00b4c-210">The message template (named format string).</span></span> 

<span data-ttu-id="00b4c-211">Una richiesta per la pagina di indice dell'app di esempio imposta:</span><span class="sxs-lookup"><span data-stu-id="00b4c-211">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="00b4c-212">Il livello del log su `Information`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-212">Log level to `Information`.</span></span>
* <span data-ttu-id="00b4c-213">L'ID evento su `1` con il nome del metodo `IndexPageRequested`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-213">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="00b4c-214">Il modello di messaggio (stringa di formato denominata) su una stringa.</span><span class="sxs-lookup"><span data-stu-id="00b4c-214">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="00b4c-215">Negli archivi di log strutturati è possibile che venga usato il nome evento quando viene specificato con l'ID evento per arricchire la registrazione.</span><span class="sxs-lookup"><span data-stu-id="00b4c-215">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="00b4c-216">Ad esempio, [Serilog](https://github.com/serilog/serilog-extensions-logging) usa il nome evento.</span><span class="sxs-lookup"><span data-stu-id="00b4c-216">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="00b4c-217"><xref:System.Action> viene richiamato attraverso un metodo di estensione fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="00b4c-217">The <xref:System.Action> is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="00b4c-218">Il metodo `IndexPageRequested` registra un messaggio per la richiesta GET di una pagina di indice nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="00b4c-218">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="00b4c-219">`IndexPageRequested` viene chiamato nel logger nel metodo `OnGetAsync` in *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="00b4c-219">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="00b4c-220">Esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="00b4c-220">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="00b4c-221">Per passare i parametri in un messaggio di log, definire un massimo di sei tipi durante la creazione del campo statico.</span><span class="sxs-lookup"><span data-stu-id="00b4c-221">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="00b4c-222">L'app di esempio registra una stringa quando viene aggiunta un'offerta definendo un tipo `string` per il campo <xref:System.Action>:</span><span class="sxs-lookup"><span data-stu-id="00b4c-222">The sample app logs a string when adding a quote by defining a `string` type for the <xref:System.Action> field:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="00b4c-223">Il modello di messaggio di log del delegato riceve i valori dei segnaposto dai tipi specificati.</span><span class="sxs-lookup"><span data-stu-id="00b4c-223">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="00b4c-224">L'app di esempio definisce un delegato per l'aggiunta di un'offerta dove il parametro dell'offerta è un elemento `string`:</span><span class="sxs-lookup"><span data-stu-id="00b4c-224">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="00b4c-225">Il metodo di estensione statico per l'aggiunta di un'offerta, `QuoteAdded`, riceve il valore dell'argomento dell'offerta e lo passa al delegato <xref:System.Action>:</span><span class="sxs-lookup"><span data-stu-id="00b4c-225">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the <xref:System.Action> delegate:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="00b4c-226">Nel modello della pagina di indice (*Pages/Index.cshtml.cs*) viene chiamato `QuoteAdded` per registrare il messaggio:</span><span class="sxs-lookup"><span data-stu-id="00b4c-226">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="00b4c-227">Esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="00b4c-227">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="00b4c-228">L'app di esempio implementa un modello [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) per l'eliminazione dell'offerta.</span><span class="sxs-lookup"><span data-stu-id="00b4c-228">The sample app implements a [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) pattern for quote deletion.</span></span> <span data-ttu-id="00b4c-229">Per un'operazione di eliminazione con esito positivo, viene registrato un messaggio informativo.</span><span class="sxs-lookup"><span data-stu-id="00b4c-229">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="00b4c-230">Per un'operazione di eliminazione con la generazione di un'eccezione, viene registrato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="00b4c-230">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="00b4c-231">Il messaggio di log per un'operazione di eliminazione con esito negativo include l'analisi dello stack dell'eccezione (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="00b4c-231">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="00b4c-232">Si noti come l'eccezione viene passata al delegato in `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="00b4c-232">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="00b4c-233">Nel modello di pagina per la pagina di indice l'eliminazione di un'offerta con esito positivo chiama il metodo `QuoteDeleted` nel logger.</span><span class="sxs-lookup"><span data-stu-id="00b4c-233">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="00b4c-234">Quando non viene trovata un'offerta per l'eliminazione, viene generata un'eccezione <xref:System.ArgumentNullException>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-234">When a quote isn't found for deletion, an <xref:System.ArgumentNullException> is thrown.</span></span> <span data-ttu-id="00b4c-235">L'eccezione viene intercettata dall'istruzione [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) e registrata chiamando il metodo `QuoteDeleteFailed` nel logger nel blocco [catch](/dotnet/csharp/language-reference/keywords/try-catch) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="00b4c-235">The exception is trapped by the [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) statement and logged by calling the `QuoteDeleteFailed` method on the logger in the [catch](/dotnet/csharp/language-reference/keywords/try-catch) block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="00b4c-236">Quando un'offerta viene eliminata, esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="00b4c-236">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="00b4c-237">Quando l'eliminazione di un'offerta ha esito negativo, esaminare l'output della console dell'app.</span><span class="sxs-lookup"><span data-stu-id="00b4c-237">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="00b4c-238">Si noti che l'eccezione è inclusa nel messaggio di log:</span><span class="sxs-lookup"><span data-stu-id="00b4c-238">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T]
       (T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() 
      in <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a><span data-ttu-id="00b4c-239">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="00b4c-239">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="00b4c-240">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) crea un delegato <xref:System.Func%601> per definire un [ambito del log](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="00b4c-240">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) creates a <xref:System.Func%601> delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="00b4c-241">Gli overload <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> permettono il passaggio di un massimo di tre parametri di tipo in una stringa di formato denominata (modello).</span><span class="sxs-lookup"><span data-stu-id="00b4c-241"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="00b4c-242">Come avviene per il metodo <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*>, la stringa specificata nel metodo <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> è un modello e non una stringa interpolata.</span><span class="sxs-lookup"><span data-stu-id="00b4c-242">As is the case with the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method, the string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="00b4c-243">I segnaposto vengono inseriti nell'ordine in cui sono specificati i tipi.</span><span class="sxs-lookup"><span data-stu-id="00b4c-243">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="00b4c-244">I nomi dei segnaposto nel modello devono essere descrittivi e coerenti tra i modelli.</span><span class="sxs-lookup"><span data-stu-id="00b4c-244">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="00b4c-245">Vengono usati come nomi di proprietà all'interno dei dati di log strutturati.</span><span class="sxs-lookup"><span data-stu-id="00b4c-245">They serve as property names within structured log data.</span></span> <span data-ttu-id="00b4c-246">Per i nomi dei segnaposto è consigliabile usare la [convenzione Pascal](/dotnet/standard/design-guidelines/capitalization-conventions).</span><span class="sxs-lookup"><span data-stu-id="00b4c-246">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="00b4c-247">Ad esempio, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="00b4c-247">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="00b4c-248">Definire un [ambito del log](xref:fundamentals/logging/index#log-scopes) da applicare a una serie di messaggi di log usando il metodo <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*>.</span><span class="sxs-lookup"><span data-stu-id="00b4c-248">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method.</span></span>

<span data-ttu-id="00b4c-249">L'app di esempio include un pulsante **Clear All** (Cancella tutto) per eliminare tutte le offerte nel database.</span><span class="sxs-lookup"><span data-stu-id="00b4c-249">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="00b4c-250">Le offerte vengono eliminate rimuovendole una alla volta.</span><span class="sxs-lookup"><span data-stu-id="00b4c-250">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="00b4c-251">Ogni volta che viene eliminata un'offerta, viene chiamato il metodo `QuoteDeleted` nel logger.</span><span class="sxs-lookup"><span data-stu-id="00b4c-251">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="00b4c-252">Viene aggiunto un ambito di log a questi messaggi di log.</span><span class="sxs-lookup"><span data-stu-id="00b4c-252">A log scope is added to these log messages.</span></span>

<span data-ttu-id="00b4c-253">Abilitare `IncludeScopes` nella sezione del logger di console di *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="00b4c-253">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

<span data-ttu-id="00b4c-254">Per creare un ambito di log, aggiungere un campo per inserire un delegato <xref:System.Func%601> per l'ambito.</span><span class="sxs-lookup"><span data-stu-id="00b4c-254">To create a log scope, add a field to hold a <xref:System.Func%601> delegate for the scope.</span></span> <span data-ttu-id="00b4c-255">L'app di esempio crea un campo denominato `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="00b4c-255">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="00b4c-256">Usare <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> per creare il delegato.</span><span class="sxs-lookup"><span data-stu-id="00b4c-256">Use <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> to create the delegate.</span></span> <span data-ttu-id="00b4c-257">È possibile specificare un massimo di tre tipi da usare come argomenti di modello quando viene richiamato il delegato.</span><span class="sxs-lookup"><span data-stu-id="00b4c-257">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="00b4c-258">L'app di esempio usa un modello di messaggio che include il numero di offerte eliminate (un tipo `int`):</span><span class="sxs-lookup"><span data-stu-id="00b4c-258">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="00b4c-259">Specificare un metodo di estensione statico per il messaggio di log.</span><span class="sxs-lookup"><span data-stu-id="00b4c-259">Provide a static extension method for the log message.</span></span> <span data-ttu-id="00b4c-260">Includere i parametri di tipo per le proprietà denominate visualizzate nel modello di messaggio.</span><span class="sxs-lookup"><span data-stu-id="00b4c-260">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="00b4c-261">L'app di esempio accetta un numero `count` di offerte da eliminare e restituisce `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="00b4c-261">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="00b4c-262">L'ambito esegue il wrapping delle chiamate di estensione di registrazione in un blocco [using](/dotnet/csharp/language-reference/keywords/using-statement):</span><span class="sxs-lookup"><span data-stu-id="00b4c-262">The scope wraps the logging extension calls in a [using](/dotnet/csharp/language-reference/keywords/using-statement) block:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="00b4c-263">Esaminare i messaggi di log nell'output della console dell'app.</span><span class="sxs-lookup"><span data-stu-id="00b4c-263">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="00b4c-264">Il risultato seguente mostra tre offerte eliminate con il messaggio di ambito di log incluso:</span><span class="sxs-lookup"><span data-stu-id="00b4c-264">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="00b4c-265">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="00b4c-265">Additional resources</span></span>

* [<span data-ttu-id="00b4c-266">Registrazione</span><span class="sxs-lookup"><span data-stu-id="00b4c-266">Logging</span></span>](xref:fundamentals/logging/index)
