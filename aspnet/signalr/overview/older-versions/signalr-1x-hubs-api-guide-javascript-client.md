---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x Guida all'API Hubs - Client JavaScript | Microsoft Docs
author: pfletcher
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 1.1 nel client JavaScript, ad esempio browser e Windows Store (WinJS) UT...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 993ad7924d8335f79aa2c3e41c00ddfa8bc26874
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837725"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a>SignalR 1.x Guida all'API Hubs - Client JavaScript
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Questo documento fornisce un'introduzione all'uso dell'API di hub per SignalR versione 1.1 nel client JavaScript, ad esempio browser e applicazioni Windows Store (WinJS).
> 
> L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server. Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client. Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server. SignalR si occupa di tutte le attività client-server.
> 
> SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR hub e connessioni permanenti o per un'esercitazione che illustra come compilare un'applicazione di SignalR completa, vedere [SignalR - Guida introduttiva](../getting-started/index.md).


## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Il proxy generato e che cosa fa per te](#genproxy)

    - [Quando usare il proxy generato](#cantusegenproxy)
- [Programma di installazione client](#clientsetup)

    - [Come fanno riferimento al proxy generato in modo dinamico](#dynamicproxy)
    - [Come creare un file fisico per SignalR generato proxy](#manualproxy)
- [Come stabilire una connessione](#establishconnection)

    - [$. connection.hub è lo stesso oggetto consente di creare tale $.hubConnection()](#connequivalence)
    - [Esecuzione asincrona del metodo start](#asyncstart)
- [Come stabilire una connessione tra domini](#crossdomain)
- [Come configurare la connessione](#configureconnection)

    - [Come specificare i parametri di stringa di query](#querystring)
    - [Come specificare il metodo di trasporto](#transport)
- [Come ottenere un proxy per una classe Hub](#getproxy)
- [Come definire i metodi sul client che il server può chiamare](#callclient)
- [Come chiamare i metodi del server dal client](#callserver)
- [Come gestire gli eventi di durata connessione](#connectionlifetime)
- [Come gestire gli errori](#handleerrors)
- [Come abilitare la registrazione lato client](#logging)

Per informazioni su come programmare il server o client .NET, vedere le risorse seguenti:

- [Guida all'API di SignalR Hubs - Server](../guide-to-the-api/hubs-api-guide-server.md)
- [Guida all'API di SignalR Hubs - Client .NET](../guide-to-the-api/hubs-api-guide-net-client.md)

I collegamenti agli argomenti di riferimento all'API sono alla versione dell'API .NET 4.5. Se si usa .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Il proxy generato e che cosa fa per te

È possibile programmare un client JavaScript per comunicare con un servizio di SignalR con o senza un proxy che SignalR genera automaticamente. Ciò che esegue il proxy per l'utente è semplificare la sintassi del codice usato per connettersi, metodi di scrittura che chiama il server, e chiamare metodi sul server.

Quando si scrive codice per chiamare i metodi di server, il proxy generato consente di usare la sintassi che sembra si erano in esecuzione una funzione locale: è possibile scrivere `serverMethod(arg1, arg2)` invece di `invoke('serverMethod', arg1, arg2)`. La sintassi del proxy generato consente inoltre di un errore lato client immediato e comprensibile se si digita un nome di metodo server. E se si crea manualmente il file che definisce i proxy, è anche possibile ottenere supporto IntelliSense per la scrittura di codice che chiama i metodi di server.

Ad esempio, si supponga di che avere la seguente classe dell'Hub sul server:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Gli esempi di codice seguenti mostrano cosa JavaScript codice simile per richiamare il `NewContosoChatMessage` su server e ricevere le chiamate del metodo di `addContosoChatMessageToPage` metodo dal server.

**Con il proxy generato**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Senza il proxy generato**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Quando usare il proxy generato

Se si vuole registrare più gestori di eventi per un metodo di client che chiama il server, è possibile usare il proxy generato. In caso contrario, è possibile scegliere di usare il proxy generato o non in base alle proprie preferenze di scrittura del codice. Se si sceglie di non usarla, non è necessario fare riferimento all'URL "/ degli hub signalr" in un `script` elemento nel codice client.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Programma di installazione client

Un client JavaScript vengono richiesti riferimenti ai file SignalR core JavaScript e jQuery. La versione di jQuery deve essere 1.6.4 o versioni successive principali, ad esempio 1.7.2, 1.8.2 o 1.9.1. Se si decide di usare il proxy generato, è necessario anche un riferimento al proxy di SignalR generati file JavaScript. Nell'esempio seguente mostra i riferimenti all'aspetto in una pagina HTML che usa il proxy generato.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Questi riferimenti devono essere incluso in questo ordine: jQuery, SignalR core in seguito e i proxy di SignalR cognome.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Come fanno riferimento al proxy generato in modo dinamico

Nell'esempio precedente, il riferimento al proxy di SignalR generato è al codice JavaScript generato in modo dinamico, non a un file fisico. SignalR crea il codice JavaScript per il proxy in tempo reale e li fornisce ai client in risposta all'URL "/ signalr/hub". Se è specificato un URL di base diversi per le connessioni SignalR nel server di `MapHubs` metodo, l'URL del file proxy generato dinamicamente corrisponde all'URL personalizzato con "/ hub" aggiunto a tale.

> [!NOTE]
> Per i client Windows 8 (Windows Store) in JavaScript, usare il file fisico proxy anziché in quello generato in modo dinamico. Per altre informazioni, vedere [come creare un file fisico per SignalR generato proxy](#manualproxy) più avanti in questo argomento.


In una visualizzazione Razor di ASP.NET MVC 4, usare il carattere tilde per fare riferimento alla radice dell'applicazione nel riferimento file proxy:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Per altre informazioni sull'uso di SignalR in MVC 4, vedere [Introduzione a SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

In una visualizzazione Razor di ASP.NET MVC 3, usare `Url.Content` relativo al riferimento al file proxy:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

In un'applicazione Web Form ASP.NET, usare `ResolveClientUrl` per il proxy di riferimento di file o la registrazione tramite ScriptManager usando un percorso relativo della radice app (a partire da una tilde):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Come regola generale, usare lo stesso metodo per specificare l'URL "/ signalr/hub" che è usato per i file CSS o JavaScript. Se si specifica un URL senza usare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si test in Visual Studio con IIS Express ma avrà esito negativo con un errore 404 durante la distribuzione in modalità IIS completa. Per altre informazioni, vedere **risoluzione dei riferimenti alle risorse a livello di radice** nelle [i server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) nel sito MSDN.

Quando si esegue un progetto web in Visual Studio 2012 in modalità di debug e se si utilizza Internet Explorer come browser, è possibile visualizzare il file proxy nella **Esplora soluzioni** sotto **documenti Script**, come illustrato di figura seguente.

![File proxy generato di JavaScript in Esplora soluzioni](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Per visualizzare il contenuto del file, fare doppio clic su **hub**. Se non si usa Visual Studio 2012 e Internet Explorer, o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file passando all'URL "/ signalR/hub". Ad esempio, se il sito è in esecuzione in `http://localhost:56699`, passare a `http://localhost:56699/SignalR/hubs` nel browser.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Come creare un file fisico per SignalR generato proxy

Come alternativa per il proxy generato dinamicamente, è possibile creare un file fisico contenente il codice proxy e fare riferimento a tale file. È possibile eseguire questa operazione per un controllo sulla memorizzazione nella cache o creazione di bundle comportamento o per ottenere IntelliSense quando si esegue la codifica le chiamate ai metodi di server.

Per creare un file proxy, seguire i passaggi seguenti:

1. Installare il [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pacchetto NuGet.
2. Aprire un prompt dei comandi e passare al *strumenti* cartella che contiene il file SignalR.exe. La cartella strumenti si trova nella posizione seguente:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Immettere il comando seguente:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Il percorso per il *. dll* è in genere il *bin* cartella nella cartella del progetto.

    Questo comando crea un file denominato *server. js* nella stessa cartella *signalr.exe*.
4. Inserire il *server. js* file in una cartella appropriata nel progetto, rinominarlo in modo appropriato per l'applicazione, quindi aggiungere un riferimento a esso al posto del riferimento "/ degli hub signalr".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Come stabilire una connessione

Prima di stabilire una connessione, è necessario creare un oggetto di connessione, creare un proxy e registrare i gestori eventi per metodi che possono essere chiamati dal server. Quando sono configurati i proxy e gestori eventi, è possibile stabilire la connessione tramite una chiamata di `start` (metodo).

Se si usa il proxy generato, non è necessario creare l'oggetto di connessione nel codice perché il codice proxy generato esegue tutto automaticamente.

<a id="nogenconnection"></a>

**Stabilire una connessione (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Stabilire una connessione (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR. Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> In genere si registrano i gestori di eventi prima di chiamare il `start` metodo per stabilire la connessione. Se si vuole registrare alcuni gestori di eventi dopo avere stabilito la connessione, è possibile farlo, ma è necessario registrare almeno una dei gestori di eventi prima di chiamare il `start` (metodo). Uno dei motivi è che possono essere presenti molti hub in un'applicazione, ma è preferibile attivare il `OnConnected` evento su ogni Hub se solo si intende utilizzare uno di essi. Quando viene stabilita la connessione, la presenza di un metodo client nel proxy dell'Hub è che indica a SignalR per attivare il `OnConnected` evento. Se non si registrano i gestori di eventi prima di chiamare il `start` metodo, sarà in grado di richiamare i metodi per l'Hub, ma l'Hub `OnConnected` non chiamare il metodo e non verrà richiamato Nessun metodo client dal server.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub è lo stesso oggetto consente di creare tale $.hubConnection()

Come si può notare dagli esempi, quando si usa il proxy generato, `$.connection.hub` fa riferimento all'oggetto connessione. Questo è lo stesso oggetto che si ottiene chiamando `$.hubConnection()` quando non si usa il proxy generato. Il codice proxy generato crea la connessione tramite l'istruzione seguente:

![Creazione di una connessione nel file proxy generato](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Quando si usa il proxy generato, è possibile eseguire qualsiasi operazione con `$.connection.hub` che è possibile eseguire con un oggetto connessione quando non si usa il proxy generato.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Esecuzione asincrona del metodo start

Il `start` metodo viene eseguito in modo asincrono. Restituisce un [oggetto Deferred jQuery](http://api.jquery.com/category/deferred-object/), il che significa che è possibile aggiungere le funzioni di callback da chiamare metodi quali `pipe`, `done`, e `fail`. Se si dispone di codice che si desidera eseguire dopo aver stabilita la connessione, ad esempio una chiamata a un metodo server, tale codice inserito in una funzione di callback o chiamarlo da una funzione di callback. Il `.done` metodo di callback viene eseguito dopo aver stabilita la connessione e dopo qualsiasi codice che è necessario `OnConnected` metodo del gestore eventi nel server al termine dell'esecuzione.

Se si inserisce l'istruzione "Ora connessi" dell'esempio precedente alla riga successiva del codice dopo il `start` chiamata al metodo (non in un `.done` callback), il `console.log` riga verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente esempio:

![Modo errato per scrivere codice che viene eseguito una volta stabilita connessione](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Come stabilire una connessione tra domini

In genere se il browser ha caricato una pagina dalla `http://contoso.com`, la connessione SignalR è nello stesso dominio, in `http://contoso.com/signalr`. Se la pagina dalla `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini. Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita. Per stabilire una connessione tra domini, assicurarsi che le connessioni tra domini sono abilitate nel server e specificare l'URL di connessione quando si crea l'oggetto connessione. SignalR userà la tecnologia appropriata per le connessioni tra domini, ad esempio [JSONP](http://en.wikipedia.org/wiki/JSONP) oppure [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

Nel server, abilitare le connessioni tra domini selezionando questa opzione quando si chiama il `MapHubs` (metodo).

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

Sul client, specificare l'URL quando si crea l'oggetto di connessione (senza il proxy generato) o prima di chiamare il metodo start (con il proxy generato).

**Codice client che specifica una connessione cross-domain (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Codice client che specifica una connessione cross-domain (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

Quando si usa la `$.hubConnection` costruttore, non è necessario includere `signalr` nell'URL quanto viene aggiunto automaticamente (a meno che non si specifica `useDefaultUrl` come `false`).

È possibile creare più connessioni a endpoint diversi.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - Non impostare `jQuery.support.cors` su true nel codice.
> 
>     ![Non impostare jQuery.support.cors su true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR gestisce l'uso di JSONP o CORS. Impostazione `jQuery.support.cors` a true Disabilita JSONP perché causa SignalR per presupporre il browser supporta la condivisione CORS.
> - Quando ci si connette a un URL localhost, Internet Explorer 10 non considera una connessione tra domini, in modo che l'applicazione funzionerà in locale con Internet Explorer 10 anche se non è stata abilitata la connessione tra domini nel server.
> - Per informazioni sull'uso di connessioni tra domini con Internet Explorer 9, vedere [thread di StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Per informazioni sull'uso di connessioni tra domini con Chrome, vedere [thread di StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR. Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Come configurare la connessione

Prima di aver stabilito una connessione, è possibile specificare parametri della stringa di query o specificare il metodo di trasporto.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Come specificare i parametri di stringa di query

Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri della stringa di query all'oggetto connessione. Negli esempi seguenti illustrano come impostare un parametro di stringa di query nel codice client.

**Impostare un valore di stringa di query prima di chiamare il metodo start (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Impostare un valore di stringa di query prima di chiamare il metodo start (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Come specificare il metodo di trasporto

Durante il processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client. Se si conosce già il trasporto da usare, è possibile ignorare questo processo di negoziazione specificando il metodo di trasporto quando si chiama il `start` (metodo).

**Codice client che specifica il metodo di trasporto (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Codice client che specifica il metodo di trasporto (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si desidera SignalR per provarle in:

**Codice client che specifica uno schema di fallback di trasporto personalizzato (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**Codice client che specifica uno schema di fallback di trasporto personalizzato (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

È possibile usare i valori seguenti per specificare il metodo di trasporto:

- "WebSocket"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Gli esempi seguenti illustrano come determinare quale metodo di trasporto viene utilizzato da una connessione.

**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [- Server - Guida all'API Hubs SignalR di ASP.NET come ottenere informazioni relative al client dalla proprietà di contesto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Per altre informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Come ottenere un proxy per una classe Hub

Ogni oggetto di connessione che crea incapsula informazioni su una connessione a un servizio SignalR contenente uno o più classi di Hub. Per comunicare con una classe di Hub, usare un oggetto proxy che create dall'utente (se non si usa il proxy generato) o che viene generato automaticamente.

Nel client il nome del proxy è una versione con notazione camel del nome della classe dell'Hub. SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.

**Classe dell'hub sul server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Ottenere un riferimento al proxy client generato per l'Hub**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Creare il proxy client per la classe Hub (senza proxy generato)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Se si decora la classe di Hub con un `HubName` attributo, usare il nome esatto senza modificare maiuscole e minuscole.

**Classe dell'hub sul server con l'attributo HubName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Ottenere un riferimento al proxy client generato per l'Hub**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Creare il proxy client per la classe Hub (senza proxy generato)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Come definire i metodi sul client che il server può chiamare

Per definire un metodo che il server è possibile chiamare da un Hub, aggiungere un gestore eventi per il proxy dell'Hub usando il `client` proprietà del proxy generato o chiamata di `on` metodo se non si usa il proxy generato. I parametri possono essere oggetti complessi.

Aggiungere il gestore dell'evento prima di chiamare il `start` metodo per stabilire la connessione. (Se si desidera aggiungere i gestori eventi dopo la chiamata di `start` metodo, vedere la nota nella [come stabilire una connessione](#establishconnection) in precedenza in questo documento e usare la sintassi illustrata per la definizione di un metodo senza usare il proxy generato.)

Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole. Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` sul client.

**Definire un metodo nel client (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Modo alternativo per definire il metodo nel client (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Definire un metodo nel client (senza il proxy generato o durante l'aggiunta dopo aver chiamato il metodo di avvio)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Codice del server che chiama il metodo client**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Negli esempi seguenti includono un oggetto complesso come un parametro del metodo.

**Definire un metodo sul client che accetta un oggetto complesso (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Definire un metodo sul client che accetta un oggetto complesso (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Codice del server che definisce l'oggetto complesso**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Codice del server che chiama il metodo client utilizzando un oggetto complesso**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Come chiamare i metodi del server dal client

Per chiamare un metodo server dal client, usare il `server` proprietà del proxy generato o `invoke` metodo sul proxy di Hub, se non si usa il proxy generato. Il valore restituito o i parametri possono essere oggetti complessi.

Passare una versione maiuscole-minuscole camel del nome del metodo dell'hub. SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.

Gli esempi seguenti illustrano come chiamare un metodo server che non ha un valore restituito e come chiamare un metodo del server che hanno un valore restituito.

**Metodo server senza l'attributo HubMethodName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Codice del server che definisce l'oggetto complesso passato un parametro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Codice client che richiama il metodo del server (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Codice client che richiama il metodo server (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Se è decorata del metodo dell'Hub con un `HubMethodName` attributo, usare il nome senza modificare maiuscole e minuscole.

**Metodo server** con un attributo HubMethodName

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Codice client che richiama il metodo del server (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Codice client che richiama il metodo server (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

Negli esempi precedenti viene illustrato come chiamare un metodo del server che non restituisce alcun valore. Negli esempi seguenti illustrano come chiamare un metodo server che ha un valore restituito.

**Codice del server per un metodo che ha un valore restituito**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**La classe Stock utilizzata per il** valore restituito

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Codice client che richiama il metodo del server (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Codice client che richiama il metodo server (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Come gestire gli eventi di durata connessione

SignalR fornisce il collegamento seguente gli eventi di durata che è possibile gestire:

- `starting`: Generato prima che tutti i dati vengono inviati tramite la connessione.
- `received`: Generato quando vengono ricevuti dati sulla connessione. Fornisce i dati ricevuti.
- `connectionSlow`: Generato quando il client rileva una connessione lenta o frequentemente eliminazione.
- `reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.
- `reconnected`: Generato quando il trasporto sottostante è stata riconnessa.
- `stateChanged`: Generato quando cambia lo stato della connessione. Fornisce lo stato precedente e il nuovo stato (connessione, connesso, riconnessione o Disconnected).
- `disconnected`: Generato quando la connessione è disconnesso.

Ad esempio, se si desidera visualizzare i messaggi di avviso quando si verificano problemi di connessione che potrebbero causare ritardi notevoli, gestire il `connectionSlow` evento.

**Gestire l'evento connectionSlow (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Gestire l'evento connectionSlow (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Come gestire gli errori

Il client SignalR JavaScript fornisce un `error` eventi che è possibile aggiungere un gestore per. È anche possibile usare il metodo hanno esito negativo per aggiungere un gestore errori risultanti da una chiamata al metodo server.

Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione SignalR restituito dopo un errore contiene informazioni minime sull'errore. Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si vuole abilitare i messaggi di errore dettagliato per risoluzione dei problemi, usare il codice seguente nel server.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.

**Aggiungere un gestore degli errori (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Aggiungere un gestore degli errori (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.

**Gestire un errore da una chiamata al metodo (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Gestire un errore da una chiamata al metodo (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Se una chiamata al metodo ha esito negativo, il `error` evento viene generato anche in modo che il codice nel `error` gestore del metodo e nel `.fail` metodo callback verrebbe eseguito.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Come abilitare la registrazione lato client

Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà dell'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.

**Abilitare la registrazione (con il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Abilitare la registrazione (senza il proxy generato)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Per visualizzare i log, aprire Strumenti di sviluppo del browser e passare alla scheda della Console. Per un'esercitazione che illustra le istruzioni dettagliate e schermata schermate che illustrano come eseguire questa operazione, vedere [trasmissione Server con ASP.NET Signalr - abilitare la registrazione](index.md).
