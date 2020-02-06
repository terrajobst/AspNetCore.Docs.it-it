---
title: Usare ASP.NET Core SignalR con Blazor webassembly
author: guardrex
description: Creare un'app di chat che usa ASP.NET Core SignalR con Blazor webassembly.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: d3605c0823e9ec3ce34fb781da66a7470aa00622
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034178"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a>Usare ASP.NET Core SignalR con il webassembly Blazer

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Questa esercitazione illustra le nozioni di base per la creazione di un'app in tempo reale usando SignalR con il webassembly blazer. Si apprenderà come:

> [!div class="checklist"]
> * Creare un progetto di app ospitata webassembly Blazer
> * Aggiungere la libreria client di SignalR
> * Aggiungere un hub SignalR
> * Aggiungere i servizi SignalR e un endpoint per l'hub SignalR
> * Aggiungere il codice del componente Razor per la chat

Al termine di questa esercitazione, si disporrà di un'app di chat funzionante.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a>Creare un progetto di app webassembly in hosting Blazer

Installare il modello [Webassembly Blazer](xref:blazor/hosting-models#blazor-webassembly) . Il pacchetto [Microsoft. AspNetCore. Blazer. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) include una versione di anteprima mentre Webassembly blazer è in anteprima. In una shell dei comandi eseguire il comando seguente:

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
```

Seguire le istruzioni per la scelta degli strumenti:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Creare un nuovo progetto.

1. Selezionare **app Blazer** e fare clic su **Avanti**.

1. Digitare "BlazorSignalRApp" nel campo **nome progetto** . Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto. Selezionare **Crea**.

1. Scegliere il modello **app Webassembly Blazer** .

1. In **Avanzate**selezionare la casella di controllo **ASP.NET Core Hosted** .

1. Selezionare **Crea**.

> [!NOTE]
> Se è stato eseguito l'aggiornamento o l'installazione di una nuova versione di Visual Studio e il modello webassembly Blazer non viene visualizzato nell'interfaccia utente di Visual Studio, reinstallare il modello usando il comando `dotnet new` illustrato in precedenza.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. In una shell dei comandi eseguire il comando seguente:

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. In Visual Studio Code aprire la cartella del progetto dell'app.

1. Quando viene visualizzata la finestra di dialogo per aggiungere asset per compilare ed eseguire il debug dell'app, selezionare **Sì**. Visual Studio Code aggiunge automaticamente la cartella *. VSCODE* con i file *Launch. JSON* e *Tasks. JSON* generati.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

1. In una shell dei comandi eseguire il comando seguente:

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. In Visual Studio per Mac aprire il progetto passando alla cartella del progetto e aprendo il file di soluzione del progetto ( *. sln*).

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

In una shell dei comandi eseguire il comando seguente:

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a>Aggiungere la libreria client di SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **BlazorSignalRApp. client** e scegliere **Gestisci pacchetti NuGet**.

1. Nella finestra di dialogo **Gestisci pacchetti NuGet** verificare che l' **origine del pacchetto** sia impostata su *NuGet.org*.

1. Con **Sfoglia** selezionato, digitare "Microsoft. AspNetCore. SignalR. client" nella casella di ricerca.

1. Nei risultati della ricerca selezionare il pacchetto di `Microsoft.AspNetCore.SignalR.Client` e selezionare **Installa**.

1. Se viene visualizzata la finestra di dialogo **Anteprima modifiche** , selezionare **OK**.

1. Se viene visualizzata la finestra di dialogo **accettazione della licenza** , selezionare Accetto se **si accettano le** condizioni di licenza.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

Nel **terminale integrato** (**visualizzare** > **terminale** dalla barra degli strumenti) eseguire i comandi seguenti:

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

1. Nella barra laterale **soluzione** fare clic con il pulsante destro del mouse sul progetto **BlazorSignalRApp. client** e scegliere **Gestisci pacchetti NuGet**.

1. Nella finestra di dialogo **Gestisci pacchetti NuGet** verificare che l'elenco a discesa origine sia impostato su *NuGet.org*.

1. Con **Sfoglia** selezionato, digitare "Microsoft. AspNetCore. SignalR. client" nella casella di ricerca.

1. Nei risultati della ricerca selezionare la casella di controllo accanto al pacchetto di `Microsoft.AspNetCore.SignalR.Client` e selezionare **Aggiungi pacchetto**.

1. Se viene visualizzata la finestra di dialogo **accettazione della licenza** , selezionare **Accetto** se si accettano le condizioni di licenza.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

In una shell dei comandi eseguire i comandi seguenti:

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a>Aggiungere un hub SignalR

Nel progetto **BlazorSignalRApp. Server** creare una cartella *Hub* (plurale) e aggiungere la classe `ChatHub` seguente (*Hub/ChatHub. cs*):

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a>Aggiungere i servizi SignalR e un endpoint per l'hub SignalR

1. Nel progetto **BlazorSignalRApp. Server** aprire il file *Startup.cs* .

1. Aggiungere lo spazio dei nomi per la classe `ChatHub` all'inizio del file:

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. Aggiungere i servizi SignalR a `Startup.ConfigureServices`:

   ```csharp
   services.AddSignalR();
   ```

1. In `Startup.Configure` tra gli endpoint per la route del controller predefinita e il fallback sul lato client, aggiungere un endpoint per l'hub:

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a>Aggiungere il codice del componente Razor per la chat

1. Nel progetto **BlazorSignalRApp. client** aprire il file *pages/index. Razor* .

1. Sostituire il markup con il codice seguente:

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a>Eseguire l'app

1. Seguire le istruzioni per gli strumenti:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. In **Esplora soluzioni**selezionare il progetto **BlazorSignalRApp. Server** . Premere **CTRL + F5** per eseguire l'app senza debug.

1. Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**. Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine:

   ![App di esempio webassembly di SignalR Blazer aperta in due finestre del browser che mostrano i messaggi scambiati.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Virgolette: *Star Trek VI: il paese non individuato* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Selezionare **debug** > **Esegui senza eseguire debug** dalla barra degli strumenti.

1. Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**. Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine:

   ![App di esempio webassembly di SignalR Blazer aperta in due finestre del browser che mostrano i messaggi scambiati.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Virgolette: *Star Trek VI: il paese non individuato* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

1. Nella barra laterale della **soluzione** selezionare il progetto **BlazorSignalRApp. Server** . Dal menu selezionare **esegui** > **Avvia senza eseguire debug**.

1. Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**. Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine:

   ![App di esempio webassembly di SignalR Blazer aperta in due finestre del browser che mostrano i messaggi scambiati.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Virgolette: *Star Trek VI: il paese non individuato* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

1. In una shell dei comandi eseguire i comandi seguenti:

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**. Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine:

   ![App di esempio webassembly di SignalR Blazer aperta in due finestre del browser che mostrano i messaggi scambiati.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Virgolette: *Star Trek VI: il paese non individuato* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

---

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state illustrate le procedure per:

> [!div class="checklist"]
> * Creare un progetto di app ospitata Blazor webassembly
> * Aggiungere la libreria client di SignalR
> * Aggiungere un hub SignalR
> * Aggiungere SignalR Services e un endpoint per l'hub SignalR
> * Aggiungere il codice del componente Razor per la chat

Per ulteriori informazioni sulla compilazione di app Blazor, vedere la documentazione di Blazor:

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:signalr/introduction>
