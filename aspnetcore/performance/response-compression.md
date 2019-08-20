---
title: Compressione della risposta in ASP.NET Core
author: guardrex
description: Informazioni sulla compressione delle risposte e su come usare il relativo middleware nelle app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/09/2019
uid: performance/response-compression
ms.openlocfilehash: e320e87179f9f1b9773a55c380684a3f3f712632
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993467"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="1a12c-103">Compressione della risposta in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a12c-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="1a12c-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1a12c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1a12c-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1a12c-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1a12c-106">La larghezza di banda di rete è una risorsa limitata.</span><span class="sxs-lookup"><span data-stu-id="1a12c-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="1a12c-107">La riduzione delle dimensioni della risposta in genere aumenta la velocità di risposta di un'app, spesso in modo significativo.</span><span class="sxs-lookup"><span data-stu-id="1a12c-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="1a12c-108">Un modo per ridurre le dimensioni del payload consiste nel comprimere le risposte di un'app.</span><span class="sxs-lookup"><span data-stu-id="1a12c-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="1a12c-109">Quando usare il middleware di compressione della risposta</span><span class="sxs-lookup"><span data-stu-id="1a12c-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="1a12c-110">Usare tecnologie di compressione delle risposte basate su server in IIS, Apache o nginx.</span><span class="sxs-lookup"><span data-stu-id="1a12c-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="1a12c-111">Le prestazioni del middleware probabilmente non corrispondono a quelle dei moduli server.</span><span class="sxs-lookup"><span data-stu-id="1a12c-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="1a12c-112">Il server [http. sys](xref:fundamentals/servers/httpsys) server e il server [gheppio](xref:fundamentals/servers/kestrel) attualmente non offrono il supporto predefinito per la compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="1a12c-113">Usare il middleware di compressione della risposta quando si è:</span><span class="sxs-lookup"><span data-stu-id="1a12c-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="1a12c-114">Impossibile utilizzare le seguenti tecnologie di compressione basate su server:</span><span class="sxs-lookup"><span data-stu-id="1a12c-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="1a12c-115">Modulo di compressione dinamica di IIS</span><span class="sxs-lookup"><span data-stu-id="1a12c-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="1a12c-116">Modulo Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="1a12c-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="1a12c-117">Compressione e decompressione nginx</span><span class="sxs-lookup"><span data-stu-id="1a12c-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="1a12c-118">Hosting diretto in:</span><span class="sxs-lookup"><span data-stu-id="1a12c-118">Hosting directly on:</span></span>
  * <span data-ttu-id="1a12c-119">[Server http. sys](xref:fundamentals/servers/httpsys) (in precedenza denominato webListener)</span><span class="sxs-lookup"><span data-stu-id="1a12c-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="1a12c-120">Server gheppio</span><span class="sxs-lookup"><span data-stu-id="1a12c-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="1a12c-121">Compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="1a12c-121">Response compression</span></span>

