---
title: Introduzione ad ASP.NET Core MVC
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dc3499c89860190b76d6be7b8abeeaef827880d6
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491257"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="efe57-103">Introduzione ad ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="efe57-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="efe57-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="efe57-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="efe57-105">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="efe57-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="efe57-106">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="efe57-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="efe57-107">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="efe57-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="efe57-108">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="efe57-108">Create a web app.</span></span>
> * <span data-ttu-id="efe57-109">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="efe57-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="efe57-110">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="efe57-110">Work with a database.</span></span>
> * <span data-ttu-id="efe57-111">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="efe57-111">Add search and validation.</span></span>

<span data-ttu-id="efe57-112">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="efe57-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="efe57-113">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="efe57-113">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="efe57-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="efe57-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="efe57-115">Dal menu di Visual Studio selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="efe57-115">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="efe57-116">Selezionare **Applicazione Web ASP.NET Core** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="efe57-116">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![nuova applicazione Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="efe57-118">Assegnare al progetto il nome **MvcMovie** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="efe57-118">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="efe57-119">È importante assegnare al progetto il nome **MvcMovie**, in modo che quando si copia il codice lo spazio dei nomi corrisponda.</span><span class="sxs-lookup"><span data-stu-id="efe57-119">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![nuova applicazione Web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="efe57-121">Selezionare **Applicazione Web (MVC)** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="efe57-121">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="efe57-122">Finestra di dialogo Nuovo progetto, .NET Core nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efe57-122">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="efe57-123">Per il progetto MVC appena creato Visual Studio ha usato il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="efe57-123">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="efe57-124">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="efe57-124">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="efe57-125">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="efe57-125">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="efe57-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="efe57-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="efe57-127">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="efe57-127">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="efe57-128">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="efe57-128">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="efe57-129">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="efe57-129">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="efe57-130">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="efe57-130">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="efe57-131">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="efe57-131">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="efe57-132">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="efe57-132">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="efe57-133">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="efe57-133">Select **Yes**</span></span>

  * <span data-ttu-id="efe57-134">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="efe57-134">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="efe57-135">`code -r MvcMovie`: carica il file di progetto *MvcMovie.csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="efe57-135">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="efe57-136">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="efe57-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="efe57-137">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="efe57-137">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="efe57-139">Selezionare **.NET Core** > **App** > **Applicazione Web (MVC)**  > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="efe57-139">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="efe57-141">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** accettare l'impostazione predefinita di **Framework di destinazione**, ovvero **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="efe57-141">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Selezione di .NET Core 2.2 per macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="efe57-143">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="efe57-143">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="efe57-144">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="efe57-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="efe57-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="efe57-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="efe57-146">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="efe57-146">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="efe57-147">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="efe57-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="efe57-148">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="efe57-148">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="efe57-149">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="efe57-149">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="efe57-150">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="efe57-150">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="efe57-151">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="efe57-151">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="efe57-152">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="efe57-152">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="efe57-153">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="efe57-153">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="efe57-155">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="efe57-155">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="efe57-157">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="efe57-157">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="efe57-158">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="efe57-158">This app doesn't track personal information.</span></span> <span data-ttu-id="efe57-159">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="efe57-159">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="efe57-161">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="efe57-161">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="efe57-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="efe57-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="efe57-164">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="efe57-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="efe57-165">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="efe57-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="efe57-166">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="efe57-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="efe57-167">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="efe57-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="efe57-168">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="efe57-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="efe57-169">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="efe57-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="efe57-170">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="efe57-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="efe57-171">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="efe57-171">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="efe57-172">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="efe57-172">This app doesn't track personal information.</span></span> <span data-ttu-id="efe57-173">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="efe57-173">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="efe57-175">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="efe57-175">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="efe57-177">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="efe57-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="efe57-178">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="efe57-178">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="efe57-179">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="efe57-179">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="efe57-180">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="efe57-180">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="efe57-181">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="efe57-181">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="efe57-182">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="efe57-182">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="efe57-183">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="efe57-183">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="efe57-184">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="efe57-184">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="efe57-185">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="efe57-185">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="efe57-186">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="efe57-186">This app doesn't track personal information.</span></span> <span data-ttu-id="efe57-187">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="efe57-187">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="efe57-189">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="efe57-189">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="efe57-191">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="efe57-191">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="efe57-192">avanti</span><span class="sxs-lookup"><span data-stu-id="efe57-192">Next</span></span>](adding-controller.md)
