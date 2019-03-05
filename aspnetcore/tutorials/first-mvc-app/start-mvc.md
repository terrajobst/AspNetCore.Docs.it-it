---
title: Introduzione ad ASP.NET Core MVC
author: rick-anderson
description: Informazioni introduttive su ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: c09c06f55c4179e9e2174f0063ab7387b7e4c31b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899229"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="ff11c-103">Introduzione ad ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ff11c-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="ff11c-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ff11c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="ff11c-105">Questa esercitazione illustra le nozioni di base della creazione di un'app Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ff11c-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="ff11c-106">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="ff11c-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="ff11c-107">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="ff11c-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ff11c-108">Creare un'app Web.</span><span class="sxs-lookup"><span data-stu-id="ff11c-108">Create a web app.</span></span>
> * <span data-ttu-id="ff11c-109">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="ff11c-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="ff11c-110">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="ff11c-110">Work with a database.</span></span>
> * <span data-ttu-id="ff11c-111">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="ff11c-111">Add search and validation.</span></span>

<span data-ttu-id="ff11c-112">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare dati relativi ai film.</span><span class="sxs-lookup"><span data-stu-id="ff11c-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="ff11c-113">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="ff11c-113">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ff11c-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff11c-114">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ff11c-115">In Visual Studio selezionare **File > Nuovo > Progetto**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-115">From Visual Studio, select  **File > New > Project**.</span></span>

![File > Nuovo > Progetto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="ff11c-117">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="ff11c-117">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="ff11c-118">Nel riquadro a sinistra selezionare **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-118">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="ff11c-119">Nel riquadro al centro selezionare **Applicazione Web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-119">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="ff11c-120">Denominare il progetto "MvcMovie" (è importante denominare il progetto "MvcMovie" in modo che quando si copia il codice lo spazio dei nomi corrisponda).</span><span class="sxs-lookup"><span data-stu-id="ff11c-120">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="ff11c-121">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-121">select **OK**</span></span>

![<span data-ttu-id="ff11c-122">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff11c-122">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="ff11c-123">Completare la finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="ff11c-123">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="ff11c-124">Nella casella di riepilogo a discesa del selettore di versione selezionare **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-124">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="ff11c-125">Selezionare **Applicazione Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="ff11c-125">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="ff11c-126">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-126">select **OK**.</span></span>

![<span data-ttu-id="ff11c-127">Finestra di dialogo Nuovo progetto, .NET nel riquadro sinistro, Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff11c-127">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="ff11c-128">Visual Studio ha usato un modello predefinito per il progetto MVC appena creato.</span><span class="sxs-lookup"><span data-stu-id="ff11c-128">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="ff11c-129">Ora è possibile disporre di un'app funzionante immettendo un nome progetto e selezionando alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="ff11c-129">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ff11c-130">Si tratta di un progetto iniziale di base che rappresenta un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="ff11c-130">This is a basic starter project, and it's a good place to start.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ff11c-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff11c-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ff11c-132">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ff11c-132">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ff11c-133">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="ff11c-133">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="ff11c-134">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ff11c-134">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ff11c-135">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="ff11c-135">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ff11c-136">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ff11c-136">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="ff11c-137">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="ff11c-137">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="ff11c-138">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-138">Select **Yes**</span></span>

  * <span data-ttu-id="ff11c-139">`dotnet new mvc -o MvcMovie`: crea un nuovo progetto ASP.NET Core MVC nella cartella *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="ff11c-139">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="ff11c-140">`code -r MvcMovie`: carica il file di progetto *MvcMovie.csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ff11c-140">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ff11c-141">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="ff11c-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ff11c-142">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-142">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="ff11c-144">Selezionare **.NET Core App (App .NET Core)** > **ASP.NET Core** > **App Web ASP.NET Core (MVC)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-144">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="ff11c-146">Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero \**.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="ff11c-146">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="ff11c-147">Denominare il progetto **MvcMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---  
<!-- End of VS tabs -->

### <a name="run-the-app"></a><span data-ttu-id="ff11c-148">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="ff11c-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ff11c-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff11c-149">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="ff11c-150">Selezionare **CTRL+F5** per eseguire l'app in modalità non di debug.</span><span class="sxs-lookup"><span data-stu-id="ff11c-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="ff11c-151">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="ff11c-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ff11c-152">Si noti che la barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="ff11c-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ff11c-153">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="ff11c-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ff11c-154">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="ff11c-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="ff11c-155">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="ff11c-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ff11c-156">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="ff11c-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ff11c-157">È possibile scegliere se avviare l'app in modalità di debug o non di debug nella voce di menu **Debug**:</span><span class="sxs-lookup"><span data-stu-id="ff11c-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debug](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ff11c-159">È possibile eseguire il debug dell'app toccando il pulsante **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ff11c-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff11c-161">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="ff11c-162">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="ff11c-162">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="ff11c-163">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ff11c-163">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="ff11c-164">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="ff11c-164">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="ff11c-165">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="ff11c-165">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ff11c-166">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="ff11c-166">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="ff11c-167">Se si avvia l'app con CTRL+F5 (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="ff11c-167">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ff11c-168">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="ff11c-168">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ff11c-169">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="ff11c-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ff11c-170">Per avviare l'app, selezionare **Esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-170">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="ff11c-171">Visual Studio per Mac avvia il server [Kestrel](xref:fundamentals/servers/index#kestrel), apre un browser e naviga all'indirizzo `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="ff11c-171">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ff11c-172">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="ff11c-172">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ff11c-173">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="ff11c-173">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ff11c-174">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="ff11c-174">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ff11c-175">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="ff11c-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ff11c-176">È possibile scegliere se avviare l'app in modalità di debug o non di dal menu **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="ff11c-176">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

------

* <span data-ttu-id="ff11c-177">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="ff11c-177">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ff11c-178">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="ff11c-178">This app doesn't track personal information.</span></span> <span data-ttu-id="ff11c-179">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ff11c-179">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](start-mvc/_static/privacy.png)

  <span data-ttu-id="ff11c-181">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="ff11c-181">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="ff11c-183">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="ff11c-183">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ff11c-184">avanti</span><span class="sxs-lookup"><span data-stu-id="ff11c-184">Next</span></span>](adding-controller.md)  
