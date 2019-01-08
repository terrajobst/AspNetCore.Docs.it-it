---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Esercitazione: Chat in tempo reale con SignalR 2 e MVC 5 | Microsoft Docs'
author: pfletcher
description: Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. SignalR è aggiungere a un'applicazione MVC 5.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098604"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Esercitazione: Chat in tempo reale con SignalR 2 e MVC 5

Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. Aggiungere SignalR a un'applicazione MVC 5 e creare una vista di chat per inviare e visualizzare i messaggi.

In questa esercitazione è:

> [!div class="checklist"]
> * Configurare il progetto
> * Eseguire l'esempio
> * Esaminare il codice

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il **sviluppo ASP.NET e web** carico di lavoro.

## <a name="set-up-the-project"></a>Configurare il progetto

Questa sezione illustra come usare SignalR 2 e Visual Studio 2017 per creare un'applicazione ASP.NET MVC 5 vuota, aggiungere la libreria di SignalR e creare l'applicazione di chat.

1. In Visual Studio, creare un'applicazione c# ASP.NET destinata a .NET Framework 4.5, denominarlo SignalRChat e fare clic su OK.

    ![Creazione di web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. Nelle **nuova applicazione Web ASP.NET - SignalRMvcChat**, selezionare **MVC** e quindi selezionare **Modifica autenticazione**.

1. Nelle **Modifica autenticazione**, selezionare **Nessuna autenticazione** e fare clic su **OK**.

    ![Seleziona Nessuna autenticazione](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. Nelle **nuova applicazione Web ASP.NET - SignalRMvcChat**, selezionare **OK**.

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.

1. Nelle **Aggiungi nuovo elemento - SignalRChat**, selezionare **installati** > **Visual C#**   >  **Web**  >  **SignalR** e quindi selezionare **classe tramite SignalR Hub (v2)**.

1. Denominare la classe *ChatHub* e aggiungerlo al progetto.

    Questo passaggio Crea il *ChatHub.cs* file di classe e aggiunge un set di file di script e i riferimenti ad assembly che supportano SignalR al progetto.

1. Sostituire il codice nella nuova *ChatHub.cs* file di classe con il seguente codice:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **classe**.

1. Denominare la nuova classe *avvio* e aggiungerlo al progetto.

1. Sostituire il codice nel *Startup.cs* file di classe con il seguente codice:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. Nelle **Esplora soluzioni**, selezionare **controller** > **HomeController.cs**.

1. Aggiungere questo metodo per la *HomeController.cs*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Questo metodo restituisce il **Chat** vista creata in un passaggio successivo.

1. Nelle **Esplora soluzioni**, fare doppio clic su **viste** > **Home**e selezionare **Aggiungi**  >    **Visualizzazione**.

1. Nelle **Aggiungi visualizzazione**, assegnare un nome alla nuova visualizzazione **Chat** e selezionare **Add**.

1. Sostituire il contenuto del **Chat.cshtml** con questo codice:

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. Nelle **Esplora soluzioni**, espandere **script**.

    Librerie di script per jQuery e SignalR sono visibili nel progetto.

    > [!IMPORTANT]
    > La gestione dei pacchetti potrebbe avere installato una versione successiva degli script di SignalR.

1. Verificare che i riferimenti a script nel blocco di codice corrispondono alle versioni dei file di script nel progetto.

    Riferimenti a script dal blocco di codice originale:

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Se non corrispondono, aggiornare il *cshtml* file.

1. Dalla barra dei menu, selezionare **File** > **Salva tutto**.

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Nella barra degli strumenti, attivare **debug degli Script** e quindi selezionare il pulsante play per eseguire l'esempio in modalità di Debug.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Quando si apre il browser, immettere un nome per l'identità di chat.

1. Copiare l'URL dal browser, aprire due altri browser e incollare l'URL nella barra degli indirizzi.

1. In ogni browser e immettere un nome univoco.

1. A questo punto, aggiungere un commento e selezionare **inviare**. Ripetere che in altri browser. I commenti vengono visualizzati in tempo reale.

    > [!NOTE]
    > Questa applicazione di chat semplice non gestisce il contesto di discussione sul server. L'hub trasmette i commenti per tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.

    Vedere come l'applicazione di chat viene eseguita in tre diversi browser. Per Susan, Anand e Tom invio di messaggi, aggiornare tutti i browser in tempo reale:

    ![Tutti i tre browser visualizzano alla stessa cronologia di chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione. È presente un file di script denominato *hub* che genera la libreria di SignalR in fase di esecuzione. Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.

    ![script hub generato automaticamente nel nodo documenti Script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione di chat SignalR illustra due attività di sviluppo di SignalR base. Viene illustrato come creare un hub. Il server utilizza tale hub dell'oggetto principale coordinamento. L'hub Usa la libreria jQuery SignalR per inviare e ricevere messaggi.

### <a name="signalr-hubs-in-the-chathubcs"></a>Hub SignalR nel ChatHub.cs

Nell'esempio di codice, il `ChatHub` deriva dalla classe di `Microsoft.AspNet.SignalR.Hub` classe. Derivazione da di `Hub` classe è un modo utile per compilare un'applicazione di SignalR. È possibile creare metodi pubblici nella classe dell'hub e quindi accedere a tali metodi chiamandole dagli script in una pagina web.

Nel codice di chat, i client chiamano il `ChatHub.Send` metodo per inviare un nuovo messaggio. L'hub, a sua volta invia il messaggio a tutti i client chiamando `Clients.All.addNewMessageToPage`.

Il `Send` metodo illustra alcuni concetti di hub:

* Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.

* Usare il `Microsoft.AspNet.SignalR.Hub.Clients` proprietà dinamica per comunicare con tutti i client connessi a questo hub.

* Chiamare una funzione nel client (ad esempio il `addNewMessageToPage` (funzione)) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>SignalR e jQuery Chat.cshtml

Il *Chat.cshtml* Visualizza file nell'esempio di codice illustra come usare la libreria jQuery SignalR per comunicare con un hub SignalR.  Il codice esegue molte attività importanti. Viene creato un riferimento al proxy generato automaticamente per l'hub e si dichiara una funzione che il server può chiamare per eseguire il push del contenuto ai client e avvia una connessione per inviare messaggi all'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> In JavaScript, il riferimento alla classe server e i relativi membri è incluso camelCase. I riferimenti di esempio di codice di C# `ChatHub` classi in JavaScript come `chatHub`.

In questo blocco di codice, si crea una funzione di callback nello script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client. La chiamata facoltativa al `htmlEncode` funzione Mostra un modo per HTML codifica il contenuto del messaggio prima di visualizzarlo nella pagina. È un modo per impedire attacchi script injection.

Questo codice consente di aprire una connessione con l'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Questo approccio assicura che si stabilisce una connessione prima dell'esecuzione il gestore dell'evento.

Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina della Chat.

## <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su SignalR, vedere le risorse seguenti:

* [Progetto di SignalR](http://signalr.net)

* [SignalR GitHub ed esempi](https://github.com/SignalR/SignalR)

* [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è:

> [!div class="checklist"]
> * Configurare il progetto
> * È stato eseguito il codice di esempio
> * Esaminare il codice

Passare all'articolo successivo per informazioni su come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza.
> [!div class="nextstepaction"]
> [App Web con la messaggistica ad alta frequenza](tutorial-high-frequency-realtime-with-signalr.md)