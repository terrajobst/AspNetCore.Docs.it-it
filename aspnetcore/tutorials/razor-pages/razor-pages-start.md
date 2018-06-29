---
title: Introduzione all'uso di pagine Razor in ASP.NET Core
author: rick-anderson
description: Informazioni di base sulla compilazione di un'app Web pagine Razor di ASP.NET Core. Pagine Razor è una funzionalità consigliata per carichi di lavoro Web in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d7cdf7c8fac3b2ac1e526c6eeee8205068964ec9
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582817"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="eeba0-104">Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eeba0-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="eeba0-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eeba0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="eeba0-106">È consigliabile usare la versione ASP.NET Core 2.1 di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="eeba0-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="eeba0-107">È **molto** più semplice da seguire e vengono illustrate più funzionalità.</span><span class="sxs-lookup"><span data-stu-id="eeba0-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="eeba0-108">Selezionare **ASP.NET Core 2.1** nel selettore di versione.</span><span class="sxs-lookup"><span data-stu-id="eeba0-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Selettore di versione nel sommario](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="eeba0-110">Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eeba0-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="eeba0-111">Le pagine Razor sono il modo consigliato per creare l'interfaccia utente per app Web in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eeba0-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="eeba0-112">Sono disponibili tre versioni di questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="eeba0-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="eeba0-113">Windows: questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="eeba0-113">Windows: This tutorial</span></span>
* <span data-ttu-id="eeba0-114">MacOS: [Introduzione a pagine Razor con Visual Studio per Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="eeba0-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="eeba0-115">macOS, Linux e Windows: [Introduzione a pagine Razor ASP.NET Core in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="eeba0-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="eeba0-116">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eeba0-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="eeba0-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="eeba0-117">Prerequisites</span></span>

<span data-ttu-id="eeba0-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="eeba0-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="eeba0-119">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="eeba0-119">Create a Razor web app</span></span>

* <span data-ttu-id="eeba0-120">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="eeba0-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="eeba0-121">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eeba0-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="eeba0-122">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="eeba0-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="eeba0-123">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="eeba0-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="eeba0-124">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="eeba0-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="eeba0-125">Selezionare **ASP.NET Core 2.1** nell'elenco a discesa, quindi selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="eeba0-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="eeba0-127">Il modello di Visual Studio crea un progetto di avvio:</span><span class="sxs-lookup"><span data-stu-id="eeba0-127">The Visual Studio template creates a starter project:</span></span>

![Esplora soluzioni](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="eeba0-129">Premere **F5** per eseguire l'app in modalità debug o **CTRL+F5** per eseguirla senza collegarsi al debugger.</span><span class="sxs-lookup"><span data-stu-id="eeba0-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="eeba0-130">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="eeba0-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="eeba0-131">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="eeba0-131">This app doesn't track personal information.</span></span> <span data-ttu-id="eeba0-132">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="eeba0-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Pagina Home o di indice](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="eeba0-134">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="eeba0-134">The following image shows the app after accepting tracking:</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="eeba0-136">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="eeba0-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="eeba0-137">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="eeba0-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="eeba0-138">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="eeba0-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="eeba0-139">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="eeba0-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="eeba0-140">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="eeba0-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="eeba0-141">Nell'immagine precedente, il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="eeba0-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="eeba0-142">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="eeba0-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="eeba0-143">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="eeba0-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="eeba0-144">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="eeba0-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="eeba0-145">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="eeba0-145">Prerequisites</span></span>

<span data-ttu-id="eeba0-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="eeba0-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="eeba0-147">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="eeba0-147">Create a Razor web app</span></span>

* <span data-ttu-id="eeba0-148">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="eeba0-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="eeba0-149">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eeba0-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="eeba0-150">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="eeba0-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="eeba0-151">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="eeba0-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="eeba0-152">![nuova applicazione Web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="eeba0-152">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="eeba0-153">Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="eeba0-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="eeba0-154">Il modello di Visual Studio crea un progetto di avvio:</span><span class="sxs-lookup"><span data-stu-id="eeba0-154">The Visual Studio template creates a starter project:</span></span>

![Esplora soluzioni](razor-pages-start/_static/se.png)

<span data-ttu-id="eeba0-156">Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger</span><span class="sxs-lookup"><span data-stu-id="eeba0-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home.png)

* <span data-ttu-id="eeba0-158">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="eeba0-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="eeba0-159">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="eeba0-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="eeba0-160">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="eeba0-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="eeba0-161">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="eeba0-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="eeba0-162">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="eeba0-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="eeba0-163">Nell'immagine precedente, il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="eeba0-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="eeba0-164">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="eeba0-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="eeba0-165">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="eeba0-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="eeba0-166">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="eeba0-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="eeba0-167">Avanti: Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="eeba0-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
