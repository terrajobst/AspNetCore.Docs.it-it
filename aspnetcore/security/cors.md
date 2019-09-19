---
title: Abilitare le richieste tra le origini (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come CORS come standard per consentire o rifiutare le richieste tra origini in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a34b77ad799a00707048c923b82b48774ce91682
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108076"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Abilitare le richieste tra le origini (CORS) in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo articolo illustra come abilitare CORS in un'app ASP.NET Core.

La sicurezza del browser impedisce a una pagina Web di eseguire richieste a un dominio diverso da quello che ha gestito la pagina Web. Questa restrizione è detta *criterio della stessa origine*. Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito. In alcuni casi potrebbe essere necessario consentire ad altri siti di effettuare richieste tra le origini all'app. Per altre informazioni, vedere l' [articolo relativo a Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Condivisione di risorse tra le origini](https://www.w3.org/TR/cors/) (CORS):

* È uno standard W3C che consente a un server di ridurre i criteri della stessa origine.
* **Non** è una funzionalità di sicurezza, CORS rilassa la sicurezza. Un'API non è più sicura consentendo CORS. Per ulteriori informazioni, vedere [funzionamento di CORS](#how-cors).
* Consente a un server di consentire in modo esplicito alcune richieste tra origini rifiutando altre.
* È più sicuro e flessibile rispetto alle tecniche precedenti, ad esempio [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Stessa origine

Due URL hanno la stessa origine se hanno schemi, host e porte identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Questi due URL hanno la stessa origine:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Questi URL hanno origini diverse da quelle dei due URL precedenti:

* `https://example.net`&ndash; Dominio diverso
* `https://www.example.com/foo.html`&ndash; Sottodominio diverso
* `http://example.com/foo.html`&ndash; Schema diverso
* `https://example.com:9000/foo.html`&ndash; Porta diversa

Internet Explorer non considera la porta durante il confronto delle origini.

## <a name="cors-with-named-policy-and-middleware"></a>CORS con criteri e middleware denominati

Il middleware CORS gestisce le richieste tra le origini. Il codice seguente Abilita CORS per l'intera app con l'origine specificata:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Il codice precedente:

* Imposta il nome del criterio su\_"myAllowSpecificOrigins". Il nome del criterio è arbitrario.
* Chiama il <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metodo di estensione, che Abilita CORS.
* Chiama <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un' [espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). L'espressione lambda accetta <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> un oggetto. Le [Opzioni di configurazione](#cors-policy-options), `WithOrigins`ad esempio, sono descritte più avanti in questo articolo.

La <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> chiamata al metodo aggiunge i servizi CORS al contenitore del servizio dell'app:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Per ulteriori informazioni, vedere [Opzioni dei criteri CORS](#cpo) in questo documento.

Il <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> metodo può concatenare i metodi, come illustrato nel codice seguente:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Nota: L'URL **non** deve contenere una barra finale (`/`). Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.

::: moniker range=">= aspnetcore-3.0"

Il codice seguente applica i criteri di CORS a tutti gli endpoint di app tramite il middleware CORS:
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> Con il routing dell'endpoint è necessario configurare il middleware CORS per l'esecuzione tra le `UseRouting` chiamate `UseEndpoints`a e. Una configurazione non corretta causerà il corretto funzionamento del middleware.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
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
Nota: `UseCors` è necessario chiamare prima `UseMvc`.

::: moniker-end

Per applicare i criteri di CORS a livello di pagina/controller/azione [, vedere Enable CORS in Razor Pages, Controllers e Action Methods](#ecors) .

Vedere [test CORS](#test) per istruzioni sul test del codice precedente.

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a>Abilitare cors con routing degli endpoint

Con il routing degli endpoint, è possibile abilitare CORS in base all'endpoint usando il `RequireCors` set di metodi di estensione.

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
::: moniker-end

## <a name="enable-cors-with-attributes"></a>Abilita CORS con attributi

[ L'&lbrack;attributoEnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fornisce un'alternativa all'applicazione di CORS a livello globale. L' `[EnableCors]` attributo Abilita CORS per i punti finali selezionati, anziché tutti i punti finali.

Usare `[EnableCors]` per specificare i criteri predefiniti e `[EnableCors("{Policy String}")]` per specificare un criterio.

L' `[EnableCors]` attributo può essere applicato a:

* Pagina Razor`PageModel`
* Controller
* Metodo di azione del controller

È possibile applicare criteri diversi a controller/pagina-modello/azione con l' `[EnableCors]` attributo. Quando l' `[EnableCors]` attributo viene applicato a controller/modello di pagina/Metodo di azione e CORS è abilitato nel middleware, vengono applicati entrambi i criteri. È consigliabile evitare di combinare i criteri. Usare l' `[EnableCors]` attributo o il middleware, non entrambi nella stessa app.

Il codice seguente applica un criterio diverso a ogni metodo:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Il codice seguente crea un criterio predefinito CORS e un criterio denominato `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Disabilitare CORS

[ L'&lbrack;attributoDisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Disabilita CORS per il controller, il modello di pagina o l'azione.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opzioni dei criteri di CORS

In questa sezione vengono descritte le varie opzioni che è possibile impostare in un criterio CORS:

* [Imposta le origini consentite](#set-the-allowed-origins)
* [Impostare i metodi HTTP consentiti](#set-the-allowed-http-methods)
* [Impostare le intestazioni della richiesta consentita](#set-the-allowed-request-headers)
* [Impostare le intestazioni di risposta esposte](#set-the-exposed-response-headers)
* [Credenziali nelle richieste tra le origini](#credentials-in-cross-origin-requests)
* [Imposta la data di scadenza preliminare](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>viene chiamato in `Startup.ConfigureServices`. Per alcune opzioni, può essere utile leggere prima la sezione [come funziona CORS](#how-cors) .

## <a name="set-the-allowed-origins"></a>Imposta le origini consentite

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Consente le richieste CORS da tutte le origini con qualsiasi`http` schema `https`(o). &ndash; `AllowAnyOrigin`non è sicuro perché *qualsiasi sito Web* può effettuare richieste tra origini per l'app.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> La `AllowAnyOrigin` definizione `AllowCredentials` di e è una configurazione non sicura e può comportare la falsificazione della richiesta tra siti. Il servizio CORS restituisce una risposta CORS non valida quando un'app è configurata con entrambi i metodi.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> La `AllowAnyOrigin` definizione `AllowCredentials` di e è una configurazione non sicura e può comportare la falsificazione della richiesta tra siti. Per un'app sicura, specificare un elenco esatto di origini se il client deve autorizzare se stesso ad accedere alle risorse del server.

::: moniker-end

`AllowAnyOrigin`influiscono sulle richieste preliminari `Access-Control-Allow-Origin` e sull'intestazione. Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash; Imposta la<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> proprietà del criterio come funzione che consente alle origini di corrispondere a un dominio con caratteri jolly configurato durante la valutazione se l'origine è consentita.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Consente qualsiasi metodo HTTP:
* Influiscono sulle richieste preliminari `Access-Control-Allow-Methods` e sull'intestazione. Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni della richiesta consentita

Per consentire l'invio di intestazioni specifiche in una richiesta CORS, denominate *intestazioni richiesta autore*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Per consentire tutte le intestazioni di richiesta di <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>autore, chiamare:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Questa impostazione influiscono sulle richieste preliminari `Access-Control-Request-Headers` e sull'intestazione. Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .

::: moniker range=">= aspnetcore-2.2"

Un criterio middleware CORS corrisponde a intestazioni specifiche specificate da `WithHeaders` è possibile solo quando le intestazioni `Access-Control-Request-Headers` inviate corrispondono esattamente alle intestazioni indicate in `WithHeaders`.

Si consideri, ad esempio, un'app configurata come segue:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Il middleware CORS rifiuta una richiesta preliminare con la seguente intestazione di richiesta `Content-Language` perché ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) non è `WithHeaders`elencato in:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

L'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS. Pertanto, il browser non tenta di eseguire la richiesta tra origini.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Il `Access-Control-Request-Headers` middleware CORS consente sempre l'invio di quattro intestazioni in, indipendentemente dai valori configurati in CorsPolicy. Headers. Questo elenco di intestazioni include:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Si consideri, ad esempio, un'app configurata come segue:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Il middleware CORS risponde correttamente a una richiesta preliminare con la seguente intestazione di richiesta `Content-Language` perché è sempre consentita:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Impostare le intestazioni di risposta esposte

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app. Per ulteriori informazioni, vedere [la pagina relativa alla condivisione delle risorse tra le origini W3C (terminologia): Intestazione](https://www.w3.org/TR/cors/#simple-response-header)della risposta semplice.

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

Le credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser non invia le credenziali con una richiesta tra le origini. Le credenziali includono i cookie e gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta tra le origini, il client deve `XMLHttpRequest.withCredentials` impostare `true`su.

Utilizzando `XMLHttpRequest` direttamente:

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

Il server deve consentire le credenziali. Per consentire le credenziali tra le origini, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>chiamare:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La risposta HTTP include un' `Access-Control-Allow-Credentials` intestazione che indica al browser che il server consente le credenziali per una richiesta tra le origini.

Se il browser invia le credenziali, ma la risposta non include `Access-Control-Allow-Credentials` un'intestazione valida, il browser non espone la risposta all'app e la richiesta tra le origini ha esito negativo.

Consentire le credenziali tra le origini costituisce un rischio per la sicurezza. Un sito Web in un altro dominio può inviare le credenziali dell'utente che ha eseguito l'accesso all'app per conto dell'utente senza la conoscenza dell'utente. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

La specifica CORS indica inoltre che l'impostazione delle `"*"` origini su (tutte le origini) non `Access-Control-Allow-Credentials` è valida se l'intestazione è presente.

### <a name="preflight-requests"></a>Richieste preliminari

Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva. Questa richiesta è detta *richiesta preliminare*. Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:

* Il metodo di richiesta è GET, HEAD o POST.
* L'app non imposta intestazioni di richiesta diverse `Accept`da `Accept-Language`, `Content-Language`, `Content-Type`, o `Last-Event-ID`.
* L' `Content-Type` intestazione, se impostata, presenta uno dei valori seguenti:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La regola sulle intestazioni di richiesta impostate per la richiesta del client si applica alle intestazioni impostate dall'app `setRequestHeader` chiamando `XMLHttpRequest` sull'oggetto. La specifica CORS chiama queste intestazioni di *richiesta autore*intestazioni. La regola non si applica alle intestazioni che il browser può impostare, `User-Agent`ad `Host`esempio, `Content-Length`o.

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

* `Access-Control-Request-Method`: Metodo HTTP che verrà utilizzato per la richiesta effettiva.
* `Access-Control-Request-Headers`: Elenco di intestazioni di richiesta impostate dall'app sulla richiesta effettiva. Come indicato in precedenza, non include le intestazioni impostate dal browser, ad esempio `User-Agent`.

Una richiesta preliminare CORS può includere un' `Access-Control-Request-Headers` intestazione, che indica al server le intestazioni inviate con la richiesta effettiva.

Per consentire intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Per consentire tutte le intestazioni di richiesta di <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>autore, chiamare:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

I browser non sono completamente coerenti nel modo `Access-Control-Request-Headers`in cui vengono impostati. Se si impostano le intestazioni su `"*"` un valore diverso <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>da (o si utilizza), `Accept`è `Content-Type`necessario includere `Origin`almeno, e, più eventuali intestazioni personalizzate che si desidera supportare.

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

La risposta include un' `Access-Control-Allow-Methods` intestazione che elenca i metodi consentiti e, `Access-Control-Allow-Headers` facoltativamente, un'intestazione, che elenca le intestazioni consentite. Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.

Se la richiesta preliminare viene negata, l'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS. Pertanto, il browser non tenta di eseguire la richiesta tra origini.

### <a name="set-the-preflight-expiration-time"></a>Imposta la data di scadenza preliminare

L' `Access-Control-Max-Age` intestazione specifica per quanto tempo la risposta alla richiesta preliminare può essere memorizzata nella cache. Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

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
    * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
    * Un Web browser immettendo l'URL nella barra degli indirizzi.
* Si tratta di un modo per consentire a un server di consentire ai browser di eseguire una richiesta dell' [API di recupero](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) o di [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) tra le origini, che altrimenti sarebbe proibita.
  * I browser (senza CORS) non possono eseguire richieste tra le origini. Prima di CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) è stato usato per aggirare questa restrizione. JSONP non usa XHR, usa il `<script>` tag per ricevere la risposta. Gli script possono essere caricati tra le origini.

La [specifica CORS](https://www.w3.org/TR/cors/) ha introdotto diverse nuove intestazioni HTTP che consentono richieste tra le origini. Se un browser supporta CORS, queste intestazioni vengono impostate automaticamente per le richieste tra le origini. Il codice JavaScript personalizzato non è necessario per abilitare CORS.

Di seguito è riportato un esempio di richiesta tra le origini. L' `Origin` intestazione fornisce il dominio del sito che sta effettuando la richiesta:

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

Se il server consente la richiesta, imposta l' `Access-Control-Allow-Origin` intestazione nella risposta. Il valore di questa intestazione corrisponde all' `Origin` intestazione della richiesta o è il valore `"*"`del carattere jolly, ovvero qualsiasi origine è consentita:

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

Se la risposta non include l' `Access-Control-Allow-Origin` intestazione, la richiesta tra le origini ha esito negativo. In particolare, il browser non consente la richiesta. Anche se il server restituisce una risposta con esito positivo, il browser non rende disponibile la risposta all'app client.

<a name="test"></a>

## <a name="test-cors"></a>Test CORS

Per testare CORS:

1. [Creare un progetto API](xref:tutorials/first-web-api). In alternativa, è possibile [scaricare l'esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Abilitare CORS usando uno degli approcci descritti in questo documento. Ad esempio:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");`deve essere usato solo per il test di un'app di esempio simile al [codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)per il download.

1. Creare un progetto di app Web (Razor Pages o MVC). Nell'esempio viene utilizzato Razor Pages. È possibile creare l'app Web nella stessa soluzione del progetto API.
1. Aggiungere il codice evidenziato seguente al file *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. Nel codice precedente sostituire `url: 'https://<web app>.azurewebsites.net/api/values/1',` con l'URL dell'app distribuita.
1. Distribuire il progetto API. Ad esempio, [eseguire la distribuzione in Azure](xref:host-and-deploy/azure-apps/index).
1. Eseguire l'app Razor Pages o MVC dal desktop e fare clic sul pulsante **test** . Usare gli strumenti F12 per esaminare i messaggi di errore.
1. Rimuovere l'origine localhost da `WithOrigins` e distribuire l'app. In alternativa, eseguire l'app client con una porta diversa. Ad esempio, eseguire da Visual Studio.
1. Eseguire il test con l'app client. Gli errori CORS restituiscono un errore, ma il messaggio di errore non è disponibile per JavaScript. Usare la scheda console negli strumenti F12 per visualizzare l'errore. A seconda del browser, viene ricevuto un errore (nella console strumenti F12) simile al seguente:

   * Uso di Microsoft Edge:

     **SEC7120: [CORS] l'origine `https://localhost:44375` non è stata `https://localhost:44375` trovata nell'intestazione della risposta Access-Control-Allow-Origin per la risorsa tra le origini in`https://webapi.azurewebsites.net/api/values/1`**

   * Uso di Chrome:

     **L'accesso a XMLHttpRequest `https://webapi.azurewebsites.net/api/values/1` da Origin `https://localhost:44375` è stato bloccato dal criterio CORS: Non è presente alcuna intestazione ' Access-Control-Allow-Origin ' nella risorsa richiesta.**

## <a name="additional-resources"></a>Risorse aggiuntive

* [Condivisione di risorse tra le origini (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
