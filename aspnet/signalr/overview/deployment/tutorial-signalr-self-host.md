---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Esercitazione: Hosting indipendente di SignalR | Microsoft Docs'
author: pfletcher
description: Questa esercitazione illustra come creare un server di SignalR 2 self-hosted e come connettersi a esso con un client JavaScript. Versioni del software utilizzate nell'esercitazione V...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 5d7d485357a6c820f11e0135e2ff9479c1965d96
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829588"
---
<a name="tutorial-signalr-self-host"></a>Esercitazione: Hosting indipendente di SignalR
====================
da [Patrick Fletcher](https://github.com/pfletcher)

[Download progetto completato](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Questa esercitazione illustra come creare un server di SignalR 2 self-hosted e come connettersi a esso con un client JavaScript.
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
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

Un server di SignalR è generalmente ospitato in un'applicazione ASP.NET in IIS, ma può anche essere self-hosted (ad esempio un'applicazione console o il servizio Windows) usando la libreria self-hosting. Questa libreria, quali tutto SignalR 2, si basa su OWIN ([Open Web Interface for .NET](http://owin.org)). OWIN definisce un'astrazione tra i server web .NET e applicazioni web. OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN.

I motivi per non effettua l'hosting in IIS includono:

- Ambienti in cui IIS non è disponibile o utile, ad esempio una server farm esistente senza IIS.
- L'overhead delle prestazioni di IIS deve essere evitata.
- SignalR funzionalità deve essere aggiunto a un'applicazione esistente che viene eseguito in un servizio di Windows, ruolo di lavoro di Azure o altri processi.

Se una soluzione è in fase di sviluppo come self-hosting per motivi di prestazioni, è consigliabile anche test l'applicazione ospitata in IIS per determinare il miglioramento delle prestazioni.

Questa esercitazione include le sezioni seguenti:

- [Creazione del server](#server)
- [L'accesso al server con un client JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Creazione del server

In questa esercitazione si creerà un server in cui è ospitato in un'applicazione console, ma il server può essere ospitato in qualsiasi tipo di processo, ad esempio un servizio di Windows o un ruolo di lavoro di Azure. Per codice di esempio per l'hosting di un server di SignalR in un servizio di Windows, visitare [Self-Hosting SignalR in un servizio Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Aprire Visual Studio 2013 con privilegi di amministratore. Selezionare **File**, **nuovo progetto**. Selezionare **Windows** sotto il **Visual c#** nodo il **modelli** riquadro e selezionare il **applicazione Console** modello. Denominare il nuovo progetto "SignalRSelfHost" e fare clic su **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Aprire la console di gestione pacchetti libreria selezionando **degli strumenti**, **Library Package Manager**, **Package Manager Console**.
3. Nella console di gestione pacchetti immettere il comando seguente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Questo comando aggiunge le librerie di SignalR 2 al progetto.
4. Nella console di gestione pacchetti immettere il comando seguente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Questo comando aggiunge la libreria owin al progetto. Questa libreria verrà usata per il supporto di domini, che è necessario per le applicazioni che ospitano SignalR e un client della pagina web in domini diversi. Poiché si sarà ospita il server di SignalR e il client web su porte diverse, ciò significa che tra domini devono essere abilitato per la comunicazione tra questi componenti.
5. Sostituire il contenuto di Program.cs con il codice seguente.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Il codice riportato sopra include le tre classi seguenti:

    - **Programma**, tra cui la **Main** metodo che definisce il percorso principale di esecuzione. In questo metodo, un'applicazione web di tipo **avvio** viene avviato all'URL specificato (`http://localhost:8080`). Se sicurezza è obbligatorio per l'endpoint, è possibile implementare SSL. Visualizzare [procedura: configurare una porta con un certificato SSL](https://msdn.microsoft.com/library/ms733791.aspx) per altre informazioni.
    - **Avvio**, la classe che contiene la configurazione per il server di SignalR (l'unica configurazione di questa esercitazione viene usato è la chiamata a `UseCors`) e la chiamata a `MapSignalR`, che consente di creare le route per gli oggetti dell'Hub nel progetto.
    - **MyHub**, la classe SignalR Hub che l'applicazione dovrà fornire ai client. Questa classe ha un solo metodo, **inviare**, che i client chiameranno per trasmettere un messaggio a tutti gli altri client connessi.
6. Compilare l'applicazione ed eseguirla. Nella finestra della console dovrebbe visualizzare l'indirizzo a cui il server è in esecuzione.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Se l'esecuzione ha esito negativo con l'eccezione `System.Reflection.TargetInvocationException was unhandled`, sarà necessario riavviare Visual Studio con privilegi di amministratore.
8. Arrestare l'applicazione prima di procedere alla sezione successiva.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>L'accesso al server con un client JavaScript

In questa sezione si userà lo stesso client JavaScript dal [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md). Ci assicureremo solo una modifica al client, che consiste nel definire in modo esplicito l'URL dell'hub. Con un'applicazione self-hosted, il server potrebbe non corrispondere esattamente allo stesso indirizzo come URL della connessione (a causa di un proxy inverso e i bilanciamenti del carico), pertanto, l'URL deve essere definito in modo esplicito.

1. Nelle **Esplora soluzioni**, fare doppio clic sulla soluzione e selezionare **Add**, **nuovo progetto**. Selezionare il **Web** nodo e selezionare il **applicazione Web ASP.NET** modello. Denominare il progetto "JavascriptClient" e fare clic su **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Selezionare il **vuoto** modello e lasciare le altre opzioni deselezionate. Selezionare **Crea progetto**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. Nella console di gestione pacchetti, selezionare il progetto "JavascriptClient" nel **progetto predefinito** elenco a discesa ed eseguire il comando seguente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Questo comando Installa le librerie di SignalR e JQuery che saranno necessari nel client.
4. Pulsante destro del mouse sul progetto, quindi scegliere **Add**, **nuovo elemento**. Selezionare il **Web** nodo e selezionare pagina HTML. Denominare la pagina **default. HTML**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Sostituire il contenuto della nuova pagina HTML con il codice seguente. Verificare che i riferimenti a script corrispondano gli script nella cartella Scripts del progetto.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Il codice seguente (evidenziato nell'esempio di codice precedente) è l'aggiunta che sono state apportate al client usato nell'esercitazione di introduzione all'uso (oltre ad aggiornare il codice a SignalR 2 versione beta). Questa riga di codice imposta in modo esplicito l'URL di connessione di base per SignalR sul server.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Pulsante destro del mouse sulla soluzione e selezionare **Imposta progetti di avvio...** . Selezionare il **progetti di avvio multipli** pulsante di opzione e impostare entrambi i progetti **azione** al **avviare**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Fare clic su "Default. HTML" e selezionare **imposta come pagina iniziale**.
8. Eseguire l'applicazione. Verrà avviata la pagina e il server. Potrebbe essere necessario ricaricare la pagina web (o selezionare **continuazione** nel debugger) se la pagina viene caricata prima che il server è stato avviato.
9. Nel browser, fornire un nome utente quando richiesto. Copiare l'URL della pagina in un'altra scheda del browser o finestra e fornire un nome utente diverso. Sarà in grado di inviare messaggi dal riquadro uno visualizzatore a altro, come nell'esercitazione introduttiva.
