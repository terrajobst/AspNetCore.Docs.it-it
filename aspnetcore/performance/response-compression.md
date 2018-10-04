---
title: Compressione delle risposte in ASP.NET Core
author: guardrex
description: Informazioni su come usare Middleware di compressione delle risposte nelle App ASP.NET Core e compressione delle risposte.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: performance/response-compression
ms.openlocfilehash: d5e0b6ed21c14f2e76396cde846c69a76ad40794
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578146"
---
# <a name="response-compression-in-aspnet-core"></a>Compressione delle risposte in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Larghezza di banda di rete è una risorsa limitata. Riduzione delle dimensioni della risposta in genere aumenta la velocità di risposta di un'app, spesso notevolmente. Un modo per ridurre le dimensioni dei payload è per la compressione delle risposte di un'app.

## <a name="when-to-use-response-compression-middleware"></a>Quando usare Middleware di compressione delle risposte

Utilizzare le tecnologie di compressione risposta basata sul server in IIS, Apache o Nginx. Le prestazioni del middleware probabilmente non corrisponde a quello dei moduli server. [Server HTTP. sys](xref:fundamentals/servers/httpsys) e [Kestrel](xref:fundamentals/servers/kestrel) non offre attualmente il supporto di compressione incorporata.

Usare il Middleware di compressione delle risposte quando si è:

