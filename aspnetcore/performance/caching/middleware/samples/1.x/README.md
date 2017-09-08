# <a name="aspnet-core-response-caching-sample-aspnet-core-1x"></a>Risposta di ASP.NET Core la memorizzazione nella cache di esempio (ASP.NET Core 1. x)

In questo esempio viene illustrato l'utilizzo di ASP.NET Core [risposta la memorizzazione nella cache Middleware](xref:performance/caching/middleware) con ASP.NET Core 1. x. Per il codice di esempio ASP.NET Core 2. x, vedere [esempio di memorizzazione nella cache di ASP.NET Core risposta (ASP.NET Core 2. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/2.x).

L'applicazione invia un `Hello World!` messaggio e l'ora corrente insieme a un `Cache-Control` intestazione per configurare il comportamento di memorizzazione nella cache. L'applicazione invia anche un `Vary` intestazione per configurare la cache per fornire una risposta solo se il `Accept-Encoding` intestazione delle richieste successive corrisponda a quello della richiesta originale.

Quando si esegue l'esempio, una risposta viene servita dalla cache quando possibile e archiviati per fino a 10 secondi.
