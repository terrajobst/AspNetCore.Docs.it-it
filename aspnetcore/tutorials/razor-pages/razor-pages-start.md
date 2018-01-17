---
title: Introduzione all'uso di pagine Razor in ASP.NET Core
author: rick-anderson
description: Introduzione all'uso di pagine Razor in ASP.NET Core
keywords: ASP.NET Core, pagine Razor, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 12/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 12685d6a391c127d78399408291c26caa0192ee2
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/21/2017
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Introduzione all'uso di pagine Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core. Le pagine Razor sono il modo consigliato per creare l'interfaccia utente per app Web in ASP.NET Core.

Sono disponibili tre versioni di questa esercitazione:

* Windows: questa esercitazione
* macOS: [Introduzione all'uso di pagine Razor con Visual Studio per Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS, Linux e Windows: [Introduzione all'uso di pagine Razor in ASP.NET Core con Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Creare una nuova applicazione Web ASP.NET Core. Denominare il progetto **RazorPagesMovie**. È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.
  ![nuova applicazione Web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)
* Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.

> [!NOTE]
> Se si vuole usare ASP.NET Core in .NET Framework, è necessario innanzitutto selezionare **.NET Framework** nell'elenco a discesa all'estrema sinistra nella finestra di dialogo, quindi è possibile selezionare la versione di ASP.NET Core desiderata.

  ![Applicazione Web (pagine Razor)](razor-pages-start/_static/np2.png)

Il modello di Visual Studio crea un progetto di avvio:

![Esplora soluzioni](razor-pages-start/_static/se.png)

Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger

![Pagina Home o di indice](razor-pages-start/_static/home.png)

* Visual Studio avvia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app. La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Localhost viene usato solo per le richieste web del computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Nell'immagine precedente, il numero di porta è 5000. Quando si esegue l'app verrà visualizzato un numero di porta diverso.
* Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[Avanti: Aggiunta di un modello](xref:tutorials/razor-pages/model)
