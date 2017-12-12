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
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Registrazione ad alte prestazioni con LoggerMessage in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) funzionalità creano delegati memorizzabile nella cache che richiedono un minor numero di allocazioni di oggetti e una riduzione del carico di elaborazione di [metodi di estensione logger](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), ad esempio `LogInformation`, `LogDebug`e `LogError`. Per gli scenari di registrazione ad alte prestazioni, utilizzare il `LoggerMessage` modello.

`LoggerMessage`offre i vantaggi delle prestazioni seguenti sui metodi di estensione Logger:

* Metodi di estensione logger richiedono tipi di valore "boxing" (conversione), ad esempio `int`, in `object`. Il `LoggerMessage` modello evita la conversione boxing usando statico `Action` campi e metodi di estensione con parametri fortemente tipizzati.
* Metodi di estensione logger necessario analizzare il modello di messaggio (stringa di formato denominata) ogni volta che viene scritto un messaggio di log. `LoggerMessage`richiede solo l'analisi di un modello di una volta quando viene definito il messaggio.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Viene illustrato l'app di esempio `LoggerMessage` funzionalità a un'offerta di base di sistema di rilevamento. L'applicazione aggiunge ed Elimina le offerte utilizzando un database in memoria. Se si verificano queste operazioni, messaggi di log sono generati mediante la `LoggerMessage` modello.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Definire (LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) crea un `Action` delegato per la registrazione di un messaggio. `Define`gli overload consentono il passaggio di parametri di tipo fino a sei a una stringa di formato denominato (modello).

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) crea un `Func` delegato per la definizione di un [log ambito](xref:fundamentals/logging/index#log-scopes). `DefineScope`gli overload è possibile passare un massimo di tre parametri di tipo in una stringa di formato denominato (modello).

## <a name="message-template-named-format-string"></a>Modello di messaggio (denominato stringa di formato)

La stringa fornita per il `Define` e `DefineScope` metodi è un modello e non una stringa interpolata. I segnaposti vengono sostituiti nell'ordine che siano specificati i tipi. Nomi di segnaposto nel modello devono essere descrittivi e coerente tra modelli. Vengono utilizzati come nomi di proprietà all'interno di dati strutturati di log. È consigliabile [Pascal maiuscole e minuscole](/dotnet/standard/design-guidelines/capitalization-conventions) per i nomi di segnaposto. Ad esempio, `{Count}`, `{FirstName}`.

## <a name="implementing-loggermessagedefine"></a>Implementazione LoggerMessage.Define

Ogni messaggio di log è un `Action` contenuti in un campo statico creato da `LoggerMessage.Define`. Ad esempio, l'app di esempio crea un campo per descrivere un messaggio di log per una richiesta GET per la pagina di indice (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Per il `Action`, specificare:

* Il livello di registrazione.
* Un identificatore dell'evento univoco ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) con il nome del metodo di estensione statici.
* Il modello di messaggio (denominato stringa di formato). 

Una richiesta per la pagina di indice dei set di app di esempio di:

* Accedere a livello di `Information`.
* Id evento `1` con il nome del `IndexPageRequested` metodo.
* Modello di messaggio (stringa di formato denominato) in una stringa.

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Registrazione strutturati archivi è possono utilizzare il nome dell'evento quando è stato fornito con l'id evento di arricchire la registrazione. Ad esempio, [Serilog](https://github.com/serilog/serilog-extensions-logging) utilizza il nome dell'evento.

Il `Action` viene richiamato tramite un metodo di estensione fortemente tipizzata. Il `IndexPageRequested` metodo registra un messaggio per una richiesta GET pagina di indice nell'app di esempio:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested`viene chiamato sul logger nel `OnGetAsync` metodo *Pages/Index.cshtml.cs*:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Esaminare l'output della console dell'app:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Per passare parametri a un messaggio di log, è possibile definire fino a sei tipi quando si crea il campo statico. L'app di esempio accede a una stringa durante l'aggiunta di un'offerta definendo un `string` tipo per il `Action` campo:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Modello di messaggio di log del delegato riceve i valori segnaposto dai tipi forniti. L'app di esempio definisce un delegato per l'aggiunta di un'offerta dove il parametro offerta è un `string`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

Il metodo di estensione statici per l'aggiunta di un'offerta, `QuoteAdded`, riceve il valore dell'argomento offerta e lo passa al `Action` delegato:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

Nel file code-behind della pagina di indice (*Pages/Index.cshtml.cs*), `QuoteAdded` viene chiamata per registrare il messaggio:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Esaminare l'output della console dell'app:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

L'applicazione di esempio implementa un `try` &ndash; `catch` modello per l'eliminazione di offerta. Per un'operazione di eliminazione ha esito positivo, viene registrato un messaggio informativo. Un messaggio di errore viene registrato per un'operazione di eliminazione quando viene generata un'eccezione. Il messaggio di log per l'operazione di eliminazione l'esito negativo include la traccia dello stack dell'eccezione (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Si noti come l'eccezione viene passato al delegato in `QuoteDeleteFailed`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

Nell'indice pagina code-behind, l'eliminazione ha esito positivo offerta chiama il `QuoteDeleted` metodo il logger. Quando non viene trovata un'offerta per l'eliminazione, un `ArgumentNullException` viene generata un'eccezione. L'eccezione viene intercettato dal `try` &ndash; `catch` istruzione e registrato chiamando il `QuoteDeleteFailed` il logger nel metodo il `catch` blocco (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Quando viene eliminata correttamente una virgoletta, esaminare l'output della console dell'app:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Quando offerta eliminazione non riesce, esaminare l'output della console dell'app. Si noti che l'eccezione è incluso nel messaggio di log:

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

## <a name="implementing-loggermessagedefinescope"></a>Implementazione LoggerMessage.DefineScope

Definire un [log ambito](xref:fundamentals/logging/index#log-scopes) da applicare a una serie di messaggi di log utilizzando il [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) metodo.

L'applicazione di esempio dispone di un **Cancella tutto** pulsante per l'eliminazione di tutte le virgolette nel database. Le virgolette vengono eliminate, rimuoverle uno alla volta. Ogni volta che viene eliminata un'offerta, il `QuoteDeleted` metodo viene chiamato sul logger. Un ambito di log viene aggiunto a questi messaggi di log.

Abilitare `IncludeScopes` nelle opzioni di logger di console:

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

Impostazione `IncludeScopes` nelle applicazioni ASP.NET Core 2.0 per abilitare gli ambiti di log è necessaria. Impostazione `IncludeScopes` tramite *appsettings* i file di configurazione è una funzionalità che ha pianificato per la versione di ASP.NET Core 2.1.

L'app di esempio cancella altri provider e aggiunge i filtri per ridurre l'output di registrazione. Questo rende più semplice visualizzare i messaggi di log dell'esempio che illustrano `LoggerMessage` funzionalità.

Per creare un ambito di log, aggiungere un campo per contenere un `Func` delegato per l'ambito. L'app di esempio crea un campo denominato `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Utilizzare `DefineScope` per creare il delegato. Fino a tre tipi possono essere specificati per l'utilizzo come argomenti di modello quando viene richiamato il delegato. L'app di esempio utilizza un modello di messaggio che include il numero di virgolette eliminate (un `int` tipo):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Fornire un metodo di estensione statici per il messaggio di log. Includere i parametri di tipo per le proprietà denominate che vengono visualizzati nel modello di messaggio. L'app di esempio accetta un `count` di offerta da eliminare e restituisce `_allQuotesDeletedScope`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Esegue il wrapping di ambito chiama l'estensione di registrazione in un `using` blocco:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Esaminare i messaggi di log di output della console dell'app. Il risultato seguente mostra tre offerte eliminate con il messaggio di ambito di log incluso:

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

## <a name="see-also"></a>Vedere anche

* [Registrazione](xref:fundamentals/logging/index)
