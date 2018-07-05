---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Esercitazione: Introduzione a SignalR 1.x e MVC 4 | Microsoft Docs'
author: pfletcher
description: Usare SignalR ASP.NET e ASP.NET MVC 4 per creare un'applicazione di chat in tempo reale.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: cced40f65700eff073aa4cff4560a137a50bbc5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399962"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Esercitazione: Introduzione a SignalR 1.x e MVC 4
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Questa esercitazione illustra come usare SignalR ASP.NET per creare un'applicazione di chat in tempo reale. Verrà aggiunta SignalR a un'applicazione MVC 4 e creare una vista di chat per inviare e visualizzare i messaggi.


## <a name="overview"></a>Panoramica

Questa esercitazione viene illustrato lo sviluppo di applicazioni web in tempo reale con ASP.NET SignalR e ASP.NET MVC 4. L'esercitazione Usa lo stesso codice dell'applicazione di chat come le [esercitazione di introduzione a SignalR](tutorial-getting-started-with-signalr.md), ma illustra come aggiungerlo a un'applicazione MVC 4 in base al modello di Internet.

In questo argomento verranno fornite le seguenti attività di sviluppo SignalR:

- Aggiunta della libreria di SignalR a un'applicazione MVC 4.
- Creazione di una classe di hub per eseguire il push del contenuto ai client.
- Usa la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.

Lo screenshot seguente mostra l'applicazione di chat completo in esecuzione in un browser.

