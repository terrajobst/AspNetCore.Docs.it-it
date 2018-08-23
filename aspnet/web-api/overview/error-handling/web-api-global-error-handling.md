---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: In ASP.NET Web API 2 gestione globale degli errori | Microsoft Docs
author: davidmatson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: a52c2a1589327421b7f498ff551145676c80e3e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827827"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Globale gestione degli errori in ASP.NET Web API 2
====================
dal [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Oggi non vi è alcun approccio facile nell'API Web per accedere o gestire gli errori a livello globale. Alcune eccezioni non gestite possono essere elaborati tramite [filtri eccezioni](exception-handling.md), ma esistono una serie di case che non è possibile gestire i filtri eccezioni. Ad esempio:

1. Eccezioni generate dai costruttori dei controller.
2. Eccezioni generate dai gestori di messaggi.
3. Eccezioni generate durante il routing.
4. Eccezioni generate durante la serializzazione del contenuto della risposta.

Si vogliono fornire un modo semplice e coerenza per accedere e gestire (laddove possibile) queste eccezioni. 

Esistono due casi principali per la gestione delle eccezioni, il caso in cui siamo in grado di inviare una risposta di errore e il caso in cui tutto quello che possiamo fare è log dell'eccezione. Un esempio per il secondo caso è quando viene generata un'eccezione durante lo streaming di contenuto della risposta; In tal caso è troppo tardi per inviare un nuovo messaggio di risposta, poiché il codice di stato, intestazioni e contenuto parziale già passate attraverso la rete, in modo che abbiamo semplicemente interrompere la connessione. Anche se l'eccezione non può essere gestita in modo da produrre un nuovo messaggio di risposta, sono ancora supportati la registrazione dell'eccezione. Nei casi in cui sia possibile rilevare un errore, è possibile restituire una risposta di errore appropriato come illustrato di seguito:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opzioni esistenti

Oltre a [filtri eccezioni](exception-handling.md), [gestori di messaggi](../advanced/http-message-handlers.md) può essere usato oggi per osservare tutte le risposte di livello 500, ma ad agire su tali risposte è difficile, poiché dispongono di un contesto relative all'errore originale. Gestori di messaggi sono anche alcune delle stesse limitazioni di filtri di eccezioni per i casi che possono gestire. Mentre l'API Web sono l'infrastruttura di traccia che acquisisce le condizioni di errore dell'infrastruttura di analisi è a scopo di diagnostica e non è progettato per appropriato per l'esecuzione in ambienti di produzione. Eccezioni globale, gestione e registrazione devono essere servizi che possono eseguire in fase di produzione e di essere inseriti in soluzioni di monitoraggio esistente (ad esempio, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Panoramica della soluzione

 Offriamo due nuovi servizi sostituibile, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) e IExceptionHandler, accedere e gestire le eccezioni non gestite. I servizi sono molto simili, con due differenze principali:

1. Supportiamo la registrazione più logger di eccezioni, ma solo un solo gestore di eccezioni.
2. Logger di eccezioni sempre chiamato, anche se abbiamo intenzione di interrompere la connessione. Gestori di eccezioni solo chiamati quando siamo ancora in grado di scegliere quale messaggio di risposta da inviare.

Entrambi i servizi di forniscono l'accesso a un contesto dell'eccezione contenente le informazioni pertinenti dal punto in cui è stata rilevata l'eccezione, in particolare [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), il [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), generata un'eccezione e l'origine dell'eccezione (i dettagli di seguito).

### <a name="design-principles"></a>Principi di progettazione

