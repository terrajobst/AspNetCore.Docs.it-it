---
title: Introduzione all'uso di pagine Razor in ASP.NET Core
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: ca5b134aaa0a9218bc3b0466c3448bd41e8cef8b
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450736"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-2.0"

È consigliabile usare la versione ASP.NET Core 2.1 di questa esercitazione. È **molto** più semplice da seguire e vengono illustrate più funzionalità. Selezionare **ASP.NET Core 2.1** nel selettore di versione.

![Selettore di versione nel sommario](razor-pages-start/_static/v21.png)

::: moniker-end

Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core. Le pagine Razor sono il modo consigliato per creare l'interfaccia utente per app Web in ASP.NET Core.

Sono disponibili tre versioni di questa esercitazione:

* Windows: questa esercitazione
* macOS: [Introduzione a Razor Pages con Visual Studio per Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS, Linux e Windows: [Introduzione a pagine Razor ASP.NET Core in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

> [!NOTE]
> È in corso un test di usabilità di una nuova proposta di struttura del sommario di ASP.NET Core.  Se si dispone di alcuni minuti per provare a cercare 7 argomenti diversi nel sommario corrente o proposto, [fare clic qui per partecipare al test](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Creare una nuova applicazione Web ASP.NET Core. Denominare il progetto **RazorPagesMovie**. È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.
 ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)
* Selezionare **ASP.NET Core 2.1** nell'elenco a discesa, quindi selezionare **Applicazione Web**.

 ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

Il modello di Visual Studio crea un progetto di avvio:

![Esplora soluzioni](razor-pages-start/_static/se2.1.png)

Premere **F5** per eseguire l'app in modalità debug o **CTRL+F5** per eseguirla senza collegarsi al debugger. Selezionare **Accept** (Accetto) per autorizzare il rilevamento. Questa app non rileva informazioni personali. Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).

![Pagina Home o di indice](razor-pages-start/_static/homeGDPR.png)

La figura seguente illustra l'app dopo aver accettato il rilevamento:

![Pagina Home o di indice](razor-pages-start/_static/home2.1.png)

* Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app. La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Localhost viene usato solo per le richieste web del computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Nell'immagine precedente, il numero di porta è 5001. Quando si esegue l'app verrà visualizzato un numero di porta diverso.
* Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Creare una nuova applicazione Web ASP.NET Core. Denominare il progetto **RazorPagesMovie**. È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.
  ![nuova applicazione Web ASP.NET Core](../../razor-pages/index/_static/np.png)
* Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

Il modello di Visual Studio crea un progetto di avvio:

![Esplora soluzioni](razor-pages-start/_static/se.png)

Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger

![Pagina Home o di indice](razor-pages-start/_static/home.png)

* Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app. La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Localhost viene usato solo per le richieste web del computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Nell'immagine precedente, il numero di porta è 5000. Quando si esegue l'app verrà visualizzato un numero di porta diverso.
* Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [Avanti: Aggiunta di un modello](xref:tutorials/razor-pages/model)
