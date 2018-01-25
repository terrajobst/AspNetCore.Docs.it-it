---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guida di ASP.NET SignalR hub API - Client JavaScript | Documenti Microsoft
author: pfletcher
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client JavaScript, ad esempio i browser e applicat (WinJS) di Windows Store...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 794ab576d3c6600911f331bab7c335476e45a0c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Guida di ASP.NET SignalR hub API - Client JavaScript
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client JavaScript, ad esempio i browser e le applicazioni Windows Store (WinJS).
> 
> L'API degli hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server. Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano metodi che eseguono il client. Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi eseguiti nel server. SignalR occuparsene di plumbing client-server.
> 
> SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR, hub e le connessioni permanenti, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Il proxy generato e funzionalità per l'utente](#genproxy)

    - [Quando utilizzare il proxy generato](#cantusegenproxy)
- [Installazione del client](#clientsetup)

    - [Come fare riferimento il proxy generato dinamicamente](#dynamicproxy)
    - [Come creare un file fisico per SignalR generato proxy](#manualproxy)
- [Come stabilire una connessione](#establishconnection)

    - [$. connection.hub è lo stesso oggetto tale $.hubConnection() Crea](#connequivalence)
    - [Esecuzione asincrona del metodo start](#asyncstart)
- [Come stabilire una connessione tra domini](#crossdomain)
- [Come configurare la connessione](#configureconnection)

    - [Come specificare i parametri di stringa di query](#querystring)
    - [Come specificare il metodo di trasporto](#transport)
- [Come ottenere un proxy per una classe di Hub](#getproxy)
- [Come definire i metodi nel client che il server può chiamare](#callclient)
- [Come chiamare i metodi di server dal client](#callserver)
- [Come gestire gli eventi di durata connessione](#connectionlifetime)
- [Come gestire gli errori](#handleerrors)
- [Come abilitare la registrazione lato client](#logging)

Per la documentazione su come programmare il server o client .NET, vedere le risorse seguenti:

- [Guida di API degli hub SignalR - Server](hubs-api-guide-server.md)
- [Guida di API degli hub SignalR - Client .NET](hubs-api-guide-net-client.md)

Il componente server di SignalR 2 disponibile solo in .NET 4.5 (se è disponibile un client .NET per SignalR 2 in .NET 4.0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Il proxy generato e funzionalità per l'utente

È possibile programmare un client JavaScript per comunicare con un servizio SignalR con o senza un proxy che genera l'errore SignalR per l'utente. Il proxy per l'utente si semplificano la sintassi del codice si utilizza per la connessione, i metodi di scrittura che il server chiama, e chiamare i metodi nel server.

Quando si scrive codice per chiamare i metodi di server, il proxy generato consente di utilizzare la sintassi che viene visualizzato come se si erano in esecuzione di una funzione locale: è possibile scrivere `serverMethod(arg1, arg2)` anziché `invoke('serverMethod', arg1, arg2)`. La sintassi proxy generato consente inoltre di un errore sul lato client immediato e comprensibile se si digita un nome di metodo server. E, se si crea manualmente il file che definisce i proxy, è inoltre possibile ottenere il supporto IntelliSense per la scrittura di codice che chiama i metodi di server.

Ad esempio, si supponga la seguente classe Hub sul server:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Gli esempi di codice seguente mostrano cosa JavaScript codice simile per richiamare il `NewContosoChatMessage` (metodo) nel server e le chiamate di ricezione il `addContosoChatMessageToPage` metodo dal server.

**Con il proxy generato**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Senza il proxy generato**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Quando utilizzare il proxy generato

Se si desidera registrare più gestori di eventi per un metodo client che chiama il server, è possibile utilizzare il proxy generato. In caso contrario, è possibile scegliere di utilizzare il proxy generato o non in base alle preferenze di codifica. Se si sceglie di non utilizzarla, non è necessario fare riferimento l'URL "o degli hub signalr" in un `script` elemento nel codice client.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Installazione del client

Un client JavaScript richiede riferimenti a jQuery e il file di JavaScript SignalR core. La versione di jQuery deve essere 1.6.4 o versioni successive principali, ad esempio 1.7.2, 1.8.2 o 1.9.1. Se si decide di utilizzare il proxy generato, è necessario anche un riferimento al proxy SignalR generati file JavaScript. Nell'esempio seguente viene illustrato l'aspetto i riferimenti in una pagina HTML che utilizza il proxy generato.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Questi riferimenti devono essere incluso in questo ordine: jQuery, SignalR core in seguito, SignalR proxy e cognome.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Come fare riferimento il proxy generato dinamicamente

Nell'esempio precedente, il riferimento al proxy generato SignalR è a codice JavaScript generato dinamicamente, non a un file fisico. SignalR crea il codice JavaScript per il proxy in tempo reale e gestisce i client in risposta all'URL di "hub signalr /". Se è specificato un URL di base diversi per le connessioni SignalR nel server il `MapSignalR` (metodo), l'URL del file proxy generato dinamicamente è l'URL personalizzato con "/ hub" aggiunte.

> [!NOTE]
> Per i client JavaScript (Windows Store) di Windows 8, usare il file fisico del proxy anziché quello generato dinamicamente. Per ulteriori informazioni, vedere [come creare un file fisico per SignalR generato proxy](#manualproxy) più avanti in questo argomento.


In un ASP.NET MVC 4 o 5 visualizzazione Razor, utilizzare la tilde per fare riferimento alla radice dell'applicazione nel riferimento file proxy:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Per ulteriori informazioni sull'utilizzo di SignalR in MVC 5, vedere [Guida introduttiva a SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

In una visualizzazione Razor di ASP.NET MVC 3, utilizzare `Url.Content` per il riferimento al file proxy:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

In un'applicazione Web Form ASP.NET, utilizzare `ResolveClientUrl` per il proxy di riferimento di file o la registrazione tramite lo ScriptManager usando un percorso relativo della radice app (inizia con una tilde):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Come regola generale, usare lo stesso metodo per specificare l'URL di "signalr/hub" usata per i file CSS o JavaScript. Se si specifica un URL senza utilizzare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si test in Visual Studio utilizzando IIS Express, ma avrà esito negativo con un errore 404, quando si distribuisce in IIS completo. Per ulteriori informazioni, vedere **la risoluzione dei riferimenti alle risorse di livello radice** in [server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) nel sito MSDN.

Quando si esegue un progetto web in Visual Studio 2013 in modalità di debug e se si utilizza Internet Explorer come browser, è possibile visualizzare il file proxy in **Esplora** in **documenti Script**, come illustrato di figura seguente.

![File proxy generato JavaScript in Esplora soluzioni](hubs-api-guide-javascript-client/_static/image1.png)

Per visualizzare il contenuto del file, fare doppio clic su **hub**. Se non si utilizza Internet Explorer e Visual Studio 2012 o 2013 o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file passando all'URL di "hub signalR /". Ad esempio, se il sito è in esecuzione in `http://localhost:56699`, passare a `http://localhost:56699/SignalR/hubs` nel browser.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Come creare un file fisico per SignalR generato proxy

In alternativa al proxy generato dinamicamente, è possibile creare un file fisico con il codice proxy e fare riferimento a tale file. È possibile eseguire tale operazione per la memorizzazione nella cache o aggregazione di comportamento di un controllo o per ottenere IntelliSense quando si esegue la codifica di chiamate ai metodi di server.

Per creare un file proxy, eseguire la procedura seguente:

1. Installare il [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pacchetto NuGet.
2. Aprire un prompt dei comandi e passare al *strumenti* cartella che contiene il file SignalR.exe. La cartella strumenti si trova nella posizione seguente:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Immettere il comando seguente:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Il percorso del *DLL* è in genere il *bin* cartella nella cartella del progetto.

    Questo comando crea un file denominato *server.js* nella stessa cartella *signalr.exe*.
4. Inserire il *server.js* file in una cartella appropriata nel progetto, rinominarlo in modo appropriato per l'applicazione e aggiungere un riferimento al posto di riferimento "/ degli hub signalr".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Come stabilire una connessione

Prima di stabilire una connessione, è necessario creare un oggetto di connessione, creare un proxy e registra i gestori eventi per metodi che possono essere chiamati dal server. Quando i proxy e gestori eventi vengono impostati, è possibile stabilire la connessione tramite la chiamata di `start` metodo.

Se si utilizza il proxy generato, non è necessario creare l'oggetto di connessione nel codice perché il codice proxy generato non viene automaticamente.

<a id="nogenconnection"></a>

**Stabilire una connessione (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Stabilire una connessione (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Il codice di esempio viene utilizzato il valore predefinito "/ signalr" URL per la connessione al servizio SignalR. Per informazioni su come specificare un URL di base diversi, vedere [ASP.NET SignalR hub API Guida - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).

Per impostazione predefinita, il percorso di hub è il server corrente. Se ci si connette a un altro server, specificare l'URL prima di chiamare il `start` (metodo), come illustrato nell'esempio seguente:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> In genere si registrano i gestori di eventi prima di chiamare il `start` metodo per stabilire la connessione. Se si desidera registrare gestori di eventi dopo avere stabilito la connessione, è possibile farlo, ma è necessario registrare almeno i gestori di eventi prima di chiamare il `start` metodo. Un motivo è che in un'applicazione possono essere presenti molti hub, ma non si vuole attivare il `OnConnected` evento su ogni Hub, se si intende solo utilizzare per uno di essi. Quando viene stabilita la connessione, la presenza di un metodo client nel proxy dell'Hub è cosa indica SignalR per attivare il `OnConnected` evento. Se non si registrano i gestori di eventi prima di chiamare il `start` (metodo), sarà possibile richiamare metodi su Hub, ma l'Hub `OnConnected` non verrà chiamato e non verrà richiamato alcun metodo client dal server.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub è lo stesso oggetto tale $.hubConnection() Crea

Come si può notare dagli esempi, quando si usa il proxy generato, `$.connection.hub` fa riferimento all'oggetto connessione. Questo è lo stesso oggetto che si ottiene chiamando `$.hubConnection()` quando non si utilizza il proxy generato. Il codice proxy generato crea la connessione per l'utente eseguendo l'istruzione seguente:

![Creazione di una connessione nel file proxy generato](hubs-api-guide-javascript-client/_static/image3.png)

Quando si usa il proxy generato, è possibile eseguire alcuna operazione con `$.connection.hub` che è possibile eseguire con un oggetto di connessione quando non si usi il proxy generato.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Esecuzione asincrona del metodo start

Il `start` metodo viene eseguito in modo asincrono. Restituisce un [oggetto posticipata jQuery](http://api.jquery.com/category/deferred-object/), il che significa che è possibile aggiungere le funzioni di callback da chiamare metodi quali `pipe`, `done`, e `fail`. Se si dispone di codice che si desidera eseguire dopo aver stabilita la connessione, ad esempio una chiamata a un metodo di server, inserire il codice in una funzione di callback o chiamarlo da una funzione di callback. Il `.done` metodo di callback viene eseguito dopo aver stabilita la connessione e dopo qualsiasi codice che il `OnConnected` metodo del gestore eventi sul server al termine dell'esecuzione.

Se si inserisce l'istruzione "Connesso" dell'esempio precedente, come la riga successiva del codice dopo il `start` chiamata al metodo (non in un `.done` callback), il `console.log` riga verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente esempio:

![Errato per la scrittura di codice che viene eseguito una volta stabilita connessione](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Come stabilire una connessione tra domini

In genere se il browser viene caricata una pagina da `http://contoso.com`, la connessione SignalR nello stesso dominio, si trova `http://contoso.com/signalr`. Se la pagina `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini. Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.

In SignalR 1. x, richieste tra domini sono stati controllati da un flag EnableCrossDomain singolo. Questo flag è controllato da JSONP sia CORS richieste. Per una maggiore flessibilità, il supporto CORS tutti è stato rimosso dal componente server di SignalR (i client JavaScript comunque usano CORS in genere se è stato rilevato che il browser supporti), e nuovi middleware OWIN è stato reso disponibile per supportare questi scenari.

Se JSONP è obbligatorio per il client (per supportare le richieste tra domini nei browser meno recenti), dovranno essere abilitata in modo esplicito impostando `EnableJSONP` sul `HubConfiguration` oggetto `true`, come illustrato di seguito. JSONP è disabilitata per impostazione predefinita, perché è meno sicura rispetto a CORS.

**Aggiunta al progetto owin:** per installare questa libreria, eseguire il comando seguente nella Console di gestione pacchetti:

`Install-Package Microsoft.Owin.Cors`

Questo comando consente di aggiungere il 2.1.0 versione del pacchetto al progetto.

### <a name="calling-usecors"></a>La chiamata UseCors

 Frammento di codice riportato di seguito viene illustrato come implementare le connessioni tra domini in 2 SignalR. 

**Implementazione le richieste tra domini in SignalR 2**

Il codice seguente viene illustrato come abilitare CORS o JSONP in un progetto SignalR 2. In questo esempio di codice Usa `Map` e `RunSignalR` anziché `MapSignalR`, in modo che il middleware CORS viene eseguita solo per le richieste di SignalR che richiedono il supporto CORS (anziché per tutto il traffico nel percorso specificato `MapSignalR`.) Mappa utilizzabile anche per qualsiasi altro middleware che deve essere eseguito per un prefisso di URL specifico, anziché per l'intera applicazione.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - Non impostare `jQuery.support.cors` su true nel codice.
> 
>     ![Non impostare jQuery.support.cors su true](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR gestisce l'utilizzo di CORS. Impostazione `jQuery.support.cors` a true Disabilita JSONP perché causa SignalR di assumere il browser supporta la condivisione CORS.
> - Quando ci si connette a un URL localhost, Internet Explorer 10 non viene considerata una connessione tra domini, pertanto l'applicazione funzionerà in locale con Internet Explorer 10, anche se non è stata abilitata la connessione tra domini nel server.
> - Per informazioni sull'utilizzo di connessioni tra domini con Internet Explorer 9, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Per informazioni sull'utilizzo di connessioni tra domini con Chrome, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Il codice di esempio viene utilizzato il valore predefinito "/ signalr" URL per la connessione al servizio SignalR. Per informazioni su come specificare un URL di base diversi, vedere [ASP.NET SignalR hub API Guida - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Come configurare la connessione

Prima di stabilire una connessione, è possibile specificare i parametri di stringa di query o specificare il metodo di trasporto.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Come specificare i parametri di stringa di query

Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri di stringa di query all'oggetto connessione. Nell'esempio seguente viene illustrato come impostare un parametro di stringa di query nel codice client.

**Impostare un valore di stringa di query prima di chiamare il metodo start (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Impostare un valore di stringa di query prima di chiamare il metodo start (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Come specificare il metodo di trasporto

Come parte del processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client. Se si conosce già il trasporto a cui si desidera utilizzare, è possibile ignorare questo processo di negoziazione specificando il metodo di trasporto quando si chiama il `start` metodo.

**Codice client che specifica il metodo di trasporto (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Codice client che specifica il metodo di trasporto (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si desidera SignalR utilizzarli:

**Codice client che specifica uno schema di fallback di trasporto personalizzato (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Codice client che specifica uno schema di trasporto personalizzato fallback (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Per specificare il metodo di trasporto, è possibile utilizzare i valori seguenti:

- "WebSocket"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Nell'esempio seguente mostrano come determinare quale metodo di trasporto viene utilizzato da una connessione.

**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [ASP.NET SignalR hub API Guida - Server - come ottenere informazioni sul client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty). Per ulteriori informazioni sui trasporti e di fallback, vedere [Introduzione a SignalR - trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Come ottenere un proxy per una classe di Hub

Ogni oggetto connessione creato incapsula informazioni su una connessione a un servizio SignalR che contiene una o più classi di Hub. Per comunicare con una classe di Hub, utilizzare un oggetto proxy che si crea manualmente (se non si usi il proxy generato) o che viene generata automaticamente.

Nel client il nome del proxy è una versione di maiuscole/minuscole camel del nome di classe dell'Hub. SignalR viene automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.

**Classe hub sul server**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Ottenere un riferimento al proxy client generato per l'Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Creare il proxy client per la classe di Hub (senza il proxy generato)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Se si decorata la classe Hub con un `HubName` attributo, utilizzare il nome esatto senza modificare maiuscole e minuscole.

**Classe hub sul server con l'attributo HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Ottenere un riferimento al proxy client generato per l'Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Creare il proxy client per la classe di Hub (senza il proxy generato)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Come definire i metodi nel client che il server può chiamare

Per definire un metodo che il server è possibile chiamare da un Hub, aggiungere un gestore eventi per il proxy dell'Hub utilizzando il `client` proprietà del proxy generato o chiamata di `on` metodo se non si utilizza il proxy generato. I parametri possono essere oggetti complessi.

Aggiungere il gestore dell'evento prima di chiamare il `start` metodo per stabilire la connessione. (Se si desidera aggiungere i gestori eventi dopo la chiamata di `start` metodo, vedere la nota nel [per stabilire una connessione](#establishconnection) più indietro in questo documento e utilizzare la sintassi illustrata per la definizione di un metodo senza utilizzare il proxy generato.)

Nome di metodo corrispondente è tra maiuscole e minuscole. Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` sul client.

**Definire un metodo nel client (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Modo alternativo per definire il metodo nel client (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Definire un metodo nel client (senza il proxy generato, o quando si aggiungono dopo aver chiamato il metodo di avvio)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Codice che chiama il metodo di client del server**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Gli esempi seguenti includono un oggetto complesso come un parametro di metodo.

**Definire un metodo nel client che accetta un oggetto complesso (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Definire un metodo nel client che accetta un oggetto complesso (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Codice del server che definisce l'oggetto complesso**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Codice del server che chiama il metodo di client utilizzando un oggetto complesso**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Come chiamare i metodi di server dal client

Per chiamare un metodo server dal client, usare il `server` proprietà del proxy generato o `invoke` metodo sul proxy di Hub, se non si utilizza il proxy generato. Il valore restituito o i parametri possono essere oggetti complessi.

Passare in una versione maiuscole-minuscole camel del nome del metodo dell'hub. SignalR viene automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.

Gli esempi seguenti mostrano come chiamare un metodo del server che non dispone di un valore restituito e come chiamare un metodo del server che hanno un valore restituito.

**Metodo server senza l'attributo HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Codice del server che definisce l'oggetto complesso passato un parametro**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Codice client che richiama il metodo server (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Codice client che richiama il metodo server (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Se è decorata del metodo dell'Hub con un `HubMethodName` attributo, utilizzare il nome senza modificare maiuscole e minuscole.

**Metodo server** con un attributo HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Codice client che richiama il metodo server (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Codice client che richiama il metodo server (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Negli esempi precedenti viene illustrano come chiamare un metodo del server che non restituisce alcun valore. Nell'esempio seguente viene illustrato come chiamare un metodo del server che ha un valore restituito.

**Codice lato server per un metodo che presenta un valore restituito**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**La classe Stock utilizzata per il** valore restituito

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Codice client che richiama il metodo server (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Codice client che richiama il metodo server (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Come gestire gli eventi di durata connessione

SignalR fornisce la seguente connessione di eventi di durata che è possibile gestire:

- `starting`: Generato prima che qualsiasi dato viene inviato tramite la connessione.
- `received`: Generato quando vengono ricevuti dati per la connessione. Fornisce i dati ricevuti.
- `connectionSlow`: Generato quando il client rileva una connessione lenta o l'eliminazione di frequente.
- `reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.
- `reconnected`: Generato quando il trasporto sottostante è stata riconnessa.
- `stateChanged`: Generato quando cambia lo stato di connessione. Fornisce lo stato precedente e il nuovo stato (connessione, connesso, riconnessione in corso o Disconnected).
- `disconnected`: Generato quando la connessione è interrotta.

Ad esempio, se si desidera visualizzare i messaggi di avviso quando si verificano problemi di connessione che potrebbero causare ritardi notevoli, è possibile gestire il `connectionSlow` evento.

**Gestire l'evento connectionSlow (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Gestire l'evento connectionSlow (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Per ulteriori informazioni, vedere [comprensione e gestione degli eventi di durata connessione in SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Come gestire gli errori

Il client SignalR JavaScript fornisce un `error` eventi che è possibile aggiungere un gestore per. È inoltre possibile utilizzare il metodo hanno esito negativo per aggiungere un gestore errori risultanti da una chiamata al metodo server.

Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione che restituisce SignalR dopo un errore contiene informazioni minime sull'errore. Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliata per motivi di sicurezza, ma se si desidera abilitare per i messaggi di errore dettagliati risoluzione dei problemi, utilizzare il codice seguente nel server.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.

**Aggiungere un gestore degli errori (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Aggiungere un gestore degli errori (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.

**Gestire un errore da una chiamata al metodo (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Gestire un errore da una chiamata al metodo (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Se una chiamata al metodo ha esito negativo, il `error` evento viene generato anche, pertanto il codice nel `error` gestore del metodo e nel `.fail` verrebbe eseguito il callback di metodo.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Come abilitare la registrazione lato client

Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà sull'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.

**Abilitare la registrazione (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Abilitare la registrazione (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Per visualizzare i registri, aprire Strumenti di sviluppo del browser e passare alla scheda della Console. Per un'esercitazione che illustra le istruzioni dettagliate e schermata schermate che illustrano come eseguire questa operazione, vedere [Server Broadcast con ASP.NET Signalr - abilitare la registrazione](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).
