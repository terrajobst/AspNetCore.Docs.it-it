---
title: Introduzione a SignalR su ASP.NET Core
author: rachelappel
description: In questa esercitazione, si crea un'app usando SignalR per ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/09/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 11c8cf079a0922e925060ad3d439ba5e095ae22e
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Introduzione a SignalR su ASP.NET Core

[Rachel Appel](https://twitter.com/rachelappel)

In questa esercitazione illustra le nozioni di base della creazione di un'applicazione in tempo reale mediante SignalR per ASP.NET Core.

   ![Soluzione](get-started/_static/signalr-get-started-finished.png)

Questa esercitazione illustra le attività di sviluppo SignalR seguenti:

> [!div class="checklist"]
> * Creare un SignalR nell'applicazione web ASP.NET Core.
> * Creare un hub di SignalR per eseguire il push del contenuto ai client.
> * Modificare il `Startup` classe e configurare l'app.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Prerequisiti

Installare il software seguente:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) o versione successiva
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 o versione successiva con il **sviluppo web ASP.NET e** carico di lavoro
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) o versione successiva
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Linguaggio c# per il codice di Visual Studio](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Creare un progetto ASP.NET di base che ospita i server e client SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Utilizzare il **File** > **nuovo progetto** menu opzione **applicazione Web di ASP.NET Core**. Denominare il progetto *SignalRChat*.

   ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. Selezionare **applicazione Web** per creare un progetto utilizzando le pagine Razor. Selezionare quindi **OK**. Assicurarsi che **ASP.NET Core 2.1** è selezionata dal selettore del framework, se SignalR viene eseguito nelle versioni precedenti di .NET.

   ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

Visual Studio include il `Microsoft.AspNetCore.SignalR` pacchetto contenente le relative librerie server come parte della relativa **applicazione Web di ASP.NET Core** modello. Tuttavia, la libreria client JavaScript per SignalR deve essere installata utilizzando *npm*.

3. Eseguire i comandi seguenti **Console di gestione pacchetti** finestra, dalla radice del progetto:

    ```console
      npm init -y
      npm install @aspnet/signalr
    ```     

4. Copia il *signalr.js* file dalla *node_modules\\ @aspnet\signalr\dist\browser*  per il *lib* cartella nel progetto.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Dal **Terminal integrata**, eseguire il comando seguente:

    ```console
      dotnet new razor -o SignalRChat
    ```

2. Installare la libreria client JavaScript usando *npm*.

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

3. Copia il *signalr.js* file dalla *node_modules\\ @aspnet\signalr\dist\browser*  per il *lib* cartella nel progetto.

-----

## <a name="create-the-signalr-hub"></a>Creazione dell'Hub di SignalR

Un hub è una classe che funge da una pipeline di alto livello che consente al client e server chiamare i metodi tra loro.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Aggiungere una classe al progetto scegliendo **File** > **New** > **File** e selezionando **classe Visual c#**.

2. Ereditare da `Microsoft.AspNetCore.SignalR.Hub`. La `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere dati.

3. Creare il `SendMessage` metodo che invia un messaggio a tutti i client connessi chat. Si noti restituisce un [attività](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), in quanto SignalR è asincrono. Codice asincrono è scalabile.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Aprire la *SignalRChat* cartella in Visual Studio Code.

2. Aggiungere una classe al progetto selezionando **File** > **nuovo File** dal menu di scelta.

3. Ereditare da `Microsoft.AspNetCore.SignalR.Hub`. Il `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere i dati ai client.

4. Aggiungere un metodo `SendMessage` alla classe. Il `SendMessage` metodo invia un messaggio a tutti i client connessi chat. Si noti restituisce un [attività](/dotnet/api/system.threading.tasks.task), in quanto SignalR è asincrono. Codice asincrono è scalabile.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Configurare il progetto per l'utilizzo di SignalR

Il server di SignalR deve essere configurato in modo da informarlo che può per passare richieste a SignalR.

1. Per configurare un progetto SignalR, modificare il progetto `Startup.ConfigureServices` metodo.

   `services.AddSignalR` Aggiunge SignalR come parte di [middleware](xref:fundamentals/middleware/index) pipeline.

2. Configurare le route per l'hub utilizzando `UseSignalR`.


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a>Creare il codice client SignalR

1. Sostituire il contenuto di *Pages\Index.cshtml* con il codice seguente:

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   HTML precedente Visualizza nome e i campi dei messaggi e un pulsante di invio. Notare i riferimenti a script nella parte inferiore: un riferimento a SignalR e *chat.js*.

2. Aggiungere un file JavaScript, denominato *chat.js*, per il *wwwroot\js* cartella. Aggiungere il codice seguente al file:

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

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

  ![Soluzione](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Risorse correlate

[Introduzione a ASP.NET Core SignalR](introduction.md)