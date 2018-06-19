---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Esercitazione: Introduzione a SignalR 1. x | Documenti Microsoft'
author: pfletcher
description: Utilizzare ASP.NET SignalR per compilare un'applicazione di chat in tempo reale in una pagina HTML.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044734"
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Esercitazione: Introduzione a SignalR 1. x
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. Verrà aggiunta SignalR a un'applicazione web ASP.NET vuota e creare una pagina HTML per l'invio e visualizzare i messaggi.


## <a name="overview"></a>Panoramica

Questa esercitazione viene illustrato lo sviluppo di SignalR viene mostrato come compilare un'applicazione di una chat semplice basato su browser. Si verrà aggiungere la libreria SignalR a un'applicazione web ASP.NET vuota, creare una classe di hub per l'invio di messaggi al client e creare una pagina HTML che consente agli utenti di inviare e ricevere messaggi di chat. Per un'esercitazione simile che illustra come creare un'applicazione di chat in MVC 4 con una visualizzazione MVC, vedere [Guida introduttiva a SignalR e MVC 4](index.md).

> [!NOTE]
> Questa esercitazione Usa la versione (1. x) di SignalR. Per informazioni dettagliate sulle modifiche tra SignalR 1. x e 2.0, vedere [SignalR aggiornamento 1. x progetti](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR è una libreria .NET open source per la compilazione di applicazioni web che richiedono l'intervento dell'utente in tempo reale o gli aggiornamenti dei dati in tempo reale. Sono esempi di applicazioni social networking, giochi multiutente, meteo di collaborazione e notizie, business o finanziari aggiornamento delle applicazioni. Spesso si tratta di applicazioni in tempo reale.

SignalR semplifica il processo di compilazione di applicazioni in tempo reale. Include una libreria del server ASP.NET e una libreria client JavaScript per renderne più semplice gestire le connessioni client-server e inviare gli aggiornamenti del contenuto ai client. È possibile aggiungere la libreria SignalR a un'applicazione ASP.NET esistente per ottenere le funzionalità in tempo reale.

Nell'esercitazione vengono illustrate le seguenti attività di sviluppo SignalR:

- Aggiunta della libreria di SignalR a un'applicazione web ASP.NET.
- Creazione di una classe di hub per eseguire il push del contenuto ai client.
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

In questa sezione viene illustrato come creare applicazioni web ASP.NET vuota aggiungere SignalR e creare l'applicazione di chat.

Prerequisiti:

- Visual Studio 2010 SP1 o 2012. Se non si dispone di Visual Studio, vedere [Scarica ASP.NET](https://www.asp.net/downloads) per ottenere il libero Visual Studio 2012 Express strumento di sviluppo.
- [Microsoft Web ASP.NET e strumenti 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Per Visual Studio 2012, il programma di installazione aggiunge nuove funzionalità ASP.NET, inclusi i modelli di SignalR per Visual Studio. Per Visual Studio 2010 SP1, un programma di installazione non è disponibile, ma è possibile completare l'esercitazione installando il pacchetto SignalR NuGet, come descritto nei passaggi di installazione.

La procedura seguente utilizza Visual Studio 2012 per creare un'applicazione Web vuota ASP.NET e aggiungere la libreria SignalR:

1. In Visual Studio, creare un'applicazione Web ASP.NET vuota.

    ![Creare web vuoto](tutorial-getting-started-with-signalr/_static/image2.png)
2. Aprire il **Package Manager Console** selezionando **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**. Immettere il comando seguente nella finestra della console:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Questo comando consente di installare la versione più recente di SignalR 1. x.
3. In **Esplora**, fare clic sul progetto, selezionare **Aggiungi | Classe**. Denominare la nuova classe **ChatHub**.
4. In **Esplora** espandere il nodo di script. Librerie di script per jQuery e SignalR sono visibili nel progetto.

    ![Riferimenti alla libreria](tutorial-getting-started-with-signalr/_static/image3.png)
5. Sostituire il codice di **ChatHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona **classe di applicazione globale** e fare clic su **Aggiungi**.

    ![Aggiungere globale](tutorial-getting-started-with-signalr/_static/image4.png)
7. Aggiungere il seguente `using` istruzioni dopo forniti `using` istruzioni nella classe Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Aggiungere la riga di codice seguente il `Application_Start` metodo della classe globale per registrare la route predefinita per gli hub SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo, selezionare la pagina Html e fare clic su **Aggiungi**.
10. In **Esplora**, fare clic sulla pagina HTML appena creato e fare clic su **imposta come pagina iniziale**.
11. Sostituire il codice predefinito nella pagina HTML con il codice seguente.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Salvare tutti** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug. La pagina HTML viene caricato in un'istanza del browser e richiede un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image5.png)
2. Immettere un nome utente.
3. Copiare l'URL dalla riga dell'indirizzo del browser e utilizzarla per aprire le due altre istanze del browser. In ogni istanza del browser, immettere un nome utente univoco.
4. In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**. I commenti deve essere visualizzato in tutte le istanze del browser.

    > [!NOTE]
    > Questa applicazione di una chat semplice non gestisce il contesto della discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Partecipare alla chat in un secondo momento gli utenti visualizzeranno messaggi aggiunti dal momento in cui che si connettono.

    La schermata seguente viene illustrata l'applicazione di chat in esecuzione in tre istanze del browser, ognuno dei quali vengono aggiornati quando un'istanza invia un messaggio:

    ![Browser chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. In **Esplora**, controllare il **documenti Script** nodo per l'applicazione in esecuzione. È presente un file script denominato **hub** che la libreria SignalR genera in modo dinamico in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.

    ![Script generato hub](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione di chat SignalR illustra due attività di sviluppo SignalR base: creazione di un hub come oggetto di coordinamento principale sul server e utilizza la libreria di jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub di SignalR

Nell'esempio di codice il **ChatHub** deriva dalla classe di **Microsoft.AspNet.SignalR.Hub** classe. Derivazione da di **Hub** classe è utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe hub e quindi accedere a tali metodi mediante chiamata dagli script jQuery in una pagina web.

Nel codice chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio. L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.broadcastMessage**.

Il **inviare** metodo illustra i diversi concetti di hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possono essere richiamati.
- Utilizzare il **Microsoft.AspNet.SignalR.Hub.Clients** proprietà dinamiche per accedere a tutti i client connessi a questo hub.
- Chiamare una funzione di jQuery sul client (ad esempio il `broadcastMessage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

La pagina HTML nell'esempio di codice viene illustrato come utilizzare la libreria jQuery SignalR per comunicare con un hub di SignalR. Attività essenziali nel codice sta dichiarando un proxy per fare riferimento all'hub, dichiarazione di una funzione che il server è possibile chiamare per contenuto push ai client e all'avvio di una connessione per inviare messaggi all'hub.

Il codice seguente dichiara un proxy per un hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> In jQuery il riferimento alla classe del server e i relativi membri è in maiuscole-minuscole camel. L'esempio di codice fa riferimento a c# **ChatHub** classe jQuery come **chatHub**.


Il codice seguente è come si crea una funzione di callback nello script. Classe dell'hub sul server chiama questa funzione per inviare gli aggiornamenti del contenuto di ogni client. Le due righe che HTML codificare il contenuto prima di visualizzarlo sono facoltative e mostrano un modo semplice per impedire attacchi script injection.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Il codice seguente viene illustrato come aprire una connessione con l'hub. Il codice avvia la connessione e passa a una funzione per gestire l'evento click nel **inviare** nella pagina HTML.

> [!NOTE]
> Questo approccio assicura che la connessione viene stabilita prima dell'esecuzione il gestore dell'evento.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale. È stato inoltre diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.

È possibile rendere l'applicazione di esempio in questa esercitazione o altre applicazioni SignalR disponibili su Internet distribuendole da un provider di hosting. Microsoft offre l'hosting web gratuito per fino a 10 siti web gratuito [account di valutazione di Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR, vedere [pubblicare il SignalR esempio della Guida introduttiva di un sito Web di Microsoft Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Per informazioni dettagliate su come distribuire un progetto web di Visual Studio a un sito Web di Windows Azure, vedere [si distribuisce un'applicazione ASP.NET a un sito Web di Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Nota: il trasporto WebSocket non è attualmente supportato per siti Web di Windows Azure. Trasporto WebSocket quando non è disponibile, SignalR utilizza gli altri trasporti disponibili come descritto nella sezione di trasporti di [Introduzione all'argomento SignalR](index.md).)

Per informazioni su concetti più avanzati di sviluppi SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
