---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: Comprensione e gestione degli eventi di durata connessione in SignalR 1. x | Documenti Microsoft
author: pfletcher
description: In questo articolo viene descritto come utilizzare gli eventi esposti dall'API di hub.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 4fe77769c27dd46967da2e1d68791d7142021d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036726"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Comprensione e gestione degli eventi di durata connessione in SignalR 1. x
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> In questo articolo viene fornita una panoramica degli eventi di connessione e riconnessione disconnessione SignalR che è possibile gestire e impostazioni di timeout e keepalive che è possibile configurare.
> 
> Si presume che hai già una certa conoscenza degli eventi di durata SignalR e connessione. Per un'introduzione a SignalR, vedere [SignalR - Panoramica - Introduzione](index.md). Per gli elenchi di eventi di durata connessione, vedere le risorse seguenti:
> 
> - [Come gestire gli eventi di durata connessione nella classe Hub](index.md)
> - [Come gestire gli eventi di durata connessione nei client JavaScript](index.md)
> - [Come gestire gli eventi di durata connessione nei client .NET](index.md)


## <a name="overview"></a>Panoramica

In questo articolo sono contenute le sezioni seguenti:

- [Scenari e la terminologia di durata connessione](#terminology)

    - [Le connessioni SignalR, le connessioni di trasporto e connessioni fisiche](#signalrvstransport)
    - [Scenari di disconnessione del trasporto](#transportdisconnect)
    - [Scenari di disconnessione client](#clientdisconnect)
    - [Scenari di disconnessione di server](#serverdisconnect)
- [Impostazioni di timeout e keepalive](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Come modificare le impostazioni di timeout e keepalive](#changetimeout)
- [Come notificare all'utente disconnessioni](#notifydisconnect)
- [Procedura: riconnettere continuamente](#continuousreconnect)
- [Disconnessione di un client nel codice server](#disconnectclientfromserver)

I collegamenti agli argomenti di riferimento API sono alla versione dell'API di .NET 4.5. Se si utilizza .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Scenari e la terminologia di durata connessione

Il `OnReconnected` gestore dell'evento in un Hub di SignalR può eseguire direttamente dopo `OnConnected` ma non dopo `OnDisconnected` per un determinato client. Il motivo che è possibile disporre di una riconnessione senza una disconnessione è che esistono diversi modi in cui viene utilizzata la parola "connessione" in SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Le connessioni SignalR, le connessioni di trasporto e connessioni fisiche

In questo articolo verrà distinguere *le connessioni SignalR*, *connessioni di trasporto*, e *connessioni fisiche*:

- **Connessione SignalR** fa riferimento a una relazione logica tra un client e un URL del server, mediante l'API SignalR e viene identificato in modo univoco da un ID di connessione. I dati relativi a questa relazione viene mantenuti da SignalR e viene utilizzati per stabilire una connessione di trasporto. Le entità finali della relazione e SignalR Elimina i dati quando il client chiama il `Stop` metodo o un limite di timeout viene raggiunto mentre tenta di ristabilire una connessione di trasporto persi SignalR.
- **Connessione di trasporto** fa riferimento a una relazione logica tra un client e un server, gestito da uno dei quattro trasporto API: WebSocket, gli eventi inviati al server, forever frame o lunghi per il polling. SignalR utilizza il trasporto di API per creare una connessione di trasporto e l'API di trasporto dipende dalla presenza di una connessione di rete fisica per creare la connessione di trasporto. La connessione di trasporto termina quando termina SignalR o quando il trasporto API rileva che la connessione fisica viene interrotta.
- **Connessione fisica** fa riferimento per i collegamenti di rete fisica, cavi, segnali senza fili, router e così via, che facilitano la comunicazione tra un computer client e un computer server. La connessione fisica deve essere presente per stabilire una connessione di trasporto e per stabilire una connessione SignalR, è necessario stabilire una connessione di trasporto. Tuttavia, interrompere la connessione fisica non sempre terminare immediatamente la connessione di trasporto o la connessione SignalR, come spiegato più avanti in questo argomento.

Nel diagramma seguente, la connessione SignalR è rappresentata dalle API di hub e livello PersistentConnection API SignalR, la connessione di trasporto è rappresentata dal livello di trasporto e la connessione fisica è rappresentata dalle linee tra il server e i client.

![Diagramma dell'architettura di SignalR](handling-connection-lifetime-events/_static/image1.png)

Quando si chiama il `Start` metodo in un client SignalR, si fornisce il codice client SignalR con tutte le informazioni necessarie per stabilire una connessione a un server fisica. Il codice client SignalR utilizza queste informazioni per eseguire una richiesta HTTP e stabilire una connessione fisica che utilizza uno dei metodi di quattro trasporto. Se la connessione di trasporto o il server non riesce, la connessione SignalR non viene più visualizzato immediatamente perché il client ha ancora le informazioni necessarie per ristabilire automaticamente una nuova connessione di trasporto per lo stesso URL di SignalR. In questo scenario, non è coinvolto nessun intervento dall'applicazione utente e quando il codice del client SignalR stabilisce una nuova connessione di trasporto, non si avvia una nuova connessione SignalR. La continuità della connessione SignalR viene riflessa nel fatto che l'ID di connessione, viene creato quando si chiama il `Start` metodo, non viene modificata.

Il `OnReconnected` gestore di eventi nell'Hub viene eseguito quando una connessione di trasporto viene ristabilita automaticamente dopo essere state perse. Il `OnDisconnected` gestore eventi viene eseguito alla fine di una connessione SignalR. Una connessione SignalR può terminare con uno dei modi seguenti:

- Se il client chiama il `Stop` (metodo), un messaggio di arresto viene inviato al server e client e server terminare immediatamente la connessione SignalR.
- Dopo aver persa la connettività tra client e server, il client tenta di ristabilire la connessione e il server è in attesa per il client di ristabilire la connessione. Se i tentativi di riconnessione non riescono e termina il periodo di timeout di disconnessione, client e server terminare la connessione SignalR. Il client interrompe il tentativo di riconnessione, ed elimina il server di rappresentazione della connessione SignalR.
- Se il client interrompe l'esecuzione senza possibilità di chiamare il `Stop` (metodo), il server attende di riconnettersi, il client e quindi termina la connessione SignalR dopo il periodo di timeout di disconnessione.
- Se il server si arresta in esecuzione, il client tenta di ristabilire la connessione (ricreare la connessione di trasporto) e quindi termina la connessione SignalR dopo il periodo di timeout di disconnessione.

Quando non siano presenti problemi di connessione e l'applicazione utente termina la connessione SignalR chiamando il `Stop` (metodo), la connessione SignalR e la connessione di trasporto iniziare e finire nello stesso momento. Nelle sezioni seguenti descrivono in dettaglio gli altri scenari.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scenari di disconnessione del trasporto

Connessioni fisiche potrebbero essere lente o potrebbero essere presenti interruzioni della connettività. A seconda di fattori quali la durata dell'interruzione, la connessione di trasporto potrebbe essere rimossi. SignalR tenta di ristabilire la connessione di trasporto. A volte la connessione di trasporto API rileva l'interruzione e la connessione di trasporto e SignalR scopre immediatamente che la connessione viene persa. In altri scenari, la connessione di trasporto API né SignalR viene a conoscenza immediatamente che la connettività è stata interrotta. Per tutti i trasporti, ad eccezione di polling lungo, il client SignalR utilizza una funzione denominata *keepalive* per verificare la presenza di una perdita di connettività che il trasporto API non è in grado di rilevare. Per informazioni sulle connessioni di polling lungo, vedere [impostazioni di Timeout e keepalive](#timeoutkeepalive) più avanti in questo argomento.

Quando una connessione è inattiva, periodicamente il server invia un pacchetto keepalive al client. Alla data che viene scritto in questo articolo, la frequenza predefinita è ogni 10 secondi. In attesa per tali pacchetti, i client possono indicare se si verifica un problema di connessione. Se un pacchetto di keepalive non viene ricevuto quando previsto, dopo un breve periodo di tempo il client presuppone che siano presenti problemi di connessione, ad esempio lentezza o interruzioni. Se keepalive non viene ancora ricevuto dopo un periodo più lungo, il client si presuppone che la connessione è stata eliminata, e inizia il tentativo di riconnessione.

Il diagramma seguente illustra gli eventi di client e server che vengono generati in uno scenario tipico quando vi sono problemi con la connessione fisica che non sono riconosciuti immediatamente dal trasporto API. Il diagramma si applica ai casi seguenti:

- Il trasporto è WebSocket forever frame o gli eventi inviati al server.
- Esistono diversi periodi di interruzione nella connessione di rete fisica.
- Il trasporto API non vengano rilevato interruzioni, in modo SignalR si basa sulla funzionalità keepalive rilevarle.

![Disconnessioni di trasporto](handling-connection-lifetime-events/_static/image2.png)

Se il client passa in modalità di riconnessione, ma non è possibile stabilire una connessione di trasporto entro il limite di timeout di disconnessione, il server interrompe la connessione di SignalR. In questo caso, il server esegue l'Hub `OnDisconnected` metodo e le code di un messaggio di disconnessione da inviare al client nel caso in cui il client riesce a connettersi più tardi. Se il client quindi riconnettersi, esso riceve il comando di disconnessione e chiama il `Stop` metodo. In questo scenario, `OnReconnected` non viene eseguito quando il client si riconnette, e `OnDisconnected` non viene eseguito quando il client chiama `Stop`. Il diagramma seguente illustra questo scenario.

![Interruzioni di trasporto - timeout del server](handling-connection-lifetime-events/_static/image3.png)

Gli eventi di durata connessione SignalR che potrebbero essere generati nel client sono i seguenti:

- `ConnectionSlow`evento di client.

    Generato quando una proporzione del periodo di timeout keepalive predefinita è trascorsi dall'ultimo messaggio o ping keepalive è stato ricevuto. Il periodo di avviso di timeout keepalive predefinito è 2/3 del timeout keepalive. Il timeout di keepalive è 20 secondi, pertanto l'avviso viene generato a circa 13 secondi.

    Per impostazione predefinita, il server invia ping keepalive ogni 10 secondi e il client verifica ping keepalive su ogni 2 secondi (un terzo della differenza tra il valore di timeout keepalive e il valore di keepalive timeout avviso).

    Se il trasporto API viene a conoscenza di una disconnessione, SignalR potrebbe essere informata della disconnessione prima che venga passato il periodo di avviso di timeout keepalive. In tal caso, il `ConnectionSlow` non essere generato un evento e SignalR verrebbero direttamente il `Reconnecting` evento.
- `Reconnecting`evento di client.

    Generato quando l'API (a) il trasporto rileva che la connessione viene persa, (b) il periodo di timeout keepalive è trascorsi dall'ultimo messaggio o ping keepalive è stato ricevuto. Il codice del client SignalR inizia durante il tentativo di ristabilire la connessione. Se si desidera che l'applicazione di eseguire un'azione quando una connessione di trasporto viene persa, è possibile gestire questo evento. Attualmente, il periodo di timeout predefinito keepalive è 20 secondi.

    Se il codice client tenta di chiamare un metodo dell'Hub mentre si trova nella modalità di riconnessione SignalR, SignalR tenterà di inviare il comando. La maggior parte dei casi, tale tentativo avrà esito negativo, ma in alcuni casi si potrebbe avere esito positivo. Per gli eventi inviati al server, forever frame e trasporti di polling lungo, SignalR utilizza due canali di comunicazione, uno che il client utilizza per inviare messaggi e uno che utilizza per ricevere i messaggi. Il canale utilizzato per la ricezione è aperto in modo permanente e che è quella che viene chiusa quando viene interrotta la connessione fisica. Il canale utilizzato per l'invio rimane disponibile, pertanto se viene ripristinata la connettività fisica, una chiamata al metodo dal client al server potrebbe essere eseguita correttamente prima che il canale di ricezione viene ristabilito. Il valore restituito non ricevuto fino a quando non SignalR riapre il canale utilizzato per la ricezione.
- `Reconnected`evento di client.

    Generato quando viene ristabilita la connessione di trasporto. Il `OnReconnected` esegue il gestore eventi nell'Hub.
- `Closed`evento client (`disconnected` eventi in JavaScript).

    Generato quando scade il periodo di timeout di disconnessione mentre il codice del client SignalR tentativo di riconnessione dopo aver perso la connessione di trasporto. Il valore predefinito di disconnettere il timeout è 30 secondi. (Questo evento viene generato anche quando la connessione viene interrotta perché il `Stop` metodo viene chiamato.)

Interruzioni di connessione di trasporto che non vengono rilevate dal trasporto API e non ritardare la ricezione di keepalive ping dal server per un tempo maggiore rispetto al periodo di avviso di timeout keepalive potrebbero non causare la generazione di eventi di durata di qualsiasi connessione.

In alcuni ambienti di rete deliberatamente chiudono le connessioni inattive e un'altra funzione dei pacchetti di keepalive è per evitare questo problema, consentendo a che queste reti sapere che una connessione SignalR è in uso. In casi estremi la frequenza predefinita di ping keepalive potrebbe non essere sufficiente per impedire connessioni chiuse. In questo caso è possibile configurare keepalive ping da inviare più spesso. Per ulteriori informazioni, vedere [impostazioni di Timeout e keepalive](#timeoutkeepalive) più avanti in questo argomento.

> [!NOTE] 
> 
> [!IMPORTANT]
> Non è garantita la sequenza di eventi descritti di seguito. SignalR esegue ogni tentativo di generare eventi di durata connessione in modo prevedibile in base a questo schema, ma esistono molti modi in cui la gestione sottostante Framework di comunicazioni, ad esempio le API di trasporto e molte varianti degli eventi di rete. Ad esempio, il `Reconnected` evento potrebbe non essere generato quando il client si riconnette, o `OnConnected` gestore nel server è possibile eseguire quando il tentativo di stabilire una connessione ha esito negativo. In questo argomento vengono descritti solo gli effetti che normalmente sarebbe prodotta da determinate circostanze tipiche.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scenari di disconnessione client

In un client del browser, il codice client SignalR che mantiene una connessione SignalR viene eseguito nel contesto di una pagina web JavaScript. Che è il motivo per cui la connessione SignalR deve terminare quando si esce da una pagina a un'altra e che del motivo per cui si dispone più connessioni con più ID di connessione se ci si connette da più finestre del browser o schede. Quando l'utente chiude una finestra del browser o una scheda, o si sposta in una nuova pagina o aggiorna la pagina, la connessione SignalR termina immediatamente perché il codice client SignalR gestisce l'evento di browser per l'utente e le chiamate di `Stop` metodo. In questi scenari, o in qualsiasi piattaforma client quando l'applicazione chiama il `Stop` (metodo), il `OnDisconnected` gestore esegue immediatamente nel server e il client genera il `Closed` evento (evento è denominato `disconnected` in JavaScript).

Se un'applicazione client o dal computer in cui viene eseguito si blocca o passa alla modalità di sospensione (ad esempio, quando l'utente chiude il portatile), il server non informato cosa è successo. Per quanto riguarda il server riconosce, la perdita del client potrebbe essere causato da interruzioni della connettività e il client potrebbe tentare di ristabilire la connessione. Pertanto, in questi scenari di attesa del server per fornire al client la possibilità di riconnettersi, e `OnDisconnected` non viene eseguito entro il periodo di timeout di disconnessione scadenza (circa 30 secondi per impostazione predefinita). Il diagramma seguente illustra questo scenario.

![Errore di computer client](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scenari di disconnessione di server

Quando un server è offline, viene riavviato, ha esito negativo, il dominio dell'applicazione ricicli, e così via, il risultato potrebbe essere simile alla perdita della connessione o l'API di trasporto e SignalR potrebbe sapere immediatamente che il server è stato SignalR potrebbe iniziare durante il tentativo di ristabilire la connessione senza generazione di `ConnectionSlow` evento. Se il client passa in modalità di riconnessione e ripristina il server o di riavvio o un nuovo server viene portato online prima che scada il periodo di timeout di disconnessione, il client verrà riconnettersi al server nuovo o ripristinato. In tal caso, la connessione SignalR continua nel client e il `Reconnected` viene generato l'evento. Nel primo server, `OnDisconnected` non viene mai eseguito e nel nuovo server, `OnReconnected` viene eseguita anche se `OnConnected` è stato mai eseguito per quel client nel server prima di. (L'effetto è lo stesso se il client si riconnette nello stesso server dopo il riciclo del dominio un riavvio o un'app, perché al riavvio del server non ha memoria delle attività di connessione precedente). Nel diagramma seguente si presuppone che il trasporto API viene a conoscenza di perdita della connessione immediatamente, pertanto la `ConnectionSlow` non viene generato l'evento.

![Errore del server e riconnessione](handling-connection-lifetime-events/_static/image5.png)

Se un server non diventa disponibile entro il periodo di timeout di disconnessione, termina la connessione SignalR. In questo scenario, il `Closed` evento (`disconnected` nei client JavaScript) viene generato nel client ma `OnDisconnected` nel server non viene mai chiamato. Nel diagramma seguente si presuppone che il trasporto API non viene comunicato la connessione interrotta, pertanto è stato rilevato dalla funzionalità keepalive SignalR e `ConnectionSlow` viene generato l'evento.

![Errore del server e timeout](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Impostazioni di timeout e keepalive

Il valore predefinito `ConnectionTimeout`, `DisconnectTimeout`, e `KeepAlive` i valori sono adatti alla maggior parte degli scenari, ma può essere modificati se l'ambiente dispone di particolari esigenze. Se l'ambiente di rete chiude le connessioni che sono inattive per 5 secondi, ad esempio, potrebbe essere necessario ridurre il valore di keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Questa impostazione rappresenta la quantità di tempo da lasciare una connessione di trasporto aperte e in attesa di una risposta prima di chiuderla e aprire una nuova connessione. Il valore predefinito è 110 secondi.

Questa impostazione si applica solo quando la funzionalità di keepalive è disabilitata, che in genere si applica solo a long trasporto di polling. Il diagramma seguente illustra l'effetto di questa impostazione su un valore long connessione di trasporto di polling.

![Durata connessione di trasporto di polling](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Questa impostazione rappresenta la quantità di tempo di attesa dopo che una connessione di trasporto viene interrotta prima che venga generato il `Disconnected` evento. Il valore predefinito è 30 secondi. Quando si imposta `DisconnectTimeout`, `KeepAlive` viene impostato automaticamente su 1/3 del `DisconnectTimeout` valore.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Questa impostazione rappresenta la quantità di tempo di attesa prima dell'invio di un pacchetto keepalive su una connessione inattiva. Il valore predefinito è 10 secondi. Questo valore non deve essere più di 1/3 del `DisconnectTimeout` valore.

Se si desidera impostare sia `DisconnectTimeout` e `KeepAlive`, impostare `KeepAlive` dopo `DisconnectTimeout`. In caso contrario il `KeepAlive` impostazione verrà sovrascritto quando `DisconnectTimeout` imposta automaticamente `KeepAlive` a 1/3 del valore di timeout.

Se si desidera disabilitare la funzionalità di keepalive, impostare `KeepAlive` su null. KeepAlive funzionalità viene disabilitata automaticamente per il nome lungo trasporto di polling.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Come modificare le impostazioni di timeout e keepalive

Per modificare i valori predefiniti per queste impostazioni, impostarli in `Application_Start` nel *Global. asax* file, come illustrato nell'esempio seguente. I valori mostrati nell'esempio di codice sono come i valori predefiniti.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Come notificare all'utente disconnessioni

In alcune applicazioni potrebbe voler visualizzare un messaggio all'utente quando sono presenti problemi di connettività. Si dispone di diverse opzioni per informazioni su come e quando eseguire questa operazione. Gli esempi di codice seguenti sono per un client JavaScript usando il proxy generato.

- Gestire il `connectionSlow` evento per visualizzare un messaggio non appena SignalR è a conoscenza di problemi di connessione, prima dell'aggiunta alla modalità di riconnessione.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Gestire il `reconnecting` evento per visualizzare un messaggio quando SignalR è a conoscenza di una disconnessione e verso la modalità di riconnessione.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Gestire il `disconnected` evento per visualizzare un messaggio quando il tentativo di ristabilire la connessione è scaduta. In questo scenario, l'unico modo per ristabilire una connessione con il server è necessario riavviare la connessione SignalR chiamando il `Start` metodo, che verrà creato un nuovo ID di connessione. Esempio di codice seguente utilizza un flag per assicurarsi che si esegue la notifica solo dopo un timeout di riconnessione, non dopo una normale fine per la connessione SignalR causata dalla chiamata di `Stop` metodo.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Procedura: riconnettere continuamente

In alcune applicazioni potrebbe voler automaticamente ristabilire la connessione dopo che è stata persa e il tentativo di riconnessione è scaduta. A tale scopo, è possibile chiamare il `Start` metodo i `Closed` gestore dell'evento (`disconnected` gestore dell'evento nei client JavaScript). Si consiglia di attendere un periodo di tempo prima di chiamare `Start` per evitare questo troppo spesso quando il server o la connessione fisica non sono disponibili. Esempio di codice seguente è per un client JavaScript usando il proxy generato.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un potenziale problema da tenere presenti nei client per dispositivi mobili è che i tentativi di riconnessione continua se non è disponibile il server o una connessione fisica potrebbe provocare batteria non necessari.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Disconnessione di un client nel codice server

SignalR versione 1.1.1 non dispone di un server incorporato API per la disconnessione dei client. Esistono [piani per l'aggiunta di questa funzionalità in futuro](https://github.com/SignalR/SignalR/issues/2101). Nella versione corrente di SignalR, il modo più semplice per disconnettere un client dal server consiste nell'implementare un metodo di disconnessione sul client e chiamare tale metodo dal server. L'esempio di codice seguente viene illustrato un metodo di disconnessione per un client JavaScript usando il proxy generato.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Sicurezza: questo metodo per la disconnessione dei client né l'API predefinito proposto risolvere lo scenario di client sono in esecuzione codice dannoso, poiché è possano riconnettere i client o potrebbe rimuovere il codice sono il `stopClient` metodo o modifica viene eseguita. Consente di implementare la protezione di informazioni sullo stato di tipo denial of service (DOS). è non in framework o il livello di server, ma piuttosto nel front-end dell'infrastruttura.
