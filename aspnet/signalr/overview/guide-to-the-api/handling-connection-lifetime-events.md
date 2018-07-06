---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: Comprensione e gestione degli eventi di durata connessione in SignalR | Microsoft Docs
author: pfletcher
description: Questo articolo descrive come usare gli eventi esposti dall'API di hub.
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 1b60a8c5db4b77880e31825583a26bdee2a1c07c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819581"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Comprensione e gestione degli eventi di durata connessione in SignalR
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Questo articolo offre una panoramica degli eventi di connessione e riconnessione disconnessione SignalR che è possibile gestire e le impostazioni di timeout e keepalive che è possibile configurare.
> 
> L'articolo si presuppone già una conoscenza di eventi di durata SignalR e connessione. Per un'introduzione a SignalR, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md). Per un elenco di eventi di durata connessione, vedere le risorse seguenti:
> 
> - [Come gestire gli eventi di durata connessione nella classe Hub](hubs-api-guide-server.md#connectionlifetime)
> - [Come gestire gli eventi di durata connessione nei client JavaScript](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [Come gestire gli eventi di durata connessione nei client .NET](hubs-api-guide-net-client.md#connectionlifetime)
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
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

In questo articolo sono contenute le sezioni seguenti:

- [Scenari e la terminologia di durata connessione](#terminology)

    - [Le connessioni SignalR, connessioni di trasporto e connessioni fisiche](#signalrvstransport)
    - [Scenari di disconnessione del trasporto](#transportdisconnect)
    - [Scenari di disconnessione client](#clientdisconnect)
    - [Scenari di disconnessione del server](#serverdisconnect)
- [Impostazioni di timeout e keepalive](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Come modificare le impostazioni di timeout e keepalive](#changetimeout)
- [Come inviare una notifica all'utente disconnessioni](#notifydisconnect)
- [Modalità di riconnessione in modo continuo](#continuousreconnect)
- [Come disconnettere un client nel codice server](#disconnectclientfromserver)
- [Rilevare il motivo di una disconnessione](#detectingreasonfordisconnection)

I collegamenti agli argomenti di riferimento all'API sono alla versione dell'API .NET 4.5. Se si usa .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Scenari e la terminologia di durata connessione

Il `OnReconnected` gestore dell'evento in un SignalR Hub può eseguire direttamente dopo `OnConnected` ma non dopo `OnDisconnected` per un determinato client. Il motivo che è possibile avere una riconnessione senza una disconnessione è che esistono diversi modi in cui viene utilizzata la parola "connessione" in SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Le connessioni SignalR, connessioni di trasporto e connessioni fisiche

Questo articolo verrà distinguere *le connessioni SignalR*, *connessioni di trasporto*, e *connessioni fisiche*:

- **Connessione SignalR** fa riferimento a una relazione logica tra un client e un URL del server, gestita dall'API SignalR e identificata da un ID di connessione. I dati relativi a questa relazione viene manutenuti da SignalR e viene usati per stabilire una connessione di trasporto. Le entità finali della relazione e SignalR Elimina i dati quando il client chiama il `Stop` metodo o un limite di timeout viene raggiunta mentre SignalR sta tentando di ristabilire una connessione di trasporto persi.
- **Connessione di trasporto** fa riferimento a una relazione logica tra un client e un server, gestito da uno dei quattro trasporto API: WebSockets, gli eventi inviati al server, forever frame o long polling. SignalR utilizza il trasporto API per creare una connessione di trasporto e l'API di trasporto dipende dalla presenza di una connessione di rete fisica per creare la connessione di trasporto. La connessione di trasporto termina quando SignalR termina o quando il trasporto API rileva che la connessione fisica viene interrotta.
- **Connessione fisica** fa riferimento per i collegamenti di rete fisica, dei cavi, segnali wireless, i router e così via, che facilitano la comunicazione tra un computer client e un computer server. La connessione fisica deve essere presente per stabilire una connessione di trasporto e deve essere stabilita una connessione di trasporto per stabilire una connessione SignalR. Tuttavia, la connessione fisica di rilievo non sempre immediatamente termina la connessione di trasporto o la connessione SignalR, come verrà spiegato più avanti in questo argomento.

Nel diagramma seguente, la connessione SignalR è rappresentata dall'API di hub e livello PersistentConnection API SignalR, la connessione di trasporto è rappresentata dal livello di trasporto e la connessione fisica è rappresentata dalle linee tra i server e i client.

![Diagramma dell'architettura di SignalR](handling-connection-lifetime-events/_static/image1.png)

Quando si chiama il `Start` metodo in un client SignalR, si fornisce il codice client di SignalR con tutte le informazioni necessarie per stabilire una connessione a un server fisica. Codice del client SignalR utilizza queste informazioni per eseguire una richiesta HTTP e stabilire una connessione fisica che utilizza uno dei metodi di quattro trasporto. Se si verifica un errore di connessione del trasporto o il server non riesce, la connessione SignalR non risolversi immediatamente perché il client dispone comunque le informazioni necessarie per ristabilire automaticamente una nuova connessione di trasporto per lo stesso URL di SignalR. In questo scenario, non è coinvolto alcun intervento dell'applicazione utente e quando il codice del client SignalR stabilisce una nuova connessione di trasporto, non si avvia una nuova connessione SignalR. La continuità della connessione SignalR si riflette il fatto che l'ID connessione, che viene creato quando si chiama il `Start` metodo, non viene modificata.

Il `OnReconnected` gestore eventi nell'Hub viene eseguito quando una connessione di trasporto viene automaticamente ristabilita dopo essere stata persa. Il `OnDisconnected` gestore eventi viene eseguito alla fine di una connessione SignalR. Una connessione SignalR può terminare con uno dei modi seguenti:

- Se il client chiama il `Stop` metodo, viene inviato un messaggio di arresto al server e client e server terminare immediatamente la connessione di SignalR.
- Dopo aver persa la connettività tra client e server, il client tenta di ristabilire la connessione e il server è in attesa per il client ristabilire la connessione. Se i tentativi per ristabilire la connessione hanno esito negativo e termina il periodo di timeout di disconnessione, client e server alla fine della connessione di SignalR. Il client interrompe il tentativo di riconnettere ed elimina il server relativa rappresentazione della connessione SignalR.
- Se il client non arresta l'esecuzione senza possibilità di chiamare il `Stop` metodo, il server attende che il client di riconnettersi e quindi termina la connessione SignalR dopo il periodo di timeout di disconnessione.
- Se il server si arresta in esecuzione, il client viene tentato riconnettersi (ricreare la connessione di trasporto) e quindi termina la connessione SignalR dopo il periodo di timeout di disconnessione.

Quando non siano presenti problemi di connessione e l'applicazione utente termina la connessione SignalR chiamando il `Stop` (metodo), la connessione SignalR e la connessione di trasporto iniziano e finiscono a nello stesso momento. Le sezioni seguenti descrivono in dettaglio gli altri scenari.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scenari di disconnessione del trasporto

Connessioni fisiche potrebbero risultare lente o potrebbero essere presenti interruzioni della connettività. A seconda di fattori quali la lunghezza dell'interruzione, la connessione di trasporto potrebbe essere eliminata. SignalR quindi tenta di ristabilire la connessione di trasporto. In alcuni casi l'API di connessione di trasporto rileva l'interruzione e rilascia la connessione di trasporto e SignalR scopre immediatamente che la connessione viene persa. In altri scenari, la connessione di trasporto API né SignalR viene reso compatibile con immediatamente che la connettività è stata persa. Per tutti i trasporti, ad eccezione di polling di lunga durata, il client SignalR utilizza una funzione denominata *keepalive* per verificare la presenza di una perdita di connettività, tra cui il trasporto API è in grado di rilevare. Per informazioni sulle connessioni di long polling, vedere [impostazioni di Timeout e keepalive](#timeoutkeepalive) più avanti in questo argomento.

Quando una connessione è inattiva, periodicamente il server invia un pacchetto di keepalive al client. Alla data che viene scritto questo articolo, la frequenza predefinita è ogni 10 secondi. Rimanendo in ascolto di questi pacchetti, i client possono indicare se si verifica un problema di connessione. Se un pacchetto di keepalive non viene ricevuto quando previsto, dopo un breve periodo di tempo il client presuppone che siano presenti problemi di connessione, ad esempio lentezza o interruzioni. Se il keepalive non viene comunque ricevuta dopo un periodo di tempo più lungo, il client presuppone che la connessione è stata eliminata, e inizia il tentativo di riconnessione.

Il diagramma seguente illustra gli eventi client e server che vengono generati in uno scenario tipico quando si verificano problemi con la connessione fisica che non sono immediatamente riconosciuti dal trasporto API. Il diagramma si applica a circostanze seguenti:

- Il trasporto è WebSockets, forever frame o gli eventi inviati al server.
- Esistono diversi periodi di interruzione della connessione di rete fisica.
- Il trasporto API non viene comunicato interruzioni, in modo da SignalR si basa sulla funzionalità di keepalive rilevarle.

![Disconnessioni di trasporto](handling-connection-lifetime-events/_static/image2.png)

Se il client passa alla modalità di riconnessione, ma non riesce a stabilire una connessione di trasporto entro il limite di timeout di disconnessione, il server termina la connessione di SignalR. In questo caso, il server esegue l'Hub `OnDisconnected` metodo e le code di un messaggio di disconnessione da inviare al client nel caso in cui il client riesce a connettersi più tardi. Se il client quindi ristabilire la connessione, riceve il comando di disconnessione e chiama il `Stop` (metodo). In questo scenario `OnReconnected` non viene eseguito quando il client si riconnette, e `OnDisconnected` non viene eseguito quando il client chiama `Stop`. Il diagramma seguente illustra questo scenario.

![Interruzioni di trasporto - timeout del server](handling-connection-lifetime-events/_static/image3.png)

Gli eventi di durata connessione SignalR che potrebbero essere generati nel client sono i seguenti:

- `ConnectionSlow` evento client.

    Generato quando una set di impostazioni proporzione del periodo di timeout di keepalive è trascorsi dall'ultimo messaggio o ping keepalive è stato ricevuto. Il periodo predefinito di avviso di timeout keepalive è 2 o 3 del timeout keepalive. Il timeout di keepalive è 20 secondi, in modo che l'avviso viene generato a circa 13 secondi.

    Per impostazione predefinita, il server invia ping keepalive ogni 10 secondi, e il client verifica ping keepalive su ogni 2 secondi (un terzo della differenza tra il valore di timeout di keepalive e il valore di avviso di timeout di keepalive).

    Se il trasporto API viene a conoscenza di una disconnessione, SignalR potrebbe essere informata della disconnessione prima del timeout di avviso keepalive passa. In tal caso, il `ConnectionSlow` eventi non vengono generati e SignalR verrebbe visualizzata direttamente la il `Reconnecting` evento.
- `Reconnecting` evento client.

    Generato quando l'API (a) il trasporto rileva che la connessione viene persa, o (b) il periodo di timeout di keepalive è trascorsi dall'ultimo messaggio o ping keepalive è stato ricevuto. Il codice del client SignalR inizia tentativo di riconnessione. Se si desidera che l'applicazione per eseguire qualche azione quando una connessione di trasporto viene persa, è possibile gestire questo evento. Attualmente, il periodo di timeout predefinito di keepalive è 20 secondi.

    Se il codice client prova a chiamare un metodo dell'Hub durante la riconnessione modalità SignalR, SignalR tenterà di inviare il comando. La maggior parte dei casi, tali tentativi avranno esito negativo, ma in alcuni casi si potrebbe avere esito positivo. Per il server ha inviato, forever frame ed eventi long polling trasporti, SignalR utilizza due canali di comunicazione, uno che il client utilizza per inviare messaggi e uno che utilizza per ricevere messaggi. Il canale utilizzato per la ricezione è quello aperto in modo permanente e che è quello che viene chiusa quando viene interrotta la connessione fisica. Il canale utilizzato per l'invio di rimane disponibile e in modo che se viene ripristinata la connettività fisica, una chiamata al metodo dal client al server potrebbe essere eseguita correttamente prima che il canale di ricezione viene ristabilito. Il valore restituito potrebbe non essere ricevuto fino a quando non SignalR riapre il canale utilizzato per la ricezione.
- `Reconnected` evento client.

    Generato quando viene ristabilita la connessione di trasporto. Il `OnReconnected` esegue il gestore eventi nell'Hub.
- `Closed` evento del client (`disconnected` eventi in JavaScript).

    Generato al termine del periodo di timeout di disconnessione mentre il codice del client SignalR è un tentativo di riconnessione dopo aver perso la connessione di trasporto. Disconnettere l'impostazione predefinita il timeout è 30 secondi. (Questo evento viene generato anche quando la connessione viene interrotta perché il `Stop` viene chiamato il metodo.)

Interruzioni di connessione di trasporto che non vengono rilevate dal trasporto API e non rimandare la ricezione di keepalive ping dal server per più tempo rispetto al periodo di avviso di timeout di keepalive potrebbero non causare la generazione di eventi di durata di qualsiasi connessione.

Alcuni ambienti di rete deliberatamente chiudono le connessioni inattive e un'altra funzione dei pacchetti di keepalive è per evitare questo problema consentendo di che queste reti sapere che una connessione SignalR è in uso. In casi estremi la frequenza predefinita di keepalive ping potrebbe non essere sufficiente a impedire connessioni chiuse. In tal caso è possibile configurare keepalive ping da inviare più spesso. Per altre informazioni, vedere [impostazioni di Timeout e keepalive](#timeoutkeepalive) più avanti in questo argomento.

> [!NOTE] 
> 
> **Importante**: la sequenza di eventi descritte di seguito non è garantita. SignalR esegue ogni tentativo di generare eventi di durata connessione in modo prevedibile in base a questo schema, ma esistono molte varianti degli eventi di rete e molti modi in cui Framework di comunicazione sottostante, ad esempio trasporto API gestirle. Ad esempio, il `Reconnected` evento potrebbe non essere generato quando il client si riconnette, o `OnConnected` gestore nel server è possibile eseguire quando il tentativo di stabilire una connessione ha esito negativo. In questo argomento descrive solo gli effetti che può essere prodotto generalmente da determinate circostanze tipiche.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scenari di disconnessione client

In un client browser, il codice client SignalR che mantiene una connessione SignalR viene eseguito nel contesto di una pagina web JavaScript. Che è il motivo per cui la connessione SignalR deve terminare quando si passa da una pagina a un'altra e tale del motivo per cui sono presenti più connessioni con ID di connessione più se ci si connette da più finestre del browser o nelle schede. Quando l'utente chiude una finestra del browser o una scheda, o si passa a una nuova pagina o aggiorna la pagina, la connessione SignalR termina immediatamente perché il codice client SignalR gestisce tale evento browser per l'utente e le chiamate di `Stop` (metodo). In questi scenari, o in qualsiasi piattaforma client quando l'applicazione chiama il `Stop` metodo, il `OnDisconnected` gestore eventi esegue immediatamente nel server e il client genera il `Closed` evento (l'evento è denominato `disconnected` in JavaScript).

Se un'applicazione client o il computer in cui è in esecuzione su arresti anomali o passa alla modalità di sospensione (ad esempio, quando l'utente chiude il portatile), il server non sia informato sui cosa è successo. Per quanto riguarda il server riconosce, la perdita del client potrebbe essere dovuto a un'interruzione della connettività e il client potrebbe tentare di ristabilire la connessione. Pertanto, in questi scenari il server è in attesa per dare la possibilità di ristabilire la connessione, il client e `OnDisconnected` non viene eseguito fino alla scadenza del periodo di timeout di disconnessione (circa 30 secondi per impostazione predefinita). Il diagramma seguente illustra questo scenario.

![Errore del computer client](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scenari di disconnessione del server

Quando un server è offline, viene riavviato, ha esito negativo, il dominio dell'applicazione comportano il riciclo e così via, il risultato potrebbe essere simile a una connessione è stata persa o l'API di trasporto e SignalR potrebbe rendersi immediatamente che il server non è più presente e SignalR potrebbe iniziare tentativo di riconnessione senza generazione di `ConnectionSlow` evento. Se il client passa alla modalità di riconnessione e se viene ripristinato il server o viene riavviata o un nuovo server viene portato online prima che scada il periodo di timeout di disconnessione, il client si riconnetterà al server nuovo o ripristinato. In tal caso, la connessione SignalR continua sul client e il `Reconnected` viene generato l'evento. Nel primo server, `OnDisconnected` non viene mai eseguito e nel nuovo server, `OnReconnected` viene eseguita anche se `OnConnected` è stata mai eseguita per tale client in tale server prima. (L'effetto è lo stesso se il client si riconnette nello stesso server dopo il riciclo del dominio un riavvio o app, perché al riavvio del server non ha memoria dell'attività di connessione precedente). Il diagramma seguente si presuppone che il trasporto API viene a conoscenza della connessione persa immediatamente, pertanto la `ConnectionSlow` non viene generato l'evento.

![Errore del server e la riconnessione](handling-connection-lifetime-events/_static/image5.png)

Se un server non diventa disponibile entro il periodo di timeout di disconnessione, termina la connessione di SignalR. In questo scenario, il `Closed` eventi (`disconnected` nel client JavaScript) viene generato nel client ma `OnDisconnected` non viene mai chiamato sul server. Il diagramma seguente si presuppone che il trasporto API non vengano rilevato la connessione è stata persa, in modo che è stato rilevato dalla funzionalità di keepalive SignalR e `ConnectionSlow` viene generato l'evento.

![Errore del server e timeout](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Impostazioni di timeout e keepalive

Il valore predefinito `ConnectionTimeout`, `DisconnectTimeout`, e `KeepAlive` valori appropriati per la maggior parte degli scenari, ma può essere modificati se l'ambiente ha particolari esigenze. Ad esempio, se l'ambiente di rete chiude le connessioni che sono inattive per 5 secondi, potrebbe essere necessario ridurre il valore di keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Questa impostazione rappresenta la quantità di tempo per lasciare una connessione di trasporto aperte e in attesa di una risposta prima di chiuderla e aprire una nuova connessione. Il valore predefinito è 110 secondi.

Questa impostazione si applica solo quando la funzionalità di keepalive è disabilitata, che in genere si applica solo a long trasporto di polling. Il diagramma seguente illustra l'effetto di questa impostazione su un valore long della connessione di trasporto di polling.

![Durata connessione di trasporto di polling](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Questa impostazione rappresenta la quantità di tempo di attesa dopo che una connessione di trasporto viene interrotta prima che venga generato il `Disconnected` evento. Il valore predefinito è 30 secondi. Quando si imposta `DisconnectTimeout`, `KeepAlive` viene impostata automaticamente su 1 o 3 del `DisconnectTimeout` valore.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Questa impostazione rappresenta la quantità di tempo di attesa prima dell'invio di un pacchetto di keepalive su una connessione inattiva. Il valore predefinito è 10 secondi. Questo valore non deve essere più di 1 o 3 del `DisconnectTimeout` valore.

Se si desidera impostare entrambe `DisconnectTimeout` e `KeepAlive`, impostare `KeepAlive` dopo `DisconnectTimeout`. In caso contrario, il `KeepAlive` impostazione verrà sovrascritto quando `DisconnectTimeout` imposta automaticamente `KeepAlive` a 1/3 del valore di timeout.

Se si desidera disabilitare la funzionalità di keepalive, impostare `KeepAlive` su null. La funzionalità di KeepAlive verrà disabilitata automaticamente per il valore long trasporto di polling.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Come modificare le impostazioni di timeout e keepalive

Per modificare i valori predefiniti per queste impostazioni, impostarle nelle `Application_Start` nella *Global. asax* file, come illustrato nell'esempio seguente. I valori mostrati nell'esempio di codice sono lo stesso come valori predefiniti.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Come inviare una notifica all'utente disconnessioni

In alcune applicazioni è possibile visualizzare un messaggio all'utente quando sono presenti problemi di connettività. Sono disponibili diverse opzioni per informazioni su come e quando eseguire questa operazione. Gli esempi di codice seguenti sono per un client JavaScript tramite il proxy generato.

- Gestire il `connectionSlow` evento per visualizzare un messaggio appena SignalR è a conoscenza di problemi di connessione, prima dell'aggiunta alla modalità di riconnessione.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Gestire il `reconnecting` evento per visualizzare un messaggio quando SignalR è a conoscenza di una disconnessione e si sta entrando in modalità di riconnessione.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Gestire il `disconnected` evento per visualizzare un messaggio quando un tentativo di riconnessione è scaduta. In questo scenario, l'unico modo per ristabilire una connessione con il server è necessario riavviare la connessione SignalR chiamando il `Start` metodo, che verrà creato un nuovo ID di connessione. Esempio di codice seguente viene usato un flag per assicurarsi che si esegue la notifica solo dopo un timeout di riconnessione, non dopo un normale fine per la connessione SignalR indotte chiamando il `Stop` (metodo).

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Modalità di riconnessione in modo continuo

In alcune applicazioni potrebbe voler automaticamente ristabilire una connessione dopo che è stata persa e il tentativo di riconnessione è scaduta. A tale scopo, è possibile chiamare il `Start` metodo dal `Closed` gestore dell'evento (`disconnected` gestore dell'evento nei client JavaScript). Si potrebbe voler attendere un periodo di tempo prima di chiamare `Start` per evitare questa operazione troppo frequentemente quando il server o la connessione fisica non sono disponibili. Esempio di codice seguente è per un client JavaScript tramite il proxy generato.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un potenziale problema da considerare nel client per dispositivi mobili è che i tentativi di riconnessione continua quando il server o una connessione fisica non è disponibile può causare inutili della batteria.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Come disconnettere un client nel codice server

SignalR versione 2 non è un server incorporato API per la disconnessione dei client. Esistono [piani per l'aggiunta di questa funzionalità in futuro](https://github.com/SignalR/SignalR/issues/2101). Nella versione di SignalR corrente, il modo più semplice per disconnettere un client dal server è per implementare un metodo di disconnessione nel client e chiamare il metodo dal server. Esempio di codice seguente viene illustrato un metodo di disconnessione per un client JavaScript tramite il proxy generato.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Security - questo metodo per la disconnessione dei client né l'API predefinito proposto si occuperanno lo scenario di attacco client che eseguono codice dannoso, poiché è stato possibile riconnettere i client o il codice di attacco potrebbe rimuovere il `stopClient` metodo o modifica cosa. Consente di implementare la protezione delle informazioni sullo stato di tipo denial of service (DOS). è non in framework o il livello del server, ma piuttosto nel front-end dell'infrastruttura.


<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Rilevare il motivo di una disconnessione

2.1 SignalR aggiunge un overload per il server `OnDisconnect` evento che indica se il client disconnesso deliberatamente piuttosto che verifichi il timeout. Il `StopCalled` parametro è true se il client ha chiuso in modo esplicito la connessione. In JavaScript, se un errore del server ha portato al client di disconnessione, le informazioni sull'errore verranno passati al client come `$.connection.hub.lastError`.

**Codice lato server c#: `stopCalled` parametro**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**Il codice JavaScript client: l'accesso a `lastError` nella `disconnect` evento.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
