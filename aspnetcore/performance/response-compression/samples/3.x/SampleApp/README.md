# <a name="response-compression-sample-application-aspnet-core-3x"></a>Applicazione di esempio di compressione delle risposte (ASP.NET Core 3. x)

Questo esempio illustra l'uso del middleware di compressione della risposta ASP.NET Core 3. x per comprimere le risposte HTTP. Nell'esempio vengono illustrati i provider di compressione gzip, Brotli e personalizzati per le risposte di tipo text e image e viene illustrato come aggiungere un tipo MIME per la compressione. Per l'esempio ASP.NET Core 2. x, vedere [applicazione di esempio di compressione delle risposte (ASP.NET Core 2. x)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Esempi

* `BrotliCompressionProvider`
  * `text/plain`
    * il file di testo **/** -lorei Ipsum a 2.044 byte compressi in ~ 979 byte.
    * **/testfile1kb.txt** -risposta file di testo a 1.033 byte che comprime a ~ 36 byte.
    * **/Trickle** -risposta rilasciata come singoli caratteri a intervalli di 1 secondo.
* `GzipCompressionProvider`
  * `text/plain`
    * il file di testo **/** -lorei Ipsum a 2.044 byte compressi in ~ 927 byte.
    * **/testfile1kb.txt** -risposta file di testo a 1.033 byte che comprime a ~ 47 byte.
    * **/Trickle** -risposta rilasciata come singoli caratteri a intervalli di 1 secondo.
  * `image/svg+xml`
    * **/banner.svg** : risposta immagine svg (Scalable Vector Graphics) a 9.707 byte che comprime a ~ 4.459 byte.
* `CustomCompressionProvider`<br>Viene illustrato come implementare un provider di compressione personalizzato per l'utilizzo con il middleware.

Quando la richiesta include l'intestazione `Accept-Encoding` e la compressione della risposta ha esito positivo, il middleware aggiunge automaticamente un'intestazione `Vary: Accept-Encoding` alla risposta. L'intestazione `Vary` indica alle cache di gestire più copie della risposta in base a valori alternativi di `Accept-Encoding`, in modo che sia una versione compressa (gzip o Brotli) che una versione non compressa vengano archiviate nelle cache per i sistemi che possono accettare la risposta compressa o non compressa.

## <a name="use-the-sample"></a>Usare l'esempio

1. Effettuare una richiesta usando [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/) [o](https://www.getpostman.com/) subspettatore all'applicazione senza un'intestazione `Accept-Encoding` e prendere nota del payload della risposta, della dimensione della risposta e delle intestazioni di risposta.
1. Aggiungere un'intestazione `Accept-Encoding: br` o `Accept-Encoding: gzip` e prendere nota delle dimensioni della risposta compressa e delle intestazioni di risposta. Le dimensioni della risposta diminuiscono e l'intestazione della risposta `Content-Encoding` è inclusa dal middleware che indica che è stata eseguita la compressione con gzip o Brotli. Quando si esamina il corpo della risposta per la risposta Loren ipsum o **testfile1kb. txt** , si noterà che il testo è compresso e illeggibile.
1. Aggiungere un'intestazione `Accept-Encoding: mycustomcompression` e prendere nota delle intestazioni della risposta. Il `CustomCompressionProvider` è un'implementazione vuota che non comprime effettivamente la risposta, ma è possibile creare un wrapper del flusso di compressione personalizzato per il metodo di `CreateStream()`.
