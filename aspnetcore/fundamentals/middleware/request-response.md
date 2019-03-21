---
title: Operazioni di richiesta e risposta in ASP.NET Core
author: jkotalik
description: Informazioni su come leggere il corpo della richiesta e scrivere il corpo della risposta in ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jkotalik
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b6e3cd4b79e0c062b271c65cd5ecbdb4ef80c3a1
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208057"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="3edac-103">Operazioni di richiesta e risposta in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3edac-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="3edac-104">Di [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="3edac-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="3edac-105">Questo articolo illustra come leggere dal corpo della richiesta e scrivere nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="3edac-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="3edac-106">Potrebbe essere necessario scrivere codice per queste operazioni durante la scrittura di middleware.</span><span class="sxs-lookup"><span data-stu-id="3edac-106">You might need to write code for these operations when you're writing middleware.</span></span> <span data-ttu-id="3edac-107">In caso contrario, in genere non è necessario scrivere questo codice, perché le operazioni sono gestite da MVC e Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3edac-107">Otherwise, you typically don't have to write this code because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="3edac-108">In ASP.NET Core 3.0 sono disponibili due astrazioni per i corpi di richiesta e risposta: <xref:System.IO.Stream> e <xref:System.IO.Pipelines.Pipe>.</span><span class="sxs-lookup"><span data-stu-id="3edac-108">In ASP.NET Core 3.0, there are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="3edac-109">Per la lettura della richiesta, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) è un <xref:System.IO.Stream> e `HttpRequest.BodyPipe` è un <xref:System.IO.Pipelines.PipeReader>.</span><span class="sxs-lookup"><span data-stu-id="3edac-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyPipe` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="3edac-110">Per la scrittura della risposta, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) è e `HttpResponse.BodyPipe` è un <xref:System.IO.Pipelines.PipeWriter>.</span><span class="sxs-lookup"><span data-stu-id="3edac-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a `HttpResponse.BodyPipe` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="3edac-111">Si consiglia di preferire le pipeline rispetto ai flussi.</span><span class="sxs-lookup"><span data-stu-id="3edac-111">We recommend pipelines over streams.</span></span> <span data-ttu-id="3edac-112">I flussi possono essere più facili da usare per alcune operazioni semplici, ma le pipeline hanno prestazioni migliori e sono più facili da usare nella maggior parte degli scenari.</span><span class="sxs-lookup"><span data-stu-id="3edac-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="3edac-113">Nella versione 3.0, ASP.NET Core inizia a usare le pipeline al posto dei flussi internamente.</span><span class="sxs-lookup"><span data-stu-id="3edac-113">In 3.0, ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="3edac-114">Alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="3edac-114">Examples include:</span></span>

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

