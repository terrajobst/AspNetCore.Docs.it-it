---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: In tempo reale ad alta frequenza con SignalR 1. x | Documenti Microsoft
author: pfletcher
description: In questa esercitazione viene illustrato come creare un'applicazione web che utilizza ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza. Ad alta frequenza messaggistica in...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="high-frequency-realtime-with-signalr-1x"></a>In tempo reale ad alta frequenza con SignalR 1. x
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> In questa esercitazione viene illustrato come creare un'applicazione web che utilizza ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza. In questo caso messaggistica ad alta frequenza significa che gli aggiornamenti che vengono inviati a un corrispettivo fisso; nel caso di questa applicazione, fino a 10 messaggi al secondo.
> 
> L'applicazione che si creerà in questa esercitazione consente di visualizzare una forma che gli utenti possono trascinare. La posizione della forma in tutti gli altri browser connesso verrà quindi aggiornata per corrispondere alla posizione della forma trascinata tramite aggiornamenti programmati.
> 
> I concetti introdotti in questa esercitazione sono applicazioni in modalità di gioco in tempo reale e altre applicazioni di simulazione.
> 
> I commenti sull'esercitazione sono iniziale. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come creare un'applicazione che condivide lo stato di un oggetto con altri browser in tempo reale. L'applicazione che verrà creata viene chiamato MoveShape. Nella pagina di MoveShape verrà visualizzato un elemento HTML Div che l'utente può trascinare; Quando l'utente trascina un elemento Div, verrà inviata al server, che indicherà quindi tutti gli altri client connessi per aggiornare la posizione della forma in modo che corrisponda alla nuova posizione.

