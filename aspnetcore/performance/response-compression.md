---
title: Compressione della risposta in ASP.NET Core
author: guardrex
description: Informazioni sulla compressione delle risposte e su come usare il relativo middleware nelle app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/response-compression
ms.openlocfilehash: b8a84418a3258e9ac43b4eadd8564c0708590bce
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726970"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="acc72-103">Compressione della risposta in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="acc72-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="acc72-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="acc72-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="acc72-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="acc72-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="acc72-106">La larghezza di banda di rete è una risorsa limitata.</span><span class="sxs-lookup"><span data-stu-id="acc72-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="acc72-107">La riduzione delle dimensioni della risposta in genere aumenta la velocità di risposta di un'app, spesso in modo significativo.</span><span class="sxs-lookup"><span data-stu-id="acc72-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="acc72-108">Un modo per ridurre le dimensioni del payload consiste nel comprimere le risposte di un'app.</span><span class="sxs-lookup"><span data-stu-id="acc72-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="acc72-109">Quando usare il middleware di compressione della risposta</span><span class="sxs-lookup"><span data-stu-id="acc72-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="acc72-110">Usare tecnologie di compressione delle risposte basate su server in IIS, Apache o nginx.</span><span class="sxs-lookup"><span data-stu-id="acc72-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="acc72-111">Le prestazioni del middleware probabilmente non corrispondono a quelle dei moduli server.</span><span class="sxs-lookup"><span data-stu-id="acc72-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="acc72-112">Il server [http. sys](xref:fundamentals/servers/httpsys) server e il server [gheppio](xref:fundamentals/servers/kestrel) attualmente non offrono il supporto predefinito per la compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="acc72-113">Usare il middleware di compressione della risposta quando si è:</span><span class="sxs-lookup"><span data-stu-id="acc72-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="acc72-114">Impossibile utilizzare le seguenti tecnologie di compressione basate su server:</span><span class="sxs-lookup"><span data-stu-id="acc72-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="acc72-115">Modulo di compressione dinamica di IIS</span><span class="sxs-lookup"><span data-stu-id="acc72-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="acc72-116">Modulo Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="acc72-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="acc72-117">Compressione e decompressione nginx</span><span class="sxs-lookup"><span data-stu-id="acc72-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="acc72-118">Hosting diretto in:</span><span class="sxs-lookup"><span data-stu-id="acc72-118">Hosting directly on:</span></span>
  * <span data-ttu-id="acc72-119">[Server http. sys](xref:fundamentals/servers/httpsys) (chiamato in precedenza webListener)</span><span class="sxs-lookup"><span data-stu-id="acc72-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="acc72-120">Server gheppio</span><span class="sxs-lookup"><span data-stu-id="acc72-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="acc72-121">Compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="acc72-121">Response compression</span></span>

