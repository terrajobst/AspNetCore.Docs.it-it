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
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="3c0a8-103">Introduzione ad ASP.NET Core MVC in macOS, Linux o Windows</span><span class="sxs-lookup"><span data-stu-id="3c0a8-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="3c0a8-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3c0a8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3c0a8-105">Questa esercitazione mostra i concetti fondamentali di compiplazione di un'app Web ASP.NET Core MVC tramite [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="3c0a8-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="3c0a8-106">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="3c0a8-107">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="3c0a8-108">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="3c0a8-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="3c0a8-109">macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3c0a8-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="3c0a8-110">Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3c0a8-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="3c0a8-111">macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3c0a8-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="3c0a8-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3c0a8-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="3c0a8-113">Creare un'app web con dotnet</span><span class="sxs-lookup"><span data-stu-id="3c0a8-113">Create a web app with dotnet</span></span>

<span data-ttu-id="3c0a8-114">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c0a8-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="3c0a8-115">Aprire la cartella *MvcMovie* in Visual Studio Code (VS Code) e selezionare il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="3c0a8-116">Selezionare **Yes** (Sì) nel messaggio di **Avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "MvcMovie".</span><span class="sxs-lookup"><span data-stu-id="3c0a8-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="3c0a8-117">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-117">Add them?"</span></span>
- <span data-ttu-id="3c0a8-118">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="3c0a8-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "MvcMovie".](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="3c0a8-122">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-122">Press **Debug** (F5) to build and run the program.</span></span>

![App in esecuzione](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="3c0a8-124">VS Code avvia il server web [Kestrel](xref:fundamentals/servers/kestrel) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="3c0a8-125">Si noti che la barra degli indirizzi visualizza `localhost:5000` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="3c0a8-126">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="3c0a8-127">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="3c0a8-128">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="3c0a8-129">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icona di spostamento in alto a destra](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="3c0a8-131">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="3c0a8-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="3c0a8-132">Guida di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c0a8-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="3c0a8-133">Introduzione</span><span class="sxs-lookup"><span data-stu-id="3c0a8-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="3c0a8-134">Debug</span><span class="sxs-lookup"><span data-stu-id="3c0a8-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="3c0a8-135">Terminale integrato</span><span class="sxs-lookup"><span data-stu-id="3c0a8-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="3c0a8-136">Tasti di scelta rapida</span><span class="sxs-lookup"><span data-stu-id="3c0a8-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="3c0a8-137">Tasti di scelta rapida di macOS</span><span class="sxs-lookup"><span data-stu-id="3c0a8-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="3c0a8-138">Tasti di scelta rapida di Linux</span><span class="sxs-lookup"><span data-stu-id="3c0a8-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="3c0a8-139">Tasti di scelta rapida di Windows</span><span class="sxs-lookup"><span data-stu-id="3c0a8-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="3c0a8-140">Successivo - Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="3c0a8-140">Next - Add a controller</span></span>](adding-controller.md)
