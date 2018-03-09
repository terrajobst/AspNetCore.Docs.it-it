---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introduzione a SignalR | Documenti Microsoft
author: pfletcher
description: "Questo articolo descrive SignalR è e alcune delle soluzioni che è stato progettato per creare."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5bb49c9c2405d232ba5e067d99f8879b3bc99361
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2018
---
<a name="introduction-to-signalr"></a>Introduzione a SignalR
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> Questo articolo descrive SignalR è e alcune delle soluzioni che è stato progettato per creare. 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).


## <a name="what-is-signalr"></a>Che cos'è SignalR?

ASP.NET SignalR è una libreria per sviluppatori ASP.NET che semplifica il processo di aggiunta di funzionalità web in tempo reale alle applicazioni. Funzionalità web in tempo reale è la possibilità di push del codice server contenuto ai client connessi immediatamente appena sarà disponibile, anziché il server di attendere che un client di richiedere nuovi dati.

SignalR può essere utilizzato per aggiungere una sorta di funzionalità web "in tempo reale" per l'applicazione ASP.NET. Mentre chat viene spesso utilizzata come esempio, è possibile eseguire più tanto. Ogni volta che un utente aggiorna una pagina web per visualizzare i nuovi dati o la pagina implementa [polling lungo](http://en.wikipedia.org/wiki/Push_technology#Long_polling) per recuperare nuovi dati, è un candidato per l'utilizzo di SignalR. Esempi includono i dashboard e monitoraggio delle applicazioni, applicazioni di collaborazione (ad esempio la modifica simultanea di documenti), gli aggiornamenti di stato di avanzamento e il form in tempo reale.

SignalR consente inoltre di completamente nuovi tipi di applicazioni web che richiedono aggiornamenti ad alta frequenza dal server, ad esempio, giochi in tempo reale.

SignalR fornisce un'API semplice per la creazione di chiamate di server a client di procedura remota (RPC) che chiamano funzioni JavaScript nel client browser (e altre piattaforme client) dal codice .NET sul lato server. SignalR include inoltre API per la gestione connessione (ad esempio, connettere e disconnettere eventi) e il raggruppamento delle connessioni.

![Chiamata di metodi con SignalR](introduction-to-signalr/_static/image1.png)

SignalR gestisce automaticamente la gestione della connessione e consente di messaggi trasmessi a tutti i client connessi contemporaneamente, ad esempio una chat room. È anche possibile inviare messaggi ai client specifico. La connessione tra il client e server è persistente, a differenza di una connessione HTTP classica, che sarà stata ristabilita per tutte le comunicazioni.

SignalR supporta la funzionalità "push" server", in cui è possibile chiamare codice server out al codice client nel browser utilizzando le chiamate RPC (Remote Procedure), anziché il modello di richiesta-risposta comune oggi sul web.

