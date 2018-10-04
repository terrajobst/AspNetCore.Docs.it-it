---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Unit test controller in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: In questo argomento vengono descritte alcune tecniche specifiche per gli unit test controller nell'API Web 2. Prima di leggere questo argomento, è possibile leggere l'esercitazione unità...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e1bb1aa120ced95db7674eae1831f2a2c7356fc0
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794824"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Unit test controller in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> In questo argomento vengono descritte alcune tecniche specifiche per gli unit test controller nell'API Web 2. Prima di leggere questo argomento, è possibile leggere l'esercitazione [Unit test ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), che illustra come aggiungere un progetto di unit test alla soluzione.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Ho utilizzato Moq, ma lo stesso concetto si applica a qualsiasi framework di simulazione. Moq 4.5.30 (e versioni successive) supporta Visual Studio 2017, Roslyn e .NET 4.5 e versioni successive.

È comune negli unit test &quot;Disponi-act-assert&quot;:

- Disponi: Configurare i prerequisiti per eseguire il test.
- ACT: Eseguire il test.
- Assert: Verificare che il test ha avuto esito positivo.

Nel passaggio arrange, si utilizzeranno spesso simulazione o gli oggetti stub. Che riduce al minimo il numero di dipendenze, in modo che il test è incentrato su un'operazione di test.

Ecco alcuni aspetti che occorre inserire unit test nei controller API Web:

- L'azione restituisce il tipo corretto di risposta.
- Parametri non validi restituiscono la risposta di errore corretto.
- L'azione chiama il metodo corretto sul livello del repository o un servizio.
- Se la risposta include un modello di dominio, verificare il tipo di modello.

Questi sono alcuni degli aspetti generali da testare, ma gli aspetti specifici dipendono dall'implementazione del controller. In particolare, fa la differenza che indica se restituiscono le azioni del controller **HttpResponseMessage** oppure **IHttpActionResult**. Per altre informazioni su questi tipi di risultati, vedere [risultati delle azioni nell'Api Web 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Test per le azioni che restituiscono HttpResponseMessage

Di seguito è riportato un esempio di un controller di cui restituire le azioni **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Si noti che il controller Usa l'inserimento delle dipendenze per inserire un `IProductRepository`. Modo più testabili, il controller perché è possibile inserire un repository fittizio. Lo unit test seguente verifica che il `Get` scritture metodo un `Product` nel corpo della risposta. Si supponga che `repository` è una simulazione `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

È importante impostare **richiedere** e **configurazione** nel controller. In caso contrario, il test avrà esito negativo con un **ArgumentNullException** oppure **InvalidOperationException**.

## <a name="testing-link-generation"></a>Generazione di collegamenti di test

Il `Post` chiamate al metodo **UrlHelper.Link** per creare i collegamenti nella risposta. Questa operazione richiede un po' più il programma di installazione dello unit test:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

Il **UrlHelper** classe necessita dei dati di route e URL di richiesta, in modo che il test ha impostare i valori per questi. Un'altra opzione consiste simulazione o stub **UrlHelper**. Con questo approccio, sostituire il valore predefinito di [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) con una versione fittizi e stub che restituisce un valore fisso.

È possibile riscrivere il test utilizzando la [Moq](https://github.com/Moq) framework. Installare il `Moq` pacchetto NuGet nel progetto di test.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

In questa versione, non è necessario configurare eventuali dati di route, perché la simulazione **UrlHelper** restituisce una stringa costante.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Test per le azioni che restituiscono IHttpActionResult

Nell'API Web 2, è possibile restituire un'azione del controller **IHttpActionResult**, che è simile alla **ActionResult** in ASP.NET MVC. Il **IHttpActionResult** interfaccia definisce un modello di comando per la creazione di risposte HTTP. Anziché creare direttamente la risposta, il controller restituisce un **IHttpActionResult**. In un secondo momento, la pipeline richiama il **IHttpActionResult** per creare la risposta. Questo approccio rende più semplice scrivere unit test, perché è possibile ignorare molti il programma di installazione necessario per **HttpResponseMessage**.

Ecco un esempio controller cui restituire le azioni **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

In questo esempio illustra alcuni modelli comuni di uso **IHttpActionResult**. Di seguito viene illustrato come eseguire lo unit test li.

### <a name="action-returns-200-ok-with-a-response-body"></a>Azione restituisce 200 (OK) con un corpo della risposta

Il `Get` chiamate al metodo `Ok(product)` se viene trovato il prodotto. Nello unit test, assicurarsi che il tipo restituito sia **OkNegotiatedContentResult** e il prodotto restituito con l'ID corretto.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Si noti che lo unit test non viene eseguito il risultato dell'azione. Si può presupporre che il risultato dell'azione consente di creare la risposta HTTP corretta. (Questo motivo il framework API Web ha un proprio unit test!)

### <a name="action-returns-404-not-found"></a>Azione restituisce 404 (non trovato)

Il `Get` chiamate al metodo `NotFound()` se il prodotto non viene trovato. In questo caso, lo unit test verifica solo se il tipo restituito è **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Azione restituisce 200 (OK) senza corpo della risposta

Il `Delete` chiamate al metodo `Ok()` per restituire una risposta HTTP 200 vuota. Uguale all'esempio precedente, lo unit test verifica il tipo restituito, in questo caso **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Azione restituisce 201 (creato) con un'intestazione di posizione

Il `Post` chiamate al metodo `CreatedAtRoute` per restituire una risposta HTTP 201 con l'URI nell'intestazione Location. Nello unit test, verificare che l'azione imposta i valori di routing corretti.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Azione restituisce un'altra 2xx con un corpo della risposta

Il `Put` chiamate al metodo `Content` per restituire una risposta HTTP 202 (accettato) con un corpo della risposta. In questo caso è simile a restituire 200 (OK), ma lo unit test dovrà verificare anche il codice di stato.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Comportamento fittizio di Entity Framework quando gli Unit test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Scrittura di test per un servizio API Web ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (post di blog di Youssef Moussaoui).
- [Debug di ASP.NET Web API with Route Debugger](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
