---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Esercitazione: Introduzione a SignalR 2 | Documenti Microsoft'
author: pfletcher
description: "In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. Verrà aggiunta SignalR a un'applicazione web ASP.NET vuota e verrà creato un pa HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a>Esercitazione: Introduzione a SignalR 2
====================
da [Patrick Fletcher](https://github.com/pfletcher)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

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

Questa esercitazione viene illustrato lo sviluppo di SignalR viene mostrato come compilare un'applicazione di una chat semplice basato su browser. Si verrà aggiungere la libreria SignalR a un'applicazione web ASP.NET vuota, creare una classe di hub per l'invio di messaggi al client e creare una pagina HTML che consente agli utenti di inviare e ricevere messaggi di chat. Per un'esercitazione simile che illustra come creare un'applicazione di chat in MVC 5 con una visualizzazione MVC, vedere [introduzione SignalR 2 e MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Questa esercitazione viene illustrato come creare applicazioni SignalR in versione 2. Per informazioni dettagliate sulle modifiche tra SignalR 1. x e 2, vedere [SignalR aggiornamento 1. x progetti](../releases/upgrading-signalr-1x-projects-to-20.md) e [note sulla versione di Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR è una libreria .NET open source per la compilazione di applicazioni web che richiedono l'intervento dell'utente in tempo reale o gli aggiornamenti dei dati in tempo reale. Sono esempi di applicazioni social networking, giochi multiutente, meteo di collaborazione e notizie, business o finanziari aggiornamento delle applicazioni. Spesso si tratta di applicazioni in tempo reale.

SignalR semplifica il processo di compilazione di applicazioni in tempo reale. Include una libreria del server ASP.NET e una libreria client JavaScript per renderne più semplice gestire le connessioni client-server e inviare gli aggiornamenti del contenuto ai client. È possibile aggiungere la libreria SignalR a un'applicazione ASP.NET esistente per ottenere le funzionalità in tempo reale.

Nell'esercitazione vengono illustrate le seguenti attività di sviluppo SignalR:

- Aggiunta della libreria di SignalR a un'applicazione web ASP.NET.
- Creazione di una classe di hub per eseguire il push del contenuto ai client.
- Creazione di una classe di avvio OWIN per configurare l'applicazione.
- Utilizzando la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.

La schermata seguente viene illustrata l'applicazione di chat in esecuzione in un browser. Ogni nuovo utente è possibile inviare commenti e vedere i commenti aggiunti dopo che l'utente viene aggiunto alla chat.

![Istanze di chat](tutorial-getting-started-with-signalr/_static/image1.png)

Nelle sezioni:

- [Impostare il progetto](#setup)
- [Eseguire l'esempio](#run)
- [Esaminare il codice](#code)
- [Passaggi successivi](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Impostare il progetto

In questa sezione viene illustrato come utilizzare Visual Studio 2013 e SignalR versione 2 per creare applicazioni web ASP.NET vuota aggiungere SignalR e creare l'applicazione di chat.

Prerequisiti:

- Visual Studio 2013. Se non si dispone di Visual Studio, vedere [Scarica ASP.NET](https://www.asp.net/downloads) per ottenere il libero Visual Studio 2013 Express strumento di sviluppo.

La procedura seguente utilizza Visual Studio 2013 per creare un'applicazione Web vuota ASP.NET e aggiungere la libreria SignalR:

1. In Visual Studio, creare un'applicazione Web ASP.NET.

    ![Creare web](tutorial-getting-started-with-signalr/_static/image2.png)
2. Nel **nuovo progetto ASP.NET** finestra, lasciare **vuoto** selezionata e fare clic su **Crea progetto**.

    ![Creare web vuoto](tutorial-getting-started-with-signalr/_static/image3.png)
3. In **Esplora**, fare clic sul progetto, selezionare **Aggiungi | Classe Hub SignalR (v2)**. Nome della classe **ChatHub.cs** e aggiungerlo al progetto. Questo passaggio viene creata la **ChatHub** classe e lo aggiunge al progetto un set di file di script e i riferimenti ad assembly che supportano SignalR.

    > [!NOTE]
    > È anche possibile aggiungere SignalR a un progetto, aprire il **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguendo un comando:

    `install-package Microsoft.AspNet.SignalR`

    Se si utilizza la console per aggiungere SignalR, creare la classe di hub SignalR come passaggio separato dopo l'aggiunta di SignalR.

    > [!NOTE]
    > Se si utilizza Visual Studio 2012, la **classe Hub SignalR (v2)** modello non sarà disponibile. È possibile aggiungere un normale **classe** chiamato `ChatHub` invece.
4. In **Esplora**, espandere il nodo di script. Librerie di script per jQuery e SignalR sono visibili nel progetto.
5. Sostituire il codice nella nuova **ChatHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Classe di avvio OWIN**. Denominare la nuova classe `Startup` e fare clic su OK.

    > [!NOTE]
    > Se si utilizza Visual Studio 2012, la **classe di avvio di OWIN** modello non sarà disponibile. È possibile aggiungere un normale **classe** chiamato `Startup` invece.
7. Modificare il contenuto della nuova classe di avvio per le operazioni seguenti.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Pagina HTML**. Denominare la nuova pagina `index.html`.
    >[!NOTE]
    >Potrebbe essere necessario modificare i numeri di versione per i riferimenti alle librerie JQuery e SignalR
9. In **Esplora**, fare clic sulla pagina HTML appena creato e fare clic su **imposta come pagina iniziale**.
10. Sostituire il codice predefinito nella pagina HTML con il codice seguente.

    > [!NOTE]
    > Una versione successiva degli script SignalR può essere installata mediante la gestione dei pacchetti. Verificare che i riferimenti a script riportato di seguito corrispondono alle versioni dei file di script nel progetto (sarà diversi se è stato aggiunto mediante NuGet anziché aggiungendo un hub di SignalR.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Salvare tutti** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug. La pagina HTML viene caricato in un'istanza del browser e richiede un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image4.png)
2. Immettere un nome utente.
3. Copiare l'URL dalla riga dell'indirizzo del browser e utilizzarla per aprire le due altre istanze del browser. In ogni istanza del browser, immettere un nome utente univoco.
4. In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**. I commenti deve essere visualizzato in tutte le istanze del browser.

    > [!NOTE]
    > Questa applicazione di una chat semplice non gestisce il contesto della discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Partecipare alla chat in un secondo momento gli utenti visualizzeranno messaggi aggiunti dal momento in cui che si connettono.

    La schermata seguente viene illustrata l'applicazione di chat in esecuzione in tre istanze del browser, ognuno dei quali vengono aggiornati quando un'istanza invia un messaggio:

    ![Browser chat](tutorial-getting-started-with-signalr/_static/image5.png)
5. In **Esplora**, controllare il **documenti Script** nodo per l'applicazione in esecuzione. È presente un file script denominato **hub** che la libreria SignalR genera in modo dinamico in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione di chat SignalR illustra due attività di sviluppo SignalR base: creazione di un hub come oggetto di coordinamento principale sul server e utilizza la libreria di jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub di SignalR

Nell'esempio di codice il **ChatHub** deriva dalla classe di **Microsoft.AspNet.SignalR.Hub** classe. Derivazione da di **Hub** classe è utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe hub e quindi accedere a tali metodi mediante chiamata dagli script in una pagina web.

Nel codice chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio. L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.broadcastMessage**.

Il **inviare** metodo illustra i diversi concetti di hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possono essere richiamati.
- Utilizzare il **Microsoft.AspNet.SignalR.Hub.Clients** proprietà dinamiche per accedere a tutti i client connessi a questo hub.
- Chiamare una funzione nel client (ad esempio il `broadcastMessage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

La pagina HTML nell'esempio di codice viene illustrato come utilizzare la libreria jQuery SignalR per comunicare con un hub di SignalR. Attività essenziali nel codice sta dichiarando un proxy per fare riferimento all'hub, dichiarazione di una funzione che il server è possibile chiamare per contenuto push ai client e all'avvio di una connessione per inviare messaggi all'hub.

Il codice seguente dichiara un riferimento a un proxy dell'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> In JavaScript, il riferimento alla classe del server e i relativi membri è in maiuscole-minuscole camel. L'esempio di codice fa riferimento a c# **ChatHub** classe in JavaScript come **chatHub**.


Il codice seguente è come si crea una funzione di callback nello script. Classe dell'hub sul server chiama questa funzione per inviare gli aggiornamenti del contenuto di ogni client. Le due righe che HTML codificare il contenuto prima di visualizzarlo sono facoltative e mostrano un modo semplice per impedire attacchi script injection.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Il codice seguente viene illustrato come aprire una connessione con l'hub. Il codice avvia la connessione e passa a una funzione per gestire l'evento click nel **inviare** nella pagina HTML.

> [!NOTE]
> Questo approccio assicura che la connessione viene stabilita prima dell'esecuzione il gestore dell'evento.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale. È stato inoltre diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.

Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR in Azure, vedere [utilizzando SignalR con le app Web in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Per informazioni dettagliate su come distribuire un progetto web di Visual Studio a un sito Web di Windows Azure, vedere [creare un'app web ASP.NET in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Per informazioni su concetti più avanzati di sviluppi SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
