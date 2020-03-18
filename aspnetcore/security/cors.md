---
title: Abilitare le richieste tra le origini (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come CORS come standard per consentire o rifiutare le richieste tra origini in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 09cc296ebf605907371619124cac00883beb6abb
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511574"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Abilitare le richieste tra le origini (CORS) in ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo articolo illustra come abilitare CORS in un'app ASP.NET Core.

La sicurezza del browser impedisce a una pagina Web di eseguire richieste a un dominio diverso da quello che ha gestito la pagina Web. Questa restrizione è detta *criterio della stessa origine*. Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito. In alcuni casi potrebbe essere necessario consentire ad altri siti di effettuare richieste tra le origini all'app. Per altre informazioni, vedere l' [articolo relativo a Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Condivisione risorse tra le origini](https://www.w3.org/TR/cors/) (CORS):

* È uno standard W3C che consente a un server di ridurre i criteri della stessa origine.
* **Non** è una funzionalità di sicurezza, CORS rilassa la sicurezza. Un'API non è più sicura consentendo CORS. Per ulteriori informazioni, vedere [funzionamento di CORS](#how-cors).
* Consente a un server di consentire in modo esplicito alcune richieste tra origini rifiutando altre.
* È più sicuro e flessibile rispetto alle tecniche precedenti, ad esempio [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Stessa origine

Due URL hanno la stessa origine se hanno schemi, host e porte identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Questi due URL hanno la stessa origine:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Questi URL hanno origini diverse da quelle dei due URL precedenti:

* `https://example.net` &ndash; dominio diverso
* `https://www.example.com/foo.html` &ndash; sottodominio diverso
* `http://example.com/foo.html` &ndash; schema diverso
* `https://example.com:9000/foo.html` &ndash; porta diversa

Internet Explorer non considera la porta durante il confronto delle origini.

## <a name="cors-with-named-policy-and-middleware"></a>CORS con criteri e middleware denominati

Il middleware CORS gestisce le richieste tra le origini. Il codice seguente Abilita CORS per l'intera app con l'origine specificata:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Il codice precedente:

* Imposta il nome del criterio su "\_myAllowSpecificOrigins". Il nome del criterio è arbitrario.
* Chiama il metodo di estensione <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, che Abilita CORS.
* Chiama <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un' [espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). L'espressione lambda accetta un oggetto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. Le [Opzioni di configurazione](#cors-policy-options), ad esempio `WithOrigins`, sono descritte più avanti in questo articolo.

La chiamata al metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> aggiunge i servizi CORS al contenitore del servizio dell'app:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Per ulteriori informazioni, vedere [Opzioni dei criteri CORS](#cpo) in questo documento.

Il metodo <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> può concatenare i metodi, come illustrato nel codice seguente:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Nota: l'URL **non** deve contenere una barra finale (`/`). Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a>Applicare i criteri di CORS a tutti gli endpoint

Il codice seguente applica i criteri di CORS a tutti gli endpoint di app tramite il middleware CORS:
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code omitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code omitted.
}
```

> [!WARNING]
> Con il routing dell'endpoint è necessario configurare il middleware CORS per l'esecuzione tra le chiamate a `UseRouting` e `UseEndpoints`. Una configurazione non corretta causerà il corretto funzionamento del middleware.

Per applicare i criteri di CORS a livello di pagina/controller/azione [, vedere Enable CORS in Razor Pages, Controllers e Action Methods](#ecors) .

Vedere [test CORS](#test) per istruzioni sul test del codice precedente.

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a>Abilitare cors con routing degli endpoint

Con il routing degli endpoint, è possibile abilitare CORS in base all'endpoint usando il set `RequireCors` di metodi di estensione.

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

Analogamente, è anche possibile abilitare CORS per tutti i controller:

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```

## <a name="enable-cors-with-attributes"></a>Abilita CORS con attributi

L'attributo [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fornisce un'alternativa all'applicazione di CORS a livello globale. L'attributo `[EnableCors]` Abilita CORS per i punti finali selezionati, anziché tutti i punti finali.

Utilizzare `[EnableCors]` per specificare i criteri predefiniti e `[EnableCors("{Policy String}")]` per specificare un criterio.

L'attributo `[EnableCors]` può essere applicato a:

* `PageModel` pagina Razor
* Controller
* Metodo di azione del controller

È possibile applicare criteri diversi a controller/pagina-modello/azione con l'attributo `[EnableCors]`. Quando l'attributo `[EnableCors]` viene applicato a controller/modello di pagina/Metodo di azione e CORS è abilitato nel middleware, vengono applicati entrambi i criteri. È consigliabile evitare di combinare i criteri. Usare il middleware o l'attributo `[EnableCors]`, non entrambi nella stessa app.

Il codice seguente applica un criterio diverso a ogni metodo:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Il codice seguente crea un criterio predefinito CORS e un criterio denominato `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Disabilitare CORS

L'attributo [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Disabilita CORS per il controller, il modello di pagina o l'azione.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opzioni dei criteri di CORS

In questa sezione vengono descritte le varie opzioni che è possibile impostare in un criterio CORS:

* [Imposta le origini consentite](#set-the-allowed-origins)
* [Impostare i metodi HTTP consentiti](#set-the-allowed-http-methods)
* [Impostare le intestazioni della richiesta consentita](#set-the-allowed-request-headers)
* [Impostare le intestazioni di risposta esposte](#set-the-exposed-response-headers)
* [Credenziali nelle richieste tra le origini](#credentials-in-cross-origin-requests)
* [Imposta la data di scadenza preliminare](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> viene chiamato `Startup.ConfigureServices`. Per alcune opzioni, può essere utile leggere prima la sezione [come funziona CORS](#how-cors) .

## <a name="set-the-allowed-origins"></a>Imposta le origini consentite

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; consente richieste CORS da tutte le origini con qualsiasi schema (`http` o `https`). `AllowAnyOrigin` non è sicuro perché *qualsiasi sito Web* può effettuare richieste tra origini per l'app.

> [!NOTE]
> La specifica di `AllowAnyOrigin` e `AllowCredentials` è una configurazione non sicura e può comportare la falsa richiesta tra siti. Il servizio CORS restituisce una risposta CORS non valida quando un'app è configurata con entrambi i metodi.

`AllowAnyOrigin` influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Origin`. Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; imposta la proprietà <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> del criterio in modo che sia una funzione che consente alle origini di corrispondere a un dominio con caratteri jolly configurato durante la valutazione se l'origine è consentita.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Consente qualsiasi metodo HTTP:
* Influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Methods`. Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni della richiesta consentita

Per consentire l'invio di intestazioni specifiche in una richiesta CORS, denominate *intestazioni richiesta autore*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Questa impostazione influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Request-Headers`. Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .


Un criterio middleware CORS corrisponde a intestazioni specifiche specificate da `WithHeaders` è possibile solo quando le intestazioni inviate in `Access-Control-Request-Headers` corrispondono esattamente alle intestazioni indicate in `WithHeaders`.

Si consideri, ad esempio, un'app configurata come segue:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Il middleware CORS rifiuta una richiesta preliminare con l'intestazione della richiesta seguente, perché `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) non è elencato in `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

L'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS. Pertanto, il browser non tenta di eseguire la richiesta tra origini.

### <a name="set-the-exposed-response-headers"></a>Impostare le intestazioni di risposta esposte

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app. Per altre informazioni, vedere la pagina relativa alla [condivisione delle risorse tra le origini W3C (terminologia): intestazione della risposta semplice](https://www.w3.org/TR/cors/#simple-response-header).

Le intestazioni di risposta disponibili per impostazione predefinita sono:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La specifica CORS chiama queste intestazioni di *risposta semplici*. Per rendere disponibili altre intestazioni per l'app, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Credenziali nelle richieste tra le origini

Le credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser non invia le credenziali con una richiesta tra le origini. Le credenziali includono i cookie e gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta tra le origini, il client deve impostare `XMLHttpRequest.withCredentials` su `true`.

Utilizzando direttamente `XMLHttpRequest`:

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

Uso dell' [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Il server deve consentire le credenziali. Per consentire le credenziali tra le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La risposta HTTP include un'intestazione `Access-Control-Allow-Credentials`, che indica al browser che il server consente le credenziali per una richiesta tra le origini.

Se il browser invia le credenziali, ma la risposta non include un'intestazione `Access-Control-Allow-Credentials` valida, il browser non espone la risposta all'app e la richiesta tra le origini ha esito negativo.

Consentire le credenziali tra le origini costituisce un rischio per la sicurezza. Un sito Web in un altro dominio può inviare le credenziali dell'utente che ha eseguito l'accesso all'app per conto dell'utente senza la conoscenza dell'utente. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

La specifica CORS indica inoltre che l'impostazione delle origini per `"*"` (tutte le origini) non è valida se è presente l'intestazione del `Access-Control-Allow-Credentials`.

### <a name="preflight-requests"></a>Richieste preliminari

Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva. Questa richiesta è detta *richiesta preliminare*. Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:

* Il metodo di richiesta è GET, HEAD o POST.
* L'app non imposta intestazioni di richiesta diverse da `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`o `Last-Event-ID`.
* L'intestazione `Content-Type`, se impostata, presenta uno dei valori seguenti:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La regola sulle intestazioni di richiesta impostate per la richiesta del client si applica alle intestazioni impostate dall'app chiamando `setRequestHeader` sull'oggetto `XMLHttpRequest`. La specifica CORS chiama queste intestazioni di *richiesta autore*intestazioni. La regola non si applica alle intestazioni che il browser può impostare, ad esempio `User-Agent`, `Host`o `Content-Length`.

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

La richiesta pre-flight usa il metodo delle opzioni HTTP. Sono incluse due intestazioni speciali:

* `Access-Control-Request-Method`: il metodo HTTP che verrà usato per la richiesta effettiva.
* `Access-Control-Request-Headers`: elenco di intestazioni di richiesta impostate dall'app sulla richiesta effettiva. Come indicato in precedenza, non include le intestazioni impostate dal browser, ad esempio `User-Agent`.

Una richiesta preliminare CORS può includere un'intestazione `Access-Control-Request-Headers`, che indica al server le intestazioni inviate con la richiesta effettiva.

Per consentire intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

I browser non sono completamente coerenti nell'impostazione `Access-Control-Request-Headers`. Se si impostano le intestazioni su un valore diverso da `"*"` (o si utilizza <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), è necessario includere almeno `Accept`, `Content-Type`e `Origin`, oltre a eventuali intestazioni personalizzate che si desidera supportare.

Di seguito è riportato un esempio di risposta alla richiesta preliminare (presupponendo che il server consenta la richiesta):

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

La risposta include un'intestazione `Access-Control-Allow-Methods` che elenca i metodi consentiti e, facoltativamente, un'intestazione `Access-Control-Allow-Headers`, che elenca le intestazioni consentite. Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.

Se la richiesta preliminare viene negata, l'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS. Pertanto, il browser non tenta di eseguire la richiesta tra origini.

### <a name="set-the-preflight-expiration-time"></a>Imposta la data di scadenza preliminare

L'intestazione `Access-Control-Max-Age` specifica per quanto tempo la risposta alla richiesta preliminare può essere memorizzata nella cache. Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Funzionamento di CORS

In questa sezione viene descritto cosa accade in una richiesta [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) al livello dei messaggi HTTP.

* CORS **non** è una funzionalità di sicurezza. CORS è uno standard W3C che consente a un server di ridurre i criteri della stessa origine.
  * Ad esempio, un attore malintenzionato potrebbe usare [Impedisci lo scripting tra siti (XSS)](xref:security/cross-site-scripting) per il sito ed eseguire una richiesta tra siti al sito abilitato per CORS per rubare le informazioni.
* L'API non è più sicura consentendo CORS.
  * Il client (browser) deve applicare CORS. Il server esegue la richiesta e restituisce la risposta, ovvero il client che restituisce un errore e blocca la risposta. Uno degli strumenti seguenti, ad esempio, visualizzerà la risposta del server:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [HttpClient .NET](/dotnet/csharp/tutorials/console-webapiclient)
    * Un Web browser immettendo l'URL nella barra degli indirizzi.
* Si tratta di un modo per consentire a un server di consentire ai browser di eseguire una richiesta dell' [API di recupero](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) o di [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) tra le origini, che altrimenti sarebbe proibita.
  * I browser (senza CORS) non possono eseguire richieste tra le origini. Prima di CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) è stato usato per aggirare questa restrizione. JSONP non usa XHR, usa il tag `<script>` per ricevere la risposta. Gli script possono essere caricati tra le origini.

La [specifica CORS](https://www.w3.org/TR/cors/) ha introdotto diverse nuove intestazioni HTTP che consentono richieste tra le origini. Se un browser supporta CORS, queste intestazioni vengono impostate automaticamente per le richieste tra le origini. Il codice JavaScript personalizzato non è necessario per abilitare CORS.

Di seguito è riportato un esempio di richiesta tra le origini. L'intestazione `Origin` fornisce il dominio del sito che sta effettuando la richiesta. L'intestazione `Origin` è obbligatoria e deve essere diversa dall'host.

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

Se il server consente la richiesta, imposta l'intestazione `Access-Control-Allow-Origin` nella risposta. Il valore di questa intestazione corrisponde all'intestazione `Origin` dalla richiesta o è il valore del carattere jolly `"*"`, che indica che è consentita un'origine:

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

Se la risposta non include l'intestazione `Access-Control-Allow-Origin`, la richiesta tra le origini ha esito negativo. In particolare, il browser non consente la richiesta. Anche se il server restituisce una risposta con esito positivo, il browser non rende disponibile la risposta all'app client.

<a name="test"></a>

## <a name="test-cors"></a>Testare CORS

Per testare CORS:

1. [Creare un progetto API](xref:tutorials/first-web-api). In alternativa, è possibile [scaricare l'esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Abilitare CORS usando uno degli approcci descritti in questo documento. Ad esempio,

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` deve essere usato solo per il test di un'app di esempio simile al [codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)per il download.

1. Creare un progetto di app Web (Razor Pages o MVC). Nell'esempio viene utilizzato Razor Pages. È possibile creare l'app Web nella stessa soluzione del progetto API.
1. Aggiungere il codice evidenziato seguente al file *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. Nel codice precedente sostituire `url: 'https://<web app>.azurewebsites.net/api/values/1',` con l'URL dell'app distribuita.
1. Distribuire il progetto API. Ad esempio, [eseguire la distribuzione in Azure](xref:host-and-deploy/azure-apps/index).
1. Eseguire l'app Razor Pages o MVC dal desktop e fare clic sul pulsante **test** . Usare gli strumenti F12 per esaminare i messaggi di errore.
1. Rimuovere l'origine localhost da `WithOrigins` e distribuire l'app. In alternativa, eseguire l'app client con una porta diversa. Ad esempio, eseguire da Visual Studio.
1. Eseguire il test con l'app client. Gli errori CORS restituiscono un errore, ma il messaggio di errore non è disponibile per JavaScript. Usare la scheda console negli strumenti F12 per visualizzare l'errore. A seconda del browser, viene ricevuto un errore (nella console strumenti F12) simile al seguente:

   * Uso di Microsoft Edge:

     **SEC7120: [CORS] la `https://localhost:44375` di origine non ha trovato `https://localhost:44375` nell'intestazione della risposta Access-Control-Allow-Origin per la risorsa tra le origini in `https://webapi.azurewebsites.net/api/values/1`**

   * Uso di Chrome:

     **L'accesso a XMLHttpRequest in `https://webapi.azurewebsites.net/api/values/1` dalla `https://localhost:44375` di origine è stato bloccato dal criterio CORS: non è presente alcuna intestazione ' Access-Control-Allow-Origin ' nella risorsa richiesta.**
     
Gli endpoint abilitati per CORS possono essere testati con uno strumento, ad esempio [Fiddler](https://www.telerik.com/fiddler) o [postazione](https://www.getpostman.com/). Quando si utilizza uno strumento, l'origine della richiesta specificata dall'intestazione `Origin` deve essere diversa dall'host che riceve la richiesta. Se la richiesta non è *tra origini* basate sul valore dell'intestazione `Origin`:

* Non è necessario che il middleware CORS elabori la richiesta.
* Le intestazioni CORS non vengono restituite nella risposta.

## <a name="cors-in-iis"></a>CORS in IIS

Quando si esegue la distribuzione in IIS, CORS deve essere eseguito prima dell'autenticazione di Windows se il server non è configurato per consentire l'accesso anonimo. Per supportare questo scenario, è necessario installare e configurare il [modulo cors di IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) per l'app.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Condivisione di risorse tra le origini (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [Introduzione al modulo CORS di IIS](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo articolo illustra come abilitare CORS in un'app ASP.NET Core.

La sicurezza del browser impedisce a una pagina Web di eseguire richieste a un dominio diverso da quello che ha gestito la pagina Web. Questa restrizione è detta *criterio della stessa origine*. Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito. In alcuni casi potrebbe essere necessario consentire ad altri siti di effettuare richieste tra le origini all'app. Per altre informazioni, vedere l' [articolo relativo a Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Condivisione risorse tra le origini](https://www.w3.org/TR/cors/) (CORS):

* È uno standard W3C che consente a un server di ridurre i criteri della stessa origine.
* **Non** è una funzionalità di sicurezza, CORS rilassa la sicurezza. Un'API non è più sicura consentendo CORS. Per ulteriori informazioni, vedere [funzionamento di CORS](#how-cors).
* Consente a un server di consentire in modo esplicito alcune richieste tra origini rifiutando altre.
* È più sicuro e flessibile rispetto alle tecniche precedenti, ad esempio [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Stessa origine

Due URL hanno la stessa origine se hanno schemi, host e porte identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Questi due URL hanno la stessa origine:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Questi URL hanno origini diverse da quelle dei due URL precedenti:

* `https://example.net` &ndash; dominio diverso
* `https://www.example.com/foo.html` &ndash; sottodominio diverso
* `http://example.com/foo.html` &ndash; schema diverso
* `https://example.com:9000/foo.html` &ndash; porta diversa

Internet Explorer non considera la porta durante il confronto delle origini.

## <a name="cors-with-named-policy-and-middleware"></a>CORS con criteri e middleware denominati

Il middleware CORS gestisce le richieste tra le origini. Il codice seguente Abilita CORS per l'intera app con l'origine specificata:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Il codice precedente:

* Imposta il nome del criterio su "\_myAllowSpecificOrigins". Il nome del criterio è arbitrario.
* Chiama il metodo di estensione <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, che Abilita CORS.
* Chiama <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un' [espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). L'espressione lambda accetta un oggetto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. Le [Opzioni di configurazione](#cors-policy-options), ad esempio `WithOrigins`, sono descritte più avanti in questo articolo.

La chiamata al metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> aggiunge i servizi CORS al contenitore del servizio dell'app:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Per ulteriori informazioni, vedere [Opzioni dei criteri CORS](#cpo) in questo documento.

Il metodo <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> può concatenare i metodi, come illustrato nel codice seguente:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Nota: l'URL **non** deve contenere una barra finale (`/`). Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.

Il codice seguente applica i criteri di CORS a tutti gli endpoint di app tramite il middleware CORS:
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```
Nota: è necessario chiamare `UseCors` prima di `UseMvc`.

Per applicare i criteri di CORS a livello di pagina/controller/azione [, vedere Enable CORS in Razor Pages, Controllers e Action Methods](#ecors) .

Vedere [test CORS](#test) per istruzioni sul test del codice precedente.

## <a name="enable-cors-with-attributes"></a>Abilita CORS con attributi

L'attributo [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fornisce un'alternativa all'applicazione di CORS a livello globale. L'attributo `[EnableCors]` Abilita CORS per i punti finali selezionati, anziché tutti i punti finali.

Utilizzare `[EnableCors]` per specificare i criteri predefiniti e `[EnableCors("{Policy String}")]` per specificare un criterio.

L'attributo `[EnableCors]` può essere applicato a:

* `PageModel` pagina Razor
* Controller
* Metodo di azione del controller

È possibile applicare criteri diversi a controller/pagina-modello/azione con l'attributo `[EnableCors]`. Quando l'attributo `[EnableCors]` viene applicato a controller/modello di pagina/Metodo di azione e CORS è abilitato nel middleware, vengono applicati entrambi i criteri. È consigliabile evitare di combinare i criteri. Usare il middleware o l'attributo `[EnableCors]`, non entrambi nella stessa app.

Il codice seguente applica un criterio diverso a ogni metodo:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Il codice seguente crea un criterio predefinito CORS e un criterio denominato `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Disabilitare CORS

L'attributo [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Disabilita CORS per il controller, il modello di pagina o l'azione.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opzioni dei criteri di CORS

In questa sezione vengono descritte le varie opzioni che è possibile impostare in un criterio CORS:

* [Imposta le origini consentite](#set-the-allowed-origins)
* [Impostare i metodi HTTP consentiti](#set-the-allowed-http-methods)
* [Impostare le intestazioni della richiesta consentita](#set-the-allowed-request-headers)
* [Impostare le intestazioni di risposta esposte](#set-the-exposed-response-headers)
* [Credenziali nelle richieste tra le origini](#credentials-in-cross-origin-requests)
* [Imposta la data di scadenza preliminare](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> viene chiamato `Startup.ConfigureServices`. Per alcune opzioni, può essere utile leggere prima la sezione [come funziona CORS](#how-cors) .

## <a name="set-the-allowed-origins"></a>Imposta le origini consentite

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; consente richieste CORS da tutte le origini con qualsiasi schema (`http` o `https`). `AllowAnyOrigin` non è sicuro perché *qualsiasi sito Web* può effettuare richieste tra origini per l'app.

> [!NOTE]
> La specifica di `AllowAnyOrigin` e `AllowCredentials` è una configurazione non sicura e può comportare la falsa richiesta tra siti. Per un'app sicura, specificare un elenco esatto di origini se il client deve autorizzare se stesso ad accedere alle risorse del server.

`AllowAnyOrigin` influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Origin`. Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; imposta la proprietà <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> del criterio in modo che sia una funzione che consente alle origini di corrispondere a un dominio con caratteri jolly configurato durante la valutazione se l'origine è consentita.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Consente qualsiasi metodo HTTP:
* Influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Methods`. Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni della richiesta consentita

Per consentire l'invio di intestazioni specifiche in una richiesta CORS, denominate *intestazioni richiesta autore*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Questa impostazione influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Request-Headers`. Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .

Il middleware CORS consente sempre di inviare quattro intestazioni nel `Access-Control-Request-Headers` indipendentemente dai valori configurati in CorsPolicy. Headers. Questo elenco di intestazioni include:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Si consideri, ad esempio, un'app configurata come segue:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Il middleware CORS risponde correttamente a una richiesta preliminare con la seguente intestazione di richiesta perché `Content-Language` è sempre consentita:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a>Impostare le intestazioni di risposta esposte

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app. Per altre informazioni, vedere la pagina relativa alla [condivisione delle risorse tra le origini W3C (terminologia): intestazione della risposta semplice](https://www.w3.org/TR/cors/#simple-response-header).

Le intestazioni di risposta disponibili per impostazione predefinita sono:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La specifica CORS chiama queste intestazioni di *risposta semplici*. Per rendere disponibili altre intestazioni per l'app, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Credenziali nelle richieste tra le origini

Le credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser non invia le credenziali con una richiesta tra le origini. Le credenziali includono i cookie e gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta tra le origini, il client deve impostare `XMLHttpRequest.withCredentials` su `true`.

Utilizzando direttamente `XMLHttpRequest`:

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

Uso dell' [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Il server deve consentire le credenziali. Per consentire le credenziali tra le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La risposta HTTP include un'intestazione `Access-Control-Allow-Credentials`, che indica al browser che il server consente le credenziali per una richiesta tra le origini.

Se il browser invia le credenziali, ma la risposta non include un'intestazione `Access-Control-Allow-Credentials` valida, il browser non espone la risposta all'app e la richiesta tra le origini ha esito negativo.

Consentire le credenziali tra le origini costituisce un rischio per la sicurezza. Un sito Web in un altro dominio può inviare le credenziali dell'utente che ha eseguito l'accesso all'app per conto dell'utente senza la conoscenza dell'utente. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

La specifica CORS indica inoltre che l'impostazione delle origini per `"*"` (tutte le origini) non è valida se è presente l'intestazione del `Access-Control-Allow-Credentials`.

### <a name="preflight-requests"></a>Richieste preliminari

Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva. Questa richiesta è detta *richiesta preliminare*. Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:

* Il metodo di richiesta è GET, HEAD o POST.
* L'app non imposta intestazioni di richiesta diverse da `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`o `Last-Event-ID`.
* L'intestazione `Content-Type`, se impostata, presenta uno dei valori seguenti:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La regola sulle intestazioni di richiesta impostate per la richiesta del client si applica alle intestazioni impostate dall'app chiamando `setRequestHeader` sull'oggetto `XMLHttpRequest`. La specifica CORS chiama queste intestazioni di *richiesta autore*intestazioni. La regola non si applica alle intestazioni che il browser può impostare, ad esempio `User-Agent`, `Host`o `Content-Length`.

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

La richiesta pre-flight usa il metodo delle opzioni HTTP. Sono incluse due intestazioni speciali:

* `Access-Control-Request-Method`: il metodo HTTP che verrà usato per la richiesta effettiva.
* `Access-Control-Request-Headers`: elenco di intestazioni di richiesta impostate dall'app sulla richiesta effettiva. Come indicato in precedenza, non include le intestazioni impostate dal browser, ad esempio `User-Agent`.

Una richiesta preliminare CORS può includere un'intestazione `Access-Control-Request-Headers`, che indica al server le intestazioni inviate con la richiesta effettiva.

Per consentire intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

I browser non sono completamente coerenti nell'impostazione `Access-Control-Request-Headers`. Se si impostano le intestazioni su un valore diverso da `"*"` (o si utilizza <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), è necessario includere almeno `Accept`, `Content-Type`e `Origin`, oltre a eventuali intestazioni personalizzate che si desidera supportare.

Di seguito è riportato un esempio di risposta alla richiesta preliminare (presupponendo che il server consenta la richiesta):

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

La risposta include un'intestazione `Access-Control-Allow-Methods` che elenca i metodi consentiti e, facoltativamente, un'intestazione `Access-Control-Allow-Headers`, che elenca le intestazioni consentite. Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.

Se la richiesta preliminare viene negata, l'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS. Pertanto, il browser non tenta di eseguire la richiesta tra origini.

### <a name="set-the-preflight-expiration-time"></a>Imposta la data di scadenza preliminare

L'intestazione `Access-Control-Max-Age` specifica per quanto tempo la risposta alla richiesta preliminare può essere memorizzata nella cache. Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Funzionamento di CORS

In questa sezione viene descritto cosa accade in una richiesta [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) al livello dei messaggi HTTP.

* CORS **non** è una funzionalità di sicurezza. CORS è uno standard W3C che consente a un server di ridurre i criteri della stessa origine.
  * Ad esempio, un attore malintenzionato potrebbe usare [Impedisci lo scripting tra siti (XSS)](xref:security/cross-site-scripting) per il sito ed eseguire una richiesta tra siti al sito abilitato per CORS per rubare le informazioni.
* L'API non è più sicura consentendo CORS.
  * Il client (browser) deve applicare CORS. Il server esegue la richiesta e restituisce la risposta, ovvero il client che restituisce un errore e blocca la risposta. Uno degli strumenti seguenti, ad esempio, visualizzerà la risposta del server:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [HttpClient .NET](/dotnet/csharp/tutorials/console-webapiclient)
    * Un Web browser immettendo l'URL nella barra degli indirizzi.
* Si tratta di un modo per consentire a un server di consentire ai browser di eseguire una richiesta dell' [API di recupero](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) o di [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) tra le origini, che altrimenti sarebbe proibita.
  * I browser (senza CORS) non possono eseguire richieste tra le origini. Prima di CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) è stato usato per aggirare questa restrizione. JSONP non usa XHR, usa il tag `<script>` per ricevere la risposta. Gli script possono essere caricati tra le origini.

La [specifica CORS](https://www.w3.org/TR/cors/) ha introdotto diverse nuove intestazioni HTTP che consentono richieste tra le origini. Se un browser supporta CORS, queste intestazioni vengono impostate automaticamente per le richieste tra le origini. Il codice JavaScript personalizzato non è necessario per abilitare CORS.

Di seguito è riportato un esempio di richiesta tra le origini. L'intestazione `Origin` fornisce il dominio del sito che sta effettuando la richiesta. L'intestazione `Origin` è obbligatoria e deve essere diversa dall'host.

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

Se il server consente la richiesta, imposta l'intestazione `Access-Control-Allow-Origin` nella risposta. Il valore di questa intestazione corrisponde all'intestazione `Origin` dalla richiesta o è il valore del carattere jolly `"*"`, che indica che è consentita un'origine:

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

Se la risposta non include l'intestazione `Access-Control-Allow-Origin`, la richiesta tra le origini ha esito negativo. In particolare, il browser non consente la richiesta. Anche se il server restituisce una risposta con esito positivo, il browser non rende disponibile la risposta all'app client.

<a name="test"></a>

## <a name="test-cors"></a>Testare CORS

Per testare CORS:

1. [Creare un progetto API](xref:tutorials/first-web-api). In alternativa, è possibile [scaricare l'esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Abilitare CORS usando uno degli approcci descritti in questo documento. Ad esempio,

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` deve essere usato solo per il test di un'app di esempio simile al [codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)per il download.

1. Creare un progetto di app Web (Razor Pages o MVC). Nell'esempio viene utilizzato Razor Pages. È possibile creare l'app Web nella stessa soluzione del progetto API.
1. Aggiungere il codice evidenziato seguente al file *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. Nel codice precedente sostituire `url: 'https://<web app>.azurewebsites.net/api/values/1',` con l'URL dell'app distribuita.
1. Distribuire il progetto API. Ad esempio, [eseguire la distribuzione in Azure](xref:host-and-deploy/azure-apps/index).
1. Eseguire l'app Razor Pages o MVC dal desktop e fare clic sul pulsante **test** . Usare gli strumenti F12 per esaminare i messaggi di errore.
1. Rimuovere l'origine localhost da `WithOrigins` e distribuire l'app. In alternativa, eseguire l'app client con una porta diversa. Ad esempio, eseguire da Visual Studio.
1. Eseguire il test con l'app client. Gli errori CORS restituiscono un errore, ma il messaggio di errore non è disponibile per JavaScript. Usare la scheda console negli strumenti F12 per visualizzare l'errore. A seconda del browser, viene ricevuto un errore (nella console strumenti F12) simile al seguente:

   * Uso di Microsoft Edge:

     **SEC7120: [CORS] la `https://localhost:44375` di origine non ha trovato `https://localhost:44375` nell'intestazione della risposta Access-Control-Allow-Origin per la risorsa tra le origini in `https://webapi.azurewebsites.net/api/values/1`**

   * Uso di Chrome:

     **L'accesso a XMLHttpRequest in `https://webapi.azurewebsites.net/api/values/1` dalla `https://localhost:44375` di origine è stato bloccato dal criterio CORS: non è presente alcuna intestazione ' Access-Control-Allow-Origin ' nella risorsa richiesta.**
     
Gli endpoint abilitati per CORS possono essere testati con uno strumento, ad esempio [Fiddler](https://www.telerik.com/fiddler) o [postazione](https://www.getpostman.com/). Quando si utilizza uno strumento, l'origine della richiesta specificata dall'intestazione `Origin` deve essere diversa dall'host che riceve la richiesta. Se la richiesta non è *tra origini* basate sul valore dell'intestazione `Origin`:

* Non è necessario che il middleware CORS elabori la richiesta.
* Le intestazioni CORS non vengono restituite nella risposta.

## <a name="cors-in-iis"></a>CORS in IIS

Quando si esegue la distribuzione in IIS, CORS deve essere eseguito prima dell'autenticazione di Windows se il server non è configurato per consentire l'accesso anonimo. Per supportare questo scenario, è necessario installare e configurare il [modulo cors di IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) per l'app.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Condivisione di risorse tra le origini (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [Introduzione al modulo CORS di IIS](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end