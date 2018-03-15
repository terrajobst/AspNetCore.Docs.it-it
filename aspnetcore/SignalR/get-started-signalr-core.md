---
title: Introduzione a SignalR su ASP.NET Core
author: rachelappel
description: Le informazioni di base della creazione di un'applicazione in tempo reale mediante SignalR per ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/06/2018
ms.prod: aspnet-core
ms.technology: dotnet-signalr
ms.topic: tutorial
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 79af59fc8c2ada71d764ada95a431e10f4f00f27
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Esercitazione: Introduzione a SignalR per ASP.NET Core

[Rachel Appel](https://twitter.com/rachelappel)

In questa esercitazione illustra le nozioni di base della creazione di un'applicazione in tempo reale mediante SignalR per ASP.NET Core.

   ![Soluzione](get-started-signalr-core/_static/signalr-get-started-finished.png)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Questa esercitazione illustra le attività di sviluppo SignalR seguenti:

> [!div class="checklist"]
> * Creare un'app web ASP.NET Core.
> * Creare un hub di SignalR per eseguire il push del contenuto ai client.
> * Utilizzare la libreria SignalR JavaScript per l'invio di messaggi e visualizza gli aggiornamenti dall'hub.

## <a name="prerequisites"></a>Prerequisiti

Installare il software seguente:

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) o versione successiva
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,6 o versione successiva con il carico di lavoro di sviluppo ASP.NET e web
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Creare un progetto ASP.NET di base che ospita i server e client SignalR

1. Utilizzare il **File** > **nuovo progetto** menu opzione **applicazione Web di ASP.NET Core**. Denominare il progetto `SignalRChat`.

  ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Selezionare **applicazione Web** per creare un progetto utilizzando le pagine Razor. Selezionare quindi **Ok**. Assicurarsi che **ASP.NET Core 2.1** è selezionata dal selettore del framework, se SignalR viene eseguito nelle versioni precedenti di .NET.

  ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  Le librerie che ospitano il codice lato server di SignalR sono inclusi nel modello di progetto. Installare il codice JavaScript sul lato client separatamente con [npm](https://www.npmjs.com/).

  ```console
   npm install @aspnet/signalr
  ```

3. Copia il *signalr.js* da *node_modules\\ @aspnet\signalr\dist\browser*  per il *wwwroot\lib* cartella nel progetto.

## <a name="create-the-signalr-hub"></a>Creazione dell'Hub di SignalR

Un hub è una classe che funge da una pipeline di alto livello che consente al client e server chiamare i metodi tra loro.

1. Aggiungere una classe al progetto scegliendo **File** > **New** > **File** e selezionando **classe Visual c#**. 

1. Ereditare da `Microsoft.AspNetCore.SignalR.Hub`. La `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere dati.

1. Creare il `Send` metodo che invia un messaggio a tutti i client connessi chat. Si noti restituisce un `Task`perché SignalR è asincrono. Codice asincrono è scalabile.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>Configurare il progetto per l'utilizzo di SignalR

Il server di SignalR deve essere configurato in modo da informarlo che può per passare richieste a SignalR.

1. Per configurare un progetto SignalR, modificare il `ConfigureServices` metodo dell'applicazione `Startup` classe mediante l'inserimento di una chiamata a `services.AddSignalR`.

  `services.AddSignalR` Aggiunge SignalR come parte di [middleware ASP.NET Core](xref:fundamentals/middleware/index) pipeline.

1. Configurare le route per l'hub utilizzando `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Creare il codice client SignalR

1. Sostituire il contenuto di *Pages\Index.cshtml* con il codice seguente:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  HTML precedente Visualizza nome e i campi dei messaggi e un pulsante di invio. Notare i riferimenti a script nella parte inferiore: un riferimento a SignalR e *chat.js*.

1. Aggiungere un file JavaScript al *wwwroot\js* cartella denominata *chat.js* e aggiungervi il codice seguente:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Eseguire l'app

1. Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale. Copiare l'URL dalla barra degli indirizzi.

1. Aprire un'altra istanza del browser (qualsiasi browser) e incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante. Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.

  ![Soluzione](get-started-signalr-core/_static/signalr-get-started-finished.png)
