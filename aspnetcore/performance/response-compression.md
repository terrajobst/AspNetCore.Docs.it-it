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
# <a name="response-compression-in-aspnet-core"></a>Compressione della risposta in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

La larghezza di banda di rete è una risorsa limitata. La riduzione delle dimensioni della risposta in genere aumenta la velocità di risposta di un'app, spesso in modo significativo. Un modo per ridurre le dimensioni del payload consiste nel comprimere le risposte di un'app.

## <a name="when-to-use-response-compression-middleware"></a>Quando usare il middleware di compressione della risposta

Usare tecnologie di compressione delle risposte basate su server in IIS, Apache o nginx. Le prestazioni del middleware probabilmente non corrispondono a quelle dei moduli server. Il server [http. sys](xref:fundamentals/servers/httpsys) server e il server [gheppio](xref:fundamentals/servers/kestrel) attualmente non offrono il supporto predefinito per la compressione.

Usare il middleware di compressione della risposta quando si è:

* Impossibile utilizzare le seguenti tecnologie di compressione basate su server:
  * [Modulo di compressione dinamica di IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Modulo Apache mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Compressione e decompressione nginx](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hosting diretto in:
  * [Server http. sys](xref:fundamentals/servers/httpsys) (chiamato in precedenza webListener)
  * [Server gheppio](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Compressione delle risposte

In genere, qualsiasi risposta non compressa in modo nativo può trarre vantaggio dalla compressione della risposta. Le risposte non compresse in modo nativo includono in genere: CSS, JavaScript, HTML, XML e JSON. Non è consigliabile comprimere asset compressi in modo nativo, ad esempio file PNG. Se si prova a comprimere ulteriormente una risposta compressa in modo nativo, è probabile che qualsiasi riduzione aggiuntiva delle dimensioni e del tempo di trasmissione venga nascosta entro il tempo richiesto per elaborare la compressione. Non comprimere file di dimensioni inferiori a circa 150-1000 byte (a seconda del contenuto del file e dell'efficienza della compressione). L'overhead di compressione di file piccoli può produrre un file compresso di dimensioni superiori a quelle del file non compresso.

Quando un client è in grado di elaborare contenuto compresso, il client deve informare il server delle sue funzionalità inviando l'intestazione `Accept-Encoding` con la richiesta. Quando un server invia contenuto compresso, deve includere informazioni nell'intestazione `Content-Encoding` sulla modalità di codifica della risposta compressa. Le designazioni di codifica del contenuto supportate dal middleware sono illustrate nella tabella seguente.

::: moniker range=">= aspnetcore-2.2"

| valori di intestazione `Accept-Encoding` | Middleware supportato | Descrizione |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Sì (impostazione predefinita)        | [Formato dati compresso Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | No                   | [Deflate formato dati compresso](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | No                   | [Interscambio XML efficiente W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Sì                  | [Formato di file gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Sì                  | Identificatore "senza codifica": la risposta non deve essere codificata. |
| `pack200-gzip`                  | No                   | [Formato di trasferimento di rete per gli archivi Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Sì                  | Qualsiasi codifica del contenuto disponibile non richiesta in modo esplicito |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| valori di intestazione `Accept-Encoding` | Middleware supportato | Descrizione |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | No                   | [Formato dati compresso Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | No                   | [Deflate formato dati compresso](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | No                   | [Interscambio XML efficiente W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Sì (impostazione predefinita)        | [Formato di file gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Sì                  | Identificatore "senza codifica": la risposta non deve essere codificata. |
| `pack200-gzip`                  | No                   | [Formato di trasferimento di rete per gli archivi Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Sì                  | Qualsiasi codifica del contenuto disponibile non richiesta in modo esplicito |

::: moniker-end

Per ulteriori informazioni, vedere l' [elenco di codici di contenuto ufficiale IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

Il middleware consente di aggiungere ulteriori provider di compressione per i valori di intestazione `Accept-Encoding` personalizzati. Per ulteriori informazioni, vedere [provider personalizzati](#custom-providers) di seguito.

Il middleware è in grado di reagire alla ponderazione del valore di qualità (qValue, `q`) quando viene inviato dal client per definire la priorità degli schemi di compressione. Per altre informazioni, vedere [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Gli algoritmi di compressione sono soggetti a un compromesso tra la velocità di compressione e l'efficacia della compressione. L' *efficacia* in questo contesto si riferisce alla dimensione dell'output dopo la compressione. La dimensione più piccola viene conseguita dalla compressione più *ottimale* .

Le intestazioni necessarie per la richiesta, l'invio, la memorizzazione nella cache e la ricezione di contenuti compressi sono descritte nella tabella seguente.

| Header             | Ruolo |
| ------------------ | ---- |
| `Accept-Encoding`  | Inviato dal client al server per indicare gli schemi di codifica del contenuto accettabili per il client. |
| `Content-Encoding` | Inviato dal server al client per indicare la codifica del contenuto nel payload. |
| `Content-Length`   | Quando si verifica la compressione, l'intestazione `Content-Length` viene rimossa, perché il contenuto del corpo viene modificato quando la risposta viene compressa. |
| `Content-MD5`      | Quando si verifica la compressione, l'intestazione `Content-MD5` viene rimossa, perché il contenuto del corpo è stato modificato e l'hash non è più valido. |
| `Content-Type`     | Specifica il tipo MIME del contenuto. Ogni risposta deve specificare il `Content-Type`. Il middleware controlla questo valore per determinare se la risposta deve essere compressa. Il middleware specifica un set di [tipi MIME predefiniti](#mime-types) che può codificare, ma è possibile sostituire o aggiungere tipi MIME. |
| `Vary`             | Quando viene inviato dal server con un valore di `Accept-Encoding` ai client e ai proxy, l'intestazione `Vary` indica al client o al proxy che deve memorizzare nella cache (variare) le risposte in base al valore dell'intestazione `Accept-Encoding` della richiesta. Il risultato della restituzione di contenuto con l'intestazione `Vary: Accept-Encoding` è che le risposte compresse e non compresse vengono memorizzate nella cache separatamente. |

Esplorare le funzionalità del middleware di compressione della risposta con l' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples). Nell'esempio vengono illustrate le operazioni seguenti:

* La compressione delle risposte delle app usando gzip e i provider di compressione personalizzati.
* Come aggiungere un tipo MIME all'elenco predefinito di tipi MIME per la compressione.

## <a name="package"></a>Pacchetto

::: moniker range=">= aspnetcore-3.0"

Il middleware di compressione della risposta è fornito dal pacchetto [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , incluso in modo implicito nelle app ASP.NET Core.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Per includere il middleware in un progetto, aggiungere un riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), che include il pacchetto [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .

::: moniker-end

## <a name="configuration"></a>Configurazione di

::: moniker range=">= aspnetcore-2.2"

Il codice seguente illustra come abilitare il middleware di compressione della risposta per i tipi MIME e i provider di compressione predefiniti ([Brotli](#brotli-compression-provider) e [gzip](#gzip-compression-provider)):

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Il codice seguente illustra come abilitare il middleware di compressione della risposta per i tipi MIME predefiniti e il [provider di compressione gzip](#gzip-compression-provider):

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

Note:

* `app.UseResponseCompression` deve essere chiamato prima di qualsiasi middleware che comprime le risposte. Per ulteriori informazioni, vedere <xref:fundamentals/middleware/index#middleware-order>.
* Per impostare l'intestazione `Accept-Encoding` della richiesta e studiare le intestazioni, le dimensioni e il corpo della risposta, usare uno strumento, ad esempio [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/) o [Postman](https://www.getpostman.com/).

Inviare una richiesta all'app di esempio senza l'intestazione `Accept-Encoding` e osservare che la risposta non è compressa. Le intestazioni `Content-Encoding` e `Vary` non sono presenti nella risposta.

![Finestra Fiddler che mostra il risultato di una richiesta senza l'intestazione Accept-Encoding. La risposta non è compressa.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

Inviare una richiesta all'app di esempio con l'intestazione `Accept-Encoding: br` (compressione Brotli) e osservare che la risposta è compressa. Le intestazioni `Content-Encoding` e `Vary` sono presenti nella risposta.

![Finestra Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore br. Le intestazioni Vary e Content-Encoding vengono aggiunte alla risposta. La risposta è compressa.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Inviare una richiesta all'app di esempio con l'intestazione `Accept-Encoding: gzip` e osservare che la risposta è compressa. Le intestazioni `Content-Encoding` e `Vary` sono presenti nella risposta.

![Finestra Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore gzip. Le intestazioni Vary e Content-Encoding vengono aggiunte alla risposta. La risposta è compressa.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a>Provider

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a>Provider di compressione Brotli

Usare il <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> per comprimere le risposte con il [formato dati compresso Brotli](https://tools.ietf.org/html/rfc7932).

Se al <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>non vengono aggiunti provider di compressione in modo esplicito:

* Il provider di compressione Brotli viene aggiunto per impostazione predefinita alla matrice di provider di compressione insieme al [provider di compressione gzip](#gzip-compression-provider).
* Per impostazione predefinita, la compressione Brotli la compressione quando il formato dati compresso Brotli è supportato dal client. Se Brotli non è supportato dal client, la compressione viene assegnata per impostazione predefinita a gzip quando il client supporta la compressione gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

È necessario aggiungere il provider di compressione Brotoli quando vengono aggiunti in modo esplicito i provider di compressione:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Impostare il livello di compressione con <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>. Per impostazione predefinita, il provider di compressione Brotli è il livello di compressione più veloce ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente. Se si desidera la compressione più efficiente, configurare il middleware per la compressione ottimale.

| Livello di compressione | Descrizione |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | La compressione deve essere completata il più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale. |
| [CompressionLevel. NoCompression](xref:System.IO.Compression.CompressionLevel) | Non deve essere eseguita alcuna compressione. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Le risposte devono essere compresse in modo ottimale, anche se la compressione impiega più tempo per il completamento. |

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

### <a name="gzip-compression-provider"></a>Provider di compressione gzip

Usare il <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> per comprimere le risposte con il [formato di file gzip](https://tools.ietf.org/html/rfc1952).

Se al <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>non vengono aggiunti provider di compressione in modo esplicito:

::: moniker range=">= aspnetcore-2.2"

* Il provider di compressione Gzip viene aggiunto per impostazione predefinita alla matrice di provider di compressione insieme al [provider di compressione Brotli](#brotli-compression-provider).
* Per impostazione predefinita, la compressione Brotli la compressione quando il formato dati compresso Brotli è supportato dal client. Se Brotli non è supportato dal client, la compressione viene assegnata per impostazione predefinita a gzip quando il client supporta la compressione gzip.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Per impostazione predefinita, il provider di compressione Gzip viene aggiunto alla matrice di provider di compressione.
* Per impostazione predefinita, la compressione viene impostata su gzip quando il client supporta la compressione gzip.

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

È necessario aggiungere il provider di compressione gzip quando vengono aggiunti in modo esplicito i provider di compressione:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

Impostare il livello di compressione con <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. Per impostazione predefinita, il provider di compressione gzip è il livello di compressione più veloce ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente. Se si desidera la compressione più efficiente, configurare il middleware per la compressione ottimale.

| Livello di compressione | Descrizione |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | La compressione deve essere completata il più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale. |
| [CompressionLevel. NoCompression](xref:System.IO.Compression.CompressionLevel) | Non deve essere eseguita alcuna compressione. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Le risposte devono essere compresse in modo ottimale, anche se la compressione impiega più tempo per il completamento. |

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

### <a name="custom-providers"></a>Provider personalizzati

Creare implementazioni di compressione personalizzate con <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. Il <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> rappresenta la codifica del contenuto prodotta da questo `ICompressionProvider`. Il middleware usa queste informazioni per scegliere il provider in base all'elenco specificato nell'intestazione `Accept-Encoding` della richiesta.

Usando l'app di esempio, il client invia una richiesta con l'intestazione `Accept-Encoding: mycustomcompression`. Il middleware usa l'implementazione della compressione personalizzata e restituisce la risposta con un'intestazione `Content-Encoding: mycustomcompression`. Il client deve essere in grado di decomprimere la codifica personalizzata per consentire il funzionamento di un'implementazione della compressione personalizzata.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

Inviare una richiesta all'app di esempio con l'intestazione `Accept-Encoding: mycustomcompression` e osservare le intestazioni della risposta. Le intestazioni `Vary` e `Content-Encoding` sono presenti nella risposta. Il corpo della risposta (non visualizzato) non è compresso dall'esempio. Non esiste un'implementazione della compressione nella classe `CustomCompressionProvider` dell'esempio. Tuttavia, l'esempio mostra dove implementare tale algoritmo di compressione.

![Finestra Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore mycustomcompression. Le intestazioni Vary e Content-Encoding vengono aggiunte alla risposta.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>MIME (tipi)

Il middleware specifica un set predefinito di tipi MIME per la compressione:

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Sostituire o aggiungere tipi MIME con le opzioni del middleware di compressione della risposta. Si noti che i tipi MIME con caratteri jolly, ad esempio `text/*` non sono supportati. L'app di esempio aggiunge un tipo MIME per `image/svg+xml` e comprime e serve l'immagine del banner ASP.NET Core (*banner. svg*).

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a>Compressione con protocollo sicuro

È possibile controllare le risposte compresse su connessioni sicure con l'opzione `EnableForHttps`, che è disabilitata per impostazione predefinita. L'uso della compressione con pagine generate dinamicamente può causare problemi di sicurezza, ad esempio gli attacchi di [reato](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violazione](https://wikipedia.org/wiki/BREACH_(security_exploit)) .

## <a name="adding-the-vary-header"></a>Aggiunta dell'intestazione Vary

Quando si comprimono le risposte in base all'intestazione `Accept-Encoding`, esistono potenzialmente più versioni compresse della risposta e una versione non compressa. Per indicare alle cache del client e del proxy che esistono più versioni e che devono essere archiviate, l'intestazione `Vary` viene aggiunta con un valore `Accept-Encoding`. In ASP.NET Core 2,0 o versioni successive, il middleware aggiunge automaticamente l'intestazione `Vary` quando la risposta viene compressa.

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Problema del middleware quando si trova dietro un proxy inverso nginx

Quando una richiesta viene sottoposta a proxy da nginx, l'intestazione `Accept-Encoding` viene rimossa. La rimozione dell'intestazione `Accept-Encoding` impedisce la compressione della risposta da parte del middleware. Per altre informazioni, vedere [Nginx: compressione e decompressione](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Questo problema viene rilevato in base [alla figura della compressione pass-through per Nginx (ASPNET/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Utilizzo della compressione dinamica di IIS

Se si dispone di un modulo di compressione dinamica IIS attivo configurato a livello di server che si desidera disabilitare per un'app, disabilitare il modulo con un'aggiunta al file *Web. config* . Per altre informazioni, vedere [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Disabilitazione di moduli IIS).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Usare uno strumento come [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)o [Postman](https://www.getpostman.com/), che consente di impostare l'intestazione `Accept-Encoding` della richiesta e studiare le intestazioni, le dimensioni e il corpo della risposta. Per impostazione predefinita, il middleware della compressione della risposta comprime le risposte che soddisfano le condizioni seguenti:

::: moniker range=">= aspnetcore-2.2"

* L'intestazione `Accept-Encoding` è presente con il valore `br`, `gzip`, `*`o la codifica personalizzata corrispondente a un provider di compressione personalizzato stabilito. Il valore non deve essere `identity` o avere un valore di qualità (qValue, `q`) impostato su 0 (zero).
* Il tipo MIME (`Content-Type`) deve essere impostato e deve corrispondere a un tipo MIME configurato nell'<xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* La richiesta non deve includere l'intestazione `Content-Range`.
* La richiesta deve usare il protocollo non sicuro (http), a meno che non sia configurato il protocollo Secure Protocol (HTTPS) nelle opzioni del middleware della compressione della risposta. *Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si Abilita la compressione del contenuto protetta.*

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* L'intestazione `Accept-Encoding` è presente con un valore di `gzip`, `*`o codifica personalizzata che corrisponde a un provider di compressione personalizzato stabilito. Il valore non deve essere `identity` o avere un valore di qualità (qValue, `q`) impostato su 0 (zero).
* Il tipo MIME (`Content-Type`) deve essere impostato e deve corrispondere a un tipo MIME configurato nell'<xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* La richiesta non deve includere l'intestazione `Content-Range`.
* La richiesta deve usare il protocollo non sicuro (http), a meno che non sia configurato il protocollo Secure Protocol (HTTPS) nelle opzioni del middleware della compressione della risposta. *Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si Abilita la compressione del contenuto protetta.*

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Mozilla Developer Network: Accept-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 sezione 3.1.2.1: codifiche del contenuto](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 sezione 4.2.3: codifica gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Specifica del formato di file GZIP versione 4,3](https://www.ietf.org/rfc/rfc1952.txt)
