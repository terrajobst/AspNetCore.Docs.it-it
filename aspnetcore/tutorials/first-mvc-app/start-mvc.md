---
title: Introduzione ad ASP.NET Core MVC e Visual Studio
author: rick-anderson
description: Introduzione ad ASP.NET Core MVC e Visual Studio
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 72852678a296107e6a2a146ed2324335a6e870ee
ms.sourcegitcommit: 77b8025c30ec2fd46d85ee2a2b497c44435d3009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="a23bb-104">Introduzione ad ASP.NET Core MVC e Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a23bb-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="a23bb-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a23bb-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="a23bb-106">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="a23bb-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a23bb-107">macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a23bb-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="a23bb-108">Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a23bb-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="a23bb-109">macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a23bb-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="a23bb-110">Installare Visual Studio e .NET Core</span><span class="sxs-lookup"><span data-stu-id="a23bb-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23bb-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23bb-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23bb-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23bb-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23bb-113">Installare Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="a23bb-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="a23bb-114">Selezionare il download Community.</span><span class="sxs-lookup"><span data-stu-id="a23bb-114">Select the Community download.</span></span> <span data-ttu-id="a23bb-115">Se Visual Studio 2017 è già installato ignorare questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="a23bb-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="a23bb-116">Home page di installazione di Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a23bb-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="a23bb-117">Eseguire il programma di installazione e selezionare i carichi di lavoro seguenti:</span><span class="sxs-lookup"><span data-stu-id="a23bb-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="a23bb-118">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="a23bb-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="a23bb-119">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="a23bb-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Sviluppo ASP.NET e Web** (in **Web e Cloud**)](start-mvc/_static/web_workload.png)

![**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="a23bb-122">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="a23bb-122">Create a web app</span></span>

<span data-ttu-id="a23bb-123">In Visual Studio selezionare **File > Nuovo > Progetto**.</span><span class="sxs-lookup"><span data-stu-id="a23bb-123">From Visual Studio, select  **File > New > Project**.</span></span>

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="a23bb-125">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="a23bb-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="a23bb-126">Nel riquadro a sinistra toccare **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="a23bb-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="a23bb-127">Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="a23bb-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="a23bb-128">Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).</span><span class="sxs-lookup"><span data-stu-id="a23bb-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="a23bb-129">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="a23bb-129">Tap **OK**</span></span>

![<span data-ttu-id="a23bb-130">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a23bb-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23bb-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23bb-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a23bb-132">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="a23bb-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="a23bb-133">Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="a23bb-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="a23bb-134">Selezionare **Web Application (Model-View-Controller)** (Applicazione Web (Model-View-Controller)).</span><span class="sxs-lookup"><span data-stu-id="a23bb-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="a23bb-135">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="a23bb-135">Tap **OK**.</span></span>

![<span data-ttu-id="a23bb-136">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a23bb-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23bb-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23bb-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23bb-138">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="a23bb-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="a23bb-139">Nella casella di riepilogo a discesa del selettore di versione, selezionare **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="a23bb-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="a23bb-140">Toccare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="a23bb-140">Tap **Web Application**</span></span>
* <span data-ttu-id="a23bb-141">Mantenere l'impostazione predefinita **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="a23bb-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="a23bb-142">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="a23bb-142">Tap **OK**.</span></span>

![Nuova app Web ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="a23bb-144">Visual Studio ha usato un modello predefinito per il progetto MVC appena creato.</span><span class="sxs-lookup"><span data-stu-id="a23bb-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="a23bb-145">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="a23bb-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="a23bb-146">Si tratta di un progetto iniziale semplice che rappresenta un punto di partenza ottimale.</span><span class="sxs-lookup"><span data-stu-id="a23bb-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="a23bb-147">Toccare **F5** per eseguire l'app in modalità di debug o **CTRL+F5** per eseguirla in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="a23bb-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![App in esecuzione](start-mvc/_static/1.png)

* <span data-ttu-id="a23bb-149">Visual Studio avvia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="a23bb-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="a23bb-150">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="a23bb-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a23bb-151">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="a23bb-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a23bb-152">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="a23bb-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a23bb-153">Nell'immagine precedente il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="a23bb-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="a23bb-154">Nell'URL nel browser viene visualizzato `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a23bb-154">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="a23bb-155">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="a23bb-155">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a23bb-156">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="a23bb-156">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a23bb-157">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="a23bb-157">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="a23bb-158">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="a23bb-158">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="a23bb-160">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="a23bb-160">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="a23bb-162">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="a23bb-162">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="a23bb-163">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="a23bb-163">The browser image above doesn't show these links.</span></span> <span data-ttu-id="a23bb-164">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="a23bb-164">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icona di spostamento in alto a destra](start-mvc/_static/2.png)

<span data-ttu-id="a23bb-166">Se l'esecuzione è in modalità di debug, toccare **MAIUSC+F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="a23bb-166">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="a23bb-167">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="a23bb-167">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="a23bb-168">avanti</span><span class="sxs-lookup"><span data-stu-id="a23bb-168">Next</span></span>](adding-controller.md)  
