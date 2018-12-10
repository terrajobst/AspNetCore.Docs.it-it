---
title: Introduzione ad ASP.NET Core MVC e Visual Studio
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC e Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 738c49272c2ae2b075866001f06ad09fe73969f9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862200"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="2523e-103">Introduzione ad ASP.NET Core MVC e Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2523e-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="2523e-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2523e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="2523e-105">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="2523e-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2523e-106">macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2523e-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="2523e-107">Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2523e-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="2523e-108">macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2523e-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

> [!NOTE]
> <span data-ttu-id="2523e-109">È in corso un test di usabilità di una nuova proposta di struttura del sommario di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2523e-109">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="2523e-110">Se si dispone di alcuni minuti per provare a cercare 7 argomenti diversi nel sommario corrente o proposto, [fare clic qui per partecipare al test](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="2523e-110">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="2523e-111">Installare Visual Studio e .NET Core</span><span class="sxs-lookup"><span data-stu-id="2523e-111">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="2523e-112">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="2523e-112">Create a web app</span></span>

<span data-ttu-id="2523e-113">In Visual Studio selezionare **File > Nuovo > Progetto**.</span><span class="sxs-lookup"><span data-stu-id="2523e-113">From Visual Studio, select  **File > New > Project**.</span></span>

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="2523e-115">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="2523e-115">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="2523e-116">Nel riquadro a sinistra toccare **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="2523e-116">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="2523e-117">Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="2523e-117">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="2523e-118">Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).</span><span class="sxs-lookup"><span data-stu-id="2523e-118">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="2523e-119">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="2523e-119">Tap **OK**</span></span>

![<span data-ttu-id="2523e-120">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2523e-120">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="2523e-121">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="2523e-121">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="2523e-122">Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="2523e-122">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="2523e-123">Selezionare **Applicazione Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="2523e-123">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="2523e-124">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="2523e-124">Tap **OK**.</span></span>

![<span data-ttu-id="2523e-125">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2523e-125">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="2523e-126">Visual Studio ha usato un modello predefinito per il progetto MVC appena creato.</span><span class="sxs-lookup"><span data-stu-id="2523e-126">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="2523e-127">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="2523e-127">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="2523e-128">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="2523e-128">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="2523e-129">Toccare **F5** per eseguire l'app in modalità di debug o **CTRL+F5** per eseguirla in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="2523e-129">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="2523e-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![app in esecuzione](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="2523e-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="2523e-131">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="2523e-131">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="2523e-132">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="2523e-132">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2523e-133">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="2523e-133">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="2523e-134">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="2523e-134">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="2523e-135">Nell'immagine precedente il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="2523e-135">In the image above, the port number is 5000.</span></span> <span data-ttu-id="2523e-136">Nell'URL nel browser viene visualizzato `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2523e-136">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="2523e-137">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="2523e-137">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="2523e-138">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="2523e-138">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="2523e-139">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="2523e-139">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="2523e-140">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="2523e-140">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="2523e-142">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="2523e-142">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="2523e-144">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="2523e-144">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="2523e-145">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="2523e-145">The browser image above doesn't show these links.</span></span> <span data-ttu-id="2523e-146">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="2523e-146">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icona di spostamento in alto a destra](start-mvc/_static/2.png)

<span data-ttu-id="2523e-148">Se l'esecuzione è in modalità di debug, toccare **MAIUSC+F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="2523e-148">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="2523e-149">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="2523e-149">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="2523e-150">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="2523e-150">Create a web app</span></span>

<span data-ttu-id="2523e-151">In Visual Studio selezionare **File > Nuovo > Progetto**.</span><span class="sxs-lookup"><span data-stu-id="2523e-151">From Visual Studio, select  **File > New > Project**.</span></span>

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="2523e-153">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="2523e-153">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="2523e-154">Nel riquadro a sinistra toccare **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="2523e-154">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="2523e-155">Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="2523e-155">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="2523e-156">Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).</span><span class="sxs-lookup"><span data-stu-id="2523e-156">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="2523e-157">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="2523e-157">Tap **OK**</span></span>

![<span data-ttu-id="2523e-158">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2523e-158">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

<span data-ttu-id="2523e-159">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="2523e-159">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="2523e-160">Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="2523e-160">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="2523e-161">Selezionare **Web Application (Model-View-Controller)** (Applicazione Web (Model-View-Controller)).</span><span class="sxs-lookup"><span data-stu-id="2523e-161">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="2523e-162">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="2523e-162">Tap **OK**.</span></span>

![<span data-ttu-id="2523e-163">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2523e-163">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

<span data-ttu-id="2523e-164">Visual Studio ha usato un modello predefinito per il progetto MVC appena creato.</span><span class="sxs-lookup"><span data-stu-id="2523e-164">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="2523e-165">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="2523e-165">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="2523e-166">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="2523e-166">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="2523e-167">Toccare **F5** per eseguire l'app in modalità di debug o **CTRL+F5** per eseguirla in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="2523e-167">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="2523e-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![app in esecuzione](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="2523e-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="2523e-169">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="2523e-169">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="2523e-170">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="2523e-170">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2523e-171">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="2523e-171">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="2523e-172">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="2523e-172">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="2523e-173">Nell'immagine precedente il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="2523e-173">In the image above, the port number is 5000.</span></span> <span data-ttu-id="2523e-174">Nell'URL nel browser viene visualizzato `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2523e-174">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="2523e-175">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="2523e-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="2523e-176">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="2523e-176">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="2523e-177">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="2523e-177">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="2523e-178">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="2523e-178">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="2523e-180">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="2523e-180">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="2523e-182">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="2523e-182">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="2523e-183">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="2523e-183">The browser image above doesn't show these links.</span></span> <span data-ttu-id="2523e-184">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="2523e-184">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icona di spostamento in alto a destra](start-mvc/_static/2.png)

<span data-ttu-id="2523e-186">Se l'esecuzione è in modalità di debug, toccare **MAIUSC+F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="2523e-186">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="2523e-187">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="2523e-187">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="2523e-188">avanti</span><span class="sxs-lookup"><span data-stu-id="2523e-188">Next</span></span>](adding-controller.md)  
