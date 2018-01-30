---
title: Risposta la memorizzazione nella cache di Middleware di ASP.NET Core
author: guardrex
description: Informazioni su come configurare e utilizzare il Middleware di memorizzazione nella cache risposta in ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/26/2017
ms.topic: article
ms.prod: asp.net-core
uid: performance/caching/middleware
ms.openlocfilehash: a355dda0fdffe1e936c119ffb477c2817f28bd9c
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2018
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Risposta la memorizzazione nella cache di Middleware di ASP.NET Core

Da [Luke Latham](https://github.com/guardrex) e [John Luo](https://github.com/JunTaoLuo)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

In questo articolo viene illustrato come configurare la memorizzazione nella cache Middleware di risposta in un'applicazione ASP.NET Core. Il middleware determina quando le risposte sono memorizzabile nella cache le risposte di archivi e funge da risposta dalla cache. Per un'introduzione alla memorizzazione nella cache di HTTP e `ResponseCache` attributo, vedere [la memorizzazione nella cache di risposta](xref:performance/caching/response).

## <a name="package"></a>Pacchetto

Per includere il middleware in un progetto, aggiungere un riferimento di [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pacchetto oppure utilizzare il [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) pacchetto (Core ASP.NET 2.0 o versione successiva quando la destinazione è .NET Core).

## <a name="configuration"></a>Configurazione

In `ConfigureServices`, aggiungere il middleware per la raccolta di servizio.

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet1&highlight=3)]

Configurare l'applicazione per utilizzare il middleware con il `UseResponseCaching` metodo di estensione, che aggiunge il middleware alla pipeline di elaborazione della richiesta. L'app di esempio aggiunge un [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) intestazione risposta che memorizza nella cache le risposte memorizzabile nella cache per un massimo di 10 secondi. L'esempio invia un [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) intestazione per configurare il middleware per fornire una risposta memorizzata nella cache solo se il [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) intestazione delle richieste successive corrisponda a quello della richiesta originale.

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet2&highlight=3,7-12)]

Risposta Middleware di memorizzazione nella cache vengono memorizzati nella cache solo le risposte del server che produce un codice di stato 200 (OK). Altre risposte, tra cui [pagine di errore](xref:fundamentals/error-handling), vengono ignorate dal middleware.

