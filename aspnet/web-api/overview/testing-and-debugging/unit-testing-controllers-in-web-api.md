---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Unit test controller in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: "In questo argomento vengono descritte alcune tecniche specifiche per gli unit test controller Web API 2. Prima di leggere questo argomento, è consigliabile leggere l'esercitazione unità..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Unit test controller in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> In questo argomento vengono descritte alcune tecniche specifiche per gli unit test controller Web API 2. Prima di leggere questo argomento, è possibile leggere l'esercitazione [Unit test ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), che mostra come aggiungere un progetto di unit test alla soluzione.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Dopo aver utilizzato Moq, ma lo stesso concetto si applica a qualsiasi framework di simulazione. Moq 4.5.30 (e versioni successive) supporta Visual Studio 2017, Roslyn, .NET 4.5 e versioni successive.

È un modello comune negli unit test &quot;Disponi-act-assert&quot;:

- Disponi: Consente di impostare tutti i prerequisiti per il test da eseguire.
- ACT: Eseguire il test.
- Asserzione: Verificare che il test ha avuto esito positivo.

Nel passaggio di disposizione, si utilizzeranno spesso fittizi o gli oggetti stub. Che riduce al minimo il numero di dipendenze, pertanto il test è incentrato su un elemento di test.

Ecco alcuni aspetti di cui si deve essere unit test nei controller di Web API:

- L'azione restituisce il tipo corretto di risposta.
- Parametri non validi restituiscono la risposta di errore corretto.
- L'azione chiama il metodo corretto sul livello del servizio o del repository.
- Se la risposta include un modello di dominio, verificare il tipo di modello.

Queste sono alcune delle operazioni generali per eseguire il test, ma gli aspetti specifici dipendono dall'implementazione del controller. In particolare, ha una grande importanza che indica se restituiscono le azioni del controller **HttpResponseMessage** o **IHttpActionResult**. Per ulteriori informazioni su questi tipi di risultati, vedere [risultati dell'azione in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Test per le azioni che restituiscono HttpResponseMessage

Ecco un esempio di un controller di cui restituire azioni **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Si noti che il controller utilizza inserimento di dipendenze di inserire un `IProductRepository`. Che rende i controller di più verificabili, poiché è possibile inserire un repository fittizio. Lo unit test seguente verifica che il `Get` metodo scrive un `Product` al corpo della risposta. Si supponga che `repository` è una simulazione `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

È importante impostare **richiesta** e **configurazione** nel controller. In caso contrario, il test avrà esito negativo con un **ArgumentNullException** o **InvalidOperationException**.

## <a name="testing-link-generation"></a>Generazione di collegamento di test

Il `Post` chiamate al metodo **UrlHelper.Link** per creare i collegamenti nella risposta. Questa operazione richiede un po' più il programma di installazione nello unit test:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

Il **UrlHelper** classe deve avere i dati della richiesta URL e route, il test deve impostare i valori per questi. Un'altra opzione consiste fittizi o stub **UrlHelper**. Con questo approccio, si sostituisce il valore predefinito di [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) con una versione fittizi o stub che restituisce un valore fisso.

Possibile riscrivere il test utilizzando il [Moq](https://github.com/Moq) framework. Installare il `Moq` pacchetto NuGet nel progetto di test.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

In questa versione, non è necessario impostare i dati di route, in quanto la simulazione **UrlHelper** restituisce una stringa costante.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Test per le azioni che restituiscono IHttpActionResult

In Web API 2, può restituire un'azione del controller **IHttpActionResult**, che è simile alla **ActionResult** in ASP.NET MVC. Il **IHttpActionResult** interfaccia definisce un modello di comando per la creazione di risposte HTTP. Anziché creare direttamente la risposta, il controller restituisca un **IHttpActionResult**. In un secondo momento, la pipeline richiama il **IHttpActionResult** per creare la risposta. Questo approccio rende più semplice scrivere unit test, in quanto è possibile ignorare un lotto del programma di installazione necessari per **HttpResponseMessage**.

Ecco un esempio controller il cui ritorno azioni **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Questo esempio illustra alcuni modelli comuni utilizzando **IHttpActionResult**. Di seguito viene illustrato come unità testarle.

### <a name="action-returns-200-ok-with-a-response-body"></a>Azione restituisce 200 (OK) con un corpo della risposta

Il `Get` chiamate al metodo `Ok(product)` se viene trovato il prodotto. Nello unit test, assicurarsi che il tipo restituito è **OkNegotiatedContentResult** e il prodotto restituito con l'ID corretto.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Si noti che lo unit test non esegue il risultato dell'azione. È possibile presupporre che il risultato dell'azione consente di creare la risposta HTTP in modo corretto. (Per tale motivo framework Web API con il proprio unit test!)

### <a name="action-returns-404-not-found"></a>Azione restituisce 404 (non trovato)

Il `Get` chiamate al metodo `NotFound()` se non viene trovato il prodotto. In questo caso, lo unit test verifica solo se il tipo restituito è **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Azione restituisce 200 (OK) con alcun corpo della risposta

Il `Delete` chiamate al metodo `Ok()` per restituire una risposta HTTP 200 vuota. Come illustrato nell'esempio precedente, lo unit test controlla il tipo restituito, in questo caso **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Azione restituisce 201 (creato) con un'intestazione di posizione

Il `Post` chiamate al metodo `CreatedAtRoute` per restituire una risposta HTTP 201 con un URI nell'intestazione Location. Nello unit test, verificare che l'azione imposta i valori corretti di routing.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Azione restituisce un'altra 2xx con un corpo della risposta

Il `Put` chiamate al metodo `Content` per restituire una risposta HTTP 202 (accettato) con un corpo della risposta. In questo caso è simile alla restituzione di 200 (OK), ma lo unit test controllare inoltre il codice di stato.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Tali Entity Framework quando gli Unit test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Scrittura di test per un servizio ASP.NET Web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (post di blog di Youssef Moussaoui).
- [Debugging ASP.NET Web API with Route Debugger](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
