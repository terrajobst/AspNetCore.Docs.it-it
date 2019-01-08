---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Esercitazione: Trasmissione server con SignalR 2 | Microsoft Docs'
author: tdykstra
description: Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di trasmissione di server.
ms.author: riande
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a6014e604613492db91b2dc6f846c3c73d938d99
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099299"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Esercitazione: Server di trasmissione con SignalR 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di trasmissione di server. Trasmissione del server significa che il server inizia le comunicazioni inviate ai client.

L'applicazione che verrà creata in questa esercitazione simula le quotazioni di borsa, uno scenario tipico per la funzionalità di trasmissione di server. Periodicamente, il server in modo casuale degli aggiornamenti di quotazioni di titoli e trasmissione gli aggiornamenti a tutti i client connessi. Nel browser, i numeri e simboli nel **cambiare** e **%** colonne modificate in modo dinamico in risposta a notifiche dal server. Se si apre browser aggiuntivi allo stesso URL, sono visualizzate le stesse modifiche ai dati e gli stessi dati contemporaneamente.

![Creazione di web](tutorial-server-broadcast-with-signalr/_static/image1.png)

In questa esercitazione è:

> [!div class="checklist"]
> * Creare il progetto
> * Impostare il codice server
> * Esaminare il codice del server
> * Impostare il codice client
> * Esaminare il codice client
> * Testare l'applicazione
> * Abilitare la registrazione

> [!IMPORTANT]
> Se non si desidera eseguire i passaggi di compilazione dell'applicazione, è possibile installare il pacchetto SignalR.Sample in un nuovo progetto di applicazione Web ASP.NET vuota. Se si installa il pacchetto NuGet senza eseguire i passaggi descritti in questa esercitazione, è necessario seguire le istruzioni riportate nel *Readme. txt* file. Per eseguire il pacchetto è necessario aggiungere una OWIN startup classe che chiama il `ConfigureSignalR` metodo nel pacchetto installato. Si riceverà un errore se non si aggiunge la classe di avvio OWIN. Vedere le [installare il campione StockTicker](#install-the-stockticker-sample) sezione di questo articolo.


