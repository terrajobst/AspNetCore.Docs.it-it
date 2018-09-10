---
title: 'Esercitazione: Introduzione a SignalR in ASP.NET Core'
author: tdykstra
description: In questa esercitazione viene creata un'app di chat che usa SignalR per ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055832"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Esercitazione: Introduzione a SignalR in ASP.NET Core

Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR. Vengono illustrate le seguenti procedure:

> [!div class="checklist"]
> * Creare un'app Web che usa SignalR in ASP.NET Core.
> * Creare un hub SignalR nel server.
> * Connettersi all'hub SignalR da client JavaScript.
> * Usare l'hub per inviare messaggi a tutti i client connessi da qualsiasi client.

Al termine, si disporrà di un'app di chat funzionante:

![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Prerequisiti

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.7.3 o successive con il carico di lavoro **Sviluppo ASP.NET e Web**
* [.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (gestore pacchetti per Node.js, usato per la libreria client JavaScript di SignalR)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all)
* [C# per Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (gestore pacchetti per Node.js, usato per la libreria client JavaScript di SignalR)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* [Visual Studio per Mac versione 7.5.4 o successiva](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all) (incluso nell'installazione di Visual Studio)
* [npm](https://www.npmjs.com/get-npm) (gestore pacchetti per Node.js, usato per la libreria client JavaScript di SignalR)

---

## <a name="create-the-project"></a>Creare il progetto

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Nel menu selezionare **File > Nuovo progetto**.

* Nella finestra di dialogo **Nuovo progetto** selezionare **Installate > Visual C# > Web > Applicazione Web ASP.NET Core**. Denominare il progetto *SignalRChat*.

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages.

* Selezionare un framework di destinazione di **.NET Core**, selezionare **ASP.NET Core 2.1** e fare clic su **OK**.

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Aprire una cartella che è possibile usare per un nuovo progetto.

* In **Terminale integrato** eseguire il comando seguente:

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Nel menu selezionare **File > Nuova soluzione**.

* Selezionare **.NET Core > App > App Web ASP.NET Core** (non selezionare **App Web ASP.NET Core (MVC)**).

* Scegliere **Avanti**.

* Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.

---

## <a name="add-the-signalr-client-library"></a>Aggiungere la libreria client di SignalR

La libreria server di SignalR è inclusa nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). È tuttavia necessario ottenere la libreria client JavaScript da [npm, il sistema di gestione pacchetti di Node.js](https://www.npmjs.com/get-npm).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* In **Console di Gestione pacchetti** (PMC), passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

2. Passare alla cartella del nuovo progetto.

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* In **Terminale** passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.

---

* Eseguire l'inizializzatore di npm per creare un file *package.json*:

  ```console
  npm init -y
  ```

  Il comando crea un output simile all'esempio seguente:

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* Installare il pacchetto della libreria client:

  ```console
  npm install @aspnet/signalr
  ```

  Il comando crea un output simile all'esempio seguente:

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

Il comando `npm install` ha scaricato la libreria client JavaScript in una sottocartella in *node_modules*. Copiarla da tale percorso in una cartella in *wwwroot* a cui è possibile fare riferimento dalla pagina Web dell'app di chat.

* Creare una cartella *signalr* in *wwwroot/lib*.

* Copiare il file *signalr.js* da *node_modules/@aspnet/signalr/dist/browser* alla nuova cartella *wwwroot/lib/signalr*.

## <a name="create-the-signalr-hub"></a>Creare l'hub di SignalR

Un [hub](xref:signalr/hubs) è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.

* Nella cartella del progetto SignalRChat creare una cartella *Hubs*.

* Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  La classe `ChatHub` eredita dalla classe [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) di SignalR. La classe `Hub` gestisce connessioni, gruppi e messaggistica.

  Il metodo `SendMessage`, che può essere chiamato da qualsiasi client connesso, invia il messaggio ricevuto a tutti i client. Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.

## <a name="configure-the-project-to-use-signalr"></a>Configurare il progetto per l'uso di SignalR

È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.

* Aggiungere il codice evidenziato seguente al file *Startup.cs*.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Con queste modifiche SignalR viene aggiunto al sistema di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) e alla pipeline [middleware](xref:fundamentals/middleware/index).

## <a name="create-the-signalr-client-code"></a>Creare il codice del client SignalR

* Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  Il codice precedente:

  * Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.
  * Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.
  * Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.

* Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  Il codice precedente:

  * Crea e avvia una connessione.
  * Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.
  * Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.

## <a name="run-the-app"></a>Eseguire l'app

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Premere **CTRL+F5** per eseguire l'app senza debug.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Premere **CTRL+F5** per eseguire l'app senza debug.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Nel menu selezionare **Esegui > Avvia senza eseguire debug**.

---

* Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.

* Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.

  Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.

  ![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console. È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript. Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni. In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.
> ![Errore di signalr.js non trovato](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Passaggi successivi

Se si vuole che i client si connettano a un'app SignalR da domini diversi, è necessario abilitare Condivisione risorse tra le origini (CORS). Per altre informazioni, vedere [Condivisione risorse tra le origini](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

Per altre informazioni su SignalR, hub e client JavaScript, vedere le risorse seguenti:

* [Introduzione a SignalR per ASP.NET Core](xref:signalr/introduction)
* [Usare gli hub in SignalR per ASP.NET Core](xref:signalr/hubs)
* [Client JavaScript di SignalR per ASP.NET Core](xref:signalr/javascript-client)
