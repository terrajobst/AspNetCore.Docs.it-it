---
title: Introduzione ad ASP.NET Core MVC
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Informazioni introduttive su ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: cfce3b5792a5d0673bae5ddbba9e2d4d515a6279
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381803"
---
# <a name="get-started-with-aspnet-core-mvc"></a>Introduzione ad ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

https://docs.microsoft.com/en-us/visualstudio/ide/visual-studio-ide?view=vs-2017

Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.

L'app gestisce un database di titoli di film. Vengono illustrate le seguenti procedure:

> [!div class="checklist"]
> * Creare un'app Web.
> * Aggiungere un modello ed eseguirne lo scaffolding.
> * Usare un database.
> * Aggiungere ricerca e convalida.

Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

> [!NOTE]
> È in corso un test di usabilità di una nuova proposta di struttura del sommario di ASP.NET Core.  Se si dispone di alcuni minuti per provare a cercare 7 argomenti diversi nel sommario corrente o proposto, [fare clic qui per partecipare al test](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a>Creare un'app Web

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

In Visual Studio selezionare **File > Nuovo > Progetto**.

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

Completare la finestra di dialogo **Nuovo progetto**:

* Nel riquadro a sinistra selezionare **.NET Core**.
* Nel riquadro al centro selezionare **Applicazione Web ASP.NET Core (.NET Core)**.
* Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).
* Scegliere **OK**.

![Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:

* Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.2**.
* Selezionare **Applicazione Web (Model-View-Controller)**
* Scegliere **OK**.

![Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio ha usato un modello predefinito per il progetto MVC appena creato. Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni. Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.

Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.

* Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app. Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Nell'immagine precedente il numero di porta è 5000. Nell'URL nel browser viene visualizzato `localhost:5000`. Quando si esegue l'app verrà visualizzato un numero di porta diverso.
* Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.
* È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:

![Menu Debug](start-mvc/_static/debug_menu.png)

* È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.

![IIS Express](start-mvc/_static/iis_express.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Nell'esercitazione si presuppone una familarità con Visual Studio Code. Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.

* Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.
* Eseguire il comando seguente:

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Risorse di compilazione e debug mancanti da "MvcMovie". Aggiungerle?)  Selezionare **Sì**.

  * `dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.
  * `code -r MvcMovie`: carica il file di progetto *MvcMovie.csproj* in Visual Studio Code.

### <a name="launch-the-app"></a>Avviare l'app

* Premere **CTRL+F5** per l'esecuzione senza il debugger.

  Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`. La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Localhost viene usato solo per le richieste web del computer locale.

  Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Selezionare **File** > **Nuova soluzione**.

  ![Nuova soluzione macOS](~/tutorials/first-web-api-mac/_static/sln.png)

* Selezionare **.NET Core App (App .NET Core)** > **ASP.NET Core** > **App Web ASP.NET Core (MVC)** > **Avanti**.

  ![Finestra di dialogo Nuovo progetto di macOS](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero **.NET Core 2.2*.

* Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.

### <a name="launch-the-app"></a>Avviare l'app

Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**. Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.

* La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Quando si esegue l'app verrà visualizzato un numero di porta diverso.
* È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.

---  
<!-- End of VS tabs -->

* Selezionare **Accept** (Accetto) per autorizzare il rilevamento. Questa app non rileva informazioni personali. Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  La figura seguente illustra l'app dopo aver accettato il rilevamento:

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.

> [!div class="step-by-step"]
> [avanti](adding-controller.md)  
