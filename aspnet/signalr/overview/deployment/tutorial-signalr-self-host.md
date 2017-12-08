---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Esercitazione: SignalR indipendente | Documenti Microsoft'
author: pfletcher
description: Questa esercitazione viene illustrato come creare un server di SignalR 2 indipendente e come connettersi con un client JavaScript. Versioni del software utilizzate nell'esercitazione V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 997756ff8d48e41da981491d6154f3107ec7a051
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-signalr-self-host"></a>Esercitazione: SignalR Host indipendente
====================
da [Patrick Fletcher](https://github.com/pfletcher)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Questa esercitazione viene illustrato come creare un server di SignalR 2 indipendente e come connettersi con un client JavaScript.
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
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

Un server di SignalR viene solitamente ospitato in un'applicazione ASP.NET in IIS, ma può anche essere indipendente (ad esempio un'applicazione console o il servizio Windows) utilizzando la libreria di host indipendente. Questa libreria, come tutte SignalR 2, si basa su OWIN ([aprire interfaccia Web per .NET](http://owin.org)). OWIN definisce un'astrazione tra i server web .NET e applicazioni web. OWIN disaccoppia l'applicazione web dal server, che rende ideale per l'hosting automatico di un'applicazione web in un processo personalizzato, all'esterno di IIS OWIN.

I motivi per cui non è ospitato in IIS includono:

- Ambienti in cui IIS non è disponibile o, ad esempio una server farm esistente senza IIS.
- L'overhead delle prestazioni di IIS deve essere evitato.
- Funzionalità di SignalR è da aggiungere a un'applicazione esistente che viene eseguito in un servizio Windows, il ruolo di lavoro di Azure o un altro processo.

Se una soluzione di sviluppo come servizio indipendente per motivi di prestazioni, si consiglia di test anche l'applicazione ospitata in IIS per determinare il miglioramento delle prestazioni.

In questa esercitazione include le sezioni seguenti:

- [La creazione del server](#server)
- [L'accesso al server con un client JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>La creazione del server

In questa esercitazione si creerà un server in cui è ospitato in un'applicazione console, ma il server può essere ospitato in un qualsiasi tipo di processo, ad esempio un servizio Windows o un ruolo di lavoro di Azure. Per il codice di esempio per l'hosting di un server di SignalR in un servizio Windows, vedere [Self-Hosting SignalR in un servizio Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Aprire Visual Studio 2013 con privilegi di amministratore. Selezionare **File**, **nuovo progetto**. Selezionare **Windows** sotto il **Visual c#** nodo il **modelli** riquadro e selezionare il **applicazione Console** modello. Denominare il nuovo progetto "SignalRSelfHost" e fare clic su **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Aprire la console di gestione pacchetti libreria selezionando **strumenti**, **Gestione pacchetti libreria**, **Package Manager Console**.
3. Nella console di gestione di pacchetto, immettere il comando seguente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Questo comando aggiunge le librerie di SignalR 2 Self-Host al progetto.
4. Nella console di gestione di pacchetto, immettere il comando seguente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Questo comando aggiunge la libreria owin al progetto. La libreria da utilizzare per il supporto di domini, è necessario per le applicazioni che ospitano SignalR e un client di pagina web in domini diversi. Poiché si sarà ospita il server di SignalR e il client web su porte diverse, ciò significa che tra domini devono essere abilitato per la comunicazione tra questi componenti.
5. Sostituire il contenuto di Program.cs con il codice seguente.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Il codice sopra riportato include tre classi:

    - **Programma**, tra cui la **Main** metodo che definisce il percorso principale di esecuzione. In questo metodo, un'applicazione web di tipo **avvio** viene avviato all'URL specificato (`http://localhost:8080`). Se è richiesta la protezione per l'endpoint, è possibile implementare SSL. Vedere [procedura: configurare una porta con un certificato SSL](https://msdn.microsoft.com/en-us/library/ms733791.aspx) per ulteriori informazioni.
    - **Avvio**, la classe che contiene la configurazione per il server di SignalR (la sola configurazione di questa esercitazione è la chiamata a `UseCors`) e la chiamata a `MapSignalR`, che consente di creare le route per tutti gli oggetti Hub nel progetto.
    - **MyHub**, la classe SignalR Hub che l'applicazione fornisce ai client. Questa classe contiene un solo metodo, **inviare**, che i client chiameranno per trasmettere un messaggio a tutti gli altri client connessi.
6. Compilare l'applicazione ed eseguirla. L'indirizzo a cui è in esecuzione il server deve visualizzare in una finestra della console.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Se si verifica un errore di esecuzione con l'eccezione `System.Reflection.TargetInvocationException was unhandled`, è necessario riavviare Visual Studio con privilegi di amministratore.
8. Arrestare l'applicazione prima di procedere alla sezione successiva.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>L'accesso al server con un client JavaScript

In questa sezione, si utilizzerà lo stesso client JavaScript dal [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md). Verrà effettuata solo una modifica al client, che consiste nel definire in modo esplicito l'URL di hub. Con un'applicazione indipendente, il server potrebbe non necessariamente essere allo stesso indirizzo come l'URL di connessione (a causa di proxy inverso e i servizi di bilanciamento del carico), pertanto l'URL deve essere definito in modo esplicito.

1. In **Esplora**del mouse sulla soluzione e scegliere **Aggiungi**, **nuovo progetto**. Selezionare il **Web** nodo e selezionare il **applicazione Web ASP.NET** modello. Denominare il progetto "JavascriptClient" e fare clic su **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Selezionare il **vuoto** modello e lasciare deselezionate le opzioni rimanenti. Selezionare **creare progetto**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. Nella console di gestione di pacchetto, selezionare il progetto "JavascriptClient" nel **progetto predefinito** elenco a discesa ed eseguire il comando seguente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Questo comando Installa le librerie di SignalR e JQuery è necessario nel client.
4. Pulsante destro del mouse sul progetto e selezionare **Aggiungi**, **nuovo elemento**. Selezionare il **Web** nodo e selezionare una pagina HTML. Denominare la pagina **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Sostituire il contenuto della nuova pagina HTML con il codice seguente. Verificare che i riferimenti a script corrispondano gli script nella cartella script del progetto.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Il codice seguente (evidenziato nell'esempio di codice precedente) è l'aggiunta che sono state apportate al client utilizzato nell'esercitazione recupero scoraggiato (oltre ad aggiornare il codice per SignalR versione 2 beta). Questa riga di codice imposta in modo esplicito l'URL di connessione di base per SignalR sul server.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Pulsante destro del mouse sulla soluzione e selezionare **Imposta progetti di avvio...** . Selezionare il **più progetti di avvio** pulsante di opzione e impostare entrambi i progetti **azione** a **avviare**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Fare clic su "Default" e selezionare **imposta come pagina iniziale**.
8. Eseguire l'applicazione. Verrà avviato il server e pagina. Potrebbe essere necessario ricaricare la pagina web (o selezionare **continua** nel debugger) se il caricamento della pagina prima dell'avvio del server.
9. Nel browser, fornire un nome utente quando richiesto. Copiare l'URL della pagina in un'altra scheda del browser o una finestra e fornire un nome utente diverso. Sarà in grado di inviare messaggi dal riquadro uno visualizzatore a altro, come illustrato nell'esercitazione Introduzione.
