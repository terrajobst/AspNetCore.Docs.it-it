---
title: Operazioni di richiesta e risposta in ASP.NET Core
author: jkotalik
description: Informazioni su come leggere il corpo della richiesta e scrivere il corpo della risposta in ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: 5e531c0ce0ed48097054fd81ddc3655a66cc7c5f
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081676"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="f7b90-103">Operazioni di richiesta e risposta in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7b90-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="f7b90-104">Di [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="f7b90-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="f7b90-105">Questo articolo illustra come leggere dal corpo della richiesta e scrivere nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="f7b90-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="f7b90-106">Il codice per queste operazioni potrebbe essere necessario quando si scrive il middleware.</span><span class="sxs-lookup"><span data-stu-id="f7b90-106">Code for these operations might be required when writing middleware.</span></span> <span data-ttu-id="f7b90-107">Al di fuori della scrittura del middleware, il codice personalizzato non è in genere necessario perché le operazioni vengono gestite da MVC e Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f7b90-107">Outside of writing middleware, custom code isn't generally required because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="f7b90-108">Esistono due astrazioni per i corpi di richiesta e risposta: <xref:System.IO.Stream> e <xref:System.IO.Pipelines.Pipe>.</span><span class="sxs-lookup"><span data-stu-id="f7b90-108">There are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="f7b90-109">Per la lettura della richiesta, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) è un <xref:System.IO.Stream> e `HttpRequest.BodyReader` è un <xref:System.IO.Pipelines.PipeReader>.</span><span class="sxs-lookup"><span data-stu-id="f7b90-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyReader` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="f7b90-110">Per la scrittura della risposta, [HttpResponse. Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) è <xref:System.IO.Stream>un `HttpResponse.BodyWriter` e è <xref:System.IO.Pipelines.PipeWriter>un.</span><span class="sxs-lookup"><span data-stu-id="f7b90-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a <xref:System.IO.Stream>, and `HttpResponse.BodyWriter` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="f7b90-111">Le pipeline sono consigliate per i flussi.</span><span class="sxs-lookup"><span data-stu-id="f7b90-111">Pipelines are recommended over streams.</span></span> <span data-ttu-id="f7b90-112">I flussi possono essere più facili da usare per alcune operazioni semplici, ma le pipeline hanno prestazioni migliori e sono più facili da usare nella maggior parte degli scenari.</span><span class="sxs-lookup"><span data-stu-id="f7b90-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="f7b90-113">ASP.NET Core sta iniziando a usare le pipeline anziché i flussi internamente.</span><span class="sxs-lookup"><span data-stu-id="f7b90-113">ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="f7b90-114">Ecco alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="f7b90-114">Examples include:</span></span>

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

