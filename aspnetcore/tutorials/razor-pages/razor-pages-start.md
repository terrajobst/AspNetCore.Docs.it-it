---
title: Introduzione all'uso di pagine Razor in ASP.NET Core
author: rick-anderson
description: Introduzione all'uso di pagine Razor in ASP.NET Core
keywords: ASP.NET Core,pagine Razor,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 8c6e281e761e69908fc742d1f19c14a00de4bd46
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="7ce79-104">Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ce79-104">Getting started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="7ce79-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7ce79-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ce79-106">Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ce79-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="7ce79-107">Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:mvc/razor-pages/index) prima di iniziare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7ce79-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="7ce79-108">Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ce79-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ce79-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7ce79-109">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="7ce79-110">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="7ce79-110">Create a Razor web app</span></span>

* <span data-ttu-id="7ce79-111">Scegliere **Nuovo > Progetto** dal menu **File**di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ce79-111">From the Visual Studio **File** menu, select **New > Project**.</span></span>
* <span data-ttu-id="7ce79-112">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ce79-112">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="7ce79-113">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="7ce79-113">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="7ce79-114">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="7ce79-114">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="7ce79-115">![nuova applicazione Web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="7ce79-115">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="7ce79-116">Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="7ce79-116">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="7ce79-117">![Applicazione Web (pagine Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="7ce79-117">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="7ce79-118">Il modello di Visual Studio crea un progetto di avvio:</span><span class="sxs-lookup"><span data-stu-id="7ce79-118">The Visual Studio template creates a starter project:</span></span>

![Esplora soluzioni](razor-pages-start/_static/se.png)

<span data-ttu-id="7ce79-120">Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger</span><span class="sxs-lookup"><span data-stu-id="7ce79-120">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home.png)

* <span data-ttu-id="7ce79-122">Visual Studio avvia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="7ce79-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="7ce79-123">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="7ce79-123">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7ce79-124">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="7ce79-124">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7ce79-125">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="7ce79-125">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="7ce79-126">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="7ce79-126">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7ce79-127">Nell'immagine precedente, il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="7ce79-127">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="7ce79-128">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="7ce79-128">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7ce79-129">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="7ce79-129">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7ce79-130">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7ce79-130">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="7ce79-131">Avanti: Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="7ce79-131">Next: Adding a model</span></span>](xref:tutorials/razor-pages/modelz)  
