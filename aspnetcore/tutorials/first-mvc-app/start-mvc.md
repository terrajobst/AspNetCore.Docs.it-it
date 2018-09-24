---
title: Introduzione ad ASP.NET Core MVC e Visual Studio
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC e Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 41f986a06ec46dc025c4e8218745b4a513e8ee2a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011706"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="64132-103">Introduzione ad ASP.NET Core MVC e Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64132-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="64132-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="64132-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="64132-105">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="64132-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="64132-106">macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="64132-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="64132-107">Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="64132-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="64132-108">macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="64132-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="64132-109">Installare Visual Studio e .NET Core</span><span class="sxs-lookup"><span data-stu-id="64132-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="64132-110">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="64132-110">Create a web app</span></span>

<span data-ttu-id="64132-111">In Visual Studio selezionare **File > Nuovo > Progetto**.</span><span class="sxs-lookup"><span data-stu-id="64132-111">From Visual Studio, select  **File > New > Project**.</span></span>

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="64132-113">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="64132-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="64132-114">Nel riquadro a sinistra toccare **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="64132-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="64132-115">Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="64132-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="64132-116">Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).</span><span class="sxs-lookup"><span data-stu-id="64132-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="64132-117">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="64132-117">Tap **OK**</span></span>

![<span data-ttu-id="64132-118">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64132-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="64132-119">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="64132-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="64132-120">Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="64132-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="64132-121">Selezionare **Web Application (Model-View-Controller)** (Applicazione Web (Model-View-Controller)).</span><span class="sxs-lookup"><span data-stu-id="64132-121">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="64132-122">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="64132-122">Tap **OK**.</span></span>

![<span data-ttu-id="64132-123">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64132-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="64132-124">Visual Studio ha usato un modello predefinito per il progetto MVC appena creato.</span><span class="sxs-lookup"><span data-stu-id="64132-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="64132-125">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="64132-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="64132-126">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="64132-126">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="64132-127">Toccare **F5** per eseguire l'app in modalità di debug o **CTRL+F5** per eseguirla in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="64132-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="64132-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![app in esecuzione](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="64132-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="64132-129">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="64132-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="64132-130">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="64132-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="64132-131">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="64132-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="64132-132">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="64132-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="64132-133">Nell'immagine precedente il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="64132-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="64132-134">Nell'URL nel browser viene visualizzato `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="64132-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="64132-135">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="64132-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="64132-136">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="64132-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="64132-137">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="64132-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="64132-138">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="64132-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="64132-140">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="64132-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="64132-142">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="64132-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="64132-143">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="64132-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="64132-144">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="64132-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icona di spostamento in alto a destra](start-mvc/_static/2.png)

<span data-ttu-id="64132-146">Se l'esecuzione è in modalità di debug, toccare **MAIUSC+F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="64132-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="64132-147">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="64132-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="64132-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="64132-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="64132-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="64132-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="64132-150">Installare Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="64132-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="64132-151">Selezionare il download Community.</span><span class="sxs-lookup"><span data-stu-id="64132-151">Select the Community download.</span></span> <span data-ttu-id="64132-152">Se Visual Studio 2017 è già installato ignorare questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="64132-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="64132-153">Home page di installazione di Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="64132-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="64132-154">Eseguire il programma di installazione e selezionare i carichi di lavoro seguenti:</span><span class="sxs-lookup"><span data-stu-id="64132-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="64132-155">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="64132-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="64132-156">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="64132-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Sviluppo ASP.NET e Web** (in **Web e Cloud**)](start-mvc/_static/web_workload.png)

![**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="64132-159">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="64132-159">Create a web app</span></span>

<span data-ttu-id="64132-160">In Visual Studio selezionare **File > Nuovo > Progetto**.</span><span class="sxs-lookup"><span data-stu-id="64132-160">From Visual Studio, select  **File > New > Project**.</span></span>

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="64132-162">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="64132-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="64132-163">Nel riquadro a sinistra toccare **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="64132-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="64132-164">Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="64132-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="64132-165">Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).</span><span class="sxs-lookup"><span data-stu-id="64132-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="64132-166">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="64132-166">Tap **OK**</span></span>

![<span data-ttu-id="64132-167">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64132-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="64132-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="64132-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="64132-169">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="64132-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="64132-170">Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="64132-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="64132-171">Selezionare **Web Application (Model-View-Controller)** (Applicazione Web (Model-View-Controller)).</span><span class="sxs-lookup"><span data-stu-id="64132-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="64132-172">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="64132-172">Tap **OK**.</span></span>

![<span data-ttu-id="64132-173">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64132-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="64132-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="64132-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="64132-175">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="64132-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="64132-176">Nella casella di riepilogo a discesa del selettore di versione, selezionare **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="64132-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="64132-177">Toccare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="64132-177">Tap **Web Application**</span></span>
* <span data-ttu-id="64132-178">Mantenere l'impostazione predefinita **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="64132-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="64132-179">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="64132-179">Tap **OK**.</span></span>

![Nuova app Web ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="64132-181">Visual Studio ha usato un modello predefinito per il progetto MVC appena creato.</span><span class="sxs-lookup"><span data-stu-id="64132-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="64132-182">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="64132-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="64132-183">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="64132-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="64132-184">Toccare **F5** per eseguire l'app in modalità di debug o **CTRL+F5** per eseguirla in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="64132-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="64132-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![app in esecuzione](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="64132-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="64132-186">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="64132-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="64132-187">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="64132-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="64132-188">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="64132-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="64132-189">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="64132-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="64132-190">Nell'immagine precedente il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="64132-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="64132-191">Nell'URL nel browser viene visualizzato `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="64132-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="64132-192">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="64132-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="64132-193">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="64132-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="64132-194">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="64132-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="64132-195">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="64132-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="64132-197">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="64132-197">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="64132-199">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="64132-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="64132-200">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="64132-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="64132-201">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="64132-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icona di spostamento in alto a destra](start-mvc/_static/2.png)

<span data-ttu-id="64132-203">Se l'esecuzione è in modalità di debug, toccare **MAIUSC+F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="64132-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="64132-204">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="64132-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="64132-205">avanti</span><span class="sxs-lookup"><span data-stu-id="64132-205">Next</span></span>](adding-controller.md)  
