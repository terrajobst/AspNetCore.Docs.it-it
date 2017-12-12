---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Esercitazione: Introduzione a SignalR 1. x e MVC 4 | Documenti Microsoft'
author: pfletcher
description: Utilizzare ASP.NET SignalR e ASP.NET MVC 4 per creare un'applicazione di chat in tempo reale.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: e678c85520613fea2a8d00de60aca04d895d6307
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Esercitazione: Introduzione a SignalR 1. x e MVC 4
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> In questa esercitazione viene illustrato come utilizzare ASP.NET SignalR per creare un'applicazione di chat in tempo reale. Verrà aggiungere SignalR a un'applicazione MVC 4 e creare una vista di chat per inviare e visualizzare i messaggi.


## <a name="overview"></a>Panoramica

Questa esercitazione viene illustrato lo sviluppo di applicazioni web in tempo reale con ASP.NET SignalR e ASP.NET MVC 4. L'esercitazione Usa lo stesso codice applicazione chat di [esercitazione introduttiva SignalR](tutorial-getting-started-with-signalr.md), ma viene illustrato come aggiungerlo a un'applicazione MVC 4 in base al modello di Internet.

In questo argomento verranno illustrate le attività di sviluppo SignalR seguenti:

- Aggiunta della libreria di SignalR a un'applicazione MVC 4.
- Creazione di una classe di hub per eseguire il push del contenuto ai client.
- Utilizzando la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.

La schermata seguente viene illustrata l'applicazione di chat completata in esecuzione in un browser.

