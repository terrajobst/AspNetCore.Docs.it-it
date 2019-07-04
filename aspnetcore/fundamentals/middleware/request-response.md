---
title: Operazioni di richiesta e risposta in ASP.NET Core
author: jkotalik
description: Informazioni su come leggere il corpo della richiesta e scrivere il corpo della risposta in ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: 0c321dad256e239b61907980c09d2c088c1407ff
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538581"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>Operazioni di richiesta e risposta in ASP.NET Core

Di [Justin Kotalik](https://github.com/jkotalik)

Questo articolo illustra come leggere dal corpo della richiesta e scrivere nel corpo della risposta. Potrebbe essere necessario scrivere codice per queste operazioni durante la scrittura di middleware. In caso contrario, in genere non è necessario scrivere questo codice, perché le operazioni sono gestite da MVC e Razor Pages.

In ASP.NET Core 3.0 sono disponibili due astrazioni per i corpi di richiesta e risposta: <xref:System.IO.Stream> e <xref:System.IO.Pipelines.Pipe>. Per la lettura della richiesta, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) è un <xref:System.IO.Stream> e `HttpRequest.BodyPipe` è un <xref:System.IO.Pipelines.PipeReader>. Per la scrittura della risposta, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) è e `HttpResponse.BodyPipe` è un <xref:System.IO.Pipelines.PipeWriter>.

Si consiglia di preferire le pipeline rispetto ai flussi. I flussi possono essere più facili da usare per alcune operazioni semplici, ma le pipeline hanno prestazioni migliori e sono più facili da usare nella maggior parte degli scenari. Nella versione 3.0, ASP.NET Core inizia a usare le pipeline al posto dei flussi internamente. Alcuni esempi:

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

I flussi sono destinati a rimanere e continuano a essere usati in tutto .NET. Per molti tipi di flussi non esistono equivalenti pipe, ad esempio `FileStreams` e `ResponseCompression`.

## <a name="stream-examples"></a>Esempi di flussi

Si supponga di voler creare un middleware che legge l'intero corpo della richiesta come un elenco di stringhe, suddividendolo in corrispondenza delle nuove righe. Un'implementazione di flusso semplice potrebbe essere simile alla seguente:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

Questo codice funziona, ma esistono alcuni problemi:

- Prima dell'aggiunta a `StringBuilder`, l'esempio crea un'altra stringa (`encodedString`) che viene eliminata immediatamente. Questo processo si verifica per tutti i byte nel flusso, pertanto il risultato è l'allocazione di memoria aggiuntiva rispetto alle dimensioni dell'intero corpo della richiesta.
- L'esempio legge l'intera stringa prima della suddivisione in corrispondenza delle nuove righe. Sarebbe più efficiente verificare la presenza di nuove righe nella matrice di byte.

Di seguito è riportato un esempio che consente di risolvere alcuni di questi problemi:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

Questo esempio:

- Non memorizza nel buffer l'intero corpo della richiesta in un `StringBuilder`, a meno che non siano presenti caratteri di nuova riga.
- Non chiama `Split` sulla stringa.

Tuttavia, esistono ancora sono alcuni problemi:

- Se i caratteri di nuova riga sono scarsi, gran parte del corpo della richiesta viene memorizzato nel buffer nella stringa.
- Vengono ancora create stringhe (`remainingString`) e tali stringhe vengono aggiunte al buffer, con conseguente allocazione aggiuntiva.

Questi problemi sono risolvibili, ma il codice sta diventando sempre più complicato con miglioramenti minimi. Le pipeline consentono di risolvere questi problemi con complicazioni minime per il codice.

## <a name="pipelines"></a>Pipeline

L'esempio seguente mostra come è possibile gestire lo stesso scenario con un `PipeReader`:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

Questo esempio consente di risolvere molti problemi delle implementazioni dei flussi:

- Non è necessario un buffer di stringhe perché `PipeReader` gestisce i byte inutilizzati.
- Le stringhe codificate vengono aggiunte direttamente all'elenco di stringhe restituite.
- La creazione di stringhe non comporta allocazioni, oltre alla memoria usata dalla stringa (ad eccezione della chiamata `ToArray()`).

## <a name="adapters"></a>Adattatori

Ora che entrambe le proprietà `Body` e `BodyPipe` sono disponibili per `HttpRequest` e `HttpResponse`, cosa accade quando si imposta `Body` su un flusso diverso? Nella versione 3.0 un nuovo set di adattatori consente di adattare automaticamente ogni tipo all'altro. Ad esempio, se si imposta `HttpRequest.Body` su un nuovo flusso, `HttpRequest.BodyPipe` viene impostato automaticamente su un nuovo `PipeReader` che esegue il wrapping di `HttpRequest.Body`. Lo stesso comportamento si applica all'impostazione della proprietà `BodyPipe`. Se `HttpResponse.BodyPipe` viene impostato su un nuovo `PipeWriter`, `HttpResponse.Body` viene impostato automaticamente su un nuovo flusso che esegue il wrapping di `HttpResponse.BodyPipe`.

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync` è una novità della versione 3.0. Viene usato per indicare che le intestazioni non sono modificabili e per eseguire callback `OnStarting`. Nella versione 3.0 Preview 3, è necessario chiamare `StartAsync` prima di usare `HttpRequest.BodyPipe`. Nelle versioni future, questa sarà una raccomandazione. Quando si usa Kestrel come server, la chiamata di StartAsync prima di usare `PipeReader` garantisce che la memoria restituita da `GetMemory` appartenga alla <xref:System.IO.Pipelines.Pipe> interna di Kestrel anziché a un buffer esterno.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) (Introduzione a System.IO.Pipelines)
- <xref:fundamentals/middleware/write>