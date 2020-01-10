---
title: Introduzione ad ASP.NET Core MVC
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 901257efdfbc7b36249233745175f5ed253da2c7
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722874"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="4e51f-103">Introduzione ad ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="4e51f-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="4e51f-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4e51f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="4e51f-105">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="4e51f-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="4e51f-106">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="4e51f-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="4e51f-107">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="4e51f-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4e51f-108">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="4e51f-108">Create a web app.</span></span>
> * <span data-ttu-id="4e51f-109">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="4e51f-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="4e51f-110">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="4e51f-110">Work with a database.</span></span>
> * <span data-ttu-id="4e51f-111">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="4e51f-111">Add search and validation.</span></span>

<span data-ttu-id="4e51f-112">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="4e51f-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="4e51f-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4e51f-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e51f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e51f-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4e51f-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4e51f-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4e51f-116">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="4e51f-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="4e51f-117">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="4e51f-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e51f-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e51f-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4e51f-119">Dal menu di Visual Studio selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="4e51f-120">Selezionare **Applicazione Web ASP.NET Core** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![nuova applicazione Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="4e51f-122">Assegnare al progetto il nome **MvcMovie** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="4e51f-123">È importante assegnare al progetto il nome **MvcMovie**, in modo che quando si copia il codice lo spazio dei nomi corrisponda.</span><span class="sxs-lookup"><span data-stu-id="4e51f-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![nuova applicazione Web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="4e51f-125">Selezionare **Applicazione Web (MVC)** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="4e51f-126">Finestra di dialogo Nuovo progetto, .NET Core nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e51f-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="4e51f-127">Per il progetto MVC appena creato Visual Studio ha usato il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="4e51f-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="4e51f-128">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="4e51f-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="4e51f-129">Si tratta di un progetto iniziale di base.</span><span class="sxs-lookup"><span data-stu-id="4e51f-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4e51f-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4e51f-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4e51f-131">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4e51f-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="4e51f-132">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="4e51f-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="4e51f-133">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="4e51f-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4e51f-134">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="4e51f-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="4e51f-135">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4e51f-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="4e51f-136">Viene visualizzata una finestra di dialogo con le **risorse necessarie per la compilazione e il debug non è presente in ' MvcMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="4e51f-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="4e51f-137">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-137">Select **Yes**</span></span>

  * <span data-ttu-id="4e51f-138">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="4e51f-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="4e51f-139">`code -r MvcMovie`: carica il file di progetto *MvcMovie. csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4e51f-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4e51f-140">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="4e51f-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4e51f-141">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-141">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="4e51f-143">Selezionare l'applicazione Web **.NET Core** > **app** > **(Model-View-Controller)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="4e51f-145">Nella finestra di dialogo **configurare la nuova ASP.NET Core API Web** impostare il **Framework di destinazione** di **.NET Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.1**.</span></span>

  ![selezione di macOS .NET Core 3,1](./start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="4e51f-147">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="4e51f-148">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="4e51f-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e51f-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e51f-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4e51f-150">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="4e51f-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="4e51f-151">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="4e51f-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="4e51f-152">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="4e51f-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4e51f-153">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4e51f-154">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="4e51f-155">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="4e51f-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4e51f-156">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="4e51f-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="4e51f-157">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="4e51f-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Debug - menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="4e51f-159">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="4e51f-161">La figura seguente mostra l'app:</span><span class="sxs-lookup"><span data-stu-id="4e51f-161">The following image shows the app:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4e51f-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4e51f-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4e51f-164">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="4e51f-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="4e51f-165">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4e51f-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="4e51f-166">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="4e51f-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="4e51f-167">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="4e51f-168">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="4e51f-169">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="4e51f-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4e51f-170">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="4e51f-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4e51f-172">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="4e51f-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4e51f-173">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="4e51f-174">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="4e51f-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="4e51f-175">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="4e51f-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4e51f-176">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4e51f-177">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="4e51f-178">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="4e51f-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="4e51f-179">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="4e51f-180">La figura seguente mostra l'app:</span><span class="sxs-lookup"><span data-stu-id="4e51f-180">The following image shows the app:</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="4e51f-182">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="4e51f-182">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4e51f-183">Successivo</span><span class="sxs-lookup"><span data-stu-id="4e51f-183">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="4e51f-184">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="4e51f-184">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="4e51f-185">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="4e51f-185">The app manages a database of movie titles.</span></span> <span data-ttu-id="4e51f-186">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="4e51f-186">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4e51f-187">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="4e51f-187">Create a web app.</span></span>
> * <span data-ttu-id="4e51f-188">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="4e51f-188">Add and scaffold a model.</span></span>
> * <span data-ttu-id="4e51f-189">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="4e51f-189">Work with a database.</span></span>
> * <span data-ttu-id="4e51f-190">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="4e51f-190">Add search and validation.</span></span>

<span data-ttu-id="4e51f-191">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="4e51f-191">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="4e51f-192">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4e51f-192">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e51f-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e51f-193">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4e51f-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4e51f-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4e51f-195">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="4e51f-195">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="4e51f-196">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="4e51f-196">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e51f-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e51f-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4e51f-198">Dal menu di Visual Studio selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-198">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="4e51f-199">Selezionare **Applicazione Web ASP.NET Core** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-199">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![nuova applicazione Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="4e51f-201">Assegnare al progetto il nome **MvcMovie** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-201">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="4e51f-202">È importante assegnare al progetto il nome **MvcMovie**, in modo che quando si copia il codice lo spazio dei nomi corrisponda.</span><span class="sxs-lookup"><span data-stu-id="4e51f-202">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![nuova applicazione Web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="4e51f-204">Selezionare **Applicazione Web (MVC)** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-204">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="4e51f-205">Finestra di dialogo Nuovo progetto, .NET Core nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e51f-205">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="4e51f-206">Per il progetto MVC appena creato Visual Studio ha usato il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="4e51f-206">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="4e51f-207">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="4e51f-207">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="4e51f-208">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="4e51f-208">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4e51f-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4e51f-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4e51f-210">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4e51f-210">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="4e51f-211">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="4e51f-211">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="4e51f-212">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="4e51f-212">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4e51f-213">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="4e51f-213">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="4e51f-214">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4e51f-214">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="4e51f-215">Viene visualizzata una finestra di dialogo con le **risorse necessarie per la compilazione e il debug non è presente in ' MvcMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="4e51f-215">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="4e51f-216">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-216">Select **Yes**</span></span>

  * <span data-ttu-id="4e51f-217">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="4e51f-217">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="4e51f-218">`code -r MvcMovie`: carica il file di progetto *MvcMovie. csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4e51f-218">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4e51f-219">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="4e51f-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4e51f-220">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-220">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="4e51f-222">Selezionare l'applicazione Web **.NET Core** > **app** > **(Model-View-Controller)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-222">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="4e51f-224">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** accettare l'impostazione predefinita di **Framework di destinazione**, ovvero **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-224">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Selezione di .NET Core 2.2 per macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="4e51f-226">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-226">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="4e51f-227">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="4e51f-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e51f-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e51f-228">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4e51f-229">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="4e51f-229">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="4e51f-230">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="4e51f-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="4e51f-231">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="4e51f-231">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4e51f-232">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-232">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4e51f-233">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="4e51f-234">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="4e51f-234">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4e51f-235">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="4e51f-235">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="4e51f-236">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="4e51f-236">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Debug - menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="4e51f-238">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-238">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="4e51f-240">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="4e51f-240">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="4e51f-241">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="4e51f-241">This app doesn't track personal information.</span></span> <span data-ttu-id="4e51f-242">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="4e51f-242">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="4e51f-244">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="4e51f-244">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4e51f-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4e51f-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4e51f-247">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="4e51f-247">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="4e51f-248">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4e51f-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="4e51f-249">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="4e51f-249">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="4e51f-250">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="4e51f-251">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-251">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="4e51f-252">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="4e51f-252">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4e51f-253">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="4e51f-253">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="4e51f-254">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="4e51f-254">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="4e51f-255">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="4e51f-255">This app doesn't track personal information.</span></span> <span data-ttu-id="4e51f-256">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="4e51f-256">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="4e51f-258">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="4e51f-258">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4e51f-260">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="4e51f-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4e51f-261">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-261">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="4e51f-262">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="4e51f-262">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="4e51f-263">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="4e51f-263">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4e51f-264">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-264">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4e51f-265">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="4e51f-265">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="4e51f-266">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="4e51f-266">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="4e51f-267">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="4e51f-267">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="4e51f-268">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="4e51f-268">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="4e51f-269">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="4e51f-269">This app doesn't track personal information.</span></span> <span data-ttu-id="4e51f-270">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="4e51f-270">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="4e51f-272">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="4e51f-272">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="4e51f-274">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="4e51f-274">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4e51f-275">Successivo</span><span class="sxs-lookup"><span data-stu-id="4e51f-275">Next</span></span>](adding-controller.md)

::: moniker-end
