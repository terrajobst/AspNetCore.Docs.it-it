---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Esercitazione: In tempo reale ad alta frequenza con SignalR 2 | Documenti Microsoft'
author: pfletcher
description: "In questa esercitazione viene illustrato come creare un'applicazione web che utilizza ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza. Ad alta frequenza messaggistica in..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5af7289392c18d58de11249c75e539c9e08954be
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Esercitazione: In tempo reale ad alta frequenza con SignalR 2
====================
da [Patrick Fletcher](https://github.com/pfletcher)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> In questa esercitazione viene illustrato come creare un'applicazione web che utilizza ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza. In questo caso messaggistica ad alta frequenza significa che gli aggiornamenti che vengono inviati a un corrispettivo fisso; nel caso di questa applicazione, fino a 10 messaggi al secondo.
> 
> L'applicazione che si creerà in questa esercitazione consente di visualizzare una forma che gli utenti possono trascinare. La posizione della forma in tutti gli altri browser connesso verrà quindi aggiornata per corrispondere alla posizione della forma trascinata tramite aggiornamenti programmati.
> 
> I concetti introdotti in questa esercitazione sono applicazioni in modalità di gioco in tempo reale e altre applicazioni di simulazione.
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
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Utilizzo di Visual Studio 2012 con questa esercitazione
> 
> 
> Per utilizzare Visual Studio 2012 con questa esercitazione, effettuare le operazioni seguenti:
> 
> - Aggiornamento del [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.
> - Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e 2013.1 strumenti Web per Visual Studio 2012**. Verrà installato come modelli di Visual Studio per le classi di SignalR **Hub**.
> - Alcuni modelli (ad esempio **classe di avvio di OWIN**) non saranno disponibili per tali file, usare un file di classe.
> 
> 
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come creare un'applicazione che condivide lo stato di un oggetto con altri browser in tempo reale. L'applicazione che verrà creata viene chiamato MoveShape. Nella pagina di MoveShape verrà visualizzato un elemento HTML Div che l'utente può trascinare; Quando l'utente trascina un elemento Div, verrà inviata al server, che indicherà quindi tutti gli altri client connessi per aggiornare la posizione della forma in modo che corrisponda alla nuova posizione.

![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

L'applicazione creata in questa esercitazione si basa su una demo di Damian Edwards. Può essere visualizzato un video contenente questa demo [qui](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

L'esercitazione verrà avviato illustrando come inviare messaggi SignalR da ogni evento che viene generato quando viene trascinata la forma. Ogni client connesso verrà quindi aggiorna la posizione della versione locale della forma ogni volta che viene ricevuto un messaggio.

Mentre l'applicazione funzionerà con questo metodo, questo non è un modello di programmazione consigliato, poiché non vi sarà alcun limite al numero di messaggi inviati, in modo che il client e il server può ottenere sovraccaricati con messaggi e potrebbero influire negativamente sulle prestazioni . L'animazione visualizzata sul client sarebbe inoltre indipendente, come la forma verrà spostata immediatamente da ogni metodo, piuttosto che lo spostamento in modo uniforme in ogni nuova posizione. Nelle sezioni successive dell'esercitazione verranno illustrato come creare una funzione di timer che limita la velocità massima con cui i messaggi vengono inviati dal client o server e come spostare la forma in modo uniforme tra i percorsi. La versione finale dell'applicazione creata in questa esercitazione può essere scaricata da [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

In questa esercitazione include le sezioni seguenti:

- [Prerequisiti](#prerequisites)
- [Creare il progetto e aggiungere il pacchetto JQuery.UI NuGet e di SignalR](#createtheproject2013)
- [Creare l'applicazione di base](#baseapp)
- [Avvio dell'hub all'avvio dell'applicazione](#startup2013)
- [Aggiungere il ciclo di client](#clientloop)
- [Aggiungere il ciclo di server](#serverloop)
- [Aggiungere un'animazione smooth sul client](#animation)
- [Passaggi successivi](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione richiede Visual Studio 2013.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Creare il progetto e aggiungere il pacchetto JQuery.UI NuGet e di SignalR

In questa sezione si creerà il progetto in Visual Studio 2013.

La procedura seguente utilizza Visual Studio 2013 per creare un'applicazione Web vuota ASP.NET e aggiungere le librerie di SignalR e jQuery.UI:

1. In Visual Studio, creare un'applicazione Web ASP.NET.

    ![Creare web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. Nel **nuovo progetto ASP.NET** finestra, lasciare **vuoto** selezionata e fare clic su **Crea progetto**.

    ![Creare web vuoto](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. In **Esplora**, fare clic sul progetto, selezionare **Aggiungi | Classe Hub SignalR (v2)**. Nome della classe **MoveShapeHub.cs** e aggiungerlo al progetto. Questo passaggio viene creata la **MoveShapeHub** classe e lo aggiunge al progetto un set di file di script e i riferimenti ad assembly che supportano SignalR.

    > [!NOTE]
    > È anche possibile aggiungere SignalR a un progetto facendo clic su **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguendo un comando:

    `install-package Microsoft.AspNet.SignalR`. 

    Se si utilizza la console per aggiungere SignalR, creare la classe di hub SignalR come passaggio separato dopo l'aggiunta di SignalR.
4. Fare clic su **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**. Nella finestra di gestione di pacchetti, eseguire il comando seguente:

    `Install-Package jQuery.UI.Combined`

    Consente di installare la libreria dell'interfaccia utente di jQuery, che si userà per animare la forma.
5. In **Esplora** espandere il nodo di script. Librerie di script per SignalR, jQuery e jQueryUI sono visibili nel progetto.

    ![Libreria riferimenti a script](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Creare l'applicazione di base

In questa sezione si creerà un'applicazione browser che invia la posizione della forma al server durante ogni evento di spostamento del mouse. Il server trasmette quindi queste informazioni per tutti gli altri client connessi non appena viene ricevuto. Si sarà espandere l'applicazione nelle sezioni successive.

1. Se è stata ancora creata la classe MoveShapeHub.cs in **Esplora**del mouse sul progetto e scegliere **Aggiungi**, **classe...** . Nome della classe **MoveShapeHub** e fare clic su **Aggiungi**.
2. Sostituire il codice nella nuova **MoveShapeHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    La `MoveShapeHub` classe precedente è un'implementazione di un hub di SignalR. Come nel [Introduzione a SignalR](tutorial-getting-started-with-signalr.md) esercitazione, l'hub è un metodo che chiameranno direttamente i client. In questo caso, il client verrà inviato un oggetto che contiene il nuovo coordinate X e Y della forma per il server, che ottiene quindi broadcast a tutti gli altri client connessi. SignalR serializzerà automaticamente questo oggetto tramite JSON.

    L'oggetto che verrà inviato al client (`ShapeModel`) contiene i membri per archiviare la posizione della forma. La versione dell'oggetto sul server contiene anche un membro per tenere traccia i del client, quali dati vengano archiviati, in modo che un determinato client non verranno inviate i propri dati. Questo membro viene utilizzato il `JsonIgnore` attributo per impedirne viene serializzato e inviato al client.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Avvio dell'hub all'avvio dell'applicazione

1. Successivamente, verrà impostata per configurare il mapping all'hub all'avvio dell'applicazione. 2 SignalR, questa operazione viene eseguita mediante l'aggiunta di una classe di avvio OWIN, verrà chiamato `MapSignalR` quando la classe di avvio `Configuration` metodo viene eseguito all'avvio di OWIN. La classe verrà aggiunta all'avvio di OWIN l'elaborazione tramite il `OwinStartup` attributo dell'assembly.

    In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Classe di avvio OWIN**. Nome della classe *avvio* e fare clic su **OK**.
2. Modificare il contenuto di Startup.cs nel modo seguente:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>Aggiunta del client

1. Successivamente, aggiungeremo il client. In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona **pagina Html**. Denominare la pagina **Default.html** e fare clic su **Aggiungi**.
2. In **Esplora**, fare clic sulla pagina appena creato e fare clic su **imposta come pagina iniziale**.
3. Sostituire il codice predefinito nella pagina HTML con il frammento di codice seguente.

    > [!NOTE]
    > Verificare che lo script fa riferimento di sotto di corrispondenza dei pacchetti aggiunti al progetto nella cartella script.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    Il codice HTML e JavaScript precedente crea un elemento Div rosso denominato forma, abilita il comportamento di trascinamento della forma utilizzando la libreria jQuery e utilizza la forma `drag` evento da inviare al server la posizione della forma.
4. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser; Sposta la forma nella finestra del browser.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Aggiungere il ciclo di client

Poiché il percorso della forma a ogni evento di spostamento del mouse di invio creerà una quantità non necessari di traffico di rete, i messaggi dal client necessario limitata. Verrà usato il codice javascript `setInterval` funzione per impostare un ciclo che invia informazioni sulla posizione di nuovo al server a un corrispettivo fisso. Il ciclo è una rappresentazione di base di un "gioco ciclo", una funzione chiamata più volte di tutte le funzionalità di un gioco o altri simulazione.

1. Aggiornare il codice client nella pagina HTML in modo che corrisponda il frammento di codice seguente.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    L'aggiornamento precedente aggiunge il `updateServerModel` (funzione), che viene chiamato su una frequenza fissa. Questa funzione Invia i dati di posizione per il server ogni volta che il `moved` flag indica la presenza di nuovi dati di posizione per l'invio.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser; Sposta la forma nella finestra del browser. Poiché il numero di messaggi che viene inviato al server verrà limitato, l'animazione non verrà visualizzati come smooth utilizzato nella sezione precedente.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Aggiungere il ciclo di server

Nell'applicazione corrente, i messaggi inviati dal server al client passare spesso man mano che vengono ricevuti. Questa operazione presenta un problema analogo come è stata individuata nel client. è possibile inviare messaggi più spesso rispetto a sono necessari e la connessione potrebbe diventare flood di conseguenza. In questa sezione viene descritto come aggiornare il server per implementare un timer che limita la frequenza dei messaggi in uscita.

1. Sostituire il contenuto di `MoveShapeHub.cs` con il frammento di codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Il codice sopra riportato si espande il client per aggiungere il `Broadcaster` (classe), che limita in uscita dei messaggi tramite la `Timer` classe da .NET framework.

    Poiché l'hub stesso sia transitorio (viene creata ogni volta che è necessario), il `Broadcaster` verrà creato come singleton. L'inizializzazione differita, introdotta in .NET 4, è possibile rinviare la creazione finché è richiesta, garantendo che la prima istanza di hub è completamente creata prima dell'avvio del timer.

    La chiamata a client `UpdateShape` funzione viene quindi spostata all'esterno dell'hub `UpdateModel` (metodo), in modo che non viene chiamato immediatamente ogni volta che vengono ricevuti i messaggi in ingresso. Al contrario, i messaggi ai client verranno inviati a una velocità di 25 chiamate al secondo, gestiti dal `_broadcastLoop` timer dall'interno di `Broadcaster` classe.

    Infine, invece di chiamare il metodo client dall'hub direttamente, il `Broadcaster` classe deve ottenere un riferimento all'hub attualmente operativa (`_hubContext`) utilizzando il `GlobalHost`.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser; Sposta la forma nella finestra del browser. Non ci sarà una differenza visibile nel browser dalla sezione precedente, ma il numero di messaggi che viene inviato al client verrà limitato.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Aggiungere un'animazione smooth sul client

L'applicazione è quasi completata, ma è possibile stabilire una ulteriori analisi utilizzo software, il movimento della forma nel client durante lo spostamento in risposta ai messaggi di server. Anziché impostare la posizione della forma nel nuovo percorso, fornito dal server, verrà utilizzata la libreria di JQuery UI `animate` funzione per spostare la forma in modo uniforme tra la posizione corrente e quella nuova.

1. Aggiornare il client `updateShape` metodo per cercare ad esempio il codice evidenziato di seguito:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    Il codice sopra riportato Sposta la forma dalla posizione precedente a quello nuovo assegnato dal server nel corso dell'intervallo di animazione (in questo caso, 100 millisecondi). Qualsiasi precedente animazione in esecuzione in una forma viene cancellata prima dell'avvio della nuova animazione.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser; Sposta la forma nella finestra del browser. Lo spostamento della forma in altra finestra dovrebbe essere visualizzato meno scatti come suo movimento è interpolata nel tempo, piuttosto che viene impostata una sola volta per ogni messaggio in ingresso.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Passaggi successivi

In questa esercitazione è stato spiegato come programmare un'applicazione che invia messaggi ad alta frequenza tra client e server di SignalR. Questo paradigma di comunicazione è utile per lo sviluppo di giochi online e le simulazioni di altra tipo, ad esempio [il gioco ShootR creato con SignalR](http://shootr.signalr.net).

L'applicazione completa creata in questa esercitazione può essere scaricato da [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Per ulteriori informazioni sui concetti di sviluppo SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)

Per una procedura dettagliata su come distribuire un'applicazione di SignalR in Azure, vedere [utilizzando SignalR con le app Web in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Per informazioni dettagliate su come distribuire un progetto web di Visual Studio a un sito Web di Windows Azure, vedere [creare un'app web ASP.NET in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).