Applicazioni SignalR è possono scalare orizzontalmente a migliaia di client che utilizzano Service Bus, SQL Server o [Redis](http://redis.io).

SignalR è open source, accessibile tramite [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR e WebSocket

SignalR utilizza il trasporto WebSocket, ove disponibili e il fallback ai trasporti precedenti se necessario. Mentre è certo che sia possibile scrivere l'applicazione direttamente WebSocket, mediante SignalR significa che molte funzionalità aggiuntive, che è necessario implementare saranno già essere stata eseguita per l'utente. In particolare, ciò significa che è possibile codificare l'applicazione in modo da sfruttare WebSocket senza doversi preoccupare di creazione di un percorso di codice separato per i client meno recenti. SignalR protegge anche da doversi preoccupare degli aggiornamenti per WebSocket, poiché SignalR continueranno a essere aggiornate per supportare le modifiche nel trasporto sottostante, fornendo all'applicazione un'interfaccia coerente tra le versioni di WebSocket.

Mentre è certamente possibile creare una soluzione che utilizza i WebSocket solo, SignalR fornisce tutte le funzionalità che necessarie per scrivere manualmente, ad esempio per altri trasporti e la revisione di applicazione per gli aggiornamenti per le implementazioni di WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Trasporti e fallback

SignalR è un'astrazione su alcuni dei trasporti che sono necessari per svolgere il lavoro in tempo reale tra client e server. Una connessione SignalR viene avviato come HTTP e quindi viene promossa a una connessione WebSocket, se disponibile. WebSocket è il trasporto ideale per SignalR, in quanto consente l'utilizzo più efficiente della memoria del server, è la latenza più bassa e presenta le caratteristiche più sottostante (ad esempio, la comunicazione full duplex tra client e server), ma dispone anche di quello più rigoroso requisiti: WebSocket richiede al server di disporre di .NET Framework 4.5 o Windows 8 e Windows Server 2012. Se non vengono soddisfatti questi requisiti, verrà effettuato un tentativo di utilizzare altri trasporti per rendere le connessioni SignalR.

### <a name="html-5-transports"></a>I trasporti HTML 5

Questi trasporti dipendono dal supporto per [HTML 5](http://en.wikipedia.org/wiki/HTML5). Se il browser client non supporta lo standard HTML 5, i trasporti precedenti verranno utilizzati.

- **WebSocket** (se il browser sia il server indicare possono supportare Websocket). WebSocket è il trasporto solo che stabilisce una connessione permanente, bidirezionale true tra client e server. Tuttavia, WebSocket ha i requisiti più rigidi; è supportato solo nelle versioni più recenti di Microsoft Internet Explorer e Google Chrome, Mozilla Firefox e ha solo un'implementazione parziale negli altri browser, ad esempio Opera e Safari.
- **Eventi del server inviati**, noto anche come EventSource (se il browser supporta inviati eventi del Server, ovvero tutti i browser, ad eccezione di Internet Explorer).

### <a name="comet-transports"></a>Trasporti Comet

I trasporti seguenti si basano le [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) modello dell'applicazione web, in cui un browser o altri client gestisce una richiesta HTTP mantenuto prolungata, che il server è possibile utilizzare per il push dei dati al client senza il client in modo specifico con la richiesta.

- **Forever Frame** (per Internet Explorer solo). Forever Frame crea un IFrame nascosto che effettua una richiesta a un endpoint nel server che non viene completata. Il server invia continuamente script al client che viene eseguito immediatamente, fornendo una connessione in tempo reale unidirezionale dal server al client. Ad esempio una richiesta HTTP standard, viene creata una nuova connessione per ogni blocco di dati che devono essere inviata la connessione dal client al server utilizza una connessione separata dal server di connessione client.
- **Polling prolungato AJAX**. Polling prolungato non crea una connessione permanente, ma invece di polling del server con una richiesta che resti aperta fino a quando il server risponde, a quel punto viene chiusa la connessione e una nuova connessione è richiesta immediatamente. Questa configurazione può comportare un'eventuale latenza durante la connessione viene reimpostato.

Per ulteriori informazioni su quali trasporti sono supportati con le configurazioni, vedere [piattaforme supportate](supported-platforms.md).

### <a name="transport-selection-process"></a>Processo di selezione di trasporto

Nell'elenco seguente vengono illustrati i passaggi che SignalR utilizza per stabilire il trasporto da utilizzare.

1. Se il browser Internet Explorer 8 o versioni precedenti, il tempo di Polling viene utilizzato.
2. Se JSONP è configurato (vale a dire il `jsonp` parametro è impostato su `true` quando la connessione è stata avviata), di Polling lungo viene utilizzato.
3. Se una connessione tra domini è stata effettuata (ovvero, se l'endpoint di SignalR non è presente nello stesso dominio della pagina di hosting), verrà utilizzato WebSocket se vengono soddisfatti i seguenti criteri:

    - Il client supporta CORS (Cross-Origin Resource Sharing). Per informazioni dettagliate in cui i client supportano CORS, vedere [CORS in caniuse.com](http://www.caniuse.com/CORS).
    - Il client supporta WebSocket
    - Il server supporta WebSocket

    Se uno di questi criteri non vengono soddisfatte, verrà utilizzato il Polling lungo. Per ulteriori informazioni sulle connessioni tra domini, vedere [per stabilire una connessione tra domini](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Se la connessione non è tra domini JSONP non è configurato, verrà utilizzato WebSocket se supporta client e server.
5. Se il client o il server non supporta WebSocket, gli eventi inviati Server viene utilizzato se disponibile.
6. Se gli eventi del Server inviato non è disponibili, viene tentata Forever Frame.
7. Se Forever Frame non riesce, viene utilizzato il tempo di Polling.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Trasporti di monitoraggio

È possibile determinare il trasporto utilizzato dall'applicazione, consentendo l'accesso dell'hub e l'apertura della finestra console del browser.

Per abilitare la registrazione per gli eventi dell'hub in un browser, aggiungere il seguente comando per l'applicazione client:

`$.connection.hub.logging = true;`

- Aprire gli strumenti di sviluppo premendo F12 in Internet Explorer e fare clic sulla scheda della Console.

    ![Console di Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- In Chrome, aprire la console, premere Ctrl + MAIUSC + J.

    ![Console di Google Chrome](introduction-to-signalr/_static/image3.png)

Con la console aperta e abilitata la registrazione, sarà in grado di visualizzare il trasporto è utilizzato da SignalR.

![Console in Internet Explorer con il trasporto WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Specifica un trasporto

La negoziazione di un trasporto richiede una certa quantità di tempo e client/server di risorse. Se sono disponibili le funzionalità client, può specificare un trasporto quando viene avviata la connessione client. Frammento di codice riportato di seguito viene illustrato come avviare una connessione utilizzando il trasporto di Polling lungo Ajax, verrebbe usato se è noto che il client non supporta tutti gli altri protocolli:

`connection.start({ transport: 'longPolling' });`

Se si desidera un client tenta di trasporti specifici in ordine, è possibile specificare un ordine di fallback. Frammento di codice riportato di seguito viene illustrato come WebSocket durante il tentativo e in mancanza di, passare direttamente al Polling lungo.

`connection.start({ transport: ['webSockets','longPolling'] });`

Le costanti di stringa per la specifica di trasporti sono definite come segue:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Le connessioni e hub

L'API SignalR contiene due modelli per la comunicazione tra client e server: connessioni permanenti e hub.

Una connessione rappresenta un endpoint di tipo semplice per l'invio di messaggi raggruppato, broadcast o singolo destinatario. Consente di API di connessione permanente (rappresentato dalla classe PersistentConnection nel codice .NET) lo sviluppatore accesso diretto al protocollo di comunicazione di basso livello che espone SignalR. Utilizzando il modello di comunicazione connessioni risulteranno familiare agli sviluppatori che hanno usato l'API basato sulla connessione, ad esempio Windows Communication Foundation.

Un Hub è una pipeline più alto livello basata sull'API di connessione che consente il client e server chiamare i metodi direttamente tra loro. SignalR gestisce la distribuzione tra limiti di computer come se fosse magic, consentendo ai client di chiamare metodi sul server come facilmente come metodi locali e viceversa. Utilizzando il modello di comunicazione hub risulteranno familiare agli sviluppatori che hanno usato la chiamata remota, ad esempio servizi remoti .NET. Utilizzo di un Hub consente inoltre di passare parametri fortemente tipizzati per metodi che consentono l'associazione del modello.

### <a name="architecture-diagram"></a>Diagramma dell'architettura

Il diagramma seguente mostra la relazione tra hub, le connessioni permanenti e le tecnologie sottostanti utilizzate per i trasporti.

![Diagramma dell'architettura SignalR con le API, i trasporti e i client](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Funzionamento di hub

Quando il codice lato server chiama un metodo nel client, viene inviato un pacchetto attraverso il trasporto attivo che contiene il nome e i parametri del metodo da chiamare (quando un oggetto viene inviato come parametro di metodo, viene serializzato usando JSON). Quindi, il client corrisponde al nome di metodo ai metodi definiti nel codice sul lato client. Se viene trovata una corrispondenza, verrà eseguito il metodo di client utilizzando i dati del parametro deserializzato.

La chiamata al metodo può essere monitorata mediante strumenti quali [Fiddler.](http://fiddler2.com/) La figura seguente mostra una chiamata al metodo inviata da un server di SignalR a un client browser web nel riquadro di registri Fiddler. La chiamata al metodo verrà inviata da un hub chiamato `MoveShapeHub`, e viene chiamato il metodo richiamato `updateShape`.

![Visualizzazione del Registro di Fiddler che mostra il traffico di SignalR](introduction-to-signalr/_static/image6.png)

In questo esempio, il nome dell'hub viene identificato con il `H` parametro; il metodo nome viene identificato con il `M` parametro e i dati inviati al metodo viene identificato con il `A` parametro. L'applicazione che ha generato questo messaggio viene creato nel [in tempo reale ad alta frequenza](tutorial-high-frequency-realtime-with-signalr.md) esercitazione.

### <a name="choosing-a-communication-model"></a>Scelta di un modello di comunicazione

La maggior parte delle applicazioni devono usare l'API di hub. L'API di connessioni possono essere utilizzata nelle circostanze seguenti:

- Il formato di deve essere specificato il messaggio effettivo inviato.
- Lo sviluppatore si preferisce utilizzare un modello di messaggistica e di distribuzione anziché un modello di chiamata remota.
- Un'applicazione esistente che utilizza un modello di messaggistica viene trasferita per l'utilizzo di SignalR.
