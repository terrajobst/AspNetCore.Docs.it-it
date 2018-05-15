---
title: Registrazione a prestazioni elevate con LoggerMessage in ASP.NET Core
author: guardrex
description: Informazioni su come usare LoggerMessage per creare delegati inseribili nella cache che richiedono un numero minore di allocazioni di oggetti per scenari di registrazione a prestazioni elevate.
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 24a75cfacfa61ca66e78deeb743baa75718dfb76
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="ea70f-103">Registrazione a prestazioni elevate con LoggerMessage in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea70f-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="ea70f-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ea70f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ea70f-105">Le funzionalità di [LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) creano delegati inseribili nella cache che richiedono un numero minore di allocazioni di oggetti e riducono il sovraccarico di calcolo rispetto ai [metodi di estensione del logger](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), ad esempio `LogInformation`, `LogDebug` e `LogError`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="ea70f-106">Per gli scenari di registrazione a prestazioni elevate, usare `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="ea70f-107">`LoggerMessage` offre i seguenti vantaggi in termini di prestazioni rispetto ai metodi di estensione del logger:</span><span class="sxs-lookup"><span data-stu-id="ea70f-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="ea70f-108">I metodi di estensione del logger richiedono una "conversione boxing" dei tipi di valori, ad esempio `int`, in `object`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="ea70f-109">`LoggerMessage` evita la conversione boxing usando campi `Action` statici e metodi di estensione con parametri fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="ea70f-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="ea70f-110">I metodi di estensione del logger devono analizzare il modello di messaggio (stringa di formato denominata) ogni volta che viene scritto un messaggio del log.</span><span class="sxs-lookup"><span data-stu-id="ea70f-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="ea70f-111">Solo `LoggerMessage` richiede una sola analisi del modello durante la definizione del messaggio.</span><span class="sxs-lookup"><span data-stu-id="ea70f-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="ea70f-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ea70f-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ea70f-113">L'app di esempio illustra le funzionalità di `LoggerMessage` con un sistema di verifica delle offerte di base.</span><span class="sxs-lookup"><span data-stu-id="ea70f-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="ea70f-114">L'app aggiunge ed elimina le offerte usando un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="ea70f-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="ea70f-115">Durante l'esecuzione di queste operazioni, i messaggi del log vengono generati usando `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="ea70f-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="ea70f-116">LoggerMessage.Define</span></span>

<span data-ttu-id="ea70f-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) crea un delegato `Action` per la registrazione di un messaggio.</span><span class="sxs-lookup"><span data-stu-id="ea70f-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="ea70f-118">Gli overload `Define` permettono il passaggio di un massimo di sei parametri di tipo in una stringa di formato denominata (modello).</span><span class="sxs-lookup"><span data-stu-id="ea70f-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="ea70f-119">La stringa specificata nel metodo `Define` è un modello e non una stringa interpolata.</span><span class="sxs-lookup"><span data-stu-id="ea70f-119">The string provided to the `Define` method is a template and not an interpolated string.</span></span> <span data-ttu-id="ea70f-120">I segnaposto vengono inseriti nell'ordine in cui sono specificati i tipi.</span><span class="sxs-lookup"><span data-stu-id="ea70f-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="ea70f-121">I nomi dei segnaposto nel modello devono essere descrittivi e coerenti tra i modelli.</span><span class="sxs-lookup"><span data-stu-id="ea70f-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="ea70f-122">Vengono usati come nomi di proprietà all'interno dei dati di log strutturati.</span><span class="sxs-lookup"><span data-stu-id="ea70f-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="ea70f-123">Per i nomi dei segnaposto è consigliabile usare la [convenzione Pascal](/dotnet/standard/design-guidelines/capitalization-conventions).</span><span class="sxs-lookup"><span data-stu-id="ea70f-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="ea70f-124">Ad esempio, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="ea70f-125">Ogni messaggio di log è un elemento `Action` contenuto in un campo statico creato da `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-125">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="ea70f-126">Ad esempio, l'app di esempio crea un campo per descrivere un messaggio di log per una richiesta GET per la pagina di indice (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea70f-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="ea70f-127">Per `Action`, specificare:</span><span class="sxs-lookup"><span data-stu-id="ea70f-127">For the `Action`, specify:</span></span>

* <span data-ttu-id="ea70f-128">Il livello del log.</span><span class="sxs-lookup"><span data-stu-id="ea70f-128">The log level.</span></span>
* <span data-ttu-id="ea70f-129">Un identificatore di evento univoco ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) con il nome del metodo di estensione statico.</span><span class="sxs-lookup"><span data-stu-id="ea70f-129">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="ea70f-130">Il modello di messaggio (stringa di formato denominata).</span><span class="sxs-lookup"><span data-stu-id="ea70f-130">The message template (named format string).</span></span> 

