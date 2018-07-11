---
title: Introduzione a SignalR in ASP.NET Core
author: rachelappel
description: In questa esercitazione viene creata un'app usando SignalR per ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830555"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Introduzione a SignalR in ASP.NET Core

[Rachel Appel](https://twitter.com/rachelappel)

Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR per ASP.NET Core.

   ![Soluzione](signalr/_static/signalr-get-started-finished.png)

In questa esercitazione vengono illustrate le attività di sviluppo di SignalR seguenti:

> [!div class="checklist"]
> * Creare un servizio SignalR in un'app Web di ASP.NET Core.
> * Creare un hub SignalR per eseguire il push dei contenuti ai client.
> * Modificare la classe `Startup` e configurare l'app.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

Installare il software seguente:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.7.3 o successive con il carico di lavoro **Sviluppo ASP.NET e Web**
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# per Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Creare un progetto ASP.NET Core che ospita il client e il server SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Selezionare l'opzione di menu **File** > **Nuovo progetto** e scegliere **Applicazione Web ASP.NET Core**. Denominare il progetto *SignalRChat*.

   ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. Selezionare **Applicazione Web** per creare un progetto usando le Razor Pages. Selezionare **OK**. Assicurarsi che nel selettore del framework sia selezionato **ASP.NET Core 2.1**, anche se SignalR viene eseguito in versioni precedenti di .NET.

   ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio include il pacchetto `Microsoft.AspNetCore.SignalR` contenente le relative librerie server come parte del modello **Applicazione Web ASP.NET Core**. Tuttavia, la libreria client JavaScript per SignalR deve essere installata usando *npm*.

3. Eseguire i comandi seguenti nella finestra **Console di gestione pacchetti**, dalla radice del progetto:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. Creare una nuova cartella denominata "signalr" all'interno della cartella *lib* nel progetto. Copiare il file *signalr.js* da *node_modules\\@aspnet\signalr\dist\browser* in questa cartella.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Da **Terminale integrato** eseguire il comando seguente:

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Installare la libreria client JavaScript usando *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Creare una nuova cartella denominata "signalr" all'interno della cartella *lib* nel progetto. Copiare il file *signalr.js* da *node_modules\\@aspnet\signalr\dist\browser* in questa cartella.

---

## <a name="create-the-signalr-hub"></a>Creare l'hub di SignalR

Un hub è una classe che funge da pipeline di alto livello che consente al client e al server di chiamare metodi tra loro.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Aggiungere una classe al progetto scegliendo **File** > **Nuovo** > **File** e selezionando **Classe di Visual C#**. Assegnare alla classe il nome `ChatHub` e al file il nome *ChatHub.cs*.

2. Ereditare da `Microsoft.AspNetCore.SignalR.Hub`. La classe `Hub` contiene proprietà ed eventi per gestire connessioni e gruppi, nonché per inviare e ricevere dati.

3. Creare il metodo `SendMessage` che invia un messaggio a tutti i client di chat connessi. Si noti che il metodo restituisce una classe [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), in quanto SignalR è asincrono. Il codice asincrono ha una scalabilità migliore.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Aprire la cartella *SignalRChat* in Visual Studio Code.

2. Aggiungere una classe al progetto selezionando **File** > **Nuovo file** dal menu. Assegnare alla classe il nome `ChatHub` e al file il nome *ChatHub.cs*.

3. Ereditare da `Microsoft.AspNetCore.SignalR.Hub`. La classe `Hub` contiene proprietà ed eventi per gestire connessioni e gruppi, nonché per inviare e ricevere dati dai client.

4. Aggiungere un metodo `SendMessage` alla classe. Il metodo `SendMessage` invia un messaggio a tutti i client di chat connessi. Si noti che il metodo restituisce una classe [Task](/dotnet/api/system.threading.tasks.task), in quanto SignalR è asincrono. Il codice asincrono ha una scalabilità migliore.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Configurare il progetto per l'uso di SignalR

Il server di SignalR deve essere configurato in modo che passi le richieste a SignalR.

1. Per configurare un progetto SignalR, modificare il metodo `Startup.ConfigureServices` del progetto.

   `services.AddSignalR` rende disponibili i servizi SignalR per il sistema di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

1. Configurare le route per gli hub con `UseSignalR` nel metodo `Configure`. `app.UseSignalR` aggiunge SignalR alla pipeline del [middleware](xref:fundamentals/middleware/index).

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>Creare il codice del client SignalR

1. Aggiungere un file JavaScript, denominato *chat.js*, alla cartella *wwwroot\js*. Aggiungere il codice seguente al file:

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   Il codice HTML precedente visualizza i campi del nome e del messaggio e un pulsante di invio. Si notino i riferimenti dello script nella parte inferiore: un riferimento a SignalR e a *chat.js*.

## <a name="run-the-app"></a>Eseguire l'app

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale. Copiare l'URL dalla barra dell'indirizzo.

1. Aprire un'altra istanza di un browser qualsiasi e incollare l'URL nella barra dell'indirizzo.

1. Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**. Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Premere **Debug** (F5) per compilare ed eseguire il programma. Quando viene eseguito il programma si apre una finestra del browser.

1. Aprire un'altra finestra del browser e caricare il sito Web in locale.

1. Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**. Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.

---

  ![Soluzione](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Risorse correlate

[Introduzione a SignalR di ASP.NET Core](xref:signalr/introduction)
