---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Messaggistica ad alta frequenza con SignalR 1.x | Microsoft Docs
author: pfletcher
description: Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza. Ad alta frequenza messaggistica in...
ms.author: riande
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: c677bcbc78eac6056c035c2b34fe659caac9c6fa
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912280"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>Messaggistica ad alta frequenza con SignalR 1.x
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza. In questo caso la messaggistica ad alta frequenza indica gli aggiornamenti che vengono inviati a una tariffa fissa; nel caso di questa applicazione, fino a 10 messaggi al secondo.
> 
> L'applicazione che si creerà in questa esercitazione consente di visualizzare una forma che gli utenti potranno trascinare. La posizione della forma in tutti gli altri browser connesso verrà quindi aggiornata in modo da corrispondere la posizione della forma trascinata usando gli aggiornamenti programmati.
> 
> I concetti introdotti in questa esercitazione sono le applicazioni in modalità di gioco in tempo reale e altre applicazioni di simulazione.
> 
> Sono Benvenuti i commenti l'esercitazione. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come creare un'applicazione che condivide lo stato di un oggetto con altri browser in tempo reale. L'applicazione che verrà creata è chiamato MoveShape. Nella pagina MoveShape verrà visualizzato un elemento HTML Div che l'utente può trascinare; Quando l'utente trascina il tag Div, verrà inviata al server, che indicherà quindi tutti gli altri client connessi per aggiornare la posizione della forma in modo che corrisponda alla nuova posizione.

