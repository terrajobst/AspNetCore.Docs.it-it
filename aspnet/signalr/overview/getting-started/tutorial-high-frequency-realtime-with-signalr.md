---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Esercitazione: Messaggistica di ad alta frequenza con SignalR 2 | Microsoft Docs'
author: pfletcher
description: Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza. Ad alta frequenza messaggistica in...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8bcc80492804aff2e5a7d3c63004a84447600530
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393248"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Esercitazione: Messaggistica di ad alta frequenza con SignalR 2
====================
da [Patrick Fletcher](https://github.com/pfletcher)

[Download progetto completato](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza. In questo caso la messaggistica ad alta frequenza indica gli aggiornamenti che vengono inviati a una tariffa fissa; nel caso di questa applicazione, fino a 10 messaggi al secondo.
> 
> L'applicazione che si creerà in questa esercitazione consente di visualizzare una forma che gli utenti potranno trascinare. La posizione della forma in tutti gli altri browser connesso verrà quindi aggiornata in modo da corrispondere la posizione della forma trascinata usando gli aggiornamenti programmati.
> 
> I concetti introdotti in questa esercitazione sono le applicazioni in modalità di gioco in tempo reale e altre applicazioni di simulazione.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Uso di Visual Studio 2012 con questa esercitazione
> 
> 
> Per usare Visual Studio 2012 con questa esercitazione, eseguire le operazioni seguenti:
> 
> - Aggiornamento di [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.
> - Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e Web Tools 2013.1 per Visual Studio 2012**. Verrà installato, ad esempio modelli di Visual Studio per le classi di SignalR **Hub**.
> - Alcuni modelli (ad esempio **classe di avvio OWIN**) non è disponibile; per questo motivo, usare invece un file di classe.
> 
> 
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come creare un'applicazione che condivide lo stato di un oggetto con altri browser in tempo reale. L'applicazione che verrà creata è chiamato MoveShape. Nella pagina MoveShape verrà visualizzato un elemento HTML Div che l'utente può trascinare; Quando l'utente trascina il tag Div, verrà inviata al server, che indicherà quindi tutti gli altri client connessi per aggiornare la posizione della forma in modo che corrisponda alla nuova posizione.

![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

L'applicazione creata in questa esercitazione si basa su una demo di Damian Edwards. Può essere visualizzato un video che contiene questa demo [qui](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

L'esercitazione verrà avviata attraverso la dimostrazione di come inviare messaggi SignalR da ogni evento generato quando la forma viene trascinata. Ogni client connesso aggiornerà quindi la posizione della versione locale della forma ogni volta che viene ricevuto un messaggio.

Mentre l'applicazione funzionerà con questo metodo, ciò non è un modello di programmazione consigliato, poiché non vi sarà alcun limite al numero di messaggi inviati, in modo che i client e server è stato possibile ottenere sovraccaricati con messaggi e potrebbero influire negativamente sulle prestazioni . L'animazione visualizzata sul client sarà ugualmente indipendente, poiché la forma viene spostata immediatamente da ogni metodo, piuttosto che lo spostamento in modo uniforme in ogni nuova posizione. Le sezioni successive dell'esercitazione illustra come creare una funzione timer che limita la velocità massima in corrispondenza del quale i messaggi vengono inviati dal client o server e come spostare la forma in modo uniforme tra le posizioni. La versione finale dell'applicazione creata in questa esercitazione può essere scaricata dal [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Questa esercitazione include le sezioni seguenti:

- [Prerequisiti](#prerequisites)
- [Creare il progetto e aggiungere il pacchetto JQuery.UI NuGet e SignalR](#createtheproject2013)
- [Creare l'applicazione di base](#baseapp)
- [Avvio dell'hub all'avvio dell'applicazione](#startup2013)
- [Aggiungere il ciclo di client](#clientloop)
- [Aggiungere il ciclo di server](#serverloop)
- [Aggiungere animazione fluida nel client](#animation)
- [Passaggi successivi](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione richiede Visual Studio 2013.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Creare il progetto e aggiungere il pacchetto JQuery.UI NuGet e SignalR

In questa sezione si creerà il progetto in Visual Studio 2013.

La procedura seguente Usa Visual Studio 2013 per creare un'applicazione Web ASP.NET vuota e aggiungere le librerie di SignalR e jQuery.UI:

1. In Visual Studio creare un'applicazione Web ASP.NET.

    ![Creazione di web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. Nel **nuovo progetto ASP.NET** finestra, lasciare **vuota** selezionata e fare clic su **Crea progetto**.

    ![Creazione di web vuoto](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. Nelle **Esplora soluzioni**, fare clic sul progetto, selezionare **Add | Classe SignalR Hub (v2)**. Denominare la classe **MoveShapeHub.cs** e aggiungerlo al progetto. Questo passaggio Crea il **MoveShapeHub** classe e viene aggiunto al progetto un set di file di script e i riferimenti ad assembly che supportano SignalR.

    > [!NOTE]
    > È anche possibile aggiungere SignalR in un progetto facendo clic **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguendo un comando:

    `install-package Microsoft.AspNet.SignalR`. 

    Se si utilizza la console per aggiungere SignalR, creare la classe hub SignalR come passaggio distinto dopo l'aggiunta di SignalR.
4. Fare clic su **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**. Nella finestra Gestione pacchetti, eseguire il comando seguente:

    `Install-Package jQuery.UI.Combined`

    Consente di installare la libreria dell'interfaccia utente di jQuery, che verrà usato per animare la forma.
5. Nelle **Esplora soluzioni** espandere il nodo di script. Librerie di script per jQuery e jQueryUI SignalR sono visibili nel progetto.

    ![Riferimenti alla libreria di script](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Creare l'applicazione di base

In questa sezione si creerà un'applicazione browser che invia la posizione della forma per il server durante ogni evento di spostamento del mouse. Il server trasmette quindi queste informazioni per tutti gli altri client connessi non appena viene ricevuto. Verranno espansi in questa applicazione nelle sezioni successive.

1. Se è ancora stata creata la classe MoveShapeHub.cs, in **Esplora soluzioni**, fare doppio clic sul progetto e selezionare **Add**, **classe...** . Denominare la classe **MoveShapeHub** e fare clic su **Add**.
2. Sostituire il codice nella nuova **MoveShapeHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    Il `MoveShapeHub` classe precedente è un'implementazione di un hub SignalR. Ad esempio la [Introduzione a SignalR](tutorial-getting-started-with-signalr.md) esercitazione, l'hub è un metodo che chiamano direttamente i client. In questo caso, il client invia un oggetto che contiene il nuovo coordinate X e Y della forma per il server, che quindi ottiene sono trasmesse a tutti gli altri client connessi. SignalR serializzerà automaticamente questo oggetto tramite JSON.

    L'oggetto che verrà inviato al client (`ShapeModel`) contiene i membri per archiviare la posizione della forma. La versione dell'oggetto nel server contiene anche un membro per tenere traccia vengono archiviati i dati del client, quali, in modo che un determinato client non verranno inviate ai propri dati. Questo membro viene utilizzato il `JsonIgnore` attributo da impedire che viene serializzato e inviato al client.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Avvio dell'hub all'avvio dell'applicazione

1. Successivamente, verrà configurato il mapping all'hub all'avvio dell'applicazione. SignalR 2, questa operazione viene eseguita mediante l'aggiunta di una classe di avvio OWIN, che chiamerà `MapSignalR` quando la classe di avvio `Configuration` metodo viene eseguito all'avvio di OWIN. La classe viene aggiunta all'avvio di OWIN elaborazione utilizzando la `OwinStartup` attributo dell'assembly.

    Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Classe di avvio OWIN**. Denominare la classe *avvio* e fare clic su **OK**.
2. Modificare il contenuto di Startup.cs come segue:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>Aggiunta del client

1. Successivamente, si aggiungerà il client. Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona **pagina Html**. Denominare la pagina **default. HTML** e fare clic su **Add**.
2. Nelle **Esplora soluzioni**, fare doppio clic su pagina appena creata e fare clic su **imposta come pagina iniziale**.
3. Sostituire il codice predefinito nella pagina HTML con il frammento di codice seguente.

    > [!NOTE]
    > Verificare che lo script fa riferimento di sotto di corrispondenza i pacchetti aggiunti al progetto nella cartella Scripts.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    Il codice HTML e JavaScript precedente crea un elemento Div rosso chiamato forma, abilita il comportamento di trascinamento della forma utilizzando la libreria jQuery e utilizza la forma `drag` evento da inviare al server la posizione della forma.
4. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. Sposta la forma nella finestra del browser.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Aggiungere il ciclo di client

Poiché la posizione della forma per ogni evento di spostamento del mouse di invio creerà una quantità non necessari di traffico di rete, i messaggi dal client devono essere limitate. Si userà il codice javascript `setInterval` funzione per impostare un ciclo che invia informazioni sulla posizione di nuovo al server a una tariffa fissa. Il ciclo è una rappresentazione di base di un "ciclo del gioco", una funzione chiamata più volte che controlla tutte le funzionalità di un gioco o in altra simulazione.

1. Aggiornare il codice client nella pagina HTML in modo che corrisponda il frammento di codice seguente.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    L'aggiornamento precedente aggiunge il `updateServerModel` (funzione), che viene chiamato in base a una frequenza fissa. Questa funzione Invia i dati di posizione per il server ogni volta che il `moved` flag indica che sono disponibili nuovi dati di posizione per inviare.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. Sposta la forma nella finestra del browser. Poiché verrà limitato il numero di messaggi che vengono inviate al server, l'animazione non verrà visualizzati come smooth come nella sezione precedente.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Aggiungere il ciclo di server

Nell'applicazione corrente, i messaggi inviati dal server al client porto spesso man mano che vengono ricevuti. Ciò costituisce un problema simile come è stata individuata nel client. i messaggi possono essere inviati più spesso sono necessari e la connessione potrebbe diventare diffusi di conseguenza. In questa sezione viene descritto come aggiornare il server per implementare un timer che limita la frequenza dei messaggi in uscita.

1. Sostituire il contenuto di `MoveShapeHub.cs` con il frammento di codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Il codice sopra riportato si espande il client per aggiungere il `Broadcaster` classe, che comporta la limitazione in uscita dei messaggi usando il `Timer` classe da .NET framework.

    Poiché lo stesso hub è transitorio (viene creato ogni volta che è necessario), il `Broadcaster` verrà creato come un singleton. L'inizializzazione differita, introdotta in .NET 4, viene utilizzato per posticiparla fino a quando non è necessario, assicurando che la prima istanza di hub viene creata completamente prima che il timer viene avviato.

    La chiamata a dei client `UpdateShape` funzione viene quindi spostata all'esterno dell'hub `UpdateModel` metodo, in modo che non venga più chiamato immediatamente ogni volta che vengono ricevuti i messaggi in arrivo. Al contrario, verranno inviati a una velocità pari a 25 chiamate al secondo, i messaggi per i client gestiti dal `_broadcastLoop` timer dall'interno la `Broadcaster` classe.

    Infine, invece di chiamare il metodo client dall'hub direttamente, il `Broadcaster` classe deve ottenere un riferimento all'hub operativi correnti (`_hubContext`) utilizzando il `GlobalHost`.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. Sposta la forma nella finestra del browser. Non ci sarà una differenza visibile nel browser dalla sezione precedente, ma verrà limitato il numero di messaggi che vengono inviate al client.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Aggiungere animazione fluida nel client

L'applicazione è quasi completata, ma possiamo rendere uno maggiori miglioramenti, il movimento della forma nel client durante lo spostamento in risposta ai messaggi di server. Anziché impostare la posizione della forma per il nuovo percorso assegnato dal server, si userà la libreria di JQuery UI `animate` funzione per spostare la forma in modo uniforme tra la posizione corrente e nuovo.

1. Aggiornare il client `updateShape` metodo per la ricerca, ad esempio il codice evidenziato seguente:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    Il codice sopra riportato consente di spostare la forma dalla posizione precedente al nuovo assegnato dal server nel corso dell'intervallo di animazione (in questo caso, 100 millisecondi). Qualsiasi animazione precedente esecuzione della forma viene cancellata prima che inizi la nuova animazione.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. Sposta la forma nella finestra del browser. Lo spostamento della forma in altra finestra dovrebbe essere visualizzato meno scatti come suo movimento è interpolata nel tempo, piuttosto che viene impostata una sola volta per ogni messaggio in arrivo.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come programmare un'applicazione di SignalR che invia messaggi ad alta frequenza tra client e server. Questo paradigma di comunicazione è utile per lo sviluppo di giochi online e altre simulazioni, ad esempio [gioco ShootR creato con SignalR](http://shootr.signalr.net).

L'applicazione completa creata in questa esercitazione può essere scaricato dal [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Per altre informazioni sui concetti di sviluppo SignalR, visitare i siti per il codice sorgente SignalR e le risorse seguenti:

- [Progetto di SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)

Per una procedura dettagliata su come distribuire un'applicazione di SignalR in Azure, vedere [uso di SignalR con App Web nel servizio App di Azure](../deployment/using-signalr-with-azure-web-sites.md). Per informazioni dettagliate su come distribuire un progetto web Visual Studio per un sito Web di Microsoft Azure, vedere [creare un'app web ASP.NET in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