![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

L'applicazione creata in questa esercitazione si basa su una demo di Damian Edwards. Può essere visualizzato un video contenente questa demo [qui](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

L'esercitazione verrà avviato illustrando come inviare messaggi SignalR da ogni evento che viene generato quando viene trascinata la forma. Ogni client connesso verrà quindi aggiorna la posizione della versione locale della forma ogni volta che viene ricevuto un messaggio.

Mentre l'applicazione funzionerà con questo metodo, questo non è un modello di programmazione consigliato, poiché non vi sarà alcun limite al numero di messaggi inviati, in modo che il client e il server può ottenere sovraccaricati con messaggi e potrebbero influire negativamente sulle prestazioni . L'animazione visualizzata sul client sarebbe inoltre indipendente, come la forma verrà spostata immediatamente da ogni metodo, piuttosto che lo spostamento in modo uniforme in ogni nuova posizione. Nelle sezioni successive dell'esercitazione verranno illustrato come creare una funzione di timer che limita la velocità massima con cui i messaggi vengono inviati dal client o server e come spostare la forma in modo uniforme tra i percorsi. La versione finale dell'applicazione creata in questa esercitazione può essere scaricata da [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

In questa esercitazione include le sezioni seguenti:

- [Prerequisiti](#prerequisites)
- [Creare il progetto](#createtheproject)
- [Aggiungere i pacchetti di ASP.NET SignalR e JQuery.UI NuGet](#nugetpackages)
- [Creare l'applicazione di base](#baseapp)
- [Aggiungere il ciclo di client](#clientloop)
- [Aggiungere il ciclo di server](#serverloop)
- [Aggiungere un'animazione smooth sul client](#animation)
- [Passaggi successivi](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione richiede Visual Studio 2012 o Visual Studio 2010. Se si utilizza Visual Studio 2010, si utilizzerà il progetto .NET Framework 4 anziché di .NET Framework 4.5.

Se si utilizza Visual Studio 2012, è consigliabile installare il [aggiornamento ASP.NET e Web strumenti 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Questo aggiornamento include nuove funzionalità, ad esempio i miglioramenti apportati alle funzionalità di pubblicazione, nuova e nuovi modelli.

Se si dispone di Visual Studio 2010, verificare che [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) è installato.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Creare il progetto

In questa sezione si creerà il progetto in Visual Studio.

1. Dal **File** dal menu **nuovo progetto**.
2. Nel **nuovo progetto** finestra di dialogo espandere **c#** in **modelli** e selezionare **Web**.
3. Selezionare il **applicazione Web ASP.NET vuota** modello, denominare il progetto *MoveShapeDemo*, fare clic su **OK**.

    ![Creazione del nuovo progetto](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Aggiungere il SignalR e i pacchetti JQuery.UI NuGet

È possibile aggiungere funzionalità di SignalR a un progetto tramite l'installazione di un pacchetto NuGet. In questa esercitazione useranno anche il pacchetto JQuery.UI per consentire la forma di trascinamento e aggiungendo un'animazione.

1. Fare clic su **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**.
2. Immettere il comando seguente nel gestore di pacchetto.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Il pacchetto di SignalR viene installato un numero di altri pacchetti NuGet come dipendenze. Al termine dell'installazione è tutti i componenti server e client, è necessari utilizzare SignalR in un'applicazione ASP.NET.
3. Immettere il comando seguente nella console di gestione pacchetti per installare i pacchetti di JQuery e JQuery.UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Creare l'applicazione di base

In questa sezione si creerà un'applicazione browser che invia la posizione della forma al server durante ogni evento di spostamento del mouse. Il server trasmette quindi queste informazioni per tutti gli altri client connessi non appena viene ricevuto. Si sarà espandere l'applicazione nelle sezioni successive.

1. In **Esplora**del mouse sul progetto e scegliere **Aggiungi**, **classe...** . Nome della classe **MoveShapeHub** e fare clic su **Aggiungi**.
2. Sostituire il codice nella nuova **MoveShapeHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    La `MoveShapeHub` classe precedente è un'implementazione di un hub di SignalR. Come nel [Introduzione a SignalR](index.md) esercitazione, l'hub è un metodo che chiameranno direttamente i client. In questo caso, il client verrà inviato un oggetto che contiene il nuovo coordinate X e Y della forma per il server, che ottiene quindi broadcast a tutti gli altri client connessi. SignalR serializzerà automaticamente questo oggetto tramite JSON.

    L'oggetto che verrà inviato al client (`ShapeModel`) contiene i membri per archiviare la posizione della forma. La versione dell'oggetto sul server contiene anche un membro per tenere traccia i del client, quali dati vengano archiviati, in modo che un determinato client non verranno inviate i propri dati. Questo membro viene utilizzato il `JsonIgnore` attributo per impedirne viene serializzato e inviato al client.
3. Successivamente, verrà impostare hub all'avvio dell'applicazione. In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Classe di applicazione globale**. Accettare il nome predefinito di *globale* e fare clic su **OK**.

    ![Aggiungere una classe di applicazione globale](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Aggiungere il seguente `using` istruzione dopo forniti **utilizzando** istruzioni nella classe Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Aggiungere la riga di codice seguente il `Application_Start` metodo della classe globale per registrare la route predefinita per SignalR.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Il file Global. asax dovrebbe essere simile al seguente:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Successivamente, aggiungeremo il client. In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona **pagina Html**. Assegnare alla pagina di un nome appropriato (ad esempio **Default.html**) e fare clic su **Aggiungi**.
7. In **Esplora**, fare clic sulla pagina appena creato e fare clic su **imposta come pagina iniziale**.
8. Sostituire il codice predefinito nella pagina HTML con il frammento di codice seguente.

    > [!NOTE]
    > Verificare che lo script fa riferimento di sotto di corrispondenza dei pacchetti aggiunti al progetto nella cartella script. In Visual Studio 2010, la versione di JQuery e aggiunto al progetto SignalR potrebbe non corrispondere ai numeri di versione riportata di seguito.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Il codice HTML e JavaScript precedente crea un elemento Div rosso denominato forma, abilita il comportamento di trascinamento della forma utilizzando la libreria jQuery e utilizza la forma `drag` evento da inviare al server la posizione della forma.
9. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser; Sposta la forma nella finestra del browser.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Aggiungere il ciclo di client

Poiché il percorso della forma a ogni evento di spostamento del mouse di invio creerà una quantità non necessari di traffico di rete, i messaggi dal client necessario limitata. Verrà usato il codice javascript `setInterval` funzione per impostare un ciclo che invia informazioni sulla posizione di nuovo al server a un corrispettivo fisso. Il ciclo è una rappresentazione di base di un "gioco ciclo", una funzione chiamata più volte di tutte le funzionalità di un gioco o altri simulazione.

1. Aggiornare il codice client nella pagina HTML in modo che corrisponda il frammento di codice seguente.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    L'aggiornamento precedente aggiunge il `updateServerModel` (funzione), che viene chiamato su una frequenza fissa. Questa funzione Invia i dati di posizione per il server ogni volta che il `moved` flag indica la presenza di nuovi dati di posizione per l'invio.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser; Sposta la forma nella finestra del browser. Poiché il numero di messaggi che viene inviato al server verrà limitato, l'animazione non verrà visualizzati come smooth utilizzato nella sezione precedente.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Aggiungere il ciclo di server

Nell'applicazione corrente, i messaggi inviati dal server al client passare spesso man mano che vengono ricevuti. Questa operazione presenta un problema analogo come è stata individuata nel client. è possibile inviare messaggi più spesso rispetto a sono necessari e la connessione potrebbe diventare flood di conseguenza. In questa sezione viene descritto come aggiornare il server per implementare un timer che limita la frequenza dei messaggi in uscita.

1. Sostituire il contenuto di `MoveShapeHub.cs` con il frammento di codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Il codice sopra riportato si espande il client per aggiungere il `Broadcaster` (classe), che limita in uscita dei messaggi tramite la `Timer` classe da .NET framework.

    Poiché l'hub stesso sia transitorio (viene creata ogni volta che è necessario), il `Broadcaster` verrà creato come singleton. L'inizializzazione differita, introdotta in .NET 4, è possibile rinviare la creazione finché è richiesta, garantendo che la prima istanza di hub è completamente creata prima dell'avvio del timer.

    La chiamata a client `UpdateShape` funzione viene quindi spostata all'esterno dell'hub `UpdateModel` (metodo), in modo che non viene chiamato immediatamente ogni volta che vengono ricevuti i messaggi in ingresso. Al contrario, i messaggi ai client verranno inviati a una velocità di 25 chiamate al secondo, gestiti dal `_broadcastLoop` timer dall'interno di `Broadcaster` classe.

    Infine, invece di chiamare il metodo client dall'hub direttamente, il `Broadcaster` classe deve ottenere un riferimento all'hub attualmente operativa (`_hubContext`) utilizzando il `GlobalHost`.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser; Sposta la forma nella finestra del browser. Non ci sarà una differenza visibile nel browser dalla sezione precedente, ma il numero di messaggi che viene inviato al client verrà limitato.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Aggiungere un'animazione smooth sul client

L'applicazione è quasi completata, ma è possibile stabilire una ulteriori analisi utilizzo software, il movimento della forma nel client durante lo spostamento in risposta ai messaggi di server. Anziché impostare la posizione della forma nel nuovo percorso, fornito dal server, verrà utilizzata la libreria di JQuery UI `animate` funzione per spostare la forma in modo uniforme tra la posizione corrente e quella nuova.

1. Aggiornare il client `updateShape` metodo per cercare ad esempio il codice evidenziato di seguito:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Il codice sopra riportato Sposta la forma dalla posizione precedente a quello nuovo assegnato dal server nel corso dell'intervallo di animazione (in questo caso, 100 millisecondi). Qualsiasi precedente animazione in esecuzione in una forma viene cancellata prima dell'avvio della nuova animazione.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser; Sposta la forma nella finestra del browser. Lo spostamento della forma in altra finestra dovrebbe essere visualizzato meno scatti come suo movimento è interpolata nel tempo, piuttosto che viene impostata una sola volta per ogni messaggio in ingresso.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Passaggi successivi

In questa esercitazione è stato spiegato come programmare un'applicazione che invia messaggi ad alta frequenza tra client e server di SignalR. Questo paradigma di comunicazione è utile per lo sviluppo di giochi online e le simulazioni di altra tipo, ad esempio [il gioco ShootR creato con SignalR](http://shootr.signalr.net).

L'applicazione completa creata in questa esercitazione può essere scaricato da [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Per ulteriori informazioni sui concetti di sviluppo SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