![Istanze di chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Nelle sezioni:

- [Configurare il progetto](#setup)
- [Eseguire l'esempio](#run)
- [Esaminare il codice](#code)
- [Passaggi successivi](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurare il progetto

Prerequisiti:

- Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio 2012 Express. Se non hai Visual Studio, vedere [download ASP.NET](https://www.asp.net/downloads) per ottenere il Visual Studio 2012 Express strumento di sviluppo gratuito.
- Per Visual Studio 2010, installare [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

In questa sezione viene illustrato come creare un'applicazione ASP.NET MVC 4, aggiungere la libreria di SignalR e creare l'applicazione di chat.

1. 1. In Visual Studio creare un'applicazione ASP.NET MVC 4, denominarlo SignalRChat e fare clic su OK.

        > [!NOTE]
        > In Visual Studio 2010, selezionare **.NET Framework 4** nel controllo elenco a discesa della versione di Framework. SignalR codice viene eseguito nelle versioni di .NET Framework 4 e 4.5.

        ![Creare web mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Selezionare il modello di applicazione Internet, deselezionare l'opzione **creare un progetto di unit test**, fare clic su OK.

         ![Creare siti internet mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Aprire il **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguire il comando seguente. Questo passaggio viene aggiunta al progetto un set di file di script e i riferimenti ad assembly che abilitano funzionalità SignalR.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. Nelle **Esplora soluzioni** espandere la cartella degli script. Si noti che le librerie di script per SignalR siano stati aggiunti al progetto.

         ![Riferimenti alla libreria](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. Nelle **Esplora soluzioni**, fare clic sul progetto, selezionare **Add | Nuova cartella**, e aggiungere una nuova cartella denominata **hub**.
      6. Fare doppio clic il **hub** cartella, fare clic su **Add | Classe**e creare una nuova classe c# denominata **ChatHub.cs**. Si userà questa classe come hub SignalR server che invia messaggi a tutti i client.

> [!NOTE]
> Se si usa Visual Studio 2012 e aver installato il [aggiornamento ASP.NET and Web Tools 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), è possibile usare il nuovo modello di elemento di SignalR per creare la classe dell'hub. A tale scopo, fare doppio clic il **hub** cartella, fare clic su **Add | Nuovo elemento**, selezionare **classe Hub SignalR (v1)** e denominare la classe **ChatHub.cs**.


1. Sostituire il codice nel **ChatHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Aprire il **Global. asax** file per il progetto, quindi aggiungere una chiamata al metodo `RouteTable.Routes.MapHubs();` come prima riga di codice nel `Application_Start` (metodo). Questo codice registra la route predefinita per SignalR hubs e deve essere chiamato prima di registrare qualsiasi altra route. Completato `Application_Start` metodo simile all'esempio seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Modificare il `HomeController` trovata nella classe **HomeController** e aggiungere il metodo seguente alla classe. Questo metodo restituisce il **Chat** vista in cui verrà creato in un passaggio successivo.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Pulsante destro del mouse all'interno di `Chat` metodo appena creato e fare clic su **Aggiungi visualizzazione** per creare un nuovo file di visualizzazione.
5. Nel **Aggiungi visualizzazione** finestra di dialogo, verificare che la casella di controllo è selezionata per **usano una pagina master o layout** (deselezionare le altre caselle di controllo) e quindi fare clic su **Aggiungi**.

    ![Aggiungere una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Modificare il nuovo file di visualizzazione denominato **Chat.cshtml**. Dopo il &lt;h2&gt; tag, incollare il codice seguente &lt;div&gt; sezione e `@section scripts` blocco di codice nella pagina. Questo script consente alla pagina inviare i messaggi di chat e visualizzare i messaggi dal server. Il codice completo per la visualizzazione di chat viene visualizzato nel blocco di codice seguente.

    > [!IMPORTANT]
    > Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, la gestione dei pacchetti potrebbe installare le versioni degli script che sono più recenti rispetto alle versioni illustrate in questo argomento. Assicurarsi che i riferimenti a script nel codice corrispondono alle versioni delle librerie di script installate nel progetto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Salva tutto** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug.
2. Nella riga dell'indirizzo del browser, accodare **/home/chat** all'URL della pagina predefinita per il progetto. La pagina di Chat viene caricata in un'istanza del browser e richiede un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Immettere un nome utente.
4. Copiare l'URL dalla riga dell'indirizzo del browser e usarlo per aprire due altre istanze del browser. In ogni istanza del browser immettere un nome utente univoco.
5. In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**. I commenti deve essere visualizzato in tutte le istanze del browser.

    > [!NOTE]
    > Questa applicazione di chat semplice non gestisce il contesto di discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.
6. Lo screenshot seguente mostra l'applicazione di chat in esecuzione in un browser.

    ![Browser di chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione. Se si usa Internet Explorer come browser, questo nodo è visibile in modalità di debug. È presente un file di script denominato **hub** che la libreria di SignalR genera in modo dinamico in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server. Se si utilizza un browser diverso da Internet Explorer, è anche possibile accedere dinamica **hub** file passando ad esso direttamente, ad esempio http://mywebsite/signalr/hubs.

    ![Script generato hub](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione di chat SignalR illustra due attività di sviluppo di SignalR base: creazione di un hub come oggetto di coordinamento principale nel server e usando la libreria jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub SignalR

Nell'esempio di codice le **ChatHub** deriva dalla classe la **Microsoft.AspNet.SignalR.Hub** classe. Derivazione dal **Hub** classe è un modo utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe dell'hub e quindi accedere a tali metodi chiamandole dagli script jQuery in una pagina web.

Nel codice di chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio. L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.addNewMessageToPage**.

Il **inviare** metodo illustra alcuni concetti di hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.
- Usare la **Microsoft.AspNet.SignalR.Hub.Clients** proprietà a cui accedere tutti i client connessi a questo hub.
- Chiamare una funzione di jQuery nel client (ad esempio il `addNewMessageToPage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

Il **Chat.cshtml** Visualizza file nell'esempio di codice illustra come usare la libreria jQuery SignalR per comunicare con un hub SignalR. Le attività di base nel codice siano creando un riferimento per il proxy generato automaticamente per l'hub, dichiara una funzione che il server può chiamare per contenuto push ai client e l'avvio di una connessione per inviare messaggi all'hub.

Il codice seguente dichiara un proxy per un hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> In jQuery il riferimento alla classe server e i relativi membri è in maiuscole/minuscole camel. Nell'esempio di codice fa riferimento il codice c# **ChatHub** classe in jQuery come **chatHub**. Se si desidera fare riferimento il `ChatHub` classe in jQuery con convenzionale convenzione Pascal maiuscole e minuscole come si farebbe in c#, modificare il file di classe ChatHub.cs. Aggiungere un `using` istruzione a cui fare riferimento il `Microsoft.AspNet.SignalR.Hubs` dello spazio dei nomi. Quindi aggiungere il `HubName` dell'attributo per il `ChatHub` classe, ad esempio `[HubName("ChatHub")]`. Infine, aggiornare il riferimento jQuery il `ChatHub` classe.


Il codice seguente viene illustrato come creare una funzione di callback nello script. La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client. La chiamata facoltativa al `htmlEncode` funzione Mostra un modo per HTML codifica il contenuto del messaggio prima di visualizzarlo nella pagina, come un modo per impedire attacchi script injection.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Il codice seguente viene illustrato come aprire una connessione con l'hub. Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina della Chat.

> [!NOTE]
> Questo approccio assicura che la connessione viene stabilita prima esecuzione del gestore dell'evento.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale. Si è appreso anche diverse attività di sviluppo di SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.

Per informazioni su concetti più avanzati sviluppi in materia di SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto di SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