<span data-ttu-id="1a12c-122">In genere, qualsiasi risposta non compressa in modo nativo può trarre vantaggio dalla compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="1a12c-123">Le risposte non compresse in modo nativo includono in genere: CSS, JavaScript, HTML, XML e JSON.</span><span class="sxs-lookup"><span data-stu-id="1a12c-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="1a12c-124">Non è consigliabile comprimere asset compressi in modo nativo, ad esempio file PNG.</span><span class="sxs-lookup"><span data-stu-id="1a12c-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="1a12c-125">Se si prova a comprimere ulteriormente una risposta compressa in modo nativo, è probabile che qualsiasi riduzione aggiuntiva delle dimensioni e del tempo di trasmissione venga nascosta entro il tempo richiesto per elaborare la compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="1a12c-126">Non comprimere file di dimensioni inferiori a circa 150-1000 byte (a seconda del contenuto del file e dell'efficienza della compressione).</span><span class="sxs-lookup"><span data-stu-id="1a12c-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="1a12c-127">L'overhead di compressione di file piccoli può produrre un file compresso di dimensioni superiori a quelle del file non compresso.</span><span class="sxs-lookup"><span data-stu-id="1a12c-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="1a12c-128">Quando un client è in grado di elaborare il contenuto compresso, il client deve informare il server delle sue `Accept-Encoding` funzionalità inviando l'intestazione con la richiesta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="1a12c-129">Quando un server invia contenuto compresso, deve includere informazioni nell' `Content-Encoding` intestazione sulla modalità di codifica della risposta compressa.</span><span class="sxs-lookup"><span data-stu-id="1a12c-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="1a12c-130">Le designazioni di codifica del contenuto supportate dal middleware sono illustrate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="1a12c-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="1a12c-131">`Accept-Encoding`valori di intestazione</span><span class="sxs-lookup"><span data-stu-id="1a12c-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="1a12c-132">Middleware supportato</span><span class="sxs-lookup"><span data-stu-id="1a12c-132">Middleware Supported</span></span> | <span data-ttu-id="1a12c-133">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1a12c-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="1a12c-134">Sì (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="1a12c-134">Yes (default)</span></span>        | [<span data-ttu-id="1a12c-135">Formato dati compresso Brotli</span><span class="sxs-lookup"><span data-stu-id="1a12c-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="1a12c-136">No</span><span class="sxs-lookup"><span data-stu-id="1a12c-136">No</span></span>                   | [<span data-ttu-id="1a12c-137">Deflate formato dati compresso</span><span class="sxs-lookup"><span data-stu-id="1a12c-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="1a12c-138">No</span><span class="sxs-lookup"><span data-stu-id="1a12c-138">No</span></span>                   | [<span data-ttu-id="1a12c-139">Interscambio XML efficiente W3C</span><span class="sxs-lookup"><span data-stu-id="1a12c-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="1a12c-140">Yes</span><span class="sxs-lookup"><span data-stu-id="1a12c-140">Yes</span></span>                  | [<span data-ttu-id="1a12c-141">Formato di file gzip</span><span class="sxs-lookup"><span data-stu-id="1a12c-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="1a12c-142">Sì</span><span class="sxs-lookup"><span data-stu-id="1a12c-142">Yes</span></span>                  | <span data-ttu-id="1a12c-143">Identificatore "senza codifica": La risposta non deve essere codificata.</span><span class="sxs-lookup"><span data-stu-id="1a12c-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="1a12c-144">No</span><span class="sxs-lookup"><span data-stu-id="1a12c-144">No</span></span>                   | [<span data-ttu-id="1a12c-145">Formato di trasferimento di rete per gli archivi Java</span><span class="sxs-lookup"><span data-stu-id="1a12c-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="1a12c-146">Sì</span><span class="sxs-lookup"><span data-stu-id="1a12c-146">Yes</span></span>                  | <span data-ttu-id="1a12c-147">Qualsiasi codifica del contenuto disponibile non richiesta in modo esplicito</span><span class="sxs-lookup"><span data-stu-id="1a12c-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="1a12c-148">`Accept-Encoding`valori di intestazione</span><span class="sxs-lookup"><span data-stu-id="1a12c-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="1a12c-149">Middleware supportato</span><span class="sxs-lookup"><span data-stu-id="1a12c-149">Middleware Supported</span></span> | <span data-ttu-id="1a12c-150">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1a12c-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="1a12c-151">No</span><span class="sxs-lookup"><span data-stu-id="1a12c-151">No</span></span>                   | [<span data-ttu-id="1a12c-152">Formato dati compresso Brotli</span><span class="sxs-lookup"><span data-stu-id="1a12c-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="1a12c-153">No</span><span class="sxs-lookup"><span data-stu-id="1a12c-153">No</span></span>                   | [<span data-ttu-id="1a12c-154">Deflate formato dati compresso</span><span class="sxs-lookup"><span data-stu-id="1a12c-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="1a12c-155">No</span><span class="sxs-lookup"><span data-stu-id="1a12c-155">No</span></span>                   | [<span data-ttu-id="1a12c-156">Interscambio XML efficiente W3C</span><span class="sxs-lookup"><span data-stu-id="1a12c-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="1a12c-157">Sì (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="1a12c-157">Yes (default)</span></span>        | [<span data-ttu-id="1a12c-158">Formato di file gzip</span><span class="sxs-lookup"><span data-stu-id="1a12c-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="1a12c-159">Sì</span><span class="sxs-lookup"><span data-stu-id="1a12c-159">Yes</span></span>                  | <span data-ttu-id="1a12c-160">Identificatore "senza codifica": La risposta non deve essere codificata.</span><span class="sxs-lookup"><span data-stu-id="1a12c-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="1a12c-161">No</span><span class="sxs-lookup"><span data-stu-id="1a12c-161">No</span></span>                   | [<span data-ttu-id="1a12c-162">Formato di trasferimento di rete per gli archivi Java</span><span class="sxs-lookup"><span data-stu-id="1a12c-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="1a12c-163">Sì</span><span class="sxs-lookup"><span data-stu-id="1a12c-163">Yes</span></span>                  | <span data-ttu-id="1a12c-164">Qualsiasi codifica del contenuto disponibile non richiesta in modo esplicito</span><span class="sxs-lookup"><span data-stu-id="1a12c-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="1a12c-165">Per ulteriori informazioni, vedere l' [elenco di codici di contenuto ufficiale IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="1a12c-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="1a12c-166">Il middleware consente di aggiungere ulteriori provider di compressione per i `Accept-Encoding` valori dell'intestazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="1a12c-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="1a12c-167">Per ulteriori informazioni, vedere [provider personalizzati](#custom-providers) di seguito.</span><span class="sxs-lookup"><span data-stu-id="1a12c-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="1a12c-168">Il middleware è in grado di reagire alla ponderazione del valore `q`di qualità (qValue) quando viene inviato dal client per definire la priorità degli schemi di compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="1a12c-169">Per ulteriori informazioni, vedere [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="1a12c-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="1a12c-170">Gli algoritmi di compressione sono soggetti a un compromesso tra la velocità di compressione e l'efficacia della compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="1a12c-171">L' *efficacia* in questo contesto si riferisce alla dimensione dell'output dopo la compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="1a12c-172">La dimensione più piccola viene conseguita dalla compressione più *ottimale* .</span><span class="sxs-lookup"><span data-stu-id="1a12c-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="1a12c-173">Le intestazioni necessarie per la richiesta, l'invio, la memorizzazione nella cache e la ricezione di contenuti compressi sono descritte nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="1a12c-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="1a12c-174">Intestazione</span><span class="sxs-lookup"><span data-stu-id="1a12c-174">Header</span></span>             | <span data-ttu-id="1a12c-175">Role</span><span class="sxs-lookup"><span data-stu-id="1a12c-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="1a12c-176">Inviato dal client al server per indicare gli schemi di codifica del contenuto accettabili per il client.</span><span class="sxs-lookup"><span data-stu-id="1a12c-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="1a12c-177">Inviato dal server al client per indicare la codifica del contenuto nel payload.</span><span class="sxs-lookup"><span data-stu-id="1a12c-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="1a12c-178">Quando si verifica la compressione `Content-Length` , l'intestazione viene rimossa, perché il contenuto del corpo viene modificato quando la risposta viene compressa.</span><span class="sxs-lookup"><span data-stu-id="1a12c-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="1a12c-179">Quando si verifica la compressione `Content-MD5` , l'intestazione viene rimossa, perché il contenuto del corpo è stato modificato e l'hash non è più valido.</span><span class="sxs-lookup"><span data-stu-id="1a12c-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="1a12c-180">Specifica il tipo MIME del contenuto.</span><span class="sxs-lookup"><span data-stu-id="1a12c-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="1a12c-181">Ogni risposta deve specificarne `Content-Type`il.</span><span class="sxs-lookup"><span data-stu-id="1a12c-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="1a12c-182">Il middleware controlla questo valore per determinare se la risposta deve essere compressa.</span><span class="sxs-lookup"><span data-stu-id="1a12c-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="1a12c-183">Il middleware specifica un set di [tipi MIME predefiniti](#mime-types) che può codificare, ma è possibile sostituire o aggiungere tipi MIME.</span><span class="sxs-lookup"><span data-stu-id="1a12c-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="1a12c-184">Quando viene inviato dal server con un valore di `Accept-Encoding` ai client e ai proxy, l' `Vary` intestazione indica al client o al proxy che deve memorizzare nella cache (variare) le risposte in base al valore `Accept-Encoding` dell'intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="1a12c-185">Il risultato della restituzione di contenuto `Vary: Accept-Encoding` con l'intestazione è che le risposte compresse e non compresse vengono memorizzate nella cache separatamente.</span><span class="sxs-lookup"><span data-stu-id="1a12c-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="1a12c-186">Esplorare le funzionalità del middleware di compressione della risposta con l' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="1a12c-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="1a12c-187">Nell'esempio vengono illustrate le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1a12c-187">The sample illustrates:</span></span>

* <span data-ttu-id="1a12c-188">La compressione delle risposte delle app usando gzip e i provider di compressione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="1a12c-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="1a12c-189">Come aggiungere un tipo MIME all'elenco predefinito di tipi MIME per la compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="1a12c-190">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="1a12c-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1a12c-191">Il middleware di compressione della risposta è fornito dal pacchetto [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , incluso in modo implicito nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a12c-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1a12c-192">Per includere il middleware in un progetto, aggiungere un riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), che include il pacchetto [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .</span><span class="sxs-lookup"><span data-stu-id="1a12c-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="1a12c-193">Configurazione</span><span class="sxs-lookup"><span data-stu-id="1a12c-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1a12c-194">Il codice seguente illustra come abilitare il middleware di compressione della risposta per i tipi MIME e i provider di compressione predefiniti ([Brotli](#brotli-compression-provider) e [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="1a12c-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1a12c-195">Il codice seguente illustra come abilitare il middleware di compressione della risposta per i tipi MIME predefiniti e il [provider di compressione gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="1a12c-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

<span data-ttu-id="1a12c-196">Note:</span><span class="sxs-lookup"><span data-stu-id="1a12c-196">Notes:</span></span>

* <span data-ttu-id="1a12c-197">`app.UseResponseCompression`deve essere chiamato prima `app.UseMvc`di.</span><span class="sxs-lookup"><span data-stu-id="1a12c-197">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="1a12c-198">Per impostare l'intestazione `Accept-Encoding` della richiesta e studiare le intestazioni, le dimensioni e il corpo della risposta, usare uno strumento, ad esempio [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/) o [Postman](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="1a12c-198">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="1a12c-199">Inviare una richiesta all'app di esempio senza l' `Accept-Encoding` intestazione e osservare che la risposta non è compressa.</span><span class="sxs-lookup"><span data-stu-id="1a12c-199">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="1a12c-200">Le `Content-Encoding` intestazioni `Vary` e non sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-200">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Finestra Fiddler che mostra il risultato di una richiesta senza l'intestazione Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1a12c-203">Inviare una richiesta all'app di esempio con l' `Accept-Encoding: br` intestazione (compressione Brotli) e osservare che la risposta è compressa.</span><span class="sxs-lookup"><span data-stu-id="1a12c-203">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="1a12c-204">Le `Content-Encoding` intestazioni `Vary` e sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-204">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Finestra Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1a12c-208">Inviare una richiesta all'app di esempio con l' `Accept-Encoding: gzip` intestazione e osservare che la risposta è compressa.</span><span class="sxs-lookup"><span data-stu-id="1a12c-208">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="1a12c-209">Le `Content-Encoding` intestazioni `Vary` e sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-209">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Finestra Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="1a12c-213">Provider</span><span class="sxs-lookup"><span data-stu-id="1a12c-213">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="1a12c-214">Provider di compressione Brotli</span><span class="sxs-lookup"><span data-stu-id="1a12c-214">Brotli Compression Provider</span></span>

<span data-ttu-id="1a12c-215">Usare per comprimere le risposte con il [formato dati compresso Brotli.](https://tools.ietf.org/html/rfc7932) <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider></span><span class="sxs-lookup"><span data-stu-id="1a12c-215">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="1a12c-216">Se nessun provider di compressione viene aggiunto in modo esplicito a <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="1a12c-216">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="1a12c-217">Il provider di compressione Brotli viene aggiunto per impostazione predefinita alla matrice di provider di compressione insieme al [provider di compressione gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="1a12c-217">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="1a12c-218">Per impostazione predefinita, la compressione Brotli la compressione quando il formato dati compresso Brotli è supportato dal client.</span><span class="sxs-lookup"><span data-stu-id="1a12c-218">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="1a12c-219">Se Brotli non è supportato dal client, la compressione viene assegnata per impostazione predefinita a gzip quando il client supporta la compressione gzip.</span><span class="sxs-lookup"><span data-stu-id="1a12c-219">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="1a12c-220">È necessario aggiungere il provider di compressione Brotoli quando vengono aggiunti in modo esplicito i provider di compressione:</span><span class="sxs-lookup"><span data-stu-id="1a12c-220">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1a12c-221">Impostare il livello di compressione <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>con.</span><span class="sxs-lookup"><span data-stu-id="1a12c-221">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="1a12c-222">Per impostazione predefinita, il provider di compressione Brotli è il livello di compressione più veloce ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente.</span><span class="sxs-lookup"><span data-stu-id="1a12c-222">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="1a12c-223">Se si desidera la compressione più efficiente, configurare il middleware per la compressione ottimale.</span><span class="sxs-lookup"><span data-stu-id="1a12c-223">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="1a12c-224">Livello di compressione</span><span class="sxs-lookup"><span data-stu-id="1a12c-224">Compression Level</span></span> | <span data-ttu-id="1a12c-225">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="1a12c-225">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="1a12c-226">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="1a12c-226">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1a12c-227">La compressione deve essere completata il più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale.</span><span class="sxs-lookup"><span data-stu-id="1a12c-227">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="1a12c-228">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="1a12c-228">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1a12c-229">Non deve essere eseguita alcuna compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-229">No compression should be performed.</span></span> |
| [<span data-ttu-id="1a12c-230">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="1a12c-230">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1a12c-231">Le risposte devono essere compresse in modo ottimale, anche se la compressione impiega più tempo per il completamento.</span><span class="sxs-lookup"><span data-stu-id="1a12c-231">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a><span data-ttu-id="1a12c-232">Provider di compressione gzip</span><span class="sxs-lookup"><span data-stu-id="1a12c-232">Gzip Compression Provider</span></span>

<span data-ttu-id="1a12c-233">Usare per comprimere le risposte con il [formato di file gzip.](https://tools.ietf.org/html/rfc1952) <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider></span><span class="sxs-lookup"><span data-stu-id="1a12c-233">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="1a12c-234">Se nessun provider di compressione viene aggiunto in modo esplicito a <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="1a12c-234">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="1a12c-235">Il provider di compressione Gzip viene aggiunto per impostazione predefinita alla matrice di provider di compressione insieme al [provider di compressione Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="1a12c-235">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="1a12c-236">Per impostazione predefinita, la compressione Brotli la compressione quando il formato dati compresso Brotli è supportato dal client.</span><span class="sxs-lookup"><span data-stu-id="1a12c-236">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="1a12c-237">Se Brotli non è supportato dal client, la compressione viene assegnata per impostazione predefinita a gzip quando il client supporta la compressione gzip.</span><span class="sxs-lookup"><span data-stu-id="1a12c-237">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="1a12c-238">Per impostazione predefinita, il provider di compressione Gzip viene aggiunto alla matrice di provider di compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-238">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="1a12c-239">Per impostazione predefinita, la compressione viene impostata su gzip quando il client supporta la compressione gzip.</span><span class="sxs-lookup"><span data-stu-id="1a12c-239">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="1a12c-240">È necessario aggiungere il provider di compressione gzip quando vengono aggiunti in modo esplicito i provider di compressione:</span><span class="sxs-lookup"><span data-stu-id="1a12c-240">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="1a12c-241">Impostare il livello di compressione <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>con.</span><span class="sxs-lookup"><span data-stu-id="1a12c-241">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="1a12c-242">Per impostazione predefinita, il provider di compressione gzip è il livello di compressione più veloce ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente.</span><span class="sxs-lookup"><span data-stu-id="1a12c-242">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="1a12c-243">Se si desidera la compressione più efficiente, configurare il middleware per la compressione ottimale.</span><span class="sxs-lookup"><span data-stu-id="1a12c-243">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="1a12c-244">Livello di compressione</span><span class="sxs-lookup"><span data-stu-id="1a12c-244">Compression Level</span></span> | <span data-ttu-id="1a12c-245">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1a12c-245">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="1a12c-246">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="1a12c-246">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1a12c-247">La compressione deve essere completata il più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale.</span><span class="sxs-lookup"><span data-stu-id="1a12c-247">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="1a12c-248">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="1a12c-248">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1a12c-249">Non deve essere eseguita alcuna compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-249">No compression should be performed.</span></span> |
| [<span data-ttu-id="1a12c-250">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="1a12c-250">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1a12c-251">Le risposte devono essere compresse in modo ottimale, anche se la compressione impiega più tempo per il completamento.</span><span class="sxs-lookup"><span data-stu-id="1a12c-251">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a><span data-ttu-id="1a12c-252">Provider personalizzati</span><span class="sxs-lookup"><span data-stu-id="1a12c-252">Custom providers</span></span>

<span data-ttu-id="1a12c-253">Creare implementazioni di compressione personalizzate <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>con.</span><span class="sxs-lookup"><span data-stu-id="1a12c-253">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="1a12c-254">Rappresenta <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> la codifica del contenuto prodotta da `ICompressionProvider` questo oggetto.</span><span class="sxs-lookup"><span data-stu-id="1a12c-254">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="1a12c-255">Il middleware usa queste informazioni per scegliere il provider in base all'elenco specificato nell' `Accept-Encoding` intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-255">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="1a12c-256">Usando l'app di esempio, il client invia una richiesta con l' `Accept-Encoding: mycustomcompression` intestazione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-256">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="1a12c-257">Il middleware usa l'implementazione della compressione personalizzata e restituisce la risposta con `Content-Encoding: mycustomcompression` un'intestazione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-257">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="1a12c-258">Il client deve essere in grado di decomprimere la codifica personalizzata per consentire il funzionamento di un'implementazione della compressione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="1a12c-258">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1a12c-259">Inviare una richiesta all'app di esempio con l' `Accept-Encoding: mycustomcompression` intestazione e osservare le intestazioni della risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-259">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="1a12c-260">Le `Vary` intestazioni `Content-Encoding` e sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-260">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="1a12c-261">Il corpo della risposta (non visualizzato) non è compresso dall'esempio.</span><span class="sxs-lookup"><span data-stu-id="1a12c-261">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="1a12c-262">Non esiste un'implementazione della compressione nella `CustomCompressionProvider` classe dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="1a12c-262">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="1a12c-263">Tuttavia, l'esempio mostra dove implementare tale algoritmo di compressione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-263">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Finestra Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="1a12c-266">MIME (tipi)</span><span class="sxs-lookup"><span data-stu-id="1a12c-266">MIME types</span></span>

<span data-ttu-id="1a12c-267">Il middleware specifica un set predefinito di tipi MIME per la compressione:</span><span class="sxs-lookup"><span data-stu-id="1a12c-267">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="1a12c-268">Sostituire o aggiungere tipi MIME con le opzioni del middleware di compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-268">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="1a12c-269">Si noti che i tipi MIME con `text/*` caratteri jolly, ad esempio, non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="1a12c-269">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="1a12c-270">L'app di esempio aggiunge un tipo MIME `image/svg+xml` per e comprime e serve l'immagine del banner ASP.NET Core (*banner. svg*).</span><span class="sxs-lookup"><span data-stu-id="1a12c-270">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="1a12c-271">Compressione con protocollo sicuro</span><span class="sxs-lookup"><span data-stu-id="1a12c-271">Compression with secure protocol</span></span>

<span data-ttu-id="1a12c-272">È possibile controllare le risposte compresse su connessioni sicure `EnableForHttps` con l'opzione, che è disabilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1a12c-272">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="1a12c-273">L'uso della compressione con pagine generate dinamicamente può causare problemi di sicurezza, ad esempio gli attacchi di [reato](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violazione](https://wikipedia.org/wiki/BREACH_(security_exploit)) .</span><span class="sxs-lookup"><span data-stu-id="1a12c-273">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="1a12c-274">Aggiunta dell'intestazione Vary</span><span class="sxs-lookup"><span data-stu-id="1a12c-274">Adding the Vary header</span></span>

<span data-ttu-id="1a12c-275">Quando si comprimono le risposte in `Accept-Encoding` base all'intestazione, sono presenti potenzialmente più versioni compresse della risposta e una versione non compressa.</span><span class="sxs-lookup"><span data-stu-id="1a12c-275">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="1a12c-276">Per indicare alle cache del client e del proxy che esistono più versioni e che devono essere archiviate `Vary` , l'intestazione viene aggiunta `Accept-Encoding` con un valore.</span><span class="sxs-lookup"><span data-stu-id="1a12c-276">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="1a12c-277">In ASP.NET Core 2,0 o versioni successive, il middleware aggiunge `Vary` automaticamente l'intestazione quando la risposta viene compressa.</span><span class="sxs-lookup"><span data-stu-id="1a12c-277">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="1a12c-278">Problema del middleware quando si trova dietro un proxy inverso nginx</span><span class="sxs-lookup"><span data-stu-id="1a12c-278">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="1a12c-279">Quando una richiesta viene sottoposta a proxy da `Accept-Encoding` nginx, l'intestazione viene rimossa.</span><span class="sxs-lookup"><span data-stu-id="1a12c-279">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="1a12c-280">La rimozione dell' `Accept-Encoding` intestazione impedisce al middleware di comprimere la risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-280">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="1a12c-281">Per altre informazioni, vedere [NGINX: Compressione e decompressione](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="1a12c-281">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="1a12c-282">Questo problema viene rilevato in base [alla figura della compressione pass-through per Nginx (ASPNET/ \#BasicMiddleware 123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="1a12c-282">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="1a12c-283">Utilizzo della compressione dinamica di IIS</span><span class="sxs-lookup"><span data-stu-id="1a12c-283">Working with IIS dynamic compression</span></span>

<span data-ttu-id="1a12c-284">Se si dispone di un modulo di compressione dinamica IIS attivo configurato a livello di server che si desidera disabilitare per un'app, disabilitare il modulo con un'aggiunta al file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="1a12c-284">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="1a12c-285">Per altre informazioni, vedere [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Disabilitazione di moduli IIS).</span><span class="sxs-lookup"><span data-stu-id="1a12c-285">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1a12c-286">risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="1a12c-286">Troubleshooting</span></span>

<span data-ttu-id="1a12c-287">Usare uno strumento come [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)o [Postman](https://www.getpostman.com/), che consente di impostare l'intestazione `Accept-Encoding` della richiesta e studiare le intestazioni, le dimensioni e il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-287">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="1a12c-288">Per impostazione predefinita, il middleware della compressione della risposta comprime le risposte che soddisfano le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1a12c-288">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="1a12c-289">L' `Accept-Encoding` intestazione è presente con un valore, `br` `gzip` `*`, o una codifica personalizzata che corrisponde a un provider di compressione personalizzato stabilito.</span><span class="sxs-lookup"><span data-stu-id="1a12c-289">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="1a12c-290">Il valore non deve essere `identity` o avere un valore di qualità ( `q`qValue) impostato su 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="1a12c-290">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="1a12c-291">Il tipo MIME (`Content-Type`) deve essere impostato e deve corrispondere a un tipo MIME configurato <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>in.</span><span class="sxs-lookup"><span data-stu-id="1a12c-291">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="1a12c-292">La richiesta non deve includere l' `Content-Range` intestazione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-292">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="1a12c-293">La richiesta deve usare il protocollo non sicuro (http), a meno che non sia configurato il protocollo Secure Protocol (HTTPS) nelle opzioni del middleware della compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-293">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="1a12c-294">*Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si Abilita la compressione del contenuto protetta.*</span><span class="sxs-lookup"><span data-stu-id="1a12c-294">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="1a12c-295">L' `Accept-Encoding` intestazione è presente con un valore di `gzip`, `*`o codifica personalizzata che corrisponde a un provider di compressione personalizzato stabilito.</span><span class="sxs-lookup"><span data-stu-id="1a12c-295">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="1a12c-296">Il valore non deve essere `identity` o avere un valore di qualità ( `q`qValue) impostato su 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="1a12c-296">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="1a12c-297">Il tipo MIME (`Content-Type`) deve essere impostato e deve corrispondere a un tipo MIME configurato <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>in.</span><span class="sxs-lookup"><span data-stu-id="1a12c-297">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="1a12c-298">La richiesta non deve includere l' `Content-Range` intestazione.</span><span class="sxs-lookup"><span data-stu-id="1a12c-298">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="1a12c-299">La richiesta deve usare il protocollo non sicuro (http), a meno che non sia configurato il protocollo Secure Protocol (HTTPS) nelle opzioni del middleware della compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="1a12c-299">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="1a12c-300">*Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si Abilita la compressione del contenuto protetta.*</span><span class="sxs-lookup"><span data-stu-id="1a12c-300">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="1a12c-301">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1a12c-301">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="1a12c-302">Rete per sviluppatori Mozilla: Accept-Encoding</span><span class="sxs-lookup"><span data-stu-id="1a12c-302">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="1a12c-303">RFC 7231 sezione 3.1.2.1: Codifica del contenuto</span><span class="sxs-lookup"><span data-stu-id="1a12c-303">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="1a12c-304">RFC 7230 sezione 4.2.3: Codifica gzip</span><span class="sxs-lookup"><span data-stu-id="1a12c-304">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="1a12c-305">Specifica del formato di file GZIP versione 4,3</span><span class="sxs-lookup"><span data-stu-id="1a12c-305">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
