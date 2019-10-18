---
title: Introduzione ad ASP.NET Core MVC
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f07afb15c0a31110c20d96a5db5c02030cefe5d5
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519100"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="85971-103">Introduzione ad ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="85971-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="85971-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="85971-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="85971-105">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="85971-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="85971-106">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="85971-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="85971-107">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="85971-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="85971-108">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="85971-108">Create a web app.</span></span>
> * <span data-ttu-id="85971-109">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="85971-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="85971-110">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="85971-110">Work with a database.</span></span>
> * <span data-ttu-id="85971-111">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="85971-111">Add search and validation.</span></span>

<span data-ttu-id="85971-112">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="85971-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="85971-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="85971-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="85971-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85971-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="85971-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="85971-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="85971-116">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="85971-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="85971-117">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="85971-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="85971-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85971-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="85971-119">Dal menu di Visual Studio selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="85971-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="85971-120">Selezionare **Applicazione Web ASP.NET Core** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="85971-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![nuova applicazione Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="85971-122">Assegnare al progetto il nome **MvcMovie** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="85971-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="85971-123">È importante assegnare al progetto il nome **MvcMovie**, in modo che quando si copia il codice lo spazio dei nomi corrisponda.</span><span class="sxs-lookup"><span data-stu-id="85971-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![nuova applicazione Web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="85971-125">Selezionare **Applicazione Web (MVC)** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="85971-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="85971-126">Finestra di dialogo Nuovo progetto, .NET Core nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="85971-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="85971-127">Per il progetto MVC appena creato Visual Studio ha usato il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="85971-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="85971-128">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="85971-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="85971-129">Si tratta di un progetto iniziale di base.</span><span class="sxs-lookup"><span data-stu-id="85971-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="85971-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="85971-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="85971-131">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="85971-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="85971-132">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="85971-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="85971-133">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="85971-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="85971-134">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="85971-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="85971-135">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="85971-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="85971-136">Viene visualizzata una finestra di dialogo con le **risorse necessarie per la compilazione e il debug non è presente in ' MvcMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="85971-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="85971-137">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="85971-137">Select **Yes**</span></span>

  * <span data-ttu-id="85971-138">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="85971-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="85971-139">`code -r MvcMovie`: carica il file di progetto *MvcMovie. csproj* nel Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="85971-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="85971-140">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="85971-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="85971-141">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="85971-141">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="85971-143">Selezionare **.NET Core** > **App** > **Applicazione Web (Model-View-Controller)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="85971-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="85971-145">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** impostare **Framework di destinazione** su **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="85971-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="85971-146">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="85971-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="85971-147">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="85971-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="85971-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85971-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="85971-149">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="85971-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="85971-150">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="85971-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="85971-151">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="85971-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="85971-152">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="85971-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="85971-153">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="85971-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="85971-154">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="85971-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="85971-155">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="85971-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="85971-156">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="85971-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="85971-158">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="85971-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="85971-160">La figura seguente mostra l'app:</span><span class="sxs-lookup"><span data-stu-id="85971-160">The following image shows the app:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="85971-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="85971-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="85971-163">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="85971-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="85971-164">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="85971-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="85971-165">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="85971-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="85971-166">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="85971-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="85971-167">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="85971-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="85971-168">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="85971-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="85971-169">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="85971-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="85971-171">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="85971-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="85971-172">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="85971-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="85971-173">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="85971-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="85971-174">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="85971-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="85971-175">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="85971-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="85971-176">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="85971-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="85971-177">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="85971-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="85971-178">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="85971-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="85971-179">La figura seguente mostra l'app:</span><span class="sxs-lookup"><span data-stu-id="85971-179">The following image shows the app:</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="85971-181">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="85971-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="85971-182">avanti</span><span class="sxs-lookup"><span data-stu-id="85971-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="85971-183">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="85971-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="85971-184">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="85971-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="85971-185">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="85971-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="85971-186">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="85971-186">Create a web app.</span></span>
> * <span data-ttu-id="85971-187">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="85971-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="85971-188">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="85971-188">Work with a database.</span></span>
> * <span data-ttu-id="85971-189">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="85971-189">Add search and validation.</span></span>

<span data-ttu-id="85971-190">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="85971-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="85971-191">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="85971-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="85971-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85971-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="85971-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="85971-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="85971-194">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="85971-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="85971-195">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="85971-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="85971-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85971-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="85971-197">Dal menu di Visual Studio selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="85971-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="85971-198">Selezionare **Applicazione Web ASP.NET Core** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="85971-198">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![nuova applicazione Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="85971-200">Assegnare al progetto il nome **MvcMovie** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="85971-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="85971-201">È importante assegnare al progetto il nome **MvcMovie**, in modo che quando si copia il codice lo spazio dei nomi corrisponda.</span><span class="sxs-lookup"><span data-stu-id="85971-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![nuova applicazione Web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="85971-203">Selezionare **Applicazione Web (MVC)** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="85971-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="85971-204">Finestra di dialogo Nuovo progetto, .NET Core nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="85971-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="85971-205">Per il progetto MVC appena creato Visual Studio ha usato il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="85971-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="85971-206">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="85971-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="85971-207">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="85971-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="85971-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="85971-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="85971-209">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="85971-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="85971-210">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="85971-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="85971-211">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="85971-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="85971-212">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="85971-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="85971-213">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="85971-213">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="85971-214">Viene visualizzata una finestra di dialogo con le **risorse necessarie per la compilazione e il debug non è presente in ' MvcMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="85971-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="85971-215">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="85971-215">Select **Yes**</span></span>

  * <span data-ttu-id="85971-216">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="85971-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="85971-217">`code -r MvcMovie`: carica il file di progetto *MvcMovie. csproj* nel Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="85971-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="85971-218">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="85971-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="85971-219">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="85971-219">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="85971-221">Selezionare **.NET Core** > **App** > **Applicazione Web (Model-View-Controller)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="85971-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="85971-223">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** accettare l'impostazione predefinita di **Framework di destinazione**, ovvero **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="85971-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Selezione di .NET Core 2.2 per macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="85971-225">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="85971-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="85971-226">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="85971-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="85971-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85971-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="85971-228">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="85971-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="85971-229">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="85971-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="85971-230">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="85971-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="85971-231">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="85971-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="85971-232">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="85971-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="85971-233">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="85971-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="85971-234">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="85971-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="85971-235">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="85971-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="85971-237">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="85971-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="85971-239">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="85971-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="85971-240">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="85971-240">This app doesn't track personal information.</span></span> <span data-ttu-id="85971-241">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="85971-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="85971-243">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="85971-243">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="85971-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="85971-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="85971-246">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="85971-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="85971-247">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="85971-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="85971-248">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="85971-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="85971-249">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="85971-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="85971-250">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="85971-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="85971-251">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="85971-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="85971-252">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="85971-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="85971-253">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="85971-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="85971-254">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="85971-254">This app doesn't track personal information.</span></span> <span data-ttu-id="85971-255">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="85971-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="85971-257">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="85971-257">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="85971-259">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="85971-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="85971-260">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="85971-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="85971-261">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="85971-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="85971-262">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="85971-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="85971-263">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="85971-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="85971-264">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="85971-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="85971-265">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="85971-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="85971-266">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="85971-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="85971-267">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="85971-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="85971-268">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="85971-268">This app doesn't track personal information.</span></span> <span data-ttu-id="85971-269">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="85971-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="85971-271">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="85971-271">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="85971-273">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="85971-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="85971-274">avanti</span><span class="sxs-lookup"><span data-stu-id="85971-274">Next</span></span>](adding-controller.md)

::: moniker-end