![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

L'applicazione creata in questa esercitazione si basa su una demo di Damian Edwards. Può essere visualizzato un video che contiene questa demo [qui](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

L'esercitazione verrà avviata attraverso la dimostrazione di come inviare messaggi SignalR da ogni evento generato quando la forma viene trascinata. Ogni client connesso aggiornerà quindi la posizione della versione locale della forma ogni volta che viene ricevuto un messaggio.

Mentre l'applicazione funzionerà con questo metodo, ciò non è un modello di programmazione consigliato, poiché non vi sarà alcun limite al numero di messaggi inviati, in modo che i client e server è stato possibile ottenere sovraccaricati con messaggi e potrebbero influire negativamente sulle prestazioni . L'animazione visualizzata sul client sarà ugualmente indipendente, poiché la forma viene spostata immediatamente da ogni metodo, piuttosto che lo spostamento in modo uniforme in ogni nuova posizione. Le sezioni successive dell'esercitazione illustra come creare una funzione timer che limita la velocità massima in corrispondenza del quale i messaggi vengono inviati dal client o server e come spostare la forma in modo uniforme tra le posizioni. La versione finale dell'applicazione creata in questa esercitazione può essere scaricata dal [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Questa esercitazione include le sezioni seguenti:

- [Prerequisiti](#prerequisites)
- [Creare il progetto](#createtheproject)
- [Aggiungere i pacchetti JQuery.UI NuGet e ASP.NET SignalR](#nugetpackages)
- [Creare l'applicazione di base](#baseapp)
- [Aggiungere il ciclo di client](#clientloop)
- [Aggiungere il ciclo di server](#serverloop)
- [Aggiungere animazione fluida nel client](#animation)
- [Passaggi successivi](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione richiede Visual Studio 2012 o Visual Studio 2010. Se viene usato Visual Studio 2010, si userà il progetto .NET Framework 4, invece di .NET Framework 4.5.

Se si usa Visual Studio 2012, è consigliabile installare il [aggiornamento ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Questo aggiornamento include nuove funzionalità, ad esempio miglioramenti delle funzionalità di pubblicazione, una novità e nuovi modelli.

Se si dispone di Visual Studio 2010, verificare che [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) è installato.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Creare il progetto

In questa sezione si creerà il progetto in Visual Studio.

1. Dal **File** dal menu **nuovo progetto**.
2. Nel **nuovo progetto** finestra di dialogo espandere **c#** sotto **modelli** e selezionare **Web**.
3. Selezionare il **applicazione Web ASP.NET vuota** al progetto il nome modello *MoveShapeDemo*, fare clic su **OK**.

    ![Creazione del nuovo progetto](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Aggiungere i pacchetti NuGet di JQuery.UI e SignalR

È possibile aggiungere funzionalità SignalR a un progetto tramite l'installazione di un pacchetto NuGet. Questa esercitazione verrà utilizzato anche il pacchetto JQuery.UI per consentire la forma da trascinare e aggiungendo un'animazione.

1. Fare clic su **strumenti | Gestione pacchetti NuGet | Console di Gestione pacchetti**.
2. Immettere il comando seguente in Gestione pacchetti.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Il pacchetto di SignalR installa svariati altri pacchetti NuGet come dipendenze. Al termine dell'installazione sono tutti i componenti server e client necessari per usare SignalR in un'applicazione ASP.NET.
3. Immettere il comando seguente nella console di gestione pacchetti per installare i pacchetti di JQuery e JQuery.UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Creare l'applicazione di base

In questa sezione si creerà un'applicazione browser che invia la posizione della forma per il server durante ogni evento di spostamento del mouse. Il server trasmette quindi queste informazioni per tutti gli altri client connessi non appena viene ricevuto. Verranno espansi in questa applicazione nelle sezioni successive.

1. Nelle **Esplora soluzioni**, fare doppio clic sul progetto e selezionare **Add**, **classe...** . Denominare la classe **MoveShapeHub** e fare clic su **Add**.
2. Sostituire il codice nella nuova **MoveShapeHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    Il `MoveShapeHub` classe precedente è un'implementazione di un hub SignalR. Ad esempio la [Introduzione a SignalR](index.md) esercitazione, l'hub è un metodo che chiamano direttamente i client. In questo caso, il client invia un oggetto che contiene il nuovo coordinate X e Y della forma per il server, che quindi ottiene sono trasmesse a tutti gli altri client connessi. SignalR serializzerà automaticamente questo oggetto tramite JSON.

    L'oggetto che verrà inviato al client (`ShapeModel`) contiene i membri per archiviare la posizione della forma. La versione dell'oggetto nel server contiene anche un membro per tenere traccia vengono archiviati i dati del client, quali, in modo che un determinato client non verranno inviate ai propri dati. Questo membro viene utilizzato il `JsonIgnore` attributo da impedire che viene serializzato e inviato al client.
3. Successivamente, verrà configurato l'hub all'avvio dell'applicazione. Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Classe di applicazione globale**. Accettare il nome predefinito della *Global* e fare clic su **OK**.

    ![Aggiungi classe di applicazione globale](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Aggiungere il codice seguente `using` istruzione dopo che l'oggetto fornito **usando** le istruzioni nella classe Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Aggiungere la riga di codice seguente il `Application_Start` metodo della classe globale per registrare la route predefinita per SignalR.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Il file Global. asax dovrebbe essere simile al seguente:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Successivamente, si aggiungerà il client. Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona **pagina Html**. Assegnare un nome appropriato della pagina (analogamente **default. HTML**) e fare clic su **Add**.
7. Nelle **Esplora soluzioni**, fare doppio clic su pagina appena creata e fare clic su **imposta come pagina iniziale**.
8. Sostituire il codice predefinito nella pagina HTML con il frammento di codice seguente.

    > [!NOTE]
    > Verificare che lo script fa riferimento di sotto di corrispondenza i pacchetti aggiunti al progetto nella cartella Scripts. In Visual Studio 2010, la versione di JQuery e SignalR aggiunto al progetto potrebbe non corrispondere ai numeri di versione riportato di seguito.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Il codice HTML e JavaScript precedente crea un elemento Div rosso chiamato forma, abilita il comportamento di trascinamento della forma utilizzando la libreria jQuery e utilizza la forma `drag` evento da inviare al server la posizione della forma.
9. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. Sposta la forma nella finestra del browser.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Aggiungere il ciclo di client

Poiché la posizione della forma per ogni evento di spostamento del mouse di invio creerà una quantità non necessari di traffico di rete, i messaggi dal client devono essere limitate. Si userà il codice javascript `setInterval` funzione per impostare un ciclo che invia informazioni sulla posizione di nuovo al server a una tariffa fissa. Il ciclo è una rappresentazione di base di un "ciclo del gioco", una funzione chiamata più volte che controlla tutte le funzionalità di un gioco o in altra simulazione.

1. Aggiornare il codice client nella pagina HTML in modo che corrisponda il frammento di codice seguente.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    L'aggiornamento precedente aggiunge il `updateServerModel` (funzione), che viene chiamato in base a una frequenza fissa. Questa funzione Invia i dati di posizione per il server ogni volta che il `moved` flag indica che sono disponibili nuovi dati di posizione per inviare.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. Sposta la forma nella finestra del browser. Poiché verrà limitato il numero di messaggi che vengono inviate al server, l'animazione non verrà visualizzati come smooth come nella sezione precedente.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Aggiungere il ciclo di server

Nell'applicazione corrente, i messaggi inviati dal server al client porto spesso man mano che vengono ricevuti. Ciò costituisce un problema simile come è stata individuata nel client. i messaggi possono essere inviati più spesso sono necessari e la connessione potrebbe diventare diffusi di conseguenza. In questa sezione viene descritto come aggiornare il server per implementare un timer che limita la frequenza dei messaggi in uscita.

1. Sostituire il contenuto di `MoveShapeHub.cs` con il frammento di codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Il codice sopra riportato si espande il client per aggiungere il `Broadcaster` classe, che comporta la limitazione in uscita dei messaggi usando il `Timer` classe da .NET framework.

    Poiché lo stesso hub è transitorio (viene creato ogni volta che è necessario), il `Broadcaster` verrà creato come un singleton. L'inizializzazione differita, introdotta in .NET 4, viene utilizzato per posticiparla fino a quando non è necessario, assicurando che la prima istanza di hub viene creata completamente prima che il timer viene avviato.

    La chiamata a dei client `UpdateShape` funzione viene quindi spostata all'esterno dell'hub `UpdateModel` metodo, in modo che non venga più chiamato immediatamente ogni volta che vengono ricevuti i messaggi in arrivo. Al contrario, verranno inviati a una velocità pari a 25 chiamate al secondo, i messaggi per i client gestiti dal `_broadcastLoop` timer dall'interno la `Broadcaster` classe.

    Infine, invece di chiamare il metodo client dall'hub direttamente, il `Broadcaster` classe deve ottenere un riferimento all'hub operativi correnti (`_hubContext`) utilizzando il `GlobalHost`.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. Sposta la forma nella finestra del browser. Non ci sarà una differenza visibile nel browser dalla sezione precedente, ma verrà limitato il numero di messaggi che vengono inviate al client.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Aggiungere animazione fluida nel client

L'applicazione è quasi completata, ma possiamo rendere uno maggiori miglioramenti, il movimento della forma nel client durante lo spostamento in risposta ai messaggi di server. Anziché impostare la posizione della forma per il nuovo percorso assegnato dal server, si userà la libreria di JQuery UI `animate` funzione per spostare la forma in modo uniforme tra la posizione corrente e nuovo.

1. Aggiornare il client `updateShape` metodo per la ricerca, ad esempio il codice evidenziato seguente:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Il codice sopra riportato consente di spostare la forma dalla posizione precedente al nuovo assegnato dal server nel corso dell'intervallo di animazione (in questo caso, 100 millisecondi). Qualsiasi animazione precedente esecuzione della forma viene cancellata prima che inizi la nuova animazione.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. Sposta la forma nella finestra del browser. Lo spostamento della forma in altra finestra dovrebbe essere visualizzato meno scatti come suo movimento è interpolata nel tempo, piuttosto che viene impostata una sola volta per ogni messaggio in arrivo.

    ![La finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come programmare un'applicazione di SignalR che invia messaggi ad alta frequenza tra client e server. Questo paradigma di comunicazione è utile per lo sviluppo di giochi online e altre simulazioni, ad esempio [gioco ShootR creato con SignalR](http://shootr.signalr.net).

L'applicazione completa creata in questa esercitazione può essere scaricato dal [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Per altre informazioni sui concetti di sviluppo SignalR, visitare i siti per il codice sorgente SignalR e le risorse seguenti:

- [Progetto di SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
