---
title: Introduzione ad ASP.NET Core MVC e Visual Studio
author: rick-anderson
description: Introduzione ad ASP.NET Core MVC e Visual Studio
manager: wpickett
ms.author: riande
ms.date: 10/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: d07d133aa0ed83962b6dc60b9fa0c42993f87843
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="6ff49-103">Introduzione ad ASP.NET Core MVC e Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ff49-103">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="6ff49-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6ff49-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="6ff49-105">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="6ff49-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="6ff49-106">macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="6ff49-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="6ff49-107">Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="6ff49-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="6ff49-108">macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="6ff49-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="6ff49-109">Installare Visual Studio e .NET Core</span><span class="sxs-lookup"><span data-stu-id="6ff49-109">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6ff49-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6ff49-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6ff49-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6ff49-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6ff49-112">Installare Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="6ff49-112">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="6ff49-113">Selezionare il download Community.</span><span class="sxs-lookup"><span data-stu-id="6ff49-113">Select the Community download.</span></span> <span data-ttu-id="6ff49-114">Se Visual Studio 2017 è già installato ignorare questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="6ff49-114">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="6ff49-115">Home page di installazione di Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="6ff49-115">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="6ff49-116">Eseguire il programma di installazione e selezionare i carichi di lavoro seguenti:</span><span class="sxs-lookup"><span data-stu-id="6ff49-116">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="6ff49-117">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="6ff49-117">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="6ff49-118">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="6ff49-118">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Sviluppo ASP.NET e Web** (in **Web e Cloud**)](start-mvc/_static/web_workload.png)

![**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="6ff49-121">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="6ff49-121">Create a web app</span></span>

<span data-ttu-id="6ff49-122">In Visual Studio selezionare **File > Nuovo > Progetto**.</span><span class="sxs-lookup"><span data-stu-id="6ff49-122">From Visual Studio, select  **File > New > Project**.</span></span>

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="6ff49-124">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="6ff49-124">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="6ff49-125">Nel riquadro a sinistra toccare **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="6ff49-125">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="6ff49-126">Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="6ff49-126">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="6ff49-127">Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).</span><span class="sxs-lookup"><span data-stu-id="6ff49-127">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="6ff49-128">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ff49-128">Tap **OK**</span></span>

![<span data-ttu-id="6ff49-129">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ff49-129">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6ff49-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6ff49-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6ff49-131">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="6ff49-131">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="6ff49-132">Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="6ff49-132">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="6ff49-133">Selezionare **Web Application (Model-View-Controller)** (Applicazione Web (Model-View-Controller)).</span><span class="sxs-lookup"><span data-stu-id="6ff49-133">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="6ff49-134">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ff49-134">Tap **OK**.</span></span>

![<span data-ttu-id="6ff49-135">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ff49-135">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6ff49-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6ff49-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6ff49-137">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="6ff49-137">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="6ff49-138">Nella casella di riepilogo a discesa del selettore di versione, selezionare **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="6ff49-138">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="6ff49-139">Toccare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="6ff49-139">Tap **Web Application**</span></span>
* <span data-ttu-id="6ff49-140">Mantenere l'impostazione predefinita **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="6ff49-140">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="6ff49-141">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ff49-141">Tap **OK**.</span></span>

![Nuova app Web ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="6ff49-143">Visual Studio ha usato un modello predefinito per il progetto MVC appena creato.</span><span class="sxs-lookup"><span data-stu-id="6ff49-143">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="6ff49-144">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="6ff49-144">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="6ff49-145">Si tratta di un progetto iniziale semplice che rappresenta un punto di partenza ottimale.</span><span class="sxs-lookup"><span data-stu-id="6ff49-145">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="6ff49-146">Toccare **F5** per eseguire l'app in modalità di debug o **CTRL+F5** per eseguirla in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="6ff49-146">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![App in esecuzione](start-mvc/_static/1.png)

* <span data-ttu-id="6ff49-148">Visual Studio avvia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="6ff49-148">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="6ff49-149">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="6ff49-149">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="6ff49-150">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="6ff49-150">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="6ff49-151">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="6ff49-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="6ff49-152">Nell'immagine precedente il numero di porta è 5000.</span><span class="sxs-lookup"><span data-stu-id="6ff49-152">In the image above, the port number is 5000.</span></span> <span data-ttu-id="6ff49-153">Nell'URL nel browser viene visualizzato `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6ff49-153">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="6ff49-154">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="6ff49-154">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="6ff49-155">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="6ff49-155">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="6ff49-156">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="6ff49-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="6ff49-157">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="6ff49-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="6ff49-159">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="6ff49-159">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="6ff49-161">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="6ff49-161">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="6ff49-162">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="6ff49-162">The browser image above doesn't show these links.</span></span> <span data-ttu-id="6ff49-163">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="6ff49-163">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icona di spostamento in alto a destra](start-mvc/_static/2.png)

<span data-ttu-id="6ff49-165">Se l'esecuzione è in modalità di debug, toccare **MAIUSC+F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="6ff49-165">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="6ff49-166">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="6ff49-166">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="6ff49-167">avanti</span><span class="sxs-lookup"><span data-stu-id="6ff49-167">Next</span></span>](adding-controller.md)  
