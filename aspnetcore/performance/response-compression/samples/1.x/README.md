# <a name="response-compression-sample-application-aspnet-core-1x"></a>Applicazione di esempio per la compressione di risposta (ASP.NET Core 1. x)

In questo esempio viene illustrato l'utilizzo di ASP.NET Core 1. x risposta compressione Middleware per comprimere le risposte HTTP per. Nell'esempio viene illustrato Gzip e provider di compressione personalizzata per le risposte del testo e immagine e viene illustrato come aggiungere un tipo MIME per la compressione. Per il codice di esempio ASP.NET Core 2. x, vedere [applicazione di esempio per la compressione di risposta (ASP.NET Core 2. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Negli esempi contenuti in questo esempio
* `GzipCompressionProvider`
  * `text/plain`
    * **/**-Risposta di file di testo Lorem Ipsum 2,044 byte che verranno compressi a 927 byte
    * **/testfile1kb.txt** -risposta di file di testo 1,033 byte che verrà compressa al 47 byte
    * **/ inserimento** -risposta inviata come singoli caratteri intervalli 1 secondo 
  * `image/svg+xml`
    * **/banner.SVG** -risposta immagine A Scalable Vector Graphics (SVG) 9,707 byte che verranno compressi a 4,459 byte
* `CustomCompressionProvider`<br>Viene illustrato come implementare un provider personalizzato di compressione per l'utilizzo con il middleware

Quando la richiesta include il `Accept-Encoding` intestazione, viene aggiunto un `Vary: Accept-Encoding` intestazione nella risposta. Il `Vary` intestazione indica le cache di mantenere più copie della risposta in base ai valori alternativi di `Accept-Encoding`, pertanto compresso (con estensione gzip) sia una versione non compressa memorizzati nella cache per i sistemi che possono accettano la compresso o risposta non compressa.

## <a name="using-the-sample"></a>Utilizzo dell'esempio
1. Effettuare una richiesta utilizzando [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) all'applicazione senza un `Accept-Encoding` intestazione e notare il payload di risposta, la dimensione della risposta, e intestazioni di risposta.
2. Aggiungere un `Accept-Encoding: gzip` intestazione e annotare la dimensione compressa della risposta e intestazioni di risposta. Vedrai Elimina, la dimensione della risposta e `Content-Encoding: gzip` intestazione della risposta è incluso per l'app di esempio. Quando si esamina il corpo della risposta per Lorem Ipsum o **testfile1kb.txt** risposta, vedere che il testo è compresso e illeggibile.
3. Aggiungere un `Accept-Encoding: mycustomcompression` intestazione e prendere nota delle intestazioni di risposta. Il `CustomCompressionProvider` è un'implementazione vuota che non viene effettivamente comprimere la risposta, ma è possibile creare un wrapper di flusso di compressione personalizzata per il `CreateStream()` metodo.
