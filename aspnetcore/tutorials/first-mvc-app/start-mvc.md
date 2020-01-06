---
title: Introduzione ad ASP.NET Core MVC
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: e70384a6f20f3ef06059ed6b51c76e923187c317
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354931"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="e1101-103">Introduzione ad ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e1101-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="e1101-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e1101-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="e1101-105">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e1101-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="e1101-106">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="e1101-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="e1101-107">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="e1101-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e1101-108">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="e1101-108">Create a web app.</span></span>
> * <span data-ttu-id="e1101-109">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="e1101-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="e1101-110">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="e1101-110">Work with a database.</span></span>
> * <span data-ttu-id="e1101-111">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="e1101-111">Add search and validation.</span></span>

<span data-ttu-id="e1101-112">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="e1101-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="e1101-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e1101-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1101-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1101-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1101-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1101-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1101-116">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1101-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="e1101-117">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="e1101-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1101-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1101-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1101-119">Dal menu di Visual Studio selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="e1101-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="e1101-120">Selezionare **Applicazione Web ASP.NET Core** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e1101-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![nuova applicazione Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="e1101-122">Assegnare al progetto il nome **MvcMovie** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1101-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="e1101-123">È importante assegnare al progetto il nome **MvcMovie**, in modo che quando si copia il codice lo spazio dei nomi corrisponda.</span><span class="sxs-lookup"><span data-stu-id="e1101-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![nuova applicazione Web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="e1101-125">Selezionare **Applicazione Web (MVC)** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1101-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="e1101-126">Finestra di dialogo Nuovo progetto, .NET Core nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1101-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="e1101-127">Per il progetto MVC appena creato Visual Studio ha usato il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="e1101-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="e1101-128">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="e1101-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e1101-129">Si tratta di un progetto iniziale di base.</span><span class="sxs-lookup"><span data-stu-id="e1101-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1101-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1101-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1101-131">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1101-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="e1101-132">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="e1101-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="e1101-133">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e1101-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e1101-134">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="e1101-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="e1101-135">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1101-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="e1101-136">Viene visualizzata una finestra di dialogo con le **risorse necessarie per la compilazione e il debug non è presente in ' MvcMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="e1101-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="e1101-137">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="e1101-137">Select **Yes**</span></span>

  * <span data-ttu-id="e1101-138">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="e1101-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="e1101-139">`code -r MvcMovie`: carica il file di progetto *MvcMovie. csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1101-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1101-140">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1101-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e1101-141">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e1101-141">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="e1101-143">Selezionare l'applicazione Web **.NET Core** > **app** > **(Model-View-Controller)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e1101-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="e1101-145">Nella finestra di dialogo **configurare la nuova ASP.NET Core API Web** impostare il **Framework di destinazione** di **.NET Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="e1101-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.1**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="e1101-146">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1101-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="e1101-147">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="e1101-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1101-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1101-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1101-149">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="e1101-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="e1101-150">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="e1101-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e1101-151">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="e1101-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e1101-152">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1101-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e1101-153">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="e1101-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="e1101-154">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="e1101-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e1101-155">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e1101-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e1101-156">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="e1101-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Debug - menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e1101-158">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="e1101-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="e1101-160">La figura seguente mostra l'app:</span><span class="sxs-lookup"><span data-stu-id="e1101-160">The following image shows the app:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1101-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1101-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1101-163">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="e1101-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="e1101-164">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e1101-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="e1101-165">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="e1101-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="e1101-166">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1101-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="e1101-167">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1101-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="e1101-168">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="e1101-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e1101-169">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e1101-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1101-171">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1101-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e1101-172">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="e1101-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="e1101-173">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="e1101-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="e1101-174">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="e1101-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e1101-175">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1101-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e1101-176">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="e1101-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e1101-177">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="e1101-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e1101-178">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="e1101-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="e1101-179">La figura seguente mostra l'app:</span><span class="sxs-lookup"><span data-stu-id="e1101-179">The following image shows the app:</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="e1101-181">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="e1101-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e1101-182">Successivo</span><span class="sxs-lookup"><span data-stu-id="e1101-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="e1101-183">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e1101-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="e1101-184">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="e1101-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="e1101-185">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="e1101-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e1101-186">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="e1101-186">Create a web app.</span></span>
> * <span data-ttu-id="e1101-187">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="e1101-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="e1101-188">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="e1101-188">Work with a database.</span></span>
> * <span data-ttu-id="e1101-189">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="e1101-189">Add search and validation.</span></span>

<span data-ttu-id="e1101-190">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="e1101-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="e1101-191">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e1101-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1101-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1101-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1101-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1101-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1101-194">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1101-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="e1101-195">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="e1101-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1101-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1101-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1101-197">Dal menu di Visual Studio selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="e1101-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="e1101-198">Selezionare **Applicazione Web ASP.NET Core** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e1101-198">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![nuova applicazione Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="e1101-200">Assegnare al progetto il nome **MvcMovie** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1101-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="e1101-201">È importante assegnare al progetto il nome **MvcMovie**, in modo che quando si copia il codice lo spazio dei nomi corrisponda.</span><span class="sxs-lookup"><span data-stu-id="e1101-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![nuova applicazione Web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="e1101-203">Selezionare **Applicazione Web (MVC)** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1101-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="e1101-204">Finestra di dialogo Nuovo progetto, .NET Core nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1101-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="e1101-205">Per il progetto MVC appena creato Visual Studio ha usato il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="e1101-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="e1101-206">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="e1101-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e1101-207">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="e1101-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1101-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1101-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1101-209">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1101-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="e1101-210">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="e1101-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="e1101-211">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e1101-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e1101-212">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="e1101-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="e1101-213">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1101-213">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="e1101-214">Viene visualizzata una finestra di dialogo con le **risorse necessarie per la compilazione e il debug non è presente in ' MvcMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="e1101-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="e1101-215">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="e1101-215">Select **Yes**</span></span>

  * <span data-ttu-id="e1101-216">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="e1101-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="e1101-217">`code -r MvcMovie`: carica il file di progetto *MvcMovie. csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1101-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1101-218">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1101-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e1101-219">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e1101-219">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="e1101-221">Selezionare l'applicazione Web **.NET Core** > **app** > **(Model-View-Controller)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e1101-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="e1101-223">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** accettare l'impostazione predefinita di **Framework di destinazione**, ovvero **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="e1101-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Selezione di .NET Core 2.2 per macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="e1101-225">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1101-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="e1101-226">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="e1101-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1101-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1101-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1101-228">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="e1101-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="e1101-229">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="e1101-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e1101-230">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="e1101-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e1101-231">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1101-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e1101-232">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="e1101-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="e1101-233">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="e1101-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e1101-234">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e1101-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e1101-235">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="e1101-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Debug - menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e1101-237">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="e1101-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="e1101-239">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="e1101-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e1101-240">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="e1101-240">This app doesn't track personal information.</span></span> <span data-ttu-id="e1101-241">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e1101-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="e1101-243">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="e1101-243">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1101-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1101-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1101-246">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="e1101-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="e1101-247">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e1101-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="e1101-248">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="e1101-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="e1101-249">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1101-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="e1101-250">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1101-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="e1101-251">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="e1101-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e1101-252">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e1101-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="e1101-253">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="e1101-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e1101-254">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="e1101-254">This app doesn't track personal information.</span></span> <span data-ttu-id="e1101-255">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e1101-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="e1101-257">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="e1101-257">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1101-259">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1101-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e1101-260">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="e1101-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="e1101-261">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="e1101-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="e1101-262">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="e1101-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e1101-263">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1101-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e1101-264">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="e1101-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e1101-265">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="e1101-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e1101-266">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="e1101-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="e1101-267">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="e1101-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e1101-268">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="e1101-268">This app doesn't track personal information.</span></span> <span data-ttu-id="e1101-269">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e1101-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="e1101-271">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="e1101-271">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="e1101-273">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="e1101-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e1101-274">Successivo</span><span class="sxs-lookup"><span data-stu-id="e1101-274">Next</span></span>](adding-controller.md)

::: moniker-end
