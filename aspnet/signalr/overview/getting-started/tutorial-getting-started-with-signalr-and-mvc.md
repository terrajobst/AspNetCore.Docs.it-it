---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Esercitazione: Introduzione a SignalR 2 e MVC 5 | Documenti Microsoft'
author: pfletcher
description: "In questa esercitazione viene illustrato come utilizzare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. Si verrà aggiunto SignalR a un'applicazione MVC 5 e creare una vista di chat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Esercitazione: Introduzione a SignalR 2 e MVC 5
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> In questa esercitazione viene illustrato come utilizzare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. Verrà aggiungere SignalR a un'applicazione MVC 5 e creare una vista di chat per inviare e visualizzare i messaggi. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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

Questa esercitazione viene illustrato lo sviluppo di applicazioni web in tempo reale con ASP.NET SignalR 2 e MVC ASP.NET 5. L'esercitazione Usa lo stesso codice applicazione chat di [esercitazione introduttiva SignalR](tutorial-getting-started-with-signalr.md), ma viene illustrato come aggiungerlo a un'applicazione MVC 5.

In questo argomento verranno illustrate le attività di sviluppo SignalR seguenti:

- Aggiunta della libreria di SignalR a un'applicazione MVC 5.
- Creazione di hub e le classi di avvio OWIN per effettuare il push del contenuto ai client.
- Utilizzando la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.

La schermata seguente viene illustrata l'applicazione di chat completata in esecuzione in un browser.

