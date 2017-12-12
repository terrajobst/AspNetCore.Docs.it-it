---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Globale gestione degli errori in ASP.NET Web API 2 | Documenti Microsoft
author: davidmatson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d2bdf04b4da2a099f3a2af100b16682c68f946f2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Globale gestione degli errori in ASP.NET Web API 2
====================
da [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Oggi non sarà più facile nell'API Web per accedere o gestire gli errori a livello globale. Alcune eccezioni non gestite possono essere elaborati tramite [filtri eccezioni](exception-handling.md), ma esistono un numero di case che non possono gestire i filtri eccezioni. Ad esempio:

1. Eccezioni generate dai costruttori di controller.
2. Eccezioni generate da gestori di messaggi.
3. Eccezioni generate durante il routing.
4. Eccezioni generate durante la serializzazione del contenuto di risposta.

Microsoft desidera fornire un modo semplice e coerenza per accedere e gestire (dove possibile) queste eccezioni. 

Esistono due casi principali per la gestione delle eccezioni, nel caso in cui è sono in grado di inviare una risposta di errore e nel caso in cui è possibile eseguire questa operazione è registro l'eccezione. Un esempio per quest'ultimo caso è quando viene generata un'eccezione all'interno di streaming del contenuto della risposta; In questo caso è troppo tardi per inviare un nuovo messaggio di risposta, poiché il codice di stato, intestazioni e contenuto parziale passate in rete, pertanto è semplicemente di interrompere la connessione. Anche se l'eccezione non gestita per produrre un nuovo messaggio di risposta, è ancora supportato registrazione dell'eccezione. Nei casi in cui è possibile rilevare un errore, è possibile restituire una risposta di errore appropriato come illustrato di seguito:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opzioni esistenti

Oltre a [filtri eccezioni](exception-handling.md), [gestori di messaggi](../advanced/http-message-handlers.md) attualmente utilizzati in modo da osservare tutte le risposte di livello 500, ma dispongono di un contesto relative all'errore originale è difficile agire su tali risposte. Gestori di messaggi sono anche alcune delle stesse limitazioni di filtri di eccezioni per i casi che possono gestire. Mentre API Web dell'infrastruttura di traccia che acquisisce le condizioni di errore dell'infrastruttura di analisi per scopi di diagnostica e non è progettato per adatto per l'esecuzione in ambienti di produzione. Globale di registrazione e gestione delle eccezioni deve essere servizi che possono eseguire durante la produzione e di essere inseriti in soluzioni di monitoraggio esistente (ad esempio, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Panoramica della soluzione

 Offriamo due nuovi servizi sostituibile, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) e IExceptionHandler, per accedere e gestire le eccezioni non gestite. I servizi sono molto simili, con due importanti differenze:

1. Sono supportati la registrazione, ma solo un solo gestore di eccezioni più logger di eccezioni.
2. Logger di eccezioni sempre chiamato, anche se si sta per interrompere la connessione. Gestori di eccezioni ottengano chiamati solo quando si è ancora in grado di scegliere quale messaggio di risposta da inviare.

Entrambi i servizi forniscono l'accesso a un contesto di eccezione che contiene informazioni rilevanti dal punto in cui è stata rilevata l'eccezione, in particolare il [HttpRequestMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/en-us/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), generata un'eccezione e l'origine dell'eccezione (dettagli riportati di seguito).

### <a name="design-principles"></a>Principi di progettazione

1. **Nessuna modifica di rilievo** perché questa funzionalità viene aggiunto in una versione secondaria, tale vincolo importante conseguenze per la soluzione è non esserci Nessuna modifica di rilievo al tipo di contratto o comportamento. Questo vincolo è esclusa da una certa pulitura, desideriamo eseguita in termini di blocchi catch esistenti attivare eccezioni in 500 risposte. Le operazioni di pulizia è un elemento che è possibile prendere in considerazione per una successiva versione principale. Se questo è importante, votare in [voce utente ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Mantenere la coerenza con l'API Web costruisce** pipeline filtro dell'API Web è un ottimo modo per gestire i problemi di montaggio incrociato con la flessibilità di applicare la logica di un ambito di azione specifici, specifici dei controller o globale. I filtri, inclusi i filtri di eccezione, sono sempre contesti di azione e il controller, anche quando registrato nell'ambito globale. Esiste che contratto più utile per i filtri, ma significa che i filtri eccezioni, di conseguenza anche a livello globale con ambito non sono più adatta per alcune eccezioni casi, ad esempio eccezioni da gestori di messaggi, in cui nessun contesto di azione o controller. Se si desidera utilizzare l'ambito flessibile offerto dai filtri per la gestione delle eccezioni, è necessario comunque i filtri eccezioni. Ma se è necessario gestire l'eccezione all'esterno di un contesto del controller, è anche necessario distinto per la gestione degli errori globale completo (qualcosa senza i vincoli di contesto azione e contesto del controller).

### <a name="when-to-use"></a>Quando utilizzare

- Logger di eccezioni sono la soluzione per visualizzare tutti eccezione non gestita rilevata dall'API Web.
- Gestori di eccezioni sono la soluzione per la personalizzazione di tutte le risposte possibili alle eccezioni non gestite rilevate dall'API Web.
- I filtri eccezioni sono la soluzione più semplice per elaborare le eccezioni di subset non gestita correlate a una determinata azione o controller.

### <a name="service-details"></a>Dettagli servizio

 Le interfacce del servizio logger e il gestore di eccezioni sono i metodi async semplice che i rispettivi contesti: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 È possibile utilizzare le classi di base per entrambe le interfacce. L'override dei metodi di base (sincronizzazione o asincrono) è tutto ciò che è necessario registrare o gestire consigliata volte. Per la registrazione di `ExceptionLogger` classe di base garantisce che il metodo di registrazione di componenti di base chiamato solo una volta per ogni eccezione (anche se in un secondo momento propaga ulteriormente nello stack e viene intercettata nuovamente). La `ExceptionHandler` classe base chiamerà il principale metodo di gestione solo per i blocchi catch eccezioni nella parte superiore dello stack di chiamate, ignorando legacy annidati. (Versioni semplificate di queste classi di base sono nell'Appendice riportata di seguito). Entrambi `IExceptionLogger` e `IExceptionHandler` ricevere informazioni sull'eccezione tramite un `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Quando il framework chiama un logger di eccezioni o un gestore di eccezioni, sempre fornirà un `Exception` e `Request`. Ad eccezione di unit test, verrà sempre anche fornire un `RequestContext`. Raramente fornirà un `ControllerContext` e `ActionContext` (solo quando si chiama il blocco catch per i filtri eccezioni). Raramente fornirà un `Response`(solo in determinati casi IIS quando all'interno di un tentativo di scrivere la risposta). Si noti che, poiché alcune di queste proprietà possono essere `null` spetta al consumer di verificare la presenza di `null` prima di accedere ai membri della classe di eccezione.`CatchBlock` una stringa che indica il blocco catch visto l'eccezione. Di seguito sono riportate le stringhe del blocco catch:

