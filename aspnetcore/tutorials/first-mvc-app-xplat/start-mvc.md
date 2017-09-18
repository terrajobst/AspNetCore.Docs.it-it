---
title: Introduzione a ASP.NET Core MVC in Mac, Linux o Windows
author: rick-anderson
description: Introduzione ad ASP.NET Core MVC e Visual Studio Code in Mac, Linux e Windows
keywords: ASP.NET Core, MVC, VS Code, Visual Studio Code, Mac, Linux, Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: cfce91271ca21dd800fb68a14389606ce6d835f5
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>Introduzione a ASP.NET Core MVC in Mac, Linux o Windows

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione mostra i concetti fondamentali di compiplazione di un'app Web ASP.NET Core MVC tramite [Visual Studio Code](https://code.visualstudio.com) (VS Code). Nell'esercitazione si presuppone una familarità con Visual Studio Code. Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni. [!INCLUDE[consider RP](../../includes/razor.md)]

Sono disponibili 3 versioni dell'esercitazione:

* macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>Installare VS Code e .NET Core

Questa esercitazione richiede il [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva. Vedere [il pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) per la versione ASP.NET Core 1.1.

Installare gli elementi seguenti:

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [estensione C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

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

  - [Tasti di scelta rapida di Mac](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Tasti di scelta rapida di Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Tasti di scelta rapida di Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[Successivo - Aggiungere un controller](adding-controller.md)
