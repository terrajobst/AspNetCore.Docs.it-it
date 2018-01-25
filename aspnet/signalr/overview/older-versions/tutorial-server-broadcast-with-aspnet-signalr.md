---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Esercitazione: Server Broadcast con ASP.NET SignalR 1. x | Documenti Microsoft'
author: pfletcher
description: "In questa esercitazione viene illustrato come creare un'applicazione web che utilizza ASP.NET SignalR per fornire funzionalità di trasmissione di server. Server broadcast significa che communic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/10/2013
ms.topic: article
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3f641b53a9ed568132909114c6cceaa957064fa2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Esercitazione: Server Broadcast con ASP.NET SignalR 1. x
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> In questa esercitazione viene illustrato come creare un'applicazione web che utilizza ASP.NET SignalR per fornire funzionalità di trasmissione di server. Trasmissione server significa che le comunicazioni inviate ai client vengono avviate dal server. Questo scenario richiede un approccio di programmazione diverso rispetto a scenari peer-to-peer, ad esempio le applicazioni di chat, in cui vengono inizializzate le comunicazioni inviate ai client da una o più dei client.
> 
> L'applicazione che si creerà in questa esercitazione consente di simulare le quotazioni di borsa, uno scenario tipico per funzionalità di trasmissione di server.
> 
> I commenti sull'esercitazione sono iniziale. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Panoramica

