---
title: Introduzione ad ASP.NET Core SignalR
author: bradygaster
description: In questa esercitazione viene creata un'app di chat che usa ASP.NET Core SignalR.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: fd3324dfa0731ae16747178d83bd93ed95dd15ce
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724472"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a>Esercitazione: Introduzione ad ASP.NET Core SignalR

Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR. Vengono illustrate le seguenti procedure:

> [!div class="checklist"]
> * Creare un progetto Web.
> * Aggiungere la libreria client di SignalR.
> * Creare un hub di SignalR.
> * Configurare il progetto per l'uso di SignalR.
> * Aggiungere codice che invia messaggi da qualsiasi client a tutti i client connessi.

Al termine, si disporrà di un'app di chat funzionante:

![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

## <a name="prerequisites"></a>Prerequisiti

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a>Creare un progetto di app Web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Nel menu selezionare **File > Nuovo progetto**.

* Nella finestra di dialogo **Crea un nuovo progetto** selezionare **Applicazione Web ASP.NET Core**, quindi selezionare **Avanti**.

* Nella finestra di dialogo **Configura il nuovo progetto**, denominare il progetto *SignalRChat*, quindi selezionare **Crea**.

* Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**. 

* Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages, quindi selezionare **Crea**.

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal) alla cartella in cui verrà creata la nuova cartella del progetto.

* Eseguire i comandi seguenti:

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Nel menu selezionare **File > Nuova soluzione**.

* Selezionare **.NET Core > App > Applicazione Web** (Non selezionare **Applicazione Web (Model-View-Controller)** ), quindi selezionare **Avanti**.

* Assicurarsi che il **framework di destinazione** sia impostato su **.NET Core 3.0**, quindi selezionare **Avanti**.

* Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.

---

## <a name="add-the-signalr-client-library"></a>Aggiungere la libreria client di SignalR

La libreria server di SignalR è inclusa nel framework condiviso ASP.NET Core 3.0. La libreria client JavaScript non viene inclusa automaticamente nel progetto. Per questa esercitazione, usare Gestione librerie (LibMan) per ottenere la libreria client da *unpkg*. Unpkg è una rete per la distribuzione di contenuti (CDN, Content Delivery Network) che può offrire qualsiasi contenuto disponibile in npm, la gestione pacchetti di Node.js.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Client-Side Library** (Libreria lato client).

* Nella finestra di dialogo **Add Client-Side Library** (Aggiungi libreria lato client) selezionare **unpkg** in **Provider**.

* Per la **Library**, immettere `@aspnet/signalr@next`.
<!-- when 3.0 is released, change @next to @latest -->

* Selezionare **Choose specific files** (Scegli file specifici), espandere la cartella *dist/browser* e selezionare *signalr.js* e *signalr.min.js*.

* Impostare **Percorso di destinazione** su *wwwroot/lib/signalr/* e selezionare **Installa**.

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione della libreria](signalr/_static/libman1.png)

  LibMan crea una cartella *wwwroot/lib/signalr* e copia i file selezionati in tale cartella.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Eseguire il comando seguente nel terminale integrato per installare LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan. Potrebbe essere necessario attendere alcuni secondi prima di visualizzare l'output.

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  I parametri specificano le opzioni seguenti:
  * Usare il provider unpkg.
  * Copiare i file nella destinazione *wwwroot/lib/signalr*.
  * Copiare solo i file specificati.

  L'output è simile all'esempio seguente:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Nel **Terminale** eseguire il comando seguente per installare LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.

* Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  I parametri specificano le opzioni seguenti:
  * Usare il provider unpkg.
  * Copiare i file nella destinazione *wwwroot/lib/signalr*.
  * Copiare solo i file specificati.

  L'output è simile all'esempio seguente:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a>Creare un hub di SignalR

Un *hub* è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.

* Nella cartella del progetto SignalRChat creare una cartella *Hubs*.

* Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:

  [!code-csharp[Startup](signalr/sample-snapshot/ChatHub.cs)]

  La classe `ChatHub` eredita dalla classe `Hub` di SignalR. La classe `Hub` gestisce connessioni, gruppi e messaggistica.

  Il metodo `SendMessage` può essere chiamato da un client connesso per inviare un messaggio a tutti i client. Il codice client JavaScript che chiama il metodo è illustrato più avanti nell'esercitazione. Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.

## <a name="configure-signalr"></a>Configurare SignalR

È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.

* Aggiungere il codice evidenziato seguente al file *Startup.cs*.

  [!code-csharp[Startup](signalr/sample-snapshot/Startup.cs?highlight=6,30,58)]

  Con queste modifiche SignalR viene aggiunto ai sistemi di inserimento delle dipendenze e di routing di ASP.NET Core.

## <a name="add-signalr-client-code"></a>Aggiungere il codice del client SignalR

* Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:

  [!code-cshtml[Index](signalr/sample-snapshot/Index.cshtml)]

  Il codice precedente:

  * Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.
  * Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.
  * Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.

* Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:

  [!code-javascript[Index](signalr/sample-snapshot/chat.js)]

  Il codice precedente:

  * Crea e avvia una connessione.
  * Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.
  * Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.

## <a name="run-the-app"></a>Eseguire l'app

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Premere **CTRL+F5** per eseguire l'app senza debug.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Eseguire il comando seguente nel terminale integrato:

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Nel menu selezionare **Esegui > Avvia senza eseguire debug**.

---

* Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.

* Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia messaggio**.

  Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.

  ![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> * Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console. È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript. Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni. In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.
>   ![Errore di signalr.js non trovato](signalr/_static/f12-console.png)
> * Se viene visualizzato l'errore ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome o NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, eseguire questi comandi per aggiornare il proprio certificato di sviluppo:
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su SignalR, vedere l'introduzione:

> [!div class="nextstepaction"]
> [Introduzione a SignalR di ASP.NET Core](xref:signalr/introduction)