<span data-ttu-id="ea70f-131">Una richiesta per la pagina di indice dell'app di esempio imposta:</span><span class="sxs-lookup"><span data-stu-id="ea70f-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="ea70f-132">Il livello del log su `Information`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-132">Log level to `Information`.</span></span>
* <span data-ttu-id="ea70f-133">L'ID evento su `1` con il nome del metodo `IndexPageRequested`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="ea70f-134">Il modello di messaggio (stringa di formato denominata) su una stringa.</span><span class="sxs-lookup"><span data-stu-id="ea70f-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="ea70f-135">Negli archivi di log strutturati è possibile che venga usato il nome evento quando viene specificato con l'ID evento per arricchire la registrazione.</span><span class="sxs-lookup"><span data-stu-id="ea70f-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="ea70f-136">Ad esempio, [Serilog](https://github.com/serilog/serilog-extensions-logging) usa il nome evento.</span><span class="sxs-lookup"><span data-stu-id="ea70f-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="ea70f-137">`Action` viene richiamato attraverso un metodo di estensione fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="ea70f-137">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="ea70f-138">Il metodo `IndexPageRequested` registra un messaggio per la richiesta GET di una pagina di indice nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="ea70f-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="ea70f-139">`IndexPageRequested` viene chiamato nel logger nel metodo `OnGetAsync` in *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ea70f-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="ea70f-140">Esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="ea70f-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="ea70f-141">Per passare i parametri in un messaggio di log, definire un massimo di sei tipi durante la creazione del campo statico.</span><span class="sxs-lookup"><span data-stu-id="ea70f-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="ea70f-142">L'app di esempio registra una stringa quando viene aggiunta un'offerta definendo un tipo `string` per il campo `Action`:</span><span class="sxs-lookup"><span data-stu-id="ea70f-142">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="ea70f-143">Il modello di messaggio di log del delegato riceve i valori dei segnaposto dai tipi specificati.</span><span class="sxs-lookup"><span data-stu-id="ea70f-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="ea70f-144">L'app di esempio definisce un delegato per l'aggiunta di un'offerta dove il parametro dell'offerta è un elemento `string`:</span><span class="sxs-lookup"><span data-stu-id="ea70f-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="ea70f-145">Il metodo di estensione statico per l'aggiunta di un'offerta, `QuoteAdded`, riceve il valore dell'argomento dell'offerta e lo passa al delegato `Action`:</span><span class="sxs-lookup"><span data-stu-id="ea70f-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="ea70f-146">Nel modello della pagina di indice (*Pages/Index.cshtml.cs*) viene chiamato `QuoteAdded` per registrare il messaggio:</span><span class="sxs-lookup"><span data-stu-id="ea70f-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="ea70f-147">Esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="ea70f-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="ea70f-148">L'app di esempio implementa un modello `try`&ndash;`catch` per l'eliminazione dell'offerta.</span><span class="sxs-lookup"><span data-stu-id="ea70f-148">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="ea70f-149">Per un'operazione di eliminazione con esito positivo, viene registrato un messaggio informativo.</span><span class="sxs-lookup"><span data-stu-id="ea70f-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="ea70f-150">Per un'operazione di eliminazione con la generazione di un'eccezione, viene registrato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="ea70f-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="ea70f-151">Il messaggio di log per un'operazione di eliminazione con esito negativo include l'analisi dello stack dell'eccezione (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea70f-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="ea70f-152">Si noti come l'eccezione viene passata al delegato in `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="ea70f-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="ea70f-153">Nel modello di pagina per la pagina di indice l'eliminazione di un'offerta con esito positivo chiama il metodo `QuoteDeleted` nel logger.</span><span class="sxs-lookup"><span data-stu-id="ea70f-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="ea70f-154">Quando non viene trovata un'offerta per l'eliminazione, viene generata un'eccezione `ArgumentNullException`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-154">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="ea70f-155">L'eccezione viene intercettata dall'istruzione `try`&ndash;`catch` e registrata chiamando il metodo `QuoteDeleteFailed` nel logger nel blocco `catch` (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea70f-155">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="ea70f-156">Quando un'offerta viene eliminata, esaminare l'output della console dell'app:</span><span class="sxs-lookup"><span data-stu-id="ea70f-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="ea70f-157">Quando l'eliminazione di un'offerta ha esito negativo, esaminare l'output della console dell'app.</span><span class="sxs-lookup"><span data-stu-id="ea70f-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="ea70f-158">Si noti che l'eccezione è inclusa nel messaggio di log:</span><span class="sxs-lookup"><span data-stu-id="ea70f-158">Note that the exception is included in the log message:</span></span>

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

## <a name="loggermessagedefinescope"></a><span data-ttu-id="ea70f-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="ea70f-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="ea70f-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) crea un delegato `Func` per definire un [ambito del log](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="ea70f-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="ea70f-161">Gli overload `DefineScope` permettono il passaggio di un massimo di tre parametri di tipo in una stringa di formato denominata (modello).</span><span class="sxs-lookup"><span data-stu-id="ea70f-161">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="ea70f-162">Come avviene per il metodo `Define`, la stringa specificata nel metodo `DefineScope` è un modello e non una stringa interpolata.</span><span class="sxs-lookup"><span data-stu-id="ea70f-162">As is the case with the `Define` method, the string provided to the `DefineScope` method is a template and not an interpolated string.</span></span> <span data-ttu-id="ea70f-163">I segnaposto vengono inseriti nell'ordine in cui sono specificati i tipi.</span><span class="sxs-lookup"><span data-stu-id="ea70f-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="ea70f-164">I nomi dei segnaposto nel modello devono essere descrittivi e coerenti tra i modelli.</span><span class="sxs-lookup"><span data-stu-id="ea70f-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="ea70f-165">Vengono usati come nomi di proprietà all'interno dei dati di log strutturati.</span><span class="sxs-lookup"><span data-stu-id="ea70f-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="ea70f-166">Per i nomi dei segnaposto è consigliabile usare la [convenzione Pascal](/dotnet/standard/design-guidelines/capitalization-conventions).</span><span class="sxs-lookup"><span data-stu-id="ea70f-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="ea70f-167">Ad esempio, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="ea70f-168">Definire un [ambito del log](xref:fundamentals/logging/index#log-scopes) da applicare a una serie di messaggi di log usando il metodo [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope).</span><span class="sxs-lookup"><span data-stu-id="ea70f-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="ea70f-169">L'app di esempio include un pulsante **Clear All** (Cancella tutto) per eliminare tutte le offerte nel database.</span><span class="sxs-lookup"><span data-stu-id="ea70f-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="ea70f-170">Le offerte vengono eliminate rimuovendole una alla volta.</span><span class="sxs-lookup"><span data-stu-id="ea70f-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="ea70f-171">Ogni volta che viene eliminata un'offerta, viene chiamato il metodo `QuoteDeleted` nel logger.</span><span class="sxs-lookup"><span data-stu-id="ea70f-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="ea70f-172">Viene aggiunto un ambito di log a questi messaggi di log.</span><span class="sxs-lookup"><span data-stu-id="ea70f-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="ea70f-173">Abilitare `IncludeScopes` nelle opzioni del logger della console:</span><span class="sxs-lookup"><span data-stu-id="ea70f-173">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[](loggermessage/sample/Program.cs?name=snippet1&highlight=10)]

<span data-ttu-id="ea70f-174">Nelle app ASP.NET Core 2.0 è necessario impostare `IncludeScopes` per abilitare gli ambiti di log.</span><span class="sxs-lookup"><span data-stu-id="ea70f-174">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="ea70f-175">L'impostazione di `IncludeScopes` tramite i file di configurazione *appsettings* è una funzionalità prevista per ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ea70f-175">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="ea70f-176">L'app di esempio cancella gli altri provider e aggiunge filtri per ridurre l'output di registrazione.</span><span class="sxs-lookup"><span data-stu-id="ea70f-176">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="ea70f-177">Ciò facilita la visualizzazione dei messaggi di log dell'esempio che descrive le funzionalità di `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="ea70f-177">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="ea70f-178">Per creare un ambito di log, aggiungere un campo per inserire un delegato `Func` per l'ambito.</span><span class="sxs-lookup"><span data-stu-id="ea70f-178">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="ea70f-179">L'app di esempio crea un campo denominato `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea70f-179">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="ea70f-180">Usare `DefineScope` per creare il delegato.</span><span class="sxs-lookup"><span data-stu-id="ea70f-180">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="ea70f-181">È possibile specificare un massimo di tre tipi da usare come argomenti di modello quando viene richiamato il delegato.</span><span class="sxs-lookup"><span data-stu-id="ea70f-181">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="ea70f-182">L'app di esempio usa un modello di messaggio che include il numero di offerte eliminate (un tipo `int`):</span><span class="sxs-lookup"><span data-stu-id="ea70f-182">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="ea70f-183">Specificare un metodo di estensione statico per il messaggio di log.</span><span class="sxs-lookup"><span data-stu-id="ea70f-183">Provide a static extension method for the log message.</span></span> <span data-ttu-id="ea70f-184">Includere i parametri di tipo per le proprietà denominate visualizzate nel modello di messaggio.</span><span class="sxs-lookup"><span data-stu-id="ea70f-184">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="ea70f-185">L'app di esempio accetta un numero `count` di offerte da eliminare e restituisce `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="ea70f-185">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="ea70f-186">L'ambito esegue il wrapping delle chiamate di estensione di registrazione in un blocco `using`:</span><span class="sxs-lookup"><span data-stu-id="ea70f-186">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="ea70f-187">Esaminare i messaggi di log nell'output della console dell'app.</span><span class="sxs-lookup"><span data-stu-id="ea70f-187">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="ea70f-188">Il risultato seguente mostra tre offerte eliminate con il messaggio di ambito di log incluso:</span><span class="sxs-lookup"><span data-stu-id="ea70f-188">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="see-also"></a><span data-ttu-id="ea70f-189">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="ea70f-189">See also</span></span>

* [<span data-ttu-id="ea70f-190">Registrazione</span><span class="sxs-lookup"><span data-stu-id="ea70f-190">Logging</span></span>](xref:fundamentals/logging/index)