1. **Nessuna modifica di rilievo** perché questa funzionalità viene aggiunto in una versione secondaria, un vincolo importante conseguenze per la soluzione è che non vi siano Nessuna modifica di rilievo al tipo di contratto o comportamento. Questo vincolo è stato stabilito una pulizia che siamo lieti di aver eseguito in termini di blocchi catch esistenti l'attivazione delle eccezioni in 500 risposte. Questa pulizia aggiuntiva è un elemento che è possibile prendere in considerazione per una successiva versione principale. Se questo è importante votare su di esso in [suggerimenti degli utenti ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Costrutti di gestione della coerenza con l'API Web** pipeline filtro dell'API Web è un ottimo modo per gestire le problematiche trasversali grazie alla flessibilità di applicare la logica in un ambito di azione specifici, specifici dei controller o globale. I filtri, inclusi i filtri eccezioni, hanno sempre contesti di azione e il controller, anche quando registrato nell'ambito globale. Esiste una che contratto risulta utile per i filtri, ma significa che i filtri eccezioni, quelli anche a livello globale con ambiti, non sono una scelta ideale per alcuni casi, ad esempio le eccezioni dai gestori di messaggi, di gestione in cui nessun contesto di azione o un controller delle eccezioni. Se si vuole usare l'ambito flessibile offerto dai filtri per la gestione delle eccezioni, è necessario comunque i filtri eccezioni. Ma se è necessario gestire eccezioni di fuori di un contesto del controller, è necessario anche un costrutto separato per la gestione degli errori globali (un valore senza i vincoli di scelta rapida e azione controller).

### <a name="when-to-use"></a>Quando usare

- Logger di eccezioni rappresentano la soluzione per visualizzare tutti i eccezione non gestito rilevata mediante l'API Web.
- Gestori di eccezioni rappresentano la soluzione per la personalizzazione di tutte le risposte possibili alle eccezioni non gestite, rilevate mediante l'API Web.
- I filtri eccezioni sono la soluzione più semplice per l'elaborazione di eccezioni subset non gestita correlate a una determinata azione o controller.

### <a name="service-details"></a>Dettagli servizio

 Le interfacce del servizio del logger e gestore di eccezioni sono i metodi asincroni semplice prendendo i rispettivi contesti: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Offriamo anche le classi di base per entrambe le interfacce. L'override dei metodi core (sincrona o asincrona) è tutto ciò che è necessaria per accedere o gestire per l'elemento consigliato volte. Per la registrazione, il `ExceptionLogger` classe di base assicura che il metodo di registrazione core solo è chiamato una volta per ogni eccezione (anche se in un secondo momento si propaga nello stack e ulteriormente viene intercettata nuovamente). Il `ExceptionHandler` classe di base chiamerà il principale metodo di gestione solo per blocchi catch di eccezioni nella parte superiore dello stack di chiamate, ignorando legacy annidati. (Versioni semplificate di queste classi di base sono nell'appendice di seguito). Entrambe `IExceptionLogger` e `IExceptionHandler` ricevere informazioni sull'eccezione tramite un `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Quando il framework chiama un logger di eccezioni o un gestore di eccezioni, sempre offrirà un' `Exception` e un `Request`. Fatta eccezione per gli unit test, fornirà sempre anche un `RequestContext`. Raramente offrirà un' `ControllerContext` e `ActionContext` (solo quando si chiama dal blocco catch per i filtri eccezioni). Molto raramente fornirà un `Response`(solo in determinati casi IIS quando durante il tentativo di scrivere la risposta). Si noti che, poiché alcune di queste proprietà può essere `null` spetta al consumer di verificare la presenza di `null` prima di accedere a membri della classe di eccezione.`CatchBlock` è una stringa che indica quale blocco catch visto l'eccezione. Come indicato di seguito sono riportate le stringhe di blocco catch:

- Server HTTP (SendAsync metodo)
- HttpControllerDispatcher (SendAsync metodo)
- HttpBatchHandler (SendAsync metodo)
- E IExceptionFilter (elaborazione dell'ApiController della pipeline filtro eccezioni in ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (per la memorizzazione nel buffer di output)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (per i flussi di output)
- Host Web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (per la memorizzazione nel buffer di output)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (per i flussi di output)
    - HttpControllerHandler.WriteErrorResponseContentAsync (per gli errori nel recupero da errori in modalità di output memorizzato nel buffer)

L'elenco di stringhe di blocco catch è anche disponibile tramite le proprietà di sola lettura statico. (La stringa del blocco catch core si trovano nel ExceptionCatchBlocks statico, la parte rimanente vengono visualizzati in una classe statica ogni per host OWIN e web).`IsTopLevelCatchBlock` è utile per seguire il modello consigliato di gestione delle eccezioni solo nella parte superiore dello stack di chiamate. Invece di abilitare le eccezioni in 500 risposte in qualsiasi posizione che si verifica un blocco catch nidificato, un gestore di eccezioni può consentire eccezioni propagazione fino a quando non si accingono a essere visualizzato dall'host.

Oltre al `ExceptionContext`, un logger Ottiene un altro frammento di informazioni tramite la versione completa `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La seconda proprietà, `CanBeHandled`, consente a un logger identificare un'eccezione che non può essere gestita. Quando la connessione sta per essere interrotta e nessun nuovo messaggio di risposta può essere inviato, verrà chiamato il logger ma il gestore verrà ***non*** chiamato, e i logger possono identificare questo scenario da questa proprietà.

In aggiuntive per il `ExceptionContext`, un gestore ottiene un ulteriore proprietà che è possibile impostare per la versione completa `ExceptionHandlerContext` per gestire l'eccezione:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un gestore di eccezioni indica che un'eccezione ha gestito impostando il `Result` proprietà su un risultato dell'azione (ad esempio, un' [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), o un risultato personalizzato). Se il `Result` proprietà è null, l'eccezione viene gestita e verrà generata l'eccezione originale.

