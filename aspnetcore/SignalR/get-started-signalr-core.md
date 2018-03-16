---
title: Introduzione a SignalR su ASP.NET Core
author: rachelappel
description: In questa esercitazione, si crea un'app usando SignalR per ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 139da5a2d0dadf51fece94b7c54ccd531e0ae8c2
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Esercitazione: Introduzione a SignalR per ASP.NET Core

[Rachel Appel](https://twitter.com/rachelappel)

In questa esercitazione illustra le nozioni di base della creazione di un'applicazione in tempo reale mediante SignalR per ASP.NET Core.

   ![Soluzione](get-started-signalr-core/_static/signalr-get-started-finished.png)

Questa esercitazione illustra le attività di sviluppo SignalR seguenti:

> [!div class="checklist"]
> * Creare un SignalR nell'applicazione web ASP.NET Core.
> * Creare un hub di SignalR per eseguire il push del contenuto ai client.
> * Modificare il `Startup` classe e configurare l'app.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Prerequisiti

Installare il software seguente:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) o versione successiva
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,6 o versione successiva con il **sviluppo web ASP.NET e** carico di lavoro
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) o versione successiva
* [Visual Studio Code](https://code.visualstudio.com/download) 
* [Linguaggio c# per il codice di Visual Studio](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Creare un progetto ASP.NET di base che ospita i server e client SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Utilizzare il **File** > **nuovo progetto** menu opzione **applicazione Web di ASP.NET Core**. Denominare il progetto *SignalRChat*.

  ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Selezionare **applicazione Web** per creare un progetto utilizzando le pagine Razor. Selezionare quindi **OK**. Assicurarsi che **ASP.NET Core 2.1** è selezionata dal selettore del framework, se SignalR viene eseguito nelle versioni precedenti di .NET.

  ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

3. Fare clic sul progetto in **Esplora soluzioni** > **Add** > **nuovo elemento** > **npm File di configurazione** . Denominare il file *package. JSON*.

4. Eseguire il comando seguente **Console di gestione pacchetti** finestra, dalla radice del progetto:

    ```console
      npm install @aspnet/signalr
    ```
5. Copia il *signalr.js* file dalla *node_modules\\ @aspnet\signalr\dist\browser*  per il *wwwroot\lib* cartella nel progetto.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Dal **Terminal integrata**, eseguire il comando seguente:
 
    ```console
      dotnet new razor -o SignalRChat
    ```

2. Installare la libreria client JavaScript usando *npm*.

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a>Creazione dell'Hub di SignalR

Un hub è una classe che funge da una pipeline di alto livello che consente al client e server chiamare i metodi tra loro.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Aggiungere una classe al progetto scegliendo **File** > **New** > **File** e selezionando **classe Visual c#**.

1. Ereditare da `Microsoft.AspNetCore.SignalR.Hub`. La `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere dati.

1. Creare il `SendMessage` metodo che invia un messaggio a tutti i client connessi chat. Si noti restituisce un [attività](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), in quanto SignalR è asincrono. Codice asincrono è scalabile.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Aprire la *SignalRChat* cartella in Visual Studio Code.

1. Aggiungere una classe al progetto selezionando **File** > **nuovo File** dal menu di scelta.

1. Ereditare da `Microsoft.AspNetCore.SignalR.Hub`. Il `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere i dati ai client.

1. Aggiungere un metodo `SendMessage` alla classe. Il `SendMessage` metodo invia un messaggio a tutti i client connessi chat. Si noti restituisce un [attività](/dotnet/api/system.threading.tasks.task), in quanto SignalR è asincrono. Codice asincrono è scalabile.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Configurare il progetto per l'utilizzo di SignalR

Il server di SignalR deve essere configurato in modo da informarlo che può per passare richieste a SignalR.

1. Per configurare un progetto SignalR, modificare il progetto `Startup.ConfigureServices` metodo.

  `services.AddSignalR` Aggiunge SignalR come parte di [middleware](xref:fundamentals/middleware/index) pipeline.

1. Configurare le route per l'hub utilizzando `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Creare il codice client SignalR

1. Sostituire il contenuto di *Pages\Index.cshtml* con il codice seguente:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  HTML precedente Visualizza nome e i campi dei messaggi e un pulsante di invio. Notare i riferimenti a script nella parte inferiore: un riferimento a SignalR e *chat.js*.

1. Aggiungere un file JavaScript, denominato *chat.js*, per il *wwwroot\js* cartella. Aggiungere il codice seguente al file:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Eseguire l'app

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale. Copiare l'URL dalla barra degli indirizzi.

1. Aprire un'altra istanza del browser (qualsiasi browser) e incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante. Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Premere **Debug** (F5) per compilare ed eseguire il programma. Eseguire il programma apre una finestra del browser.

1. Aprire un'altra finestra del browser e caricare il sito Web in locale in essa.

1. Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante. Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.

-----

  ![Soluzione](get-started-signalr-core/_static/signalr-get-started-finished.png)