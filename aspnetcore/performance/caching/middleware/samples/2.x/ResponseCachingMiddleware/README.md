# <a name="aspnet-core-response-caching-sample"></a>Esempio di memorizzazione nella cache di ASP.NET Core risposta

In questo esempio viene illustrato l'utilizzo di ASP.NET Core [Middleware la memorizzazione nella cache risposta](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

L'applicazione risponde con la pagina di indice, incluso un `Cache-Control` intestazione per configurare il comportamento di memorizzazione nella cache. L'applicazione imposta inoltre il `Vary` intestazione per configurare la cache per fornire una risposta solo se il `Accept-Encoding` intestazione delle richieste successive corrisponda a quello della richiesta originale.

Quando si esegue l'esempio, la pagina di indice viene servita dalla cache quando memorizzati e memorizzati nella cache per un massimo di 10 secondi.