<span data-ttu-id="3edac-115">I flussi sono destinati a rimanere e</span><span class="sxs-lookup"><span data-stu-id="3edac-115">Streams aren't going away.</span></span> <span data-ttu-id="3edac-116">continuano a essere usati in tutto .NET. Per molti tipi di flussi non esistono equivalenti pipe, ad esempio `FileStreams` e `ResponseCompression`.</span><span class="sxs-lookup"><span data-stu-id="3edac-116">They continue to be used throughout .NET, and many stream types don't have pipe equivalents, like `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="3edac-117">Esempi di flussi</span><span class="sxs-lookup"><span data-stu-id="3edac-117">Stream examples</span></span>

<span data-ttu-id="3edac-118">Si supponga di voler creare un middleware che legge l'intero corpo della richiesta come un elenco di stringhe, suddividendolo in corrispondenza delle nuove righe.</span><span class="sxs-lookup"><span data-stu-id="3edac-118">Suppose you want to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="3edac-119">Un'implementazione di flusso semplice potrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="3edac-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="3edac-120">Questo codice funziona, ma esistono alcuni problemi:</span><span class="sxs-lookup"><span data-stu-id="3edac-120">This code works, but there are some issues:</span></span>

- <span data-ttu-id="3edac-121">Prima dell'aggiunta a `StringBuilder`, l'esempio crea un'altra stringa (`encodedString`) che viene eliminata immediatamente.</span><span class="sxs-lookup"><span data-stu-id="3edac-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="3edac-122">Questo processo si verifica per tutti i byte nel flusso, pertanto il risultato è l'allocazione di memoria aggiuntiva rispetto alle dimensioni dell'intero corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="3edac-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
- <span data-ttu-id="3edac-123">L'esempio legge l'intera stringa prima della suddivisione in corrispondenza delle nuove righe.</span><span class="sxs-lookup"><span data-stu-id="3edac-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="3edac-124">Sarebbe più efficiente verificare la presenza di nuove righe nella matrice di byte.</span><span class="sxs-lookup"><span data-stu-id="3edac-124">It would be more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="3edac-125">Di seguito è riportato un esempio che consente di risolvere alcuni di questi problemi:</span><span class="sxs-lookup"><span data-stu-id="3edac-125">Here's an example that fixes some of these issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="3edac-126">Questo esempio:</span><span class="sxs-lookup"><span data-stu-id="3edac-126">This example:</span></span>

- <span data-ttu-id="3edac-127">Non memorizza nel buffer l'intero corpo della richiesta in un `StringBuilder`, a meno che non siano presenti caratteri di nuova riga.</span><span class="sxs-lookup"><span data-stu-id="3edac-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
- <span data-ttu-id="3edac-128">Non chiama `Split` sulla stringa.</span><span class="sxs-lookup"><span data-stu-id="3edac-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="3edac-129">Tuttavia, esistono ancora sono alcuni problemi:</span><span class="sxs-lookup"><span data-stu-id="3edac-129">However, there are still are a few issues:</span></span>

- <span data-ttu-id="3edac-130">Se i caratteri di nuova riga sono scarsi, gran parte del corpo della richiesta viene memorizzato nel buffer nella stringa.</span><span class="sxs-lookup"><span data-stu-id="3edac-130">If newline characters are sparse, much of the request body is buffered in the string .</span></span>
- <span data-ttu-id="3edac-131">Vengono ancora create stringhe (`remainingString`) e tali stringhe vengono aggiunte al buffer, con conseguente allocazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="3edac-131">It still creates strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="3edac-132">Questi problemi sono risolvibili, ma il codice sta diventando sempre più complicato con miglioramenti minimi.</span><span class="sxs-lookup"><span data-stu-id="3edac-132">These issues are fixable, but the code is becoming more and more complicated with little improvement.</span></span> <span data-ttu-id="3edac-133">Le pipeline consentono di risolvere questi problemi con complicazioni minime per il codice.</span><span class="sxs-lookup"><span data-stu-id="3edac-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="3edac-134">Pipeline</span><span class="sxs-lookup"><span data-stu-id="3edac-134">Pipelines</span></span>

<span data-ttu-id="3edac-135">L'esempio seguente mostra come è possibile gestire lo stesso scenario con un `PipeReader`:</span><span class="sxs-lookup"><span data-stu-id="3edac-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="3edac-136">Questo esempio consente di risolvere molti problemi delle implementazioni dei flussi:</span><span class="sxs-lookup"><span data-stu-id="3edac-136">This example fixes many issues that the streams implementations had:</span></span>

- <span data-ttu-id="3edac-137">Non è necessario un buffer di stringhe perché `PipeReader` gestisce i byte inutilizzati.</span><span class="sxs-lookup"><span data-stu-id="3edac-137">There is no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
- <span data-ttu-id="3edac-138">Le stringhe codificate vengono aggiunte direttamente all'elenco di stringhe restituite.</span><span class="sxs-lookup"><span data-stu-id="3edac-138">Encoded strings are directly added to the list of returned strings.</span></span>
- <span data-ttu-id="3edac-139">La creazione di stringhe non comporta allocazioni, oltre alla memoria usata dalla stringa (ad eccezione della chiamata `ToArray()`).</span><span class="sxs-lookup"><span data-stu-id="3edac-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="3edac-140">Adattatori</span><span class="sxs-lookup"><span data-stu-id="3edac-140">Adapters</span></span>

<span data-ttu-id="3edac-141">Ora che entrambe le proprietà `Body` e `BodyPipe` sono disponibili per `HttpRequest` e `HttpResponse`, cosa accade quando si imposta `Body` su un flusso diverso?</span><span class="sxs-lookup"><span data-stu-id="3edac-141">Now that both `Body` and `BodyPipe` properties are available for `HttpRequest` and `HttpResponse`, what happens when you set `Body` to a different stream?</span></span> <span data-ttu-id="3edac-142">Nella versione 3.0 un nuovo set di adattatori consente di adattare automaticamente ogni tipo all'altro.</span><span class="sxs-lookup"><span data-stu-id="3edac-142">In 3.0, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="3edac-143">Ad esempio, se si imposta `HttpRequest.Body` su un nuovo flusso, `HttpRequest.BodyPipe` viene impostato automaticamente su un nuovo `PipeReader` che esegue il wrapping di `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="3edac-143">For example, if you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyPipe` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span> <span data-ttu-id="3edac-144">Lo stesso comportamento si applica all'impostazione della proprietà `BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="3edac-144">The same behavior applies to setting the `BodyPipe` property.</span></span> <span data-ttu-id="3edac-145">Se `HttpResponse.BodyPipe` viene impostato su un nuovo `PipeWriter`, `HttpResponse.Body` viene impostato automaticamente su un nuovo flusso che esegue il wrapping di `HttpResponse.BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="3edac-145">If `HttpResponse.BodyPipe` is set to a new `PipeWriter`, the `HttpResponse.Body` is automatically set to a new stream that wraps `HttpResponse.BodyPipe`.</span></span>

## <a name="startasync"></a><span data-ttu-id="3edac-146">StartAsync</span><span class="sxs-lookup"><span data-stu-id="3edac-146">StartAsync</span></span>

<span data-ttu-id="3edac-147">`HttpResponse.StartAsync` è una novità della versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="3edac-147">`HttpResponse.StartAsync` is new in 3.0.</span></span> <span data-ttu-id="3edac-148">Viene usato per indicare che le intestazioni non sono modificabili e per eseguire callback `OnStarting`.</span><span class="sxs-lookup"><span data-stu-id="3edac-148">It is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="3edac-149">Nella versione 3.0 Preview 3, è necessario chiamare `StartAsync` prima di usare `HttpRequest.BodyPipe`. Nelle versioni future, questa sarà una raccomandazione.</span><span class="sxs-lookup"><span data-stu-id="3edac-149">In 3.0-preview3, you have to call `StartAsync` before using `HttpRequest.BodyPipe`, and in future releases, it will be a recommendation.</span></span> <span data-ttu-id="3edac-150">Quando si usa Kestrel come server, la chiamata di StartAsync prima di usare `PipeReader` garantisce che la memoria restituita da `GetMemory` appartenga alla <xref:System.IO.Pipelines.Pipe> interna di Kestrel anziché a un buffer esterno.</span><span class="sxs-lookup"><span data-stu-id="3edac-150">When using Kestrel as a server, calling StartAsync before using the `PipeReader` guarantees that memory returned by `GetMemory` will belong to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3edac-151">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3edac-151">Additional resources</span></span>

- <span data-ttu-id="3edac-152">[Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) (Introduzione a System.IO.Pipelines)</span><span class="sxs-lookup"><span data-stu-id="3edac-152">[Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)</span></span>
- <xref:fundamentals/middleware/write>