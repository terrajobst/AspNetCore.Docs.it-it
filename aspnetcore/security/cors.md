---
title: Abilitare le richieste Multiorigine (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come condivisione CORS come standard per consentire o rifiutare le richieste multiorigine in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/08/2019
uid: security/cors
ms.openlocfilehash: bc3a0883043a4d6fa33c1ff76fcb7be457b6b840
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899346"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Abilitare le richieste Multiorigine (CORS) in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo articolo illustra come abilitare CORS in un'app ASP.NET Core.

Protezione del browser impedisce che una pagina web inviando richieste a un dominio diverso da quello che ha gestito la pagina web. Questa restrizione viene chiamata il *criterio della stessa origine*. Il criterio della stessa origine impedisce a un sito dannoso di leggere i dati sensibili da un altro sito. In alcuni casi, si potrebbe voler consentire che ad altri siti effettuare richieste multiorigine per l'app. Per altre informazioni, vedere la [articolo Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Cross Origin Resource Sharing, condivisione](https://www.w3.org/TR/cors/) (le origini CORS):

* È un W3C standard che consente a un server di ridurre i criteri di corrispondenza dell'origine.
* Viene **non** una funzionalità di sicurezza, CORS rilassa sicurezza. Un'API non è più sicura, consentendo a CORS. Per altre informazioni, vedere [funziona come CORS](#how-cors).
* Consente a un server consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.
* È più sicura e più flessibile di tecniche precedenti, ad esempio [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([procedura per il download](xref:index#how-to-download-a-sample))

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

Internet Explorer non considera la porta quando si confrontano le entità Origin.

## <a name="cors-with-named-policy-and-middleware"></a>CORS con criteri denominati e middleware

Middleware CORS gestisce le richieste multiorigine. Il codice seguente Abilita CORS per l'intera app con l'entità origin specificata:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Il codice precedente:

* Imposta il nome dei criteri per "_myAllowSpecificOrigins". Il nome del criterio è arbitrario.
* Le chiamate di <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metodo di estensione, che consente di core.
* Le chiamate <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un [espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). L'operatore lambda accetta un <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> oggetto. [Opzioni di configurazione](#cors-policy-options), ad esempio `WithOrigins`, sono descritti più avanti in questo articolo.

Il <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> chiamata al metodo aggiunge servizi CORS al contenitore del servizio dell'app:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Per altre informazioni, vedere [le opzioni dei criteri CORS](#cpo) in questo documento.

Il <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> metodo possibile concatenare i metodi, come illustrato nel codice seguente:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Il codice evidenziato seguente applica i criteri CORS per tutti gli endpoint di App tramite [Middleware CORS](#enable-cors-with-cors-middleware):

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet3&highlight=12)]

Visualizzare [abilitare CORS in Razor Pages, controller e metodi di azione](#ecors) per applicare i criteri CORS a livello di pagina/controller/azione.

Nota:

* `UseCors` deve essere chiamato prima `UseMvc`.
* L'URL deve **non** contengono una barra finale (`/`). Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.

Visualizzare [Test CORS](#test) per istruzioni su come testare il codice precedente.

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a>Abilitare CORS con attributi

Il [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attributo fornisce un'alternativa all'applicazione di condivisione CORS a livello globale. Il `[EnableCors]` attributo Abilita CORS per punti di fine selezionati anziché tutti i punti di fine.

Uso `[EnableCors]` per specificare i criteri predefiniti e `[EnableCors("{Policy String}")]` per specificare un criterio.

Il `[EnableCors]` attributo può essere applicato a:

* Pagina Razor `PageModel`
* Controller
* Metodo di azione del controller

È possibile applicare criteri diversi per controller/pagina-modello/azione con il `[EnableCors]` attributo. Quando il `[EnableCors]` attributo viene applicato a un metodo di azione/controller/pagina-modello e condivisione CORS è abilitata nel middleware, entrambi i criteri vengono applicati. È consigliabile evitare di combinazione di criteri. Usare il `[EnableCors]` attributo o un middleware, non entrambi nella stessa app.

Il codice seguente si applica un criterio diverso per ogni metodo:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Il codice seguente crea un criterio predefinito CORS e un criterio denominato `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Disabilitare la condivisione CORS

Il [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attributo disabilita CORS per il controller/pagina-modello/azione.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opzioni dei criteri CORS

Questa sezione descrive le varie opzioni che possono essere impostate in un criterio CORS:

* [Impostare le origini consentite](#set-the-allowed-origins)
* [Impostare i metodi HTTP consentiti](#set-the-allowed-http-methods)
* [Impostare le intestazioni di richieste consentite](#set-the-allowed-request-headers)
* [Impostare le intestazioni di risposta esposto](#set-the-exposed-response-headers)
* [Credenziali in richieste multiorigine](#credentials-in-cross-origin-requests)
* [Impostare l'ora di scadenza preliminare](#set-the-preflight-expiration-time)

 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> viene chiamato `Startup.ConfigureServices`. Per alcune opzioni, potrebbe essere utile leggere il [funziona come CORS](#how-cors) sezione prima di tutto.

## <a name="set-the-allowed-origins"></a>Impostare le origini consentite

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Consente le richieste CORS da tutte le entità Origin con qualsiasi schema (`http` o `https`). `AllowAnyOrigin` non è sicura perché *qualsiasi sito Web* possono eseguire richieste multiorigine per l'app.

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > Che specifica `AllowAnyOrigin` e `AllowCredentials` è una configurazione non protetta e può comportare la richiesta intersito falsa. Il servizio CORS restituisce una risposta CORS non valida quando un'app è configurata con entrambi i metodi.

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > Che specifica `AllowAnyOrigin` e `AllowCredentials` è una configurazione non protetta e può comportare la richiesta intersito falsa. Per un'app protetta, specificare l'elenco esatto di origini, se è necessario autorizzare il client per accedere alle risorse di server.

  ::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**. 
-->

  `AllowAnyOrigin` le richieste di verifica preliminare interessa e `Access-Control-Allow-Origin` intestazione. Per altre informazioni, vedere la [richieste di verifica preliminare](#preflight-requests) sezione.

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Imposta il <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> proprietà dei criteri è una funzione che consente le origini corrispondere a un dominio configurato con caratteri jolly quando si valuta se l'origine è consentita.

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Consente a qualsiasi metodo HTTP:
* Le richieste di verifica preliminare interessa e `Access-Control-Allow-Methods` intestazione. Per altre informazioni, vedere la [richieste di verifica preliminare](#preflight-requests) sezione.

### <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni di richieste consentite

Consentire o meno intestazioni specifiche da inviare in una richiesta CORS, chiamato *creare le intestazioni di richiesta*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Per consentire tutti gli autori il intestazioni della richiesta, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Questa impostazione interessa le richieste preliminari e `Access-Control-Request-Headers` intestazione. Per altre informazioni, vedere la [richieste di verifica preliminare](#preflight-requests) sezione.

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

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app. Per altre informazioni, vedere [W3C Cross-Origin Resource Sharing (terminologia): Intestazione della risposta semplice](https://www.w3.org/TR/cors/#simple-response-header).

Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La specifica CORS chiama queste intestazioni *intestazioni di risposta semplice*. Per rendere disponibili app per le altre intestazioni, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Credenziali in richieste multiorigine

Credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser non invia le credenziali con una richiesta multiorigine. Le credenziali includono i cookie e gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta multiorigine, il client deve impostare `XMLHttpRequest.withCredentials` a `true`.

Usando `XMLHttpRequest` direttamente:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Uso di jQuery:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

Usando il [recuperare API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Il server deve consentire le credenziali. Per consentire le credenziali tra le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La risposta HTTP include un `Access-Control-Allow-Credentials` intestazione, che indica al browser che il server consenta le credenziali per una richiesta multiorigine.

Se il browser invia le credenziali, ma la risposta non include un valore valido `Access-Control-Allow-Credentials` intestazione, il browser non espone la risposta all'app e la richiesta multiorigine non riesce.

Consentire le credenziali di cross-origin è un rischio per la sicurezza. Un sito Web all'indirizzo di un altro dominio può inviare le credenziali dell'utente connesso all'app per conto dell'utente senza il consenso dell'utente. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

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

* `Access-Control-Request-Method`: Il metodo HTTP che verrà usato per la richiesta effettiva.
* `Access-Control-Request-Headers`: Elenco di intestazioni di richiesta che l'app imposta nella richiesta effettiva. Come indicato in precedenza, questo non include le intestazioni che imposta il browser, ad esempio `User-Agent`.

Una richiesta CORS preliminare può includere un `Access-Control-Request-Headers` intestazione, che indica al server le intestazioni che vengono inviate con la richiesta effettiva.

Per consentire a intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Per consentire tutti gli autori il intestazioni della richiesta, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

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

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Funzionamento della condivisione CORS

In questa sezione viene descritto cosa succede un [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) richiesta a livello di messaggi HTTP.

* CORS è **non** una funzionalità di sicurezza. CORS è un standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine.
  * Ad esempio, è possibile usare un attore malintenzionato [impedire Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) contro il sito ed eseguire una richiesta tra siti con il relativo sito abilitato per CORS di sottrarre informazioni.
* L'API non è più sicuro, consentendo a CORS.
  * Spetta al client (browser) per applicare CORS. Il server esegue la richiesta e restituisce la risposta, ma il client che restituisce un errore e i blocchi la risposta. Ad esempio, uno qualsiasi dei seguenti strumenti verrà visualizzata la risposta del server:
     * [Fiddler](https://www.telerik.com/fiddler)
     * [Postman](https://www.getpostman.com/)
     * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
     * Un web browser immettendo l'URL nella barra degli indirizzi.
* È un modo per un server per consentire i browser per l'esecuzione di una cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) oppure [recuperare API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) richiesta che in caso contrario, potrebbe essere non è consentito.
  * Browser (senza CORS) non è possibile eseguire l'operazione di richieste multiorigine. Prima di CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) usata per aggirare questa limitazione. JSONP non usa XHR, Usa il `<script>` tag per ricevere la risposta. Gli script possono essere caricati cross-origin.

Il [specifica CORS]() introdotte diverse nuove intestazioni HTTP che attivare richieste multiorigine. Se un browser supporta la condivisione CORS, imposta le intestazioni delle automaticamente per le richieste multiorigine. Il codice JavaScript personalizzato non è necessario abilitare CORS.

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

<a name="test"></a>

## <a name="test-cors"></a>Testare CORS

Testare CORS:

1. [Creare un progetto API](xref:tutorials/first-web-api). In alternativa, è possibile [scaricare l'esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Abilitare la condivisione CORS tramite uno degli approcci in questo documento. Ad esempio:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]
  
  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` deve essere utilizzato solo per i test di un'app di esempio simile al [scaricare codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Creare un progetto di app web (Razor Pages o MVC). L'esempio Usa le pagine Razor. È possibile creare l'app web nella stessa soluzione come progetto di API.
1. Aggiungere il codice evidenziato seguente per il *index. cshtml* file:

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. Nel codice precedente sostituire `url: 'https://<web app>.azurewebsites.net/api/values/1',` con l'URL per l'app distribuita.
1. Distribuire il progetto di API. Ad esempio, [Distribuisci in Azure](xref:host-and-deploy/azure-apps/index).
1. Eseguire l'app per Razor Pages o MVC dal desktop e fare clic sui **Test** pulsante. Usare gli strumenti F12 per esaminare i messaggi di errore.
1. Rimuovere l'origine di localhost da `WithOrigins` e distribuire l'app. In alternativa, eseguire l'app client con una porta diversa. Ad esempio, eseguire da Visual Studio.
1. Il test con l'app client. Gli errori CORS restituiscono un errore, ma il messaggio di errore non è disponibile a JavaScript. Utilizzare la scheda della console negli strumenti F12 per visualizzare l'errore. A seconda del browser, viene visualizzato un errore (nella console di strumenti F12) simile al seguente:

  * Utilizzo di Microsoft Edge:

    **SEC7120: [CORS] l'origine 'https://localhost:44375'non ha trovato'https://localhost:44375'nell'intestazione della risposta Access-Control-Allow-Origin per la risorsa cross-origin'https://webapi.azurewebsites.net/api/values/1'.**

  * Usa Chrome:

    **Accesso a XMLHttpRequest in 'https://webapi.azurewebsites.net/api/values/1'dall'origine'https://localhost:44375' è stata bloccata dal criterio CORS: Nessuna intestazione 'Access-Control-Allow-Origin' è presente nella risorsa richiesta.**

## <a name="additional-resources"></a>Risorse aggiuntive

* [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