<span data-ttu-id="f7b90-115">I flussi non vengono rimossi dal Framework.</span><span class="sxs-lookup"><span data-stu-id="f7b90-115">Streams aren't being removed from the framework.</span></span> <span data-ttu-id="f7b90-116">I flussi continuano a essere usati in .NET e molti tipi di flusso non hanno equivalenti di pipe, `FileStreams` ad `ResponseCompression`esempio e.</span><span class="sxs-lookup"><span data-stu-id="f7b90-116">Streams continue to be used throughout .NET, and many stream types don't have pipe equivalents, such as `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="f7b90-117">Esempi di flussi</span><span class="sxs-lookup"><span data-stu-id="f7b90-117">Stream examples</span></span>

<span data-ttu-id="f7b90-118">Si supponga che l'obiettivo sia quello di creare un middleware che legga l'intero corpo della richiesta come un elenco di stringhe, suddividendo le nuove righe.</span><span class="sxs-lookup"><span data-stu-id="f7b90-118">Suppose the goal is to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="f7b90-119">Un'implementazione di flusso semplice potrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="f7b90-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="f7b90-120">Questo codice funziona, ma esistono alcuni problemi:</span><span class="sxs-lookup"><span data-stu-id="f7b90-120">This code works, but there are some issues:</span></span>

* <span data-ttu-id="f7b90-121">Prima dell'aggiunta a `StringBuilder`, l'esempio crea un'altra stringa (`encodedString`) che viene eliminata immediatamente.</span><span class="sxs-lookup"><span data-stu-id="f7b90-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="f7b90-122">Questo processo si verifica per tutti i byte nel flusso, pertanto il risultato è l'allocazione di memoria aggiuntiva rispetto alle dimensioni dell'intero corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="f7b90-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
* <span data-ttu-id="f7b90-123">L'esempio legge l'intera stringa prima della suddivisione in corrispondenza delle nuove righe.</span><span class="sxs-lookup"><span data-stu-id="f7b90-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="f7b90-124">È più efficiente verificare la presenza di nuove righe nella matrice di byte.</span><span class="sxs-lookup"><span data-stu-id="f7b90-124">It's more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="f7b90-125">Di seguito è riportato un esempio in cui vengono corretti alcuni dei problemi precedenti:</span><span class="sxs-lookup"><span data-stu-id="f7b90-125">Here's an example that fixes some of the preceding issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="f7b90-126">Questo esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="f7b90-126">This preceding example:</span></span>

* <span data-ttu-id="f7b90-127">Non memorizza nel buffer l'intero corpo della richiesta in un `StringBuilder`, a meno che non siano presenti caratteri di nuova riga.</span><span class="sxs-lookup"><span data-stu-id="f7b90-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
* <span data-ttu-id="f7b90-128">Non chiama `Split` sulla stringa.</span><span class="sxs-lookup"><span data-stu-id="f7b90-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="f7b90-129">Tuttavia, esistono ancora sono alcuni problemi:</span><span class="sxs-lookup"><span data-stu-id="f7b90-129">However, there are still are a few issues:</span></span>

* <span data-ttu-id="f7b90-130">Se i caratteri di nuova riga sono di tipo sparse, gran parte del corpo della richiesta viene memorizzato nel buffer nella stringa.</span><span class="sxs-lookup"><span data-stu-id="f7b90-130">If newline characters are sparse, much of the request body is buffered in the string.</span></span>
* <span data-ttu-id="f7b90-131">Il codice continua a creare stringhe (`remainingString`) e le aggiunge al buffer di stringa, il che comporta un'allocazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="f7b90-131">The code continues to create strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="f7b90-132">Questi problemi sono risolvibili, ma il codice sta diventando progressivamente più complicato con un lieve miglioramento.</span><span class="sxs-lookup"><span data-stu-id="f7b90-132">These issues are fixable, but the code is becoming progressively more complicated with little improvement.</span></span> <span data-ttu-id="f7b90-133">Le pipeline consentono di risolvere questi problemi con complicazioni minime per il codice.</span><span class="sxs-lookup"><span data-stu-id="f7b90-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="f7b90-134">Pipeline</span><span class="sxs-lookup"><span data-stu-id="f7b90-134">Pipelines</span></span>

<span data-ttu-id="f7b90-135">L'esempio seguente mostra come è possibile gestire lo stesso scenario con un `PipeReader`:</span><span class="sxs-lookup"><span data-stu-id="f7b90-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="f7b90-136">Questo esempio consente di risolvere molti problemi delle implementazioni dei flussi:</span><span class="sxs-lookup"><span data-stu-id="f7b90-136">This example fixes many issues that the streams implementations had:</span></span>

* <span data-ttu-id="f7b90-137">Non è necessario un buffer di stringa perché gestisce i `PipeReader` byte che non sono stati usati.</span><span class="sxs-lookup"><span data-stu-id="f7b90-137">There's no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
* <span data-ttu-id="f7b90-138">Le stringhe codificate vengono aggiunte direttamente all'elenco di stringhe restituite.</span><span class="sxs-lookup"><span data-stu-id="f7b90-138">Encoded strings are directly added to the list of returned strings.</span></span>
* <span data-ttu-id="f7b90-139">La creazione di stringhe non comporta allocazioni, oltre alla memoria usata dalla stringa (ad eccezione della chiamata `ToArray()`).</span><span class="sxs-lookup"><span data-stu-id="f7b90-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="f7b90-140">Adattatori</span><span class="sxs-lookup"><span data-stu-id="f7b90-140">Adapters</span></span>

<span data-ttu-id="f7b90-141">Entrambe `Body` le `BodyReader/BodyWriter` proprietà e sono disponibili `HttpRequest` per `HttpResponse`e.</span><span class="sxs-lookup"><span data-stu-id="f7b90-141">Both `Body` and `BodyReader/BodyWriter` properties are available for `HttpRequest` and `HttpResponse`.</span></span> <span data-ttu-id="f7b90-142">Quando si imposta `Body` su un flusso diverso, un nuovo set di adapter adatta automaticamente ogni tipo all'altro.</span><span class="sxs-lookup"><span data-stu-id="f7b90-142">When you set `Body` to a different stream, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="f7b90-143">Se si imposta `HttpRequest.Body` su un nuovo flusso, `HttpRequest.BodyReader` viene impostato automaticamente su `HttpRequest.Body`un nuovo `PipeReader` oggetto che esegue il wrapping.</span><span class="sxs-lookup"><span data-stu-id="f7b90-143">If you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyReader` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span>

## <a name="startasync"></a><span data-ttu-id="f7b90-144">StartAsync</span><span class="sxs-lookup"><span data-stu-id="f7b90-144">StartAsync</span></span>

<span data-ttu-id="f7b90-145">`HttpResponse.StartAsync`viene utilizzato per indicare che le intestazioni non sono modificabili e `OnStarting` per eseguire i callback.</span><span class="sxs-lookup"><span data-stu-id="f7b90-145">`HttpResponse.StartAsync` is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="f7b90-146">Quando si usa gheppio come server, la `StartAsync` chiamata di prima `PipeReader` di usare garantisce che la `GetMemory` memoria restituita da appartenga <xref:System.IO.Pipelines.Pipe> a un buffer interno di Gheppio anziché a un buffer esterno.</span><span class="sxs-lookup"><span data-stu-id="f7b90-146">When using Kestrel as a server, calling `StartAsync` before using the `PipeReader` guarantees that memory returned by `GetMemory` belongs to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7b90-147">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f7b90-147">Additional resources</span></span>

* <span data-ttu-id="f7b90-148">[Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) (Introduzione a System.IO.Pipelines)</span><span class="sxs-lookup"><span data-stu-id="f7b90-148">[Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)</span></span>
* <xref:fundamentals/middleware/write>
