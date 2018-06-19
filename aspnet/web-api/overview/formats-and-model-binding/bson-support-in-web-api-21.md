---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Supporto BSON in ASP.NET Web API 2.1 | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2018
ms.locfileid: "27984583"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Supporto BSON in ASP.NET Web API 2.1
====================
da [Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 introduce il supporto per BSON. In questo argomento viene illustrato come utilizzare BSON nel controller di Web API (lato server) in un'app client .NET.

## <a name="what-is-bson"></a>Che cos'è BSON?

[BSON](http://bsonspec.org/) è un formato di serializzazione binaria. "BSON" è l'acronimo di "JSON binario", ma BSON e JSON vengono serializzati in modo molto diverso. BSON è "JSON-like", poiché gli oggetti sono rappresentati come coppie nome-valore, simile a JSON. A differenza di JSON, tipi di dati numerici vengono archiviati come byte non di stringhe

BSON è stato progettato per essere leggero, facile da esaminare e veloce per codificare o decodificare.

- BSON è paragonabile dimensioni in formato JSON. A seconda dei dati, un payload BSON può essere minori o maggiori di un payload JSON. Per la serializzazione di dati binari, ad esempio un file di immagine, BSON è inferiore a JSON, perché i dati binari non sono con codifica base64.
- Documenti BSON sono facili da analizzare, perché gli elementi sono preceduti da un campo di lunghezza, in modo da un parser può ignorare gli elementi senza decodifica li.
- Codifica e decodifica sono efficienti, perché i tipi di dati numerici vengono archiviati come numeri, non le stringhe.

Native client, ad esempio di App client .NET, possono trarre vantaggio dall'utilizzo di BSON al posto di formati basati su testo, ad esempio JSON o XML. Per i client del browser, probabilmente si desidera utilizzare JSON, perché JavaScript è possibile convertire direttamente il payload JSON.

Fortunatamente, l'API Web utilizza [negoziazione del contenuto](content-negotiation.md), in modo che l'API può supportare entrambi i formati e consentire di scegliere il client.

## <a name="enabling-bson-on-the-server"></a>Abilitazione di BSON sul Server

Nella configurazione dell'API Web, aggiungere il **BsonMediaTypeFormatter** alla raccolta di formattatori.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

A questo punto se il client richiede "application/bson", API Web utilizzerà il formattatore BSON.

Per associare BSON con altri tipi di supporti, è necessario aggiungerli alla raccolta SupportedMediaTypes. Il codice seguente aggiunge "application/vnd.contoso" per i tipi di supporto:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Sessione HTTP di esempio

In questo esempio verrà utilizzata la classe di modello seguente più controller API Web semplice:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Un client può inviare la richiesta HTTP seguente:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Di seguito è riportata la risposta:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Ho sostituito qui i dati binari con &quot;.&quot; caratteri. La seguente schermata illustrato Fiddler i valori esadecimali non elaborati.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Utilizzo di BSON con HttpClient

App client .NET è possibile utilizzare il formattatore BSON e **HttpClient**. Per ulteriori informazioni su **HttpClient**, vedere [la chiamata a una Web API da un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Il codice seguente invia una richiesta GET che accetta BSON e quindi deserializza il payload BSON nella risposta.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Per richiedere BSON dal server, impostare l'intestazione Accept su "application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Per deserializzare il corpo della risposta, utilizzare il **BsonMediaTypeFormatter**. Questo formattatore non è presente nella raccolta di formattatori predefinito, è necessario specificarlo quando si leggono il corpo della risposta:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Nell'esempio seguente viene illustrato come inviare una richiesta POST contenente BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Gran parte di questo codice è uguale all'esempio precedente. Ma nel **PostAsync** (metodo), specificare **BsonMediaTypeFormatter** come il formattatore:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>La serializzazione di tipi primitivi di primo livello

Ogni documento BSON è un elenco di coppie chiave/valore. La specifica di BSON non definisce una sintassi per la serializzazione di un singolo valore non elaborato, ad esempio un numero intero o stringa.

Per aggirare questa limitazione, il **BsonMediaTypeFormatter** considera i tipi primitivi come un caso speciale. Prima della serializzazione, converte il valore in una coppia chiave/valore con la chiave "Value". Ad esempio, si supponga che il controller API restituisce un valore integer:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Prima della serializzazione, il formattatore BSON converte questo nella coppia chiave/valore seguenti:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Quando si esegue la deserializzazione, il formattatore converte i dati il valore originale. Tuttavia, i client che usano un parser BSON saranno necessario gestire questa situazione, se l'API web restituisce i valori non elaborati. In generale, è consigliabile la restituzione di dati strutturati, piuttosto che i valori non elaborati.

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio BSON API Web](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Formattatori di Media](media-formatters.md)