## <a name="prerequisites"></a>Prerequisiti

 * [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il **sviluppo ASP.NET e web** carico di lavoro.

## <a name="create-the-project"></a>Creare il progetto

Questa sezione illustra come usare Visual Studio 2017 per creare un'applicazione Web ASP.NET vuota.

1. In Visual Studio, creare un'applicazione Web ASP.NET.

    ![Creazione di web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. Nel **nuova applicazione Web ASP.NET - SignalR.StockTicker** finestra, lasciare **vuota** selezionato e selezionare **OK**.

## <a name="set-up-the-server-code"></a>Impostare il codice server

In questa sezione è impostare il codice eseguito nel server.

### <a name="create-the-stock-class"></a>Creare la classe Stock

È necessario creare innanzitutto le *Stock* classe che si userà per archiviare e trasmettere informazioni di riferimento di un modello.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **classe**.

1. Denominare la classe *Stock* e aggiungerlo al progetto.

1. Sostituire il codice nel *Stock.cs* file con il seguente codice:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Sono le due proprietà che si imposteranno quando si crea stocks `Symbol` (ad esempio, MSFT per Microsoft) e `Price`. Le altre proprietà dipende da come e quando si imposta `Price`. La prima volta è impostato `Price`, il valore ottiene propagato a `DayOpen`. Successivamente, quando si imposta `Price`, l'app viene calcolato il `Change` e `PercentChange` i valori delle proprietà in base alla differenza tra `Price` e `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Creare le classi StockTickerHub e StockTicker

Si userà l'API di Hub di SignalR per gestire l'interazione di server a client. Oggetto `StockTickerHub` classe che deriva dal `SignalRHub` classe gestirà la ricezione di chiamate al metodo e le connessioni dai client. È anche necessario mantenere i dati azionari ed eseguire un `Timer` oggetto. Il `Timer` oggetto attiverà periodicamente aggiornamenti prezzo indipendenti di connessioni client. È possibile inserire queste funzioni in un `Hub` classe, poiché gli hub sono temporanei. L'app crea un `Hub` istanza della classe per ogni attività nell'hub, come le connessioni e chiamate dal client al server. In modo che il meccanismo che mantiene i dati azionari, aggiorna i prezzi e trasmette gli aggiornamenti di prezzo deve essere eseguito in una classe separata. È possibile denominare la classe `StockTicker`.

![La trasmissione da StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Si vuole solo un'istanza del `StockTicker` classe per l'esecuzione nel server, quindi è necessario configurare un riferimento da ognuna `StockTickerHub` istanza per il singleton `StockTicker` istanza. Il `StockTicker` classe deve trasmettere ai client ha dati azionari e attiva gli aggiornamenti, ma `StockTicker` non è un `Hub` classe. Il `StockTicker` classe deve ottenere un riferimento all'oggetto di contesto di connessione del SignalR Hub. Quindi possibile usare l'oggetto di contesto di connessione SignalR per trasmettere ai client.

#### <a name="create-stocktickerhubcs"></a>Creare StockTickerHub.cs

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.

1. Nelle **Aggiungi nuovo elemento - SignalR.StockTicker**, selezionare **installati** > **Visual C#**   >  **Web**  >  **SignalR** e quindi selezionare **classe tramite SignalR Hub (v2)**.

1. Denominare la classe *StockTickerHub* e aggiungerlo al progetto.

    Questo passaggio Crea il *StockTickerHub.cs* file di classe. Contemporaneamente, aggiunge un set di file di script e i riferimenti agli assembly che supporta SignalR al progetto.

1. Sostituire il codice nel *StockTickerHub.cs* file con il seguente codice:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Salvare il file.

L'app Usa la [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) classe per definire metodi che i client possono chiamare sul server. Si definisce un metodo: `GetAllStocks()`. Quando un client si connette inizialmente al server, chiama questo metodo per ottenere un elenco di tutti i titoli di borsa con i relativi prezzi correnti. Il metodo può eseguire in modo sincrono e restituire `IEnumerable<Stock>` perché restituisce i dati dalla memoria.

Se il metodo deve ottenere i dati in questo modo qualcosa che richiederebbe l'intervento di attesa, ad esempio una ricerca nel database o una chiamata al servizio web, si specificherà `Task<IEnumerable<Stock>>` come valore restituito per abilitare l'elaborazione asincrona. Per altre informazioni, vedere [Guida all'API di hub di ASP.NET SignalR - Server - quando è necessario eseguire in modo asincrono](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

Il `HubName` attributo specifica la modalità l'app verrà fatto riferimento l'Hub nel codice JavaScript nel client. Il nome predefinito sul client se non si usa questo attributo, è una versione camelCase del nome della classe, che in questo caso sarebbe `stockTickerHub`.

Come si vedrà in seguito quando si crea il `StockTicker` (classe), l'app crea un'istanza singleton della classe in statica `Instance` proprietà. Istanza singleton di `StockTicker` in memoria, indipendentemente dal numero di client di connessione o disconnessione. Tale istanza è quello che fa il `GetAllStocks()` metodo utilizzato per restituire informazioni sulle azioni corrente.

#### <a name="create-stocktickercs"></a>Creare StockTicker.cs

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **classe**.

1. Denominare la classe *StockTicker* e aggiungerlo al progetto.

1. Sostituire il codice nel *StockTicker.cs* file con il seguente codice:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Poiché tutti i thread verranno eseguita la stessa istanza del codice StockTicker, la classe StockTicker deve essere thread-safe.

### <a name="examine-the-server-code"></a>Esaminare il codice del server

Se si esamina il codice del server, aiuterà a comprendere il funzionamento dell'app.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Archiviare l'istanza singleton in un campo statico

Il codice inizializza il metodo statico `_instance` campo sottostante il `Instance` proprietà con un'istanza della classe. Poiché il costruttore è privato, è l'unica istanza della classe in grado di creare l'app. L'app Usa [inizializzazione differita](/dotnet/framework/performance/lazy-initialization) per il `_instance` campo. Non è per motivi di prestazioni. È per assicurarsi che la creazione dell'istanza è thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Ogni volta che un client si connette al server, una nuova istanza della classe StockTickerHub in esecuzione in un thread separato Ottiene l'istanza singleton di StockTicker dal `StockTicker.Instance` proprietà statica, come illustrato in precedenza nel `StockTickerHub` classe.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Archiviazione dei dati predefinite in un oggetto ConcurrentDictionary

Il costruttore inizializza la `_stocks` raccolta con alcuni dati di esempio azionario, e `GetAllStocks` restituisce i titoli di borsa. Come illustrato in precedenza, questa raccolta di titoli di borsa viene restituita da `StockTickerHub.GetAllStocks`, che è un metodo server il `Hub` classe che i client possono chiamare.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

La raccolta di titoli azionari è definita come un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) tipo per la sicurezza dei thread. In alternativa, è possibile usare una [dizionario](https://msdn.microsoft.com/library/xfhwa508.aspx) dell'oggetto e il dizionario di blocco in modo esplicito quando si apportano modifiche a esso.

Per questa applicazione di esempio, è bene per archiviare i dati dell'applicazione in memoria e i dati andranno persi quando l'app Elimina il `StockTicker` istanza. In un'applicazione reale, si procederà con un archivio dati back-end, ad esempio un database.

#### <a name="periodically-updating-stock-prices"></a>Aggiornamento periodico dei prezzi delle azioni

Il costruttore viene avviato un `Timer` oggetto periodicamente chiamare i metodi che aggiornano quotazioni di titoli in modo casuale.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` le chiamate `UpdateStockPrices`, che passa valore null nel parametro state. Prima di aggiornare i prezzi, l'app acquisisce un blocco per il `_updateStockPricesLock` oggetto. Il codice controlla se un altro thread sta già aggiornando i prezzi e quindi chiama `TryUpdateStockPrice` su ciascun titolo nell'elenco. Il `TryUpdateStockPrice` metodo decide se modifica il prezzo azionario e quella da modificare. Se viene modificato il prezzo delle azioni, l'app chiama `BroadcastStockPrice` per trasmettere la modifica del prezzo azionario a tutti i client connessi.

Il `_updatingStockPrices` flag designata [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) per assicurarsi che sia thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

In un'applicazione reale, la `TryUpdateStockPrice` metodo chiamare un servizio web per cercare il prezzo. In questo codice, l'app Usa un generatore di numeri casuali per apportare modifiche in modo casuale.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Recupero del contesto di SignalR in modo che la classe StockTicker può trasmettere ai client

Perché le variazioni di prezzo derivano qui nel `StockTicker` dell'oggetto, è l'oggetto che deve chiamare un' `updateStockPrice` metodo su tutti i client connessi. In un `Hub` (classe), è disponibile un'API per la chiamata di metodi client, ma `StockTicker` non deriva dalle `Hub` classe e non contiene un riferimento a qualsiasi `Hub` oggetto. Per trasmettere ai client connessi, il `StockTicker` classe deve ottenere l'istanza del contesto SignalR per le `StockTickerHub` classe e usarlo per chiamare metodi sul client.

Il codice ottiene un riferimento al contesto di SignalR quando crea l'istanza della classe singleton, che fanno riferimento a passate al costruttore, e viene inserita nel costruttore di `Clients` proprietà.

Esistono due motivi per cui si desidera ottenere il contesto di una sola volta: recupero del contesto è un'attività costosa e messa in funzione dopo che assicura che l'app mantiene l'ordine previsto dei messaggi inviati ai client.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Introduzione di `Clients` proprietà di contesto e il relativo inserimento `StockTickerClient` proprietà consente di scrivere codice per chiamare i metodi di client che ha lo stesso aspetto come avviene in un `Hub` classe. Ad esempio, per trasmettere a tutti i client, è possibile scrivere `Clients.All.updateStockPrice(stock)`.

Il `updateStockPrice` metodo che si chiamano in `BroadcastStockPrice` non esiste già. Tale servizio verrà aggiunto in un secondo momento quando si scrive codice che viene eseguito sul client. È possibile fare riferimento a `updateStockPrice` qui perché `Clients.All` è dinamico, ovvero l'app verrà valutata l'espressione in fase di esecuzione. Quando viene eseguita questa chiamata al metodo, SignalR invierà il nome del metodo e il valore del parametro al client, e se il client dispone di un metodo denominato `updateStockPrice`, l'app verrà chiamare il metodo e passare il valore del parametro.

`Clients.All` mezzi di trasmissione a tutti i client. SignalR offre altre opzioni per specificare quale client o i gruppi di client da inviare. Per altre informazioni, vedere [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrare la route di SignalR

Il server deve conoscere l'URL da intercettare e indirizzare a SignalR. A tale scopo, aggiungere una classe di avvio OWIN:

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.

1. Nelle **Aggiungi nuovo elemento - SignalR.StockTicker** selezionate **installati** > **Visual C#**   >  **Web** e quindi selezionare **classe di avvio OWIN**.

1. Denominare la classe *avvio* e selezionare **OK**.

1. Sostituire il codice predefinito nel *Startup.cs* file con il seguente codice:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Ora completato la configurazione il codice del server. Nella sezione successiva, è possibile configurare il client.

## <a name="set-up-the-client-code"></a>Impostare il codice client

In questa sezione è impostare il codice eseguito sul client.

### <a name="create-the-html-page-and-javascript-file"></a>Creare la pagina HTML e file JavaScript

La pagina HTML verrà visualizzati i dati e i dati verrà organizzati i file JavaScript.

#### <a name="create-stocktickerhtml"></a>Creare StockTicker.html

In primo luogo, si aggiungerà il client HTML.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **pagina HTML**.

1. Denominare il file *StockTicker* e selezionare **OK**.

1. Sostituire il codice predefinito nel *StockTicker.html* file con il seguente codice:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    Il codice HTML crea una tabella con cinque colonne, una riga di intestazione e una riga di dati con una singola cella che si estende su tutte le cinque colonne. La riga dati Mostra "caricamento in corso" momentaneamente all'avvio dell'app. Il codice JavaScript verrà rimuovere tale riga e aggiungere nelle righe Dell sul posto con dati azionari recuperati dal server.

    Specificano i tag di script:

    * Il file di script jQuery.

    * Il file di script principale di SignalR.

    * Il file di script proxy SignalR.

    * Un file di script StockTicker che verrà creata in un secondo momento.

    L'app genera in modo dinamico il file di script proxy SignalR. Specifica l'URL "hub signalr /" e definisce i metodi di proxy per i metodi sulla classe Hub, in questo caso, per `StockTickerHub.GetAllStocks`. Se si preferisce, è possibile generare manualmente questo file JavaScript usando [SignalR utilità](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). Non dimenticare di disabilitare la creazione di file dinamici nel `MapHubs` chiamata al metodo.

1. Nelle **Esplora soluzioni**, espandere **script**.

    Librerie di script per jQuery e SignalR sono visibili nel progetto.

    > [!IMPORTANT]
    > Package manager installerà una versione successiva degli script di SignalR.

1. Aggiornare i riferimenti di script nel blocco di codice in modo che corrispondano alle versioni dei file di script nel progetto.

1. Nelle **Esplora soluzioni**, fare doppio clic su *StockTicker.html*, quindi selezionare **imposta come pagina iniziale**.

#### <a name="create-stocktickerjs"></a>Creare StockTicker.js

A questo punto creare il file JavaScript.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **JavaScript File**.

1. Denominare il file *StockTicker* e selezionare **OK**.

1. Aggiungere questo codice per il *StockTicker.js* file:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Esaminare il codice client

Se si esamina il codice client, consente di informazioni su come il codice client interagisce con il codice del server per il funzionamento dell'app.

#### <a name="starting-the-connection"></a>Avvio della connessione

`$.connection` fa riferimento al proxy di SignalR. Il codice ottiene un riferimento al proxy per il `StockTickerHub` classe e lo inserisce nella `ticker` variabile. Il nome del proxy è il nome che è stato impostato dal `HubName` attributo:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Dopo aver definito tutte le funzioni e variabili, l'ultima riga di codice nel file Inizializza la connessione SignalR chiamando SignalR `start` (funzione). Il `start` funzione viene eseguita in modo asincrono e restituisce un [jQuery Deferred oggetto](http://api.jquery.com/category/deferred-object/). È possibile chiamare la funzione eseguita per specificare la funzione da chiamare dopo il completamento dell'azione asincrona.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Recupero di tutti i titoli di borsa

Il `init` chiamate di funzione di `getAllStocks` funzione sul server e utilizza le informazioni che il server restituisce per aggiornare la tabella predefinita. Si noti che, per impostazione predefinita, è necessario utilizzare la notazione camel nel client anche se il nome del metodo è la convenzione pascal maiuscole/minuscole nel server. La regola di sistema camelCasing si applica solo ai metodi, non oggetti. Ad esempio, intende `stock.Symbol` e `stock.Price`, non `stock.symbol` o `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

Nel `init` metodo, l'app crea codice HTML per una riga della tabella per ogni oggetto dei titoli ricevuto dal server tramite una chiamata `formatStock` alle proprietà del formato delle `stock` dell'oggetto e quindi chiamando `supplant` per sostituire i segnaposto tra il `rowTemplate` variabile con il `stock` i valori delle proprietà dell'oggetto. Il codice HTML risultante viene quindi aggiunto alla tabella dei titolo.

> [!NOTE]
> Si chiama `init` passandola come un `callback` funzione che verrà eseguito dopo asincrona `start` al termine della funzione. Se è stato chiamato `init` come istruzione JavaScript separata dopo avere chiamato `start`, la funzione avrà esito negativo perché viene eseguita immediatamente senza attendere che la funzione di avvio alla fine di stabilire una connessione. In tal caso, il `init` funzione cercherebbe di chiamare il `getAllStocks` funzionare prima che l'app stabilisce una connessione al server.

#### <a name="getting-updated-stock-prices"></a>Recupero di quotazioni di titoli aggiornati

Quando il server viene modificato il prezzo del titolo, chiama il `updateStockPrice` sui client connessi. L'app aggiunge la funzione per la proprietà client del `stockTicker` proxy per renderlo disponibile per le chiamate dal server.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

Il `updateStockPrice` un oggetto predefinito ricevuto dal server in una tabella di righe esattamente come in formati di funzione di `init` (funzione). Anziché aggiungere la riga nella tabella, trova riga corrente del titolo nella tabella e la sostituisce tale riga con quello nuovo.

## <a name="test-the-application"></a>Testare l'applicazione

È possibile testare l'app per assicurarsi che funzioni. Tutte le finestre del browser verrà visualizzata la tabella di scorte in tempo reale vengono visualizzati con quotazioni di titoli ripristinarne.

1. Nella barra degli strumenti, attivare **debug degli Script** e quindi selezionare il pulsante play per eseguire l'app in modalità di Debug.

    ![Screenshot di utente attivando la modalità di debug e selezionare Riproduci.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Una finestra del browser verrà aperta la visualizzazione di **Live tabella Stock**. La tabella azionario Mostra inizialmente la riga "caricamento in corso", quindi, dopo un breve periodo di tempo, l'app Mostra i primi dati azionari, e iniziare a modificare i prezzi delle azioni.

1. Copiare l'URL dal browser, aprire due altri browser e incollare l'URL nella barra degli indirizzi.

    La visualizzazione predefinita iniziale è quello utilizzato per il primo browser e le modifiche vengono eseguite contemporaneamente.

1. Chiudere tutti i browser, aprire un nuovo browser e passare allo stesso URL.

    L'oggetto singleton StockTicker continuato ad essere eseguiti nel server. Il **Live tabella Stock** mostra che i titoli di borsa hanno continuato a cambiare. Non verranno visualizzati nella tabella con zero inizio modificare cifre.

1. Chiudere il browser.

## <a name="enable-logging"></a>Abilitare la registrazione

SignalR è una funzione di registrazione predefiniti che è possibile abilitare nel client per semplificare la risoluzione dei problemi. In questa sezione abilitare la registrazione e vedere gli esempi che illustrano come log indicano quale dei seguenti metodi di trasporto utilizza SignalR:

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), supportata da IIS 8 e i browser correnti.

* [Gli eventi inviati al server](http://en.wikipedia.org/wiki/Server-sent_events), supportato dai browser diversi da Internet Explorer.

* [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supportata da Internet Explorer.

* [AJAX polling prolungato](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supportata da tutti i browser.

Per una connessione specifica, SignalR sceglie il metodo migliore di trasporto che supportano il client sia nel server.

1. Aprire *StockTicker.js*.

1. Aggiungere la riga evidenziata di codice per abilitare la registrazione immediatamente prima del codice che inizializza la connessione alla fine del file:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Premere **F5** per eseguire il progetto.

1. Finestra Strumenti di sviluppo del browser di aprire e selezionare la Console per visualizzare i log. Si potrebbe essere necessario aggiornare la pagina per visualizzare i log di negoziare il metodo di trasporto per una nuova connessione SignalR.

    * Se si esegue Internet Explorer 10 in Windows 8 (IIS 8), il metodo di trasporto consiste **WebSockets**.

    * Se si esegue Internet Explorer 10 in Windows 7 (IIS 7.5), il metodo di trasporto consiste **iframe**.

    * Se si eseguono 19 Firefox in Windows 8 (IIS 8), il metodo di trasporto consiste **WebSockets**.

        > [!TIP]
        > In Firefox, installare il componente aggiuntivo Firebug per ottenere una finestra della Console.

    * Se si eseguono 19 Firefox in Windows 7 (IIS 7.5), il metodo di trasporto consiste **inviate al server** gli eventi.

## <a name="install-the-stockticker-sample"></a>Installare il campione StockTicker

Il [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installa l'applicazione StockTicker. Il pacchetto NuGet include più funzionalità rispetto alla versione semplificata che è stato creato da zero. In questa sezione dell'esercitazione, si installa il pacchetto NuGet e rivedere le nuove funzionalità e il codice che li implementa.

> [!IMPORTANT]
> Se si installa il pacchetto senza eseguire i passaggi precedenti in questa esercitazione, è necessario aggiungere una classe di avvio OWIN al progetto. Questo file Readme. txt per il pacchetto NuGet illustra questo passaggio.

### <a name="install-the-signalrsample-nuget-package"></a>Installare il pacchetto SignalR.Sample NuGet

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.

1. In **Gestione pacchetti NuGet: SignalR.StockTicker**, selezionare **Sfoglia**.

1. Dal **origine pacchetto**, selezionare **nuget.org**.

1. Immettere *SignalR.Sample* nella casella di ricerca e selezionare **Microsoft.AspNet.SignalR.Sample** > **installare**.

1. Nelle **Esplora soluzioni**, espandere il *SignalR.Sample* cartella.

    Installazione del pacchetto SignalR.Sample creato nella cartella e il relativo contenuto.

1. Nel *SignalR.Sample* cartella, fare doppio clic su *StockTicker.html*, quindi selezionare **imposta come pagina iniziale**.

    > [!NOTE]
    > Installazione di SignalR.Sample NuGet il pacchetto potrebbe cambiare la versione di jQuery presenti nelle *script* cartella. Il nuovo *StockTicker.html* che consente di installare il pacchetto nel file la *SignalR.Sample* cartella sarà sincronizzata con la versione di jQuery che installa il pacchetto, ma se si vuole eseguire originale *StockTicker.html* file anche in questo caso, potrebbe essere necessario aggiornare innanzitutto il riferimento di jQuery nel tag di script.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

 La tabella che si è visto nella prima app aveva funzionalità utili. L'applicazione di teleborsa complete Mostra le nuove funzionalità: una finestra di scorrimento orizzontale che mostra i dati azionari e azioni che modificano i colori che sono in aumento e rientra.

1. Premere **F5** per eseguire l'app.

     Quando si esegue l'app per la prima volta, "mercato" è "chiuso" ed è visibile una tabella statica e una finestra del ticker che non viene effettuato uno scorrimento.

1. Selezionare **Open Market**.

    ![Screenshot della finestra di ticker in tempo reale.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * Il **Live Stock Ticker** finestra Avvia scorra in orizzontale e il server inizia a trasmettere periodicamente le modifiche di prezzo delle azioni in modo casuale.

    * Ogni volta che viene modificato un prezzo azionario, l'app aggiornerà di entrambi i **Live tabella Stock** e il **Live Stock Ticker**.

    * Quando la variazione di prezzo del titolo è positiva, l'app Mostra le azioni con uno sfondo verde.

    * Quando la modifica è negativa, l'app Mostra le azioni con uno sfondo rosso.

1. Selezionare **chiudere mercato**.

    * La tabella viene aggiornata stop.

    * Il ticker Arresta lo scorrimento.

1. Selezionare **reimpostare**.

    * Tutti i dati azionari viene reimpostato.

    * L'app consente di ripristinare lo stato iniziale prima di variazioni di prezzo avviato.

1. Copiare l'URL dal browser, aprire due altri browser e incollare l'URL nella barra degli indirizzi.

1. Si vedranno gli stessi dati aggiornati in modo dinamico al momento stesso in ogni browser.

1. Quando si seleziona uno dei controlli, tutti i browser rispondono esattamente nello stesso momento.

### <a name="live-stock-ticker-display"></a>Visualizzazione Stock Ticker in tempo reale

Il **Live Stock Ticker** visualizzato è un elenco non ordinato in un `<div>` elemento formattato in una singola riga dagli stili CSS. L'app consente di inizializzare e aggiorna il ticker di nello stesso modo della tabella: sostituendo i segnaposto in una `<li>` stringa di modello e in modo dinamico aggiungendo il `<li>` elementi dal `<ul>` elemento. L'app include lo scorrimento tramite il componente aggiuntivo jQuery `animate` funzione per variare il margine a sinistra dell'elenco non ordinato all'interno di `<div>`.

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

Le quotazioni di borsa codice HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

Le quotazioni di borsa codice CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

Scorrere verso il codice jQuery che consente di:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Metodi aggiuntivi nel server in cui il client può chiamare

Per aggiungere la flessibilità per l'app, sono disponibili metodi aggiuntivi che può chiamare l'app.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

Il `StockTickerHub` classe definisce quattro metodi aggiuntivi che il client può chiamare:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

L'app chiama `OpenMarket`, `CloseMarket`, e `Reset` in risposta ai pulsanti nella parte superiore della pagina. Illustrano il motivo di un client attiva un cambiamento di stato immediatamente propagato a tutti i client. Ognuno di questi metodi chiama un metodo `StockTicker` classe che comporta la modifica dello stato di immissione sul mercato e quindi trasmette il nuovo stato.

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

Nel `StockTicker` (classe), l'app mantiene lo stato del mercato con un `MarketState` proprietà che restituisce un `MarketState` valore enum:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Ciascuno dei metodi che modificano lo stato di immissione sul mercato eseguire questa operazione all'interno di un blocco di blocco perché il `StockTicker` classe deve essere thread-safe:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Per assicurarsi che questo codice è thread-safe, il `_marketState` campo sottostante la `MarketState` proprietà designata `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Il `BroadcastMarketStateChange` e `BroadcastMarketReset` metodi sono simili al metodo BroadcastStockPrice che hai già visto, ad eccezione del fatto che chiamano metodi diversi definiti nel client:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funzioni aggiuntive sul client che il server può chiamare

Il `updateStockPrice` funzione ora gestisce la tabella e la visualizzazione del ticker e Usa `jQuery.Color` per un attimo i colori rossi e verdi.

Nuove funzioni nel *SignalR.StockTicker.js* abilitare e disabilitare i pulsanti in base allo stato di immissione sul mercato. Sono anche arrestare o avviare il **Live Stock Ticker** lo scorrimento orizzontale. Poiché molte funzioni vengono aggiunti al `ticker.client`, l'app Usa la [jQuery estendere funzione](http://api.jquery.com/jQuery.extend/) per aggiungerli.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Programma di installazione client aggiuntivi dopo aver stabilito la connessione

Dopo che il client stabilisce la connessione, dispone di alcune operazioni aggiuntive da eseguire:

* Verificare se il mercato è aperto o chiuso per chiamare l'appropriato `marketOpened` o `marketClosed` (funzione).

* Collegare le chiamate al metodo server per i pulsanti.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

I metodi di server non sono collegati i pulsanti fino a dopo che l'app stabilisce la connessione. Si tratta pertanto il codice non è possibile chiamare i metodi di server vengono rese disponibili.

## <a name="additional-resources"></a>Risorse aggiuntive

In questa esercitazione si è appreso come programmare un'applicazione di SignalR che trasmette i messaggi dal server a tutti i client connessi. A questo punto è possibile trasmettere i messaggi su base periodica e in risposta a notifiche da qualsiasi client. È possibile usare il concetto di istanza singleton multithread per mantenere lo stato di server in scenari di giochi online multiplayer. Per un esempio, vedere [gioco ShootR basato su SignalR](https://github.com/NTaylorMullen/ShootR).

Per esercitazioni che illustrano scenari di comunicazione peer-to-peer, vedere [Introduzione a SignalR](introduction-to-signalr.md) e [l'aggiornamento in tempo reale con SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Per altre informazioni su SignalR, vedere le risorse seguenti:

* [ASP.NET SignalR](../../index.md)
* [Progetto di SignalR](http://signalr.net/)
* [SignalR GitHub ed esempi](https://github.com/SignalR/SignalR)
* [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è:

> [!div class="checklist"]
> * Creazione del progetto
> * Impostare il codice server
> * Esaminare il codice del server
> * Impostare il codice client
> * Esaminare il codice client
> * Testare l'applicazione
> * Registrazione abilitata

Passare all'articolo successivo per informazioni su come creare un'applicazione web in tempo reale che usa ASP.NET SignalR 2.
> [!div class="nextstepaction"]
> [Creare app web in tempo reale con SignalR](real-time-web-applications-with-signalr.md)