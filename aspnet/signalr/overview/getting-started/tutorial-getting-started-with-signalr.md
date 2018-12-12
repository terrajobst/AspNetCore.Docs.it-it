---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Esercitazione: Introduzione a SignalR 2 | Microsoft Docs'
author: pfletcher
description: In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. Verrà aggiunta SignalR a un'applicazione web ASP.NET vuota e verrà creato un indirizzo pa HTML...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3b06e7d0a602e061112adbceba92276f836f6311
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287338"
---
<a name="tutorial-getting-started-with-signalr-2"></a>Esercitazione: Introduzione a SignalR 2
====================
da [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Download progetto completato](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. Verrà aggiunta SignalR a un'applicazione web ASP.NET vuota e creare una pagina HTML per l'invio e visualizzare i messaggi. 
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

Questa esercitazione introduce lo sviluppo di SignalR, che illustrano come creare un'applicazione di chat basata su browser semplice. Si verrà aggiungere la libreria di SignalR a un'applicazione web ASP.NET vuota, creare una classe di hub per l'invio di messaggi per i client e creare una pagina HTML che consente agli utenti di inviare e ricevere i messaggi di chat. Per un'esercitazione simile che mostra come creare un'applicazione di chat in MVC 5 con una visualizzazione MVC, vedere [Introduzione a SignalR 2 e MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Questa esercitazione illustra come creare applicazioni SignalR in versione 2. Per informazioni dettagliate sulle modifiche apportate tra SignalR 1.x e 2, vedere [i progetti di aggiornamento SignalR 1.x](../releases/upgrading-signalr-1x-projects-to-20.md) e [note sulla versione di Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR è una libreria .NET open source per la compilazione di applicazioni web che richiedono l'interazione dell'utente in tempo reale o gli aggiornamenti dei dati in tempo reale. Ad esempio le applicazioni basati su social network, giochi multiutente, meteo, collaborazione e nelle novità, business o finanziari aggiornare le applicazioni. Spesso si tratta di applicazioni in tempo reale.

SignalR semplifica il processo di compilazione di applicazioni in tempo reale. Include una raccolta di server ASP.NET e una libreria client JavaScript per renderne più semplice gestire le connessioni client a server e il push degli aggiornamenti del contenuto ai client. È possibile aggiungere la libreria di SignalR a un'applicazione ASP.NET esistente per ottenere funzionalità in tempo reale.

L'esercitazione illustra le attività di sviluppo SignalR seguenti:

- Aggiunta della libreria di SignalR a un'applicazione web ASP.NET.
- Creazione di una classe di hub per eseguire il push del contenuto ai client.
- Creazione di una classe di avvio OWIN per configurare l'applicazione.
- Usa la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.

Lo screenshot seguente mostra l'applicazione di chat in esecuzione in un browser. Ogni nuovo utente è possibile inviare commenti e vedere i commenti aggiunti dopo che l'utente viene aggiunto alla chat.

![Istanze di chat](tutorial-getting-started-with-signalr/_static/image1.png)

Nelle sezioni:

- [Configurare il progetto](#setup)
- [Eseguire l'esempio](#run)
- [Esaminare il codice](#code)
- [Passaggi successivi](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurare il progetto

Questa sezione illustra come usare Visual Studio 2013 e versione 2 di SignalR per creare un'applicazione web ASP.NET vuota, aggiungere SignalR e creare l'applicazione di chat.

Prerequisiti:

- Visual Studio 2013. Se non hai Visual Studio, vedere [download ASP.NET](https://www.asp.net/downloads) per ottenere il Visual Studio 2013 Express strumento di sviluppo gratuito.

La procedura seguente Usa Visual Studio 2013 per creare un'applicazione Web ASP.NET vuota e aggiungere la libreria di SignalR:

1. In Visual Studio, creare un'applicazione Web ASP.NET.

    ![Creazione di web](tutorial-getting-started-with-signalr/_static/image2.png)
2. Nel **nuovo progetto ASP.NET** finestra, lasciare **vuota** selezionata e fare clic su **Crea progetto**.

    ![Creazione di web vuoto](tutorial-getting-started-with-signalr/_static/image3.png)
3. Nelle **Esplora soluzioni**, fare clic sul progetto, selezionare **Add | Classe SignalR Hub (v2)**. Denominare la classe **ChatHub.cs** e aggiungerlo al progetto. Questo passaggio Crea il **ChatHub** classe e viene aggiunto al progetto un set di file di script e i riferimenti ad assembly che supportano SignalR.

    > [!NOTE]
    > È inoltre possibile aggiungere SignalR a un progetto aprendo il **strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti** ed eseguire un comando:

    `install-package Microsoft.AspNet.SignalR`

    Se si utilizza la console per aggiungere SignalR, creare la classe hub SignalR come passaggio distinto dopo l'aggiunta di SignalR.

    > [!NOTE]
    > Se si usa Visual Studio 2012, il **classe tramite SignalR Hub (v2)** modello non saranno disponibile. È possibile aggiungere un normale **classe** chiamato `ChatHub` invece.
4. Nelle **Esplora soluzioni**, espandere il nodo di script. Librerie di script per jQuery e SignalR sono visibili nel progetto.
5. Sostituire il codice nella nuova **ChatHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Classe di avvio OWIN**. Denominare la nuova classe `Startup` e fare clic su OK.

    > [!NOTE]
    > Se si usa Visual Studio 2012, il **classe di avvio OWIN** modello non saranno disponibile. È possibile aggiungere un normale **classe** chiamato `Startup` invece.
7. Modificare il contenuto della nuova classe di avvio come segue.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Pagina HTML**. Denominare la nuova pagina `index.html`.
    >[!NOTE]
    >Potrebbe essere necessario modificare i numeri di versione per i riferimenti alle librerie JQuery e SignalR
9. Nelle **Esplora soluzioni**, fare doppio clic su pagina HTML appena creato e fare clic su **imposta come pagina iniziale**.
10. Sostituire il codice predefinito nella pagina HTML con il codice seguente.

    > [!NOTE]
    > Per la gestione dei pacchetti può essere installata una versione successiva degli script di SignalR. Verificare che i riferimenti allo script riportato di seguito corrispondono alle versioni dei file di script nel progetto (che sarà diversi se è stato aggiunto mediante NuGet anziché aggiungendo un hub di SignalR.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Salva tutto** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug. La pagina HTML viene caricata in un'istanza del browser e richiede un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image4.png)
2. Immettere un nome utente.
3. Copiare l'URL dalla riga dell'indirizzo del browser e usarlo per aprire due altre istanze del browser. In ogni istanza del browser immettere un nome utente univoco.
4. In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**. I commenti deve essere visualizzato in tutte le istanze del browser.

    > [!NOTE]
    > Questa applicazione di chat semplice non gestisce il contesto di discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.

    Lo screenshot seguente mostra l'applicazione di chat in esecuzione in tre istanze del browser, ognuno dei quali vengono aggiornate quando un'istanza di invia un messaggio:

    ![Browser di chat](tutorial-getting-started-with-signalr/_static/image5.png)
5. Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione. È presente un file di script denominato **hub** che la libreria di SignalR genera in modo dinamico in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione di chat SignalR illustra due attività di sviluppo di SignalR base: creazione di un hub come oggetto di coordinamento principale nel server e usando la libreria jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub SignalR

Nell'esempio di codice le **ChatHub** deriva dalla classe la **Microsoft.AspNet.SignalR.Hub** classe. Derivazione dal **Hub** classe è un modo utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe dell'hub e quindi accedere a tali metodi chiamandole dagli script in una pagina web.

Nel codice di chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio. L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.broadcastMessage**.

Il **inviare** metodo illustra alcuni concetti di hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.
- Usare la **Microsoft.AspNet.SignalR.Hub.Clients** proprietà dinamiche per accedere a tutti i client connessi a questo hub.
- Chiamare una funzione nel client (ad esempio il `broadcastMessage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

La pagina HTML nel codice di esempio mostra come usare la libreria jQuery SignalR per comunicare con un hub SignalR. Le attività di base nel codice siano dichiarando un proxy per l'hub, dichiara una funzione che il server può chiamare per contenuto push ai client e l'avvio di una connessione per inviare messaggi all'hub di riferimento.

Il codice seguente dichiara un riferimento a un proxy di hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> In JavaScript il riferimento alla classe server e i relativi membri è in maiuscole/minuscole camel. Nell'esempio di codice fa riferimento il codice c# **ChatHub** classi in JavaScript come **chatHub**.


Il codice seguente è come si crea una funzione di callback nello script. La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client. Le due righe che HTML codificare il contenuto prima di visualizzarla sono facoltative e mostrano un modo semplice per impedire attacchi script injection.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Il codice seguente viene illustrato come aprire una connessione con l'hub. Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina HTML.

> [!NOTE]
> Questo approccio assicura che la connessione viene stabilita prima esecuzione del gestore dell'evento.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale. Si è appreso anche diverse attività di sviluppo di SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.

Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR in Azure, vedere [uso di SignalR con App Web nel servizio App di Azure](../deployment/using-signalr-with-azure-web-sites.md). Per informazioni dettagliate su come distribuire un progetto web Visual Studio per un sito Web di Microsoft Azure, vedere [creare un'app web ASP.NET in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Per informazioni su concetti più avanzati sviluppi in materia di SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto di SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
