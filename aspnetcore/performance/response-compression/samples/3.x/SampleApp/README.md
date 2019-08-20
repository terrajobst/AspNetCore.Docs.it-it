# <a name="response-compression-sample-application-aspnet-core-3x"></a>Applicazione di esempio di compressione delle risposte (ASP.NET Core 3. x)

Questo esempio illustra l'uso del middleware di compressione della risposta ASP.NET Core 3. x per comprimere le risposte HTTP. Nell'esempio vengono illustrati i provider di compressione gzip, Brotli e personalizzati per le risposte di tipo text e image e viene illustrato come aggiungere un tipo MIME per la compressione. Per l'esempio ASP.NET Core 2. x, vedere [applicazione di esempio di compressione delle risposte (ASP.NET Core 2. x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Esempi

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -Lorel ipsum risposta al file di testo a 2.044 byte compressi in ~ 979 byte.
    * **/testfile1kb.txt** -risposta file di testo a 1.033 byte che comprime a ~ 36 byte.
    * **/Trickle** -risposta rilasciata come singoli caratteri a intervalli di 1 secondo.
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorel ipsum risposta al file di testo a 2.044 byte compressi in ~ 927 byte.
    * **/testfile1kb.txt** -risposta file di testo a 1.033 byte che comprime a ~ 47 byte.
    * **/Trickle** -risposta rilasciata come singoli caratteri a intervalli di 1 secondo.
  * `image/svg+xml`
    * **/banner.svg** : risposta immagine svg (Scalable Vector Graphics) a 9.707 byte che comprime a ~ 4.459 byte.
* `CustomCompressionProvider`<br>Viene illustrato come implementare un provider di compressione personalizzato per l'utilizzo con il middleware.

Quando la richiesta include l' `Accept-Encoding` intestazione e la compressione della risposta ha esito positivo, il `Vary: Accept-Encoding` middleware aggiunge automaticamente un'intestazione alla risposta. L' `Vary` intestazione indica alle cache di gestire più copie della risposta in base ai valori alternativi di `Accept-Encoding`, quindi sia una versione compressa (gzip o Brotli) che una versione non compressa vengono archiviate nelle cache per i sistemi che possono accettare il risposta compressa o non compressa.

## <a name="use-the-sample"></a>Usare l'esempio

1. Effettuare una richiesta usando [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/) o [Postman](https://www.getpostman.com/) all'applicazione senza un' `Accept-Encoding` intestazione e prendere nota del payload della risposta, della dimensione della risposta e delle intestazioni di risposta.
1. Aggiungere un' `Accept-Encoding: br` intestazione `Accept-Encoding: gzip` o e prendere nota delle dimensioni della risposta compressa e delle intestazioni di risposta. Le dimensioni della risposta vengono rilasciate e l'intestazione della `Content-Encoding` risposta viene inclusa dal middleware, che indica che si è verificata una compressione con gzip o Brotli. Quando si esamina il corpo della risposta per la risposta Loren ipsum o **testfile1kb. txt** , si noterà che il testo è compresso e illeggibile.
1. Aggiungere un' `Accept-Encoding: mycustomcompression` intestazione e prendere nota delle intestazioni della risposta. È un'implementazione vuota che non comprime effettivamente la risposta, ma è possibile creare un wrapper del flusso di compressione personalizzato per `CreateStream()` il metodo. `CustomCompressionProvider`
