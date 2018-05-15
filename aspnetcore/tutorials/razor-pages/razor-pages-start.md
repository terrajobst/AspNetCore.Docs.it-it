---
title: Introduzione all'uso di pagine Razor in ASP.NET Core
author: rick-anderson
description: Informazioni di base sulla compilazione di un'app Web pagine Razor di ASP.NET Core. Pagine Razor è una funzionalità consigliata per carichi di lavoro Web in ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: caf4376c0a02931eeec85e5067a082b37ef9da68
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="16a8b-104">Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16a8b-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="16a8b-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16a8b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="16a8b-106">Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16a8b-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="16a8b-107">Le pagine Razor sono il modo consigliato per creare l'interfaccia utente per app Web in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16a8b-107">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="16a8b-108">Sono disponibili tre versioni di questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="16a8b-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="16a8b-109">Windows: questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="16a8b-109">Windows: This tutorial</span></span>
* <span data-ttu-id="16a8b-110">MacOS: [Introduzione a pagine Razor con Visual Studio per Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="16a8b-110">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="16a8b-111">macOS, Linux e Windows: [Introduzione a pagine Razor ASP.NET Core in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="16a8b-111">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="16a8b-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16a8b-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16a8b-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="16a8b-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="16a8b-114">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="16a8b-114">Create a Razor web app</span></span>

* <span data-ttu-id="16a8b-115">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="16a8b-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="16a8b-116">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16a8b-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="16a8b-117">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="16a8b-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="16a8b-118">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="16a8b-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="16a8b-119">![nuova applicazione Web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="16a8b-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="16a8b-120">Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="16a8b-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="16a8b-121">Il modello di Visual Studio crea un progetto di avvio:</span><span class="sxs-lookup"><span data-stu-id="16a8b-121">The Visual Studio template creates a starter project:</span></span>

![Esplora soluzioni](razor-pages-start/_static/se.png)

<span data-ttu-id="16a8b-123">Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger</span><span class="sxs-lookup"><span data-stu-id="16a8b-123">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home.png)

* <span data-ttu-id="16a8b-125">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="16a8b-125">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="16a8b-126">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="16a8b-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="16a8b-127">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="16a8b-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="16a8b-128">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="16a8b-128">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="16a8b-129">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="16a8b-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="16a8b-130">Nell'immagine precedente, il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="16a8b-130">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="16a8b-131">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="16a8b-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="16a8b-132">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="16a8b-132">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="16a8b-133">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="16a8b-133">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

> [!div class="step-by-step"]
> [<span data-ttu-id="16a8b-134">Avanti: Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="16a8b-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
