# <a name="response-compression-sample-application-aspnet-core-2x"></a>Applicazione di esempio di risposta compressione (ASP.NET Core 2.x)

In questo esempio viene illustrato l'utilizzo di ASP.NET Core 2.x Middleware di compressione delle risposte per la compressione delle risposte HTTP. L'esempio dimostra Gzip, Brotli e provider di compressione personalizzato per le risposte del testo e immagine e viene illustrato come aggiungere un tipo MIME per la compressione. Per esempio ASP.NET Core 1.x, vedere [applicazione di esempio di risposta compressione (ASP.NET Core 1.x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Esempi

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum testo risposta file in byte 2,044 che comprime a ~ 979 byte.
    * **/testfile1kb.txt** -risposta di file di testo in byte 1,033 che comprime a ~ 36 byte.
    * **/ inserimento** -relativa risposta inviata come singoli caratteri in intervalli di 1 secondo.
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum testo risposta file in byte 2,044 che comprime a ~ 927 byte.
    * **/testfile1kb.txt** -risposta di file di testo in byte 1,033 che comprime a circa 47 byte.
    * **/ inserimento** -relativa risposta inviata come singoli caratteri in intervalli di 1 secondo.
  * `image/svg+xml`
    * **/banner.SVG** -risposta immagine A Scalable Vector Graphics (SVG) a byte 9,707 che comprime a ~ 4,459 byte.
* `CustomCompressionProvider`<br>Viene illustrato come implementare un provider di compressione personalizzato per l'uso con il middleware.

Quando la richiesta include il `Accept-Encoding` compressione intestazione e la risposta ha esito positivo, il middleware aggiunge automaticamente un `Vary: Accept-Encoding` intestazione alla risposta. Il `Vary` intestazione indica le cache per mantenere più copie della risposta in base ai valori alternativi di `Accept-Encoding`, pertanto entrambi un compresso (Gzip o Brotli) versione non compressa vengono memorizzati nella cache per i sistemi che possono accettano il compressi o della risposta non compressa.

## <a name="use-the-sample"></a>Usare il codice di esempio

1. Effettuare una richiesta usando [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), o [Postman](https://www.getpostman.com/) all'applicazione senza un `Accept-Encoding` intestazione e annotare il payload di risposta, le dimensioni di risposta, e intestazioni della risposta.
1. Aggiungere un `Accept-Encoding: br` o `Accept-Encoding: gzip` intestazione e tenere presenti le dimensioni delle risposte compresso e le intestazioni di risposta. Elimina la dimensione della risposta e il `Content-Encoding` intestazione della risposta è incluso per il middleware che indica che la compressione con entrambi Gzip o si è verificato Brotli. Quando si esamina il corpo della risposta per il Lorem Ipsum oppure **testfile1kb.txt** risposta, si noterà che il testo è leggibile e non compressi.
1. Aggiungere un `Accept-Encoding: mycustomcompression` intestazione e annotare le intestazioni della risposta. Il `CustomCompressionProvider` è un'implementazione vuota che in realtà non comprime la risposta, ma è possibile creare un wrapper di flusso di compressione personalizzato per il `CreateStream()` (metodo).
