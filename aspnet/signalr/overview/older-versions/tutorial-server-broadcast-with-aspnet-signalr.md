---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Esercitazione: Trasmissione server con ASP.NET SignalR 1.x | Microsoft Docs'
author: bradygaster
description: Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR per fornire funzionalità di trasmissione di server. Trasmissione server significa che communic...
ms.author: bradyg
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 258a55bf72c4b3425d001f478620fa9651952b3f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837468"
---
<a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Esercitazione: Trasmissione server con ASP.NET SignalR 1.x
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR per fornire funzionalità di trasmissione di server. Trasmissione del server significa che le comunicazioni inviate ai client avviate tramite il server. Questo scenario richiede un approccio di programmazione diverso rispetto agli scenari peer-to-peer, ad esempio applicazioni chat, in cui vengono avviate le comunicazioni inviate ai client da una o più dei client.
> 
> L'applicazione che verrà creata in questa esercitazione simula le quotazioni di borsa, uno scenario tipico per la funzionalità di trasmissione di server.
> 
> Sono Benvenuti i commenti l'esercitazione. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Panoramica

Il [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pacchetto NuGet consente di installare un'applicazione di teleborsa esempio simulato in un progetto di Visual Studio. Nella prima parte di questa esercitazione, si creerà una versione semplificata dell'applicazione da zero. Nella parte restante dell'esercitazione, si verrà installare il pacchetto NuGet, rivedere le funzionalità aggiuntive e codice che crea.

L'applicazione di teleborsa è un rappresentante di un tipo di applicazione in tempo reale in cui vuoi periodicamente "push", o la trasmissione, le notifiche dal server per tutti i client connessi.

L'applicazione che verrà compilata nella prima parte di questa esercitazione consente di visualizzare una griglia con dati azionari.

![Versione iniziale StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Periodicamente in modo casuale il server Aggiorna quotazioni di titoli e inserisce gli aggiornamenti a tutti i client connessi. Nel browser i numeri e simboli nel **cambiare** e **%** colonne modificate in modo dinamico in risposta a notifiche dal server. Se si apre browser aggiuntivi allo stesso URL, sono visualizzate le stesse modifiche ai dati e gli stessi dati contemporaneamente.

Questa esercitazione include le sezioni seguenti:

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
> Se non si desidera eseguire i passaggi di compilazione dell'applicazione, è possibile installare il pacchetto SignalR.Sample in una nuova **applicazione Web ASP.NET vuota** dei progetti e leggere con attenzione questi passaggi per ottenere una spiegazione del codice. La prima parte dell'esercitazione vengono illustrati un subset del codice SignalR.Sample e la seconda parte illustra le funzionalità principali di funzionalità aggiuntive nel pacchetto SignalR.Sample.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, assicurarsi che si dispone di Visual Studio 2012 o 2010 SP1 installato nel computer. Se non hai Visual Studio, vedere [download ASP.NET](https://www.asp.net/downloads) per ottenere il gratuito Visual Studio Express 2012 per Web.

Se si dispone di Visual Studio 2010, verificare che [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) è installato.

<a id="createproject"></a>

## <a name="create-the-project"></a>Creare il progetto

1. Dal **File** dal menu **nuovo progetto**.
2. Nel **nuovo progetto** finestra di dialogo espandere **c#** sotto **modelli** e selezionare **Web**.
3. Selezionare il **applicazione Web ASP.NET vuota** al progetto il nome modello *SignalR.StockTicker*, fare clic su **OK**.

    ![Finestra di dialogo Nuovo progetto](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>Aggiungere i pacchetti NuGet di SignalR

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>Aggiungere i pacchetti NuGet di JQuery e SignalR

È possibile aggiungere funzionalità SignalR a un progetto tramite l'installazione di un pacchetto NuGet.

1. Fare clic su **strumenti | Gestione pacchetti NuGet | Console di Gestione pacchetti**.
2. Immettere il comando seguente in Gestione pacchetti.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    Il pacchetto di SignalR installa svariati altri pacchetti NuGet come dipendenze. Al termine dell'installazione sono tutti i componenti server e client necessari per usare SignalR in un'applicazione ASP.NET.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Impostare il codice server

In questa sezione è impostare il codice eseguito nel server.

### <a name="create-the-stock-class"></a>Creare la classe Stock

Iniziare creando la classe modello azionario che si userà per archiviare e trasmettere informazioni su un titolo.

1. Creare un nuovo file di classe nella cartella del progetto, denominarlo *Stock.cs*, quindi sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    Le due proprietà che si imposteranno quando si crea stock sono il simbolo (ad esempio, MSFT per Microsoft) e il prezzo. Le altre proprietà variano a seconda di come e quando si imposta prezzo. La prima volta che è impostare prezzo, il valore ottiene propagato a DayOpen. Tutte le volte successive quando si imposta prezzo, la modifica e i valori delle proprietà valori vengono calcolati in base alla differenza tra prezzo e DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Creare le classi StockTicker e StockTickerHub

Si userà l'API di Hub di SignalR per gestire l'interazione di server a client. Una classe StockTickerHub che deriva dalla classe SignalR Hub gestirà la ricezione di chiamate al metodo e le connessioni dai client. È anche necessario mantenere i dati azionari e l'esecuzione di un oggetto Timer per attivare periodicamente gli aggiornamenti di prezzo, indipendentemente dalle connessioni client. È possibile inserire queste funzioni in una classe di Hub, perché le istanze di Hub sono temporanee. Un'istanza della classe Hub viene creata per ogni operazione nell'hub, ad esempio connessioni e chiamate dal client al server. In modo che il meccanismo che mantiene i dati azionari, aggiorna i prezzi e trasmette gli aggiornamenti di prezzo deve essere eseguito in una classe separata, che è possibile assegnare un nome StockTicker.

![La trasmissione da StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Si vuole solo un'istanza della classe StockTicker in esecuzione nel server, pertanto è necessario configurare un riferimento da ogni istanza StockTickerHub all'istanza singleton di StockTicker. La classe StockTicker deve essere in grado di trasmettere ai client in quanto dispone i dati azionari e attiva gli aggiornamenti, ma StockTicker non è una classe di Hub. Pertanto, la classe StockTicker deve ottenere un riferimento all'oggetto di contesto di connessione del SignalR Hub. Quindi possibile usare l'oggetto di contesto di connessione SignalR per trasmettere ai client.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e fare clic su **Aggiungi nuovo elemento**.
2. Se si dispone di Visual Studio 2012 con il [ASP.NET e Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=279941), fare clic su **Web** sotto **Visual c#** e selezionare il **classe Hub SignalR** modello di elemento. In caso contrario, selezionare la **classe** modello.
3. Denominare la nuova classe *StockTickerHub.cs*, quindi fare clic su **Add**.

    ![Aggiungere StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    Il [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) classe viene utilizzata per definire metodi che i client possono chiamare sul server. Si sta definendo un metodo: `GetAllStocks()`. Quando un client si connette inizialmente al server, chiama questo metodo per ottenere un elenco di tutti i titoli di borsa con i relativi prezzi correnti. Il metodo può eseguire in modo sincrono e restituire `IEnumerable<Stock>` perché restituisce i dati dalla memoria. Se il metodo deve ottenere i dati in questo modo qualcosa che richiederebbe l'intervento di attesa, ad esempio una ricerca nel database o una chiamata al servizio web, si specificherà `Task<IEnumerable<Stock>>` come valore restituito per abilitare l'elaborazione asincrona. Per altre informazioni, vedere [Guida all'API di hub di ASP.NET SignalR - Server - quando è necessario eseguire in modo asincrono](index.md).

    L'attributo HubName specifica come verrà fatto riferimento nell'Hub nel codice JavaScript nel client. Il nome predefinito sul client se non si usa questo attributo è una versione con notazione camel del nome della classe, che in questo caso sarebbe stockTickerHub.

    Come si vedrà in seguito quando si crea la classe StockTicker, viene creata un'istanza singleton della classe nella relativa proprietà istanza statica. Che istanza singleton di StockTicker rimane in memoria, indipendentemente dal numero di client connessione o disconnessione e quell'istanza viene usato il metodo GetAllStocks per restituire informazioni sulle azioni corrente.
5. Creare un nuovo file di classe nella cartella del progetto, denominarlo *StockTicker.cs*, quindi sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Poiché più thread verrà eseguita la stessa istanza del codice StockTicker, la classe StockTicker deve essere affidabile.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Archiviare l'istanza singleton in un campo statico

    Il codice inizializza il metodo statico \_campo di istanza sottostante alla proprietà di istanza con un'istanza della classe e questo è l'unica istanza della classe che può essere creata, perché il costruttore è contrassegnato come privato. [L'inizializzazione differita](https://msdn.microsoft.com/library/dd997286.aspx) viene usato per il \_campo di istanza, non per motivi di prestazioni ma per assicurarsi che la creazione dell'istanza è affidabile.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    Ogni volta che un client si connette al server, una nuova istanza della classe StockTickerHub in esecuzione in un thread separato Ottiene l'istanza singleton StockTicker dalla proprietà statica StockTicker.Instance, come illustrato in precedenza nella classe StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Archiviazione dei dati predefinite in un oggetto ConcurrentDictionary

    Il costruttore inizializza il \_raccolta titoli di borsa con alcuni dati di esempio borsa e GetAllStocks restituisce i titoli di borsa. Come illustrato in precedenza, questa raccolta di titoli azionari, a sua volta viene restituita da StockTickerHub.GetAllStocks che è un metodo server nella classe Hub che i client possono chiamare.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    La raccolta di titoli azionari è definita come un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) tipo per la sicurezza dei thread. In alternativa, è possibile usare una [dizionario](https://msdn.microsoft.com/library/xfhwa508.aspx) dell'oggetto e il dizionario di blocco in modo esplicito quando si apportano modifiche a esso.

    Per questa applicazione di esempio, è bene per archiviare i dati dell'applicazione in memoria e i dati andranno persi quando l'istanza StockTicker venga eliminata. In un'applicazione reale si ridimensiona un archivio dati back-end, ad esempio un database.

    ### <a name="periodically-updating-stock-prices"></a>Aggiornamento periodico dei prezzi delle azioni

    Il costruttore viene avviato un oggetto Timer che chiama periodicamente i metodi che aggiornano quotazioni di titoli in modo casuale.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices viene chiamato dal Timer, che passa valore null nel parametro state. Prima di aggiornare i prezzi indicati, viene applicato un blocco \_updateStockPricesLock oggetto. Il codice controlla se un altro thread sta già aggiornando i prezzi e quindi chiama TryUpdateStockPrice su ciascun titolo nell'elenco. Il metodo TryUpdateStockPrice decide se si desidera modificare il prezzo azionario e a modificarlo. Se viene modificato il prezzo azionario, BroadcastStockPrice viene chiamato per la modifica del prezzo azionario di trasmettere a tutti i client connessi.

    Il \_updatingStockPrices flag è contrassegnato come [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) per assicurarsi che l'accesso a esso sia affidabile.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    In un'applicazione reale, chiamare il metodo TryUpdateStockPrice un servizio web per cercare il prezzo; In questo codice Usa un generatore di numeri casuali per apportare modifiche in modo casuale.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Recupero del contesto di SignalR in modo che la classe StockTicker può trasmettere ai client

    Perché le variazioni di prezzo derivano qui nell'oggetto StockTicker, questo è l'oggetto che deve chiamare un metodo updateStockPrice su tutti i client connessi. In una classe Hub è disponibile un'API per la chiamata di metodi del client, ma StockTicker non deriva dalla classe dell'Hub e non è un riferimento a qualsiasi oggetto di Hub. Pertanto, per poter trasmettere ai client connessi, la classe StockTicker dispone ottenere l'istanza del contesto SignalR per la classe StockTickerHub e usarlo per chiamare metodi sul client.

    Il codice ottiene un riferimento al contesto di SignalR quando crea l'istanza della classe singleton, che fanno riferimento a passate al costruttore, e il costruttore inserisce nella proprietà client.

    Esistono due motivi per cui si desidera ottenere il contesto di una sola volta: recupero del contesto è un'operazione dispendiosa e messa in funzione una sola volta consente di mantenere l'ordine previsto dei messaggi inviati ai client.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    Recupera la proprietà client del contesto e il relativo inserimento nella proprietà StockTickerClient consente di scrivere codice per chiamare i metodi di client che ha lo stesso aspetto come verrebbe mostrata in una classe dell'Hub. Per trasmettere a tutti i client, ad esempio, è possibile scrivere Clients.All.updateStockPrice(stock).

    Il metodo updateStockPrice che si sta chiamando in BroadcastStockPrice non esiste già; tale servizio verrà aggiunto in un secondo momento quando si scrive codice che viene eseguito sul client. È possibile fare riferimento a updateStockPrice qui perché Clients.All è dinamico, ovvero che l'espressione verrà valutata in fase di esecuzione. Quando viene eseguita questa chiamata al metodo, SignalR invierà il nome del metodo e il valore del parametro per il client e se il client dispone di un metodo denominato updateStockPrice, tale metodo verrà chiamato e il valore di parametro verrà passato ad esso.

    Clients.All si intende inviare a tutti i client. SignalR offre altre opzioni per specificare quale client o i gruppi di client da inviare. Per altre informazioni, vedere [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrare la route di SignalR

Il server deve conoscere l'URL da intercettare e indirizzare a SignalR. Per eseguire operazioni che si aggiungerà codice per il *Global. asax* file.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e quindi fare clic su **Aggiungi nuovo elemento**.
2. Selezionare il **classe di applicazione globale** modello di elemento e quindi fare clic su **Add**.

    ![Aggiungere Global. asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. Aggiungere il codice di registrazione delle route SignalR all'applicazione\_Start (metodo):

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    Per impostazione predefinita, l'URL di base per tutto il traffico di SignalR è "/ signalr", e "hub signalr /" viene usato per recuperare un file JavaScript generato dinamicamente che definisce i proxy per tutti gli hub hai nell'applicazione. Il metodo MapHubs include gli overload che consentono di specificare un URL di base diversi e alcune opzioni di SignalR in un'istanza di [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) classe.
4. Aggiungere un tramite informativa nella parte superiore del file:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Salvare e chiudere il *Global. asax* file e compilare il progetto.

Il codice del server di configurazione è stata completata. Nella sezione successiva è possibile configurare il client.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Impostare il codice client

1. Creare un nuovo file HTML nella cartella del progetto e denominarlo *StockTicker.html*.
2. Sostituire il codice del modello con il codice seguente:

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    Il codice HTML crea una tabella con 5 colonne, una riga di intestazione e una riga di dati con una singola cella che si estende su tutte le 5 colonne. La riga di dati viene visualizzato "caricamento in corso" e verrà visualizzata solo momentaneamente all'avvio dell'applicazione. Il codice JavaScript verrà rimuovere tale riga e aggiungere nelle righe Dell sul posto con dati azionari recuperati dal server.

    I tag di script specificano il file di script jQuery, il file di script di SignalR core, il file di script proxy SignalR e un file di script StockTicker che verrà creata in un secondo momento. Il file di script proxy SignalR, che specifica l'URL "hub signalr /", viene generato dinamicamente e definisce i metodi di proxy per i metodi sulla classe Hub, in questo caso per StockTickerHub.GetAllStocks. Se si preferisce, è possibile generare manualmente questo file JavaScript usando [SignalR utilità](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) e disabilitare la creazione di file dinamici nella chiamata al metodo MapHubs.
3. > [!IMPORTANT]
   > Assicurarsi che il file JavaScript fa riferimento *StockTicker.html* siano corretti. Vale a dire, assicurarsi che la versione di jQuery nel tag dello script (1.8.2 nell'esempio) sia la stessa versione di jQuery del progetto *script* cartella e assicurarsi che la versione di SignalR nel tag dello script sia identico SignalR versione del progetto *script* cartella. Se necessario, modificare i nomi dei file nei tag dello script.
4. Nelle **Esplora soluzioni**, fare doppio clic su *StockTicker.html*, quindi fare clic su **imposta come pagina iniziale**.
5. Creare un nuovo file JavaScript nella cartella del progetto e denominarla *StockTicker.js*...
6. Sostituire il codice del modello con il codice seguente:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $.connection fa riferimento al proxy di SignalR. Il codice ottiene un riferimento al proxy per la classe StockTickerHub e lo inserisce nella variabile di ticker. Il nome del proxy è il nome che è stato impostato dall'attributo [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    Dopo aver definite tutte le funzioni e variabili, l'ultima riga di codice nel file Inizializza la connessione SignalR, chiamando la funzione di avvio di SignalR. Funzione di avvio viene eseguita in modo asincrono e restituisce un [jQuery Deferred oggetto](http://api.jquery.com/category/deferred-object/), ovvero è possibile chiamare la funzione eseguita per specificare la funzione da chiamare quando l'operazione asincrona viene completata...

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    La funzione init chiama la funzione getAllStocks sul server e utilizza le informazioni che il server restituisce per aggiornare la tabella predefinita. Si noti che per impostazione predefinita, è necessario usare le iniziali maiuscole e minuscole sul client anche se il nome del metodo è la convenzione pascal maiuscole/minuscole nel server. La regola di maiuscole/minuscole camel si applica solo ai metodi, non oggetti. Ad esempio, è fare riferimento alle scorte. Simbolo e magazzino. Prezzo, non stock.symbol o stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    Se necessaria per l'utilizzo di maiuscole e minuscole pascal sul client o se si vuole usare un nome di metodo completamente diverso, è possibile decorare il metodo Hub con l'attributo HubMethodName allo stesso modo si decorata la classe Hub con l'attributo HubName.

    Nel metodo init, codice HTML per una riga della tabella viene creata per ogni oggetto dei titoli ricevuto dal server dal chiamante formatStock alle proprietà del formato dell'oggetto predefinita e quindi chiamando rimpiazzare (definito in cima *StockTicker.js*) per sostituire i segnaposto nella variabile rowTemplate con i valori delle proprietà oggetto dei titoli. Il codice HTML risultante viene quindi aggiunto alla tabella dei titolo.

    La chiamata di init passandolo come una funzione di callback che viene eseguita dopo la funzione di avvio asincrono viene completata. Se è stato chiamato init come istruzione JavaScript separata dopo la chiamata iniziale, la funzione avrà esito negativo perché viene eseguita immediatamente senza attendere che la funzione di avvio alla fine di stabilire una connessione. In tal caso, la funzione init tentava di chiamare la funzione getAllStocks prima che venga stabilita la connessione al server.

    Quando il server viene modificato il prezzo del titolo, chiama il updateStockPrice sui client connessi. La funzione viene aggiunto alla proprietà client del proxy stockTicker per renderla disponibile alle chiamate dal server.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    La funzione updateStockPrice formatta un oggetto predefinito ricevuto dal server in una riga della tabella come avviene per la funzione init. Tuttavia, anziché aggiungere la riga nella tabella, trova riga corrente del titolo della tabella e sostituisce tale riga con quello nuovo.

<a id="test"></a>

## <a name="test-the-application"></a>Testare l'applicazione

1. Premere F5 per eseguire l'applicazione in modalità di debug.

    Vengono inizialmente visualizzate nella tabella azionario di "caricamento in corso" riga, quindi dopo un breve ritardo che verranno visualizzati i dati azionari iniziali e quindi di avviare le quotazioni di titoli da modificare.

    ![Caricamento](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![Tabella predefinita iniziale](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Tabella predefinita che riceve le modifiche dal server](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Copiare l'URL dalla barra degli indirizzi del browser e incollarlo in una o più finestre del browser nuovo.

    La visualizzazione predefinita iniziale è quello utilizzato per il primo browser e le modifiche vengono eseguite contemporaneamente.
3. Chiudere tutti i browser e aprire un nuovo browser, quindi passare allo stesso URL.

    L'oggetto singleton StockTicker ha continuato a eseguire nel server, per consentirne la visualizzazione di tabella predefinite mostra che i titoli di borsa hanno continuato a cambiare. (Non viene visualizzata la tabella con zero inizio modificare cifre.)
4. Chiudere il browser.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Abilitare la registrazione

SignalR è una funzione di registrazione predefiniti che è possibile abilitare nel client per semplificare la risoluzione dei problemi. In questa sezione abilitare la registrazione e vedere gli esempi che illustrano come log indicano quale dei seguenti metodi di trasporto utilizza SignalR:

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), supportata da IIS 8 e i browser correnti.
- [Gli eventi inviati al server](http://en.wikipedia.org/wiki/Server-sent_events), supportato dai browser diversi da Internet Explorer.
- [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supportata da Internet Explorer.
- [AJAX polling prolungato](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supportata da tutti i browser.

Per una connessione specifica, SignalR sceglie il metodo migliore di trasporto che supportano il client sia nel server.

1. Aprire *StockTicker.js* e aggiungere una riga di codice per abilitare la registrazione immediatamente prima del codice che inizializza la connessione alla fine del file:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Premere F5 per eseguire il progetto.
3. Finestra Strumenti di sviluppo del browser di aprire e selezionare la Console per visualizzare i log. Si potrebbe essere necessario aggiornare la pagina per visualizzare i log di negoziare il metodo di trasporto per una nuova connessione Signalr.

    Se si esegue Internet Explorer 10 in Windows 8 (IIS 8), il metodo di trasporto è WebSocket.

    ![Console di Internet Explorer 10 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    Se si esegue Internet Explorer 10 in Windows 7 (IIS 7.5), il metodo di trasporto è iframe.

    ![IE 10 Console, IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    In Firefox, installare il componente aggiuntivo Firebug per ottenere una finestra della Console. Se si esegue il 19 di Firefox in Windows 8 (IIS 8), il metodo di trasporto è WebSocket.

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    Se si esegue il 19 di Firefox in Windows 7 (IIS 7.5), il metodo di trasporto è il server ha inviato gli eventi.

    ![Firefox 19 Console IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Installare ed esaminare l'esempio StockTicker completo

L'applicazione StockTicker installata mediante il [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pacchetto NuGet include più funzionalità rispetto alla versione semplificata appena creata da zero. In questa sezione dell'esercitazione, si installa il pacchetto NuGet e rivedere le nuove funzionalità e il codice che li implementa.

### <a name="install-the-signalrsample-nuget-package"></a>Installare il pacchetto SignalR.Sample NuGet

1. Nelle **Esplora soluzioni**, fare clic sul progetto e fare clic su **Gestisci pacchetti NuGet**.
2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **Online**, immettere *SignalR.Sample* nel **Cerca Online** casella e quindi fare clic su  **Installare** nella **SignalR.Sample** pacchetto.

    ![Installare il pacchetto SignalR.Sample](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. Nel *Global. asax* del file, commentare la RouteTable.Routes.MapHubs(); della riga di aggiunto in precedenza nell'applicazione\_Start (metodo).

    Il codice nel *Global. asax* non è più necessario perché il pacchetto SignalR.Sample registra la route di SignalR nel *App\_Start/RegisterHubs.cs* file:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    La classe WebActivator che fa riferimento l'attributo dell'assembly è incluso nel pacchetto WebActivatorEx NuGet, che viene installato come una dipendenza del pacchetto SignalR.Sample.
4. Nelle **Esplora soluzioni**, espandere il *SignalR.Sample* cartella in cui è stata creata mediante l'installazione del pacchetto SignalR.Sample.
5. Nel *SignalR.Sample* cartella, fare doppio clic su *StockTicker.html*, quindi fare clic su **imposta come pagina iniziale**.

    > [!NOTE]
    > Installazione di SignalR.Sample NuGet il pacchetto potrebbe cambiare la versione di jQuery presenti nelle *script* cartella. Il nuovo *StockTicker.html* che consente di installare il pacchetto nel file la *SignalR.Sample* cartella sarà sincronizzata con la versione di jQuery che installa il pacchetto, ma se si vuole eseguire originale *StockTicker.html* file anche in questo caso, potrebbe essere necessario aggiornare innanzitutto il riferimento di jQuery nel tag di script.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

1. Premere F5 per eseguire l'applicazione.

    Oltre alla griglia in cui è illustrato in precedenza, l'applicazione di teleborsa completo viene visualizzata una finestra di scorrimento orizzontale che consente di visualizzare gli stessi dati azionari. Quando si esegue l'applicazione per la prima volta, il "mercato" è "chiuso" e visualizzata una griglia statica e una finestra del ticker che non viene effettuato uno scorrimento.

    ![Avvio dello schermo StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    Quando fa clic su **Open Market**, il **Live Stock Ticker** finestra Avvia scorra in orizzontale e il server inizia a trasmettere periodicamente le modifiche di prezzo delle azioni in modo casuale. Ogni volta che un prezzo azionario cambia, entrambi i **Live tabella Stock** griglia e il **Live Stock Ticker** casella vengono aggiornati. Quando la variazione di prezzo del titolo è positiva, le scorte viene visualizzata con uno sfondo verde, e quando la modifica è negativa, le scorte viene visualizzata con uno sfondo rosso.

    ![Aprire sul mercato app StockTicker,](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    Il **Chiudi mercato** pulsante interrompe le modifiche e lo scorrimento di ticker e il **reimpostare** pulsante Reimposta tutti i dati azionari allo stato iniziale prima dell'avvio di variazioni di prezzo. Se si aprono più finestre del browser e passare allo stesso URL, si vedranno gli stessi dati aggiornati in modo dinamico al momento stesso in ogni browser. Quando si fa clic su uno dei pulsanti, tutti i browser rispondono esattamente nello stesso momento.

### <a name="live-stock-ticker-display"></a>Visualizzazione Stock Ticker in tempo reale

Il **Live Stock Ticker** visualizzato è un elenco non ordinato in un elemento div che viene formattato in una singola riga dagli stili CSS. Il ticket viene inizializzata e aggiornata esattamente come la tabella: sostituendo i segnaposto in una &lt;li&gt; stringa di modello e in modo dinamico aggiungendo il &lt;li&gt; elementi per il &lt;ul&gt; elemento. Lo scorrimento viene eseguito usando la funzione di animare jQuery per variare il margine a sinistra dell'elenco non ordinato entro l'elemento DIV.

Le quotazioni di borsa HTML:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

Le quotazioni di borsa CSS:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

Scorrere verso il codice jQuery che consente di:

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Metodi aggiuntivi nel server in cui il client può chiamare

La classe StockTickerHub definisce quattro metodi aggiuntivi che il client può chiamare:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

OpenMarket CloseMarket e reimposta vengono chiamati in risposta ai pulsanti nella parte superiore della pagina. Illustrano il motivo di un client attiva un cambiamento di stato che immediatamente viene propagato a tutti i client. Ognuno di questi metodi chiama un metodo nella classe StockTicker che interessa lo stato di immissione sul mercato, modificare e quindi trasmette il nuovo stato.

Nella classe StockTicker, lo stato del mercato è gestito da una proprietà MarketState che restituisce un valore di enumerazione MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Ciascuno dei metodi che modificano lo stato di immissione sul mercato eseguire questa operazione all'interno di un blocco di blocco perché la classe StockTicker deve essere affidabile:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Per garantire che questo codice è affidabile, il \_marketState campo sottostante alla proprietà MarketState è contrassegnato come volatile,

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

I metodi BroadcastMarketStateChange e BroadcastMarketReset sono simili al metodo BroadcastStockPrice che hai già visto, ad eccezione del fatto che chiamano metodi diversi definiti nel client:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funzioni aggiuntive sul client che il server può chiamare

La funzione updateStockPrice ora gestisce sia la griglia e la visualizzazione di ticker, e Usa jQuery.Color per un attimo i colori rossi e verdi.

Nuove funzioni nel *SignalR.StockTicker.js* abilitare e disabilitare i pulsanti di base sul mercato lo stato e arrestare o avviare il ticker finestra lo scorrimento orizzontale. Poiché vengono aggiunti più funzioni ticker.client, il [jQuery estendere funzione](http://api.jquery.com/jQuery.extend/) consente di aggiungerli.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Programma di installazione client aggiuntivi dopo aver stabilito la connessione

Dopo che il client stabilisce la connessione, dispone di alcune operazioni aggiuntive da eseguire: verificare se il mercato è aperto o chiuso per chiamare il marketOpened appropriata o la funzione marketClosed e collegare le chiamate al metodo server per i pulsanti.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

I metodi server non sono collegati i pulsanti fino a dopo aver stabilita la connessione, in modo che il codice non è possibile provare a chiamare i metodi del server prima che siano disponibili.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come programmare un'applicazione di SignalR che trasmette i messaggi dal server a tutti i client connessi, su base periodica e in risposta a notifiche da qualsiasi client. Il modello di utilizzo di un'istanza singleton multithread per mantenere lo stato del server anche utilizzabile anche in scenari di giochi online multiplayer. Per un esempio, vedere [gioco ShootR basato su SignalR](https://github.com/NTaylorMullen/ShootR).

Per esercitazioni che illustrano scenari di comunicazione peer-to-peer, vedere [Introduzione a SignalR](index.md) e [l'aggiornamento in tempo reale con SignalR](index.md).

Per informazioni sui concetti di sviluppo più avanzati SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [ASP.NET SignalR](https://asp.net/signalr/)
- [Progetto di SignalR](http://signalr.net/)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
