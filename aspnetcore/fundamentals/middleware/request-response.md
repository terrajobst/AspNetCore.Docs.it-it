---
title: Operazioni di richiesta e risposta in ASP.NET Core
author: jkotalik
description: Informazioni su come leggere il corpo della richiesta e scrivere il corpo della risposta in ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b473fa02e1d23f02bc5d2e15fa54ab7b1dbbb17c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667216"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>Operazioni di richiesta e risposta in ASP.NET Core

Di [Justin Kotalik](https://github.com/jkotalik)

Questo articolo illustra come leggere dal corpo della richiesta e scrivere nel corpo della risposta. Il codice per queste operazioni potrebbe essere necessario quando si scrive il middleware. Al di fuori della scrittura del middleware, il codice personalizzato non è in genere necessario perché le operazioni vengono gestite da MVC e Razor Pages.

Esistono due astrazioni per i corpi di richiesta e risposta: <xref:System.IO.Stream> e <xref:System.IO.Pipelines.Pipe>. Per la lettura della richiesta, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) è un <xref:System.IO.Stream> e `HttpRequest.BodyReader` è un <xref:System.IO.Pipelines.PipeReader>. Per la scrittura della risposta, [HttpResponse. Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) è un <xref:System.IO.Stream>e `HttpResponse.BodyWriter` è un <xref:System.IO.Pipelines.PipeWriter>.

Le [pipeline](/dotnet/standard/io/pipelines) sono consigliate per i flussi. I flussi possono essere più facili da usare per alcune operazioni semplici, ma le pipeline hanno prestazioni migliori e sono più facili da usare nella maggior parte degli scenari. ASP.NET Core sta iniziando a usare le pipeline anziché i flussi internamente. Tra gli esempi sono inclusi:

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

I flussi non vengono rimossi dal Framework. I flussi continuano a essere usati in .NET e molti tipi di flusso non hanno equivalenti di pipe, ad esempio `FileStreams` e `ResponseCompression`.

## <a name="stream-examples"></a>Esempi di flussi

Si supponga che l'obiettivo sia quello di creare un middleware che legga l'intero corpo della richiesta come un elenco di stringhe, suddividendo le nuove righe. Un'implementazione di flusso semplice potrebbe essere simile alla seguente:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

Questo codice funziona, ma esistono alcuni problemi:

* Prima dell'aggiunta a `StringBuilder`, l'esempio crea un'altra stringa (`encodedString`) che viene eliminata immediatamente. Questo processo si verifica per tutti i byte nel flusso, pertanto il risultato è l'allocazione di memoria aggiuntiva rispetto alle dimensioni dell'intero corpo della richiesta.
* L'esempio legge l'intera stringa prima della suddivisione in corrispondenza delle nuove righe. È più efficiente verificare la presenza di nuove righe nella matrice di byte.

Di seguito è riportato un esempio in cui vengono corretti alcuni dei problemi precedenti:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

Questo esempio precedente:

* Non memorizza nel buffer l'intero corpo della richiesta in un `StringBuilder`, a meno che non siano presenti caratteri di nuova riga.
* Non chiama `Split` sulla stringa.

Tuttavia, esistono ancora sono alcuni problemi:

* Se i caratteri di nuova riga sono di tipo sparse, gran parte del corpo della richiesta viene memorizzato nel buffer nella stringa.
* Il codice continua a creare stringhe (`remainingString`) e le aggiunge al buffer di stringa, il che comporta un'allocazione aggiuntiva.

Questi problemi sono risolvibili, ma il codice sta diventando progressivamente più complicato con un lieve miglioramento. Le pipeline consentono di risolvere questi problemi con complicazioni minime per il codice.

## <a name="pipelines"></a>Pipeline

L'esempio seguente mostra come è possibile gestire lo stesso scenario con un `PipeReader`:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

Questo esempio consente di risolvere molti problemi delle implementazioni dei flussi:

* Non è necessario un buffer di stringa perché il `PipeReader` gestisce i byte che non sono stati usati.
* Le stringhe codificate vengono aggiunte direttamente all'elenco di stringhe restituite.
* La creazione di stringhe non comporta allocazioni, oltre alla memoria usata dalla stringa (ad eccezione della chiamata `ToArray()`).

## <a name="adapters"></a>adapter

Le proprietà `Body` e `BodyReader/BodyWriter` sono disponibili per `HttpRequest` e `HttpResponse`. Quando si imposta `Body` su un flusso diverso, un nuovo set di adattatori adatta automaticamente ogni tipo all'altro. Se si imposta `HttpRequest.Body` su un nuovo flusso, `HttpRequest.BodyReader` viene impostato automaticamente su un nuovo `PipeReader` che esegue il wrapping `HttpRequest.Body`.

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync` viene utilizzato per indicare che le intestazioni non sono modificabili e per eseguire `OnStarting` callback. Quando si usa gheppio come server, la chiamata di `StartAsync` prima di usare il `PipeReader` garantisce che la memoria restituita da `GetMemory` appartenga al <xref:System.IO.Pipelines.Pipe> interno di Gheppio anziché a un buffer esterno.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) (Introduzione a System.IO.Pipelines)
* <xref:fundamentals/middleware/write>
