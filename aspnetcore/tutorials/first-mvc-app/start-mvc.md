---
title: Introduzione ad ASP.NET Core MVC e Visual Studio
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC e Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 1fb3947023843341403f4355c6ae1e61d7e4f6b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38217978"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>Introduzione ad ASP.NET Core MVC e Visual Studio

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Sono disponibili 3 versioni dell'esercitazione:

* macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Installare Visual Studio e .NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Creare un'app Web

In Visual Studio selezionare **File > Nuovo > Progetto**.

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

Completare la finestra di dialogo **Nuovo progetto**:

* Nel riquadro a sinistra toccare **.NET Core**
* Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**
* Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).
* Toccare **OK**.

![Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:

* Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.1**
* Selezionare **Web Application (Model-View-Controller)** (Applicazione Web (Model-View-Controller)).
* Toccare **OK**.

![Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio ha usato un modello predefinito per il progetto MVC appena creato. Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni. Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.

Toccare **F5** per eseguire l'app in modalità di debug o **CTRL+F5** per eseguirla in modalità non di debug.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![app in esecuzione](start-mvc/_static/1.png)

* Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app. Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Nell'immagine precedente il numero di porta è 5000. Nell'URL nel browser viene visualizzato `localhost:5000`. Quando si esegue l'app verrà visualizzato un numero di porta diverso.
* Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.
* È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:

![Menu Debug](start-mvc/_static/debug_menu.png)

* È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.

![IIS Express](start-mvc/_static/iis_express.png)

Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**. L'immagine del browser visualizzata sopra non mostra questi collegamenti. A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.

![Icona di spostamento in alto a destra](start-mvc/_static/2.png)

Se l'esecuzione è in modalità di debug, toccare **MAIUSC+F5** per arrestare il debug.

Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Installare Visual Studio Community 2017. Selezionare il download Community. Se Visual Studio 2017 è già installato ignorare questo passaggio.

* [Home page di installazione di Visual Studio 2017](https://www.visualstudio.com/)

Eseguire il programma di installazione e selezionare i carichi di lavoro seguenti:

* **Sviluppo ASP.NET e Web** (in **Web e Cloud**)
* **Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)

![**Sviluppo ASP.NET e Web** (in **Web e Cloud**)](start-mvc/_static/web_workload.png)

![**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Creare un'app Web

In Visual Studio selezionare **File > Nuovo > Progetto**.

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

Completare la finestra di dialogo **Nuovo progetto**:

* Nel riquadro a sinistra toccare **.NET Core**
* Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**
* Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).
* Toccare **OK**.

![Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:

* Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.-**
* Selezionare **Web Application (Model-View-Controller)** (Applicazione Web (Model-View-Controller)).
* Toccare **OK**.

![Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:

* Nella casella di riepilogo a discesa del selettore di versione, selezionare **ASP.NET Core 1.1**
* Toccare **Applicazione Web**.
* Mantenere l'impostazione predefinita **Nessuna autenticazione**.
* Toccare **OK**.

![Nuova app Web ASP.NET Core](start-mvc/_static/p3.png)

---

Visual Studio ha usato un modello predefinito per il progetto MVC appena creato. Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni. Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.

Toccare **F5** per eseguire l'app in modalità di debug o **CTRL+F5** per eseguirla in modalità non di debug.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![app in esecuzione](start-mvc/_static/1.png)

* Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app. Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Nell'immagine precedente il numero di porta è 5000. Nell'URL nel browser viene visualizzato `localhost:5000`. Quando si esegue l'app verrà visualizzato un numero di porta diverso.
* Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.
* È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:

![Menu Debug](start-mvc/_static/debug_menu.png)

* È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.

![IIS Express](start-mvc/_static/iis_express.png)

Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**. L'immagine del browser visualizzata sopra non mostra questi collegamenti. A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.

![Icona di spostamento in alto a destra](start-mvc/_static/2.png)

Se l'esecuzione è in modalità di debug, toccare **MAIUSC+F5** per arrestare il debug.

Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.

::: moniker-end
> [!div class="step-by-step"]
> [avanti](adding-controller.md)  
