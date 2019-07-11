---
title: Compressione delle risposte in ASP.NET Core
author: guardrex
description: Informazioni sulla compressione delle risposte e su come usare il relativo middleware nelle app ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: performance/response-compression
ms.openlocfilehash: d5d2da3dc0a8a452de97d98161d429389d2f7638
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815609"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="5e9aa-103">Compressione delle risposte in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e9aa-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="5e9aa-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5e9aa-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5e9aa-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e9aa-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5e9aa-106">Larghezza di banda di rete è una risorsa limitata.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="5e9aa-107">Riduzione delle dimensioni della risposta in genere aumenta la velocità di risposta di un'app, spesso notevolmente.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="5e9aa-108">Un modo per ridurre le dimensioni dei payload è per la compressione delle risposte di un'app.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="5e9aa-109">Quando usare Middleware di compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="5e9aa-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="5e9aa-110">Utilizzare le tecnologie di compressione risposta basata sul server in IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="5e9aa-111">Le prestazioni del middleware probabilmente non corrisponde a quello dei moduli server.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="5e9aa-112">[Server HTTP. sys](xref:fundamentals/servers/httpsys) server e [Kestrel](xref:fundamentals/servers/kestrel) server non offre attualmente il supporto di compressione incorporata.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="5e9aa-113">Usare il Middleware di compressione delle risposte quando si è:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="5e9aa-114">Non è possibile usare le tecnologie di compressione basati su server seguenti:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="5e9aa-115">Modulo di compressione dinamica di IIS</span><span class="sxs-lookup"><span data-stu-id="5e9aa-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="5e9aa-116">Modulo Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="5e9aa-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="5e9aa-117">La decompressione e la compressione di Nginx</span><span class="sxs-lookup"><span data-stu-id="5e9aa-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="5e9aa-118">Hosting direttamente in:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-118">Hosting directly on:</span></span>
  * <span data-ttu-id="5e9aa-119">[Server HTTP. sys](xref:fundamentals/servers/httpsys) (precedentemente denominato WebListener)</span><span class="sxs-lookup"><span data-stu-id="5e9aa-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="5e9aa-120">Server kestrel</span><span class="sxs-lookup"><span data-stu-id="5e9aa-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="5e9aa-121">Compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="5e9aa-121">Response compression</span></span>

