---
title: Middleware di compressione di risposta per ASP.NET Core
author: guardrex
description: Informazioni sulla compressione di risposta e come utilizzare il Middleware di compressione risposta nelle applicazioni ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: 152799500577dd09247bcee8c87cde39ca20aa79
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729574"
---
# <a name="response-compression-middleware-for-aspnet-core"></a><span data-ttu-id="87834-103">Middleware di compressione di risposta per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="87834-103">Response Compression Middleware for ASP.NET Core</span></span>

<span data-ttu-id="87834-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="87834-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="87834-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="87834-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="87834-106">Larghezza di banda di rete è una risorsa limitata.</span><span class="sxs-lookup"><span data-stu-id="87834-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="87834-107">Riduzione delle dimensioni della risposta in genere aumenta la velocità di risposta di un'app, spesso in modo significativo.</span><span class="sxs-lookup"><span data-stu-id="87834-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="87834-108">Un modo per ridurre le dimensioni di payload consiste nella compressione delle risposte di un'app.</span><span class="sxs-lookup"><span data-stu-id="87834-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="87834-109">Quando utilizzare il Middleware di compressione risposta</span><span class="sxs-lookup"><span data-stu-id="87834-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="87834-110">Utilizzare le tecnologie di compressione risposta basata sul server in IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="87834-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="87834-111">Le prestazioni del middleware probabilmente non corrisponderanno a quello dei moduli server.</span><span class="sxs-lookup"><span data-stu-id="87834-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="87834-112">[Server HTTP. sys](xref:fundamentals/servers/httpsys) e [Kestrel](xref:fundamentals/servers/kestrel) attualmente non offre il supporto della compressione incorporata.</span><span class="sxs-lookup"><span data-stu-id="87834-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="87834-113">Utilizzare il Middleware di compressione risposta quando si è:</span><span class="sxs-lookup"><span data-stu-id="87834-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="87834-114">Impossibile utilizzare le tecnologie di compressione basata su server seguenti:</span><span class="sxs-lookup"><span data-stu-id="87834-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="87834-115">Modulo di compressione dinamica di IIS</span><span class="sxs-lookup"><span data-stu-id="87834-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="87834-116">Modulo mod_deflate Apache</span><span class="sxs-lookup"><span data-stu-id="87834-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="87834-117">Nginx compressione e decompressione</span><span class="sxs-lookup"><span data-stu-id="87834-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="87834-118">Hosting direttamente in:</span><span class="sxs-lookup"><span data-stu-id="87834-118">Hosting directly on:</span></span>
  * <span data-ttu-id="87834-119">[Server HTTP. sys](xref:fundamentals/servers/httpsys) (in precedenza denominato [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="87834-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="87834-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="87834-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="87834-121">Compressione di risposta</span><span class="sxs-lookup"><span data-stu-id="87834-121">Response compression</span></span>

<span data-ttu-id="87834-122">In genere, le risposte compresse in modo nativo possono trarre vantaggio dalla compressione di risposta.</span><span class="sxs-lookup"><span data-stu-id="87834-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="87834-123">Le risposte compresse in modo nativo in genere includono: CSS, JavaScript, HTML, XML e JSON.</span><span class="sxs-lookup"><span data-stu-id="87834-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="87834-124">Non deve comprimere asset compresso in modo nativo, ad esempio i file PNG.</span><span class="sxs-lookup"><span data-stu-id="87834-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="87834-125">Se si tenta di comprimere ulteriormente una risposta compressa in modo nativo, qualsiasi lieve riduzione aggiuntiva in fase di dimensioni e la trasmissione verrà probabilmente essere rimane in ombra base al tempo impiegato per elaborare la compressione.</span><span class="sxs-lookup"><span data-stu-id="87834-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="87834-126">Non comprimere i file più piccoli di circa 150-1000 byte (a seconda del contenuto del file e l'efficienza della compressione).</span><span class="sxs-lookup"><span data-stu-id="87834-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="87834-127">L'overhead di compressione di file di piccole dimensioni può produrre un file compresso più grande del file non compresso.</span><span class="sxs-lookup"><span data-stu-id="87834-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="87834-128">Quando un client può elaborare il contenuto compresso, il client deve informare il server delle rispettive funzionalità inviando il `Accept-Encoding` intestazione con la richiesta.</span><span class="sxs-lookup"><span data-stu-id="87834-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="87834-129">Quando un server invia il contenuto compresso, deve includere informazioni di `Content-Encoding` intestazione nella modalità di codifica risposta compressa.</span><span class="sxs-lookup"><span data-stu-id="87834-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="87834-130">Designazioni di codifica del contenuto supportati dal middleware vengono visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="87834-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="87834-131">`Accept-Encoding` valori di intestazione</span><span class="sxs-lookup"><span data-stu-id="87834-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="87834-132">Middleware supportati</span><span class="sxs-lookup"><span data-stu-id="87834-132">Middleware Supported</span></span> | <span data-ttu-id="87834-133">Descrizione</span><span class="sxs-lookup"><span data-stu-id="87834-133">Description</span></span>                                                 |
| ------------------------------- | :------------------: | ----------------------------------------------------------- |
| `br`                            | <span data-ttu-id="87834-134">No</span><span class="sxs-lookup"><span data-stu-id="87834-134">No</span></span>                   | <span data-ttu-id="87834-135">Formato di dati compressi Brotli</span><span class="sxs-lookup"><span data-stu-id="87834-135">Brotli Compressed Data Format</span></span>                               |
| `compress`                      | <span data-ttu-id="87834-136">No</span><span class="sxs-lookup"><span data-stu-id="87834-136">No</span></span>                   | <span data-ttu-id="87834-137">Formato di dati "comprimere" UNIX</span><span class="sxs-lookup"><span data-stu-id="87834-137">UNIX "compress" data format</span></span>                                 |
| `deflate`                       | <span data-ttu-id="87834-138">No</span><span class="sxs-lookup"><span data-stu-id="87834-138">No</span></span>                   | <span data-ttu-id="87834-139">i dati compressi in formato "zlib" dati "deflate"</span><span class="sxs-lookup"><span data-stu-id="87834-139">"deflate" compressed data inside the "zlib" data format</span></span>     |
| `exi`                           | <span data-ttu-id="87834-140">No</span><span class="sxs-lookup"><span data-stu-id="87834-140">No</span></span>                   | <span data-ttu-id="87834-141">W3C XML efficiente interscambio</span><span class="sxs-lookup"><span data-stu-id="87834-141">W3C Efficient XML Interchange</span></span>                               |
| `gzip`                          | <span data-ttu-id="87834-142">Sì (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="87834-142">Yes (default)</span></span>        | <span data-ttu-id="87834-143">formato del file gzip</span><span class="sxs-lookup"><span data-stu-id="87834-143">gzip file format</span></span>                                            |
| `identity`                      | <span data-ttu-id="87834-144">Yes</span><span class="sxs-lookup"><span data-stu-id="87834-144">Yes</span></span>                  | <span data-ttu-id="87834-145">"Nessuna codifica" identificatore: la risposta non deve essere codificata.</span><span class="sxs-lookup"><span data-stu-id="87834-145">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="87834-146">No</span><span class="sxs-lookup"><span data-stu-id="87834-146">No</span></span>                   | <span data-ttu-id="87834-147">Formato di trasferimento di rete per gli archivi di Java</span><span class="sxs-lookup"><span data-stu-id="87834-147">Network Transfer Format for Java Archives</span></span>                   |
| `*`                             | <span data-ttu-id="87834-148">Yes</span><span class="sxs-lookup"><span data-stu-id="87834-148">Yes</span></span>                  | <span data-ttu-id="87834-149">Qualsiasi contenuto disponibile, la codifica non è richiesto in modo esplicito</span><span class="sxs-lookup"><span data-stu-id="87834-149">Any available content encoding not explicitly requested</span></span>     |

<span data-ttu-id="87834-150">Per ulteriori informazioni, vedere il [IANA ufficiale elenco codifica contenuto](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="87834-150">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="87834-151">Il middleware consente di aggiungere provider di compressione aggiuntiva per personalizzato `Accept-Encoding` valori di intestazione.</span><span class="sxs-lookup"><span data-stu-id="87834-151">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="87834-152">Per ulteriori informazioni, vedere [provider personalizzati](#custom-providers) sotto.</span><span class="sxs-lookup"><span data-stu-id="87834-152">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="87834-153">Il middleware è in grado di rispondere al valore di qualità (qvalue, `q`) quando vengono inviati dal client per gli schemi di compressione la priorità di ponderazione.</span><span class="sxs-lookup"><span data-stu-id="87834-153">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="87834-154">Per ulteriori informazioni, vedere [RFC 7231: codifica](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="87834-154">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="87834-155">Gli algoritmi di compressione sono soggetti a un compromesso tra velocità di compressione e l'efficacia della compressione.</span><span class="sxs-lookup"><span data-stu-id="87834-155">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="87834-156">*Efficacia* in questo contesto si riferisce alla dimensione dell'output dopo la compressione.</span><span class="sxs-lookup"><span data-stu-id="87834-156">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="87834-157">La dimensione più piccola viene ottenuta da più *ottimale* compressione.</span><span class="sxs-lookup"><span data-stu-id="87834-157">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="87834-158">Le intestazioni coinvolti nella richiesta, invio, la memorizzazione nella cache e la ricezione di contenuto compresso sono descritti nella tabella riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="87834-158">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="87834-159">Header</span><span class="sxs-lookup"><span data-stu-id="87834-159">Header</span></span>             | <span data-ttu-id="87834-160">Ruolo</span><span class="sxs-lookup"><span data-stu-id="87834-160">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="87834-161">Inviata dal client al server per indicare gli schemi accettabili per il client di codifica del contenuto.</span><span class="sxs-lookup"><span data-stu-id="87834-161">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="87834-162">Inviati dal server al client per indicare la codifica del contenuto nel payload.</span><span class="sxs-lookup"><span data-stu-id="87834-162">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="87834-163">Quando si verifica la compressione, il `Content-Length` intestazione viene rimosso, poiché le modifiche di contenuto del corpo quando la risposta è compresso.</span><span class="sxs-lookup"><span data-stu-id="87834-163">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="87834-164">Quando si verifica la compressione, il `Content-MD5` intestazione viene rimosso, poiché è stato modificato il contenuto del corpo e l'hash non è più valido.</span><span class="sxs-lookup"><span data-stu-id="87834-164">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="87834-165">Specifica il tipo MIME del contenuto.</span><span class="sxs-lookup"><span data-stu-id="87834-165">Specifies the MIME type of the content.</span></span> <span data-ttu-id="87834-166">Ogni risposta è necessario specificare il relativo `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="87834-166">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="87834-167">Il middleware controlla questo valore per determinare se la risposta deve essere compresso.</span><span class="sxs-lookup"><span data-stu-id="87834-167">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="87834-168">Il middleware specifica un set di [predefinito di tipi MIME](#mime-types) che può codificare, ma è possibile sostituire o aggiungere i tipi MIME.</span><span class="sxs-lookup"><span data-stu-id="87834-168">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="87834-169">Quando vengono inviati dal server con un valore di `Accept-Encoding` ai client e proxy, il `Vary` intestazione indica al client o proxy di memorizzazione nella cache (variare) le risposte in base al valore del `Accept-Encoding` intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="87834-169">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="87834-170">Il risultato di restituzione del contenuto con il `Vary: Accept-Encoding` intestazione è che entrambi compressi e decompressi risposte memorizzate nella cache separatamente.</span><span class="sxs-lookup"><span data-stu-id="87834-170">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="87834-171">È possibile esplorare le funzionalità del Middleware di compressione risposta con il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="87834-171">You can explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="87834-172">Nell'esempio viene illustrato:</span><span class="sxs-lookup"><span data-stu-id="87834-172">The sample illustrates:</span></span>

* <span data-ttu-id="87834-173">La compressione delle risposte di app con gzip e provider di compressione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="87834-173">The compression of app responses using gzip and custom compression providers.</span></span>
* <span data-ttu-id="87834-174">Come aggiungere un tipo MIME per l'elenco predefinito di tipi MIME per la compressione.</span><span class="sxs-lookup"><span data-stu-id="87834-174">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="87834-175">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="87834-175">Package</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="87834-176">Per includere il middleware nel progetto, aggiungere un riferimento per il [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="87834-176">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span> <span data-ttu-id="87834-177">Questa funzionalità è disponibile per le app destinate ad ASP.NET Core 1.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="87834-177">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="87834-178">Per includere il middleware nel progetto, aggiungere un riferimento per il [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacchetto oppure utilizzare il [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="87834-178">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="87834-179">Per includere il middleware nel progetto, aggiungere un riferimento per il [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacchetto oppure utilizzare il [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="87834-179">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="87834-180">Configurazione</span><span class="sxs-lookup"><span data-stu-id="87834-180">Configuration</span></span>

<span data-ttu-id="87834-181">Il codice seguente viene illustrato come abilitare il Middleware di compressione di risposta con la compressione gzip predefinita e per i tipi MIME predefiniti.</span><span class="sxs-lookup"><span data-stu-id="87834-181">The following code shows how to enable the Response Compression Middleware with the default gzip compression and for default MIME types.</span></span>

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

> [!NOTE]
> <span data-ttu-id="87834-182">Utilizzare uno strumento come [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) per impostare il `Accept-Encoding` intestazione della richiesta ed esaminare le intestazioni di risposta, dimensioni e corpo.</span><span class="sxs-lookup"><span data-stu-id="87834-182">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="87834-183">Inviare una richiesta per l'app di esempio senza il `Accept-Encoding` intestazione e osservare che la risposta non compressa.</span><span class="sxs-lookup"><span data-stu-id="87834-183">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="87834-184">Il `Content-Encoding` e `Vary` intestazioni non sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="87834-184">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Finestra di Fiddler che mostra il risultato di una richiesta senza l'intestazione Accept-Encoding.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="87834-187">Inviare una richiesta per l'app di esempio con il `Accept-Encoding: gzip` intestazione e osservare che la risposta è compresso.</span><span class="sxs-lookup"><span data-stu-id="87834-187">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="87834-188">Il `Content-Encoding` e `Vary` le intestazioni sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="87834-188">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Finestra di Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore gzip.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="87834-192">Provider</span><span class="sxs-lookup"><span data-stu-id="87834-192">Providers</span></span>

### <a name="gzipcompressionprovider"></a><span data-ttu-id="87834-193">GzipCompressionProvider</span><span class="sxs-lookup"><span data-stu-id="87834-193">GzipCompressionProvider</span></span>

<span data-ttu-id="87834-194">Usare la [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) per comprimere le risposte con gzip.</span><span class="sxs-lookup"><span data-stu-id="87834-194">Use the [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) to compress responses with gzip.</span></span> <span data-ttu-id="87834-195">Questo è il provider di compressione predefinito se non è specificato.</span><span class="sxs-lookup"><span data-stu-id="87834-195">This is the default compression provider if none are specified.</span></span> <span data-ttu-id="87834-196">È possibile impostare la compressione a livello con il [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span><span class="sxs-lookup"><span data-stu-id="87834-196">You can set the compression level with the [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span></span>

<span data-ttu-id="87834-197">Il provider di compressione gzip per impostazione predefinita il livello di compressione più veloce ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), che potrebbe non produrre la compressione più efficiente.</span><span class="sxs-lookup"><span data-stu-id="87834-197">The gzip compression provider defaults to the fastest compression level ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="87834-198">Se si desidera usare la compressione più efficiente, è possibile configurare il middleware per la compressione ottimale.</span><span class="sxs-lookup"><span data-stu-id="87834-198">If the most efficient compression is desired, you can configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="87834-199">Livello di compressione</span><span class="sxs-lookup"><span data-stu-id="87834-199">Compression Level</span></span> | <span data-ttu-id="87834-200">Descrizione</span><span class="sxs-lookup"><span data-stu-id="87834-200">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="87834-201">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="87834-201">CompressionLevel.Fastest</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="87834-202">La compressione deve essere completata più rapidamente possibile, anche se l'output risultante non compressi in modo ottimale.</span><span class="sxs-lookup"><span data-stu-id="87834-202">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="87834-203">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="87834-203">CompressionLevel.NoCompression</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="87834-204">Non deve essere eseguita alcuna compressione.</span><span class="sxs-lookup"><span data-stu-id="87834-204">No compression should be performed.</span></span> |
| [<span data-ttu-id="87834-205">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="87834-205">CompressionLevel.Optimal</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="87834-206">Le risposte devono essere compressi in modo ottimale, anche se la compressione richiede più tempo per completare.</span><span class="sxs-lookup"><span data-stu-id="87834-206">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="87834-207">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="87834-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="87834-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="87834-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

---

## <a name="mime-types"></a><span data-ttu-id="87834-209">MIME (tipi)</span><span class="sxs-lookup"><span data-stu-id="87834-209">MIME types</span></span>

<span data-ttu-id="87834-210">Il middleware specifica un set predefinito di tipi MIME per la compressione:</span><span class="sxs-lookup"><span data-stu-id="87834-210">The middleware specifies a default set of MIME types for compression:</span></span>

* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

<span data-ttu-id="87834-211">È possibile sostituire o aggiungere tipi MIME con le opzioni del Middleware di compressione di risposta.</span><span class="sxs-lookup"><span data-stu-id="87834-211">You can replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="87834-212">Si noti che con caratteri jolly MIME tipi, ad esempio `text/*` non sono supportate.</span><span class="sxs-lookup"><span data-stu-id="87834-212">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="87834-213">L'app di esempio aggiunge un tipo MIME per `image/svg+xml` e consente di comprimere e serve ASP.NET Core immagine del banner (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="87834-213">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="87834-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="87834-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="87834-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="87834-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

---

### <a name="custom-providers"></a><span data-ttu-id="87834-216">Provider personalizzati</span><span class="sxs-lookup"><span data-stu-id="87834-216">Custom providers</span></span>

<span data-ttu-id="87834-217">È possibile creare le implementazioni di compressione personalizzata con [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span><span class="sxs-lookup"><span data-stu-id="87834-217">You can create custom compression implementations with [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span></span> <span data-ttu-id="87834-218">Il [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) rappresenta il contenuto da questo codifica `ICompressionProvider` produce.</span><span class="sxs-lookup"><span data-stu-id="87834-218">The [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="87834-219">Il middleware utilizza queste informazioni per scegliere il provider in base all'elenco specificato nella `Accept-Encoding` intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="87834-219">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="87834-220">Usando l'app di esempio, il client invia una richiesta con la `Accept-Encoding: mycustomcompression` intestazione.</span><span class="sxs-lookup"><span data-stu-id="87834-220">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="87834-221">Il middleware utilizza l'implementazione della compressione personalizzata e restituisce la risposta con un `Content-Encoding: mycustomcompression` intestazione.</span><span class="sxs-lookup"><span data-stu-id="87834-221">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="87834-222">Il client deve essere in grado di decomprimere la codifica personalizzata in ordine per un'implementazione personalizzata di compressione funzionare.</span><span class="sxs-lookup"><span data-stu-id="87834-222">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="87834-223">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="87834-223">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="87834-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="87834-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

<span data-ttu-id="87834-225">Inviare una richiesta per l'app di esempio con il `Accept-Encoding: mycustomcompression` intestazione e osservare le intestazioni di risposta.</span><span class="sxs-lookup"><span data-stu-id="87834-225">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="87834-226">Il `Vary` e `Content-Encoding` le intestazioni sono presenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="87834-226">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="87834-227">L'esempio non è compressa il corpo della risposta (non illustrato).</span><span class="sxs-lookup"><span data-stu-id="87834-227">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="87834-228">Non è un'implementazione della compressione nel `CustomCompressionProvider` classe dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="87834-228">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="87834-229">Tuttavia, nell'esempio viene illustrato dove si implementa un algoritmo di compressione.</span><span class="sxs-lookup"><span data-stu-id="87834-229">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Finestra di Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="87834-232">Compressione con protocollo sicuro</span><span class="sxs-lookup"><span data-stu-id="87834-232">Compression with secure protocol</span></span>

<span data-ttu-id="87834-233">Le risposte compresse tramite connessioni protette possono essere controllate con i `EnableForHttps` opzione, è disabilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="87834-233">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="87834-234">Utilizzo di compressione con pagine generate in modo dinamico può causare problemi di sicurezza, ad esempio il [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violazione](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacchi.</span><span class="sxs-lookup"><span data-stu-id="87834-234">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="87834-235">Aggiunta di intestazione Vary</span><span class="sxs-lookup"><span data-stu-id="87834-235">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="87834-236">Quando la compressione delle risposte in base il `Accept-Encoding` intestazione, vi sono potenzialmente più versioni compresse della risposta e una versione non compressa.</span><span class="sxs-lookup"><span data-stu-id="87834-236">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="87834-237">Per indicare di cache del client e del proxy che più versioni esistano e devono essere archiviate, il `Vary` intestazione viene aggiunto con un `Accept-Encoding` valore.</span><span class="sxs-lookup"><span data-stu-id="87834-237">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="87834-238">In ASP.NET Core 2.0 o versione successiva, il middleware aggiunge il `Vary` intestazione automaticamente quando la risposta è compresso.</span><span class="sxs-lookup"><span data-stu-id="87834-238">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="87834-239">Quando la compressione delle risposte in base il `Accept-Encoding` intestazione, vi sono potenzialmente più versioni compresse della risposta e una versione non compressa.</span><span class="sxs-lookup"><span data-stu-id="87834-239">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="87834-240">Per indicare di cache del client e del proxy che più versioni esistano e devono essere archiviate, il `Vary` intestazione viene aggiunto con un `Accept-Encoding` valore.</span><span class="sxs-lookup"><span data-stu-id="87834-240">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="87834-241">In ASP.NET Core 1.x, aggiunta di `Vary` intestazione nella risposta viene eseguita manualmente:</span><span class="sxs-lookup"><span data-stu-id="87834-241">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

<span data-ttu-id="87834-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="87834-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="87834-243">Problema di middleware dietro un proxy inverso Nginx</span><span class="sxs-lookup"><span data-stu-id="87834-243">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="87834-244">Quando una richiesta viene inviata da Nginx, il `Accept-Encoding` intestazione viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="87834-244">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="87834-245">Ciò impedisce il middleware di compressione della risposta.</span><span class="sxs-lookup"><span data-stu-id="87834-245">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="87834-246">Per ulteriori informazioni, vedere [NGINX: la compressione e decompressione](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="87834-246">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="87834-247">Questo problema viene rilevato da [scoprire la compressione di tipo pass-through per Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="87834-247">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="87834-248">Utilizzo con la compressione dinamica di IIS</span><span class="sxs-lookup"><span data-stu-id="87834-248">Working with IIS dynamic compression</span></span>

<span data-ttu-id="87834-249">Se si dispone di un active modulo di compressione dinamica IIS configurata a livello di server che si desidera disabilitare per un'app, è possibile farlo con un componente aggiuntivo per il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="87834-249">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="87834-250">Per altre informazioni, vedere [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Disabilitazione di moduli IIS).</span><span class="sxs-lookup"><span data-stu-id="87834-250">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="87834-251">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="87834-251">Troubleshooting</span></span>

<span data-ttu-id="87834-252">Utilizzare uno strumento come [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/), che consente di impostare il `Accept-Encoding` intestazione della richiesta ed esaminare le intestazioni di risposta, dimensioni e corpo.</span><span class="sxs-lookup"><span data-stu-id="87834-252">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="87834-253">Il Middleware di compressione risposta comprime le risposte che soddisfano le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="87834-253">The Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="87834-254">Il `Accept-Encoding` è presente con un valore di intestazione `gzip`, `*`, o codifica personalizzata che corrisponde a un provider di compressione personalizzata che è stata stabilita.</span><span class="sxs-lookup"><span data-stu-id="87834-254">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="87834-255">Il valore non deve essere `identity` o hanno un valore di qualità (qvalue, `q`) impostazione pari a 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="87834-255">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="87834-256">Il tipo MIME (`Content-Type`) deve essere impostata e deve corrispondere a un tipo MIME configurato nel [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span><span class="sxs-lookup"><span data-stu-id="87834-256">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span></span>
* <span data-ttu-id="87834-257">La richiesta non deve includere il `Content-Range` intestazione.</span><span class="sxs-lookup"><span data-stu-id="87834-257">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="87834-258">La richiesta deve utilizzare il protocollo non sicuro (http), a meno che un protocollo sicuro (https) viene configurato nelle opzioni del Middleware di compressione di risposta.</span><span class="sxs-lookup"><span data-stu-id="87834-258">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="87834-259">*Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) durante l'abilitazione della compressione del contenuto protetta.*</span><span class="sxs-lookup"><span data-stu-id="87834-259">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87834-260">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="87834-260">Additional resources</span></span>

* [<span data-ttu-id="87834-261">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="87834-261">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="87834-262">Middleware</span><span class="sxs-lookup"><span data-stu-id="87834-262">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="87834-263">Mozilla Developer Network: Codifica</span><span class="sxs-lookup"><span data-stu-id="87834-263">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="87834-264">Della sezione RFC 7231 3.1.2.1: Chiaramente tutti i codici del contenuto</span><span class="sxs-lookup"><span data-stu-id="87834-264">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="87834-265">RFC 7230 sezione 4.2.3: Codifica Gzip</span><span class="sxs-lookup"><span data-stu-id="87834-265">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="87834-266">Versione specifica del formato file GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="87834-266">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
