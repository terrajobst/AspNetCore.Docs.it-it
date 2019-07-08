---
title: Middleware di ASP.NET Core della memorizzazione nella cache
author: guardrex
description: Informazioni su come configurare e usare il middleware di memorizzazione nella cache delle risposte in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
uid: performance/caching/middleware
ms.openlocfilehash: d6756ce16396133da643cc08ca0f48369479ad3a
ms.sourcegitcommit: b9e914ef274b5ec359582f299724af6234dce135
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2019
ms.locfileid: "67596146"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Middleware di ASP.NET Core della memorizzazione nella cache

Dal [Luke Latham](https://github.com/guardrex) e [John Luo](https://github.com/JunTaoLuo)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

Questo articolo illustra come configurare Middleware di memorizzazione nella cache delle risposte in un'app ASP.NET Core. Il middleware determina quando le risposte sono inseribili nella cache, archivi di risposte e funge da risposta dalla cache. Per un'introduzione alla memorizzazione nella cache HTTP e il [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) dell'attributo, vedere [risposte nella cache](xref:performance/caching/response).

## <a name="configuration"></a>Configurazione

Usare la [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o aggiungere un riferimento al pacchetto le [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pacchetto.

In `Startup.ConfigureServices`, aggiungere il Middleware di memorizzazione nella cache delle risposte alla raccolta di servizio:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Configurare l'app per usare il middleware con il <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> metodo di estensione, che aggiunge il middleware alla pipeline di elaborazione delle richieste in `Startup.Configure`:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

L'app di esempio aggiunge le intestazioni per controllare la memorizzazione nella cache nelle richieste successive:

* [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; memorizza nella cache le risposte inseribili nella cache fino a 10 secondi.
* [Variare](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; consente di configurare il middleware per servire una risposta memorizzata nella cache solo se il [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) intestazione delle richieste successive corrisponda a quello della richiesta originale.

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

Middleware di memorizzazione nella cache delle risposte nella cache solo risposte del server che generano un codice di stato 200 (OK). Altre risposte, compresi [pagine di errore](xref:fundamentals/error-handling), vengono ignorati dal middleware.

> [!WARNING]
> Le risposte che contiene il contenuto ai client autenticati devono essere contrassegnate come non memorizzabile nella cache per evitare che il middleware da archiviare e gestire tali le risposte. Visualizzare [condizioni per la memorizzazione nella cache](#conditions-for-caching) per informazioni dettagliate sul modo in cui il middleware determina se una risposta è memorizzabile nella cache.

## <a name="options"></a>Opzioni

Nella tabella seguente vengono visualizzate le opzioni di memorizzazione nella cache di risposta.

| Opzione | Descrizione |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | La dimensione inseribili nella cache più grande per il corpo della risposta in byte. Il valore predefinito è `64 * 1024 * 1024` (64 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Il limite di dimensioni per il middleware di cache di risposta in byte. Il valore predefinito è `100 * 1024 * 1024` (100 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Determina se le risposte vengono memorizzate nella cache nei percorsi tra maiuscole e minuscole. Il valore predefinito è `false`. |

L'esempio seguente configura il middleware di:

* Risposte nella cache con dimensioni corpo minore o uguale a 1024 byte.
* Store le risposte dai percorsi tra maiuscole e minuscole. Ad esempio, `/page1` e `/Page1` vengono archiviati separatamente.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Quando si usa MVC / web controller dell'API o modelli di pagina Razor Pages, il [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attributo specifica i parametri necessari per impostare le intestazioni appropriate per la memorizzazione nella cache di risposta. L'unico parametro del `[ResponseCache]` attributo, che richiede esclusivamente il middleware <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, che non corrisponde a un'intestazione HTTP effettiva. Per altre informazioni, vedere <xref:performance/caching/response#responsecache-attribute>.

Se non si usa la `[ResponseCache]` attributo, la memorizzazione nella cache di risposta può essere variata con `VaryByQueryKeys`. Usare la <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> direttamente dai [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Usando un singolo valore uguale a `*` in `VaryByQueryKeys` varia a seconda della cache tutti i parametri di query di richiesta.

## <a name="http-headers-used-by-response-caching-middleware"></a>Intestazioni HTTP usate dal Middleware di memorizzazione nella cache delle risposte

Nella tabella seguente vengono fornite informazioni sulle intestazioni HTTP che influiscono sulla memorizzazione nella cache di risposta.

| Intestazione | Dettagli |
| ------ | ------- |
| `Authorization` | Se l'intestazione esiste, non è memorizzato nella cache la risposta. |
| `Cache-Control` | Il middleware prende in considerazione esclusivamente la memorizzazione nella cache le risposte contrassegnate con il `public` direttiva della cache. Controllare la memorizzazione nella cache con i parametri seguenti:<ul><li>max-age</li><li>max-stale&#8224;</li><li>nuova sessione di Min</li><li>must-revalidate</li><li>no-cache</li><li>no-store</li><li>only-if-cached</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224;Se non viene specificato alcun limite per `max-stale`, il middleware viene eseguita alcuna azione.<br>&#8225;`proxy-revalidate`ha lo stesso effetto `must-revalidate`.<br><br>Per altre informazioni, vedere [RFC 7231: Richiesta delle direttive Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | Oggetto `Pragma: no-cache` intestazione nella richiesta produce lo stesso effetto `Cache-Control: no-cache`. Questa intestazione viene sottoposto a override dalle direttive rilevanti nel `Cache-Control` intestazione, se presente. Considerato per garantire la compatibilità con HTTP/1.0. |
| `Set-Cookie` | Se l'intestazione esiste, non è memorizzato nella cache la risposta. Qualsiasi middleware nella pipeline di elaborazione della richiesta che consente di impostare uno o più cookie impedisce il Middleware di memorizzazione nella cache delle risposte di memorizzazione nella cache la risposta (ad esempio, il [provider TempData basato su cookie](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | Il `Vary` intestazione usata per variare la risposta memorizzata nella cache da un'altra intestazione. Ad esempio, risposte memorizzate nella cache dalla codifica includendo il `Vary: Accept-Encoding` intestazione, che memorizza nella cache le risposte per le richieste con le intestazioni `Accept-Encoding: gzip` e `Accept-Encoding: text/plain` separatamente. Una risposta con un valore di intestazione di `*` non viene mai archiviato. |
| `Expires` | Una risposta considerata non aggiornata da questa intestazione non viene archiviata o recuperata a meno che non viene sottoposto a override da altri `Cache-Control` intestazioni. |
| `If-None-Match` | La risposta completa viene servita dalla cache se non è il valore `*` e il `ETag` della risposta non corrisponde ad alcuno dei valori forniti. In caso contrario, viene fornita una risposta 304 (non modificato). |
| `If-Modified-Since` | Se il `If-None-Match` intestazione non è presente, un'intera risposta viene servita dalla cache se la data risposta memorizzata nella cache è più recente rispetto al valore fornito. In caso contrario, un *304 - non modificato* risposta viene servita. |
| `Date` | Quando utilizzata dalla cache, il `Date` intestazione viene impostata dal middleware se non è stato fornito la risposta originali. |
| `Content-Length` | Quando utilizzata dalla cache, il `Content-Length` intestazione viene impostata dal middleware se non è stato fornito la risposta originali. |
| `Age` | Il `Age` intestazione inviata nella risposta originale viene ignorato. Il middleware calcola un nuovo valore durante l'utilizzo di una risposta memorizzata nella cache. |

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
* In `Startup.Configure`, Middleware di memorizzazione nella cache delle risposte deve essere posizionato prima del middleware che richiedono la memorizzazione nella cache. Per altre informazioni, vedere <xref:fundamentals/middleware/index>.
* Il `Authorization` intestazione non deve essere presente.
* `Cache-Control` parametri dell'intestazione devono essere validi e la risposta deve essere contrassegnata `public` e non contrassegnati come `private`.
* Il `Pragma: no-cache` intestazione non deve essere presente se la `Cache-Control` intestazione non è presente, come il `Cache-Control` esegue l'override dell'intestazione di `Pragma` intestazione quando è presente.
* Il `Set-Cookie` intestazione non deve essere presente.
* `Vary` i parametri di intestazione devono essere validi e non è uguale a `*`.
* Il `Content-Length` valore dell'intestazione (se impostata) deve corrispondere alle dimensioni del corpo della risposta.
* Il <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> non viene usato.
* La risposta non deve essere non aggiornata in base al `Expires` intestazione e il `max-age` e `s-maxage` memorizzare nella cache delle direttive.
* Il buffer risposte deve essere completate correttamente. Le dimensioni della risposta devono essere minore dell'applicazione configurata o default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>. Le dimensioni del corpo della risposta devono essere minore dell'applicazione configurata o default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.
* La risposta deve essere inseribili nella cache in base al [RFC 7234](https://tools.ietf.org/html/rfc7234) specifiche. Ad esempio, il `no-store` direttiva non deve essere presente nei campi di intestazione di richiesta o risposta. Vedere *sezione 3: Memorizzazione delle risposte nella cache* dei [RFC 7234](https://tools.ietf.org/html/rfc7234) per informazioni dettagliate.

> [!NOTE]
> Il sistema antifalsificazione per generare i token di protezione per evitare Cross-Site Request Forgery (CSRF) attacks set il `Cache-Control` e `Pragma` intestazioni `no-cache` in modo che non vengono memorizzate nella cache le risposte. Per informazioni su come disabilitare i token antifalsificazione per elementi del form HTML, vedere <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
