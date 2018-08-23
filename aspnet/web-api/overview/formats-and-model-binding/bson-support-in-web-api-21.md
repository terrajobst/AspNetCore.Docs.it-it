---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Supporto di BSON nell'API Web ASP.NET 2.1 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 709fb0266c0725176358a1bd0d08b3e07fa6e2a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839027"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Supporto di BSON nell'API Web ASP.NET 2.1
====================
da [Mike Wasson](https://github.com/MikeWasson)

API Web 2.1 introduce il supporto di BSON. In questo argomento viene illustrato come utilizzare BSON nel controller dell'API Web (lato server) e in un'app client .NET.

## <a name="what-is-bson"></a>Che cos'è BSON?

[BSON](http://bsonspec.org/) è un formato di serializzazione binaria. "BSON" è l'acronimo di "Binary JSON", ma BSON e JSON vengono serializzati in modo molto diverso. BSON è "JSON simile a", perché gli oggetti sono rappresentati come coppie nome-valore, simile al JSON. A differenza di JSON, tipi di dati numerici vengono archiviati in byte, non le stringhe

BSON è stato progettato per essere leggero, facile da analizzare e veloci da codificare/decodificare.

- BSON è dimensioni simili al JSON. A seconda dei dati, un payload BSON può essere minori o maggiori di un payload JSON. Per la serializzazione di dati binari, ad esempio un file di immagine, BSON è inferiore a JSON, perché i dati binari non sono con codifica base64.
- Documenti BSON sono facili da analizzare, perché gli elementi sono preceduti da un campo di lunghezza, in modo che un parser può ignorare gli elementi senza decodifica li.
- Codifica e decodifica sono efficienti, poiché i tipi di dati numerici vengono archiviati come numeri, non le stringhe.

I client nativi, ad esempio le app client .NET, possono trarre vantaggio dall'utilizzo BSON al posto di formati basati su testo come JSON o XML. Per i client del browser, è probabile che si desideri utilizzare JSON, perché JavaScript è possibile convertire direttamente il payload JSON.

Fortunatamente, API Web Usa [negoziazione del contenuto](content-negotiation.md), in modo che l'API può supportare entrambi i formati e consentire al client scegliere.

## <a name="enabling-bson-on-the-server"></a>Abilitazione di BSON nel Server

Nella configurazione dell'API Web, aggiungere il **BsonMediaTypeFormatter** alla raccolta di formattatori.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

A questo punto se il client richiede "application/bson", API Web Usa il formattatore BSON.

Per associare altri tipi di supporto BSON, aggiungerli alla raccolta SupportedMediaTypes. Il codice seguente aggiunge "application/vnd.contoso" per tipi di supporto:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Sessione HTTP di esempio

In questo esempio si userà la classe di modello seguenti oltre a un controller API Web semplice:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Un client può inviare la richiesta HTTP seguente:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Ecco la risposta:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Qui ho sostituito i dati binari con &quot;.&quot; caratteri. Lo screenshot seguente illustrato Fiddler i valori esadecimali non elaborati.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Uso di BSON con HttpClient

Le app client .NET possono usare il formattatore BSON con **HttpClient**. Per altre informazioni sulle **HttpClient**, vedere [la chiamata a una Web API da un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Il codice seguente invia una richiesta GET che accetta BSON e quindi deserializza il payload BSON nella risposta.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Per richiedere BSON dal server, impostare l'intestazione Accept su "application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Per deserializzare il corpo della risposta, usare il **BsonMediaTypeFormatter**. Questo formattatore non è nella raccolta di formattatori predefinita, pertanto è necessario specificarlo quando si leggono il corpo della risposta:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

L'esempio seguente viene illustrato come inviare una richiesta POST contenente BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Gran parte di questo codice è lo stesso dell'esempio precedente. Ma nel **PostAsync** metodo, specificare **BsonMediaTypeFormatter** come il formattatore:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>La serializzazione di tipi primitivi di primo livello

Ogni documento BSON è un elenco di coppie chiave/valore. La specifica di BSON non definisce una sintassi per la serializzazione di un singolo valore non elaborato, ad esempio un numero intero o stringa.

Per aggirare questa limitazione, il **BsonMediaTypeFormatter** considera i tipi primitivi come un caso speciale. Prima della serializzazione, converte il valore nella coppia chiave/valore con la chiave "Value". Si supponga, ad esempio, che il controller API restituisce un valore integer:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Prima della serializzazione, il formattatore BSON Converte questa per la coppia chiave/valore seguente:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Quando si esegue la deserializzazione, il formattatore converte i dati il valore originale. Tuttavia, i client che usano un parser BSON saranno necessario gestire questa situazione, se l'API web restituisce i valori non elaborati. In generale, è necessario considerare la restituzione di dati strutturati, anziché i valori non elaborati.

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di BSON API Web](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Formattatori di file multimediali](media-formatters.md)