Per le eccezioni nella parte superiore dello stack di chiamate, è stato scelto un passaggio aggiuntivo per assicurarsi che la risposta è appropriata per i chiamanti di API. Se l'eccezione si propaga fino a host, il chiamante viene visualizzata la schermata gialla di morte o alcuni altri host fornita risposta che viene in genere in formato HTML e non viene in genere una risposta di errore API appropriata. In questi casi, l'avvio di risultato non null e solo se un gestore di eccezioni personalizzate lo imposta in modo esplicito il backup a `null` (non gestite) eccezione propagheranno all'host. L'impostazione `Result` a `null` in questi casi può essere utile per due scenari:

1. API Web ospitata in OWIN con middleware registrato prima di/esterno API Web di gestione delle eccezioni personalizzata.
2. Il debug locale tramite un browser, dove il gialla schermata è effettivamente una risposta utile per un'eccezione non gestita.

Per i logger di eccezioni e i gestori di eccezioni, non eseguono alcuna operazione per recuperare, se il logger o gestore stesso genera un'eccezione. (Diverso da lasciare che l'eccezione di propagazione, lasciare commenti e suggerimenti nella parte inferiore della pagina se è necessario un approccio migliore). Il contratto per i logger di eccezioni e gestori consiste nella deve non possibilità di eccezioni di propagarsi fino ai relativi chiamanti; in caso contrario, l'eccezione verrà semplicemente propagato, spesso fino all'host generando un errore HTML (ad esempio, ASP. Schermata di giallo di NET) inviato al client (che in genere non rappresenta l'opzione preferita per i chiamanti di API che prevedono JSON o XML).

## <a name="examples"></a>Esempi

### <a name="tracing-exception-logger"></a>Logger di eccezioni di traccia

Il logger di eccezioni di sotto di inviare i dati di eccezione per le origini di traccia configurate (che include la finestra di output di Debug in Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Gestore di eccezioni di messaggio di errore personalizzato

Nell'esempio seguente produce una risposta di errore personalizzati ai client, tra cui un indirizzo di posta elettronica per contattare il supporto tecnico.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>La registrazione di filtri eccezioni

Se si usa il modello di progetto "Applicazione Web di ASP.NET MVC 4" per creare il progetto, inserire il codice di configurazione Web API all'interno di `WebApiConfig` nella classe la *avvia* cartella:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Appendice: Dettagli di classe di Base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
