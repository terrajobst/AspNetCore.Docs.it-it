---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Esercitazione: Creare app in tempo reale ad alta frequenza con SignalR 2 | Microsoft Docs'
author: bradygaster
description: Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 44aaa2b0c059de310e963f642fa56c2f00a7e443
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836727"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Esercitazione: Creare app in tempo reale ad alta frequenza con SignalR 2

Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza. In questo caso, "messaggistica ad alta frequenza" significa che il server invia gli aggiornamenti a una tariffa fissa. È inviare fino a 10 messaggi al secondo.

L'applicazione creata consente di visualizzare una forma che gli utenti potranno trascinare. Il server Aggiorna la posizione della forma in tutti i browser connessi per corrispondere alla posizione della forma trascinata usando gli aggiornamenti programmati.

I concetti introdotti in questa esercitazione sono le applicazioni in modalità di gioco in tempo reale e altre applicazioni di simulazione.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il progetto
> * Creare l'applicazione di base
> * Eseguire il mapping all'hub all'avvio di app
> * Aggiungere il client
> * Eseguire l'app
> * Aggiungere il ciclo di client
> * Aggiungere il ciclo di server
> * Aggiungere animazioni uniformi

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il **sviluppo ASP.NET e web** carico di lavoro.

## <a name="set-up-the-project"></a>Configurare il progetto

In questa sezione si crea il progetto in Visual Studio 2017.

Questa sezione illustra come usare Visual Studio 2017 per creare un'applicazione Web ASP.NET vuota e aggiungere le librerie di SignalR e jQuery.UI.

