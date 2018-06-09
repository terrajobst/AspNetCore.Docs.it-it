---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Abilitare le richieste Cross-Origin in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: Viene illustrato come supportare condivisione delle risorse Multiorigine (CORS) nell'API Web ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508380"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Abilitare le richieste Cross-Origin in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Protezione del browser impedisce che una pagina web effettua le richieste AJAX a un altro dominio. Questa restrizione viene chiamata il *criteri stessa origine*e impedisce a un sito di leggere i dati sensibili da un altro sito. Tuttavia, in alcuni casi è consigliabile consentire ad altri siti di chiamare l'API web.
> 
> [Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) è uno standard W3C che consente a un server per ridurre i criteri della stessa origine. Tramite condivisione CORS, un server può consentire in modo esplicito alcune richieste cross-origin durante il rifiuto di altri utenti. CORS è più sicura e più flessibile di tecniche precedenti, ad esempio [JSONP](http://en.wikipedia.org/wiki/JSONP). In questa esercitazione viene illustrato come abilitare CORS nell'applicazione Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - 2.2 API Web


<a id="intro"></a>
## <a name="introduction"></a>Introduzione

Questa esercitazione viene illustrato che il supporto CORS in ASP.NET Web API. Si inizierà creando due progetti ASP.NET, uno denominato "servizio Web", che ospita un controller API Web, e l'altra chiamato "WebClient", che chiama WebService. Poiché le due applicazioni sono ospitate in domini diversi, una richiesta AJAX da WebClient al servizio Web è una richiesta multiorigine.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Che cos'è "Stessa origine"?

Se dispongono di porte, host e gli schemi identici, due URL abbia la stessa origine. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Questi due URL abbia la stessa origine:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Questi URL sono origini diverse rispetto a quelli due:

- `http://example.net` -Altro dominio
- `http://example.com:9000/foo.html` -Porta diversa
- `https://example.com/foo.html` -Schema differente
- `http://www.example.com/foo.html` -Sottodominio diverso

> [!NOTE]
> Internet Explorer non considera la porta quando si confrontano le origini.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Creare il progetto servizio Web

> [!NOTE]
> In questa sezione si presuppone che si conosce già la modalità creare progetti API Web. In caso contrario, vedere [Introduzione a ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Avviare Visual Studio e creare un nuovo **applicazione Web ASP.NET** progetto. Selezionare il **vuoto** modello di progetto. In "Aggiungere cartelle e i riferimenti per core", selezionare il **API Web** casella di controllo. Facoltativamente, selezionare l'opzione "Ospita nel Cloud" per distribuire l'app in Microsoft Azure. Microsoft offre l'hosting web disponibile fino a 10 siti Web in un [senza account di valutazione Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Aggiungere un controller API Web denominato `TestController` con il codice seguente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

È possibile eseguire l'applicazione localmente o distribuire in Azure. (Per le schermate in questa esercitazione, è distribuita in App Web di servizio App di Azure.) Per verificare che l'API web funziona, passare a `http://hostname/api/test/`, dove *hostname* è il dominio in cui è distribuita l'applicazione. Verrà visualizzato il testo della risposta, &quot;ottenere: messaggio di prova&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Creare il progetto WebClient

Creare un altro progetto di applicazione Web ASP.NET e selezionare il **MVC** modello di progetto. Facoltativamente, selezionare **Modifica autenticazione** > **Nessuna autenticazione**. Non è necessaria l'autenticazione per questa esercitazione.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

In Esplora soluzioni, aprire il file Views/Home/Index.cshtml. Sostituire il codice in questo file con le operazioni seguenti:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Per il *serviceUrl* variabile, utilizzare l'URI dell'applicazione servizio Web. A questo punto eseguire localmente l'app WebClient o pubblicarla in un altro sito Web.

Fare clic sul pulsante "Provalo" Invia una richiesta AJAX applicazione di servizio Web, utilizzando il metodo HTTP elencato nella casella a discesa (GET, POST o PUT). Ciò consente di esaminare le richieste tra origini diverse. Attualmente, l'applicazione di servizio Web non supportano CORS, pertanto se si fa clic sul pulsante, si otterrà un errore.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Se si guarda il traffico HTTP in uno strumento come [Fiddler](http://www.telerik.com/fiddler), si noterà che il browser invia una richiesta GET e la richiesta ha esito positivo, ma la chiamata di AJAX restituisce un errore. È importante comprendere che criteri stessa origine non impediscano il browser da *invio* la richiesta. Invece, impedisce l'applicazione di visualizzare il *risposta*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Abilitare CORS

Ora consente di abilitare CORS nell'applicazione di servizio Web. In primo luogo, aggiungere il pacchetto NuGet di CORS. In Visual Studio, dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, digitare il comando seguente:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Questo comando installa il pacchetto più recente e aggiorna tutte le dipendenze, incluse le librerie di API Web di base. Utente-flag di versione da una versione specifica di destinazione. Il pacchetto CORS richiede Web API 2.0 o versione successiva.

Aprire il file App\_Start/WebApiConfig.cs. Aggiungere il codice seguente per il **WebApiConfig.Register** metodo.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Successivamente, aggiungere il **[EnableCors]** attributo la `TestController` classe:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Per il *origini* parametro, usare l'URI in cui è distribuita l'applicazione WebClient. Questo consente le richieste tra le origini da WebClient, durante la disattivazione comunque tutte le altre richieste tra domini. In un secondo momento, verranno descritti i parametri per **[EnableCors]** in modo più dettagliato.

Non includere una barra rovesciata alla fine del *origini* URL.

Ridistribuire l'applicazione di servizio Web aggiornata. Non è necessario aggiornare WebClient. A questo punto la richiesta AJAX dal WebClient dovrebbe avere esito positivo. Sono consentiti tutti i metodi GET, PUT e POST.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Funzionamento di CORS

In questa sezione descrive cosa accade in una richiesta CORS, a livello dei messaggi HTTP. È importante comprendere il funzionamento di CORS, in modo che sia possibile configurare il **[EnableCors]** attributo correttamente e risolvere i problemi se ciò non funziona come previsto.

La specifica CORS introduce diverse nuove intestazioni HTTP che consentono di richieste cross-origin. Se un browser supporta la condivisione CORS, imposta le intestazioni automaticamente per le richieste di cross-origin. non è necessario eseguire alcuna operazione speciali nel codice JavaScript.

Di seguito è riportato un esempio di una richiesta multiorigine. L'intestazione "Origin" fornisce il dominio del sito che effettua la richiesta.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin. Il valore di questa intestazione corrisponde all'intestazione di origine o è il valore del carattere jolly "\*", che significa che è consentito qualsiasi tipo di origine.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX. In particolare, il browser non consente la richiesta. Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'applicazione client.

**Richieste preliminari**

Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, denominata "richiesta preliminare," prima dell'invio della richiesta per la risorsa effettiva.

Il browser è possibile ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:

- Il metodo di richiesta è GET, HEAD o POST, *e*
- L'applicazione non imposta le intestazioni di richiesta diverso da Accept, Accept-Language, Content-Language, Content-Type o ID di evento Last, *e*
- L'intestazione Content-Type (se impostato) è uno dei seguenti: 

    - application/x-www-form-urlencoded
    - multipart/form-data
    - Testo

Si applica la regola sulle intestazioni di richiesta per le intestazioni che l'applicazione imposta chiamando **setRequestHeader** sul **XMLHttpRequest** oggetto. (Specifica CORS chiama queste "intestazioni di richiesta autore"). La regola non si applica alle intestazioni di *browser* impostare, ad esempio User-Agent, l'Host o Content-Length.

Di seguito è riportato un esempio di una richiesta preliminare:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP. Include due intestazioni speciali:

- Access-Control-Request-Method: Il metodo HTTP utilizzato per la richiesta effettiva.
- Access-Control-Request-Headers: Un elenco di intestazioni di richiesta che il *applicazione* impostare per la richiesta effettiva. (Nuovamente, questo non includono le intestazioni che imposta il browser).

Di seguito è riportata una risposta di esempio, supponendo che il server consenta la richiesta:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

La risposta include un'intestazione Access-controllo-Allow-Methods che elenca i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Consenti-Headers, che elenca le intestazioni consentite. Se la richiesta preliminare ha esito positivo, il browser invia la richiesta, come descritto in precedenza.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Regole di ambito per [EnableCors]

È possibile abilitare CORS per ogni azione, per ogni controller o a livello globale per tutti i controller API Web nell'applicazione.

**Per ogni azione**

Per abilitare CORS per una singola azione, impostare il **[EnableCors]** attributo del metodo di azione. L'esempio seguente Abilita condivisione CORS per il `GetItem` solo di metodo.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Per ogni Controller**

Se si imposta **[EnableCors]** sulla classe controller, si applica a tutte le azioni nel controller. Per disabilitare CORS per un'azione, aggiungere il **[DisableCors]** attributo all'azione. L'esempio seguente Abilita CORS per ogni metodo ad eccezione di `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**A livello globale**

Per abilitare CORS per tutti i controller API Web nell'applicazione, passare un **EnableCorsAttribute** istanza per il **EnableCors** metodo:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Se si imposta l'attributo più di un ambito, l'ordine di precedenza è:

1. Operazione
2. Controller
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Impostare le origini consentite

Il *origini* parametro il **[EnableCors]** attributo specifica che le origini sono consentite per accedere alla risorsa. Il valore è un elenco delimitato da virgole di origini consentite.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

È inoltre possibile utilizzare il valore del carattere jolly "\*" per consentire le richieste da eventuali origini.

Valutare con attenzione prima di consentire le richieste provenienti da qualsiasi origine. Significa che letteralmente qualsiasi sito Web di effettuare chiamate AJAX per l'API web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

Il *metodi* parametro il **[EnableCors]** attributo specifica quali metodi HTTP consentiti per accedere alla risorsa. Per consentire tutti i metodi, utilizzare il valore del carattere jolly "\*". Nell'esempio seguente consente solo richieste GET e POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni di richieste consentito

In precedenza descritto come una richiesta preliminare potrebbe includere un'intestazione Access-Control-Request-Headers, elenco di intestazioni HTTP impostate dall'applicazione (la cosiddetta "author intestazioni delle richieste"). Il *intestazioni* parametro il **[EnableCors]** attributo specifica le intestazioni di richiesta di autore sono consentite. Per consentire eventuali intestazioni, impostare *intestazioni* per "\*". A intestazioni specifiche whitelist, impostare *intestazioni* a un elenco delimitato da virgole di intestazioni consentite:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Tuttavia, i browser non sono completamente coerenti in modalità impostano Access-Control-Request-Headers. Ad esempio, Chrome include attualmente "origine"; mentre FireFox non include le intestazioni standard, ad esempio "Accetto", anche quando l'applicazione vengono impostati nello script.

Se si imposta *intestazioni* su un valore diverso da "\*", è necessario includere almeno "accetta", "content-type" e "origine", più eventuali intestazioni personalizzate che si desidera supportare.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Impostare le intestazioni di risposta consentiti

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione. Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:

- Cache-Control
- Content-Language
- Content-Type
- Scadenza
- Ultima modifica
- Pragma

Le specifiche CORS chiama questi [le intestazioni di risposta semplice](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Per rendere disponibili per l'applicazione altre intestazioni, impostare il *exposedHeaders* parametro di **[EnableCors]**.

Nell'esempio seguente, il controller `Get` metodo imposta un'intestazione personalizzata denominata 'Intestazione X-personalizzata'. Per impostazione predefinita, il browser non espone questa intestazione in una richiesta multiorigine. Per rendere disponibile l'intestazione, incluso 'Intestazione X-personalizzata' *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Passaggio di credenziali in richieste Cross-Origin

Credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser non invia credenziali con una richiesta multiorigine. Le credenziali includono i cookie, nonché gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta multiorigine, il client deve impostare **XMLHttpRequest.withCredentials** su true.

Utilizzando **XMLHttpRequest** direttamente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

In jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Inoltre, il server deve concedere le credenziali. Per consentire le credenziali di cross-origin nell'API Web, impostare il **SupportsCredentials** proprietà su true nel **[EnableCors]** attributo:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Se questa proprietà è true, la risposta HTTP includerà un'intestazione controllo-Consenti-le credenziali di accesso. Questa intestazione indica al browser che il server consenta le credenziali per una richiesta multiorigine.

Se il browser invia le credenziali, ma la risposta non include un'intestazione controllo-Consenti-le credenziali di accesso valida, il browser non espone la risposta all'applicazione e la richiesta AJAX non riesce.

Prestare molta attenzione impostazione **SupportsCredentials** su true, in quanto significa che un sito Web in un altro dominio può inviare le credenziali dell'utente connesso all'API Web per conto dell'utente, senza che l'utente viene presa in considerazione. Le specifiche CORS stati anche tale impostazione *origini* a &quot; \* &quot; non è valido se **SupportsCredentials** è true.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Provider criteri CORS personalizzato

Il **[EnableCors]** attributo implementa il **ICorsPolicyProvider** interfaccia. È possibile fornire la propria implementazione mediante la creazione di una classe che deriva da **attributo** e implementa **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Ora è possibile applicare l'attributo qualsiasi posizione che si inseriscono **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Ad esempio, un provider di criteri CORS personalizzato potrebbe leggere le impostazioni da un file di configurazione.

In alternativa all'utilizzo degli attributi, è possibile registrare un **ICorsPolicyProviderFactory** oggetto creato da **ICorsPolicyProvider** oggetti.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Per impostare il **ICorsPolicyProviderFactory**, chiamare il **SetCorsPolicyProviderFactory** il metodo di estensione all'avvio, come indicato di seguito:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Supporto del browser

Il pacchetto di Web API CORS è una tecnologia sul lato server. Il browser dell'utente deve inoltre supportano CORS. Fortunatamente, le versioni correnti di tutti i browser principali includono [il supporto per CORS](http://caniuse.com/cors).

Internet Explorer 8 e Internet Explorer 9 necessario supporto parziale per CORS, usando l'oggetto XDomainRequest legacy anziché XMLHttpRequest. Per ulteriori informazioni, vedere [XDomainRequest - soluzioni alternative, limitazioni e restrizioni](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
