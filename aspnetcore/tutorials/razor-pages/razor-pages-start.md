---
title: Introduzione all'uso di pagine Razor in ASP.NET Core
author: rick-anderson
description: Introduzione all'uso di pagine Razor in ASP.NET Core
keywords: ASP.NET Core,pagine Razor,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: c22ee2554992d15df2f6b92ee5da6805ab80b73a
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a>Introduzione all'uso di pagine Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core. Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:mvc/razor-pages/index) prima di iniziare questa esercitazione. Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

* Scegliere **Nuovo > Progetto** dal menu **File**di Visual Studio.
* Creare una nuova applicazione Web ASP.NET Core. Denominare il progetto **RazorPagesMovie**. È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.
 ![nuova applicazione Web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)
* Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.
 ![Applicazione Web (pagine Razor)](../../mvc/razor-pages/index/_static/np2.png)

Il modello di Visual Studio crea un progetto di avvio:

![Esplora soluzioni](razor-pages-start/_static/se.png)

Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger

![Pagina Home o di indice](razor-pages-start/_static/home.png)

* Visual Studio avvia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app. La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Localhost viene usato solo per le richieste web del computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Nell'immagine precedente, il numero di porta è 5000. Quando si esegue l'app verrà visualizzato un numero di porta diverso.
* Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[Avanti: Aggiunta di un modello](xref:tutorials/razor-pages/modelz)  
