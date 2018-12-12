---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Esercitazione: Introduzione a SignalR 1.x | Microsoft Docs'
author: pfletcher
description: Usare SignalR ASP.NET per compilare un'applicazione di chat in tempo reale in una pagina HTML.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 720a4879f5fbe3c0c2b4c7809cb94c22547329c3
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287363"
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Esercitazione: Introduzione a SignalR 1.x
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. Verrà aggiunta SignalR a un'applicazione web ASP.NET vuota e creare una pagina HTML per l'invio e visualizzare i messaggi.


## <a name="overview"></a>Panoramica

Questa esercitazione introduce lo sviluppo di SignalR, che illustrano come creare un'applicazione di chat basata su browser semplice. Si verrà aggiungere la libreria di SignalR a un'applicazione web ASP.NET vuota, creare una classe di hub per l'invio di messaggi per i client e creare una pagina HTML che consente agli utenti di inviare e ricevere i messaggi di chat. Per un'esercitazione simile che mostra come creare un'applicazione di chat in MVC 4 con una visualizzazione MVC, vedere [Introduzione a SignalR e MVC 4](index.md).

> [!NOTE]
> Questa esercitazione Usa la versione finale (1.x) di SignalR. Per informazioni dettagliate sulle modifiche apportate tra SignalR 1.x e 2.0, vedere [l'aggiornamento di SignalR 1.x progetti](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR è una libreria .NET open source per la compilazione di applicazioni web che richiedono l'interazione dell'utente in tempo reale o gli aggiornamenti dei dati in tempo reale. Ad esempio le applicazioni basati su social network, giochi multiutente, meteo, collaborazione e nelle novità, business o finanziari aggiornare le applicazioni. Spesso si tratta di applicazioni in tempo reale.

SignalR semplifica il processo di compilazione di applicazioni in tempo reale. Include una raccolta di server ASP.NET e una libreria client JavaScript per renderne più semplice gestire le connessioni client a server e il push degli aggiornamenti del contenuto ai client. È possibile aggiungere la libreria di SignalR a un'applicazione ASP.NET esistente per ottenere funzionalità in tempo reale.

L'esercitazione illustra le attività di sviluppo SignalR seguenti:

- Aggiunta della libreria di SignalR a un'applicazione web ASP.NET.
- Creazione di una classe di hub per eseguire il push del contenuto ai client.
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

In questa sezione viene illustrato come creare un'applicazione web ASP.NET vuota, aggiungere SignalR e creare l'applicazione di chat.

Prerequisiti:

- Visual Studio 2010 SP1 o 2012. Se non hai Visual Studio, vedere [download ASP.NET](https://www.asp.net/downloads) per ottenere il Visual Studio 2012 Express strumento di sviluppo gratuito.
- [Microsoft ASP.NET e Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Per Visual Studio 2012, il programma di installazione aggiunge nuove funzionalità ASP.NET, inclusi i modelli di SignalR per Visual Studio. Per Visual Studio 2010 SP1, non è disponibile un programma di installazione, ma è possibile completare l'esercitazione installando il pacchetto NuGet di SignalR, come descritto nei passaggi di configurazione.

La procedura seguente Usa Visual Studio 2012 per creare un'applicazione Web ASP.NET vuota e aggiungere la libreria di SignalR:

1. In Visual Studio creare un'applicazione Web ASP.NET vuota.

    ![Creazione di web vuoto](tutorial-getting-started-with-signalr/_static/image2.png)
2. Aprire il **Console di gestione pacchetti** selezionando **strumenti | Gestione pacchetti NuGet | Console di gestione pacchetti**. Immettere il comando seguente nella finestra della console:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Questo comando installa la versione più recente di SignalR 1.x.
3. Nelle **Esplora soluzioni**, fare clic sul progetto, selezionare **Add | Classe**. Denominare la nuova classe **ChatHub**.
4. Nelle **Esplora soluzioni** espandere il nodo di script. Librerie di script per jQuery e SignalR sono visibili nel progetto.

    ![Riferimenti alla libreria](tutorial-getting-started-with-signalr/_static/image3.png)
5. Sostituire il codice nel **ChatHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona **classe di applicazione globale** e fare clic su **Add**.

    ![Aggiungere globale](tutorial-getting-started-with-signalr/_static/image4.png)
7. Aggiungere il codice seguente `using` istruzioni dopo l'oggetto fornito `using` le istruzioni nella classe Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Aggiungere la riga di codice seguente il `Application_Start` metodo della classe globale per registrare la route predefinita per gli hub SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo, selezionare la pagina Html e fare clic su **Add**.
10. Nelle **Esplora soluzioni**, fare doppio clic su pagina HTML appena creato e fare clic su **imposta come pagina iniziale**.
11. Sostituire il codice predefinito nella pagina HTML con il codice seguente.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Salva tutto** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug. La pagina HTML viene caricata in un'istanza del browser e richiede un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image5.png)
2. Immettere un nome utente.
3. Copiare l'URL dalla riga dell'indirizzo del browser e usarlo per aprire due altre istanze del browser. In ogni istanza del browser immettere un nome utente univoco.
4. In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**. I commenti deve essere visualizzato in tutte le istanze del browser.

    > [!NOTE]
    > Questa applicazione di chat semplice non gestisce il contesto di discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.

    Lo screenshot seguente mostra l'applicazione di chat in esecuzione in tre istanze del browser, ognuno dei quali vengono aggiornate quando un'istanza di invia un messaggio:

    ![Browser di chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione. È presente un file di script denominato **hub** che la libreria di SignalR genera in modo dinamico in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.

    ![Script generato hub](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione di chat SignalR illustra due attività di sviluppo di SignalR base: creazione di un hub come oggetto di coordinamento principale nel server e usando la libreria jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub SignalR

Nell'esempio di codice le **ChatHub** deriva dalla classe la **Microsoft.AspNet.SignalR.Hub** classe. Derivazione dal **Hub** classe è un modo utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe dell'hub e quindi accedere a tali metodi chiamandole dagli script jQuery in una pagina web.

Nel codice di chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio. L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.broadcastMessage**.

Il **inviare** metodo illustra alcuni concetti di hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.
- Usare la **Microsoft.AspNet.SignalR.Hub.Clients** proprietà dinamiche per accedere a tutti i client connessi a questo hub.
- Chiamare una funzione di jQuery nel client (ad esempio il `broadcastMessage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

La pagina HTML nel codice di esempio mostra come usare la libreria jQuery SignalR per comunicare con un hub SignalR. Le attività di base nel codice siano dichiarando un proxy per l'hub, dichiara una funzione che il server può chiamare per contenuto push ai client e l'avvio di una connessione per inviare messaggi all'hub di riferimento.

Il codice seguente dichiara un proxy per un hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> In jQuery il riferimento alla classe server e i relativi membri è in maiuscole/minuscole camel. Nell'esempio di codice fa riferimento il codice c# **ChatHub** classe in jQuery come **chatHub**.


Il codice seguente è come si crea una funzione di callback nello script. La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client. Le due righe che HTML codificare il contenuto prima di visualizzarla sono facoltative e mostrano un modo semplice per impedire attacchi script injection.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Il codice seguente viene illustrato come aprire una connessione con l'hub. Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina HTML.

> [!NOTE]
> Questo approccio assicura che la connessione viene stabilita prima esecuzione del gestore dell'evento.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale. Si è appreso anche diverse attività di sviluppo di SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.

È possibile rendere l'applicazione di esempio in questa esercitazione o altre applicazioni SignalR disponibile tramite Internet per la distribuzione in un provider di hosting. Microsoft offre l'hosting web gratuito per fino a 10 siti web in una liberazione [account di valutazione di Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR, vedere [pubblicare i SignalR esempio della Guida introduttiva come un sito Web di Microsoft Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Per informazioni dettagliate su come distribuire un progetto web Visual Studio per un sito Web di Microsoft Azure, vedere [distribuzione di un'applicazione ASP.NET a un sito Web di Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Nota: Il trasporto WebSocket non è attualmente supportato per siti Web di Azure. Il trasporto WebSocket quando non è disponibile, SignalR utilizza altri trasporti disponibili come descritto nella sezione dei trasporti del [Introduzione a SignalR argomento](index.md).)

Per informazioni su concetti più avanzati sviluppi in materia di SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto di SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
