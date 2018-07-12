---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introduzione a SignalR | Microsoft Docs
author: pfletcher
description: Questo articolo descrive che cos'è SignalR e alcune delle soluzioni che è stato progettato per creare.
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 2f4353e88287f0716967362e255d94df0c522475
ms.sourcegitcommit: 260abb706ed17f07a53288d8a0c3e69fc13e7468
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38966744"
---
<a name="introduction-to-signalr"></a>Introduzione a SignalR
====================

È disponibile una versione aggiornata di questa esercitazione [qui](/aspnet/core/tutorials/signalr) usando la versione più recente di Visual Studio. Usa la nuova esercitazione [ASP.NET Core](/aspnet/core/), che offre numerosi miglioramenti in questa esercitazione.

da [Patrick Fletcher](https://github.com/pfletcher)

> Questo articolo descrive che cos'è SignalR e alcune delle soluzioni che è stato progettato per creare. 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).


## <a name="what-is-signalr"></a>Che cos'è SignalR?

ASP.NET SignalR è una libreria per sviluppatori ASP.NET che semplifica il processo di aggiunta di funzionalità web in tempo reale alle applicazioni. La funzionalità web in tempo reale è la possibilità di push del codice server contenuto ai client connessi immediatamente appena sarà disponibile, anziché il server di attendere che un client richieda nuovi dati.

SignalR è utilizzabile per aggiungere una sorta di funzionalità web "in tempo reale" per l'applicazione ASP.NET. Mentre chat viene spesso usata come esempio, è possibile eseguire molto di più. Ogni volta che un utente consente di aggiornare una pagina web per visualizzare i nuovi dati o la pagina implementa [polling lungo](http://en.wikipedia.org/wiki/Push_technology#Long_polling) per recuperare nuovi dati, è un candidato per l'uso di SignalR. Gli esempi includono i dashboard e monitoraggio delle applicazioni, dei processi applicazioni collaborative (ad esempio, la modifica simultanea di documenti), gli aggiornamenti di stato di avanzamento e forme in tempo reale.

SignalR consente inoltre completamente nuovi tipi di applicazioni web che richiedono aggiornamenti ad alta frequenza dal server, ad esempio, i giochi in tempo reale.

SignalR fornisce un'API semplice per la creazione di server a client remote procedure call (RPC) che chiamano le funzioni di JavaScript nel client browser (e altre piattaforme client) dal codice .NET sul lato server. SignalR include API per la gestione della connessione (ad esempio, connettere e disconnettere gli eventi) e il raggruppamento delle connessioni.

![Richiamare metodi con SignalR](introduction-to-signalr/_static/image1.png)

SignalR gestisce automaticamente la gestione della connessione e consente di trasmettere messaggi a tutti i client connessi contemporaneamente, ad esempio una chat room. È anche possibile inviare messaggi a client specifici. La connessione tra il client e server è permanente, a differenza di una connessione HTTP classica, che sarà stata ristabilita per tutte le comunicazioni.

SignalR supporta le funzionalità di "push del server", in cui è possibile chiamare codice server out al codice client nel browser tramite le chiamate RPC (Remote Procedure), anziché il modello di richiesta-risposta comune nel web oggi stesso.

Applicazioni SignalR possono aumentare fino a migliaia di client tramite il Bus di servizio, SQL Server o [Redis](http://redis.io).

SignalR è open source, accessibile attraverso [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR e WebSocket

SignalR utilizza il trasporto WebSocket nuova in cui è disponibile e il fallback ai trasporti precedenti dove necessario. Sebbene sia certamente possibile scrivere l'applicazione utilizza i WebSocket direttamente, tramite SignalR significa che saranno già molte delle funzionalità aggiuntive da implementare è stata eseguita è. In particolare, ciò significa che è possibile codificare l'applicazione possa sfruttare i vantaggi di WebSocket senza doversi preoccupare di creazione di un percorso di codice separato per i client meno recenti. SignalR anche ci consente di nascondere doversi preoccupare degli aggiornamenti per WebSocket, poiché SignalR continueranno a essere aggiornato per supportare le modifiche nel trasporto sottostante, che fornisce all'applicazione un'interfaccia coerente in tutte le versioni di WebSocket.

Mentre è certamente possibile creare una soluzione usando WebSocket da solo, SignalR fornisce tutte le funzionalità che è necessario scrivere personalmente, ad esempio il fallback per altri tipi di trasporto e modifica l'applicazione degli aggiornamenti per le implementazioni di WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Trasporti e i fallback

SignalR è un'astrazione su alcuni dei trasporti che sono necessari per svolgere il lavoro in tempo reale tra client e server. Una connessione SignalR inizia come HTTP e viene quindi promossa a una connessione WebSocket, se disponibile. WebSocket è il trasporto ideale per SignalR, poiché consente di ottimizzare l'uso della memoria del server, con la latenza più bassa e presenta le funzionalità più sottostante (ad esempio comunicazione full duplex tra client e server), ma include anche i più rigorosi requisiti: WebSocket richiede che il server di usare Windows Server 2012 o Windows 8 e .NET Framework 4.5. Se questi requisiti non vengono soddisfatti, verrà effettuato un tentativo di utilizzare altri tipi di trasporto per rendere le connessioni SignalR.

### <a name="html-5-transports"></a>HTML 5 trasporta

Supporto per dipendono da questi trasporti [HTML 5](http://en.wikipedia.org/wiki/HTML5). Se il browser client non supporta lo standard HTML 5, verranno utilizzati trasporti meno recenti.

- **WebSocket** (se il browser sia nel server indicano possono supportare Websocket). WebSocket è il trasporto solo che stabilisce una connessione permanente, bidirezionale true tra client e server. Tuttavia, WebSocket presenta anche i più rigorosi requisiti; si è completamente supportato solo nelle versioni più recenti di Microsoft Internet Explorer, Google Chrome e Mozilla Firefox e offre solo un'implementazione parziale negli altri browser, ad esempio Opera e Safari.
- **Eventi del server inviati**, noto anche come EventSource (se il browser supporta inviati eventi del Server, che è in pratica tutti i browser, ad eccezione di Internet Explorer).

### <a name="comet-transports"></a>Trasporti Comet

Trasporti seguenti si basano le [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) modello di applicazione web, in cui un browser o da altri client gestisce una richiesta HTTP mantenuto prolungata, che il server può usare per effettuare il push dei dati al client senza il client in modo specifico con la richiesta.

- **Forever Frame** (per Internet Explorer solo). Forever Frame consente di creare un IFrame nascosto che effettua una richiesta a un endpoint nel server che non viene completata. Il server invia continuamente script al client che viene eseguito immediatamente, fornendo una connessione in tempo reale unidirezionale dal server al client. La connessione da client a server utilizza una connessione separata dal server di connessione client, ad esempio una richiesta HTTP standard, viene creata una nuova connessione per ogni porzione di dati che devono essere inviate.
- **AJAX polling prolungato**. Polling prolungato non crea una connessione permanente, ma invece esegue il polling del server con una richiesta che rimane aperto fino a quando il server risponde, a questo punto la connessione verrà chiusa e viene richiesto immediatamente una nuova connessione. Questo può comportare una certa latenza durante la connessione viene reimpostato.

Per altre informazioni su quali trasporti sono supportati con le configurazioni, vedere [piattaforme supportate](supported-platforms.md).

### <a name="transport-selection-process"></a>Processo di selezione del trasporto

Nell'elenco seguente vengono illustrati i passaggi che usa SignalR per decidere quale trasporto per utilizzare.

1. Se il browser Internet Explorer 8 o versioni precedenti, viene usato di Polling lungo.
2. Se JSONP è configurato (vale a dire il `jsonp` parametro è impostato su `true` quando viene avviata la connessione), Long Polling viene usato.
3. Se viene effettuata una connessione cross-domain (vale a dire, se l'endpoint di SignalR non è presente nello stesso dominio della pagina di hosting), quindi WebSocket verrà utilizzato se vengono soddisfatti i criteri seguenti:

   - Il client supporta la condivisione CORS (Cross-Origin Resource Sharing). Per informazioni dettagliate in cui i client supportano CORS, vedere [CORS in caniuse.com](http://www.caniuse.com/CORS).
   - Il client supporta i WebSocket
   - Il server supporta i WebSocket

     Se i criteri seguenti non vengono soddisfatti, verrà utilizzato di Polling lungo. Per altre informazioni sulle connessioni tra domini, vedere [come stabilire una connessione cross-domain](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Se JSONP non è configurata e la connessione non è tra domini, WebSocket verrà utilizzato se il client e server lo supportano.
5. Se il client o il server non supportano WebSocket, inviati gli eventi del Server viene utilizzato se è disponibile.
6. Se gli eventi del Server inviato non è disponibili, viene tentata Forever Frame.
7. Se Forever Frame non riesce, viene usato di Polling lungo.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>I trasporti di monitoraggio

È possibile determinare quali il trasporto utilizzato dall'applicazione, consentendo l'accesso all'hub e l'apertura della finestra console del browser.

Per abilitare la registrazione per gli eventi dell'hub in un browser, aggiungere il seguente comando per l'applicazione client:

`$.connection.hub.logging = true;`

- Aprire gli strumenti di sviluppo premendo F12 in Internet Explorer e fare clic sulla scheda della Console.

    ![Console di Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- In Chrome, aprire la console, premere Ctrl + MAIUSC + J.

    ![Console di Google Chrome](introduction-to-signalr/_static/image3.png)

Apri console e la registrazione attivata, sarà possibile visualizzare quale trasporto è utilizzato da SignalR.

![Console di Internet Explorer che mostra che il trasporto WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Specifica un trasporto

La negoziazione di un trasporto richiede una certa quantità di tempo e il client/server delle risorse. Se le funzionalità client sono note, quindi un trasporto è possibile specificare quando viene avviata la connessione client. Il frammento di codice seguente viene illustrato come avviare una connessione usando il trasporto di Polling lungo Ajax, come verrebbero utilizzati qualora si era noto che il client non supporta tutti gli altri protocolli:

`connection.start({ transport: 'longPolling' });`

Se si desidera che un client di provare a specifici trasporti in ordine, è possibile specificare un ordine di fallback. Il frammento di codice seguente viene illustrato tentativo WebSocket e in mancanza di questo, passare direttamente al Polling lungo.

`connection.start({ transport: ['webSockets','longPolling'] });`

Le costanti stringa che specificano i trasporti sono definite come segue:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Le connessioni e hub

L'API SignalR contiene due modelli per la comunicazione tra client e server: le connessioni persistenti e gli hub.

Una connessione rappresenta un endpoint semplice per l'invio di messaggi raggruppato, trasmissione o singolo destinatario. Consente di API di connessione permanente (rappresentato nel codice .NET dalla classe PersistentConnection) accedere al protocollo di comunicazione di basso livello che SignalR espone direttamente allo sviluppatore. Usando il modello di comunicazione connessioni saranno nota agli sviluppatori che hanno usato le API basate su connessione, ad esempio Windows Communication Foundation.

Un Hub è una pipeline più alto livello basata sull'API di connessione che consente il client e server di chiamare direttamente i metodi a vicenda. SignalR gestisce l'invio attraverso i limiti della macchina come se fosse magic, consentendo ai client di chiamare metodi sul server come facilmente come metodi locali e viceversa. Usando il modello di comunicazione hub risulteranno familiare agli sviluppatori che hanno usato la chiamata remota, API, ad esempio servizi remoti .NET. Usando un Hub consente inoltre di passare parametri fortemente tipizzati a metodi per consentire l'associazione di modelli.

### <a name="architecture-diagram"></a>Diagramma dell'architettura

Il diagramma seguente mostra la relazione tra hub, le connessioni persistenti e delle tecnologie sottostanti usate per i trasporti.

![Diagramma dell'architettura SignalR con le API, trasporti e i client](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Funzionamento di hub

Quando il codice lato server chiama un metodo sul client, viene inviato un pacchetto attraverso il trasporto attivo che contiene il nome e parametri del metodo da chiamare (quando un oggetto viene inviato come parametro di metodo, viene serializzato usando JSON). Il client quindi corrisponde al nome di metodo ai metodi definiti nel codice lato client. Se non esiste una corrispondenza, verrà eseguito il metodo client usando i dati del parametro deserializzato.

La chiamata al metodo può essere monitorata mediante strumenti come [Fiddler.](http://fiddler2.com/) L'immagine seguente illustra una chiamata di metodo inviata da un server di SignalR per un client browser web nel riquadro dei log di Fiddler. La chiamata al metodo inviata da un hub chiamato `MoveShapeHub`, e viene chiamato il metodo richiamato `updateShape`.

![Visualizzazione dei log di Fiddler che mostra il traffico di SignalR](introduction-to-signalr/_static/image6.png)

In questo esempio, il nome dell'hub viene identificato con il `H` parametro; il metodo nome viene identificato con il `M` parametro e i dati inviati al metodo viene identificato con il `A` parametro. L'applicazione che ha generato questo messaggio viene creato nel [in tempo reale ad alta frequenza](tutorial-high-frequency-realtime-with-signalr.md) esercitazione.

### <a name="choosing-a-communication-model"></a>Scelta di un modello di comunicazione

La maggior parte delle applicazioni deve usare l'API di hub. L'API di connessioni può essere usata nelle circostanze seguenti:

- Il formato di deve essere specificato il messaggio effettivo inviato.
- Lo sviluppatore si preferisce usare un modello di messaggistica e dispatching anziché a un modello di chiamata remota.
- Un'applicazione esistente che usa un modello di messaggistica è in corso trasferita per usare SignalR.