<span data-ttu-id="5e9aa-122">In genere, una risposta compressa in modo nativo può trarre vantaggio dalla compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="5e9aa-123">Le risposte in modo non nativo compressione in genere includono: CSS, JavaScript, HTML, XML e JSON.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="5e9aa-124">È consigliabile non comprimere asset compressi in modo nativo, ad esempio i file PNG.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="5e9aa-125">Se si tenta di comprimere ulteriormente una risposta compressa in modo nativo, un'altra riduzione piccole dimensioni e la trasmissione dei tempi verrà probabilmente essere eclissata dal tempo impiegato per elaborare la compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="5e9aa-126">Non comprimere i file di dimensioni inferiori a circa 150-1000 byte (a seconda del contenuto del file e l'efficienza della compressione).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="5e9aa-127">L'overhead di compressione dei file di piccole dimensioni può produrre un file compresso più grande rispetto al file non compresso.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="5e9aa-128">Quando un client riesce a elaborare il contenuto compresso, il client deve informare il server delle rispettive funzionalità mediante l'invio di `Accept-Encoding` intestazione con la richiesta.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="5e9aa-129">Quando un server invia il contenuto compresso, deve includere le informazioni nel `Content-Encoding` intestazione nella modalità di codifica della risposta compressa.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="5e9aa-130">Designazioni di codifica del contenuto supportati dal middleware vengono visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="5e9aa-131">`Accept-Encoding` valori di intestazione</span><span class="sxs-lookup"><span data-stu-id="5e9aa-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="5e9aa-132">Middleware supportati</span><span class="sxs-lookup"><span data-stu-id="5e9aa-132">Middleware Supported</span></span> | <span data-ttu-id="5e9aa-133">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="5e9aa-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="5e9aa-134">Sì (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="5e9aa-134">Yes (default)</span></span>        | [<span data-ttu-id="5e9aa-135">Formato di dati compressi Brotli</span><span class="sxs-lookup"><span data-stu-id="5e9aa-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="5e9aa-136">No</span><span class="sxs-lookup"><span data-stu-id="5e9aa-136">No</span></span>                   | [<span data-ttu-id="5e9aa-137">Formato di dati compressi DEFLATE</span><span class="sxs-lookup"><span data-stu-id="5e9aa-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="5e9aa-138">No</span><span class="sxs-lookup"><span data-stu-id="5e9aa-138">No</span></span>                   | [<span data-ttu-id="5e9aa-139">W3C XML efficiente interscambio</span><span class="sxs-lookup"><span data-stu-id="5e9aa-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="5e9aa-140">Yes</span><span class="sxs-lookup"><span data-stu-id="5e9aa-140">Yes</span></span>                  | [<span data-ttu-id="5e9aa-141">Formato del file gzip</span><span class="sxs-lookup"><span data-stu-id="5e9aa-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="5e9aa-142">Sì</span><span class="sxs-lookup"><span data-stu-id="5e9aa-142">Yes</span></span>                  | <span data-ttu-id="5e9aa-143">Identificatore "Alcuna codifica": La risposta non deve essere codificata.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="5e9aa-144">No</span><span class="sxs-lookup"><span data-stu-id="5e9aa-144">No</span></span>                   | [<span data-ttu-id="5e9aa-145">Formato di trasferimento di rete per gli archivi Java</span><span class="sxs-lookup"><span data-stu-id="5e9aa-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="5e9aa-146">Sì</span><span class="sxs-lookup"><span data-stu-id="5e9aa-146">Yes</span></span>                  | <span data-ttu-id="5e9aa-147">Qualsiasi contenuto disponibile la codifica non è richiesto in modo esplicito</span><span class="sxs-lookup"><span data-stu-id="5e9aa-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="5e9aa-148">`Accept-Encoding` valori di intestazione</span><span class="sxs-lookup"><span data-stu-id="5e9aa-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="5e9aa-149">Middleware supportati</span><span class="sxs-lookup"><span data-stu-id="5e9aa-149">Middleware Supported</span></span> | <span data-ttu-id="5e9aa-150">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="5e9aa-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="5e9aa-151">No</span><span class="sxs-lookup"><span data-stu-id="5e9aa-151">No</span></span>                   | [<span data-ttu-id="5e9aa-152">Formato di dati compressi Brotli</span><span class="sxs-lookup"><span data-stu-id="5e9aa-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="5e9aa-153">No</span><span class="sxs-lookup"><span data-stu-id="5e9aa-153">No</span></span>                   | [<span data-ttu-id="5e9aa-154">Formato di dati compressi DEFLATE</span><span class="sxs-lookup"><span data-stu-id="5e9aa-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="5e9aa-155">No</span><span class="sxs-lookup"><span data-stu-id="5e9aa-155">No</span></span>                   | [<span data-ttu-id="5e9aa-156">W3C XML efficiente interscambio</span><span class="sxs-lookup"><span data-stu-id="5e9aa-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="5e9aa-157">Sì (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="5e9aa-157">Yes (default)</span></span>        | [<span data-ttu-id="5e9aa-158">Formato del file gzip</span><span class="sxs-lookup"><span data-stu-id="5e9aa-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="5e9aa-159">Sì</span><span class="sxs-lookup"><span data-stu-id="5e9aa-159">Yes</span></span>                  | <span data-ttu-id="5e9aa-160">Identificatore "Alcuna codifica": La risposta non deve essere codificata.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="5e9aa-161">No</span><span class="sxs-lookup"><span data-stu-id="5e9aa-161">No</span></span>                   | [<span data-ttu-id="5e9aa-162">Formato di trasferimento di rete per gli archivi Java</span><span class="sxs-lookup"><span data-stu-id="5e9aa-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="5e9aa-163">Yes</span><span class="sxs-lookup"><span data-stu-id="5e9aa-163">Yes</span></span>                  | <span data-ttu-id="5e9aa-164">Qualsiasi contenuto disponibile la codifica non è richiesto in modo esplicito</span><span class="sxs-lookup"><span data-stu-id="5e9aa-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="5e9aa-165">Per altre informazioni, vedere la [IANA contenuto codifica elenco ufficiale](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="5e9aa-166">Il middleware consente di aggiungere i provider di compressione aggiuntiva per l'installazione personalizzata `Accept-Encoding` i valori di intestazione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="5e9aa-167">Per altre informazioni, vedere [provider personalizzati](#custom-providers) sotto.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="5e9aa-168">Il middleware è in grado di reagire al valore di qualità (qvalue, `q`) ponderazione quando inviato dal client per definire le priorità di schemi di compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="5e9aa-169">Per altre informazioni, vedere [RFC 7231: Codifica](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="5e9aa-170">Algoritmi di compressione sono soggetti a raggiungere un compromesso tra velocità di compressione e l'efficacia della compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="5e9aa-171">*L'efficacia* in questo contesto si riferisce alle dimensioni dell'output dopo la compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="5e9aa-172">La dimensione più piccola avviene tramite più *ottimali* la compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="5e9aa-173">Le intestazioni coinvolti nella richiesta, l'invio, la memorizzazione nella cache e la ricezione di contenuto compresso sono descritti nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="5e9aa-174">Intestazione</span><span class="sxs-lookup"><span data-stu-id="5e9aa-174">Header</span></span>             | <span data-ttu-id="5e9aa-175">Role</span><span class="sxs-lookup"><span data-stu-id="5e9aa-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="5e9aa-176">Inviato dal client al server per indicare gli schemi accettabili per il client di codifica del contenuto.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="5e9aa-177">Inviato dal server al client per indicare la codifica del contenuto nel payload.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="5e9aa-178">Quando si verifica la compressione, la `Content-Length` intestazione viene rimosso, poiché le modifiche del contenuto del corpo quando la risposta viene compresso.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="5e9aa-179">Quando si verifica la compressione, la `Content-MD5` intestazione viene rimosso, poiché è stato modificato il contenuto del corpo e l'hash non è più valido.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="5e9aa-180">Specifica il tipo MIME del contenuto.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="5e9aa-181">Ogni risposta deve specificare relativo `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="5e9aa-182">Il middleware controlla questo valore per determinare se la risposta deve essere compresso.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="5e9aa-183">Il middleware specifica un set di [predefinito di tipi MIME](#mime-types) che può codificare, ma è possibile sostituire o aggiungere tipi MIME.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="5e9aa-184">Quando inviato dal server con il valore `Accept-Encoding` ai client e i proxy, il `Vary` intestazione indica al client o al proxy di memorizzazione nella cache (variare) delle risposte in base al valore del `Accept-Encoding` intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="5e9aa-185">Il risultato della restituzione del contenuto con il `Vary: Accept-Encoding` intestazione è che entrambi compressi e decompressi le risposte vengono memorizzate nella cache separatamente.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="5e9aa-186">Scopri le funzionalità del Middleware di compressione delle risposte con il [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="5e9aa-187">L'esempio illustra:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-187">The sample illustrates:</span></span>

* <span data-ttu-id="5e9aa-188">La compressione delle risposte di app con Gzip e provider di compressione personalizzato.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="5e9aa-189">Come aggiungere un tipo MIME per l'elenco predefinito di tipi MIME per la compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="5e9aa-190">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="5e9aa-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5e9aa-191">Per includere il middleware in un progetto, aggiungere un riferimento per la [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app), che include il [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5e9aa-192">Per includere il middleware in un progetto, aggiungere un riferimento per la [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage), che include il [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e9aa-193">Per includere il middleware in un progetto, aggiungere un riferimento per la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="5e9aa-194">Configurazione</span><span class="sxs-lookup"><span data-stu-id="5e9aa-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5e9aa-195">Il codice seguente viene illustrato come abilitare il Middleware di compressione di risposta per la compressione dei provider e tipi MIME predefiniti ([Brotli](#brotli-compression-provider) e [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="5e9aa-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5e9aa-196">Il codice seguente viene illustrato come abilitare il Middleware di compressione di risposta per i tipi MIME predefiniti e il [Provider di compressione Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="5e9aa-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="5e9aa-197">Note:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-197">Notes:</span></span>

* <span data-ttu-id="5e9aa-198">`app.UseResponseCompression` deve essere chiamato prima `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-198">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="5e9aa-199">Usare uno strumento, ad esempio [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), o [Postman](https://www.getpostman.com/) impostare il `Accept-Encoding` intestazione della richiesta e studiare le intestazioni di risposta, dimensioni e corpo.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-199">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="5e9aa-200">Inviare una richiesta all'app di esempio senza il `Accept-Encoding` intestazione e osservare che la risposta non è compressa.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="5e9aa-201">Il `Content-Encoding` e `Vary` intestazioni non sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Finestra di Fiddler che mostra il risultato di una richiesta senza l'intestazione Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5e9aa-204">Inviare una richiesta all'app di esempio con il `Accept-Encoding: br` intestazione (compressione Brotli) e osservare che la risposta è compresso.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="5e9aa-205">Il `Content-Encoding` e `Vary` le intestazioni sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Finestra di Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5e9aa-209">Inviare una richiesta all'app di esempio con il `Accept-Encoding: gzip` intestazione e osservare che la risposta è compresso.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="5e9aa-210">Il `Content-Encoding` e `Vary` le intestazioni sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Finestra di Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="5e9aa-214">Provider</span><span class="sxs-lookup"><span data-stu-id="5e9aa-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="5e9aa-215">Provider di compressione Brotli</span><span class="sxs-lookup"><span data-stu-id="5e9aa-215">Brotli Compression Provider</span></span>

<span data-ttu-id="5e9aa-216">Usare la <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> per comprimere le risposte con i [formato di dati compressi Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="5e9aa-217">Se non vengono aggiunti in modo esplicito alcun provider di compressione di <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="5e9aa-218">Il Provider di compressione Brotli viene aggiunto per impostazione predefinita nella matrice di provider di compressione con il [provider di compressione Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="5e9aa-219">La compressione per impostazione predefinita la compressione Brotli quando il formato di dati compressi Brotli è supportato dal client.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="5e9aa-220">Se Brotli non è supportata dal client, impostazione predefinita la compressione su Gzip quando il client supporta la compressione Gzip.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="5e9aa-221">Il Provider di compressione Brotoli deve essere aggiunto quando vengono aggiunti esplicitamente tutti i provider di compressione:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5e9aa-222">Impostare il livello con la compressione <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="5e9aa-223">Il Provider di compressione Brotli per impostazione predefinita il livello di compressione più veloce ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="5e9aa-224">Se si desidera usare la compressione più efficiente, configurare il middleware di compressione non ottimale.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="5e9aa-225">Livello di compressione</span><span class="sxs-lookup"><span data-stu-id="5e9aa-225">Compression Level</span></span> | <span data-ttu-id="5e9aa-226">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="5e9aa-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="5e9aa-227">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="5e9aa-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="5e9aa-228">La compressione deve completare più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="5e9aa-229">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="5e9aa-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="5e9aa-230">Non deve essere eseguita alcuna compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="5e9aa-231">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="5e9aa-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="5e9aa-232">Le risposte devono essere compressi in modo ottimale, anche se la compressione richiede più tempo per il completamento.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="5e9aa-233">Provider di compressione gzip</span><span class="sxs-lookup"><span data-stu-id="5e9aa-233">Gzip Compression Provider</span></span>

<span data-ttu-id="5e9aa-234">Usare la <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> per comprimere le risposte con i [formato del file Gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="5e9aa-235">Se non vengono aggiunti in modo esplicito alcun provider di compressione di <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5e9aa-236">Il Provider di compressione Gzip viene aggiunto per impostazione predefinita per la matrice di provider di compressione con il [Provider di compressione Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="5e9aa-237">La compressione per impostazione predefinita la compressione Brotli quando il formato di dati compressi Brotli è supportato dal client.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="5e9aa-238">Se Brotli non è supportata dal client, impostazione predefinita la compressione su Gzip quando il client supporta la compressione Gzip.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="5e9aa-239">Il Provider di compressione Gzip viene aggiunto per impostazione predefinita nella matrice di provider di compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="5e9aa-240">Quando il client supporta la compressione Gzip per impostazione predefinita la compressione su Gzip.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="5e9aa-241">Il Provider di compressione Gzip deve essere aggiunto quando vengono aggiunti esplicitamente tutti i provider di compressione:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="5e9aa-242">Impostare il livello con la compressione <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="5e9aa-243">Il Provider di compressione Gzip per impostazione predefinita il livello di compressione più veloce ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="5e9aa-244">Se si desidera usare la compressione più efficiente, configurare il middleware di compressione non ottimale.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="5e9aa-245">Livello di compressione</span><span class="sxs-lookup"><span data-stu-id="5e9aa-245">Compression Level</span></span> | <span data-ttu-id="5e9aa-246">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="5e9aa-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="5e9aa-247">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="5e9aa-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="5e9aa-248">La compressione deve completare più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="5e9aa-249">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="5e9aa-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="5e9aa-250">Non deve essere eseguita alcuna compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="5e9aa-251">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="5e9aa-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="5e9aa-252">Le risposte devono essere compressi in modo ottimale, anche se la compressione richiede più tempo per il completamento.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="5e9aa-253">Provider personalizzati</span><span class="sxs-lookup"><span data-stu-id="5e9aa-253">Custom providers</span></span>

<span data-ttu-id="5e9aa-254">Creare le implementazioni di compressione personalizzato con <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="5e9aa-255">Il <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> rappresenta il contenuto da questo codifica `ICompressionProvider` produce.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="5e9aa-256">Il middleware utilizza queste informazioni per scegliere il provider in base all'elenco specificato nel `Accept-Encoding` intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="5e9aa-257">Usa l'app di esempio, il client invia una richiesta con il `Accept-Encoding: mycustomcompression` intestazione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="5e9aa-258">Il middleware utilizza l'implementazione di compressione personalizzato e restituisce la risposta con un `Content-Encoding: mycustomcompression` intestazione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="5e9aa-259">Il client deve essere in grado di decomprimere la codifica personalizzata affinché un'implementazione di compressione personalizzato lavorare.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5e9aa-260">Inviare una richiesta all'app di esempio con il `Accept-Encoding: mycustomcompression` intestazione e osservare le intestazioni della risposta.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="5e9aa-261">Il `Vary` e `Content-Encoding` le intestazioni sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="5e9aa-262">Il corpo della risposta (non mostrato) non viene compressa dall'esempio.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="5e9aa-263">Non c'è un'implementazione della compressione nel `CustomCompressionProvider` classe dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="5e9aa-264">Tuttavia, l'esempio mostra in cui è necessario implementare un algoritmo di compressione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Finestra di Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="5e9aa-267">MIME (tipi)</span><span class="sxs-lookup"><span data-stu-id="5e9aa-267">MIME types</span></span>

<span data-ttu-id="5e9aa-268">Il middleware specifica un set predefinito di tipi MIME per la compressione:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="5e9aa-269">Sostituire o appendere gli tipi MIME con le opzioni di Middleware di compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="5e9aa-270">Si noti che con caratteri jolly MIME tipi, ad esempio `text/*` non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="5e9aa-271">L'app di esempio aggiunge un tipo MIME per `image/svg+xml` comprime e viene usato ASP.NET Core immagine del banner (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="5e9aa-272">Compressione con protocollo di protezione</span><span class="sxs-lookup"><span data-stu-id="5e9aa-272">Compression with secure protocol</span></span>

<span data-ttu-id="5e9aa-273">Le risposte compresse su connessioni protette possono essere controllate con la `EnableForHttps` opzione, che è disabilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="5e9aa-274">Con la compressione delle pagine generate in modo dinamico può causare problemi di sicurezza, ad esempio la [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violazione](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacchi.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="5e9aa-275">Aggiunta l'intestazione Vary</span><span class="sxs-lookup"><span data-stu-id="5e9aa-275">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e9aa-276">Quando la compressione delle risposte in base il `Accept-Encoding` intestazione, esistono potenzialmente più versioni compresse della risposta e una versione non compressa.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="5e9aa-277">Per indicare a cache del client e del proxy che più versioni esistano e devono essere archiviate, il `Vary` intestazione viene aggiunta con un `Accept-Encoding` valore.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="5e9aa-278">In ASP.NET Core 2.0 o versioni successive, il middleware aggiunge il `Vary` intestazione automaticamente quando la risposta viene compresso.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e9aa-279">Quando la compressione delle risposte in base il `Accept-Encoding` intestazione, esistono potenzialmente più versioni compresse della risposta e una versione non compressa.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-279">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="5e9aa-280">Per indicare a cache del client e del proxy che più versioni esistano e devono essere archiviate, il `Vary` intestazione viene aggiunta con un `Accept-Encoding` valore.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-280">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="5e9aa-281">In ASP.NET Core 1.x, aggiungendo il `Vary` intestazione alla risposta viene eseguita manualmente:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-281">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="5e9aa-282">Problema di middleware quando dietro un proxy inverso di Nginx</span><span class="sxs-lookup"><span data-stu-id="5e9aa-282">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="5e9aa-283">Quando una richiesta viene trasmessa tramite proxy da Nginx, il `Accept-Encoding` intestazione viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-283">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="5e9aa-284">Rimozione del `Accept-Encoding` intestazione impedisce che il middleware di compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-284">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="5e9aa-285">Per altre informazioni, vedere [NGINX: La compressione e decompressione](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-285">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="5e9aa-286">Questo problema viene rilevato da [capire la compressione di tipo pass-through per Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-286">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="5e9aa-287">Utilizzo con la compressione dinamica di IIS</span><span class="sxs-lookup"><span data-stu-id="5e9aa-287">Working with IIS dynamic compression</span></span>

<span data-ttu-id="5e9aa-288">Se si dispone di un active modulo di compressione dinamica IIS configurata a livello di server che si vuole disabilitare per un'app, disabilitare il modulo con un componente aggiuntivo per il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-288">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="5e9aa-289">Per altre informazioni, vedere [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Disabilitazione di moduli IIS).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-289">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="5e9aa-290">risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="5e9aa-290">Troubleshooting</span></span>

<span data-ttu-id="5e9aa-291">Usare uno strumento come [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), o [Postman](https://www.getpostman.com/), che consentono di impostare il `Accept-Encoding` intestazione della richiesta e studiare le intestazioni di risposta, dimensioni e corpo.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-291">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="5e9aa-292">Per impostazione predefinita, Middleware di compressione delle risposte comprime le risposte che soddisfano le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5e9aa-292">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5e9aa-293">Il `Accept-Encoding` è presente con un valore di intestazione `br`, `gzip`, `*`, o codifica personalizzata che corrisponde a un provider di compressione personalizzato che è stata stabilita.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-293">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="5e9aa-294">Il valore non deve essere `identity` o avere un valore di qualità (qvalue, `q`) impostazione pari a 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-294">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="5e9aa-295">Il tipo MIME (`Content-Type`) deve essere impostata e deve corrispondere a un tipo MIME, configurato nel <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-295">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="5e9aa-296">La richiesta non deve includere il `Content-Range` intestazione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-296">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="5e9aa-297">La richiesta deve utilizzare il protocollo non sicuro (http), a meno che un protocollo sicuro (https) viene configurato nelle opzioni del Middleware di compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-297">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="5e9aa-298">*Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si abilita la compressione di contenuto protetta.*</span><span class="sxs-lookup"><span data-stu-id="5e9aa-298">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="5e9aa-299">Il `Accept-Encoding` è presente con un valore di intestazione `gzip`, `*`, o codifica personalizzata che corrisponde a un provider di compressione personalizzato che è stata stabilita.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-299">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="5e9aa-300">Il valore non deve essere `identity` o avere un valore di qualità (qvalue, `q`) impostazione pari a 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="5e9aa-300">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="5e9aa-301">Il tipo MIME (`Content-Type`) deve essere impostata e deve corrispondere a un tipo MIME, configurato nel <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-301">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="5e9aa-302">La richiesta non deve includere il `Content-Range` intestazione.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-302">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="5e9aa-303">La richiesta deve utilizzare il protocollo non sicuro (http), a meno che un protocollo sicuro (https) viene configurato nelle opzioni del Middleware di compressione delle risposte.</span><span class="sxs-lookup"><span data-stu-id="5e9aa-303">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="5e9aa-304">*Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si abilita la compressione di contenuto protetta.*</span><span class="sxs-lookup"><span data-stu-id="5e9aa-304">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5e9aa-305">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5e9aa-305">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="5e9aa-306">Mozilla Developer Network: Accept-Encoding</span><span class="sxs-lookup"><span data-stu-id="5e9aa-306">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="5e9aa-307">RFC 7231 Section 3.1.2.1: Chiaramente tutti i codici del contenuto</span><span class="sxs-lookup"><span data-stu-id="5e9aa-307">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="5e9aa-308">RFC 7230 sezione 4.2.3: Codifica gzip</span><span class="sxs-lookup"><span data-stu-id="5e9aa-308">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="5e9aa-309">Versione specifica del formato file GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="5e9aa-309">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