![Istanze di chat](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Nelle sezioni:

- [Impostare il progetto](#setup)
- [Eseguire l'esempio](#run)
- [Esaminare il codice](#code)
- [Passaggi successivi](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Impostare il progetto

Prerequisiti:

- Visual Studio 2013. Se non si dispone di Visual Studio, vedere [Scarica ASP.NET](https://www.asp.net/downloads) per ottenere il libero Visual Studio 2013 Express strumento di sviluppo.

In questa sezione viene illustrato come creare un'applicazione ASP.NET MVC 5, aggiungere la libreria SignalR e creare l'applicazione di chat.

1. In Visual Studio, creare un'applicazione c# ASP.NET destinato a .NET Framework 4.5, denominarla SignalRChat e fare clic su OK.

    ![Creare web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. Nel `New ASP.NET Project` finestra di dialogo e selezionare **MVC**, fare clic su **Modifica autenticazione**.

    ![Creare web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Selezionare **Nessuna autenticazione** nel **Modifica autenticazione** finestra di dialogo e fare clic su **OK**.

    ![Non selezionare Nessuna autenticazione](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Se si seleziona un provider di autenticazione diversi per l'applicazione, un `Startup.cs` classe verrà creata automaticamente; non è necessario crearne di proprie `Startup.cs` classe nel passaggio 10 seguente.
4. Fare clic su **OK** nel **nuovo progetto ASP.NET** finestra di dialogo.
5. Aprire il **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguire il comando seguente. Questo passaggio viene aggiunto al progetto un set di file di script e i riferimenti ad assembly che abilita la funzionalità di SignalR.

    `install-package Microsoft.AspNet.SignalR`
6. In **Esplora**, espandere la cartella degli script. Si noti che le librerie di script per SignalR siano stati aggiunti al progetto.

    ![Cartella degli script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. In **Esplora**, fare clic sul progetto, selezionare **Aggiungi | Nuova cartella**, e aggiungere una nuova cartella denominata **hub**.
8. Fare doppio clic su di **hub** cartella, fare clic su **Aggiungi | Nuovo elemento**, selezionare il **Visual c# | Web | SignalR** nodo il **installato** riquadro, selezionare **classe Hub SignalR (v2)** dal riquadro centrale e creare un nuovo hub denominato **ChatHub.cs**. Utilizzare questa classe come un hub di SignalR server che invia messaggi a tutti i client.

    ![Crea nuovo hub](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Sostituire il codice di **ChatHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Creare una nuova classe denominata Startup.cs. Modificare il contenuto del file per le operazioni seguenti.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Modificare il `HomeController` classe trovata nel **Controllers/HomeController.cs** e aggiungere il metodo seguente alla classe. Questo metodo restituisce il **Chat** visualizzazione che verrà creato in un passaggio successivo.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Fare doppio clic su di **Views/Home** cartella e selezionare **Aggiungi.... | Visualizzazione**.
13. Nel **Aggiungi visualizzazione** finestra di dialogo, nome, la nuova vista **Chat**.

    ![Aggiungere una vista](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Sostituire il contenuto di **Chat.cshtml** con il codice seguente.

    > [!IMPORTANT]
    > Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, la gestione dei pacchetti potrebbe installare una versione del file di script SignalR più recenti rispetto a quella illustrata in questo argomento. Assicurarsi che il riferimento allo script nel codice corrisponda alla versione della libreria di script installata nel progetto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Salvare tutti** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug.
2. Nella riga dell'indirizzo del browser, aggiungere **/home/chat** all'URL della pagina predefinita per il progetto. Caricamento della pagina di Chat in un'istanza del browser e richiede un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Immettere un nome utente.
4. Copiare l'URL dalla riga dell'indirizzo del browser e utilizzarla per aprire le due altre istanze del browser. In ogni istanza del browser, immettere un nome utente univoco.
5. In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**. I commenti deve essere visualizzato in tutte le istanze del browser.

    > [!NOTE]
    > Questa applicazione di una chat semplice non gestisce il contesto della discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Partecipare alla chat in un secondo momento gli utenti visualizzeranno messaggi aggiunti dal momento in cui che si connettono.
6. La schermata seguente viene illustrata l'applicazione di chat in esecuzione in un browser.

    ![Browser chat](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. In **Esplora**, controllare il **documenti Script** nodo per l'applicazione in esecuzione. Questo nodo viene visualizzato in modalità di debug, se si utilizza Internet Explorer come browser. È presente un file script denominato **hub** che la libreria SignalR genera in modo dinamico in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server. Se si utilizza un browser diverso da Internet Explorer, è inoltre possibile accedere dinamica **hub** file passando ad esso direttamente, ad esempio http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione di chat SignalR illustra due attività di sviluppo SignalR base: creazione di un hub come oggetto di coordinamento principale sul server e utilizza la libreria di jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub di SignalR

Nell'esempio di codice il **ChatHub** deriva dalla classe di **Microsoft.AspNet.SignalR.Hub** classe. Derivazione da di **Hub** classe è utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe hub e quindi accedere a tali metodi mediante chiamata dagli script in una pagina web.

Nel codice chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio. L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.addNewMessageToPage**.

Il **inviare** metodo illustra i diversi concetti di hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possono essere richiamati.
- Utilizzare il **Microsoft.AspNet.SignalR.Hub.Clients** proprietà per accedere a tutti i client connessi a questo hub.
- Chiamare una funzione nel client (ad esempio il `addNewMessageToPage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

Il **Chat.cshtml** Visualizza file nell'esempio di codice viene illustrato come utilizzare la libreria jQuery SignalR per comunicare con un hub di SignalR. Attività essenziali nel codice crea un riferimento per il proxy generato automaticamente per l'hub, dichiarazione di una funzione che il server è possibile chiamare per contenuto push ai client e all'avvio di una connessione per inviare messaggi all'hub.

Il codice seguente dichiara un riferimento a un proxy dell'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> In JavaScript, il riferimento alla classe del server e i relativi membri è in maiuscole-minuscole camel. L'esempio di codice fa riferimento a c# **ChatHub** classe in JavaScript come **chatHub**. Se si desidera fare riferimento il `ChatHub` classe in jQuery con Pascal convenzionale maiuscole e minuscole come si farebbe in c#, modificare il file di classe ChatHub.cs. Aggiungere un `using` istruzione per fare riferimento il `Microsoft.AspNet.SignalR.Hubs` dello spazio dei nomi. Aggiungere quindi il `HubName` attributo la `ChatHub` classe, ad esempio `[HubName("ChatHub")]`. Infine, aggiornare il riferimento jQuery la `ChatHub` classe.


Il codice seguente viene illustrato come creare una funzione di callback nello script. Classe dell'hub sul server chiama questa funzione per inviare gli aggiornamenti del contenuto di ogni client. La chiamata a facoltativo di `htmlEncode` funzione Mostra un modo per HTML codifica il contenuto del messaggio prima di visualizzarlo nella pagina, come un modo per impedire attacchi script injection.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Il codice seguente viene illustrato come aprire una connessione con l'hub. Il codice avvia la connessione e passa a una funzione per gestire l'evento click nel **inviare** nella pagina Chat.

> [!NOTE]
> Questo approccio assicura che la connessione viene stabilita prima dell'esecuzione il gestore dell'evento.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale. È stato inoltre diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.

Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR in Azure, vedere [utilizzando SignalR con le app Web in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Per informazioni dettagliate su come distribuire un progetto web di Visual Studio a un sito Web di Windows Azure, vedere [creare un'app web ASP.NET in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Per informazioni su concetti più avanzati di sviluppi SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
