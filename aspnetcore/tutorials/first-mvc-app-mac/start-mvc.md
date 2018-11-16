---
title: Introduzione ad ASP.NET Core MVC e Visual Studio per Mac
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC e Visual Studio
ms.author: riande
ms.date: 8/23/2017
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 059ac1f7fa94d97adc958be3c0b936cdfa7f6d3e
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225473"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="5dfca-103">Introduzione ad ASP.NET Core MVC e Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="5dfca-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="5dfca-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5dfca-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5dfca-105">Questa esercitazione mostra i concetti fondamentali di creazione di un'app Web ASP.NET Core MVC usando [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="5dfca-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="5dfca-106">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="5dfca-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="5dfca-107">macOS: [Compilare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="5dfca-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="5dfca-108">Windows: [Compilare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="5dfca-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="5dfca-109">Linux, macOS, e Windows: [Compilare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="5dfca-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5dfca-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5dfca-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="5dfca-111">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="5dfca-111">Create a web app</span></span>

<span data-ttu-id="5dfca-112">In Visual Studio selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="5dfca-112">From Visual Studio, select **File > New Solution**.</span></span>

![Nuova soluzione macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="5dfca-114">Selezionare **.NET Core App >.ASP.NET Core > App Web ASP.NET Core (MVC) > Avanti**.</span><span class="sxs-lookup"><span data-stu-id="5dfca-114">Select **.NET Core App >  ASP.NET Core > ASP.NET Core Web App (MVC) > Next**.</span></span>

![Finestra di dialogo Nuovo progetto di macOS](start-mvc/1.png)

<span data-ttu-id="5dfca-116">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="5dfca-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![Finestra di dialogo Nuovo progetto di macOS](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="5dfca-118">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="5dfca-118">Launch the app</span></span>

<span data-ttu-id="5dfca-119">In Visual Studio selezionare **Esegui > Avvia senza eseguire debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="5dfca-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="5dfca-120">Visual Studio avvia [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="5dfca-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Browser con nuovo progetto](start-mvc/b1.png)

* <span data-ttu-id="5dfca-122">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="5dfca-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5dfca-123">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="5dfca-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="5dfca-124">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="5dfca-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="5dfca-125">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="5dfca-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="5dfca-126">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="5dfca-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="5dfca-127">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="5dfca-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="5dfca-128">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="5dfca-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="5dfca-129">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="5dfca-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Browser con Nuovo progetto](start-mvc/b2.png)

<span data-ttu-id="5dfca-131">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="5dfca-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5dfca-132">avanti</span><span class="sxs-lookup"><span data-stu-id="5dfca-132">Next</span></span>](adding-controller.md)  
