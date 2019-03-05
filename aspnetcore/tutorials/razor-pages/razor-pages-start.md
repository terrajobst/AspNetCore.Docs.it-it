---
title: "Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 81a2a76fc1cecc78b69226fe714d7c9272b04bf7
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899191"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa è la prima esercitazione di una serie. [La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.

[!INCLUDE[](~/includes/advancedRP.md)]

Al termine della serie si otterrà un'app che gestisce un database di film.  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare un'app Web di Razor Pages.
> * Eseguire l'app.
> * Esaminare i file di progetto.

Alla fine di questa esercitazione si avrà un'app Web Razor Pages, che verrà compilata in esercitazioni successive.

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a>Creare un'app Web di Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.

* Creare una nuova applicazione Web ASP.NET Core. Denominare il progetto **RazorPagesMovie**. È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Selezionare **ASP.NET Core 2.2** nell'elenco a discesa e quindi selezionare **Applicazione Web**.

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Viene creato il progetto iniziale seguente:

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.

* Eseguire i comandi seguenti:

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.
  * Il comando `code` visualizza la cartella *RazorPagesMovie* in una nuova istanza di Visual Studio Code.

  Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)

* Selezionare **Sì**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Da un terminale eseguire i comandi seguenti:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.

## <a name="open-the-project"></a>Aprire il progetto

Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a>Eseguire l'app

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Premere CTRL+F5 per l'esecuzione senza il debugger.

  [!INCLUDE[](~/includes/trustCertVS.md)]

  Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app. La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Localhost viene usato solo per le richieste web del computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Premere **CTRL+F5** per l'esecuzione senza il debugger.

  [!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`. La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Localhost viene usato solo per le richieste web del computer locale.
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Per avviare l'app, selezionare **Esegui > Avvia senza eseguire debug**. Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.

[!INCLUDE[](~/includes/trustCertMac.md)]

<!-- End of VS tabs -->

---

* Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.

  Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a>Esaminare i file di progetto

Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.

### <a name="pages-folder"></a>Cartella Pages

Contiene le pagine Razor e i file di supporto. Ogni pagina Razor è una coppia di file:

* Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.
* Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.

I nomi dei file di supporto iniziano con un carattere di sottolineatura. Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine. Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina. Per ulteriori informazioni, vedere <xref:mvc/views/layout>.


### <a name="wwwroot-folder"></a>Cartella wwwroot

Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS. Per ulteriori informazioni, vedere <xref:fundamentals/static-files>.

### <a name="appsettingsjson"></a>appSettings.json

Contiene i dati di configurazione, ad esempio le stringhe di connessione. Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.

### <a name="programcs"></a>Program.cs

Contiene il punto di ingresso per il programma. Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.

### <a name="startupcs"></a>Startup.cs

Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie. Per ulteriori informazioni, vedere <xref:fundamentals/startup>.

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creazione di un'app Web di Razor Pages.
> * Esecuzione dell'app.
> * Esame dei file di progetto.

Passare all'esercitazione successiva della serie:

> [!div class="step-by-step"]
> [Aggiungere un modello](xref:tutorials/razor-pages/model)