- Server HTTP (SendAsync metodo)
- HttpControllerDispatcher. (SendAsync metodo di)
- HttpBatchHandler. (SendAsync metodo di)
- IExceptionFilter (elaborazione dell'ApiController della pipeline filtro di eccezione in ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (per la memorizzazione nel buffer di output)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (per i flussi di output)
- Host Web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (per la memorizzazione nel buffer di output)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (per i flussi di output)
    - HttpControllerHandler.WriteErrorResponseContentAsync (per gli errori di recupero da errori in modalità di output memorizzato nel buffer)

L'elenco di stringhe di blocco catch è anche disponibile tramite la proprietà statico di sola lettura. (La stringa del blocco catch core presenti il ExceptionCatchBlocks statico, la parte rimanente vengono visualizzati in una classe statica ogni per OWIN e web host).`IsTopLevelCatchBlock` è utile per il modello consigliato di gestione delle eccezioni solo nella parte superiore dello stack di chiamate seguente. Anziché attivare eccezioni in 500 risposte ovunque che si verifica un blocco catch nidificato, un gestore di eccezioni può consentire eccezioni propagazione fino a quando non sono in corso di essere visualizzato dall'host.

Oltre al `ExceptionContext`, una parte di più di informazioni tramite la versione completa di Ottiene un logger `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La seconda proprietà, `CanBeHandled`, consente a un logger identificare un'eccezione non gestita. Quando la connessione sta per essere interrotta e nessun nuovo messaggio di risposta può essere inviato, verrà chiamato il logger ma il gestore verrà ***non*** chiamato, e i logger possono identificare questo scenario da questa proprietà.

In aggiuntive per il `ExceptionContext`, un gestore ottiene una proprietà di altre può essere impostata in completa `ExceptionHandlerContext` per gestire l'eccezione:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un gestore di eccezioni indica che è gestita un'eccezione impostando il `Result` proprietà al risultato di un'azione (ad esempio, un [ExceptionResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), o un risultato personalizzato). Se il `Result` proprietà è null, viene gestita l'eccezione e verrà generata l'eccezione originale.

Per le eccezioni nella parte superiore dello stack di chiamate, è richiesto un passaggio aggiuntivo per assicurare che la risposta è appropriata per i chiamanti di API. Se l'eccezione si propaga fino a host, il chiamante verrà visualizzata la schermata gialla di morte o un altro host fornito in risposta a cui è in genere in formato HTML e non viene in genere una risposta di errore API appropriata. In questi casi, eseguire il backup viene avviato il risultato non null e solo se un gestore di eccezioni personalizzato imposta in modo esplicito per `null` (non gestite) l'eccezione propagherà all'host. Impostazione `Result` a `null` in tali casi, può essere utile per due scenari:

1. OWIN ospitato API Web con middleware registrato prima/esterno API Web di gestione delle eccezioni personalizzata.
2. Il debug locale tramite un browser, dove il gialla schermata è effettivamente una risposta utile per un'eccezione non gestita.

Per logger di eccezioni e gestori di eccezioni, non eseguire alcuna operazione per il ripristino se il logger o stesso gestore genera un'eccezione. (Diverso da lasciare che l'eccezione di propagazione, lasciare commenti e suggerimenti nella parte inferiore della pagina, se si dispone di un approccio migliore.) Il contratto per gestori e i logger di eccezioni è che essi deve non consentire eccezioni propagazione fino a relativi chiamanti; in caso contrario, l'eccezione solo propagherà, spesso a tutti gli altri host generando un errore HTML (ad esempio, ASP. Schermata di giallo di NET) inviati al client (che in genere non rappresenta l'opzione preferita per i chiamanti di API che prevedono JSON o XML).

## <a name="examples"></a>Esempi

### <a name="tracing-exception-logger"></a>Logger di eccezioni di traccia

Logger di eccezioni di sotto di inviare i dati di eccezione per le origini di traccia configurate (inclusa la finestra di output di Debug in Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Gestore di eccezioni di messaggio di errore personalizzato

Nell'esempio seguente produce una risposta di errore personalizzati ai client, tra cui un indirizzo di posta elettronica per contattare il supporto tecnico.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrazione di filtri eccezioni

Se si utilizza il modello di progetto "Applicazione Web di ASP.NET MVC 4" per creare il progetto, inserire il codice di configurazione Web API all'interno di `WebApiConfig` nella classe di *avvia* cartella:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Appendice: Classe di Base dettagli

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
