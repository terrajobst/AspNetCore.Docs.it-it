---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Esercitazione: Chat in tempo reale con SignalR 2 | Microsoft Docs'
author: bradygaster
description: In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. SignalR è aggiungere a un'applicazione web ASP.NET vuota.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 90f2c03fbda522e3a46200bc0132cc74100ce70f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836792"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Esercitazione: Chat in tempo reale con SignalR 2

Questa esercitazione illustra come usare SignalR per creare un'applicazione di chat in tempo reale. Aggiungere SignalR a un'applicazione web ASP.NET vuota e creare una pagina HTML per l'invio e visualizzare i messaggi.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il progetto
> * Eseguire l'esempio
> * Esaminare il codice

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il **sviluppo ASP.NET e web** carico di lavoro.

## <a name="set-up-the-project"></a>Configurare il progetto

Questa sezione illustra come usare SignalR 2 e Visual Studio 2017 per creare un'applicazione web ASP.NET vuota, aggiungere SignalR e creare l'applicazione di chat.

1. In Visual Studio, creare un'applicazione Web ASP.NET.

    ![Creazione di web](tutorial-getting-started-with-signalr/_static/image2.png)

1. Nel **nuovo progetto ASP.NET - SignalRChat** finestra, lasciare **vuota** selezionato e selezionare **OK**.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.

1. Nelle **Aggiungi nuovo elemento - SignalRChat**, selezionare **installati** > **Visual C#**   >  **Web**  >  **SignalR** e quindi selezionare **classe tramite SignalR Hub (v2)**.

1. Denominare la classe *ChatHub* e aggiungerlo al progetto.

    Questo passaggio Crea il *ChatHub.cs* file di classe e aggiunge un set di file di script e i riferimenti ad assembly che supportano SignalR al progetto.

1. Sostituire il codice nella nuova *ChatHub.cs* file di classe con il seguente codice:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.

1. Nelle **Aggiungi nuovo elemento - SignalRChat** selezionate **installati** > **Visual C#**   >  **Web** e quindi Selezionare **classe di avvio OWIN**.

1. Denominare la classe *avvio* e aggiungerlo al progetto.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **pagina HTML**.

1. Denominare la nuova pagina *indice* e selezionare **OK**.

1. Nelle **Esplora soluzioni**, la pagina HTML creata e scegliere **imposta come pagina iniziale**.

1. Sostituire il codice predefinito nella pagina HTML con il seguente codice:

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. Nelle **Esplora soluzioni**, espandere **script**.

    Librerie di script per jQuery e SignalR sono visibili nel progetto.

    > [!IMPORTANT]
    > La gestione dei pacchetti potrebbe avere installato una versione successiva degli script di SignalR.

1. Verificare che i riferimenti a script nel blocco di codice corrispondono alle versioni dei file di script nel progetto.

    Riferimenti a script dal blocco di codice originale:
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Se non corrispondono, aggiornare il *HTML* file.

1. Dalla barra dei menu, selezionare **File** > **Salva tutto**.

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Nella barra degli strumenti, attivare **debug degli Script** e quindi selezionare il pulsante play per eseguire l'esempio in modalità di Debug.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image3.png)

1. Quando si apre il browser, immettere un nome per l'identità di chat.

1. Copiare l'URL dal browser, aprire due altri browser e incollare l'URL nella barra degli indirizzi.

1. In ogni browser e immettere un nome univoco.

1. A questo punto, aggiungere un commento e selezionare **inviare**. Ripetere che in altri browser. I commenti vengono visualizzati in tempo reale.

    > [!NOTE]
    > Questa applicazione di chat semplice non gestisce il contesto di discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.

    Vedere come l'applicazione di chat viene eseguita in tre diversi browser. Per Susan, Anand e Tom invio di messaggi, aggiornare tutti i browser in tempo reale:

    ![Tutti i tre browser visualizzano alla stessa cronologia di chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione. È presente un file di script denominato *hub* che genera la libreria di SignalR in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.

    ![script hub generato automaticamente nel nodo documenti Script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione SignalRChat illustra due attività di sviluppo di SignalR base. Viene illustrato come creare un hub. Il server utilizza tale hub dell'oggetto principale coordinamento. L'hub Usa la libreria jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs-in-the-chathubcs"></a>Hub SignalR nel ChatHub.cs

Nell'esempio di codice precedente, il `ChatHub` deriva dalla classe di `Microsoft.AspNet.SignalR.Hub` classe. Derivazione da di `Hub` classe è un modo utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe dell'hub e quindi usare tali metodi chiamandole dagli script in una pagina web.

Nel codice di chat, i client chiamano il `ChatHub.Send` metodo per inviare un nuovo messaggio. L'hub invia quindi il messaggio a tutti i client chiamando `Clients.All.broadcastMessage`.

Il `Send` metodo illustra alcuni concetti di hub:

* Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.

* Usare il `Microsoft.AspNet.SignalR.Hub.Clients` proprietà dinamica per comunicare con tutti i client connessi a questo hub.

* Chiamare una funzione nel client (ad esempio il `broadcastMessage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>SignalR e jQuery in Index. HTML

Il *index. HTML* pagina nell'esempio di codice illustra come usare la libreria jQuery SignalR per comunicare con un hub SignalR. Il codice esegue molte attività importanti. Dichiara un proxy per l'hub di riferimento, si dichiara una funzione che il server può chiamare per contenuto push ai client e avvia una connessione per inviare messaggi all'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> In JavaScript deve essere camelCase il riferimento alla classe server e i relativi membri. I riferimenti di esempio di codice di C# *ChatHub* classi in JavaScript come `chatHub`.

In questo blocco di codice, si crea una funzione di callback nello script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client. Le due righe di tale codifica HTML il contenuto prima di visualizzarla sono facoltativi e Mostra un buon metodo per impedire l'inserimento di script.

Questo codice consente di aprire una connessione con l'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Questo approccio assicura che il codice stabilisce una connessione prima dell'esecuzione il gestore dell'evento.

Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina HTML.

## <a name="get-the-code"></a>Ottenere il codice

[Download progetto completato](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su SignalR, vedere le risorse seguenti:

* [Progetto di SignalR](http://signalr.net)

* [SignalR Github ed esempi](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è:

> [!div class="checklist"]
> * Configurare il progetto
> * È stato eseguito il codice di esempio
> * Esaminare il codice

Passare all'articolo successivo per informazioni su come usare SignalR e MVC 5.
> [!div class="nextstepaction"]
> [SignalR 2 e MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)