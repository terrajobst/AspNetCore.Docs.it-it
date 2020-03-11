# <a name="aspnet-core-response-caching-sample"></a>Esempio di memorizzazione nella cache della risposta ASP.NET Core

Questo esempio illustra l'uso del middleware di [memorizzazione nella cache della risposta](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)ASP.NET Core.

L'app risponde con la relativa pagina di indice, inclusa un'intestazione `Cache-Control` per configurare il comportamento della memorizzazione nella cache. L'app imposta anche l'intestazione `Vary` per configurare la cache in modo da fornire la risposta solo se l'intestazione `Accept-Encoding` delle richieste successive corrisponde a quella della richiesta originale.

Quando si esegue l'esempio, la pagina di indice viene servita dalla cache quando viene archiviata e memorizzata nella cache per un massimo di 10 secondi.

Per testare il comportamento di memorizzazione nella cache:

* Non usare un browser per testare il comportamento di memorizzazione nella cache. I browser spesso aggiungono un'intestazione di controllo della cache al ricaricamento che impedisce al middleware di servire una pagina memorizzata nella cache. Ad esempio, un `Cache-Control` intestazione con un valore `max-age=0`) potrebbe essere aggiunto dal browser.
* Usare uno strumento per sviluppatori che consente di impostare in modo esplicito le intestazioni della richiesta, ad esempio <a href="https://www.telerik.com/fiddler">Fiddler</a> o <a href="https://www.getpostman.com/">postazione</a>.
