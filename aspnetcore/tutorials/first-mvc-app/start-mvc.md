---
title: Introduzione ad ASP.NET Core MVC
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: b3544769b0e40fc27f5b6c939c6d7b5f9082f854
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682803"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="65600-103">Introduzione ad ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="65600-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="65600-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="65600-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="65600-105">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="65600-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="65600-106">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="65600-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="65600-107">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="65600-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65600-108">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="65600-108">Create a web app.</span></span>
> * <span data-ttu-id="65600-109">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="65600-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="65600-110">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="65600-110">Work with a database.</span></span>
> * <span data-ttu-id="65600-111">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="65600-111">Add search and validation.</span></span>

<span data-ttu-id="65600-112">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="65600-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="65600-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="65600-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65600-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65600-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65600-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65600-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65600-116">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="65600-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="65600-117">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="65600-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65600-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65600-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="65600-119">Dal menu di Visual Studio selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="65600-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="65600-120">Selezionare **Applicazione Web ASP.NET Core** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="65600-120">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![nuova applicazione Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="65600-122">Assegnare al progetto il nome **MvcMovie** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="65600-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="65600-123">È importante assegnare al progetto il nome **MvcMovie**, in modo che quando si copia il codice lo spazio dei nomi corrisponda.</span><span class="sxs-lookup"><span data-stu-id="65600-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![nuova applicazione Web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="65600-125">Selezionare **Applicazione Web (MVC)** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="65600-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="65600-126">Finestra di dialogo Nuovo progetto, .NET Core nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65600-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="65600-127">Per il progetto MVC appena creato Visual Studio ha usato il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="65600-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="65600-128">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="65600-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="65600-129">Si tratta di un progetto iniziale di base.</span><span class="sxs-lookup"><span data-stu-id="65600-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65600-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65600-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="65600-131">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="65600-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="65600-132">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="65600-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="65600-133">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="65600-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="65600-134">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="65600-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="65600-135">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="65600-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="65600-136">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="65600-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="65600-137">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="65600-137">Select **Yes**</span></span>

  * <span data-ttu-id="65600-138">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="65600-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="65600-139">`code -r MvcMovie`: carica il file di progetto *MvcMovie.csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="65600-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65600-140">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="65600-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="65600-141">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="65600-141">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="65600-143">Selezionare **.NET Core** > **App** > **Applicazione Web (Model-View-Controller)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="65600-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="65600-145">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** impostare **Framework di destinazione** su **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="65600-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="65600-146">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="65600-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="65600-147">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="65600-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65600-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65600-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="65600-149">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="65600-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="65600-150">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="65600-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="65600-151">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="65600-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="65600-152">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="65600-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="65600-153">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="65600-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="65600-154">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="65600-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="65600-155">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="65600-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="65600-156">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="65600-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="65600-158">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="65600-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)


  <span data-ttu-id="65600-160">La figura seguente mostra l'app:</span><span class="sxs-lookup"><span data-stu-id="65600-160">The following image shows the app:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65600-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65600-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="65600-163">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="65600-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="65600-164">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="65600-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="65600-165">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="65600-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="65600-166">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="65600-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="65600-167">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="65600-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="65600-168">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="65600-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="65600-169">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="65600-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="65600-170">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="65600-170">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="65600-171">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="65600-171">This app doesn't track personal information.</span></span> <span data-ttu-id="65600-172">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="65600-172">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="65600-174">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="65600-174">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65600-176">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="65600-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="65600-177">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="65600-177">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="65600-178">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="65600-178">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="65600-179">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="65600-179">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="65600-180">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="65600-180">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="65600-181">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="65600-181">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="65600-182">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="65600-182">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="65600-183">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="65600-183">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="65600-184">La figura seguente mostra l'app:</span><span class="sxs-lookup"><span data-stu-id="65600-184">The following image shows the app:</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="65600-186">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="65600-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="65600-187">avanti</span><span class="sxs-lookup"><span data-stu-id="65600-187">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="65600-188">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="65600-188">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="65600-189">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="65600-189">The app manages a database of movie titles.</span></span> <span data-ttu-id="65600-190">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="65600-190">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65600-191">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="65600-191">Create a web app.</span></span>