> [!WARNING]
> Le risposte che includono il contenuto per i clienti autenticati devono essere contrassegnate come non memorizzabile nella cache per impedire il middleware da archiviare e serve le risposte. Vedere [le condizioni per la memorizzazione nella cache](#conditions-for-caching) per informazioni dettagliate su come il middleware determina se una risposta memorizzabile nella cache.

## <a name="options"></a>Opzioni

Il middleware offre tre opzioni per la memorizzazione nella cache di controllo risposta.

| Opzione                | Valore predefinito |
| --------------------- | ------------- |
| UseCaseSensitivePaths | Determina se vengono memorizzati nella cache le risposte nei percorsi tra maiuscole e minuscole.</p><p>Il valore predefinito è `false`. |
| MaximumBodySize       | La dimensione massima memorizzabile nella cache per il corpo della risposta in byte.</p>Il valore predefinito è `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | Il limite per il middleware di cache di risposta in byte. Il valore predefinito è `100 * 1024 * 1024` (100 MB). |

Nell'esempio seguente consente di configurare il middleware di:

* Memorizzare nella cache le risposte minore o uguale a 1024 byte.
* Archiviare le risposte dai percorsi tra maiuscole e minuscole (ad esempio, `/page1` e `/Page1` vengono archiviati separatamente).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Quando si usano controller MVC o Web API o modelli di pagine, pagine Razor il `ResponseCache` attributo specifica i parametri necessari per l'impostazione delle intestazioni appropriate per la memorizzazione nella cache di risposta. L'unico parametro del `ResponseCache` attributo che richiede necessariamente il middleware è `VaryByQueryKeys`, che non corrisponde a un'intestazione HTTP effettiva. Per ulteriori informazioni, vedere [ResponseCache attributo](xref:performance/caching/response#responsecache-attribute).

Se non si utilizza il `ResponseCache` attributo, la memorizzazione nella cache di risposta possono variare con il `VaryByQueryKeys` funzionalità. Utilizzare il `ResponseCachingFeature` direttamente dal `IFeatureCollection` del `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a>Intestazioni HTTP utilizzate dal Middleware di memorizzazione nella cache di risposta

La memorizzazione nella cache dal middleware di risposta viene configurato utilizzando le intestazioni HTTP.

| Header | Dettagli |
| ------ | ------- |
| Autorizzazione | Se l'intestazione esiste, non è memorizzato nella cache la risposta. |
| Cache-Control | Il middleware prende in considerazione esclusivamente la memorizzazione nella cache le risposte contrassegnate con il `public` direttiva della cache. Controllare la memorizzazione nella cache con i parametri seguenti:<ul><li>max-age</li><li>max-stale&#8224;</li><li>Min-nuova</li><li>must-revalidate</li><li>no-cache</li><li>no-store</li><li>solo se-memorizzato nella cache</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224; se non viene specificato alcun limite `max-stale`, il middleware non esegue alcuna azione.<br>&#8225; `proxy-revalidate` ha lo stesso effetto `must-revalidate`.<br><br>Per ulteriori informazioni, vedere [RFC 7231: richiesta direttive Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragma | Oggetto `Pragma: no-cache` intestazione nella richiesta produce lo stesso effetto `Cache-Control: no-cache`. Questa intestazione viene sottoposto a override dalle direttive rilevanti nel `Cache-Control` intestazione, se presente. Considerati per la compatibilità con HTTP/1.0. |
| Set-Cookie | Se l'intestazione esiste, non è memorizzato nella cache la risposta. |
| Variare | Il `Vary` intestazione viene utilizzata per variare la risposta memorizzata nella cache da un'altra intestazione. Ad esempio, nella cache le risposte codificando includendo il `Vary: Accept-Encoding` intestazione, che memorizza nella cache le risposte per le richieste con intestazioni `Accept-Encoding: gzip` e `Accept-Encoding: text/plain` separatamente. Una risposta con un valore di intestazione di `*` non venga mai memorizzata. |
| Scadenza | Una risposta considerata non aggiornata da questa intestazione non è memorizzata o recuperata a meno che non viene sottoposto a override da altri `Cache-Control` intestazioni. |
| If-None-Match | L'intera risposta viene servita dalla cache, se il valore non `*` e `ETag` della risposta non corrisponde ad alcuno dei valori forniti. In caso contrario, viene fornita una risposta 304 (non modificato). |
| If-Modified-Since | Se il `If-None-Match` intestazione non è presente, un'intera risposta viene servita dalla cache se la data di risposta memorizzata nella cache è più recente rispetto al valore fornito. In caso contrario, viene fornita una risposta 304 (non modificato). |
| Data | Quando Gestione dalla cache, il `Date` intestazione viene impostata dal middleware se esso non è stato fornito nella risposta originale. |
| Lunghezza del contenuto | Quando Gestione dalla cache, il `Content-Length` intestazione viene impostata dal middleware se esso non è stato fornito nella risposta originale. |
| Età | Il `Age` intestazione inviata nella risposta originale viene ignorato. Il middleware calcola un nuovo valore per la gestione di una risposta memorizzata nella cache. |

## <a name="caching-respects-request-cache-control-directives"></a>La memorizzazione nella cache rispetta le direttive Cache-Control richiesta

Il middleware rispetta le regole del [specifica la memorizzazione nella cache di HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2). Le regole richiedono una cache per rispettare un valido `Cache-Control` intestazione inviata dal client. In specifica di un client può eseguire richieste con un `no-cache` valore dell'intestazione e forza il server per generare una nuova risposta per ogni richiesta. Non è attualmente alcun controllo sviluppatore su questo comportamento di memorizzazione nella cache quando si utilizza il middleware perché il middleware conforme alla memorizzazione nella cache specifica ufficiale.

Per un maggiore controllo sul comportamento di memorizzazione nella cache, esplorare altre funzionalità di memorizzazione nella cache di ASP.NET Core. Vedere gli argomenti seguenti:

* [Memorizzazione nella cache in memoria](xref:performance/caching/memory)
* [Uso di una cache distribuita](xref:performance/caching/distributed)
* [Helper di Tag in componenti di base di ASP.NET MVC di memorizzare nella cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se il comportamento di memorizzazione nella cache non è come previsto, verificare che le risposte siano memorizzabile nella cache e in grado di servito dalla cache. Esaminare le intestazioni in ingresso della richiesta e della risposta in uscita. Abilitare [registrazione](xref:fundamentals/logging/index) per facilitare il debug.

Durante il test e risoluzione dei problemi di comportamento di memorizzazione nella cache, un browser può impostare le intestazioni di richiesta che influiscono sulla memorizzazione nella cache in modalità indesiderati. Ad esempio, è possibile impostare un browser di `Cache-Control` intestazione `no-cache` o `max-age=0` durante l'aggiornamento di una pagina. Gli strumenti seguenti è possono impostare esplicitamente le intestazioni di richiesta e sono preferiti per il test di memorizzazione nella cache:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Condizioni per la memorizzazione nella cache

* La richiesta deve restituire una risposta del server con un codice di stato 200 (OK).
* Il metodo di richiesta deve essere GET o HEAD.
* Middleware terminal, ad esempio [Middleware File statico](xref:fundamentals/static-files), non è necessario elaborare la risposta prima il Middleware di memorizzazione nella cache della risposta.
* Il `Authorization` intestazione non deve essere presente.
* `Cache-Control`parametri dell'intestazione deve essere validi, e la risposta deve essere contrassegnata `public` e non contrassegnato come `private`.
* Il `Pragma: no-cache` intestazione non deve essere presente se la `Cache-Control` intestazione non è presente, come il `Cache-Control` intestazione esegue l'override di `Pragma` intestazione quando è presente.
* Il `Set-Cookie` intestazione non deve essere presente.
* `Vary`parametri dell'intestazione devono essere valido e non è uguale a `*`.
* Il `Content-Length` valore di intestazione (se impostato) deve corrispondere alle dimensioni del corpo della risposta.
* Il [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) non viene utilizzato.
* La risposta non deve essere non aggiornata come specificato da di `Expires` intestazione e il `max-age` e `s-maxage` direttive di memorizzare nella cache.
* La memorizzazione nel buffer di risposta deve essere completata correttamente e le dimensioni della risposta devono essere minore di o predefinito `SizeLimit`.
* La risposta deve essere memorizzabile nella cache in base al [7234 RFC](https://tools.ietf.org/html/rfc7234) specifiche. Ad esempio, il `no-store` direttiva non deve essere presente nei campi di intestazione di richiesta o risposta. Vedere *sezione 3: archiviare le risposte nella cache* di [7234 RFC](https://tools.ietf.org/html/rfc7234) per informazioni dettagliate.

> [!NOTE]
> Il sistema non riproducibili per generare i token di protezione per evitare Cross-Site Request Forgery (CSRF) attacks set il `Cache-Control` e `Pragma` intestazioni per `no-cache` in modo che non sono memorizzati nella cache le risposte.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Avvio dell'applicazione](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware)
* [Memorizzazione nella cache in memoria](xref:performance/caching/memory)
* [Uso di una cache distribuita](xref:performance/caching/distributed)
* [Rilevare le modifiche apportate con i token di modifica](xref:fundamentals/primitives/change-tokens)
* [Memorizzazione nella cache delle risposte](xref:performance/caching/response)
* [Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
