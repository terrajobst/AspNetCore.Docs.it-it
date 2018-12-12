---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Esercitazione: Introduzione a SignalR 2 e MVC 5 | Microsoft Docs'
author: pfletcher
description: Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. Si verrà aggiunto SignalR a un'applicazione MVC 5 e creare una vista di chat...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 568f82daa67f33736c2bf7a45a3e1339f265c487
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287514"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Esercitazione: Introduzione a SignalR 2 e MVC 5
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. Verrà aggiunta SignalR a un'applicazione MVC 5 e creare una vista di chat per inviare e visualizzare i messaggi.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - MVC 5
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

Questa esercitazione viene illustrato lo sviluppo di applicazioni web in tempo reale con ASP.NET SignalR 2 e ASP.NET MVC 5. L'esercitazione Usa lo stesso codice dell'applicazione di chat come le [esercitazione di introduzione a SignalR](tutorial-getting-started-with-signalr.md), ma illustra come aggiungerlo a un'applicazione MVC 5.

In questo argomento verranno fornite le seguenti attività di sviluppo SignalR:

- Aggiunta della libreria di SignalR a un'applicazione MVC 5.
- Creazione dell'hub e le classi di avvio OWIN per eseguire il push del contenuto ai client.
- Usa la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.

Lo screenshot seguente mostra l'applicazione di chat completo in esecuzione in un browser.