1. In Visual Studio, creare un'applicazione Web ASP.NET.

    ![Creazione di web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. Nel **nuova applicazione Web ASP.NET - MoveShapeDemo** finestra, lasciare **vuota** selezionato e selezionare **OK**.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.

1. Nelle **Aggiungi nuovo elemento - MoveShapeDemo**, selezionare **installati** > **Visual C#**   >  **Web**  >  **SignalR** e quindi selezionare **classe tramite SignalR Hub (v2)**.

1. Denominare la classe *MoveShapeHub* e aggiungerlo al progetto.

    Questo passaggio Crea il *MoveShapeHub.cs* file di classe. Contemporaneamente, viene aggiunto un set di file di script e i riferimenti ad assembly che supportano SignalR al progetto.

1. Selezionare **strumenti di** > **Gestione pacchetti NuGet** > **la Console di Gestione pacchetti**.

1. Nelle **Console di gestione pacchetti**, eseguire questo comando:

    ```console
    Install-Package jQuery.UI.Combined
    ```

    Il comando consente di installare la libreria dell'interfaccia utente di jQuery. Utilizzarla per animare la forma.

1. Nelle **Esplora soluzioni**, espandere il nodo di script.

    ![Riferimenti alla libreria di script](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Librerie di script per jQuery e jQueryUI SignalR sono visibili nel progetto.

## <a name="create-the-base-application"></a>Creare l'applicazione di base

In questa sezione si crea un'applicazione browser. L'app invia la posizione della forma per il server durante ogni evento di spostamento del mouse. Il server trasmette queste informazioni per tutti gli altri client connesso in tempo reale. Altre informazioni sull'applicazione nelle sezioni successive.

1. Aprire il *MoveShapeHub.cs* file.

1. Sostituire il codice nel *MoveShapeHub.cs* file con il seguente codice:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Salvare il file.

Il `MoveShapeHub` classe è un'implementazione di un hub SignalR. Come nel [Introduzione a SignalR](tutorial-getting-started-with-signalr.md) esercitazione, l'hub ha un metodo che i client chiamano direttamente. In questo caso, il client invia un oggetto con il nuovo X e Y coordinate della forma per il server. Queste coordinate ottengano sono trasmesse a tutti gli altri client connessi. SignalR serializza automaticamente questo oggetto tramite JSON.

L'app invia la `ShapeModel` oggetto al client. Dispone di membri per archiviare la posizione della forma. La versione dell'oggetto sul server ha anche un membro per tenere traccia vengono archiviati dati del client. Questo oggetto impedisce il server di inviare i dati del client a se stesso. Questo membro viene utilizzato il `JsonIgnore` attributo per mantenere l'applicazione dalla serializzazione dei dati e lo invia al client.

## <a name="map-to-the-hub-when-app-starts"></a>Eseguire il mapping all'hub all'avvio di app

Successivamente, configurare il mapping all'hub all'avvio dell'applicazione. In SignalR 2, l'aggiunta di una classe di avvio OWIN crea il mapping.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.

1. Nelle **Aggiungi nuovo elemento - MoveShapeDemo** selezionate **installati** > **Visual C#**   >  **Web** e quindi Selezionare **classe di avvio OWIN**.

1. Denominare la classe *avvio* e selezionare **OK**.

1. Sostituire il codice predefinito nel *Startup.cs* file con il seguente codice:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

Le chiamate di classe di avvio OWIN `MapSignalR` quando l'app viene eseguita la `Configuration` (metodo). L'app aggiunge la classe di avvio dell'OWIN elaborazione utilizzando la `OwinStartup` attributo dell'assembly.

## <a name="add-the-client"></a>Aggiungere il client

Aggiungere la pagina HTML per il client.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **pagina HTML**.

1. Denominare la pagina **predefinite** e selezionare **OK**.

1. Nelle **Esplora soluzioni**, fare doppio clic su *default. HTML* e selezionare **imposta come pagina iniziale**.

1. Sostituire il codice predefinito nel *default. HTML* file con il seguente codice:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. Nelle **Esplora soluzioni**, espandere **script**.

    Librerie di script per jQuery e SignalR sono visibili nel progetto.

    > [!IMPORTANT]
    > La gestione dei pacchetti viene installata una versione successiva degli script di SignalR.

1. Aggiornare i riferimenti di script nel blocco di codice in modo che corrispondano alle versioni dei file di script nel progetto.

Questo codice HTML e JavaScript consente di creare una linea rossa `div` chiamato `shape`. Abilita il comportamento di trascinamento della forma utilizzando la libreria jQuery e viene utilizzato il `drag` evento da inviare al server la posizione della forma.

## <a name="run-the-app"></a>Eseguire l'app

È possibile eseguire l'app a se'e funziona. Quando si trascina la forma intorno a una finestra del browser, la forma viene spostato troppo in altri browser.

1. Nella barra degli strumenti, attivare **debug degli Script** e quindi selezionare il pulsante play per eseguire l'applicazione in modalità di Debug.

    ![Screenshot di utente attivando la modalità di debug e selezionare Riproduci.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Consente di aprire una finestra del browser con il cerchio rosso nell'angolo superiore destro.

1. Copiare l'URL della pagina.

1. Aprire un altro browser e incollare l'URL nella barra degli indirizzi.

1. Trascinare la forma in una delle finestre del browser. Segue la forma nella finestra del browser.

Mentre l'applicazione funzioni con questo metodo, non è un modello di programmazione consigliato. Non vi è alcun limite al numero di messaggi inviati. Di conseguenza, i client e server ottenere sovraccaricati con messaggi e comporta una riduzione delle prestazioni. Inoltre, l'app Visualizza un'animazione indipendente sul client. Questa animazione scatti accade perché la forma si sposta immediatamente da ogni metodo. È preferibile se la forma viene spostata in modo uniforme in ogni nuova posizione. Successivamente, descrive come risolvere questi problemi.

## <a name="add-the-client-loop"></a>Aggiungere il ciclo di client

Inviare la posizione della forma per ogni evento di spostamento del mouse, viene creata una quantità di traffico di rete non necessaria. L'app deve limitare i messaggi dal client.

Usare il codice javascript `setInterval` funzione per impostare un ciclo che invia informazioni sulla posizione di nuovo al server a una tariffa fissa. Il ciclo è una rappresentazione di base di un "ciclo del gioco". È una funzione chiamata più volte che controlla tutte le funzionalità di un gioco.

1. Sostituire il codice client di *default. HTML* file con il seguente codice:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > È necessario sostituire i riferimenti a script nuovamente. Le versioni degli script nel progetto devono corrispondere.

    Questo nuovo codice aggiunge il `updateServerModel` (funzione). Essere stato chiamato in base a una frequenza fissa. La funzione Invia i dati di posizione per il server ogni volta che il `moved` flag indica che sono disponibili nuovi dati di posizione per inviare.

1. Selezionare il pulsante di riproduzione per avviare l'applicazione

1. Copiare l'URL della pagina.

1. Aprire un altro browser e incollare l'URL nella barra degli indirizzi.

1. Trascinare la forma in una delle finestre del browser. Segue la forma nella finestra del browser.

Poiché l'app limita il numero di messaggi che vengono inviate al server, l'animazione non sarà più visualizzato come smooth ha inizialmente.

## <a name="add-the-server-loop"></a>Aggiungere il ciclo di server

Nell'applicazione corrente, i messaggi inviati dal server al client porto spesso loro ricezione. Il traffico di rete presenta un problema analogo come illustrato nel client.

L'app può inviare messaggi più spesso sono necessari. Di conseguenza, la connessione può diventare diffusi. In questa sezione viene descritto come aggiornare il server per aggiungere un timer che limita la frequenza dei messaggi in uscita.

1. Sostituire il contenuto di `MoveShapeHub.cs` con questo codice:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Selezionare il pulsante di riproduzione per avviare l'applicazione.

1. Copiare l'URL della pagina.

1. Aprire un altro browser e incollare l'URL nella barra degli indirizzi.

1. Trascinare la forma in una delle finestre del browser.

Questo codice si espande il client per aggiungere il `Broadcaster` classe. La nuova classe limita i messaggi in uscita mediante il `Timer` classe da .NET framework.

È consigliabile sapere che lo stesso hub è transitorio. Viene creato ogni volta che è necessaria. In modo che l'app crea le `Broadcaster` come singleton. Usa l'inizializzazione differita per rinviare la `Broadcaster`della creazione fino a quando non è necessaria. In questo modo si garantisce che l'app viene creata la prima istanza di hub completamente prima di avviare il timer.

La chiamata a dei client `UpdateShape` funzione viene quindi spostata all'esterno dell'hub `UpdateModel` (metodo). Non è più viene chiamato immediatamente ogni volta che l'app riceve i messaggi in arrivo. Al contrario, l'app invia i messaggi per i client con una frequenza pari a 25 chiamate al secondo. Il processo viene gestito dal `_broadcastLoop` timer dall'interno di `Broadcaster` classe.

Infine, invece di chiamare il metodo client dall'hub direttamente, il `Broadcaster` classe deve ottenere un riferimento al sistema attualmente `_hubContext` hub. Ottiene il riferimento con il `GlobalHost`.

## <a name="add-smooth-animation"></a>Aggiungere animazioni uniformi

L'applicazione è quasi terminata, ma possiamo rendere un miglioramento più. L'app consente di spostare la forma sul client in risposta ai messaggi di server. Anziché impostare la posizione della forma per il nuovo percorso assegnato dal server, usare la libreria di JQuery UI `animate` (funzione). È possibile spostare la forma in modo uniforme tra la posizione corrente e nuovo.

1. Aggiornare il client `updateShape` metodo nella *default. HTML* file simile al codice evidenziato di seguito:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Selezionare il pulsante di riproduzione per avviare l'applicazione.

1. Copiare l'URL della pagina.

1. Aprire un altro browser e incollare l'URL nella barra degli indirizzi.

1. Trascinare la forma in una delle finestre del browser.

Lo spostamento della forma in altra finestra meno fluido. L'app crea un'interpolazione suo movimento nel tempo, piuttosto che viene impostata una sola volta per ogni messaggio in arrivo.

Questo codice consente di spostare la forma dalla posizione precedente a quello nuovo. Il server indica la posizione della forma nel corso dell'intervallo di animazione. In questo caso, che è 100 millisecondi. L'app consente di cancellare qualsiasi animazione precedente in esecuzione in una forma prima che inizi la nuova animazione.

## <a name="get-the-code"></a>Ottenere il codice

[Download progetto completato](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a>Risorse aggiuntive

È utile per lo sviluppo di giochi online e altre simulazioni, ad esempio si è appena appreso come il paradigma comune della comunicazione [gioco ShootR creato con SignalR](https://shootr.azurewebsites.net/).

Per altre informazioni su SignalR, vedere le risorse seguenti:

* [Progetto di SignalR](http://signalr.net)

* [SignalR GitHub ed esempi](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il progetto
> * Creazione dell'applicazione di base
> * Il mapping all'hub all'avvio di app
> * Aggiunta del client
> * È stata eseguita l'app
> * Aggiunto il ciclo di client
> * Aggiunta di ciclo del server
> * Aggiunta di animazioni uniformi

Passare all'articolo successivo per informazioni su come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di trasmissione di server.
> [!div class="nextstepaction"]
> [SignalR 2 e la trasmissione di server](tutorial-server-broadcast-with-signalr.md)