---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Azione risultante nell'API Web 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 7726829ac9eba339ff3ac1c94c86132cb1090783
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395529"
---
<a name="action-results-in-web-api-2"></a>Risultati dell'azione nell'API Web 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

In questo argomento viene descritto come API Web ASP.NET converte il valore restituito da un'azione del controller in un messaggio di risposta HTTP.

Un'azione del controller API Web può restituire uno dei seguenti:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Un altro tipo

A seconda di quale di questi viene restituito, API Web Usa un meccanismo diverso per creare la risposta HTTP.

| Tipo restituito | Come API Web crea la risposta |
| --- | --- |
| void | Restituire vuoto 204 (nessun contenuto) |
| **HttpResponseMessage** | Convertire direttamente in un messaggio di risposta HTTP. |
| **IHttpActionResult** | Chiamare **ExecuteAsync** per creare un **HttpResponseMessage**, quindi convertire in un messaggio di risposta HTTP. |
| Altro tipo | Scrivere il valore restituito serializzato nel corpo della risposta; Restituisce il codice 200 (OK). |

La parte restante di questo argomento descrive ogni opzione in modo più dettagliato.

## <a name="void"></a>void

Se il tipo restituito è `void`, API Web restituisce semplicemente una risposta HTTP vuota con il codice di stato 204 (nessun contenuto).

Controller di esempio:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Risposta HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Se l'azione restituisce un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), API Web converte il valore restituito direttamente in un messaggio di risposta HTTP, utilizzando le proprietà delle **HttpResponseMessage** oggetto per popolare il risposta.

Questa opzione offre un notevole controllo sul messaggio di risposta. Ad esempio, l'azione del controller seguente imposta l'intestazione Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Risposta:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Se si passa un modello di dominio per il **CreateResponse** metodo, API Web Usa un [formattatore di media](../formats-and-model-binding/media-formatters.md) per scrivere il modello serializzato nel corpo della risposta.

[!code-csharp[Main](action-results/samples/sample5.cs)]

API Web Usa l'intestazione Accept nella richiesta per scegliere il formattatore. Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

Il **IHttpActionResult** interfaccia è stato introdotto in API Web 2. In pratica, definisce un **HttpResponseMessage** factory. Di seguito sono riportati alcuni vantaggi dell'uso di **IHttpActionResult** interfaccia:

- Consente di semplificare [unit test](../testing-and-debugging/unit-testing-controllers-in-web-api.md) i controller.
- Sposta la logica comune per la creazione di risposte HTTP in classi separate.
- Rende l'intento dell'azione del controller più chiara, nascondendo i dettagli di basso livello di costruire la risposta.

**IHttpActionResult** contiene un solo metodo, **ExecuteAsync**, che crea in modo asincrono un' **HttpResponseMessage** istanza.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Se un'azione del controller restituisce un **IHttpActionResult**, chiamate all'API Web di **ExecuteAsync** metodo per creare un **HttpResponseMessage**. Viene poi convertita la **HttpResponseMessage** in un messaggio di risposta HTTP.

Ecco una semplice implementazione di **IHttpActionResult** che crea una risposta di testo normale:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Azione del controller di esempio:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Risposta:

[!code-console[Main](action-results/samples/sample9.cmd)]

Più spesso, si userà il **IHttpActionResult** implementazioni definite nel **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** dello spazio dei nomi. Il **ApiController** classe definisce i metodi helper che restituiscono queste risultati dell'azione predefinita.

Nell'esempio seguente, se la richiesta non corrisponde a un ID prodotto esistente, il controller chiama [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) per creare una risposta 404 (non trovato). In caso contrario, il controller chiama [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), che consente di creare una risposta 200 (OK) che contiene il prodotto.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Altri tipi restituiti

Per tutti gli altri tipi restituiti, API Web Usa un [formattatore di media](../formats-and-model-binding/media-formatters.md) per serializzare il valore restituito. API Web scrive il valore serializzato nel corpo della risposta. Il codice di stato della risposta è 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Uno svantaggio di questo approccio è che è possibile restituire direttamente un codice di errore, ad esempio 404. Tuttavia, è possibile generare una **HttpResponseException** i codici di errore. Per altre informazioni, vedere [gestione delle eccezioni nell'API Web ASP.NET](../error-handling/exception-handling.md).

API Web Usa l'intestazione Accept nella richiesta per scegliere il formattatore. Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).

Richiesta di esempio

[!code-console[Main](action-results/samples/sample12.cmd)]

Esempio di risposta:

[!code-console[Main](action-results/samples/sample13.cmd)]