![Istanze di chat](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Nelle sezioni:

- [Configurare il progetto](#setup)
- [Eseguire l'esempio](#run)
- [Esaminare il codice](#code)
- [Passaggi successivi](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurare il progetto

Prerequisiti:

- Visual Studio 2013. Se non hai Visual Studio, vedere [download ASP.NET](https://www.asp.net/downloads) per ottenere il Visual Studio 2013 Express strumento di sviluppo gratuito.

In questa sezione viene illustrato come creare un'applicazione ASP.NET MVC 5, aggiungere la libreria di SignalR e creare l'applicazione di chat.

1. In Visual Studio, creare un'applicazione c# ASP.NET destinata a .NET Framework 4.5, denominarlo SignalRChat e fare clic su OK.

    ![Creazione di web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. Nel `New ASP.NET Project` finestra di dialogo e selezionare **MVC**, fare clic su **Modifica autenticazione**.

    ![Creazione di web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Selezionare **Nessuna autenticazione** nel **Modifica autenticazione** finestra di dialogo e fare clic su **OK**.

    ![Seleziona Nessuna autenticazione](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Se si seleziona un provider di autenticazione diversi per l'applicazione, un `Startup.cs` classe verrà creata automaticamente; non è necessario creare il proprio `Startup.cs` classe nel passaggio 10 seguente.
4. Fare clic su **OK** nel **nuovo progetto ASP.NET** finestra di dialogo.
5. Apri il **strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti** ed eseguire il comando riportato di seguito. Questo passaggio viene aggiunta al progetto un set di file di script e i riferimenti ad assembly che abilitano funzionalità SignalR.

    `install-package Microsoft.AspNet.SignalR`
6. Nelle **Esplora soluzioni**, espandere la cartella degli script. Si noti che le librerie di script per SignalR siano stati aggiunti al progetto.

    ![Cartella degli script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. Nelle **Esplora soluzioni**, fare clic sul progetto, selezionare **Add | Nuova cartella**, e aggiungere una nuova cartella denominata **hub**.
8. Fare doppio clic il **hub** cartella, fare clic su **Add | Nuovo elemento**, selezionare il **Visual c# | Web | SignalR** nodo il **Installed** riquadro, selezionare **classe tramite SignalR Hub (v2)** nel riquadro centrale e creare un nuovo hub denominato **ChatHub.cs**. Si userà questa classe come hub SignalR server che invia messaggi a tutti i client.

    ![Crea nuovo hub](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Sostituire il codice nel **ChatHub** classe con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Creare una nuova classe denominata Startup.cs. Modificare il contenuto del file come segue.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Modificare il `HomeController` trovata nella classe **HomeController** e aggiungere il metodo seguente alla classe. Questo metodo restituisce il **Chat** vista in cui verrà creato in un passaggio successivo.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Fare doppio clic il **Views/Home** cartella e selezionare **Aggiungi... | Visualizzazione**.
13. Nel **Aggiungi visualizzazione** finestra di dialogo nome alla nuova visualizzazione **Chat**.

    ![Aggiungere una vista](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Sostituire il contenuto del **Chat.cshtml** con il codice seguente.

    > [!IMPORTANT]
    > Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, la gestione dei pacchetti potrebbe installare una versione del file script SignalR più recenti rispetto a quella illustrata in questo argomento. Assicurarsi che il riferimento allo script nel codice corrisponda alla versione della libreria di script installata nel progetto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Salva tutto** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug.
2. Nella riga dell'indirizzo del browser, accodare **/home/chat** all'URL della pagina predefinita per il progetto. La pagina di Chat viene caricata in un'istanza del browser e richiede un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Immettere un nome utente.
4. Copiare l'URL dalla riga dell'indirizzo del browser e usarlo per aprire due altre istanze del browser. In ogni istanza del browser immettere un nome utente univoco.
5. In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**. I commenti deve essere visualizzato in tutte le istanze del browser.

    > [!NOTE]
    > Questa applicazione di chat semplice non gestisce il contesto di discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.
6. Lo screenshot seguente mostra l'applicazione di chat in esecuzione in un browser.

    ![Browser di chat](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione. Se si usa Internet Explorer come browser, questo nodo è visibile in modalità di debug. È presente un file di script denominato **hub** che la libreria di SignalR genera in modo dinamico in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server. Se si utilizza un browser diverso da Internet Explorer, è anche possibile accedere dinamica **hub** file passando ad esso direttamente, ad esempio http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione di chat SignalR illustra due attività di sviluppo di SignalR base: creazione di un hub come oggetto di coordinamento principale nel server e usando la libreria jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub SignalR

Nell'esempio di codice le **ChatHub** deriva dalla classe la **Microsoft.AspNet.SignalR.Hub** classe. Derivazione dal **Hub** classe è un modo utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe dell'hub e quindi accedere a tali metodi chiamandole dagli script in una pagina web.

Nel codice di chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio. L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.addNewMessageToPage**.

Il **inviare** metodo illustra alcuni concetti di hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.
- Usare la **Microsoft.AspNet.SignalR.Hub.Clients** proprietà a cui accedere tutti i client connessi a questo hub.
- Chiamare una funzione nel client (ad esempio il `addNewMessageToPage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

Il **Chat.cshtml** Visualizza file nell'esempio di codice illustra come usare la libreria jQuery SignalR per comunicare con un hub SignalR. Le attività di base nel codice siano creando un riferimento per il proxy generato automaticamente per l'hub, dichiara una funzione che il server può chiamare per contenuto push ai client e l'avvio di una connessione per inviare messaggi all'hub.

Il codice seguente dichiara un riferimento a un proxy di hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> In JavaScript il riferimento alla classe server e i relativi membri è in maiuscole/minuscole camel. Nell'esempio di codice fa riferimento il codice c# **ChatHub** classi in JavaScript come **chatHub**. Se si desidera fare riferimento il `ChatHub` classe in jQuery con convenzionale convenzione Pascal maiuscole e minuscole come si farebbe in c#, modificare il file di classe ChatHub.cs. Aggiungere un `using` istruzione a cui fare riferimento il `Microsoft.AspNet.SignalR.Hubs` dello spazio dei nomi. Quindi aggiungere il `HubName` dell'attributo per il `ChatHub` classe, ad esempio `[HubName("ChatHub")]`. Infine, aggiornare il riferimento jQuery il `ChatHub` classe.


Il codice seguente viene illustrato come creare una funzione di callback nello script. La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client. La chiamata facoltativa al `htmlEncode` funzione Mostra un modo per HTML codifica il contenuto del messaggio prima di visualizzarlo nella pagina, come un modo per impedire attacchi script injection.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Il codice seguente viene illustrato come aprire una connessione con l'hub. Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina della Chat.

> [!NOTE]
> Questo approccio assicura che la connessione viene stabilita prima esecuzione del gestore dell'evento.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale. Si è appreso anche diverse attività di sviluppo di SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.

Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR in Azure, vedere [uso di SignalR con App Web nel servizio App di Azure](../deployment/using-signalr-with-azure-web-sites.md). Per informazioni dettagliate su come distribuire un progetto web Visual Studio per un sito Web di Microsoft Azure, vedere [creare un'app web ASP.NET in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Per informazioni su concetti più avanzati sviluppi in materia di SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:

- [Progetto di SignalR](http://signalr.net)
- [SignalR Github ed esempi](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
