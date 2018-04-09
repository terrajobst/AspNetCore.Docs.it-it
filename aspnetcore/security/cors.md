---
title: Abilitare le richieste tra le origini (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come CORS come standard per consentire o rifiutare le richieste cross-origin in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 3c5d0840426c7ed52353a7832a1a1959027121de
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Abilitare le richieste tra le origini (CORS) in ASP.NET Core

Da [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)

Protezione del browser impedisce che una pagina web effettua le richieste AJAX a un altro dominio. Questa restrizione viene chiamata il *criteri stessa origine*e impedisce a un sito di leggere i dati sensibili da un altro sito. Tuttavia, in alcuni casi è consigliabile consentire ad altri siti richieste cross-origin per l'API web.

[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) è uno standard W3C che consente a un server per ridurre i criteri della stessa origine. Tramite condivisione CORS, un server può consentire in modo esplicito alcune richieste cross-origin durante il rifiuto di altri utenti. CORS è più sicura e più flessibile di tecniche precedenti, ad esempio [JSONP](https://wikipedia.org/wiki/JSONP). In questo argomento viene illustrato come abilitare CORS in un'applicazione ASP.NET Core.

## <a name="what-is-same-origin"></a>Che cos'è "stessa origine"?

Se dispongono di porte, host e gli schemi identici, due URL abbia la stessa origine. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Questi due URL abbia la stessa origine:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Questi URL sono origini diverse rispetto a quelli due:

* `http://example.net` -Altro dominio

* `http://www.example.com/foo.html` -Sottodominio diverso

* `https://example.com/foo.html` -Schema differente

* `http://example.com:9000/foo.html` -Porta diversa

> [!NOTE]
> Internet Explorer non viene considerata la porta quando si confrontano le origini.

## <a name="setting-up-cors"></a>Impostazione di CORS

Per impostare CORS per l'applicazione aggiungere le `Microsoft.AspNetCore.Cors` pacchetto al progetto.

Aggiungere i servizi CORS in Startup.cs:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Abilitazione di CORS con middleware

Per abilitare CORS per l'intera applicazione, aggiungere il middleware CORS la richiesta di pipeline utilizzando il `UseCors` metodo di estensione. Si noti che il middleware CORS deve precedere qualsiasi endpoint definiti nell'applicazione che si desidera supportare le richieste di multiorigine (ad esempio, prima di qualsiasi chiamata a `UseMvc`).

È possibile specificare un criterio di cross-origin quando si aggiunge il middleware CORS tramite la `CorsPolicyBuilder` classe. È possibile ottenere questo risultato in due modi. Il primo consiste nel chiamare UseCors con un'espressione lambda:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Nota:** l'URL deve essere specificato senza una barra finale (`/`). Se l'URL termina con `/`, verrà restituito il confronto `false` e non verrà restituita alcuna intestazione.

L'espressione lambda ha un `CorsPolicyBuilder` oggetto. È disponibile un elenco di [opzioni di configurazione](#cors-policy-options) più avanti in questo argomento. In questo esempio, i criteri consentono le richieste tra le origini da `http://example.com` e non altre origini.

Si noti che CorsPolicyBuilder presenta un'API intuitiva, pertanto è possibile concatenare le chiamate al metodo:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

Il secondo approccio consiste nel definire uno o più criteri CORS denominati e quindi selezionare i criteri in base al nome in fase di esecuzione.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Questo esempio aggiunge un criterio CORS denominato "AllowSpecificOrigin". Per selezionare i criteri, passare il nome di `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Abilitazione di CORS in MVC

In alternativa, è possibile utilizzare MVC per applicare specifiche CORS per ogni azione, per ogni controller o a livello globale per tutti i controller. Quando si utilizza MVC per abilitare CORS vengono utilizzati gli stessi servizi CORS, ma non il middleware CORS.

### <a name="per-action"></a>Per ogni azione

Per specificare un criterio CORS per un'azione specifica aggiungere il `[EnableCors]` attributo all'azione. Specificare il nome del criterio.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Per ogni controller

Per specificare il criterio CORS per un controller specifico aggiungere il `[EnableCors]` attributo alla classe controller. Specificare il nome del criterio.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>A livello globale

È possibile abilitare a livello globale CORS per tutti i controller mediante l'aggiunta di `CorsAuthorizationFilterFactory` filtro all'insieme di filtri globali:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

L'ordine di precedenza è: azione, controller, globale. A livello di azione criteri hanno la precedenza sui criteri a livello di controller e i criteri a livello di controller hanno la precedenza sui criteri globali.

### <a name="disable-cors"></a>Disabilitare CORS

Per disabilitare CORS per un controller o un'azione, utilizzare il `[DisableCors]` attributo.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Opzioni dei criteri CORS

Questa sezione descrive le varie opzioni che è possibile impostare un criterio CORS.

* [Impostare le origini consentite](#set-the-allowed-origins)

* [Impostare i metodi HTTP consentiti](#set-the-allowed-http-methods)

* [Impostare le intestazioni di richieste consentito](#set-the-allowed-request-headers)

* [Impostare le intestazioni di risposta esposto](#set-the-exposed-response-headers)

* [Credenziali in richieste cross-origin](#credentials-in-cross-origin-requests)

* [Impostare l'ora di scadenza preliminare](#set-the-preflight-expiration-time)

Per alcune opzioni può essere utile leggere [funziona come CORS](#how-cors-works) prima.

### <a name="set-the-allowed-origins"></a>Impostare le origini consentite

Per consentire a uno o più origini specifiche:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Per consentire tutte le origini:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Valutare con attenzione prima di consentire le richieste provenienti da qualsiasi origine. Significa che letteralmente qualsiasi sito Web di effettuare chiamate AJAX all'API.

### <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

Per consentire a tutti i metodi HTTP:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Questo influisce sulle richieste preliminari e intestazione Access-controllo-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni di richieste consentito

Una richiesta preliminare CORS potrebbe includere un'intestazione Access-Control-Request-Headers, elenco di intestazioni HTTP impostate dall'applicazione (la cosiddetta "author intestazioni delle richieste").

A intestazioni specifiche whitelist:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Per consentire tutti creare intestazioni di richiesta:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Browser non sono completamente coerenti in modalità impostano Access-Control-Request-Headers. Se si imposta le intestazioni a un valore diverso da "*", è necessario includere almeno "accetta", "content-type" e "origine", più eventuali intestazioni personalizzate che si desidera supportare.

### <a name="set-the-exposed-response-headers"></a>Impostare le intestazioni di risposta esposto

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione. (Vedere [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:

* Cache-Control

* Content-Language

* Content-Type

* Scadenza

* Ultima modifica

* Pragma

Le specifiche CORS chiama questi *le intestazioni di risposta semplice*. Per rendere disponibili per l'applicazione altre intestazioni:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Credenziali in richieste cross-origin

Credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser non invia credenziali con una richiesta multiorigine. Le credenziali includono i cookie, nonché gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta multiorigine, il client deve impostare XMLHttpRequest.withCredentials su true.

Utilizzando XMLHttpRequest direttamente:

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

Inoltre, il server deve concedere le credenziali. Per consentire le credenziali di cross-origin:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Ora la risposta HTTP includerà un'intestazione controllo-Consenti-le credenziali di accesso, che indica al browser che il server consenta le credenziali per una richiesta multiorigine.

Se il browser invia le credenziali, ma la risposta non include un'intestazione controllo-Consenti-le credenziali di accesso valida, il browser non esporre la risposta all'applicazione e la richiesta AJAX non riesce.

Prestare attenzione quando si utilizza le credenziali di cross-origin. Un sito Web in un altro dominio può inviare le credenziali dell'utente connesso all'app per conto dell'utente senza il consenso dell'utente. La specifica di CORS indica anche tale impostazione origine da "*" (tutte le origini) non è valido se il `Access-Control-Allow-Credentials` intestazione è presente.

### <a name="set-the-preflight-expiration-time"></a>Impostare l'ora di scadenza preliminare

L'intestazione di controllo accesso-Max-Age specifica quanto tempo può essere memorizzati nella cache la risposta alla richiesta preliminare. Per impostare questa intestazione:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Funzionamento di CORS

In questa sezione descrive cosa accade in una richiesta CORS al livello di messaggi HTTP. È importante comprendere come funziona la condivisione CORS in modo che il criterio CORS può essere configurato correttamente e risolverli verificarsi comportamenti imprevisti.

La specifica CORS introduce diverse nuove intestazioni HTTP che consentono di richieste cross-origin. Se un browser supporta la condivisione CORS, imposta le intestazioni automaticamente per le richieste cross-origin. Il codice JavaScript personalizzato non è necessario abilitare CORS.

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

Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin nella risposta. Il valore di questa intestazione corrisponde all'intestazione di origine dalla richiesta o è il valore del carattere jolly "*", che significa che è consentita qualsiasi origine:

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

Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX. In particolare, il browser non consente la richiesta. Anche se il server restituisce una risposta con esito positivo, il browser non verificare la risposta disponibili per l'applicazione client.

### <a name="preflight-requests"></a>Richieste preliminari

Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, denominata "richiesta preliminare," prima dell'invio della richiesta per la risorsa effettiva. Il browser è possibile ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:

* Il metodo di richiesta è GET, HEAD o POST, e

* L'applicazione non imposta le intestazioni di richiesta diverso da Accept, Accept-Language, Content-Language, Content-Type o ID di evento Last, e

* L'intestazione Content-Type (se impostato) è uno dei seguenti:

  * application/x-www-form-urlencoded

  * multipart/form-data

  * Testo

Si applica la regola sulle intestazioni di richiesta per le intestazioni che l'applicazione imposta chiamando setRequestHeader sull'oggetto XMLHttpRequest. (Specifica CORS chiama queste "intestazioni di richiesta autore"). La regola non si applica alle intestazioni in cui è possibile impostare il browser, ad esempio User-Agent, l'Host o Content-Length.

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

* Access-Control-Request-Method: Il metodo HTTP utilizzato per la richiesta effettiva.

* Access-Control-Request-Headers: Un elenco di intestazioni di richiesta che l'applicazione è impostata su richiesta effettiva. (Nuovamente, questo non include le intestazioni che imposta il browser.)

Di seguito è riportata una risposta di esempio, supponendo che il server consenta la richiesta:

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

La risposta include un'intestazione Access-controllo-Allow-Methods che elenca i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Consenti-Headers, che elenca le intestazioni consentite. Se la richiesta preliminare ha esito positivo, il browser invia la richiesta, come descritto in precedenza.
