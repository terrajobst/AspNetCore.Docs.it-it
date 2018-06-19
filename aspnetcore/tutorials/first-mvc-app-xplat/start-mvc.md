---
title: Introduzione ad ASP.NET Core MVC in macOS, Linux o Windows
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC e Visual Studio Code in macOS, Linux e Windows
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 50fbd54c6b0cc1146271afda7e45a0dab590dd7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30893560"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>Introduzione ad ASP.NET Core MVC in macOS, Linux o Windows

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione mostra i concetti fondamentali di compiplazione di un'app Web ASP.NET Core MVC tramite [Visual Studio Code](https://code.visualstudio.com) (VS Code). Nell'esercitazione si presuppone una familarità con Visual Studio Code. Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni. 

[!INCLUDE [consider RP](../../includes/razor.md)]

Sono disponibili 3 versioni dell'esercitazione:

* macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a>Creare un'app web con dotnet

Da un terminale eseguire i comandi seguenti:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Aprire la cartella *MvcMovie* in Visual Studio Code (VS Code) e selezionare il file *Startup.cs*.

- Selezionare **Yes** (Sì) nel messaggio di **Avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "MvcMovie". e se si vuole aggiungerle.
- Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "MvcMovie". e se si vuole aggiungerle. Non visualizzare più questo messaggio, Non ora, Sì e anche Informazioni - esistono dipendenze non risolte - Ripristina - Chiudi](../web-api-vsc/_static/vsc_restore.png)

Premere **Debug** (F5) per compilare ed eseguire il programma.

![App in esecuzione](../first-mvc-app/start-mvc/_static/1.png)

VS Code avvia il server web [Kestrel](xref:fundamentals/servers/kestrel) ed esegue l'app. Si noti che la barra degli indirizzi visualizza `localhost:5000` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale.

Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**. L'immagine del browser visualizzata sopra non mostra questi collegamenti. A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.

![Icona di spostamento in alto a destra](../first-mvc-app/start-mvc/_static/2.png)

Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.

## <a name="visual-studio-code-help"></a>Guida di Visual Studio Code

- [Introduzione](https://code.visualstudio.com/docs)
- [Debug](https://code.visualstudio.com/docs/editor/debugging)
- [Terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Tasti di scelta rapida](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Tasti di scelta rapida di macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Tasti di scelta rapida di Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Tasti di scelta rapida di Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Successivo - Aggiungere un controller](adding-controller.md)