Il [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pacchetto NuGet consente di installare un'applicazione di teleborsa esempio simulato in un progetto di Visual Studio. Nella prima parte di questa esercitazione, si creerà una versione semplificata dell'applicazione da zero. Nella parte restante dell'esercitazione, consentiranno di installare il pacchetto NuGet di esaminare le funzionalità aggiuntive e il codice che crea.

L'applicazione di teleborsa è un rappresentante di un tipo di applicazione in tempo reale in cui si desidera eseguire periodicamente "push", o trasmissione, le notifiche da server a tutti i client connessi.

L'applicazione che compili nella prima parte di questa esercitazione consente di visualizzare una griglia con dati azionari.

![Versione iniziale di StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Periodicamente in modo casuale il server per aggiornare i prezzi azionari e inserisce gli aggiornamenti a tutti i client connessi. Nel browser, i numeri e simboli di **modificare** e  **%**  colonne modificate in modo dinamico in risposta a notifiche dal server. Se si apre browser aggiuntive per lo stesso URL, sono visualizzate contemporaneamente gli stessi dati e le stesse modifiche ai dati.

In questa esercitazione include le sezioni seguenti:

- [Prerequisiti](#prerequisites)
- [Creare il progetto](#createproject)
- [Aggiungere i pacchetti NuGet di SignalR](#nugetpackages)
- [Impostare il codice server](#server)
- [Impostare il codice client](#client)
- [Testare l'applicazione](#test)
- [Abilitare la registrazione](#enablelogging)
- [Installare ed esaminare l'esempio StockTicker completo](#fullsample)
- [Passaggi successivi](#nextsteps)

> [!NOTE]
> Se non si desidera seguire le istruzioni di compilazione dell'applicazione, è possibile installare il pacchetto SignalR.Sample in un nuovo **applicazione Web ASP.NET vuota** dei progetti e leggere questi passaggi per ottenere una spiegazione del codice. La prima parte dell'esercitazione contiene un sottoinsieme del codice SignalR.Sample e la seconda parte spiega le funzionalità principali di funzionalità aggiuntive nel pacchetto SignalR.Sample.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, verificare che si dispone di Visual Studio 2012 o 2010 SP1 è installato nel computer in uso. Se non si dispone di Visual Studio, vedere [Scarica ASP.NET](https://www.asp.net/downloads) per ottenere il libero Visual Studio Express 2012 per Web.

Se si dispone di Visual Studio 2010, verificare che [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) è installato.

<a id="createproject"></a>

## <a name="create-the-project"></a>Creare il progetto

1. Dal **File** dal menu **nuovo progetto**.
2. Nel **nuovo progetto** finestra di dialogo espandere **c#** in **modelli** e selezionare **Web**.
3. Selezionare il **applicazione Web ASP.NET vuota** modello, denominare il progetto *SignalR.StockTicker*, fare clic su **OK**.

    ![Finestra di dialogo Nuovo progetto](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>Aggiungere i pacchetti NuGet di SignalR

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>Aggiungere il SignalR e i pacchetti JQuery NuGet

È possibile aggiungere funzionalità di SignalR a un progetto tramite l'installazione di un pacchetto NuGet.

1. Fare clic su **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**.
2. Immettere il comando seguente nel gestore di pacchetto.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    Il pacchetto di SignalR viene installato un numero di altri pacchetti NuGet come dipendenze. Al termine dell'installazione è tutti i componenti server e client, è necessari utilizzare SignalR in un'applicazione ASP.NET.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Impostare il codice server

In questa sezione è impostare il codice che viene eseguito nel server.

### <a name="create-the-stock-class"></a>Creare la classe di magazzino

Iniziare creando la classe modello Stock che si userà per archiviare e trasmettere informazioni su un titolo.

1. Creare un nuovo file di classe nella cartella del progetto, denominarlo *Stock.cs*, quindi sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    Le due proprietà che verrà impostata quando si crea le scorte sono il simbolo (ad esempio, MSFT per Microsoft) e il prezzo. Le altre proprietà variano a seconda di come e quando si imposta prezzo. La prima volta, che si imposta prezzo, il valore ottiene propagato a DayOpen. Tutte le volte successive quando è impostata prezzo, la modifica e i valori delle proprietà valori vengono calcolati in base alla differenza tra prezzo e DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Creare le classi StockTicker e StockTickerHub

Utilizzare l'API di Hub SignalR per gestire l'interazione di client-server. Una classe StockTickerHub che deriva dalla classe SignalR Hub gestirà la ricezione di chiamate al metodo e le connessioni dai client. È necessario anche gestire dati azionari ed eseguire un oggetto Timer per avviare periodicamente gli aggiornamenti di prezzo, indipendentemente dalle connessioni client. Impossibile attivare queste funzioni in una classe di Hub, poiché le istanze di Hub sono temporanee. Per ogni operazione nell'hub, ad esempio le connessioni e chiamate dal client al server, viene creata un'istanza della classe Hub. Pertanto, il meccanismo che consente di mantenere dati azionari, aggiorna i prezzi e trasmette gli aggiornamenti di prezzo deve essere eseguito in una classe separata, che è possibile assegnare un nome StockTicker.

![La trasmissione da StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Si desidera solo un'istanza della classe StockTicker in esecuzione sul server, pertanto è necessario impostare un riferimento da ogni istanza StockTickerHub all'istanza singleton StockTicker. La classe StockTicker deve essere in grado di trasmettere ai client perché dispone di dati azionari e attivare gli aggiornamenti, ma StockTicker non è una classe di Hub. Pertanto, la classe StockTicker deve ottenere un riferimento all'oggetto di contesto di connessione SignalR Hub. Quindi possibile utilizzare l'oggetto di contesto di connessione SignalR per trasmettere ai client.

1. In **Esplora**, fare clic sul progetto e fare clic su **Aggiungi nuovo elemento**.
2. Se si dispone di Visual Studio 2012 con il [ASP.NET e aggiornamento di 2012.2 degli strumenti Web](https://go.microsoft.com/fwlink/?LinkId=279941), fare clic su **Web** in **Visual c#** e selezionare il **classe Hub SignalR** modello di elemento. In caso contrario, selezionare il **classe** modello.
3. Denominare la nuova classe *StockTickerHub.cs*, quindi fare clic su **Aggiungi**.

    ![Aggiungere StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    Il [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) classe viene utilizzata per definire i metodi che il client può chiamare sul server. Si definisce un metodo: `GetAllStocks()`. Quando un client si connette inizialmente al server, chiama questo metodo per ottenere un elenco di tutte le scorte con i prezzi corrente. Il metodo è possibile eseguire in modo sincrono e restituire `IEnumerable<Stock>` perché restituisce dati dalla memoria. Se il metodo deve ottenere i dati da qualcosa che prevede l'attesa, ad esempio una ricerca nel database o una chiamata al servizio web, è possibile specificare `Task<IEnumerable<Stock>>` come valore restituito per abilitare l'elaborazione asincrona. Per ulteriori informazioni, vedere [ASP.NET SignalR hub API Guida - Server - quando è necessario eseguire in modo asincrono](index.md).

    L'attributo HubName specifica come viene fatto riferimento nell'Hub nel codice JavaScript nel client. Il nome predefinito sul client se non si usa questo attributo è una versione di maiuscole/minuscole camel del nome della classe, che in questo caso sarebbe stockTickerHub.

    Come si vedrà in un secondo momento quando si crea la classe StockTicker, viene creata un'istanza singleton della classe nella relativa proprietà di istanza statica. Istanza singleton del StockTicker rimane in memoria indipendentemente dal numero di client di connessione o disconnessione e in cui tale istanza viene usato il metodo GetAllStocks per restituire le informazioni correnti.
5. Creare un nuovo file di classe nella cartella del progetto, denominarlo *StockTicker.cs*, quindi sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Poiché verrà eseguita la stessa istanza di codice StockTicker più thread, la classe StockTicker deve essere affidabile.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Archiviare l'istanza singleton in un campo statico

    Il codice inizializza statica \_campo di istanza sottostante alla proprietà di istanza con un'istanza della classe e questa è l'unica istanza della classe che può essere creata, perché il costruttore è contrassegnato come privato. [L'inizializzazione differita](https://msdn.microsoft.com/library/dd997286.aspx) viene utilizzato per il \_campo di istanza, non per motivi di prestazioni, ma per garantire che la creazione dell'istanza è affidabile.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    Ogni volta che un client si connette al server, una nuova istanza della classe StockTickerHub in esecuzione in un thread separato Ottiene l'istanza singleton StockTicker dalla proprietà statica StockTicker.Instance, come illustrato in precedenza nella classe StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>L'archiviazione dei dati predefinite in un oggetto ConcurrentDictionary

    Il costruttore inizializza la \_raccolta scorte con alcuni dati di esempio azionario e GetAllStocks restituisce le scorte. Come illustrato in precedenza, questa raccolta di titoli azionari, a sua volta viene restituita da StockTickerHub.GetAllStocks che è un metodo nella classe Hub che i client possono chiamare server.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    L'insieme di azioni viene definito come un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) tipo per la thread safety. In alternativa, è possibile utilizzare un [dizionario](https://msdn.microsoft.com/library/xfhwa508.aspx) dell'oggetto e il dizionario di blocco in modo esplicito quando si apportano modifiche a esso.

    Per questa applicazione di esempio è OK per archiviare i dati dell'applicazione in memoria e di perdere i dati quando viene eliminata l'istanza di StockTicker. In un'applicazione reale si procederà con un archivio dati back-end, ad esempio un database.

    ### <a name="periodically-updating-stock-prices"></a>Aggiornamento periodico dei prezzi azionari

    Il costruttore, viene avviato un oggetto Timer che chiama periodicamente i metodi di aggiornamento di quotazioni in modo casuale.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices viene chiamato dal Timer, che consente di passare null nel parametro state. Prima di aggiornare i prezzi, viene utilizzato un blocco \_updateStockPricesLock oggetto. Il codice controlla se un altro thread sta già aggiornando i prezzi e quindi chiama TryUpdateStockPrice su ciascuna azione nell'elenco. Il metodo TryUpdateStockPrice decide di modificare il prezzo azionario e la quantità per modificarlo. Se viene modificato il prezzo azionario, BroadcastStockPrice viene chiamato per trasmettere la modifica del prezzo di tutti i client connessi.

    Il \_updatingStockPrices flag è contrassegnato come [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) per garantire che l'accesso a esso è affidabile.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    In un'applicazione reale, il metodo TryUpdateStockPrice chiamare un servizio web per cercare il prezzo; In questo codice Usa un generatore di numeri casuali per apportare modifiche in modo casuale.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Ottenere il contesto di SignalR in modo che la classe StockTicker possibile trasmettere ai client

    Perché le variazioni di prezzo derivano qui nell'oggetto StockTicker, questo è l'oggetto che è necessario chiamare un metodo updateStockPrice su tutti i client connessi. In una classe Hub è disponibile un'API per la chiamata dei metodi client, ma StockTicker non deriva dalla classe dell'Hub e non dispone di un riferimento a qualsiasi oggetto di Hub. Pertanto, per poter trasmettere ai client connessi, la classe StockTicker dispone ottenere l'istanza del contesto SignalR per la classe StockTickerHub e utilizzarlo per chiamare i metodi nei client.

    Il codice ottiene un riferimento al contesto di SignalR quando crea l'istanza di classe singleton, i passaggi che fanno riferimento al costruttore, e il costruttore lo inserisce nella proprietà client.

    Esistono due motivi per cui si desidera ottenere il contesto di una sola volta: ottenere il contesto è un'operazione dispendiosa e recupero di una volta consente di mantenere l'ordine previsto dei messaggi inviati ai client.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    Ottenere la proprietà client del contesto e inserirlo nella proprietà StockTickerClient consente di scrivere codice per chiamare i metodi di client che abbia lo stesso aspetto come avviene in una classe di Hub. Per la trasmissione a tutti i client, ad esempio, è possibile scrivere Clients.All.updateStockPrice(stock).

    Il metodo updateStockPrice che si sta chiamando in BroadcastStockPrice non esiste ancora; si verrà aggiunto in un secondo momento quando si scrive codice che viene eseguito sul client. È possibile fare riferimento qui updateStockPrice perché Clients.All è dinamico, ovvero che l'espressione verrà valutata in fase di esecuzione. Quando viene eseguita la chiamata al metodo, SignalR invierà il nome del metodo e il valore del parametro per il client e, se il client dispone di un metodo denominato updateStockPrice, tale metodo verrà chiamato e verrà passato il valore del parametro.

    Clients.All indica a tutti i client di invio. SignalR offre altre opzioni per specificare quali gruppi di client per inviare ai client. Per ulteriori informazioni, vedere [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrare la route di SignalR

Il server deve sapere quale URL di intercettare e indirizzare a SignalR. Per eseguire si aggiungerà codice di *Global. asax* file.

1. In **Esplora**, fare clic sul progetto e quindi fare clic su **Aggiungi nuovo elemento**.
2. Selezionare il **classe di applicazione globale** modello di elemento e quindi fare clic su **Aggiungi**.

    ![Aggiungere Global. asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. Aggiungere il codice di registrazione delle route SignalR per l'applicazione\_Start (metodo):

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    Per impostazione predefinita, l'URL di base per tutto il traffico SignalR è "/ signalr", e "hub signalr /" viene utilizzato per recuperare un file JavaScript generato in modo dinamico che definisce i proxy per tutti gli hub sono nell'applicazione. Il metodo MapHubs include gli overload che consentono di specificare un URL di base diverso e alcune opzioni di SignalR in un'istanza di [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) classe.
4. Aggiungere un tramite l'istruzione all'inizio del file:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Salvare e chiudere il *Global. asax* file e compilare il progetto.

Il codice del server di configurazione è stata completata. Nella sezione successiva verrà impostato il client.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Impostare il codice client

1. Creare un nuovo file HTML nella cartella del progetto e denominarlo *StockTicker.html*.
2. Sostituire il codice del modello con il codice seguente:

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    Il codice HTML crea una tabella con 5 colonne, una riga di intestazione e una riga di dati con una singola cella che si estende a tutte le colonne di 5. La riga di dati viene visualizzato "caricamento in corso" e verrà visualizzata solo momentaneamente all'avvio dell'applicazione. Il codice JavaScript verrà rimuovere tale riga e nelle righe Dell sul posto con i dati recuperati dal server.

    I tag di script specificano il file di script jQuery, il file di script SignalR core, il file di script proxy SignalR e un file di script StockTicker che verrà creato in un secondo momento. Il file di script proxy SignalR, che specifica l'URL "hub signalr /", viene generato dinamicamente e definisce i metodi del proxy per i metodi della classe di Hub, in questo caso per StockTickerHub.GetAllStocks. Se si preferisce, è possibile generare manualmente questo file JavaScript utilizzando [SignalR utilità](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) e disabilitare la creazione dinamica di file nella chiamata al metodo MapHubs.
3. > [!IMPORTANT]
 > Assicurarsi che il file di JavaScript fa riferimento *StockTicker.html* siano corretti. Vale a dire, assicurarsi che la versione di jQuery nel tag di script (1.8.2 nell'esempio) sia identica a quella del jQuery del progetto *script* cartella e assicurarsi che la versione di SignalR il tag di script è identico per SignalR versione del progetto *script* cartella. Se necessario, modificare i nomi dei file nei tag script.
4. In **Esplora**, fare doppio clic su *StockTicker.html*, quindi fare clic su **imposta come pagina iniziale**.
5. Creare un nuovo file JavaScript nella cartella del progetto e denominarla *StockTicker.js*...
6. Sostituire il codice del modello con il codice seguente:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $.connection fa riferimento al proxy di SignalR. Il codice ottiene un riferimento al proxy per la classe StockTickerHub e lo inserisce nella variabile del titolo. Il nome del proxy è il nome che è stato impostato dall'attributo [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    Dopo aver definite tutte le variabili e funzioni, l'ultima riga di codice nel file Inizializza la connessione SignalR chiamando la funzione di avvio di SignalR. La funzione di avvio viene eseguita in modo asincrono e restituisce un [oggetto posticipata jQuery](http://api.jquery.com/category/deferred-object/), ovvero è possibile chiamare la funzione done per specificare la funzione da chiamare quando l'operazione asincrona viene completata...

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    La funzione init chiama la funzione getAllStocks sul server e utilizza le informazioni che il server restituisce per aggiornare la tabella di magazzino. Si noti che per impostazione predefinita, è necessario utilizzare le maiuscole e minuscole nel client anche se il nome del metodo base alla convezione pascal sul server. La regola di maiuscole/minuscole camel o solo per metodi, non oggetti. Ad esempio, si fa riferimento al magazzino. Simbolo e magazzino. Prezzo, non stock.symbol o stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    Se necessaria per l'utilizzo di maiuscole e minuscole pascal sul client o se si desidera utilizzare un nome di metodo completamente diverso, è possibile decorare il metodo dell'Hub con l'attributo HubMethodName allo stesso modo è decorato la classe Hub con l'attributo HubName.

    Nel metodo init, HTML per una riga della tabella viene creata per ogni oggetto azionario ricevuto dal server dal chiamante formatStock alle proprietà di formato di oggetto predefinito e quindi chiamando rimpiazzare (definito nella parte superiore di *StockTicker.js*) per sostituire i segnaposto nella variabile rowTemplate con valori di proprietà dell'oggetto predefinite. Il codice HTML risultante viene quindi aggiunto alla tabella di magazzino.

    Per chiamare init, passarlo come una funzione di callback che viene eseguita al termine della funzione di avvio asincrono. Se è stato chiamato init come istruzione JavaScript separata dopo la chiamata iniziale, la funzione avrà esito negativo perché viene eseguita immediatamente senza attendere che la funzione di avvio al fine di stabilire la connessione. In tal caso, la funzione init cercherà di chiamare la funzione getAllStocks prima che venga stabilita la connessione al server.

    Quando il server viene modificato un prezzo azionario, chiama il updateStockPrice sul client connessi. La funzione viene aggiunto alla proprietà client del proxy stockTicker per renderla disponibile per le chiamate dal server.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    La funzione updateStockPrice formatta un oggetto predefinito ricevuto dal server in una riga della tabella come avviene per la funzione di inizializzazione. Tuttavia, invece di aggiungere la riga della tabella, trova la riga corrente del titolo della tabella ed sostituisce tale riga con quello nuovo.

<a id="test"></a>

## <a name="test-the-application"></a>Testare l'applicazione

1. Premere F5 per eseguire l'applicazione in modalità di debug.

    Vengono inizialmente visualizzate nella tabella predefinita di "caricamento in corso" riga, quindi dopo un breve intervallo di visualizzazione dei dati azionari iniziali e quindi di avviare i prezzi delle azioni modificare.

    ![Caricamento](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![Tabella predefinita iniziale](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Tabella scorta riceve le modifiche dal server](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Copiare l'URL dalla barra degli indirizzi del browser e incollarlo in uno o più finestre del browser nuovo.

    La visualizzazione predefinita iniziale corrisponde al primo browser e le modifiche vengono eseguite contemporaneamente.
3. Chiudere tutti i browser e aprire un nuovo browser, quindi passare allo stesso URL.

    L'oggetto singleton StockTicker continua in esecuzione nel server, pertanto la visualizzazione della tabella predefinite mostra che è sono costantemente le scorte di modificare. (Si non vedere la tabella con zero iniziale modificare cifre).
4. Chiudere il browser.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Abilitare la registrazione

SignalR è una funzione di registrazione predefiniti che è possibile abilitare sul client per facilitare la risoluzione dei problemi. In questa sezione abilitare la registrazione e vedere esempi che illustrano come registri può sapere quale dei seguenti metodi di trasporto SignalR utilizza:

- [WebSocket](http://en.wikipedia.org/wiki/WebSocket), supportata da IIS 8 e i browser correnti.
- [Gli eventi inviati al server](http://en.wikipedia.org/wiki/Server-sent_events), supportata dal browser diverso da Internet Explorer.
- [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supportato da Internet Explorer.
- [Polling prolungato AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)supportati da tutti i browser.

Per ogni connessione specifica, SignalR sceglie il metodo migliore di trasporto che supportano sia il server e client.

1. Aprire *StockTicker.js* e aggiungere una riga di codice per abilitare la registrazione immediatamente prima del codice che inizializza la connessione alla fine del file:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Premere F5 per eseguire il progetto.
3. Aprire una finestra di strumenti per gli sviluppatori del browser e selezionare la Console per visualizzare i log. Potrebbe essere necessario aggiornare la pagina per visualizzare i registri di negoziare il metodo di trasporto per una nuova connessione Signalr.

    Se si utilizza Internet Explorer 10 in Windows 8 (IIS 8), il metodo di trasporto è WebSocket.

    ![Console di Internet Explorer 10 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    Se si utilizza Internet Explorer 10 in Windows 7 (IIS 7.5), il metodo di trasporto è iframe.

    ![Internet Explorer 10 Console, IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    In Firefox, installare il componente aggiuntivo Firebug per ottenere una finestra della Console. Se si utilizza Firefox 19 in Windows 8 (IIS 8), il metodo di trasporto è WebSocket.

    ![Firefox 19 IIS 8 WebSocket](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    Se si utilizza Firefox 19 in Windows 7 (IIS 7.5), il metodo di trasporto è eventi inviati al server.

    ![Console di IIS 7.5 Firefox 19](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Installare ed esaminare l'esempio StockTicker completo

L'applicazione StockTicker che viene installato per il [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pacchetto NuGet include caratteristiche aggiuntive rispetto alla versione semplificata appena creata da zero. In questa sezione dell'esercitazione, installare il pacchetto NuGet e rivedere le nuove funzionalità e il codice che li implementa.

### <a name="install-the-signalrsample-nuget-package"></a>Installare il pacchetto SignalR.Sample NuGet

1. In **Esplora**, fare clic sul progetto e fare clic su **Gestisci pacchetti NuGet**.
2. Nel **Gestisci pacchetti NuGet** la finestra di dialogo, fare clic su **Online**, immettere *SignalR.Sample* nel **ricerca Online** casella e quindi fare clic su  **Installare** nel **SignalR.Sample** pacchetto.

    ![Installare il pacchetto SignalR.Sample](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. Nel *Global. asax* del file, impostare come commento il RouteTable.Routes.MapHubs(); riga a cui sono stati aggiunti in precedenza nell'applicazione\_Start (metodo).

    Il codice in *Global. asax* non è più necessario perché il pacchetto SignalR.Sample registra la route SignalR nel *App\_Start/RegisterHubs.cs* file:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    La classe WebActivator che fa riferimento l'attributo di assembly è incluso nel pacchetto WebActivatorEx NuGet, che viene installato come una dipendenza del pacchetto SignalR.Sample.
4. In **Esplora**, espandere il *SignalR.Sample* cartella che è stato creato tramite l'installazione del pacchetto SignalR.Sample.
5. Nel *SignalR.Sample* cartella, fare doppio clic su *StockTicker.html*, quindi fare clic su **imposta come pagina iniziale**.

    > [!NOTE]
    > Installazione di SignalR.Sample NuGet il pacchetto potrebbe cambiare la versione di jQuery presenti nel *script* cartella. Il nuovo *StockTicker.html* file per installare il pacchetto di *SignalR.Sample* cartella sarà sincronizzata con la versione di jQuery che installa il pacchetto, ma se si desidera eseguire originale *StockTicker.html* file nuovamente, potrebbe essere necessario aggiornare innanzitutto il riferimento di jQuery nel tag di script.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

1. Premere F5 per eseguire l'applicazione.

    Oltre alla griglia in cui è illustrato in precedenza, l'applicazione di teleborsa completo viene visualizzata una finestra di scorrimento orizzontale che consente di visualizzare gli stessi dati azionari. Quando si esegue l'applicazione per la prima volta, "market" è "chiuso" e si noterà una griglia statica e una finestra di quotazioni di borsa che non lo scorrimento.

    ![Inizio schermata StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    Quando fa clic su **Open Market**, **Live borsa** avvia casella di scorrimento orizzontale e il server inizia a trasmettere periodicamente le modifiche di prezzo delle azioni in modo casuale. Ogni volta che un prezzo azionario cambia, sia il **Live tabella Stock** griglia e **Live borsa** casella vengono aggiornati. Quando variazione di prezzo di un titolo è positivo, il codice viene visualizzato con uno sfondo verde, e quando la modifica è negativa, il codice viene visualizzato con uno sfondo rosso.

    ![Aprire mercato StockTicker app,](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    Il **mercato Chiudi** pulsante interrompe le modifiche e lo scorrimento di ticker e **reimpostare** pulsante Reimposta tutti i dati azionari allo stato iniziale prima dell'avvio di variazioni di prezzo. Se si aprono più finestre del browser e passare allo stesso URL, verranno visualizzati i dati stessi aggiornati dinamicamente contemporaneamente in ogni browser. Quando si fa clic su uno dei pulsanti, tutti i browser rispondono esattamente nello stesso momento.

### <a name="live-stock-ticker-display"></a>Visualizzazione di borsa in tempo reale

Il **Live borsa** visualizzato è un elenco non ordinato in un elemento div che verrà formattato in una singola riga mediante gli stili CSS. Al simbolo non è inizializzato e aggiornato nello stesso modo della tabella: sostituendo i segnaposto in una &lt;li&gt; stringa di modello e in modo dinamico aggiungendo il &lt;li&gt; elementi per il &lt;ul&gt; elemento. Le barre di scorrimento viene eseguita tramite la funzione animare jQuery per variare il margine sinistro dell'elenco non ordinato all'interno di tag div.

Il titolo HTML:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

Il titolo CSS:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

Scorrere il codice jQuery che consente di:

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Metodi aggiuntivi nel server in cui il client può chiamare

La classe StockTickerHub definisce quattro metodi aggiuntivi che il client può chiamare:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

OpenMarket CloseMarket e la reimpostazione vengono chiamati in risposta ai pulsanti nella parte superiore della pagina. Illustrano il modello di attivazione di una modifica nello stato in cui verrà immediatamente propagata a tutti i client di un client. Ognuno di questi metodi chiama un metodo nella classe StockTicker che influisce sullo stato di mercato, modificare e quindi trasmette il nuovo stato.

Nella classe StockTicker, lo stato del mercato è gestito da una proprietà MarketState che restituisce un valore di enumerazione MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Ciascuno dei metodi che modificano lo stato di mercato farlo all'interno di un blocco di blocco perché la classe StockTicker deve essere affidabile:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Per garantire che il codice è affidabile, il \_marketState campo sottostante alla proprietà MarketState è contrassegnato come volatile,

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

I metodi BroadcastMarketStateChange e BroadcastMarketReset sono simili al metodo BroadcastStockPrice che già verificato, ad eccezione del fatto che chiamano metodi definiti nel client:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funzioni aggiuntive sul client che il server può chiamare

La funzione updateStockPrice ora gestisce sia la griglia e la visualizzazione del titolo, e utilizza jQuery.Color per flash colori rosso e verde.

Nuove funzioni in *SignalR.StockTicker.js* abilitare e disabilitare i pulsanti in base a stato di mercato e sono arrestare o avviare il titolo finestra lo scorrimento orizzontale. Poiché ticker.client, vengono aggiunti più funzioni di [jQuery estendere funzione](http://api.jquery.com/jQuery.extend/) viene utilizzato per aggiungerli.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Programma di installazione client aggiuntive dopo aver stabilito la connessione

Dopo che il client stabilisce la connessione, dispone di alcune operazioni aggiuntive da eseguire: verificare se il mercato è aperto o chiuso per chiamare la marketOpened appropriato o la funzione marketClosed e collegare le chiamate al metodo server ai pulsanti.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

I metodi di server non sono collegati ai pulsanti fino a dopo aver stabilita la connessione, in modo che il codice non è possibile provare a chiamare i metodi di server prima che siano disponibili.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato spiegato come programmare un'applicazione di SignalR che trasmette i messaggi dal server a tutti i client connessi con cadenza periodica e in risposta a notifiche da qualsiasi client. Il modello di utilizzo di un'istanza singleton a thread multipli per mantenere lo stato del server anche utilizzabile anche negli scenari di giochi online a più giocatori. Per un esempio, vedere [il gioco ShootR basato su SignalR](https://github.com/NTaylorMullen/ShootR).

Per le esercitazioni che illustrano scenari di comunicazione peer-to-peer, vedere [Introduzione a SignalR](index.md) e [l'aggiornamento in tempo reale con SignalR](index.md).

Per informazioni su concetti più avanzati di sviluppo SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [ASP.NET SignalR](https://asp.net/signalr/)
- [Progetto SignalR](http://signalr.net/)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
