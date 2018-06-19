---
title: Introduzione ad ASP.NET Core MVC e Visual Studio per Mac
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC e Visual Studio
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: ffa620f07251c52c785672d8fbeefacac31ed4c1
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/21/2018
ms.locfileid: "30896640"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Introduzione ad ASP.NET Core MVC e Visual Studio per Mac

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione mostra i concetti fondamentali di creazione di un'app Web ASP.NET Core MVC usando [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/). 

[!INCLUDE [consider RP](../../includes/razor.md)]

Sono disponibili 3 versioni dell'esercitazione:

* macOS: [Compilare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Compilare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* Linux, macOS, e Windows: [Compilare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a>Creare un'app Web

In Visual Studio selezionare **File > Nuova soluzione**.

![Nuova soluzione macOS](../first-web-api-mac/_static/sln.png)

Selezionare **.NET Core App >.ASP.NET Core > App web > Avanti**.

![Finestra di dialogo Nuovo progetto di macOS](start-mvc/1.png)

Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.

![Finestra di dialogo Nuovo progetto di macOS](start-mvc/2.png)

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio selezionare **Esegui > Avvia senza eseguire debug** per avviare l'app. Visual Studio avvia [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.

![Browser con nuovo progetto](start-mvc/b1.png)

* La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Quando si esegue l'app verrà visualizzato un numero di porta diverso.
* È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.

Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**. L'immagine del browser visualizzata sopra non mostra questi collegamenti. A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.

![Browser con Nuovo progetto](start-mvc/b2.png)

Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.

> [!div class="step-by-step"]
> [avanti](adding-controller.md)  