<span data-ttu-id="acc72-122">In genere, qualsiasi risposta non compressa in modo nativo può trarre vantaggio dalla compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="acc72-123">Le risposte non compresse in modo nativo includono in genere: CSS, JavaScript, HTML, XML e JSON.</span><span class="sxs-lookup"><span data-stu-id="acc72-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="acc72-124">Non è consigliabile comprimere asset compressi in modo nativo, ad esempio file PNG.</span><span class="sxs-lookup"><span data-stu-id="acc72-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="acc72-125">Se si prova a comprimere ulteriormente una risposta compressa in modo nativo, è probabile che qualsiasi riduzione aggiuntiva delle dimensioni e del tempo di trasmissione venga nascosta entro il tempo richiesto per elaborare la compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="acc72-126">Non comprimere file di dimensioni inferiori a circa 150-1000 byte (a seconda del contenuto del file e dell'efficienza della compressione).</span><span class="sxs-lookup"><span data-stu-id="acc72-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="acc72-127">L'overhead di compressione di file piccoli può produrre un file compresso di dimensioni superiori a quelle del file non compresso.</span><span class="sxs-lookup"><span data-stu-id="acc72-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="acc72-128">Quando un client è in grado di elaborare contenuto compresso, il client deve informare il server delle sue funzionalità inviando l'intestazione `Accept-Encoding` con la richiesta.</span><span class="sxs-lookup"><span data-stu-id="acc72-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="acc72-129">Quando un server invia contenuto compresso, deve includere informazioni nell'intestazione `Content-Encoding` sulla modalità di codifica della risposta compressa.</span><span class="sxs-lookup"><span data-stu-id="acc72-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="acc72-130">Le designazioni di codifica del contenuto supportate dal middleware sono illustrate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="acc72-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="acc72-131">valori di intestazione `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="acc72-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="acc72-132">Middleware supportato</span><span class="sxs-lookup"><span data-stu-id="acc72-132">Middleware Supported</span></span> | <span data-ttu-id="acc72-133">Descrizione</span><span class="sxs-lookup"><span data-stu-id="acc72-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="acc72-134">Sì (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="acc72-134">Yes (default)</span></span>        | [<span data-ttu-id="acc72-135">Formato dati compresso Brotli</span><span class="sxs-lookup"><span data-stu-id="acc72-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="acc72-136">No</span><span class="sxs-lookup"><span data-stu-id="acc72-136">No</span></span>                   | [<span data-ttu-id="acc72-137">Deflate formato dati compresso</span><span class="sxs-lookup"><span data-stu-id="acc72-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="acc72-138">No</span><span class="sxs-lookup"><span data-stu-id="acc72-138">No</span></span>                   | [<span data-ttu-id="acc72-139">Interscambio XML efficiente W3C</span><span class="sxs-lookup"><span data-stu-id="acc72-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="acc72-140">Sì</span><span class="sxs-lookup"><span data-stu-id="acc72-140">Yes</span></span>                  | [<span data-ttu-id="acc72-141">Formato di file gzip</span><span class="sxs-lookup"><span data-stu-id="acc72-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="acc72-142">Sì</span><span class="sxs-lookup"><span data-stu-id="acc72-142">Yes</span></span>                  | <span data-ttu-id="acc72-143">Identificatore "senza codifica": la risposta non deve essere codificata.</span><span class="sxs-lookup"><span data-stu-id="acc72-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="acc72-144">No</span><span class="sxs-lookup"><span data-stu-id="acc72-144">No</span></span>                   | [<span data-ttu-id="acc72-145">Formato di trasferimento di rete per gli archivi Java</span><span class="sxs-lookup"><span data-stu-id="acc72-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="acc72-146">Sì</span><span class="sxs-lookup"><span data-stu-id="acc72-146">Yes</span></span>                  | <span data-ttu-id="acc72-147">Qualsiasi codifica del contenuto disponibile non richiesta in modo esplicito</span><span class="sxs-lookup"><span data-stu-id="acc72-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="acc72-148">valori di intestazione `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="acc72-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="acc72-149">Middleware supportato</span><span class="sxs-lookup"><span data-stu-id="acc72-149">Middleware Supported</span></span> | <span data-ttu-id="acc72-150">Descrizione</span><span class="sxs-lookup"><span data-stu-id="acc72-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="acc72-151">No</span><span class="sxs-lookup"><span data-stu-id="acc72-151">No</span></span>                   | [<span data-ttu-id="acc72-152">Formato dati compresso Brotli</span><span class="sxs-lookup"><span data-stu-id="acc72-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="acc72-153">No</span><span class="sxs-lookup"><span data-stu-id="acc72-153">No</span></span>                   | [<span data-ttu-id="acc72-154">Deflate formato dati compresso</span><span class="sxs-lookup"><span data-stu-id="acc72-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="acc72-155">No</span><span class="sxs-lookup"><span data-stu-id="acc72-155">No</span></span>                   | [<span data-ttu-id="acc72-156">Interscambio XML efficiente W3C</span><span class="sxs-lookup"><span data-stu-id="acc72-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="acc72-157">Sì (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="acc72-157">Yes (default)</span></span>        | [<span data-ttu-id="acc72-158">Formato di file gzip</span><span class="sxs-lookup"><span data-stu-id="acc72-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="acc72-159">Sì</span><span class="sxs-lookup"><span data-stu-id="acc72-159">Yes</span></span>                  | <span data-ttu-id="acc72-160">Identificatore "senza codifica": la risposta non deve essere codificata.</span><span class="sxs-lookup"><span data-stu-id="acc72-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="acc72-161">No</span><span class="sxs-lookup"><span data-stu-id="acc72-161">No</span></span>                   | [<span data-ttu-id="acc72-162">Formato di trasferimento di rete per gli archivi Java</span><span class="sxs-lookup"><span data-stu-id="acc72-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="acc72-163">Sì</span><span class="sxs-lookup"><span data-stu-id="acc72-163">Yes</span></span>                  | <span data-ttu-id="acc72-164">Qualsiasi codifica del contenuto disponibile non richiesta in modo esplicito</span><span class="sxs-lookup"><span data-stu-id="acc72-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="acc72-165">Per ulteriori informazioni, vedere l' [elenco di codici di contenuto ufficiale IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="acc72-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="acc72-166">Il middleware consente di aggiungere ulteriori provider di compressione per i valori di intestazione `Accept-Encoding` personalizzati.</span><span class="sxs-lookup"><span data-stu-id="acc72-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="acc72-167">Per ulteriori informazioni, vedere [provider personalizzati](#custom-providers) di seguito.</span><span class="sxs-lookup"><span data-stu-id="acc72-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="acc72-168">Il middleware è in grado di reagire alla ponderazione del valore di qualità (qValue, `q`) quando viene inviato dal client per definire la priorità degli schemi di compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="acc72-169">Per altre informazioni, vedere [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="acc72-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="acc72-170">Gli algoritmi di compressione sono soggetti a un compromesso tra la velocità di compressione e l'efficacia della compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="acc72-171">L' *efficacia* in questo contesto si riferisce alla dimensione dell'output dopo la compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="acc72-172">La dimensione più piccola viene conseguita dalla compressione più *ottimale* .</span><span class="sxs-lookup"><span data-stu-id="acc72-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="acc72-173">Le intestazioni necessarie per la richiesta, l'invio, la memorizzazione nella cache e la ricezione di contenuti compressi sono descritte nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="acc72-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="acc72-174">Intestazione</span><span class="sxs-lookup"><span data-stu-id="acc72-174">Header</span></span>             | <span data-ttu-id="acc72-175">Role</span><span class="sxs-lookup"><span data-stu-id="acc72-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="acc72-176">Inviato dal client al server per indicare gli schemi di codifica del contenuto accettabili per il client.</span><span class="sxs-lookup"><span data-stu-id="acc72-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="acc72-177">Inviato dal server al client per indicare la codifica del contenuto nel payload.</span><span class="sxs-lookup"><span data-stu-id="acc72-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="acc72-178">Quando si verifica la compressione, l'intestazione `Content-Length` viene rimossa, perché il contenuto del corpo viene modificato quando la risposta viene compressa.</span><span class="sxs-lookup"><span data-stu-id="acc72-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="acc72-179">Quando si verifica la compressione, l'intestazione `Content-MD5` viene rimossa, perché il contenuto del corpo è stato modificato e l'hash non è più valido.</span><span class="sxs-lookup"><span data-stu-id="acc72-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="acc72-180">Specifica il tipo MIME del contenuto.</span><span class="sxs-lookup"><span data-stu-id="acc72-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="acc72-181">Ogni risposta deve specificare il `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="acc72-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="acc72-182">Il middleware controlla questo valore per determinare se la risposta deve essere compressa.</span><span class="sxs-lookup"><span data-stu-id="acc72-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="acc72-183">Il middleware specifica un set di [tipi MIME predefiniti](#mime-types) che può codificare, ma è possibile sostituire o aggiungere tipi MIME.</span><span class="sxs-lookup"><span data-stu-id="acc72-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="acc72-184">Quando viene inviato dal server con un valore di `Accept-Encoding` ai client e ai proxy, l'intestazione `Vary` indica al client o al proxy che deve memorizzare nella cache (variare) le risposte in base al valore dell'intestazione `Accept-Encoding` della richiesta.</span><span class="sxs-lookup"><span data-stu-id="acc72-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="acc72-185">Il risultato della restituzione di contenuto con l'intestazione `Vary: Accept-Encoding` è che le risposte compresse e non compresse vengono memorizzate nella cache separatamente.</span><span class="sxs-lookup"><span data-stu-id="acc72-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="acc72-186">Esplorare le funzionalità del middleware di compressione della risposta con l' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="acc72-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="acc72-187">Nell'esempio vengono illustrate le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="acc72-187">The sample illustrates:</span></span>

* <span data-ttu-id="acc72-188">La compressione delle risposte delle app usando gzip e i provider di compressione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="acc72-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="acc72-189">Come aggiungere un tipo MIME all'elenco predefinito di tipi MIME per la compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="acc72-190">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="acc72-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="acc72-191">Il middleware di compressione della risposta è fornito dal pacchetto [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , incluso in modo implicito nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="acc72-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="acc72-192">Per includere il middleware in un progetto, aggiungere un riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), che include il pacchetto [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .</span><span class="sxs-lookup"><span data-stu-id="acc72-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="acc72-193">Configurazione</span><span class="sxs-lookup"><span data-stu-id="acc72-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="acc72-194">Il codice seguente illustra come abilitare il middleware di compressione della risposta per i tipi MIME e i provider di compressione predefiniti ([Brotli](#brotli-compression-provider) e [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="acc72-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="acc72-195">Il codice seguente illustra come abilitare il middleware di compressione della risposta per i tipi MIME predefiniti e il [provider di compressione gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="acc72-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="acc72-196">Note:</span><span class="sxs-lookup"><span data-stu-id="acc72-196">Notes:</span></span>

* <span data-ttu-id="acc72-197">`app.UseResponseCompression` deve essere chiamato prima di qualsiasi middleware che comprime le risposte.</span><span class="sxs-lookup"><span data-stu-id="acc72-197">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="acc72-198">Per altre informazioni, vedere <xref:fundamentals/middleware/index#middleware-order>.</span><span class="sxs-lookup"><span data-stu-id="acc72-198">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="acc72-199">Per impostare l'intestazione della richiesta `Accept-Encoding` e studiare le intestazioni, le dimensioni e il corpo della risposta, usare uno strumento come [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)o [postazione](https://www.getpostman.com/) .</span><span class="sxs-lookup"><span data-stu-id="acc72-199">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="acc72-200">Inviare una richiesta all'app di esempio senza l'intestazione `Accept-Encoding` e osservare che la risposta non è compressa.</span><span class="sxs-lookup"><span data-stu-id="acc72-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="acc72-201">Le intestazioni `Content-Encoding` e `Vary` non sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Finestra Fiddler che mostra il risultato di una richiesta senza l'intestazione Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="acc72-204">Inviare una richiesta all'app di esempio con l'intestazione `Accept-Encoding: br` (compressione Brotli) e osservare che la risposta è compressa.</span><span class="sxs-lookup"><span data-stu-id="acc72-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="acc72-205">Le intestazioni `Content-Encoding` e `Vary` sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Finestra Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="acc72-209">Inviare una richiesta all'app di esempio con l'intestazione `Accept-Encoding: gzip` e osservare che la risposta è compressa.</span><span class="sxs-lookup"><span data-stu-id="acc72-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="acc72-210">Le intestazioni `Content-Encoding` e `Vary` sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Finestra Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="acc72-214">Provider</span><span class="sxs-lookup"><span data-stu-id="acc72-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="acc72-215">Provider di compressione Brotli</span><span class="sxs-lookup"><span data-stu-id="acc72-215">Brotli Compression Provider</span></span>

<span data-ttu-id="acc72-216">Usare il <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> per comprimere le risposte con il [formato dati compresso Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="acc72-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="acc72-217">Se al <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>non vengono aggiunti provider di compressione in modo esplicito:</span><span class="sxs-lookup"><span data-stu-id="acc72-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="acc72-218">Il provider di compressione Brotli viene aggiunto per impostazione predefinita alla matrice di provider di compressione insieme al [provider di compressione gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="acc72-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="acc72-219">Per impostazione predefinita, la compressione Brotli la compressione quando il formato dati compresso Brotli è supportato dal client.</span><span class="sxs-lookup"><span data-stu-id="acc72-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="acc72-220">Se Brotli non è supportato dal client, la compressione viene assegnata per impostazione predefinita a gzip quando il client supporta la compressione gzip.</span><span class="sxs-lookup"><span data-stu-id="acc72-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="acc72-221">È necessario aggiungere il provider di compressione Brotoli quando vengono aggiunti in modo esplicito i provider di compressione:</span><span class="sxs-lookup"><span data-stu-id="acc72-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="acc72-222">Impostare il livello di compressione con <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="acc72-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="acc72-223">Per impostazione predefinita, il provider di compressione Brotli è il livello di compressione più veloce ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente.</span><span class="sxs-lookup"><span data-stu-id="acc72-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="acc72-224">Se si desidera la compressione più efficiente, configurare il middleware per la compressione ottimale.</span><span class="sxs-lookup"><span data-stu-id="acc72-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="acc72-225">Compression Level</span><span class="sxs-lookup"><span data-stu-id="acc72-225">Compression Level</span></span> | <span data-ttu-id="acc72-226">Descrizione</span><span class="sxs-lookup"><span data-stu-id="acc72-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="acc72-227">CompressionLevel. più veloce</span><span class="sxs-lookup"><span data-stu-id="acc72-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="acc72-228">La compressione deve essere completata il più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale.</span><span class="sxs-lookup"><span data-stu-id="acc72-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="acc72-229">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="acc72-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="acc72-230">Non deve essere eseguita alcuna compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="acc72-231">CompressionLevel. Optimal</span><span class="sxs-lookup"><span data-stu-id="acc72-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="acc72-232">Le risposte devono essere compresse in modo ottimale, anche se la compressione impiega più tempo per il completamento.</span><span class="sxs-lookup"><span data-stu-id="acc72-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="acc72-233">Provider di compressione gzip</span><span class="sxs-lookup"><span data-stu-id="acc72-233">Gzip Compression Provider</span></span>

<span data-ttu-id="acc72-234">Usare il <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> per comprimere le risposte con il [formato di file gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="acc72-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="acc72-235">Se al <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>non vengono aggiunti provider di compressione in modo esplicito:</span><span class="sxs-lookup"><span data-stu-id="acc72-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="acc72-236">Il provider di compressione Gzip viene aggiunto per impostazione predefinita alla matrice di provider di compressione insieme al [provider di compressione Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="acc72-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="acc72-237">Per impostazione predefinita, la compressione Brotli la compressione quando il formato dati compresso Brotli è supportato dal client.</span><span class="sxs-lookup"><span data-stu-id="acc72-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="acc72-238">Se Brotli non è supportato dal client, la compressione viene assegnata per impostazione predefinita a gzip quando il client supporta la compressione gzip.</span><span class="sxs-lookup"><span data-stu-id="acc72-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="acc72-239">Per impostazione predefinita, il provider di compressione Gzip viene aggiunto alla matrice di provider di compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="acc72-240">Per impostazione predefinita, la compressione viene impostata su gzip quando il client supporta la compressione gzip.</span><span class="sxs-lookup"><span data-stu-id="acc72-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="acc72-241">È necessario aggiungere il provider di compressione gzip quando vengono aggiunti in modo esplicito i provider di compressione:</span><span class="sxs-lookup"><span data-stu-id="acc72-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="acc72-242">Impostare il livello di compressione con <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="acc72-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="acc72-243">Per impostazione predefinita, il provider di compressione gzip è il livello di compressione più veloce ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente.</span><span class="sxs-lookup"><span data-stu-id="acc72-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="acc72-244">Se si desidera la compressione più efficiente, configurare il middleware per la compressione ottimale.</span><span class="sxs-lookup"><span data-stu-id="acc72-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="acc72-245">Compression Level</span><span class="sxs-lookup"><span data-stu-id="acc72-245">Compression Level</span></span> | <span data-ttu-id="acc72-246">Descrizione</span><span class="sxs-lookup"><span data-stu-id="acc72-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="acc72-247">CompressionLevel. più veloce</span><span class="sxs-lookup"><span data-stu-id="acc72-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="acc72-248">La compressione deve essere completata il più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale.</span><span class="sxs-lookup"><span data-stu-id="acc72-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="acc72-249">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="acc72-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="acc72-250">Non deve essere eseguita alcuna compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="acc72-251">CompressionLevel. Optimal</span><span class="sxs-lookup"><span data-stu-id="acc72-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="acc72-252">Le risposte devono essere compresse in modo ottimale, anche se la compressione impiega più tempo per il completamento.</span><span class="sxs-lookup"><span data-stu-id="acc72-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="acc72-253">Provider personalizzati</span><span class="sxs-lookup"><span data-stu-id="acc72-253">Custom providers</span></span>

<span data-ttu-id="acc72-254">Creare implementazioni di compressione personalizzate con <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="acc72-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="acc72-255">Il <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> rappresenta la codifica del contenuto prodotta da questo `ICompressionProvider`.</span><span class="sxs-lookup"><span data-stu-id="acc72-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="acc72-256">Il middleware usa queste informazioni per scegliere il provider in base all'elenco specificato nell'intestazione `Accept-Encoding` della richiesta.</span><span class="sxs-lookup"><span data-stu-id="acc72-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="acc72-257">Usando l'app di esempio, il client invia una richiesta con l'intestazione `Accept-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="acc72-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="acc72-258">Il middleware usa l'implementazione della compressione personalizzata e restituisce la risposta con un'intestazione `Content-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="acc72-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="acc72-259">Il client deve essere in grado di decomprimere la codifica personalizzata per consentire il funzionamento di un'implementazione della compressione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="acc72-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="acc72-260">Inviare una richiesta all'app di esempio con l'intestazione `Accept-Encoding: mycustomcompression` e osservare le intestazioni della risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="acc72-261">Le intestazioni `Vary` e `Content-Encoding` sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="acc72-262">Il corpo della risposta (non visualizzato) non è compresso dall'esempio.</span><span class="sxs-lookup"><span data-stu-id="acc72-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="acc72-263">Non esiste un'implementazione della compressione nella classe `CustomCompressionProvider` dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="acc72-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="acc72-264">Tuttavia, l'esempio mostra dove implementare tale algoritmo di compressione.</span><span class="sxs-lookup"><span data-stu-id="acc72-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Finestra Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="acc72-267">MIME (tipi)</span><span class="sxs-lookup"><span data-stu-id="acc72-267">MIME types</span></span>

<span data-ttu-id="acc72-268">Il middleware specifica un set predefinito di tipi MIME per la compressione:</span><span class="sxs-lookup"><span data-stu-id="acc72-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="acc72-269">Sostituire o aggiungere tipi MIME con le opzioni del middleware di compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="acc72-270">Si noti che i tipi MIME con caratteri jolly, ad esempio `text/*` non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="acc72-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="acc72-271">L'app di esempio aggiunge un tipo MIME per `image/svg+xml` e comprime e serve l'immagine del banner ASP.NET Core (*banner. svg*).</span><span class="sxs-lookup"><span data-stu-id="acc72-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="acc72-272">Compressione con protocollo sicuro</span><span class="sxs-lookup"><span data-stu-id="acc72-272">Compression with secure protocol</span></span>

<span data-ttu-id="acc72-273">È possibile controllare le risposte compresse su connessioni sicure con l'opzione `EnableForHttps`, che è disabilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="acc72-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="acc72-274">L'uso della compressione con pagine generate dinamicamente può causare problemi di sicurezza, ad esempio gli attacchi di [reato](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violazione](https://wikipedia.org/wiki/BREACH_(security_exploit)) .</span><span class="sxs-lookup"><span data-stu-id="acc72-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="acc72-275">Aggiunta dell'intestazione Vary</span><span class="sxs-lookup"><span data-stu-id="acc72-275">Adding the Vary header</span></span>

<span data-ttu-id="acc72-276">Quando si comprimono le risposte in base all'intestazione `Accept-Encoding`, esistono potenzialmente più versioni compresse della risposta e una versione non compressa.</span><span class="sxs-lookup"><span data-stu-id="acc72-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="acc72-277">Per indicare alle cache del client e del proxy che esistono più versioni e che devono essere archiviate, l'intestazione `Vary` viene aggiunta con un valore `Accept-Encoding`.</span><span class="sxs-lookup"><span data-stu-id="acc72-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="acc72-278">In ASP.NET Core 2,0 o versioni successive, il middleware aggiunge automaticamente l'intestazione `Vary` quando la risposta viene compressa.</span><span class="sxs-lookup"><span data-stu-id="acc72-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="acc72-279">Problema del middleware quando si trova dietro un proxy inverso nginx</span><span class="sxs-lookup"><span data-stu-id="acc72-279">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="acc72-280">Quando una richiesta viene sottoposta a proxy da nginx, l'intestazione `Accept-Encoding` viene rimossa.</span><span class="sxs-lookup"><span data-stu-id="acc72-280">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="acc72-281">La rimozione dell'intestazione `Accept-Encoding` impedisce la compressione della risposta da parte del middleware.</span><span class="sxs-lookup"><span data-stu-id="acc72-281">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="acc72-282">Per altre informazioni, vedere [Nginx: compressione e decompressione](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="acc72-282">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="acc72-283">Questo problema viene rilevato in base [alla figura della compressione pass-through per Nginx (ASPNET/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="acc72-283">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="acc72-284">Utilizzo della compressione dinamica di IIS</span><span class="sxs-lookup"><span data-stu-id="acc72-284">Working with IIS dynamic compression</span></span>

<span data-ttu-id="acc72-285">Se si dispone di un modulo di compressione dinamica IIS attivo configurato a livello di server che si desidera disabilitare per un'app, disabilitare il modulo con un'aggiunta al file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="acc72-285">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="acc72-286">Per altre informazioni, vedere [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Disabilitazione di moduli IIS).</span><span class="sxs-lookup"><span data-stu-id="acc72-286">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="acc72-287">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="acc72-287">Troubleshooting</span></span>

<span data-ttu-id="acc72-288">Usare uno strumento come [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)o [postazione](https://www.getpostman.com/), che consente di impostare l'intestazione della richiesta `Accept-Encoding` e studiare le intestazioni, le dimensioni e il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-288">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="acc72-289">Per impostazione predefinita, il middleware della compressione della risposta comprime le risposte che soddisfano le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="acc72-289">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="acc72-290">L'intestazione `Accept-Encoding` è presente con il valore `br`, `gzip`, `*`o la codifica personalizzata corrispondente a un provider di compressione personalizzato stabilito.</span><span class="sxs-lookup"><span data-stu-id="acc72-290">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="acc72-291">Il valore non deve essere `identity` o avere un valore di qualità (qValue, `q`) impostato su 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="acc72-291">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="acc72-292">Il tipo MIME (`Content-Type`) deve essere impostato e deve corrispondere a un tipo MIME configurato nell'<xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="acc72-292">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="acc72-293">La richiesta non deve includere l'intestazione `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="acc72-293">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="acc72-294">La richiesta deve usare il protocollo non sicuro (http), a meno che non sia configurato il protocollo Secure Protocol (HTTPS) nelle opzioni del middleware della compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-294">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="acc72-295">*Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si Abilita la compressione del contenuto protetta.*</span><span class="sxs-lookup"><span data-stu-id="acc72-295">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="acc72-296">L'intestazione `Accept-Encoding` è presente con un valore di `gzip`, `*`o codifica personalizzata che corrisponde a un provider di compressione personalizzato stabilito.</span><span class="sxs-lookup"><span data-stu-id="acc72-296">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="acc72-297">Il valore non deve essere `identity` o avere un valore di qualità (qValue, `q`) impostato su 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="acc72-297">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="acc72-298">Il tipo MIME (`Content-Type`) deve essere impostato e deve corrispondere a un tipo MIME configurato nell'<xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="acc72-298">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="acc72-299">La richiesta non deve includere l'intestazione `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="acc72-299">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="acc72-300">La richiesta deve usare il protocollo non sicuro (http), a meno che non sia configurato il protocollo Secure Protocol (HTTPS) nelle opzioni del middleware della compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="acc72-300">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="acc72-301">*Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si Abilita la compressione del contenuto protetta.*</span><span class="sxs-lookup"><span data-stu-id="acc72-301">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="acc72-302">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="acc72-302">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="acc72-303">Mozilla Developer Network: Accept-Encoding</span><span class="sxs-lookup"><span data-stu-id="acc72-303">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="acc72-304">RFC 7231 sezione 3.1.2.1: codifiche del contenuto</span><span class="sxs-lookup"><span data-stu-id="acc72-304">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="acc72-305">RFC 7230 sezione 4.2.3: codifica gzip</span><span class="sxs-lookup"><span data-stu-id="acc72-305">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="acc72-306">Specifica del formato di file GZIP versione 4,3</span><span class="sxs-lookup"><span data-stu-id="acc72-306">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