* Non è possibile usare le tecnologie di compressione basati su server seguenti:
  * [Modulo di compressione dinamica di IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Modulo Apache mod_deflate](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [La decompressione e la compressione di Nginx](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hosting direttamente in:
  * [Server HTTP. sys](xref:fundamentals/servers/httpsys) (chiamato in precedenza [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Compressione delle risposte

In genere, una risposta compressa in modo nativo può trarre vantaggio dalla compressione delle risposte. Le risposte in modo non nativo compressione in genere includono: CSS, JavaScript, HTML, XML e JSON. È consigliabile non comprimere asset compressi in modo nativo, ad esempio i file PNG. Se si tenta di comprimere ulteriormente una risposta compressa in modo nativo, un'altra riduzione piccole dimensioni e la trasmissione dei tempi verrà probabilmente essere eclissata dal tempo impiegato per elaborare la compressione. Non comprimere i file di dimensioni inferiori a circa 150-1000 byte (a seconda del contenuto del file e l'efficienza della compressione). L'overhead di compressione dei file di piccole dimensioni può produrre un file compresso più grande rispetto al file non compresso.

Quando un client riesce a elaborare il contenuto compresso, il client deve informare il server delle rispettive funzionalità mediante l'invio di `Accept-Encoding` intestazione con la richiesta. Quando un server invia il contenuto compresso, deve includere le informazioni nel `Content-Encoding` intestazione nella modalità di codifica della risposta compressa. Designazioni di codifica del contenuto supportati dal middleware vengono visualizzati nella tabella seguente.

::: moniker range=">= aspnetcore-2.2"

| `Accept-Encoding` valori di intestazione | Middleware supportati | Descrizione |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Sì (impostazione predefinita)        | [Formato di dati compressi Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | No                   | [Formato di dati compressi DEFLATE](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | No                   | [W3C XML efficiente interscambio](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Yes                  | [Formato del file gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Yes                  | Identificatore "Alcuna codifica": la risposta non deve essere codificata. |
| `pack200-gzip`                  | No                   | [Formato di trasferimento di rete per gli archivi Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Yes                  | Qualsiasi contenuto disponibile la codifica non è richiesto in modo esplicito |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| `Accept-Encoding` valori di intestazione | Middleware supportati | Descrizione |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | No                   | [Formato di dati compressi Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | No                   | [Formato di dati compressi DEFLATE](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | No                   | [W3C XML efficiente interscambio](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Sì (impostazione predefinita)        | [Formato del file gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Yes                  | Identificatore "Alcuna codifica": la risposta non deve essere codificata. |
| `pack200-gzip`                  | No                   | [Formato di trasferimento di rete per gli archivi Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Yes                  | Qualsiasi contenuto disponibile la codifica non è richiesto in modo esplicito |

::: moniker-end

Per altre informazioni, vedere la [IANA contenuto codifica elenco ufficiale](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

Il middleware consente di aggiungere i provider di compressione aggiuntiva per l'installazione personalizzata `Accept-Encoding` i valori di intestazione. Per altre informazioni, vedere [provider personalizzati](#custom-providers) sotto.

Il middleware è in grado di reagire al valore di qualità (qvalue, `q`) ponderazione quando inviato dal client per definire le priorità di schemi di compressione. Per altre informazioni, vedere [RFC 7231: codifica](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Algoritmi di compressione sono soggetti a raggiungere un compromesso tra velocità di compressione e l'efficacia della compressione. *L'efficacia* in questo contesto si riferisce alle dimensioni dell'output dopo la compressione. La dimensione più piccola avviene tramite più *ottimali* la compressione.

Le intestazioni coinvolti nella richiesta, l'invio, la memorizzazione nella cache e la ricezione di contenuto compresso sono descritti nella tabella seguente.

| Intestazione             | Ruolo |
| ------------------ | ---- |
| `Accept-Encoding`  | Inviato dal client al server per indicare gli schemi accettabili per il client di codifica del contenuto. |
| `Content-Encoding` | Inviato dal server al client per indicare la codifica del contenuto nel payload. |
| `Content-Length`   | Quando si verifica la compressione, la `Content-Length` intestazione viene rimosso, poiché le modifiche del contenuto del corpo quando la risposta viene compresso. |
| `Content-MD5`      | Quando si verifica la compressione, la `Content-MD5` intestazione viene rimosso, poiché è stato modificato il contenuto del corpo e l'hash non è più valido. |
| `Content-Type`     | Specifica il tipo MIME del contenuto. Ogni risposta deve specificare relativo `Content-Type`. Il middleware controlla questo valore per determinare se la risposta deve essere compresso. Il middleware specifica un set di [predefinito di tipi MIME](#mime-types) che può codificare, ma è possibile sostituire o aggiungere tipi MIME. |
| `Vary`             | Quando inviato dal server con il valore `Accept-Encoding` ai client e i proxy, il `Vary` intestazione indica al client o al proxy di memorizzazione nella cache (variare) delle risposte in base al valore del `Accept-Encoding` intestazione della richiesta. Il risultato della restituzione del contenuto con il `Vary: Accept-Encoding` intestazione è che entrambi compressi e decompressi le risposte vengono memorizzate nella cache separatamente. |

Scopri le funzionalità del Middleware di compressione delle risposte con il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). L'esempio illustra:

* La compressione delle risposte di app con Gzip e provider di compressione personalizzato.
* Come aggiungere un tipo MIME per l'elenco predefinito di tipi MIME per la compressione.

## <a name="package"></a>Pacchetto

::: moniker range=">= aspnetcore-2.1"

Per includere il middleware in un progetto, aggiungere un riferimento per la [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app), che include il [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacchetto.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Per includere il middleware in un progetto, aggiungere un riferimento per la [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage), che include il [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacchetto.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Per includere il middleware in un progetto, aggiungere un riferimento per la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacchetto.

::: moniker-end

## <a name="configuration"></a>Configurazione

::: moniker range=">= aspnetcore-2.2"

Il codice seguente viene illustrato come abilitare il Middleware di compressione di risposta per la compressione dei provider e tipi MIME predefiniti ([Brotli](#brotli-compression-provider) e [Gzip](#gzip-compression-provider)):

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Il codice seguente viene illustrato come abilitare il Middleware di compressione di risposta per i tipi MIME predefiniti e il [Provider di compressione Gzip](#gzip-compression-provider):

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

> [!NOTE]
> Usare uno strumento come [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) impostare il `Accept-Encoding` intestazione della richiesta e studiare le intestazioni di risposta, dimensioni e corpo.

Inviare una richiesta all'app di esempio senza il `Accept-Encoding` intestazione e osservare che la risposta non è compressa. Il `Content-Encoding` e `Vary` intestazioni non sono presenti nella risposta.

![Finestra di Fiddler che mostra il risultato di una richiesta senza l'intestazione Accept-Encoding. La risposta non viene compressa.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

Inviare una richiesta all'app di esempio con il `Accept-Encoding: br` intestazione (compressione Brotli) e osservare che la risposta è compresso. Il `Content-Encoding` e `Vary` le intestazioni sono presenti nella risposta.

![Finestra di Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore br. Le intestazioni possono variare e Content-Encoding vengono aggiunte alla risposta. La risposta è compresso.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Inviare una richiesta all'app di esempio con il `Accept-Encoding: gzip` intestazione e osservare che la risposta è compresso. Il `Content-Encoding` e `Vary` le intestazioni sono presenti nella risposta.

![Finestra di Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore gzip. Le intestazioni possono variare e Content-Encoding vengono aggiunte alla risposta. La risposta è compresso.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a>Provider

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a>Provider di compressione Brotli

Usare la `BrotliCompressionProvider` per comprimere le risposte con i [formato di dati compressi Brotli](https://tools.ietf.org/html/rfc7932).

Se non vengono aggiunti in modo esplicito alcun provider di compressione di <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Il Provider di compressione Brotli viene aggiunto per impostazione predefinita nella matrice di provider di compressione con il [provider di compressione Gzip](#gzip-compression-provider).
* La compressione per impostazione predefinita la compressione Brotli quando il formato di dati compressi Brotli è supportato dal client. Se Brotli non è supportata dal client, impostazione predefinita la compressione su Gzip quando il client supporta la compressione Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Il Provider di compressione Brotoli deve essere aggiunto quando vengono aggiunti esplicitamente tutti i provider di compressione:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

Impostare il livello con la compressione `BrotliCompressionProviderOptions`. Il Provider di compressione Brotli per impostazione predefinita il livello di compressione più veloce ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente. Se si desidera usare la compressione più efficiente, configurare il middleware di compressione non ottimale.

| Livello di compressione | Descrizione |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | La compressione deve completare più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Non deve essere eseguita alcuna compressione. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Le risposte devono essere compressi in modo ottimale, anche se la compressione richiede più tempo per il completamento. |

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

Usare la <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> per comprimere le risposte con i [formato del file Gzip](https://tools.ietf.org/html/rfc1952).

Se non vengono aggiunti in modo esplicito alcun provider di compressione di <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

::: moniker range=">= aspnetcore-2.2"

* Il Provider di compressione Gzip viene aggiunto per impostazione predefinita per la matrice di provider di compressione con il [Provider di compressione Brotli](#brotli-compression-provider).
* La compressione per impostazione predefinita la compressione Brotli quando il formato di dati compressi Brotli è supportato dal client. Se Brotli non è supportata dal client, impostazione predefinita la compressione su Gzip quando il client supporta la compressione Gzip.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Il Provider di compressione Gzip viene aggiunto per impostazione predefinita nella matrice di provider di compressione.
* Quando il client supporta la compressione Gzip per impostazione predefinita la compressione su Gzip.

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Il Provider di compressione Gzip deve essere aggiunto quando vengono aggiunti esplicitamente tutti i provider di compressione:

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

Impostare il livello con la compressione <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. Il Provider di compressione Gzip per impostazione predefinita il livello di compressione più veloce ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), che potrebbe non produrre la compressione più efficiente. Se si desidera usare la compressione più efficiente, configurare il middleware di compressione non ottimale.

| Livello di compressione | Descrizione |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | La compressione deve completare più rapidamente possibile, anche se l'output risultante non viene compresso in modo ottimale. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Non deve essere eseguita alcuna compressione. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Le risposte devono essere compressi in modo ottimale, anche se la compressione richiede più tempo per il completamento. |

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

Creare le implementazioni di compressione personalizzato con <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. Il <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> rappresenta il contenuto da questo codifica `ICompressionProvider` produce. Il middleware utilizza queste informazioni per scegliere il provider in base all'elenco specificato nel `Accept-Encoding` intestazione della richiesta.

Usa l'app di esempio, il client invia una richiesta con il `Accept-Encoding: mycustomcompression` intestazione. Il middleware utilizza l'implementazione di compressione personalizzato e restituisce la risposta con un `Content-Encoding: mycustomcompression` intestazione. Il client deve essere in grado di decomprimere la codifica personalizzata affinché un'implementazione di compressione personalizzato lavorare.

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

Inviare una richiesta all'app di esempio con il `Accept-Encoding: mycustomcompression` intestazione e osservare le intestazioni della risposta. Il `Vary` e `Content-Encoding` le intestazioni sono presenti nella risposta. Il corpo della risposta (non mostrato) non viene compressa dall'esempio. Non c'è un'implementazione della compressione nel `CustomCompressionProvider` classe dell'esempio. Tuttavia, l'esempio mostra in cui è necessario implementare un algoritmo di compressione.

![Finestra di Fiddler che mostra il risultato di una richiesta con l'intestazione Accept-Encoding e il valore mycustomcompression. Le intestazioni possono variare e Content-Encoding vengono aggiunte alla risposta.](response-compression/_static/request-custom-compression.png)

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

Sostituire o appendere gli tipi MIME con le opzioni di Middleware di compressione delle risposte. Si noti che con caratteri jolly MIME tipi, ad esempio `text/*` non sono supportati. L'app di esempio aggiunge un tipo MIME per `image/svg+xml` comprime e viene usato ASP.NET Core immagine del banner (*banner.svg*).

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a>Compressione con protocollo di protezione

Le risposte compresse su connessioni protette possono essere controllate con la `EnableForHttps` opzione, che è disabilitata per impostazione predefinita. Con la compressione delle pagine generate in modo dinamico può causare problemi di sicurezza, ad esempio la [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violazione](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacchi.

## <a name="adding-the-vary-header"></a>Aggiunta l'intestazione Vary

::: moniker range=">= aspnetcore-2.0"

Quando la compressione delle risposte in base il `Accept-Encoding` intestazione, esistono potenzialmente più versioni compresse della risposta e una versione non compressa. Per indicare a cache del client e del proxy che più versioni esistano e devono essere archiviate, il `Vary` intestazione viene aggiunta con un `Accept-Encoding` valore. In ASP.NET Core 2.0 o versioni successive, il middleware aggiunge il `Vary` intestazione automaticamente quando la risposta viene compresso.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Quando la compressione delle risposte in base il `Accept-Encoding` intestazione, esistono potenzialmente più versioni compresse della risposta e una versione non compressa. Per indicare a cache del client e del proxy che più versioni esistano e devono essere archiviate, il `Vary` intestazione viene aggiunta con un `Accept-Encoding` valore. In ASP.NET Core 1.x, aggiungendo il `Vary` intestazione alla risposta viene eseguita manualmente:

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Problema di middleware quando dietro un proxy inverso di Nginx

Quando una richiesta viene trasmessa tramite proxy da Nginx, il `Accept-Encoding` intestazione viene rimosso. Ciò impedisce che il middleware di compressione della risposta. Per altre informazioni, vedere [NGINX: compressione e decompressione](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Questo problema viene rilevato da [capire la compressione di tipo pass-through per Nginx (BasicMiddleware 123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Utilizzo con la compressione dinamica di IIS

Se si dispone di un active modulo di compressione dinamica IIS configurata a livello di server che si vuole disabilitare per un'app, disabilitare il modulo con un componente aggiuntivo per il *Web. config* file. Per altre informazioni, vedere [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Disabilitazione di moduli IIS).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Usare uno strumento come [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), o [Postman](https://www.getpostman.com/), che consentono di impostare il `Accept-Encoding` intestazione della richiesta e studiare le intestazioni di risposta, dimensioni e corpo. Per impostazione predefinita, Middleware di compressione delle risposte comprime le risposte che soddisfano le condizioni seguenti:

::: moniker range=">= aspnetcore-2.2"

* Il `Accept-Encoding` è presente con un valore di intestazione `br`, `gzip`, `*`, o codifica personalizzata che corrisponde a un provider di compressione personalizzato che è stata stabilita. Il valore non deve essere `identity` o avere un valore di qualità (qvalue, `q`) impostazione pari a 0 (zero).
* Il tipo MIME (`Content-Type`) deve essere impostata e deve corrispondere a un tipo MIME, configurato nel <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* La richiesta non deve includere il `Content-Range` intestazione.
* La richiesta deve utilizzare il protocollo non sicuro (http), a meno che un protocollo sicuro (https) viene configurato nelle opzioni del Middleware di compressione delle risposte. *Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si abilita la compressione di contenuto protetta.*

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Il `Accept-Encoding` è presente con un valore di intestazione `gzip`, `*`, o codifica personalizzata che corrisponde a un provider di compressione personalizzato che è stata stabilita. Il valore non deve essere `identity` o avere un valore di qualità (qvalue, `q`) impostazione pari a 0 (zero).
* Il tipo MIME (`Content-Type`) deve essere impostata e deve corrispondere a un tipo MIME, configurato nel <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* La richiesta non deve includere il `Content-Range` intestazione.
* La richiesta deve utilizzare il protocollo non sicuro (http), a meno che un protocollo sicuro (https) viene configurato nelle opzioni del Middleware di compressione delle risposte. *Si noti il pericolo [descritto in precedenza](#compression-with-secure-protocol) quando si abilita la compressione di contenuto protetta.*

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Mozilla Developer Network: Codifica](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 sezione 3.1.2.1: Chiaramente tutti i codici contenuti](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 sezione 4.2.3: Codifica Gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Versione specifica del formato file GZIP 4.3](http://www.ietf.org/rfc/rfc1952.txt)
