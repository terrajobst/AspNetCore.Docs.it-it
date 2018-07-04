---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Abilitazione di richieste Multiorigine nell'API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Viene illustrato come il supporto di condivisione delle risorse Multiorigine (CORS) nell'API Web ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 603eb55f8fa0b629d0287b66086b9495ef55faf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381089"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Abilitazione di richieste Multiorigine nell'API Web ASP.NET 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Protezione del browser impedisce a una pagina web da creare richieste AJAX a un altro dominio. Questa restrizione viene chiamata il *criterio della stessa origine*e impedisce a un sito dannoso di leggere dati sensibili da un altro sito. Tuttavia, in alcuni casi si potrebbe voler consentire ad altri siti di chiamare l'API web.
> 
> [Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (le origini CORS) è uno standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine. Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre. CORS è più sicuri e flessibili rispetto a tecniche precedenti, ad esempio [JSONP](http://en.wikipedia.org/wiki/JSONP). Questa esercitazione illustra come abilitare CORS nell'applicazione API Web.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - API Web 2.2


<a id="intro"></a>
## <a name="introduction"></a>Introduzione

Questa esercitazione illustra che il supporto di CORS nell'API Web ASP.NET. Si inizierà creando due progetti ASP.NET – uno chiamato "servizio Web", che ospita un controller API Web, e l'altro denominato "WebClient", che chiama WebService. Poiché le due applicazioni sono ospitate in domini diversi, una richiesta AJAX da WebClient per servizio Web è una richiesta multiorigine.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Che cos'è "Origin stesso"?

Due URL abbia la stessa origine se dispongono di porte, host e gli schemi identici. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Questi due URL abbia la stessa origine:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Questi URL sono le entità Origin diversa rispetto al precedente due:

- `http://example.net` -Altro dominio
- `http://example.com:9000/foo.html` -Porta diversa
- `https://example.com/foo.html` -Schema differente
- `http://www.example.com/foo.html` -Sottodominio diverso

> [!NOTE]
> Internet Explorer non considera la porta quando si confrontano le entità Origin.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Creare il progetto servizio Web

> [!NOTE]
> In questa sezione presuppone che si conosca già come creare progetti API Web. In caso contrario, vedere [Introduzione all'API Web ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Avviare Visual Studio e creare una nuova **applicazione Web ASP.NET** progetto. Selezionare il **vuoto** modello di progetto. In "Aggiungere cartelle e riferimenti per la funzionalità di base", selezionare la **API Web** casella di controllo. Facoltativamente, selezionare l'opzione "Ospita nel Cloud" per distribuire l'app in Microsoft Azure. Microsoft offre l'hosting web gratuito per fino a 10 siti Web in un [account Azure gratuito](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Aggiungere un controller API Web denominato `TestController` con il codice seguente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

È possibile eseguire l'applicazione in locale o distribuirla in Azure. (Per le schermate contenute in questa esercitazione, distribuito in App Web di servizio App di Azure.) Per verificare che l'API web è operativa, passare a `http://hostname/api/test/`, dove *hostname* è il dominio in cui è distribuita l'applicazione. Il testo di risposta, dovrebbe &quot;OTTIENI: messaggio di prova&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Creare il progetto WebClient

Creare un altro progetto di applicazione Web ASP.NET e selezionare il **MVC** modello di progetto. Facoltativamente, selezionare **Modifica autenticazione** > **Nessuna autenticazione**. L'autenticazione non è necessario per questa esercitazione.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

In Esplora soluzioni aprire il file Views/Home/Index.cshtml. Sostituire il codice in questo file con il codice seguente:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Per il *serviceUrl* variabile, usare l'URI del servizio Web app. A questo punto eseguire localmente l'app WebClient o pubblicarla in un altro sito Web.

Scegliendo il pulsante "prova" Invia una richiesta AJAX all'app, servizio Web utilizzando il metodo HTTP elencato nella casella a discesa (GET, POST o PUT). Questo modo è possibile esaminare le diverse richieste multiorigine. Al momento, l'app del servizio Web non supportano CORS, in modo che se si fa clic sul pulsante, si otterrà un errore.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Se si osserva il traffico HTTP in uno strumento, ad esempio [Fiddler](http://www.telerik.com/fiddler), si noterà che il browser invia la richiesta GET e la richiesta ha esito positivo, ma la chiamata AJAX restituisce un errore. È importante comprendere che Criteri di corrispondenza dell'origine non impediscano il browser dal *invio* la richiesta. Al contrario, impedisce all'applicazione di visualizzare il *risposta*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Abilitare CORS

A questo punto è possibile abilitare CORS nell'app di servizio Web. In primo luogo, aggiungere il pacchetto NuGet di CORS. In Visual Studio dal **degli strumenti** dal menu **Library Package Manager**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, digitare il comando seguente:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Questo comando installa il pacchetto più recente e aggiorna tutte le dipendenze, incluse le librerie di API Web di base. Utente-flag di versione come destinazione una versione specifica. Il pacchetto CORS richiede Web API 2.0 o versione successiva.

Aprire il file App\_Start/WebApiConfig.cs. Aggiungere il codice seguente per il **webapiconfig. Register** (metodo).

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Successivamente, aggiungere il **[EnableCors]** dell'attributo di `TestController` classe:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Per il *le entità Origin* parametro, usare l'URI in cui è distribuita l'applicazione WebClient. In questo modo le richieste multiorigine da WebClient, mentre comunque impedire tutte le altre richieste tra domini. In un secondo momento, descriverò i parametri per **[EnableCors]** in modo più dettagliato.

Non includere una barra rovesciata alla fine del *le entità Origin* URL.

Ridistribuire l'applicazione di servizio Web aggiornato. Non devi aggiornare WebClient. A questo punto la richiesta AJAX da WebClient avrà esito positivo. Sono consentiti tutti i metodi GET, PUT e POST.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Funzionamento della condivisione CORS

In questa sezione viene descritto cosa succede in una richiesta CORS, il livello dei messaggi HTTP. È importante comprendere il funzionamento di CORS, in modo che sia possibile configurare il **[EnableCors]** attributo correttamente e risolvere i problemi se ciò non funziona come previsto.

La specifica CORS introduce diverse nuove intestazioni HTTP che attivare richieste multiorigine. Se un browser supporta la condivisione CORS, imposta le intestazioni automaticamente per le richieste multiorigine. non è necessario eseguire alcuna operazione speciale nel codice JavaScript.

Di seguito è riportato un esempio di una richiesta multiorigine. L'intestazione "Origin" offre il dominio del sito che effettua la richiesta.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin. Il valore di questa intestazione corrisponde all'intestazione di origine o è il valore del carattere jolly "\*", che significa che è consentita qualsiasi origine.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX non riesce. In particolare, il browser non consente la richiesta. Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'applicazione client.

**Richieste preliminari**

Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, definita "richiesta preliminare," prima dell'invio della richiesta effettiva per la risorsa.

Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:

- Il metodo di richiesta è GET, HEAD o POST, *e*
- L'applicazione non imposta le intestazioni di richiesta diversa da Accept, Accept-Language, Content-Language, Content-Type o Last-eventi-ID, *e*
- L'intestazione Content-Type (se impostato) è uno dei seguenti: 

    - application/x-www-form-urlencoded
    - multipart/form-data
    - text/plain

Si applica la regola sulle intestazioni di richiesta alle intestazioni che l'applicazione imposta chiamando **setRequestHeader** nel **XMLHttpRequest** oggetto. (Specifica CORS chiama questi "intestazioni di richiesta autore"). La regola non si applica alle intestazioni il *browser* impostare, ad esempio User-Agent, Host o Content-Length.

Di seguito è riportato un esempio di una richiesta preliminare:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP. Include due intestazioni speciali:

- Access-Control-Request-Method: Metodo HTTP che verrà usato per la richiesta effettiva.
- Access-Control-Request-Headers: Un elenco di intestazioni di richiesta che il *applicazione* impostare per la richiesta effettiva. (Anche in questo caso, ciò non includono le intestazioni che imposta il browser.)

Ecco un esempio di risposta, presupponendo che il server consente la richiesta:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

La risposta include un'intestazione Access-Control-Allow-Methods in cui sono elencati i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Consenti-Headers, che elenca le intestazioni consentite. Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva, come descritto in precedenza.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Regole di ambito per [EnableCors]

È possibile abilitare CORS per ogni azione, per ogni controller o a livello globale per tutti i controller API Web nell'applicazione.

**Per ogni azione**

Per abilitare CORS per una singola azione, impostare il **[EnableCors]** attributo del metodo di azione. L'esempio seguente Abilita CORS per il `GetItem` solo metodo.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Per ogni Controller**

Se si imposta **[EnableCors]** nella classe controller, si applica a tutte le azioni nel controller. Per disabilitare CORS per un'azione, aggiungere il **[DisableCors]** attributo all'azione. L'esempio seguente Abilita CORS per qualsiasi metodo ad eccezione `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**A livello globale**

Per abilitare CORS per tutti i controller API Web nell'applicazione, passare un **EnableCorsAttribute** dell'istanza per il **EnableCors** metodo:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Se si imposta l'attributo più di un ambito, l'ordine di precedenza è:

1. Operazione
2. Controller
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Impostare le origini consentite

Il *le entità Origin* parametro delle **[EnableCors]** attributo specifica quali origini sono consentite per accedere alla risorsa. Il valore è un elenco delimitato da virgole di origini consentite.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

È anche possibile usare il valore del carattere jolly "\*" per consentire le richieste da tutte le entità Origin.

Si consideri con attenzione prima di consentire le richieste provenienti da qualsiasi origine. Significa che letteralmente qualsiasi sito Web può eseguire chiamate AJAX all'API Web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

Il *metodi* parametro delle **[EnableCors]** attributo specifica quali metodi HTTP consentiti per accedere alla risorsa. Per consentire tutti i metodi, usare il valore del carattere jolly "\*". Nell'esempio seguente consente solo le richieste GET e POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni di richieste consentite

In precedenza è stato descritto come una richiesta preliminare può includere un'intestazione Access-Control-Request-Headers, elenca le intestazioni HTTP impostate dall'applicazione (la cosiddetta "creare le intestazioni delle richieste"). Il *intestazioni* parametro delle **[EnableCors]** attributo specifica quali intestazioni di richiesta di autore sono consentite. Per consentire tutte le intestazioni, impostare *intestazioni* a "\*". A elenco elementi consentiti intestazioni specifiche, impostare *intestazioni* a un elenco delimitato da virgole delle intestazioni consentite:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Tuttavia, i browser non sono completamente coerenti nel modo in cui siano impostate Access-Control-Request-Headers. Ad esempio Chrome include attualmente "origin"; mentre FireFox non include le intestazioni standard, ad esempio "Accept", anche quando l'applicazione vengono impostati nello script.

Se si imposta *intestazioni* su un valore qualsiasi diverso da "\*", è necessario includere almeno "accept", "content-type" e "origin", oltre a eventuali intestazioni personalizzate che si desidera supportare.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Impostare le intestazioni di risposta consentite

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione. Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:

- Cache-Control
- Content-Language
- Content-Type
- Alla scadenza
- Ultima modifica
- (Pragma)

La specifica CORS chiama questi [intestazioni di risposta semplice](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Per rendere altre intestazioni disponibili per l'applicazione, impostare il *exposedHeaders* del parametro **[EnableCors]**.

Nell'esempio seguente, il controller `Get` metodo imposta un'intestazione personalizzata denominata "X-Custom-Header". Per impostazione predefinita, il browser non espone questa intestazione in una richiesta multiorigine. Per rendere disponibile l'intestazione, includere "X-Custom-Header" nella *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Passaggio di credenziali in richieste Multiorigine

Credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser invia credenziali con una richiesta multiorigine. Le credenziali includono i cookie, nonché gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta multiorigine, il client deve impostare **XMLHttpRequest.withCredentials** su true.

Usando **XMLHttpRequest** direttamente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

In jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Inoltre, il server deve consentire le credenziali. Per consentire le credenziali multiorigine nell'API Web, impostare il **SupportsCredentials** proprietà su true nella **[EnableCors]** attributo:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Se questa proprietà è true, la risposta HTTP includerà un'intestazione di accesso-controllo-Allow-Credentials. Questa intestazione indica al browser che il server consenta le credenziali per una richiesta multiorigine.

Se il browser invia le credenziali, ma la risposta non include un'intestazione di accesso-controllo-Allow-Credentials valida, il browser non espone la risposta all'applicazione e la richiesta AJAX non riesce.

Impostazione cautela **SupportsCredentials** su true, perché significa che un sito Web all'indirizzo di un altro dominio può inviare le credenziali dell'utente ha eseguito l'accesso all'API Web per conto dell'utente, senza che l'utente ne sia consapevole. La specifica CORS inoltre stabilito che l'impostazione *le entità Origin* al &quot; \* &quot; non è valido se **SupportsCredentials** è true.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Provider criteri CORS personalizzato

Il **[EnableCors]** attributo implementa le **ICorsPolicyProvider** interfaccia. È possibile fornire un'implementazione personalizzata creando una classe che deriva da **attributo** e implementa **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

A questo punto è possibile applicare l'attributo in altre posizioni che si inseriscono **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Ad esempio, un provider criteri CORS personalizzato è stato possibile leggere le impostazioni da un file di configurazione.

Come alternativa all'uso di attributi, è possibile registrare un' **ICorsPolicyProviderFactory** oggetto creato da **ICorsPolicyProvider** oggetti.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Per impostare il **ICorsPolicyProviderFactory**, chiamare il **SetCorsPolicyProviderFactory** metodo di estensione all'avvio, come indicato di seguito:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Supporto del browser

Il pacchetto di CORS per l'API Web è una tecnologia lato server. Il browser dell'utente deve anche supportare CORS. Fortunatamente, includono le versioni correnti di tutti i principali browser [supporto per CORS](http://caniuse.com/cors).

Internet Explorer 8 e Internet Explorer 9 hanno un supporto parziale per la condivisione CORS, utilizzando l'oggetto XDomainRequest legacy anziché XMLHttpRequest. Per altre informazioni, vedere [XDomainRequest - le restrizioni, limitazioni e le soluzioni alternative](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
