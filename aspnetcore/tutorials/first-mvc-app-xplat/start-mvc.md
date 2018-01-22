---
title: Introduzione a ASP.NET Core MVC in Mac, Linux o Windows
author: rick-anderson
description: Introduzione ad ASP.NET Core MVC e Visual Studio Code in Mac, Linux e Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: f439b6414d95f6edd1c2201c8aee043f1eab9b76
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="2736b-103">Introduzione a ASP.NET Core MVC in Mac, Linux o Windows</span><span class="sxs-lookup"><span data-stu-id="2736b-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="2736b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2736b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2736b-105">Questa esercitazione mostra i concetti fondamentali di compiplazione di un'app Web ASP.NET Core MVC tramite [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="2736b-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="2736b-106">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2736b-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="2736b-107">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="2736b-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="2736b-108">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="2736b-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2736b-109">macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2736b-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="2736b-110">Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2736b-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="2736b-111">macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2736b-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="2736b-112">Installare VS Code e .NET Core</span><span class="sxs-lookup"><span data-stu-id="2736b-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="2736b-113">Questa esercitazione richiede il [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="2736b-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="2736b-114">Vedere [il pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) per la versione ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="2736b-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="2736b-115">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2736b-115">Install the following:</span></span>

* <span data-ttu-id="2736b-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="2736b-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="2736b-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2736b-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="2736b-118">VS Code [estensione C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="2736b-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="2736b-119">Creare un'app web con dotnet</span><span class="sxs-lookup"><span data-stu-id="2736b-119">Create a web app with dotnet</span></span>

<span data-ttu-id="2736b-120">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2736b-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="2736b-121">Aprire la cartella *MvcMovie* in Visual Studio Code (VS Code) e selezionare il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2736b-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="2736b-122">Selezionare **Yes** (Sì) nel messaggio di **Avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "MvcMovie".</span><span class="sxs-lookup"><span data-stu-id="2736b-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="2736b-123">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="2736b-123">Add them?"</span></span>
- <span data-ttu-id="2736b-124">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="2736b-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "MvcMovie".](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="2736b-128">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="2736b-128">Press **Debug** (F5) to build and run the program.</span></span>

![App in esecuzione](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="2736b-130">VS Code avvia il server web [Kestrel](xref:fundamentals/servers/kestrel) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="2736b-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="2736b-131">Si noti che la barra degli indirizzi visualizza `localhost:5000` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="2736b-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="2736b-132">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="2736b-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="2736b-133">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="2736b-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="2736b-134">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="2736b-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="2736b-135">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="2736b-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icona di spostamento in alto a destra](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="2736b-137">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="2736b-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="2736b-138">Guida di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2736b-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="2736b-139">Introduzione</span><span class="sxs-lookup"><span data-stu-id="2736b-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="2736b-140">Debug</span><span class="sxs-lookup"><span data-stu-id="2736b-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="2736b-141">Terminale integrato</span><span class="sxs-lookup"><span data-stu-id="2736b-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="2736b-142">Tasti di scelta rapida</span><span class="sxs-lookup"><span data-stu-id="2736b-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="2736b-143">Tasti di scelta rapida di Mac</span><span class="sxs-lookup"><span data-stu-id="2736b-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="2736b-144">Tasti di scelta rapida di Linux</span><span class="sxs-lookup"><span data-stu-id="2736b-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="2736b-145">Tasti di scelta rapida di Windows</span><span class="sxs-lookup"><span data-stu-id="2736b-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="2736b-146">Successivo - Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="2736b-146">Next - Add a controller</span></span>](adding-controller.md)