![Istanze di chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Nelle sezioni:

- [Impostare il progetto](#setup)
- [Eseguire l'esempio](#run)
- [Esaminare il codice](#code)
- [Passaggi successivi](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Impostare il progetto

Prerequisiti:

- Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio Express 2012. Se non si dispone di Visual Studio, vedere [Scarica ASP.NET](https://www.asp.net/downloads) per ottenere il libero Visual Studio 2012 Express strumento di sviluppo.
- Per Visual Studio 2010, installare [ASP.NET MVC 4](https://www.microsoft.com/en-us/download/details.aspx?id=30683).

In questa sezione viene illustrato come creare un'applicazione ASP.NET MVC 4, aggiungere la libreria SignalR e creare l'applicazione di chat.

1. 1. In Visual Studio crea un'applicazione ASP.NET MVC 4, denominarla SignalRChat e fare clic su OK.

        > [!NOTE]
        > In Visual Studio 2010, selezionare **.NET Framework 4** nel controllo elenco a discesa della versione di Framework. Codice di SignalR viene eseguito nelle versioni di .NET Framework 4 e 4.5.

        ![Creare web mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
    2. Selezionare il modello di applicazione Internet, deselezionare l'opzione **creare un progetto di unit test**, fare clic su OK.

        ![Creare il sito internet mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
    3. Aprire il **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguire il comando seguente. Questo passaggio viene aggiunto al progetto un set di file di script e i riferimenti ad assembly che abilita la funzionalità di SignalR.

        `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
    4. In **Esplora** espandere la cartella degli script. Si noti che le librerie di script per SignalR siano stati aggiunti al progetto.

        ![Riferimenti alla libreria](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
    5. In **Esplora**, fare clic sul progetto, selezionare **Aggiungi | Nuova cartella**, e aggiungere una nuova cartella denominata **hub**.
    6. Fare doppio clic su di **hub** cartella, fare clic su **Aggiungi | Classe**e creare una nuova classe c# denominata **ChatHub.cs**. Utilizzare questa classe come un hub di SignalR server che invia messaggi a tutti i client.

> [!NOTE]
> Se si utilizza Visual Studio 2012 e aver installato il [aggiornamento ASP.NET e Web strumenti 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), è possibile utilizzare il nuovo modello di elemento di SignalR per creare la classe di hub. A tale scopo, fare doppio clic su di **hub** cartella, fare clic su **Aggiungi | Nuovo elemento**selezionare **classe Hub SignalR (v1)**e il nome della classe **ChatHub.cs**.


1. Sostituire il codice di **ChatHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Aprire il **Global. asax** file per il progetto e aggiungere una chiamata al metodo `RouteTable.Routes.MapHubs();` come prima riga di codice nel `Application_Start` metodo. Il codice registra la route predefinita per gli hub SignalR e deve essere chiamato prima di registrare qualsiasi altre route. Completato `Application_Start` metodo è simile alla seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Modificare il `HomeController` classe trovata nel **Controllers/HomeController.cs** e aggiungere il metodo seguente alla classe. Questo metodo restituisce il **Chat** visualizzazione che verrà creato in un passaggio successivo.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Fare doppio clic all'interno di `Chat` metodo appena creato, quindi fare clic su **Aggiungi visualizzazione** per creare un nuovo file di visualizzazione.
5. Nel **Aggiungi visualizzazione** finestra di dialogo, verificare che la casella di controllo è selezionata per **utilizza una layout o pagina master** , deselezionare le caselle di controllo e quindi fare clic su **Aggiungi**.

    ![Aggiungere una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Modificare il nuovo file di visualizzazione denominato **Chat.cshtml**. Dopo il &lt;h2&gt; tag, incollare il codice seguente &lt;div&gt; sezione e `@section scripts` blocco di codice nella pagina. Questo script consente la pagina inviare i messaggi di chat e visualizzare i messaggi dal server. Il codice completo per la visualizzazione di chat viene visualizzato nel blocco di codice seguente.

    > [!IMPORTANT]
    > Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, la gestione dei pacchetti potrebbero installare le versioni degli script che sono più recenti rispetto alle versioni illustrate in questo argomento. Assicurarsi che i riferimenti a script nel codice corrispondono alle versioni delle librerie di script installate nel progetto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Salvare tutti** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug.
2. Nella riga dell'indirizzo del browser, aggiungere **/home/chat** all'URL della pagina predefinita per il progetto. Caricamento della pagina di Chat in un'istanza del browser e richiede un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Immettere un nome utente.
4. Copiare l'URL dalla riga dell'indirizzo del browser e utilizzarla per aprire le due altre istanze del browser. In ogni istanza del browser, immettere un nome utente univoco.
5. In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**. I commenti deve essere visualizzato in tutte le istanze del browser.

    > [!NOTE]
    > Questa applicazione di una chat semplice non gestisce il contesto della discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Partecipare alla chat in un secondo momento gli utenti visualizzeranno messaggi aggiunti dal momento in cui che si connettono.
6. La schermata seguente viene illustrata l'applicazione di chat in esecuzione in un browser.

    ![Browser chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. In **Esplora**, controllare il **documenti Script** nodo per l'applicazione in esecuzione. Questo nodo viene visualizzato in modalità di debug, se si utilizza Internet Explorer come browser. È presente un file script denominato **hub** che la libreria SignalR genera in modo dinamico in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server. Se si utilizza un browser diverso da Internet Explorer, è inoltre possibile accedere dinamica **hub** file passando ad esso direttamente, ad esempio http://mywebsite/signalr/hubs.

    ![Script generato hub](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione di chat SignalR illustra due attività di sviluppo SignalR base: creazione di un hub come oggetto di coordinamento principale sul server e utilizza la libreria di jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub di SignalR

Nell'esempio di codice il **ChatHub** deriva dalla classe di **Microsoft.AspNet.SignalR.Hub** classe. Derivazione da di **Hub** classe è utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe hub e quindi accedere a tali metodi mediante chiamata dagli script jQuery in una pagina web.

Nel codice chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio. L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.addNewMessageToPage**.

Il **inviare** metodo illustra i diversi concetti di hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possono essere richiamati.
- Utilizzare il **Microsoft.AspNet.SignalR.Hub.Clients** proprietà per accedere a tutti i client connessi a questo hub.
- Chiamare una funzione di jQuery sul client (ad esempio il `addNewMessageToPage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

Il **Chat.cshtml** Visualizza file nell'esempio di codice viene illustrato come utilizzare la libreria jQuery SignalR per comunicare con un hub di SignalR. Attività essenziali nel codice crea un riferimento per il proxy generato automaticamente per l'hub, dichiarazione di una funzione che il server è possibile chiamare per contenuto push ai client e all'avvio di una connessione per inviare messaggi all'hub.

Il codice seguente dichiara un proxy per un hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> In jQuery il riferimento alla classe del server e i relativi membri è in maiuscole-minuscole camel. L'esempio di codice fa riferimento a c# **ChatHub** classe jQuery come **chatHub**. Se si desidera fare riferimento il `ChatHub` classe in jQuery con Pascal convenzionale maiuscole e minuscole come si farebbe in c#, modificare il file di classe ChatHub.cs. Aggiungere un `using` istruzione per fare riferimento il `Microsoft.AspNet.SignalR.Hubs` dello spazio dei nomi. Aggiungere quindi il `HubName` attributo la `ChatHub` classe, ad esempio `[HubName("ChatHub")]`. Infine, aggiornare il riferimento jQuery la `ChatHub` classe.


Il codice seguente viene illustrato come creare una funzione di callback nello script. Classe dell'hub sul server chiama questa funzione per inviare gli aggiornamenti del contenuto di ogni client. La chiamata a facoltativo di `htmlEncode` funzione Mostra un modo per HTML codifica il contenuto del messaggio prima di visualizzarlo nella pagina, come un modo per impedire attacchi script injection.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Il codice seguente viene illustrato come aprire una connessione con l'hub. Il codice avvia la connessione e passa a una funzione per gestire l'evento click nel **inviare** nella pagina Chat.

> [!NOTE]
> Questo approccio assicura che la connessione viene stabilita prima dell'esecuzione il gestore dell'evento.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale. È stato inoltre diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.

Per informazioni su concetti più avanzati di sviluppi SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
