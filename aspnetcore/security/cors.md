---
title: Abilitare le richieste Multiorigine (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come condivisione CORS come standard per consentire o rifiutare le richieste multiorigine in un'app ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2018
ms.locfileid: "41837485"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Abilitare le richieste Multiorigine (CORS) in ASP.NET Core

Dal [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)

Protezione del browser impedisce a una pagina web da creare richieste AJAX a un altro dominio. Questa restrizione viene chiamata il *criterio della stessa origine*e impedisce a un sito dannoso di leggere dati sensibili da un altro sito. Tuttavia, in alcuni casi si potrebbe voler consentire ad altri siti di effettuare richieste multiorigine nell'API web.

[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (le origini CORS) è uno standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine. Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre. CORS è più sicuri e flessibili rispetto a tecniche precedenti, ad esempio [JSONP](https://wikipedia.org/wiki/JSONP). Questo argomento illustra come abilitare CORS in un'applicazione ASP.NET Core.

## <a name="what-is-same-origin"></a>Che cos'è "origin stesso"?

Due URL abbia la stessa origine se dispongono di porte, host e gli schemi identici. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Questi due URL abbia la stessa origine:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Questi URL sono le entità Origin diversa rispetto al precedente due:

* `http://example.net` -Altro dominio

* `http://www.example.com/foo.html` -Sottodominio diverso

* `https://example.com/foo.html` -Schema differente

* `http://example.com:9000/foo.html` -Porta diversa

> [!NOTE]
> Internet Explorer non considera la porta quando si confrontano le entità Origin.

## <a name="enable-cors"></a>Abilitare CORS

::: moniker range="<= aspnetcore-1.1"

Per configurare CORS per l'applicazione, aggiungere il `Microsoft.AspNetCore.Cors` pacchetto al progetto.

::: moniker-end

Chiamare [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>L'abilitazione di CORS con il middleware

Per abilitare CORS, aggiungere il middleware CORS alla pipeline richiesta utilizzando il `UseCors` metodo di estensione. Il middleware CORS deve precedere qualsiasi endpoint definito nell'app in cui si desidera supportare richieste multiorigine (ad esempio, prima di qualsiasi chiamata a `UseMvc`).

È possibile specificare un criteri multiorigine quando si aggiunge il middleware CORS usando la [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) classe. È possibile ottenere questo risultato in due modi. Il primo consiste nel chiamare `UseCors` con un'espressione lambda:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Nota:** deve specificare l'URL senza una barra finale (`/`). Se l'URL termina con `/`, verrà restituito il confronto `false` e non verrà restituita alcuna intestazione.

L'operatore lambda accetta un `CorsPolicyBuilder` oggetto. Troverai un elenco del [opzioni di configurazione](#cors-policy-options) più avanti in questo argomento. In questo esempio, il criterio consente le richieste multiorigine dal `http://example.com` e non le entità Origin.

CorsPolicyBuilder ha un'API fluent, pertanto è possibile concatenare chiamate al metodo:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

Il secondo approccio consiste nel definire uno o più criteri CORS denominati e quindi selezionare il criterio in base al nome in fase di esecuzione.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Questo esempio viene aggiunto un criterio CORS denominato "AllowSpecificOrigin". Per selezionare i criteri, passare il nome da `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Abilitazione della condivisione CORS in MVC

In alternativa, è possibile usare MVC per applicare CORS specifici per ogni azione, per ogni controller o a livello globale per tutti i controller. Quando si usa MVC per abilitare CORS vengono usati gli stessi servizi CORS, ma non è il middleware CORS.

### <a name="per-action"></a>Per ogni azione

Per specificare un criterio CORS per un'azione specifica, aggiungere il `[EnableCors]` attributo all'azione. Specificare il nome del criterio.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Per ogni controller

Per specificare i criteri CORS per un controller specifico, aggiungere il `[EnableCors]` attributo alla classe controller. Specificare il nome del criterio.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>A livello globale

È possibile abilitare a livello globale CORS per tutti i controller mediante l'aggiunta di `CorsAuthorizationFilterFactory` filtro alla raccolta di filtri globali:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

L'ordine di precedenza è: azione, controller, globale. I criteri a livello di azione hanno la precedenza sui criteri a livello di controller e i criteri a livello di controller hanno la precedenza sui criteri globali.

### <a name="disable-cors"></a>Disabilitare la condivisione CORS

Per disabilitare CORS per un controller o azione, usare il `[DisableCors]` attributo.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Opzioni dei criteri CORS

Questa sezione descrive le varie opzioni che è possibile impostare in un criterio CORS.

* [Impostare le origini consentite](#set-the-allowed-origins)

* [Impostare i metodi HTTP consentiti](#set-the-allowed-http-methods)

* [Impostare le intestazioni di richieste consentite](#set-the-allowed-request-headers)

* [Impostare le intestazioni di risposta esposto](#set-the-exposed-response-headers)

* [Credenziali in richieste multiorigine](#credentials-in-cross-origin-requests)

* [Impostare l'ora di scadenza preliminare](#set-the-preflight-expiration-time)

Per alcune opzioni, potrebbe essere utile leggere [funziona come CORS](#how-cors-works) prima.

### <a name="set-the-allowed-origins"></a>Impostare le origini consentite

Per consentire a uno o più origini specifiche:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Consentire tutte le origini:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Si consideri con attenzione prima di consentire le richieste provenienti da qualsiasi origine. Significa che letteralmente qualsiasi sito Web può eseguire chiamate AJAX all'API.

### <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

Per consentire a tutti i metodi HTTP:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Questo influisce sulle richieste preliminari e dell'intestazione Access-Control-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni di richieste consentite

Una richiesta CORS preliminare può includere un'intestazione Access-Control-Request-Headers, elenca le intestazioni HTTP impostate dall'applicazione (la cosiddetta "creare le intestazioni delle richieste").

Alle intestazioni specifiche whitelist:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Per consentire tutti gli autori il intestazioni della richiesta:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

I browser non sono completamente coerenti nel modo in cui siano impostate Access-Control-Request-Headers. Se si imposta le intestazioni a qualsiasi elemento diverso da "*", è necessario includere almeno "accept", "content-type" e "origin", oltre a eventuali intestazioni personalizzate che si desidera supportare.

### <a name="set-the-exposed-response-headers"></a>Impostare le intestazioni di risposta esposto

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione. (Vedere [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:

* Cache-Control

* Content-Language

* Content-Type

* Alla scadenza

* Ultima modifica

* (Pragma)

La specifica CORS chiama questi *intestazioni di risposta semplice*. Per rendere disponibili per l'applicazione altre intestazioni:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Credenziali in richieste multiorigine

Credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser non invia credenziali con una richiesta multiorigine. Le credenziali includono i cookie, nonché gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta multiorigine, il client deve impostare XMLHttpRequest.withCredentials su true.

Uso diretto di XMLHttpRequest:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

In jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Inoltre, il server deve consentire le credenziali. Per consentire le credenziali di cross-origin:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Ora la risposta HTTP includerà un'intestazione di accesso-controllo-Allow-Credentials, che indica al browser che il server consenta le credenziali per una richiesta multiorigine.

Se il browser invia le credenziali, ma la risposta non include un'intestazione di accesso-controllo-Allow-Credentials valida, il browser non espone la risposta all'applicazione e la richiesta AJAX non riesce.

Prestare attenzione quando si consente le credenziali di cross-origin. Un sito Web all'indirizzo di un altro dominio può inviare le credenziali dell'utente ha eseguito l'accesso all'app per conto dell'utente senza il consenso dell'utente. La specifica CORS indica anche che l'impostazione origini `"*"` (tutte le origini) non è valido se la `Access-Control-Allow-Credentials` intestazione è presente.

### <a name="set-the-preflight-expiration-time"></a>Impostare l'ora di scadenza preliminare

L'intestazione di accesso-controllo-Max-Age specifica quanto tempo può essere memorizzati nella cache la risposta alla richiesta preliminare. Per impostare questa intestazione:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Funzionamento della condivisione CORS

In questa sezione viene descritto cosa succede in una richiesta CORS a livello di messaggi HTTP. È importante comprendere il funzionamento di CORS in modo che il criterio CORS può essere configurato correttamente e il debug quando si verificano comportamenti imprevisti.

La specifica CORS introduce diverse nuove intestazioni HTTP che attivare richieste multiorigine. Se un browser supporta la condivisione CORS, imposta le intestazioni delle automaticamente per le richieste multiorigine. Il codice JavaScript personalizzato non è necessario abilitare CORS.

Di seguito è riportato un esempio di una richiesta multiorigine. Il `Origin` intestazione fornisce il dominio del sito che effettua la richiesta:

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin nella risposta. Il valore di questa intestazione corrisponde all'intestazione di origine della richiesta o è il valore del carattere jolly "*", che significa che è consentita qualsiasi origine:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX non riesce. In particolare, il browser non consente la richiesta. Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'applicazione client.

### <a name="preflight-requests"></a>Richieste preliminari

Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, definita "richiesta preliminare," prima dell'invio della richiesta effettiva per la risorsa. Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:

* Il metodo di richiesta è GET, HEAD o POST, e

* L'applicazione non imposta le intestazioni di richiesta diversa da Accept, Accept-Language, Content-Language, Content-Type o Last-eventi-ID, e

* L'intestazione Content-Type (se impostato) è uno dei seguenti:

  * application/x-www-form-urlencoded

  * multipart/form-data

  * text/plain

Si applica la regola sulle intestazioni di richiesta alle intestazioni che l'applicazione imposta chiamando setRequestHeader sull'oggetto XMLHttpRequest. (Specifica CORS chiama questi "intestazioni di richiesta autore"). La regola non si applica alle intestazioni in cui è possibile impostare il browser, ad esempio User-Agent, Host o Content-Length.

Di seguito è riportato un esempio di una richiesta preliminare:

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP. Include due intestazioni speciali:

* Access-Control-Request-Method: Metodo HTTP che verrà usato per la richiesta effettiva.

* Access-Control-Request-Headers: Un elenco di intestazioni di richiesta che l'applicazione è impostata su richiesta effettiva. (Anche in questo caso, ciò non include le intestazioni che il browser imposta).

Ecco un esempio di risposta, presupponendo che il server consente la richiesta:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

La risposta include un'intestazione Access-Control-Allow-Methods in cui sono elencati i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Consenti-Headers, che elenca le intestazioni consentite. Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva, come descritto in precedenza.