> * <span data-ttu-id="65600-192">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="65600-192">Add and scaffold a model.</span></span>
> * <span data-ttu-id="65600-193">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="65600-193">Work with a database.</span></span>
> * <span data-ttu-id="65600-194">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="65600-194">Add search and validation.</span></span>

<span data-ttu-id="65600-195">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="65600-195">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="65600-196">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="65600-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65600-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65600-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65600-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65600-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65600-199">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="65600-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="65600-200">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="65600-200">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65600-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65600-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="65600-202">Dal menu di Visual Studio selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="65600-202">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="65600-203">Selezionare **Applicazione Web ASP.NET Core** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="65600-203">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![nuova applicazione Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="65600-205">Assegnare al progetto il nome **MvcMovie** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="65600-205">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="65600-206">È importante assegnare al progetto il nome **MvcMovie**, in modo che quando si copia il codice lo spazio dei nomi corrisponda.</span><span class="sxs-lookup"><span data-stu-id="65600-206">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![nuova applicazione Web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="65600-208">Selezionare **Applicazione Web (MVC)** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="65600-208">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="65600-209">Finestra di dialogo Nuovo progetto, .NET Core nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65600-209">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="65600-210">Per il progetto MVC appena creato Visual Studio ha usato il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="65600-210">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="65600-211">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="65600-211">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="65600-212">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="65600-212">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65600-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65600-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="65600-214">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="65600-214">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="65600-215">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="65600-215">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="65600-216">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="65600-216">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="65600-217">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="65600-217">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="65600-218">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="65600-218">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="65600-219">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="65600-219">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="65600-220">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="65600-220">Select **Yes**</span></span>

  * <span data-ttu-id="65600-221">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="65600-221">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="65600-222">`code -r MvcMovie`: carica il file di progetto *MvcMovie.csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="65600-222">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65600-223">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="65600-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="65600-224">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="65600-224">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="65600-226">Selezionare **.NET Core** > **App** > **Applicazione Web (Model-View-Controller)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="65600-226">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="65600-228">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** accettare l'impostazione predefinita di **Framework di destinazione**, ovvero **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="65600-228">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Selezione di .NET Core 2.2 per macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="65600-230">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="65600-230">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="65600-231">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="65600-231">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65600-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65600-232">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="65600-233">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="65600-233">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="65600-234">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="65600-234">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="65600-235">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="65600-235">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="65600-236">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="65600-236">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="65600-237">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="65600-237">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="65600-238">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="65600-238">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="65600-239">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="65600-239">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="65600-240">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="65600-240">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="65600-242">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="65600-242">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="65600-244">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="65600-244">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="65600-245">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="65600-245">This app doesn't track personal information.</span></span> <span data-ttu-id="65600-246">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="65600-246">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="65600-248">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="65600-248">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65600-250">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65600-250">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="65600-251">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="65600-251">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="65600-252">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="65600-252">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="65600-253">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="65600-253">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="65600-254">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="65600-254">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="65600-255">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="65600-255">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="65600-256">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="65600-256">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="65600-257">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="65600-257">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="65600-258">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="65600-258">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="65600-259">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="65600-259">This app doesn't track personal information.</span></span> <span data-ttu-id="65600-260">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="65600-260">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="65600-262">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="65600-262">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65600-264">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="65600-264">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="65600-265">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="65600-265">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="65600-266">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="65600-266">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="65600-267">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="65600-267">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="65600-268">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="65600-268">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="65600-269">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="65600-269">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="65600-270">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="65600-270">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="65600-271">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="65600-271">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="65600-272">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="65600-272">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="65600-273">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="65600-273">This app doesn't track personal information.</span></span> <span data-ttu-id="65600-274">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="65600-274">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="65600-276">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="65600-276">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="65600-278">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="65600-278">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="65600-279">avanti</span><span class="sxs-lookup"><span data-stu-id="65600-279">Next</span></span>](adding-controller.md)

::: moniker-end
