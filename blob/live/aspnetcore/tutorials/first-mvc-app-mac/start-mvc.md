---
title: Introduzione ad ASP.NET Core MVC e Visual Studio per Mac
author: rick-anderson
description: Introduzione ad ASP.NET Core MVC e Visual Studio
keywords: ASP.NET Core,MVC, Visual Studio per Mac, Entity Framework
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 21f115eec924d5e4b21ad78398c8cbd99e02a0a8
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="e9f15-104">Introduzione ad ASP.NET Core MVC e Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e9f15-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="e9f15-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e9f15-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e9f15-106">Questa esercitazione mostra i concetti fondamentali di creazione di un'app Web ASP.NET Core MVC usando [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="e9f15-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="e9f15-107">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="e9f15-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e9f15-108">macOS: [Compilare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e9f15-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="e9f15-109">Windows: [Compilare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e9f15-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="e9f15-110">Linux, macOS, e Windows: [Compilare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e9f15-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9f15-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e9f15-111">Prerequisites</span></span>

<span data-ttu-id="e9f15-112">Questa esercitazione richiede il [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="e9f15-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="e9f15-113">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e9f15-113">Install the following:</span></span>

- <span data-ttu-id="e9f15-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="e9f15-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="e9f15-115">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e9f15-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="e9f15-116">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="e9f15-116">Create a web app</span></span>

<span data-ttu-id="e9f15-117">In Visual Studio selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e9f15-117">From Visual Studio, select **File > New Solution**.</span></span>

![Nuova soluzione macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="e9f15-119">Selezionare **.NET Core App >.ASP.NET Core > App web > Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e9f15-119">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![Finestra di dialogo Nuovo progetto di macOS](start-mvc/1.png)

<span data-ttu-id="e9f15-121">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e9f15-121">Name the project **MvcMovie**, and then select **Create**.</span></span>

![Finestra di dialogo Nuovo progetto di macOS](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="e9f15-123">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="e9f15-123">Launch the app</span></span>

<span data-ttu-id="e9f15-124">In Visual Studio selezionare **Esegui > Avvia senza eseguire debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="e9f15-124">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="e9f15-125">Visual Studio avvia [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="e9f15-125">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Browser con nuovo progetto](start-mvc/b1.png)

* <span data-ttu-id="e9f15-127">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="e9f15-127">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e9f15-128">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="e9f15-128">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e9f15-129">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="e9f15-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e9f15-130">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="e9f15-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e9f15-131">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="e9f15-131">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="e9f15-132">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="e9f15-132">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e9f15-133">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="e9f15-133">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e9f15-134">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="e9f15-134">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Browser con Nuovo progetto](start-mvc/b2.png)

<span data-ttu-id="e9f15-136">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="e9f15-136">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e9f15-137">Successivo</span><span class="sxs-lookup"><span data-stu-id="e9f15-137">Next</span></span>](adding-controller.md)  
