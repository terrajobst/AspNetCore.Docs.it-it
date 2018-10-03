---
title: Abilitare le richieste Multiorigine (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come condivisione CORS come standard per consentire o rifiutare le richieste multiorigine in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: cfbf24edb1dae76f676d51738b0d57266688d53e
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045588"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Abilitare le richieste Multiorigine (CORS) in ASP.NET Core

Dal [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)

Protezione del browser impedisce che una pagina web inviando richieste a un dominio diverso da quello che ha gestito la pagina web. Questa restrizione viene chiamata il *criterio della stessa origine*. Il criterio della stessa origine impedisce a un sito dannoso di leggere i dati sensibili da un altro sito. In alcuni casi, si potrebbe voler consentire che ad altri siti effettuare richieste multiorigine per l'app.

[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (le origini CORS) è uno standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine. Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre. CORS è più sicura e più flessibile di tecniche precedenti, ad esempio [JSONP](https://wikipedia.org/wiki/JSONP). Questo argomento illustra come abilitare CORS in un'app ASP.NET Core.

## <a name="same-origin"></a>Origine stessa

Due URL abbia la stessa origine se dispongono di porte, host e gli schemi identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Questi due URL abbia la stessa origine:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Questi URL sono le entità Origin diversi rispetto i due URL precedente:

* `https://example.net` &ndash; Dominio diverso
* `https://www.example.com/foo.html` &ndash; Sottodominio diverso
* `http://example.com/foo.html` &ndash; Schema differente
* `https://example.com:9000/foo.html` &ndash; Porta diversa

> [!NOTE]
> Internet Explorer non considera la porta quando si confrontano le entità Origin.

## <a name="register-cors-services"></a>Registrare i servizi CORS

::: moniker range=">= aspnetcore-2.1"

Riferimento di [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto le [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacchetto.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Riferimento di [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage) oppure aggiungere un riferimento al pacchetto le [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacchetto.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aggiungere un riferimento al pacchetto di [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacchetto.

::: moniker-end

Chiamare <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` per aggiungere servizi CORS al contenitore del servizio dell'app:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>Abilitare CORS

Dopo la registrazione di servizi CORS, utilizzare uno degli approcci seguenti per abilitare CORS in un'app ASP.NET Core:

* [Middleware CORS](#enable-cors-with-cors-middleware) &ndash; criteri CORS si applicano a livello globale per l'app tramite il middleware.
* [CORS in MVC](#enable-cors-in-mvc) &ndash; criteri applicare CORS per ogni azione o per ogni controller. Non usare il Middleware CORS.

### <a name="enable-cors-with-cors-middleware"></a>Abilitare CORS con il Middleware CORS

Middleware CORS gestisce le richieste multiorigine per l'app. Per abilitare CORS Middleware nella pipeline di elaborazione della richiesta, chiamare il <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metodo di estensione in `Startup.Configure`.

Middleware CORS devono precedere qualsiasi endpoint definito nell'app in cui si desidera supportare richieste multiorigine (ad esempio, prima della chiamata a `UseMvc` per il Middleware di pagine Razor MVC /).

Oggetto *criteri multiorigine* può essere specificato quando si aggiunge il Middleware CORS usando la <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> classe. Esistono due approcci per la definizione di un criterio CORS:

* Chiamare `UseCors` con un'espressione lambda:

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  L'operatore lambda accetta un <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> oggetto. [Opzioni di configurazione](#cors-policy-options), ad esempio `WithOrigins`, sono descritti più avanti in questo argomento. Nell'esempio precedente, il criterio consente le richieste multiorigine dal `https://example.com` e non le entità Origin.

  È necessario specificare l'URL senza una barra finale (`/`). Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.

  `CorsPolicyBuilder` ha un'API fluent, pertanto è possibile concatenare chiamate al metodo:

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* Definire uno o più criteri CORS denominati e selezionare i criteri in base al nome in fase di esecuzione. L'esempio seguente aggiunge un criterio CORS definite dall'utente denominato *AllowSpecificOrigin*. Per selezionare i criteri, passare il nome da `UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>Abilitare CORS in MVC

In alternativa, è possibile utilizzare MVC per applicare i criteri CORS specifici per ogni azione o per ogni controller. Quando si usa MVC per abilitare CORS, vengono usati i servizi registrati CORS. Non viene usato il Middleware CORS.

### <a name="per-action"></a>Per ogni azione

Per specificare un criterio CORS per un'azione specifica, aggiungere il [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attributo all'azione. Specificare il nome del criterio.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>Per ogni controller

Per specificare i criteri CORS per un controller specifico, aggiungere il [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attributo alla classe controller. Specificare il nome del criterio.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

L'ordine di precedenza è:

1. action
1. controller

### <a name="disable-cors"></a>Disabilitare la condivisione CORS

Per disabilitare CORS per un controller o azione, usare il [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attributo:

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>Opzioni dei criteri CORS

Questa sezione descrive le varie opzioni che è possibile impostare in un criterio CORS.

* [Impostare le origini consentite](#set-the-allowed-origins)
* [Impostare i metodi HTTP consentiti](#set-the-allowed-http-methods)
* [Impostare le intestazioni di richieste consentite](#set-the-allowed-request-headers)
* [Impostare le intestazioni di risposta esposto](#set-the-exposed-response-headers)
* [Credenziali in richieste multiorigine](#credentials-in-cross-origin-requests)
* [Impostare l'ora di scadenza preliminare](#set-the-preflight-expiration-time)

Per alcune opzioni, potrebbe essere utile leggere il [funziona come CORS](#how-cors-works) sezione prima di tutto.

### <a name="set-the-allowed-origins"></a>Impostare le origini consentite

Per consentire a uno o più origini specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

Per consentire tutte le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

Si consideri con attenzione prima di consentire le richieste provenienti da qualsiasi origine. Consentire le richieste provenienti da qualsiasi origine significa che *qualsiasi sito Web* possono eseguire richieste multiorigine per l'app.

Questa impostazione influisce [preparare le richieste e l'intestazione Access-Control-Allow-Origin](#preflight-requests) (descritto più avanti in questo argomento).

### <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

Per consentire a tutti i metodi HTTP, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

Questa impostazione influisce [preparare le richieste e l'intestazione Access-Control-Allow-Methods](#preflight-requests) (descritto più avanti in questo argomento).

### <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni di richieste consentite

Consentire o meno intestazioni specifiche da inviare in una richiesta CORS, chiamato *creare le intestazioni di richiesta*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Per consentire tutti gli autori il intestazioni della richiesta, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

Questa impostazione influisce [preparare le richieste e l'intestazione Access-Control-Request-Headers](#preflight-requests) (descritto più avanti in questo argomento).

::: moniker range=">= aspnetcore-2.2"

Una corrispondenza di criteri di Middleware CORS a intestazioni specifiche specificato da `WithHeaders` è possibile solo quando le intestazioni inviate `Access-Control-Request-Headers` corrispondere esattamente le intestazioni riportate `WithHeaders`.

Ad esempio, si consideri un'applicazione configurata come segue:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Middleware CORS rifiuta una richiesta preliminare con intestazione di richiesta seguente, in quanto `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) non è elencato `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

L'app restituisce una *200 OK* risposta, ma non invia nuovamente le intestazioni CORS. Pertanto, il browser non tenta la richiesta multiorigine.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Middleware CORS consente sempre le intestazioni di quattro nel `Access-Control-Request-Headers` da inviare indipendentemente dai valori configurati in CorsPolicy.Headers. Questo elenco di intestazioni include:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Ad esempio, si consideri un'applicazione configurata come segue:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Middleware CORS risponde correttamente a una richiesta preliminare con intestazione di richiesta seguente poiché `Content-Language` viene sempre incluso nella whitelist:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Impostare le intestazioni di risposta esposto

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app. Per altre informazioni, vedere [W3C Cross-Origin Resource Sharing (terminologia): intestazione della risposta semplice](https://www.w3.org/TR/cors/#simple-response-header).

Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La specifica CORS chiama queste intestazioni *intestazioni di risposta semplice*. Per rendere disponibili app per le altre intestazioni, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Credenziali in richieste multiorigine

Credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser non invia le credenziali con una richiesta multiorigine. Le credenziali includono i cookie e gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta multiorigine, il client deve impostare `XMLHttpRequest.withCredentials` a `true`.

Usando `XMLHttpRequest` direttamente:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

In jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Inoltre, il server deve consentire le credenziali. Per consentire le credenziali tra le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

La risposta HTTP include un `Access-Control-Allow-Credentials` intestazione, che indica al browser che il server consenta le credenziali per una richiesta multiorigine.

Se il browser invia le credenziali, ma la risposta non include un valore valido `Access-Control-Allow-Credentials` intestazione, il browser non espone la risposta all'app e la richiesta multiorigine non riesce.

Prestare attenzione quando si consente le credenziali di cross-origin. Un sito Web all'indirizzo di un altro dominio può inviare le credenziali dell'utente connesso all'app per conto dell'utente senza il consenso dell'utente.

La specifica CORS indica anche che l'impostazione origini `"*"` (tutte le origini) non è valido se la `Access-Control-Allow-Credentials` intestazione è presente.

### <a name="preflight-requests"></a>Richieste preliminari

Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva. Questa richiesta viene chiamata un *richiesta preliminare*. Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:

* Il metodo di richiesta è GET, HEAD o POST.
* L'app non imposta le intestazioni delle richieste diverso da `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, o `Last-Event-ID`.
* Il `Content-Type` intestazione, se impostata, dispone di uno dei valori seguenti:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La regola nelle intestazioni di richiesta impostata per la richiesta del client vengono applicati alle intestazioni che l'app imposta chiamando `setRequestHeader` nella `XMLHttpRequest` oggetto. La specifica CORS chiama queste intestazioni *creare le intestazioni di richiesta*. La regola non è valida per il browser è possibile impostare, ad esempio le intestazioni `User-Agent`, `Host`, o `Content-Length`.

Di seguito è riportato un esempio di una richiesta preliminare:

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP. Include due intestazioni speciali:

* `Access-Control-Request-Method`: Metodo HTTP che verrà usato per la richiesta effettiva.
* `Access-Control-Request-Headers`: Elenco di intestazioni di richiesta che l'app imposta nella richiesta effettiva. Come indicato in precedenza, questo non include le intestazioni che imposta il browser, ad esempio `User-Agent`.

Una richiesta CORS preliminare può includere un `Access-Control-Request-Headers` intestazione, che indica al server le intestazioni che vengono inviate con la richiesta effettiva.

Per consentire a intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Per consentire tutti gli autori il intestazioni della richiesta, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

I browser non sono completamente coerenti nel modo in cui sono impostati `Access-Control-Request-Headers`. Se si imposta le intestazioni a qualsiasi elemento diverso da `"*"` (oppure usare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), è necessario includere almeno `Accept`, `Content-Type`, e `Origin`, oltre a eventuali intestazioni personalizzate che si desidera supportare.

Di seguito è riportato un esempio di risposta alla richiesta preliminare (presupponendo che il server consente la richiesta):

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

La risposta include un' `Access-Control-Allow-Methods` intestazione in cui sono elencati i metodi consentiti e, facoltativamente, un `Access-Control-Allow-Headers` intestazione, che elenca le intestazioni consentite. Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.

Se la richiesta preliminare viene negata, l'app restituisce una *200 OK* risposta, ma non invia nuovamente le intestazioni CORS. Pertanto, il browser non tenta la richiesta multiorigine.

### <a name="set-the-preflight-expiration-time"></a>Impostare l'ora di scadenza preliminare

Il `Access-Control-Max-Age` intestazione specifica quanto tempo può essere memorizzati nella cache la risposta alla richiesta preliminare. Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a>Funzionamento della condivisione CORS

In questa sezione viene descritto cosa succede in una richiesta CORS a livello di messaggi HTTP. È importante comprendere il funzionamento di CORS in modo che il criterio CORS può essere configurato correttamente e il debug quando si verificano comportamenti imprevisti.

La specifica CORS introduce diverse nuove intestazioni HTTP che attivare richieste multiorigine. Se un browser supporta la condivisione CORS, imposta le intestazioni delle automaticamente per le richieste multiorigine. Il codice JavaScript personalizzato non è necessario abilitare CORS.

Di seguito è riportato un esempio di una richiesta multiorigine. Il `Origin` intestazione fornisce il dominio del sito che effettua la richiesta:

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Se il server consente la richiesta, viene impostato il `Access-Control-Allow-Origin` intestazione nella risposta. Il valore di questa intestazione corrisponde a sia la `Origin` intestazione dalla richiesta o è il valore del carattere jolly `"*"`, che significa che è consentita qualsiasi origine:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Se la risposta non include il `Access-Control-Allow-Origin` ha esito negativo di intestazione, la richiesta multiorigine. In particolare, il browser non consente la richiesta. Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'app client.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
