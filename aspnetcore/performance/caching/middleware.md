---
title: Middleware di ASP.NET Core della memorizzazione nella cache
author: guardrex
description: Informazioni su come configurare e usare il middleware di memorizzazione nella cache delle risposte in ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2019
uid: performance/caching/middleware
ms.openlocfilehash: c7c3dbd0c9cf029fa6921d77450e780768c8aa6e
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647915"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Middleware di ASP.NET Core della memorizzazione nella cache

Dal [Luke Latham](https://github.com/guardrex) e [John Luo](https://github.com/JunTaoLuo)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).

Questo articolo illustra come configurare Middleware di memorizzazione nella cache delle risposte in un'app ASP.NET Core. Il middleware determina quando le risposte sono inseribili nella cache, archivi di risposte e funge da risposta dalla cache. Per un'introduzione alla memorizzazione nella cache HTTP e il `ResponseCache` dell'attributo, vedere [risposte nella cache](xref:performance/caching/response).

## <a name="package"></a>Pacchetto

::: moniker range=">= aspnetcore-2.1"

Riferimento di [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto le [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pacchetto.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Riferimento di [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage) oppure aggiungere un riferimento al pacchetto le [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pacchetto.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Aggiungere un riferimento al pacchetto di [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pacchetto.

::: moniker-end

## <a name="configuration"></a>Configurazione

In `Startup.ConfigureServices`, aggiungere il middleware per la raccolta di servizio.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=9)]

Configurare l'app per usare il middleware con il `UseResponseCaching` metodo di estensione, che aggiunge il middleware alla pipeline di elaborazione della richiesta. L'app di esempio aggiunge un [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) intestazione nella risposta che memorizza nella cache le risposte inseribili nella cache fino a 10 secondi. L'esempio invia un [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) intestazione per configurare il middleware per servire una risposta memorizzata nella cache solo se il [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) intestazione delle richieste successive corrisponda a quello della richiesta originale. Nell'esempio di codice seguente [CacheControlHeaderValue](/dotnet/api/microsoft.net.http.headers.cachecontrolheadervalue) e [HeaderNames](/dotnet/api/microsoft.net.http.headers.headernames) richiedono una `using` istruzione per i [Microsoft.Net.Http.Headers](/dotnet/api/microsoft.net.http.headers) spazio dei nomi.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=17,22-29)]

Middleware di memorizzazione nella cache delle risposte nella cache solo risposte del server che generano un codice di stato 200 (OK). Altre risposte, compresi [pagine di errore](xref:fundamentals/error-handling), vengono ignorati dal middleware.

> [!WARNING]
> Le risposte che contiene il contenuto ai client autenticati devono essere contrassegnate come non memorizzabile nella cache per evitare che il middleware da archiviare e gestire tali le risposte. Visualizzare [condizioni per la memorizzazione nella cache](#conditions-for-caching) per informazioni dettagliate sul modo in cui il middleware determina se una risposta è memorizzabile nella cache.

## <a name="options"></a>Opzioni

Il middleware offre tre opzioni per la memorizzazione nella cache di controllo risposta.

| Opzione                | Descrizione |
| --------------------- | ----------- |
| UseCaseSensitivePaths | Determina se le risposte vengono memorizzate nella cache nei percorsi tra maiuscole e minuscole. Il valore predefinito è `false`. |
| MaximumBodySize       | La dimensione inseribili nella cache più grande per il corpo della risposta in byte. Il valore predefinito è `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | Il limite di dimensioni per il middleware di cache di risposta in byte. Il valore predefinito è `100 * 1024 * 1024` (100 MB). |

L'esempio seguente configura il middleware di:

* Memorizzare nella cache le risposte minore o uguale a 1024 byte.
* Store le risposte dai percorsi distinzione maiuscole/minuscole (ad esempio, `/page1` e `/Page1` vengono archiviati separatamente).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Quando si usano controller MVC o Web API o modelli di pagina Razor Pages, il `ResponseCache` attributo specifica i parametri necessari per impostare le intestazioni appropriate per la memorizzazione nella cache di risposta. L'unico parametro del `ResponseCache` attributo, che richiede esclusivamente il middleware `VaryByQueryKeys`, che non corrisponde a un'intestazione HTTP effettiva. Per altre informazioni, vedere [attributo ResponseCache](xref:performance/caching/response#responsecache-attribute).

Se non si usa la `ResponseCache` attributo, la memorizzazione nella cache di risposta possono essere modificati con la `VaryByQueryKeys` funzionalità. Usare la `ResponseCachingFeature` direttamente dai `IFeatureCollection` del `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Usando un singolo valore uguale a `*` in `VaryByQueryKeys` varia a seconda della cache tutti i parametri di query di richiesta.

## <a name="http-headers-used-by-response-caching-middleware"></a>Intestazioni HTTP usate dal Middleware di memorizzazione nella cache delle risposte

La memorizzazione nella cache dal middleware di risposta viene configurato usando le intestazioni HTTP.

| Intestazione | Dettagli |
| ------ | ------- |
| Autorizzazione | Se l'intestazione esiste, non è memorizzato nella cache la risposta. |
| Cache-Control | Il middleware prende in considerazione esclusivamente la memorizzazione nella cache le risposte contrassegnate con il `public` direttiva della cache. Controllare la memorizzazione nella cache con i parametri seguenti:<ul><li>max-age</li><li>max-stale&#8224;</li><li>nuova sessione di Min</li><li>must-revalidate</li><li>no-cache</li><li>no-store</li><li>only-if-cached</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224;Se non viene specificato alcun limite per `max-stale`, il middleware viene eseguita alcuna azione.<br>&#8225;`proxy-revalidate`ha lo stesso effetto `must-revalidate`.<br><br>Per altre informazioni, vedere [RFC 7231: Richiesta delle direttive Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragma | Oggetto `Pragma: no-cache` intestazione nella richiesta produce lo stesso effetto `Cache-Control: no-cache`. Questa intestazione viene sottoposto a override dalle direttive rilevanti nel `Cache-Control` intestazione, se presente. Considerato per garantire la compatibilità con HTTP/1.0. |
| Set-Cookie | Se l'intestazione esiste, non è memorizzato nella cache la risposta. Qualsiasi middleware nella pipeline di elaborazione della richiesta che consente di impostare uno o più cookie impedisce il Middleware di memorizzazione nella cache delle risposte di memorizzazione nella cache la risposta (ad esempio, il [provider TempData basato su cookie](xref:fundamentals/app-state#tempdata)).  |
| Variare | Il `Vary` intestazione usata per variare la risposta memorizzata nella cache da un'altra intestazione. Ad esempio, risposte memorizzate nella cache dalla codifica includendo il `Vary: Accept-Encoding` intestazione, che memorizza nella cache le risposte per le richieste con le intestazioni `Accept-Encoding: gzip` e `Accept-Encoding: text/plain` separatamente. Una risposta con un valore di intestazione di `*` non viene mai archiviato. |
| Alla scadenza | Una risposta considerata non aggiornata da questa intestazione non viene archiviata o recuperata a meno che non viene sottoposto a override da altri `Cache-Control` intestazioni. |
| If-None-Match | La risposta completa viene servita dalla cache se non è il valore `*` e il `ETag` della risposta non corrisponde ad alcuno dei valori forniti. In caso contrario, viene fornita una risposta 304 (non modificato). |
| If-Modified-Since | Se il `If-None-Match` intestazione non è presente, un'intera risposta viene servita dalla cache se la data risposta memorizzata nella cache è più recente rispetto al valore fornito. In caso contrario, viene fornita una risposta 304 (non modificato). |
| Data | Quando utilizzata dalla cache, il `Date` intestazione viene impostata dal middleware se non è stato fornito la risposta originali. |
| Content-Length | Quando utilizzata dalla cache, il `Content-Length` intestazione viene impostata dal middleware se non è stato fornito la risposta originali. |
| Età | Il `Age` intestazione inviata nella risposta originale viene ignorato. Il middleware calcola un nuovo valore durante l'utilizzo di una risposta memorizzata nella cache. |

## <a name="caching-respects-request-cache-control-directives"></a>La memorizzazione nella cache rispetta le direttive Cache-Control richiesta

Il middleware rispetta le regole del [specifica la memorizzazione nella cache di HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2). Le regole richiedono una cache a rispettare un valido `Cache-Control` intestazione inviato dal client. In specifica di un client può eseguire richieste con un `no-cache` valore dell'intestazione e forza il server a generare una nuova risposta per ogni richiesta. Attualmente, non è presente alcun controllo per gli sviluppatori su questo comportamento di memorizzazione nella cache quando si usa il middleware in quanto il middleware è conforme alla specifica di memorizzazione nella cache ufficiale.

Per un maggiore controllo sul comportamento di memorizzazione nella cache, esplorare altre funzionalità di memorizzazione nella cache di ASP.NET Core. Vedere gli argomenti seguenti:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se il comportamento di memorizzazione nella cache non è come previsto, verificare che le risposte sono inseribili nella cache e in grado di essere servite dalla cache. Esaminare le intestazioni in entrata della richiesta e della risposta in uscita. Abilitare [logging](xref:fundamentals/logging/index) per facilitare il debug.

Durante il test e risoluzione dei problemi di comportamento di memorizzazione nella cache, un browser può impostare le intestazioni di richiesta che influiscono sulla memorizzazione nella cache in modi indesiderati. Ad esempio, è possibile impostare un browser il `Cache-Control` intestazione `no-cache` o `max-age=0` durante l'aggiornamento di una pagina. Gli strumenti seguenti possono impostare in modo esplicito le intestazioni di richiesta e sono preferibili per testare la memorizzazione nella cache:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Condizioni per la memorizzazione nella cache

* La richiesta deve restituire una risposta del server con un codice di stato 200 (OK).
* Il metodo di richiesta deve essere GET o HEAD.
* In `Startup.Configure`, Middleware di memorizzazione nella cache delle risposte deve essere posizionato prima del middleware che richiedono la compressione. Per altre informazioni, vedere <xref:fundamentals/middleware/index>.
* Il `Authorization` intestazione non deve essere presente.
* `Cache-Control` parametri dell'intestazione devono essere validi e la risposta deve essere contrassegnata `public` e non contrassegnati come `private`.
* Il `Pragma: no-cache` intestazione non deve essere presente se la `Cache-Control` intestazione non è presente, come il `Cache-Control` esegue l'override dell'intestazione di `Pragma` intestazione quando è presente.
* Il `Set-Cookie` intestazione non deve essere presente.
* `Vary` i parametri di intestazione devono essere validi e non è uguale a `*`.
* Il `Content-Length` valore dell'intestazione (se impostata) deve corrispondere alle dimensioni del corpo della risposta.
* Il [IHttpSendFileFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) non viene usato.
* La risposta non deve essere non aggiornata in base al `Expires` intestazione e il `max-age` e `s-maxage` memorizzare nella cache delle direttive.
* Il buffer risposte deve essere completate correttamente e le dimensioni della risposta devono essere minore dell'applicazione configurata o default `SizeLimit`.
* La risposta deve essere inseribili nella cache in base al [RFC 7234](https://tools.ietf.org/html/rfc7234) specifiche. Ad esempio, il `no-store` direttiva non deve essere presente nei campi di intestazione di richiesta o risposta. Vedere *sezione 3: Memorizzazione delle risposte nella cache* dei [RFC 7234](https://tools.ietf.org/html/rfc7234) per informazioni dettagliate.

> [!NOTE]
> Il sistema antifalsificazione per generare i token di protezione per evitare Cross-Site Request Forgery (CSRF) attacks set il `Cache-Control` e `Pragma` intestazioni `no-cache` in modo che non vengono memorizzate nella cache le risposte. Per informazioni su come disabilitare i token antifalsificazione per elementi del form HTML, vedere [configurazione di ASP.NET Core anti falsificazione](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
